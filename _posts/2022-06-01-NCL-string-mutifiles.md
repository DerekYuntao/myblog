---
layout: post
title: "NCL script: read string array from an ascii file and subtract variable names"
date: 2022-06-01
description: How to read string arrays, split and subtract strings in NCL
share: true
tags:
 - NCL
---

Reference:
Reads a file that contains ASCII representations of basic data types.
[asciiread](https://www.ncl.ucar.edu/Document/Functions/Built-in/asciiread.shtml)

Splits a string into an array of strings given one or more delimiters.
[str_split](https://www.ncl.ucar.edu/Document/Functions/Built-in/str_split.shtml)


```powershell

;*************************************************
; hybrid_pres
;************************************************
; These files are loaded by default in NCL V6.2.0 and newer
; load "$NCARG_ROOT/lib/ncarg/nclscripts/csm/gsn_code.ncl"
; load "$NCARG_ROOT/lib/ncarg/nclscripts/csm/gsn_csm.ncl"
;************************************************
begin
;************************************************

case = "PI" ;"20ka"

prename = "*/f.e13.Fi1850C5CN.f19.PI.tagging_18O.01.cam.h0."

fname = asciiread("*/tagging_hybrlev_filename_Ovapor_" + case + ".txt", -1, "string")  
print(fname)
Nm = dimsizes(fname)
nrows = dimsizes(fname)

do i = 0, Nm-1
    strs = str_split(fname(i), ".")
    if (case .eq. "20ka") then
        var = strs(17)
    else if (case .eq. "15.5ka") then
        var = strs(18)
    else if (case .eq. "PI") then
        var = strs(15)
    end if 
    end if
    end if   

    indat = addfile(fname(i), "r")  
    ;************************************************
    ; read needed variables from file
    data_orig = indat->$var$            ;subtracted variable name
   
    fout_name = **

    ; remove existing file
    if fileexists(fout_name) then
        system("rm -rf " + fout_name)
    end if   

    setfileoption("nc","Format","NetCDF4Classic")
    fout = addfile(fout_name, "c")

    ;In order to write large (> 2 GB) variables to a NC file, we have to set the NetCDF "Format" option to 
    ;"LargeFile" or to "NetCDF4Classic". 
    ; below is a large NC file to output
    fout->$var$ = dataP
    fout->lon = indat->lon
    fout->lat = indat->lat
    fout->time = indat->time
    fout->lev_p = pnew

end do

end
```

Last update: 06/01/2022