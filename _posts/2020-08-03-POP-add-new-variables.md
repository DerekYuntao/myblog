---
layout: post
title: "Adding new variables in POP"
date: 2020-08-03
description: How to add new variables in POP
share: true
tags:
 - CESM
 - Tech-accumulate
---
Reference:
[http://www.cesm.ucar.edu/models/cesm1.2/pop2/doc/faq/]

CESM POP2 model produces the following files:

log files:
ocn.log.yymmdd-sssss     output log (text) files
diagnostics files:
${CASE}.pop.dd*     general diagnostics
${CASE}.pop.do*     overflows
${CASE}.pop.dt*      transport
${CASE}.pop.dv*      velocity
time-averaged history files:
${CASE}.pop.h*     (the "tavg" files)
restart files:
${CASE}.pop.r.*       model variables
${CASE}.pop.rh.*     time-averaged history
${CASE}.pop.ro.*     overflows
restart pointer files:
rpointer.ocn.restart.*
rpointer.ocn.tavg.*
rpointer.ocn.ovf.*

## How do I add a POP2 variable to the tavg output files?

Suppose you want to add a CESM POP2 variable to your tavg output files, and the variable is not in the {gxivj}_tavg_contents file.

First, search the CESM POP2 Fortran modules to see if your variable, VARNAME, is a model variable. If the model variable exists, then search the code to see if there is a tavg "handle" (tavg_VARNAME) associated with that model variable. If so, all you need to do is add the tavg variable name to the tavg_contents file.

For example, suppose you are interested in putting the time average of RHOU, the product of potential density and the U velocity, into a POP2 time-averaged output file in your gx1v6 ocean-resolution case. You've already searched the gx1v6_tavg_contents file, so you know RHOU is not there. So you next search the POP2 modules, and you find references to the variable RHOU and its tavg handle, tavg_RHOU, in the advection.F90 module. This is good news, because all you need to do to accomplish your goal is to add RHOU to your tavg_contents file:
```powershell
$cd $CASE/SourceMods/src.pop2
cp $CODEROOT/models/ocn/pop2/input_templates/gx1v6_tavg_contents .
add the following line to your gx1v6_tavg_contents file:
1  RHOU
run your $CASE (no rebuild is necessary)
```

**In most cases**, one want to add new variables defined by himself, and of course the variables can't be found in CESM POP2 Fortran modules, nor in the tavg "handle" for that variable, you will need to modify the POP2 code.
## How do I add a POP2 variable to the tavg output files? 
Suppose you are interested in putting the time average of the U velocity at level 5 into a POP2 time-averaged output file. Confirming that U velocity at level 5 is not on the list of gx1v6_tavg_contents file. Next, you've searched the POP2 modules, and although you found references to UVEL, tavg_UVEL, tavg_U1_8, and lots of tavg_U* variables, you've found nothing that looks like U velocity at level five.

You will need to modify the POP2 code. In adding support for new tavg output variables, it is easiest to mimic developments for similar variables, so let's use tavg_U1_8, the vertical average of U velocity over model levels one through eight, as our guide. To instrument the POP2 code to write out our new varible, U velocity at level 5, do the following:
```powershell
$cd $CASE/SourceMods/src.pop2
cp $CODEROOT/models/ocn/pop2/source/baroclinic.F90 .
edit baroclinic, using variable tavg_U1_8 as a guide, adding the following lines in the appropriate places:
          integer (int_kind) :: tavg_U5_5  ! ID for U velocity in layer five


          call define_tavg_field(tavg_U5_5,'U5_5',2,                          &
                                 long_name='Zonal Velocity lvls 5-5',         &
                                 units='centimeter/s', grid_loc='2221')
          if (k == 5)  &
          call accumulate_tavg_field(UVEL(:,:,k,curtime,iblock),tavg_U5_5,iblock,k)
cd $CASE
build (or rebuild), then run your $CASE
examine your $CASE.pop.h.yyyy-mm.nc file, where you should find your new variable, U5_5.
```

## Exlplaination on subroutine define_tavg_field
```fortran
 subroutine define_tavg_field(id, short_name, ndims, tavg_method,       & 293,15
                                  long_name, units,                     &
                                  grid_loc, valid_range,                &
                                  field_loc, field_type,coordinates,    &
                                  scale_factor,                         &
                                  nftype,                               &
                                  nstd_fields,                          &
                                  num_nstd_fields, max_nstd_fields      )

! !DESCRIPTION:
!  Initializes description of an available field and returns location
!  in the available fields array for use in later tavg calls.
```

 subroutine accumulate_tavg_field(ARRAY,field_id,block,k,const) 291,2

## Exlplaination on subroutine accumulate_tavg_field
```fortran
 subroutine accumulate_tavg_field(ARRAY,field_id,block,k,const) 
! !DESCRIPTION:
!  This routine updates a tavg field.  If the time average of the
!  field is requested, it accumulates a time sum of a field by 
!  multiplying by the time step and accumulating the sum into the 
!  tavg buffer array.  If the min or max of a field is requested, it
!  checks the current value and replaces the min, max if the current
!  value is less than or greater than the stored value.
!
! !REVISION HISTORY:
!  same as module

! !INPUT PARAMETERS:

   integer (int_kind), intent(in) :: &
      block,           &! local block address (in baroclinic distribution)
      k,               &! vertical level
      field_id          ! index in available fields for tavg field info (the accumulated output data id)

   real (r8), dimension(nx_block,ny_block), intent(in) :: &
      ARRAY             ! array of data for this block to add to 
                        !  accumulated sum in tavg buffer
   real (r8), optional, intent(in) ::  &
      const
!EOP
!BOC
```
**Instance**: Diagnoise surface heat/freshwater flux terms and add then as new output variables (See next: Fluxadj 3)
