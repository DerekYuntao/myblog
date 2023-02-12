---
layout: post
title: "Model data interpolation from pressure level to sigma level"
date: 2023-02-12
description: A self-defined python function for model vertical interpolation
share: true
tags:
 - python
---

The python code below shows how to interpolate a model data from pressure level to sigma level.

```python
"""
@author: Yuntao Bao
contact: bao.291@osu.edu
"""

import numpy as np
import netCDF4 as nc4
import xarray as xr
   
def interp_hybsigma2lev(lev_from, lev_to, datain):
    nlev = len(lev_sigma_to)
    new_data = np.zeros((nt,nlev,ny,nx), dtype='float32')
    print('Start linear interpolation:')
    for t in range(nt):
        print('OK', t)
        for j in range(ny):
            for i in range(nx):
                Finterp = interpolate.interp1d(lev_from[t,:,j,i], datain[t,:,j,i], 
                                                kind='linear', fill_value='extrapolate')    
                new_data[t,:,j,i] = Finterp(lev_to)
    return new_data

if __name__ == "__main__":
    # modify here to change the variable
    var = 'Q' 

    # input data file with a variable to be interpolated; must also include the pressure level values (e.g. 'lev_p' in CESM)
    fin = 'f.e13.Fi1850C5.f02_f02.cam.h0.'+var+'.nc'
    # input data file with a variable 'surface pressure'
    fps = ps_dir + 'f.e13.Fi1850C5.f02_f02.cam.h0.PS.nc'

    dsdat = nc4.Dataset(fin)
    dsps = nc4.Dataset(fps)
    data = dsdat[var][:].data
    ps = dsps['PS'][:].data/100   # hPa

    plev = dsdat['lev_p'][:].data
    lon = dsdat['lon'][:].data
    lat = dsdat['lat'][:].data
    time = dsdat['time'][:].data
    [nx, ny, nz, nt] = len(lon), len(lat), len(lev), len(time)
    print(lev)

    # broadcasting the arrys
    lev_sigma_from = plev[None, :, None, None]/PS[:, None, :, :]
    # You can also define your own sigma level array!
    lev_sigma_to = np.array([0.1,0.15,0.2,0.25,0.3,0.35,0.4,0.45,0.5,0.55,0.6,\
                            0.65,0.7,0.75,0.78,0.81,0.84,0.87,0.9,0.93,\
                            0.96,1.0], dtype='float32')
    print(lev_sigma_to) 

    # call function to interpolate
    data_new = interp_plev2sigma(lev_sigma_from, lev_sigma_to, data)
    print('End interpolation')

```     

Last updated: 02/12/2023