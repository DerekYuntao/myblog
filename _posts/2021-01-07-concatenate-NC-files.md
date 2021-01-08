---
layout: post
title: "Concatenante Multiple NC Files"
date: 2021-01-07
description: Several ways to concatenate NC files
share: true
tags:
 - python
 - Big data
---

There are several ways to concatenante NectCDF files into a time series:
## Way1 - CDO:

    cdo cat b.e13.Bi1850C5.f19_g16.TS.0001-0999.ANN.nc b.e13.Bi1850C5.f19_g16.TS.1000-1999.ANN.nc b.e13.Bi1850C5.f19_g16.TS.2000-2999.ANN.nc ... b.e13.Bi1850C5.f19_g16.TS.TS.8000-8999.ANN.nc TS.test.cdo.merge.nc

## Way2 - pycdo:
To install this package with conda run one of the following:

    conda install -c conda-forge python-cdo 
    conda install -c conda-forge/label/gcc7 python-cdo 
    conda install -c conda-forge/label/cf201901 python-cdo 
    conda install -c conda-forge/label/cf202003 python-cdo 

code:
```python
from cdo import Cdo
cdo = Cdo()
...
#fname is a list of file path
cdo.cat(input = [x for x in fname],output = fout_name)  
```
Unfortunately, pycdo is not applicable for variable like MOC in pop.

## Way3 - NCO:

    ncrcat -h b.e13.Bi1850C5.f19_g16.TS.0001-0999.ANN.nc b.e13.Bi1850C5.f19_g16.TS.1000-1999.ANN.nc b.e13.Bi1850C5.f19_g16.TS.2000-2999.ANN.nc ... b.e13.Bi1850C5.f19_g16.TS.TS.8000-8999.ANN.nc TS.test.cdo.merge.nc

## Way4 - pynco:
```python
from nco import Nco
nco = Nco()
...
nco.ncrcat(input = [x for x in fname],output = fout_name)  
```
Unfortunately, pynco is not applicable for variable like MOC in pop.

## Way5 - xarray: 

`open_mfdataset` open multiple files as a single dataset.
```python
    import xarray as xr
    ...
    time = np.arange(20000-5,11000-5,-10)
    merged_ds = xr.open_mfdataset(fname, combine='by_coords', concat_dim='time')
    merged_ds["time_bp"]=(['time'], time)
    print(merged_ds)
    
    merged_ds.to_netcdf(path = fout_name, encoding = {'TS': {'dtype':'float32'}})
```    


If all the data is loaded to workspace from input file list, then how do we concatenates these data in to a single array?

转载自blog:
**从xarray走向netCDF处理：合并与计算**
Reference: <http://www.meteoai.cn/post/%E4%BB%8Exarray%E8%B5%B0%E5%90%91netcdf%E5%A4%84%E7%90%86%E5%90%88%E5%B9%B6%E4%B8%8E%E8%AE%A1%E7%AE%97/>

数据合并主要是两种形式

维度的拼接：如将日数据合成为年数据，就属于在时间维度上的合并。

变量的合并：如将多个物理量合到同一个*Dataset*中。

xarray围绕着这两种合并方式介绍了concatenate, merge, combine, update四种方法。我在这里就挑最常用的跟大家聊聊。

**维度拼接**

使用 concat() 方法可以实现维度的拼接。

