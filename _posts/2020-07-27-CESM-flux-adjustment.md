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
$F_T=HρC_p(T^obs-T^eq)/((HρC_p)/μ_T )=μ_T(T^obs-T^eq)$    unit:w/m2
$F_S=H(S^obs-S^eq)/(H/μ_S )=μ_S(S^obs-S^eq)*86400$   (unit: mm/day)
Note that 
$$τ_T=(HρC_p)/μ_T$ and τ_S=H/μ_S $$

Inspired by CESM restoring experiment ([see](https://derekyuntao.github.io/jekyll-clean-dark/2020/07/CESM-restoring/)), we can use similar settings for ocean surface flux adjustment. To be more confident, we would first understand what code is doing when restoring.
There are two ways to do 
Here are some pre-assignment parameters and variables:

module grid
KMT            ,&! k index of deepest grid cell on T grid
KMU            ,&! k index of deepest grid cell on U grid
dz                ,&! thickness of layer k
DZU, DZT               ! thickness of U,T cell for pbc

