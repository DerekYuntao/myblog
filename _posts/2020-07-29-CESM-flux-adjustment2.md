---
layout: post
title: "!!Pending: Total Surface Heat Flux and Temp Tendency calculated in POP"
date: 2020-07-29
description: Explain POP code calculating total sea surface heat flux and temperature tendency in POP
share: true
tags:
 - CESM
---

There are two ways to calculate sea surface temperature artificial forcing (restoring term): [in form of flux or in form of tendency](https://derekyuntao.github.io/jekyll-clean-dark/2020/07/CESM-flux-adjustment1/). Correspondingly, total **sea surface heat flux** is calculated by adding restoring flux term, or total temperature tendency is calculated by adding **SST restoring tendency**. We want to make sure if we can get the same results using arbitrary way to perform restoring or flux adjustment. 

***forcing_fields.F90*** defines some variables recived from coupler and used for passing to coupler. Note that ***module forcing_fields*** is called by ***module ocn_comp_mct***, which is the main driver for the POP. Inside this module, ***subroutine ocn_export_mct*** export pop fields to the CCSM cpl7 driver. ***subroutine ocn_export_mct*** is called by ***subroutine ocn_init_mct*** and ***subroutine ocn_run_mct***, which initialize and run POP for a coupling interval.  ***module ocn_comp_mct: subroutine ocn_run_mct; subroutine ocn_init_mct*** is called by called by ***Program ccsm_driver***. Through ***Program ccsm_driver*** and more modules, STF(surface tracer fluxes) and TFW(tracer content in freshwater flux) can be transfered to atm and other components.
Note that there is another subroutine ***ocn_import_mct*** in ***module ocn_comp_mct***, where unpack fluxes from coupler in POP.

## 1. module forcing_fields
Below contains the forcing fields necessary for supporting high-level coupling.  
```fortran
module forcing_fields

!BOP
! !MODULE: forcing_fields

! !DESCRIPTION:
!  Contains the forcing fields necessary for supporting high-level coupling.
!  These fields originally resided in modules forcing and forcing_coupled.
...
! !PUBLIC DATA MEMBERS:

   real (r8), dimension(nx_block,ny_block,max_blocks_clinic),public ::  &
      EVAP_F = c0,       &! evaporation   flux    from cpl (kg/m2/s)
      PREC_F = c0,       &! precipitation flux    from cpl (kg/m2/s)
                          ! (rain + snow)
      SNOW_F = c0,       &! snow          flux    from cpl (kg/m2/s)
      MELT_F = c0,       &! melt          flux    from cpl (kg/m2/s)
      ROFF_F = c0,       &! river runoff  flux    from cpl (kg/m2/s)
      IOFF_F = c0,       &! ice   runoff  flux    from cpl (kg/m2/s)
      SALT_F = c0,       &! salt          flux    from cpl (kg(salt)/m2/s)
      SENH_F = c0,       &! sensible heat flux    from cpl (W/m2   )
      LWUP_F = c0,       &! longwave heat flux up from cpl (W/m2   )
      LWDN_F = c0,       &! longwave heat flux dn from cpl (W/m2   )
      MELTH_F= c0         ! melt     heat flux    from cpl (W/m2   )

   integer(kind=int_kind), public :: &
      ATM_CO2_PROG_nf_ind = 0, & ! bottom atm level prognostic co2
      ATM_CO2_DIAG_nf_ind = 0    ! bottom atm level diagnostic co2

   real (r8), dimension(nx_block,ny_block,2,max_blocks_clinic), &
      public, target :: &
      SMF,  &!  surface momentum fluxes (wind stress)
      SMFT   !  surface momentum fluxes on T points if avail

   real (r8), dimension(nx_block,ny_block,nt,max_blocks_clinic), &
      public, target :: &
      STF,  &!  surface tracer fluxes
      TFW    ! tracer content in freshwater flux

   logical (log_kind), public :: &
      lsmft_avail   ! true if SMFT is an available field

   real (r8), dimension(nx_block,ny_block,max_blocks_clinic), &
      public, target ::  &
      IFRAC,             &! ice fraction; not initialized in this routine
      U10_SQR,           &! 10m wind speed squared; not initialized in this routine
      ATM_PRESS,         &! atmospheric pressure forcing
      FW,FW_OLD           ! freshwater flux at T points (cm/s)
                          ! FW_OLD is at time n-1
 end module forcing_fields
```

## 2. module ocn_comp_mct
Below shows the routine to convert and pack variables calculated in POP, and sent the variables (fields) to CCSM coupler (cpl7 driver).
```fortran
subroutine ocn_export_mct(o2x_o, errorCode)   

! !DESCRIPTION:
!  This routine calls the routines necessary to send pop fields to
!  the CCSM cpl7 driver
!
! !INPUT/OUTPUT PARAMETERS:

   type(mct_aVect)   , intent(inout) :: o2x_o
...

!-----------------------------------------------------------------------
!  local variables
!-----------------------------------------------------------------------

   integer (int_kind) :: n, iblock
           
   character (char_len)    :: label
 
   integer (int_kind) ::  &
      i,j,k

   real (r8), dimension(nx_block,ny_block) ::   &
      WORK1, WORK2,      &! local work space
      WORK3, WORK4

   real (r8), dimension(nx_block,ny_block,max_blocks_clinic) ::   &
        WORKA               ! local work space with full block dimension

   real (r8) ::   &
      m2percm2,   &
      gsum

   type (block) :: this_block ! local block info
...

!-----------------------------------------------------------------------
!     convert and pack surface temperature
!-----------------------------------------------------------------------

   n = 0
   do iblock = 1, nblocks_clinic
      this_block = get_block(blocks_clinic(iblock),iblock)
      do j=this_block%jb,this_block%je
      do i=this_block%ib,this_block%ie
         n = n + 1
         o2x_o%rAttr(index_o2x_So_t,n) =   &
             SBUFF_SUM(i,j,iblock,index_o2x_So_t)/tlast_coupled + T0_Kelvin
      enddo
      enddo
   enddo

!-----------------------------------------------------------------------
!     convert and pack salinity
!-----------------------------------------------------------------------

   n = 0
   do iblock = 1, nblocks_clinic
      this_block = get_block(blocks_clinic(iblock),iblock)
      do j=this_block%jb,this_block%je
      do i=this_block%ib,this_block%ie
         n = n + 1
         o2x_o%rAttr(index_o2x_So_s,n) =   &
             SBUFF_SUM(i,j,iblock,index_o2x_So_s)*salt_to_ppt/tlast_coupled
      enddo
      enddo
   enddo
```
Where are *index_o2x_So_t* and *index_o2x_So_s* meaing? Trace back to ***subroutine pop_sum_buffer*** to get:
```fortran
 subroutine pop_sum_buffer 
! !DESCRIPTION:
!  This routine accumulates sums for averaging fields to
!  be sent to the coupler
...
   real (r8), dimension(nx_block,ny_block,max_blocks_clinic) ::  &
      WORK                ! local work arrays
   ...

!-----------------------------------------------------------------------
!  accumulate sums of U,V,T,S and GRADP
!  accumulate sum of co2 flux, if requested. Implicitly use zero flux if fco2 field not registered yet
!  ice formation flux is handled separately in ice routine
!-----------------------------------------------------------------------
  ...
   SBUFF_SUM(:,:,iblock,index_o2x_So_t ) =   &
      SBUFF_SUM(:,:,iblock,index_o2x_So_t ) + delt*  &
                                   TRACER(:,:,1,1,curtime,iblock)

   SBUFF_SUM(:,:,iblock,index_o2x_So_s ) =   &
      SBUFF_SUM(:,:,iblock,index_o2x_So_s ) + delt*  &
                                   TRACER(:,:,1,2,curtime,iblock)
   ...
 end subroutine pop_sum_buffer   
```
The code indicats that *index_o2x_So_t* represents for timely-accumulated SST and *index_o2x_So_s* for SSS.

Now let's trun back to total surface forcing calculation. *forcing.F90* is the heart of sea surface heat flux and surface fresh water flux calculation**
## 3. module forcing_shf
Below are some definions on variables in *module forcing_shf*.
```fortran
 module forcing
 !BOP
! !MODULE: forcing
!
! !DESCRIPTION:
!  This is the main driver module for all surface and interior
!  forcing.  It contains necessary forcing fields as well as
!  necessary routines for call proper initialization and
!  update routines for those fields.
! !USES:
   use constants
   use blocks
   use distribution
   use domain
   use grid
   use ice, only: salice, tfreez, FW_FREEZE
   use forcing_ws
   use forcing_shf
   use forcing_sfwf
   use forcing_pt_interior
   use forcing_s_interior
   use forcing_ap
   use forcing_coupled, only: set_combined_forcing, tavg_coupled_forcing,  &
       liceform, qsw_12hr_factor, qsw_distrb_iopt, qsw_distrb_iopt_cosz, &
       tday00_interval_beg, interval_cum_dayfrac, QSW_COSZ_WGHT_NORM, &
       QSW_COSZ_WGHT, compute_cosz
   use forcing_tools
   use passive_tracers, only: set_sflux_passive_tracers
   use prognostic
   use tavg
   use movie, only: define_movie_field, movie_requested, update_movie_field
   use time_management
   use exit_mod
#ifdef CCSMCOUPLED
   use shr_sys_mod, only: shr_sys_abort
#endif

   !*** ccsm
   use sw_absorption, only: set_chl
   use registry
   use forcing_fields

   implicit none
   private
   save

! !PUBLIC MEMBER FUNCTIONS:

   public :: init_forcing,           &
             set_surface_forcing,    &
             tavg_forcing,           &
             movie_forcing

...

   subroutine init_forcing 

! !DESCRIPTION:
!  Initializes forcing by calling a separate routines for
!  wind stress, heat flux, fresh water flux, passive tracer flux,
!  interior restoring, and atmospheric pressure.
...
!-----------------------------------------------------------------------
!  initialize forcing arrays
!-----------------------------------------------------------------------

   ATM_PRESS = c0
   FW        = c0
   FW_OLD    = c0
   SMF       = c0
   SMFT      = c0
   STF       = c0
   TFW       = c0

!-----------------------------------------------------------------------
!  call individual initialization routines
!-----------------------------------------------------------------------

   call init_ws(SMF,SMFT,lsmft_avail)    !wind stress
   !*** NOTE: with bulk NCEP forcing init_shf must be called before init_sfwf

   call init_shf (STF)
   call init_sfwf(STF)
   call init_pt_interior
   call init_s_interior
   call init_ap(ATM_PRESS)

!-----------------------------------------------------------------------
!EOC

 end subroutine init_forcing
...

 subroutine set_surface_forcing 
! !DESCRIPTION:
!  Calls surface forcing routines if necessary.
!  If forcing does not depend on the ocean state, then update
!     forcing if current time is greater than the appropriate
!     interpolation time or if it is the first step.
!  If forcing DOES depend on the ocean state, then call every
!     timestep.  interpolation check will be done within the set\_*
!     routine.
!  Interior restoring is assumed to take place every
!     timestep and is set in subroutine tracer\_update, but
!     updating the data fields must occur here outside
!     any block loops.
...
!-----------------------------------------------------------------------
!  Get any interior restoring data and interpolate if necessary.
!-----------------------------------------------------------------------

   call get_pt_interior_data
   call get_s_interior_data

!-----------------------------------------------------------------------
!  Call individual forcing update routines.
!-----------------------------------------------------------------------
...

   !*** NOTE: with bulk NCEP and partially-coupled forcing 
   !***       set_shf must be called before set_sfwf

   call set_shf(STF)
   call set_sfwf(STF,FW,TFW)

   if ( shf_formulation  == 'partially-coupled' .or.  &
        sfwf_formulation == 'partially-coupled' ) then
      call set_combined_forcing(STF,FW,TFW)
   endif
...

end subroutine set_surface_forcing
```
From the line *call set_combined_forcing(STF,FW,TFW)*, we trace back to *subroutine set_combined_forcing* in *module forcing_coupled*
```fortran
!BOP
!MODULE: forcing_coupled

! !DESCRIPTION:
!  This module contains all the routines necessary for coupling POP to
!  atmosphere and sea ice models using the NCAR CCSM flux coupler.  To
!  enable the routines in this module, the coupled ifdef option must
!  be specified during the make process.
...
subroutine set_combined_forcing (STF,FW,TFW)
! !DESCRIPTION:
!
! This routine combines heat flux components into the STF array and
! converts from W/m**2, then combines terms when the "partially-coupled"
! has been selected
! !INPUT/OUTPUT PARAMETERS:
 
   real (r8), dimension(nx_block,ny_block,nt,max_blocks_clinic), &
      intent(inout) :: &
      STF,             &! surface tracer fluxes at current timestep
      TFW               !  tracer concentration in water flux

   real (r8), dimension(nx_block,ny_block,max_blocks_clinic),  &
      intent(inout) ::  &
      FW                !  fresh water flux


!EOP
!BOC
!-----------------------------------------------------------------------
!  local variables
!-----------------------------------------------------------------------

  integer (int_kind) ::  &
     iblock,             &! local address of current block
     n                    ! index

#if CCSMCOUPLED
  real (r8), dimension(nx_block,ny_block,max_blocks_clinic) ::  &
     WORK1, WORK2        ! local work arrays

   !*** need to zero out any padded cells
   WORK1 = c0
   WORK2 = c0

   if ( shf_formulation == 'partially-coupled' ) then
     !$OMP PARALLEL DO PRIVATE(iblock)
     do iblock=1,nblocks_clinic
       STF(:,:,1,iblock) =  SHF_COMP(:,:,iblock,shf_comp_wrest)     &
                   + SHF_COMP(:,:,iblock,shf_comp_srest)     &
                   + SHF_COMP(:,:,iblock,shf_comp_cpl)
     enddo
     !$OMP END PARALLEL DO
   endif

   if ( sfwf_formulation == 'partially-coupled' ) then
     if (sfc_layer_type == sfc_layer_varthick .and.  &
         .not. lfw_as_salt_flx) then
       !$OMP PARALLEL DO PRIVATE(iblock)
       do iblock=1,nblocks_clinic
          STF(:,:,2,iblock) =  SFWF_COMP(:,:,  iblock,sfwf_comp_wrest) &
                             + SFWF_COMP(:,:,  iblock,sfwf_comp_srest)
          FW(:,:,iblock)    =  SFWF_COMP(:,:,  iblock,sfwf_comp_cpl)   &
                             + SFWF_COMP(:,:,  iblock,sfwf_comp_flxio)
          TFW(:,:,:,iblock) =   TFW_COMP(:,:,:,iblock, tfw_comp_cpl)    &
                             +  TFW_COMP(:,:,:,iblock, tfw_comp_flxio)
       enddo
       !$OMP END PARALLEL DO
     else
           if ( lms_balance ) then

         !$OMP PARALLEL DO PRIVATE(iblock)
         do iblock=1,nblocks_clinic
           WORK1(:,:,iblock) = SFWF_COMP(:,:,iblock,sfwf_comp_flxio) /  &
                                salinity_factor
           WORK2(:,:,iblock) = SFWF_COMP(:,:,iblock,sfwf_comp_cpl)
         enddo
         !$OMP END PARALLEL DO

         call ms_balancing (WORK2, EVAP_F,PREC_F, MELT_F, ROFF_F, IOFF_F,   &
                            SALT_F, QFLUX, 'salt', ICEOCN_F=WORK1)

         !$OMP PARALLEL DO PRIVATE(iblock)
         do iblock=1,nblocks_clinic
           STF(:,:,2,iblock) =  SFWF_COMP(:,:,iblock,sfwf_comp_wrest)  &
                              + SFWF_COMP(:,:,iblock,sfwf_comp_srest)  &
                              + WORK2(:,:,iblock)                      &
                              + SFWF_COMP(:,:,iblock,sfwf_comp_flxio)* &
                                MASK_SR(:,:,iblock)
         enddo
         !$OMP END PARALLEL DO

       else     

         !$OMP PARALLEL DO PRIVATE(iblock)
         do iblock=1,nblocks_clinic
           STF(:,:,2,iblock) =  SFWF_COMP(:,:,iblock,sfwf_comp_wrest)  &
                              + SFWF_COMP(:,:,iblock,sfwf_comp_srest)  &
                              + SFWF_COMP(:,:,iblock,sfwf_comp_cpl)    &
                              + SFWF_COMP(:,:,iblock,sfwf_comp_flxio) 
         enddo
         !$OMP END PARALLEL DO
 
       endif
     endif
   endif
!-----------------------------------------------------------------------
!EOC

 end subroutine set_combined_forcing

The code below tells that STF(:,:,1,:) is surface heat fluxes tracer and STF(:,:,1,:) is surface fresh water flux tracer. Again, note that *subroutine set_combined_forcing* combines restoring SHF and SFWF components into the STF array and is called by *subroutine set_surface_forcing* in *module forcing_coupled*. 
Now we go back to *module forcing_coupled*
```fortran
 subroutine tavg_forcing 
! !DESCRIPTION:
!  This routine accumulates tavg diagnostics related to surface
!  forcing.
...
   real (r8), dimension(nx_block,ny_block) :: &
      WORK                ! local temp space for tavg diagnostics

!-----------------------------------------------------------------------
!  compute and accumulate tavg forcing diagnostics
!-----------------------------------------------------------------------

   !$OMP PARALLEL DO PRIVATE(iblock,this_block,WORK)

   do iblock = 1,nblocks_clinic

      this_block = get_block(blocks_clinic(iblock),iblock)

      if (tavg_requested(tavg_SHF)) then
         where (KMT(:,:,iblock) > 0)
            WORK = (STF(:,:,1,iblock)+SHF_QSW(:,:,iblock))/ &
                   hflux_factor ! W/m^2
         elsewhere
            WORK = c0
         end where

         call accumulate_tavg_field(WORK,tavg_SHF,iblock,1)
      endif

      if (tavg_requested(tavg_SHF_QSW)) then
         where (KMT(:,:,iblock) > 0)
            WORK = SHF_QSW(:,:,iblock)/hflux_factor ! W/m^2
         elsewhere
            WORK = c0
         end where

         call accumulate_tavg_field(WORK,tavg_SHF_QSW,iblock,1)
      endif

      if (tavg_requested(tavg_SFWF)) then
         if (sfc_layer_type == sfc_layer_varthick .and. &
             .not. lfw_as_salt_flx) then
            where (KMT(:,:,iblock) > 0)
               WORK = FW(:,:,iblock)*seconds_in_year*mpercm ! m/yr
            elsewhere
               WORK = c0
            end where
         else
            where (KMT(:,:,iblock) > 0) ! convert to kg(freshwater)/m^2/s
               WORK = STF(:,:,2,iblock)/salinity_factor
            elsewhere
               WORK = c0
            end where
         endif

         call accumulate_tavg_field(WORK,tavg_SFWF,iblock,1)
      endif

      if (tavg_requested(tavg_SFWF_WRST)) then
         if ( sfwf_formulation == 'partially-coupled' ) then
            where (KMT(:,:,iblock) > 0) ! convert to kg(freshwater)/m^2/s
               WORK = SFWF_COMP(:,:,iblock,sfwf_comp_wrest)/salinity_factor
            elsewhere
               WORK = c0
            end where
         else
            WORK = c0
         endif
         call accumulate_tavg_field(WORK,tavg_SFWF_WRST,iblock,1)
      endif
      ...
         end do

   !$OMP END PARALLEL DO

   if (registry_match('lcoupled')) call tavg_coupled_forcing
   
 end subroutine tavg_forcing






and prepare to be used to be packed and send to other components of CESM by coupler.


## Appendix 
***Program ccsm_driver*** take reaction to different sets of component status.
```fortran
program ccsm_driver
!-------------------------------------------------------------------------------
! Purpose: Main program for NCAR CCSM4/cpl7. Can have different
!          land, sea-ice, and ocean models plugged in at compile-time.
!          These models can be either: stub, dead, data, or active
!          components or some combination of the above.
!
!               stub -------- Do nothing.
!               dead -------- Send analytic data back.
!               data -------- Send data back interpolated from input files.
!               prognostic -- Prognostically simulate the given component.
!
! Method: Call appropriate initialization, run (time-stepping), and 
!         finalization routines.
!-------------------------------------------------------------------------------
```

## 3. ***module shr_flux_mod***
```fortran
!-------------------------------------------------------------------------------
! PURPOSE:
!   computes atm/ocn surface fluxes
!
! NOTES: 
!   o all fluxes are positive downward
!   o net heat flux = net sw + lw up + lw down + sen + lat
!   o here, tstar = <WT>/U*, and qstar = <WQ>/U*.
!   o wind speeds should all be above a minimum speed (eg. 1.0 m/s)
! 
! ASSUMPTIONS:
!   o Neutral 10m drag coeff: cdn = .0027/U10 + .000142 + .0000764 U10
!   o Neutral 10m stanton number: ctn = .0327 sqrt(cdn), unstable
!                                 ctn = .0180 sqrt(cdn), stable
!   o Neutral 10m dalton number:  cen = .0346 sqrt(cdn)
!   o The saturation humidity of air at T(K): qsat(T)  (kg/m^3)
!-------------------------------------------------------------------------------
```