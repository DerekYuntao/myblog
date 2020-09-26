---
layout: post
title: "Run a NCL script with command arguments & interpolate variables from hybrid to pressure coordinate"
date: 2020-09-26
description: How to run a NCL script with arguments in command line
share: true
tags:
 - NCL
---

A latest new finding to run NCL stramlined!
Reference:
[NCL command line options and arguments](https://www.ncl.ucar.edu/Document/Manuals/Ref_Manual/NclCLO.shtml)

```powershell
;*************************************************
; hybrid2pres.ncl
;************************************************
; These files are loaded by default in NCL V6.2.0 and newer
; load "$NCARG_ROOT/lib/ncarg/nclscripts/csm/gsn_code.ncl"
; load "$NCARG_ROOT/lib/ncarg/nclscripts/csm/gsn_csm.ncl"
;************************************************
begin
;************************************************
;var0 =(/"Z3","T","Q","OMEGA","U","V"/)
var = var0     ;var0 is from command argument!
print(var)

inf = addfile("b.e13.Bi1850C5.f19_g16.fluxadj.PI.ctl.cam.h0.0131-01.nc","r")  ;info data

prename = "b.e13.Bi1850C5CN.f19_g16.alpha01b.09.cam.h0."
in_ps = addfile(prename + "PS.050001-060312.nc","r")  

indat = addfile(prename + var + ".050001-060312.nc","r")
;************************************************
; read needed variables from file
data_orig = indat->$var$                     ; select variable to ave
P0mb = 0.01*inf->P0                          ; unit hPa
hyam = inf->hyam                             ; get a coefficiants
hybm = inf->hybm                             ; get b coefficiants
PS   = in_ps->PS                             ; get surface pressure: Pa
;************************************************
; define other arguments required by vinth2p
; type of interpolation: 1 = linear, 2 = log, 3 = loglog
;************************************************
interp = 2 
;Is extrapolation desired if data is outside the range of PS
extrap = False
; create an array of desired pressure levels:
pnew = (/100., 125., 150., 175., 200., 225., 250., 300., 350., 400., 450., 500., 550., 600., 650., 700., 750., 775., 800., 825., 850., 875., 900., 925., 950., 975., 1000./)         

;************************************************
; calculate data on pressure levels, and  note
; the 7th argument is not used, and so is set to 1.
;************************************************
dataP = vinth2p(data_orig,hyam,hybm,pnew,PS,interp,P0mb,1,extrap)
dataP@units = data_orig@units
dataP@long_name = data_orig@long_name
printVarSummary(dataP)

;************************************************
;output new files
fout = addfile ("b.e13.Bi1850C5CN.f19_g16.alpha01b.09.cam.h0.pressurelev."+var+".050001-060312.nc", "c")
fout->$var$ = dataP
fout->lon = indat->lon
fout->lat = indat->lat
fout->time = indat->time

end

```

In Linux commad line, we can run *hybrid2pres.ncl* with arguments as follows.
```powershell
ncl hybrid2pres.ncl 'var0="Z3"'
```

Last update: 09/26/2020