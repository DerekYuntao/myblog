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

[POP2 Forcing namelists](http://www.cesm.ucar.edu/models/cesm1.2/pop2/doc/users/node67.html)

In order to perform the experiments, Let's read and understand what is code doing when restoring.

Here are some pre-assignment parameters and variables:

module grid
KMT            ,&! k index of deepest grid cell on T grid
KMU            ,&! k index of deepest grid cell on U grid
dz                ,&! thickness of layer k
DZU, DZT               ! thickness of U,T cell for pbc