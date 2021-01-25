---
layout: post
title: "Download ERA5 in batch"
date: 2021-01-25
description: Download ERA5 data in batch using python
share: true
tags:
 - python
 - Big data
---

## ERA5 URL
[ERA5 monthly averaged data on pressure levels from 1979 to present](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-pressure-levels-monthly-means?tab=form)

[ERA5 monthly averaged data on single levels from 1979 to present](https://cds.climate.copernicus.eu/cdsapp#!/dataset/reanalysis-era5-single-levels-monthly-means?tab=form)

## Presettings
**Step1: install and  Climate Data Store(CDS) API on Windows
    conda config --add channels conda-forge
    conda install cdsapi

**Step2: 
login to CDS and 

**Step3: 
Copy 2 line code displyed on the screen and paste them into a  %USERPROFILE%\.cdsapirc file, where in your windows environment, %USERPROFILE% is usually located at C:\Users\Username folder.

referece:
https://confluence.ecmwf.int/display/CKB/How+to+install+and+use+CDS+API+on+Windows
https://confluence.ecmwf.int/display/CKB/How+to+install+and+use+CDS+API+on+macOS

## Request code
Example to download ERA5.1 monthly mean temperature at 50 hPa at a regular lat/lon grid in NetCDF format.
referece:
<https://confluence.ecmwf.int/display/CKB/How+to+download+ERA5>

```python
#!/usr/bin/env python
import cdsapi
c = cdsapi.Client()
c.retrieve('reanalysis-era5.1-complete', { # Please note the addition '.1' for ERA5.1!
                                           # Keywords 'expver' and 'class' can be dropped. They are obsolete
                                           # since their values are imposed by 'reanalysis-era5.1-complete'
    'date': '2005-01-01',                  # Valid range:  2000-01-01 to 2006-12-31. Always first of the month for monthly means
    'levelist': '50',                      # Pressure level at 50 hPa
    'levtype': 'pl',
    'param': '130.128',                    # Full information at https://apps.ecmwf.int/codes/grib/param-db/
    'stream': 'moda',                      # Monthly means (of Daily means).
    'type': 'an',
    'grid'    : '1.0/1.0',                 # Latitude/longitude grid resolution.
    'format'  : 'netcdf',                  # Output needs to be regular lat-lon, so only works in combination with 'grid'!
}, 'era5.1-temperature-monthly-mean.nc')
```

Expamle to download ERA5 monthly mean single level surface data.
```python
import cdsapi

c = cdsapi.Client()

#modify these two lines
var_name = 'surface_pressure'
output_path = 'I:/ERA5/sp.era5.mon.nc'

c.retrieve(
    'reanalysis-era5-single-levels-monthly-means',
    {
        'format': 'netcdf',
        'grid': '1.0/1.0',  
        'expver': '1',
        'variable': var_name,
        'year': [
            '1979', '1980', '1981',
            '1982', '1983', '1984',
            '1985', '1986', '1987',
            '1988', '1989', '1990',
            '1991', '1992', '1993',
            '1994', '1995', '1996',
            '1997', '1998', '1999',
            '2000', '2001', '2002',
            '2003', '2004', '2005',
            '2006', '2007', '2008',
            '2009', '2010', '2011',
            '2012', '2013', '2014',
            '2015', '2016', '2017',
            '2018', '2019', '2020',
        ],
        'month': [
            '01', '02', '03',
            '04', '05', '06',
            '07', '08', '09',
            '10', '11', '12',
        ],
        'product_type': 'monthly_averaged_reanalysis',
        'time': '00:00',
    },
    output_path)
```

## How to check request status?
Entering:
https://cds.climate.copernicus.eu/live/queue

## What is expver mean?
**MARS 
Identifies the experiment version. Each experiment or model run is assigned a unique code (version). Production data is assigned 1 or 2, and experimental data in Operations 11, 12 ,... IFS research experiments run by ECMWF or by Member States have a four character experiment identifier.

**Dissemination
Identifies the model version. Production data is usually assigned 1. Experimental or pre-operational test data in dissemination, as available, will be given other numbers.


The ERA5 hourly and monthly data are made available with a 3 month delay. This means that after a month has passed, another month's worth of ERA5 data is written to the dataset.

ERA5T (near real time) data are used to fill the gap between the end of the ERA5 data and 5 days before the present date. The oldest month of these is overwritten each month as new ERA5 data become available.

So currently (January 2020):

ERA5 data are currently from 1979 - 31/10/19.
ERA5T data are from 1/11/19- 26/1/20 (5 day delay)

However for data from 00 UTC to 06 on 1/11/19, accumulated variables (fluxes, precipitation) come from ERA5 (which has 'experiment version' 0001), while instantaneous variables (e.g temperature) come from ERA5T (which has 'experiment version' 0005). 
This is because the accumulated variables are derived from the forecast fields of ERA5.

For November 2019, data is separated in the following way:

00-06 UTC on 1/11/19 from ERA5 (expver 0001)
07-23 UTC on 1/11/19 and 2-30/11/19 from ERA5T (expver 0005)
When these data are converted to netCDF a single time coordinate is used which covers the period 1-30/11/19 (720 hourly timesteps).
Both expver versions use the full time extent of time coordinate but the expver 0001 data only covers the first 7 timesteps, the remaining 713 timesteps are 'padded' with empty fields.
For the expver 0005 data, the first 7 timesteps are padded with empty fields, with the remaining timesteps coming from the ERA5T data.

By combing the valid data from each variable, the full ERA5/5T data coverage will be obtained for the entire time range (all of November 2019 in this case).

When the November 2019 ERA5 data are released in February 2020, they will overwrite the ERA5T data for November and for accumulated variables for 00-06 in 1/12/19.

This process will be repeated each month as new ERA5 data are released, overwriting the existing ERA5T data.

reference:
<https://confluence.ecmwf.int/pages/viewpage.action?pageId=124752178>
<https://confluence.ecmwf.int/pages/viewpage.action?pageId=171414041>

Last update: 01/25/2021



