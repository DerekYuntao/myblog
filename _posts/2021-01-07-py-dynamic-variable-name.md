---
layout: post
title: "Generate dynamic variable names in python"
date: 2021-01-07
description: How to generate dynamic variable names in python
share: true
tags:
 - python
---

Generate **dynamic local variable names** in python using `locals()[<varname_variable>]`. 
e.g.

    #variable name is a variable
    var_need = "TS"
    locals()[var_need+"_ANN"] = fw.createVariable(var_need + "_ANN", 'float', ("time", "lat", "lon"))
    locals()[var_need+"_DJF"] = fw.createVariable(var_need + "_DJF", 'float', ("time", "lat", "lon"))
    locals()[var_need+"_MAM"] = fw.createVariable(var_need + "_MAM", 'float', ("time", "lat", "lon"))
    locals()[var_need+"_JJA"] = fw.createVariable(var_need + "_JJA", 'float', ("time", "lat", "lon"))
    locals()[var_need+"_SON"] = fw.createVariable(var_need + "_SON", 'float', ("time", "lat", "lon"))       

output:

    TS_ANN = fw.createVariable(var_need + "_ANN", 'float', ("time", "lat", "lon"))
    TS_DJF = fw.createVariable(var_need + "_DJF", 'float', ("time", "lat", "lon"))
    TS_MAM = fw.createVariable(var_need + "_MAM", 'float', ("time", "lat", "lon"))
    TS_JJA = fw.createVariable(var_need + "_JJA", 'float', ("time", "lat", "lon"))
    TS_SON = fw.createVariable(var_need + "_SON", 'float', ("time", "lat", "lon"))

Last update: 01/07/2021

