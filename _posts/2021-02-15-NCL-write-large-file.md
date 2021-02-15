---
layout: post
title: "Write large NC file in NCL"
date: 2021-02-15
description: How to output a large NC file (>2GB) in NCL
share: true
tags:
 - NCL
---

Sets a number of file-format-specific options in ***setfileoption***.

For NC file formate, a value of "Classic" indicates that a standard NetCDF file should be created. Standard NetCDF files are more limited with respect to size. Assuming the underlying file system has support for large files, the total size can exceed 2 GB, but there are severe restrictions regarding the number of large variables and the order in which they are written. In general, the standard format should be used if and only if it is known that the total file size will be less than 2 GB.

The two synonymous values, "LargeFile" and "64BitOffset", indicate that a NetCDF file with support for larger variables and a theoretically much larger total size (about 9.22e+18 bytes) should be created. Each fixed-size variable, or each 'record' (first dimension) of a variable with an unlimited dimension of a 64-bit offset file can have a size of up to 4 GB.

In order to write large variables (>2GB) to a NetCDF file, you must set NetCDF Format option to *LargeFile* or *NetCDF4Classic *before you open the file for creation.
e.g.
    setfileoption("nc","Format","NetCDF4Classic")
    fout = addfile ("TEMP_large.nc", "c")

Reference:
https://www.ncl.ucar.edu/Document/Functions/Built-in/setfileoption.shtml

Last update: 02/15/2021