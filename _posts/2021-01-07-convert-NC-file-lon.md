---
layout: post
title: "Convert longitude from 0~360 to -180~180 for a NC file"
date: 2021-01-07
description: How to convert longitude from 0~360 to -180~180 for a NC file
share: true
tags:
 - python
---

If longitude of a NetCDF file is in a form of 0~360 and one want to convert it to -180~180. There are two ways to make it.
## Way1 - CDO:

    cdo sellonlatbox,-180,180,-90,90 input.nc output.nc 
    
`sellonlatbox` is also used for selecting regions in with specified lon and lat. 

## Way2 - pycdo:
```python
from cdo import Cdo 
cdo = Cdo() 

data = cdo.sellonlatbox(
    '-180,180,-90,90', 
    input = 'input.nc', 
    output = 'output.nc'
)     

#or
# data = cdo.sellonlatbox(
#    '-180,180,-90,90', 
#    input = 'input.nc',  
```

Last update: 01/07/2021