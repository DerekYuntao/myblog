---
layout: post
title: "CLM interpinic"
date: 2021-02-27
description: CLM interpinic
share: true
tags:
 - CESM
---

When I run CAM model forced by prescribed SST, there comes an error and model integration abort.

    ERROR:: PFT weights are SIGNIFICANTLY different from the input finidat file and fsurdat file(s).
    ERROR:: maximum difference is  0.100000000000050049E-01  max allowed =  0.500000000000000010E-03
    ERROR:: Run interpinic on your initial condition file to interpolate to the new surface dataset"

What is the problem and how to fix it?
This error happens when the PFT weights have changed between version of the model. We need to create an right initial condition for CLM. Do a 1 day run out-of-the-box and then interpolate to this initial condition using interpinic.

In my case, my CAM-only run `FiC5.f19` (CLM%SP) is a hybrid run forced by either OBS SST/coupled odel SST, with the initial restart files from another coupled run `Bi1850C5.f19_g16.05` (CLM%SP). It is the grid difference of initial conditions between Bi1850C5.f19_g16.05/CLM and FiC5.f19/CLM that cause the problem. Thus, we have to interpolate the grid of the restart file of Bi1850C5.f19_g16.05/CLM to CLM-only grid.

## Using interpinic to interpolate initial conditions to different resolutions
"interpinic" is used to interpolate initial conditions from one resolution to another. In order to do the interpolation you must first run CLM to create a restart file to use as the "template" to interpolate into. Running from arbitrary initial conditions (i.e. finidat = ' ') for a single time-step is sufficient to do this. Make sure the model produces a restart file. You also need to make sure that you setup the same configuration that you want to run the model with, when you create the template file.

**Step1: Perform a 1-day CLM run**
In my case, I choose compset `IiS` to make sure the same configuration with my CAM run. To check supported CLM configuratoin, see [here](https://derekyuntao.github.io/jekyll-clean-dark/2021/02/CESM-tips/). 

Parts of shell code:
```powershell
set casename=f.e13.IiSP.f19.PI.CLMrun

cd .../CESM/scripts
/create_newcase -case /cesm_case/{$casename} -res f19_f19 -mach cheyenne -compset IiSP

cd ~/cesm_case/{$casename}

./xmlchange -file env_run.xml -id RUN_STARTDATE -val 1948-12-31
./xmlchange -file env_run.xml -id DATM_CLMNCEP_YR_ALIGN -val 1948
# cold start
./xmlchange -file env_run.xml -id CLM_FORCE_COLDSTART -val on
./cesm_setup

sed -i '/HAS_F2008_CONTIGUOUS/c\HAS_F2008_CONTIGUOUS:=FALSE' Macros
./{$casename}.build
```

**step2: Use interpinic to interpolate CLM initial conditions**

Command line options to interpinic:
-i = Input filename to interpolate from
-o = Output interpolated file, and starting template file

```powershell
#!/bin/bash
# back up CLM run restart file
cp .../f.e13.IiSP.f19.PI.CLMrun/run/f.e13.IiSP.f19.PI.CLMrun.clm2.r.1949-01-01-00000.nc /glade/scratch/yttp/f.e13.IiSP.f19.PI.CLMrun/run/f.e13.IiSP.f19.PI.CLMrun.clm2.r.1949-01-01-00000.nc.original

#interpolate grid from FiC5.f19 run to IiSP.f19
cd ../CESM/models/lnd/clm/tools/clm4_0/interpinic
#gmake OPT=TRUE SMP=TRUE

interpinic_cheyenne -o .../f.e13.IiSP.f19.PI.CLMrun/run/f.e13.IiSP.f19.PI.CLMrun.clm2.r.1949-01-01-00000.nc -i .../f.e13.FiC5.f19.PI.HadiSST_force/run/b.e13.Bi1850C5.f19_g16.05.clm2.r.0451-01-01-00000.nc

# overwrite restart file b.e13.Bi1850C5.f19_g16.05.clm2.r.0451-01-01-00000.nc for Bi1850C5.f19_g16.05 run
cd .../f.e13.FiC5.f19.PI.HadiSST_force/run/
mv b.e13.Bi1850C5.f19_g16.05.clm2.r.0451-01-01-00000.nc b.e13.Bi1850C5.f19_g16.05.clm2.r.0451-01-01-00000.nc.original
cp .../f.e13.IiSP.f19.PI.CLMrun/run/f.e13.IiSP.f19.PI.CLMrun.clm2.r.1949-01-01-00000.nc b.e13.Bi1850C5.f19_g16.05.clm2.r.0451-01-01-00000.nc
```

Reference:
[Notes on running CESM with COSP using the CESM scripts based on code from the cesm 1.0.4 release](https://www.cgd.ucar.edu/staff/jenkay/cosp/bluefire_scripts/readme.txt)

[Using interpinic to interpolate initial conditions to different resolutions](https://www.cesm.ucar.edu/models/ccsm4.0/clm/models/lnd/clm/doc/UsersGuide/x1779.html)

Last update: 02/27/2021
