---
layout: post
title: "Automatically generate all file names in a directory in Python"
date: 2022-06-01
description: How to generate all file names in a directory and output them in an ascii file.
share: true
tags:
 - python
---

References:
[How to use Glob() function to find files recursively in Python?](https://www.geeksforgeeks.org/how-to-use-glob-function-to-find-files-recursively-in-python/)


```python
import xarray as xr
import glob

#case = 'f.e13.Fi1850C5CN.f19.20ka.ice.tagging_18O.01'
case = 'f.e13.Fi1850C5CN.f19.13.2ka.ice_ghg_orb_wtr.tagging_18O.01'
cs =  '13.2ka'

var = ['H216OV', 'H218OV',
    'AFR18OV', 'ANT18OV', 'ARC18OV', 'AUS18OV', 'CAS18OV', 'EAS18OV',
    'EEP18OV', 'EQA18OV', 'EQI18OV', 'EUR18OV', 'GRN18OV', 'NAM18OV',
    'NAS18OV', 'NNA18OV', 'NNP18OV', 'SAM18OV', 'SAS18OV', 'SNA18OV',
    'SNP18OV', 'SOC18OV', 'SSA18OV', 'SSI18OV', 'SSP18OV', 'TAS18OV',
    'WEP18OV',
    'AFRV', 'ANTV', 'ARCV', 'AUSV', 'CASV', 'EASV',
    'EEPV', 'EQAV', 'EQIV', 'EURV', 'GRNV', 'NAMV',
    'NASV', 'NNAV', 'NNPV', 'SAMV', 'SASV', 'SNAV',
    'SNPV', 'SOCV', 'SSAV', 'SSIV', 'SSPV', 'TASV',
    'WEPV']

rootdir = '/' + case + '/tseries/atm/h0/'
tardir = '/' + cs + '/press_lev/'
os.system('mkdir -p ' + tardir)

fl = glob.glob(rootdir + case + '*.nc')
if cs != '20ka':
    fl1 = [f for f in fl for v in var if '.' +  v + '.' in f and 'monthly' not in f]
else:    
    fl1 = [f for f in fl for v in var if '.' +  v + '.' in f]

Nline = len(fl1)
fout= '/tagging_hybrlev_filename_Ovapor_'+cs+'.txt'
with open(fout, 'wt') as f:
    for line in fl1:
        f.write(line + '\n')

```

Last update: 2022/06/01