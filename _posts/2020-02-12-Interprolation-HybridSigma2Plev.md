---
layout: post
title: "CESM data interpolation from hybrid-sigma level to pressure level"
date: 2023-02-12
description: A self-defined python function for model vertical interpolation
share: true
tags:
 - python
 - CESM
---

The python code below shows how to interpolate a data from CESM hybrid sigma level to pressure level.

```python
"""
Created on Wed Nov 16 2022

@author: Yuntao Bao
contact: bao.291@osu.edu
"""

import numpy as np
import netCDF4 as nc4
import xarray as xr
   
def interp_hybsigma2lev(lev_from, lev_to, datain):
    nlev = len(lev_to)  
    print('Start linear interpolation:')
    new_data = np.zeros((nt,nlev,ny,nx), dtype='float32')
    for t in range(nt):
        print('OK', t)
        for j in range(ny):
            for i in range(nx):
                new_data[t,:,j,i] = np.interp(lev_to , lev_from[t,:,j,i], datain[t,:,j,i])    
    return new_data

if __name__ == "__main__":
    # modify here to change the variable
    var = 'Q' 

    # must included a CESM output file with 'hyam' and 'hybm' inside
    info = nc4.Dataset('.../f.e13.Fi1850C5.f02_f02.cam.h0.000101-002512.nc','r') 
    hyam = info['hyam'][:].data
    hybm = info['hybm'][:].data
    P0 = 1000

    # input data file with a variable to be interpolated
    fin = 'f.e13.Fi1850C5.f02_f02.cam.h0.'+var+'.nc'
    # input data file with a variable 'surface pressure'
    fps = ps_dir + 'f.e13.Fi1850C5.f02_f02.cam.h0.PS.nc'

    dsdat = nc4.Dataset(fin)
    dsps = nc4.Dataset(fps)
    data = dsdat[var][:].data
    ps = dsps['PS'][:].data/100   # hPa

    lev = dsdat['lev_p'][:].data
    lon = dsdat['lon'][:].data
    lat = dsdat['lat'][:].data
    time = dsdat['time'][:].data
    [nx, ny, nz, nt] = len(lon), len(lat), len(lev), len(time)
    print(lev)

    press = P0*hyam[None,:,None,None] + hybm[None,:,None,None]*ps[:,None,:,:]
    lev_from = press
    # You can also define your own pressure level array!
    lev_to = np.array([100., 125., 150., 175., 200., 225.,\
                    250., 300., 350., 400., 450., 500., 550., \
                    600., 650., 700., 750., 775., 800., 825., \
                    850., 875., 900., 925., 950., 975., 1000.], dtype='float32') 
    print(lev_from) 
    print(lev_to)

    # call function to interpolate
    data_new = interp_hybsigma2lev(lev_from, lev_to, data)
    print('End linear interpolation')

```     

If you want to contiune interpolate data in pressure level to pure sigma level, see: [Model data interpolation from pressure level to sigma level](https://novarizark.github.io/2018/01/28/decoupling-cesm/)

Last updated: 02/12/2023