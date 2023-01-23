---
layout: post
title: "Tips for NCO/CDO"
date: 2021-02-19
description:  Some frequently used tips for NCO/CDO
share: true
tags:
 - Linux
 - Big data
---

## Rename a variable: ncrename  ##
-a
old_name, new_name Attribute renaming. 
-d
old_name, new_name Dimension renaming. 
-v
old_name, new_name Variable renaming.
-i
Interactive. ncrename will prompt for confirmation before overwriting an existing file.

e.g. Rename the variable p to pressure and t to temperature in netCDF in.nc. In this case p must exist in the input file, but the presence of t is optional:

    ncrename -v p,pressure -v .t,temperature atm202102.nc

ncrename does not automatically attach dimensions to variables of the same name. If you want to rename a coordinate variable:

    ncrename -d lon,longitude -v lon,longitude atm202102.nc

Create netCDF out.nc identical to in.nc except the attribute _FillValue is changed to missing_value:

    ncrename -a _FillValue,missing_value atm202102.nc

## Extract a level of NC data: ncks ##
-d <variable of the level>, <number of the level>: extract data at “n”th level from depth coordinate.

    ncks -F -d z_t,1 b.e13.Bi1850C5CN.f19_g16.alpha01b.09.pop.h.TEMP.050001-060312.nc  b.e13.Bi1850C5CN.f19_g16.alpha01b.09.pop.h.potenSST5m.050001-060312.nc

## Calculate difference for variables between two NC files: ncdiff ##
subtracts variables in file2 from the corresponding variables (those with the same name) in file1 and stores the results in output file3. 
The difference variables must be the same name in two NC files.
e.g.

    ncdiff -v SST_cpl /glade/p/cesmdata/cseg/inputdata/atm/cam/sst/sst_HadOIBl_bc_1x1_2000climo_c180511.nc PHC_WOA_SST5m_1deg.nc OBS_sst_climo_diff.nc

## Change longitude range from 0~360 to -180~180 ##

    cdo sellonlatbox,-180,180,-90,90 infile.nc outfile.nc

## Time mean: climatology

    ncwa -a time,bnds b.e13.Bi1850C5.f19_g16.05.pop.h.TEMP.ltm.040001-049912_0.nc b.e13.Bi1850C5.f19_g16.05.pop.h.TEMP.ltm.040001-049912.nc

    cdo timmean b.e13.Bi1850C5.f19_g16.05.pop.h.TEMP.040001-049912.nc b.e13.Bi1850C5.f19_g16.05.pop.h.TEMP.ltm.040001-049912.nc

Last update: 01/23/2023    