下面是演示数据，来源于2018年和2019年前三个月的ERA-Interim月平均数据。
```python
>>> ds2018
<xarray.Dataset>
Dimensions:    (latitude: 241, longitude: 480, time: 12)
Coordinates:
  * longitude  (longitude) float32 0.0 0.75 1.5 2.25 ... 357.75 358.5 359.25
  * latitude   (latitude) float32 90.0 89.25 88.5 87.75 ... -88.5 -89.25 -90.0
  * time       (time) datetime64[ns] 2018-01-01 2018-02-01 ... 2018-12-01
Data variables:
    u10        (time, latitude, longitude) float32 ...
    v10        (time, latitude, longitude) float32 ...
    t2m        (time, latitude, longitude) float32 ...
Attributes:
    Conventions:  CF-1.6

>>> ds2019
<xarray.Dataset>
Dimensions:    (latitude: 241, longitude: 480, time: 3)
Coordinates:
  * longitude  (longitude) float32 0.0 0.75 1.5 2.25 ... 357.75 358.5 359.25
  * latitude   (latitude) float32 90.0 89.25 88.5 87.75 ... -88.5 -89.25 -90.0
  * time       (time) datetime64[ns] 2019-01-01 2019-02-01 2019-03-01
Data variables:
    u10        (time, latitude, longitude) float32 ...
    v10        (time, latitude, longitude) float32 ...
    t2m        (time, latitude, longitude) float32 ...
Attributes:
    Conventions:  CF-1.6
```    
ds2018时间维度为12，ds2019时间维度为3，下面使用 concat() 合并后时间维度为15
```python
>>> xr.concat([ds2018, ds2019], dim='time')
<xarray.Dataset>
Dimensions:    (latitude: 241, longitude: 480, time: 15)
Coordinates:
  * longitude  (longitude) float32 0.0 0.75 1.5 2.25 ... 357.75 358.5 359.25
  * latitude   (latitude) float32 90.0 89.25 88.5 87.75 ... -88.5 -89.25 -90.0
  * time       (time) datetime64[ns] 2018-01-01 2018-02-01 ... 2019-03-01
Data variables:
    u10        (time, latitude, longitude) float32 -0.9599868 ... 4.5229325
    v10        (time, latitude, longitude) float32 3.1737509 ... -2.289166
    t2m        (time, latitude, longitude) float32 248.46857 ... 225.19632
Attributes:
    Conventions:  CF-1.6
```

**变量合并**

使用 merge() 方法，可以将ds2018中的u10和ds2019中的t2m合并到一起，而且在时间维上缺失会自动设置为nan。
```python
>>> xr.merge([ds2018.u10, ds2019.t2m])
<xarray.Dataset>
Dimensions:    (latitude: 241, longitude: 480, time: 15)
Coordinates:
  * time       (time) datetime64[ns] 2018-01-01 2018-02-01 ... 2019-03-01
  * longitude  (longitude) float32 0.0 0.75 1.5 2.25 ... 357.75 358.5 359.25
  * latitude   (latitude) float32 90.0 89.25 88.5 87.75 ... -88.5 -89.25 -90.0
Data variables:
    u10        (time, latitude, longitude) float32 -0.9599868 -0.9599868 ... nan
    t2m        (time, latitude, longitude) float32 nan nan ... 225.19632
```

**数据计算**

最基本的计算就是进行加减乘除，任意一个DataArray或者Dataset都可以直接进行四则运算。

除此以外，xarray还可以帮你快速地求出平均值，方差，最小值，最大值等。你可以指定具体对那个维度进行计算，如果不指定维度默认会对所有维度进行计算。

比如要对经、纬两个维度进行平均，最后的结果只有时间维的12个值。

而且xarray在时间维上的计算还有很多贴心的用法，比如月数据转年数据，月数据转季节数据。
```python
>>>temp = (ds['t2m'] - 273.15).groupby('time.season')

>>>ds2018.std(dim='time')
<xarray.Dataset>
Dimensions:    (latitude: 241, longitude: 480)
Coordinates:
  * longitude  (longitude) float32 0.0 0.75 1.5 2.25 ... 357.75 358.5 359.25
  * latitude   (latitude) float32 90.0 89.25 88.5 87.75 ... -88.5 -89.25 -90.0
Data variables:
    u10        (latitude, longitude) float32 1.9192954 1.9192954 ... 1.2133
    v10        (latitude, longitude) float32 1.3066719 1.3066719 ... 1.577495
    t2m        (latitude, longitude) float32 9.5681305 9.5681305 ... 11.313364
```

**Add a variable into a NC file**
```python
data_set=xr.Dataset(coords={'lon': (['x', 'y'], lon),
                    'lat': (['x', 'y'], lat),
                    'time': pd.date_range('2021-01-01', periods=3)})
temp=...
data_set["Temperature"]=(['x', 'y', 'time'],  temp)
```

Last update: 01/07/2021

