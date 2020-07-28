---
layout: post
title: "CESM Flux Adjustment"
date: 2020-07-27
description: Modify POP2 surface forcing to make a flux adjustment
share: true
tags:
 - CESM
 - Tech-accumulate
---

[Check POP2 Forcing namelists](http://www.cesm.ucar.edu/models/cesm1.2/pop2/doc/users/node67.html)

To quickly check CESM1.0 code online, [click here.](http://www.cesm.ucar.edu/models/cesm1.2/cesm/cesmBbrowser/)

Temperature and salinity flux adjustment term can be written as follows:
$F_T=HρC_p(T^{obs}-T^{eq})/(HρC_p/μ_T )=μ_T(T^{obs}-T^{eq})$      unit:w/m2
$F_S=H(S^{obs}-S^{eq})/(H/μ_S )=μ_S(S^{obs}-S^{eq})*86400$      unit: mm/day
Note that $τ_T=(HρC_p)/μ_T$ and $τ_S=\frac{H}{μ_S} $

Inspired by [CESM restoring experiment](https://derekyuntao.github.io/jekyll-clean-dark/2020/07/CESM-restoring/), we can use similar settings and modify code slightly for ocean surface flux adjustment. To be more confident, we want to understand what code is doing when restoring.

There are two ways to restore SST and SSS in POP2.
## Way1: Starting from forcing_shf/sfwf.F90

First check forcing_shf.F90.
```fortran
subroutine init_shf(STF)
! !DESCRIPTION:
!  Initializes surface heat flux forcing by either calculating
!  or reading in the surface heat flux.  Also do initial
!  book-keeping concerning when new data is needed for the temporal
!  interpolation and when the forcing will need to be updated.

! !OUTPUT PARAMETERS:

   real (r8), dimension(nx_block,ny_block,nt,max_blocks_clinic), &
      intent(out) :: &
      STF    ! surface tracer flux - this routine only modifies
             ! the slice corresponding to temperature (tracer 1)
```             
STF is the surface trace flux which is the final result calculated and output by this script. What is each dimension of the STF means? 
In ***module blocks***, there are several lines of code defining *nx_block* and *ny_block*.
```fortran
! !DEFINED PARAMETERS:

   integer (int_kind), parameter, public :: &
      nghost = 2       ! number of ghost cells around each block

   integer (int_kind), parameter, public :: &! size of block domain in
      nx_block = block_size_x + 2*nghost,   &!  x,y dir including ghost
      ny_block = block_size_y + 2*nghost     !  cells 

! !PUBLIC DATA MEMBERS:

   integer (int_kind), public :: &
      nblocks_tot      ,&! total number of blocks in decomposition
      nblocks_x        ,&! tot num blocks in i direction
      nblocks_y          ! tot num blocks in j direction


```

In ***module domain***, there are code indicates that *nt* is related to variable identification and should be larger than 2 (for temp, salinitiy...).
```fortran
!----------------------------------------------------------------------
!
!  perform some basic checks on domain
!
!----------------------------------------------------------------------
   else if (nt < 2) then
      !***
      !*** nt must be at least 2 to hold temp,salinitiy
      !***
      call exit_POP(sigAbort,'Invalid tracer number: nt < 2')
```

Here are some pre-assignment parameters and variables:

module grid
KMT            ,&! k index of deepest grid cell on T grid
KMU            ,&! k index of deepest grid cell on U grid
dz                ,&! thickness of layer k
DZU, DZT               ! thickness of U,T cell for pbc

