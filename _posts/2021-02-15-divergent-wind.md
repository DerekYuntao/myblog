---
layout: post
title: "Calculate Divergent Wind and Potential Velocity in NCL"
date: 2021-02-15
description: How to calculate divergent wind and potential velocity in NCL
share: true
tags:
 - NCL
 - Research
---

``` powershell
; divwnd_potenfunc_calculate
; calculate divergence wind and potential function 
;************************************************
; These files are loaded by default in NCL V6.2.0 and newer
; load "$NCARG_ROOT/lib/ncarg/nclscripts/csm/gsn_code.ncl"
; load "$NCARG_ROOT/lib/ncarg/nclscripts/csm/gsn_csm.nc"

;*****************************************************************
; self-defind function to calculate climatology mean for each month
function clim_monmean(dimsz, datain, Ndim)
begin
; calculate monthly climatology
nt = dimsz(0)/12

if Ndim .eq. 4 then
data_mon = new((/nt, 12, dimsz(1), dimsz(2), dimsz(3)/), "float")
do i = 0,11
    data_mon(:,i,:,:,:) = datain(i:dimsz(0)-1:12,:,:,:)
end do
data_clim = dim_avg_n(data_mon, 0)

else   ;for surface pressure
data_mon = new((/nt, 12, dimsz(2), dimsz(3)/), "float")
do i = 0,11
    data_mon(:,i,:,:) = datain(i:dimsz(0)-1:12,:,:)
end do
data_clim = dim_avg_n_Wrap(data_mon, 0)
end if

return data_clim
end

function chi_calcu(dimsz, div, index)
begin
;calculate velocity potential
chi = new((/12, dimsz(1), dimsz(2), dimsz(3)/), "float")
do it=0,11
do iz=0,dimsz(1)-1
    ;Values must be in ascending latitudinal order
    if index .eq. 1 then
    chi(it,iz,:,:) = ilapsG_Wrap(div(it,iz,::-1,:), 0)
    else
    chi(it,iz,:,:) = ilapsG_Wrap(div(it,iz,:,:), 0)
    end if
end do
end do
chi@long_name = "velocity potential"
chi@units     = "m/s" 
return chi
end

;******************************************************************

;main function
;************************************************
begin
;************************************************
; read data
top_lev = 1   ;1hPa
PI_start = 0   ; for model only
PI_end = 719   ; for model only
index = 2     ;1:OBS;  2:Model   ;modify here!!

if (index .eq. 1) then     ;OBS
dir = "..."
f1 = addfile(dir + "uwnd.era5.mon.1000-1hPa.nc","r")
f2 = addfile(dir + "vwnd.era5.mon.1000-1hPa.nc","r")
f3 = addfile(dir + "sp.era5.mon.nc","r")

u0 = short2flt(f1->u(:,{top_lev:1000},:,:))  
v0 = short2flt(f2->v(:,{top_lev:1000},:,:))  
sp0 = short2flt(f3->sp)

;calculate climatology mean for each month
dimsz = dimsizes(u0)
print(dimsz)
u = clim_monmean(dimsz, u0, 4)
v = clim_monmean(dimsz, v0, 4)
sp = clim_monmean(dimsz, sp0(:,::-1,:), 3)    ;must reverse lat to -90-90 for ERA5 data!!
copy_VarMeta(u0(0,:,:,:), u(0,:,:,:))
copy_VarMeta(v0(0,:,:,:), v(0,:,:,:))

else     ;Model
dir = "..."
f1 = addfile(dir + "b.e13.Bi1850C5CN.f19_g16.alpha01b.09.cam.h0.pressurelev.1000-1hPa.U.050001-060312.nc","r")
f2 = addfile(dir + "b.e13.Bi1850C5CN.f19_g16.alpha01b.09.cam.h0.pressurelev.1000-1hPa.V.050001-060312.nc","r")
f3 = addfile(dir + "b.e13.Bi1850C5CN.f19_g16.alpha01b.09.cam.h0.PS.050001-060312.nc","r")
u0 = f1->U(PI_start:PI_end,{top_lev:1000},:,:)
v0 = f2->V(PI_start:PI_end,{top_lev:1000},:,:)
sp0 = f3->PS(PI_start:PI_end,:,:)

;calculate climatology mean for each month
dimsz = dimsizes(u0)
print(dimsz)
u = clim_monmean(dimsz, u0, 4)
v = clim_monmean(dimsz, v0, 4)
sp = clim_monmean(dimsz, sp0, 3)
copy_VarMeta(u0(0,:,:,:), u(0,:,:,:))
copy_VarMeta(v0(0,:,:,:), v(0,:,:,:))
copy_VarMeta(sp0(0,:,:), sp(0,:,:))

end if
printVarSummary(sp)
printVarSummary(u)

;*****************************************************************************
;calculate divergent wind
; ************  OBS
if (index .eq. 1) then  
; calculate divergence  (global) 
div = uv2dv_cfd(u, v, u&latitude, u&longitude, 3)  ;3 for cyclic in longitude
copy_VarMeta(u, div)
print("OK divergence")

; calculate divergent wind components gobally
; input values must be in ascending latitude order and array must be on a global grid
; output: ud and vd
;ud = new(dimsizes(div),typeof(div))	; zonal divergent wind 
;vd = new(dimsizes(div),typeof(div))	; meridional divergent wind
;copy_VarMeta(u, ud)
;copy_VarMeta(v, vd)
;printVarSummary(ud)
;print(ud&latitude)
;dv2uvf(div,ud(:,:,::-1,:),vd(:,:,::-1,:))  ; fixed grid 

divwnd = dv2uvF_Wrap(div(:,:,::-1,:))  ;fixed grid
; input values must be in ascending latitude order
printVarSummary(divwnd)
ud = divwnd(0,:,:,:,:)
vd = divwnd(1,:,:,:,:)
printVarSummary(ud)
;print(ud&latitude)
delete(u)
delete(v)

;clculate velocity potential
chi = chi_calcu(dimsz, div, index)
copy_VarMeta(div(0,:,0,0), chi(0,:,0,0))
printVarSummary(chi)
;print(chi&latitude)


; ************ Model
else   
; calculate divergence  (global)  
div = uv2dv_cfd(u, v, u&lat, u&lon, 3)
copy_VarMeta(u, div)

; calculate divergent wind components gobally
; input values must be in ascending latitude order and array must be on a global grid
divwnd = dv2uvG_Wrap(div(:,:,:,:))  ;Gussian grid
; input values must be in ascending latitude order
printVarSummary(divwnd)
ud = divwnd(0,:,:,:,:)
vd = divwnd(1,:,:,:,:)
printVarSummary(ud)
;print(ud&lat)
delete(u)
delete(v)

;clculate velocity potential
chi = chi_calcu(dimsz, div, index)
copy_VarMeta(div(0,:,0,0), chi(0,:,0,0))
printVarSummary(chi)
;print(chi&lat)
end if

;********************************************************************************
; output  files
out_dir = "..."
if  (index.eq.1) then
if fileexists(out_dir + "ERA5.divwnd.ltm.mon.nc") then
    system("rm -rf "+out_dir + "ERA5.divwnd.ltm.mon.nc")
end if    
fout = addfile (out_dir + "ERA5.divwnd.ltm.mon.nc", "c")
fout->ud = ud
fout->vd = vd
fout->chi = chi
fout->sp = sp

else
if fileexists(out_dir + "PI.ctl.divwnd.ltm.mon.nc") then
    system("rm -rf "+out_dir + "PI.ctl.divwnd.ltm.mon.nc")
end if    
fout = addfile (out_dir + "PI.ctl.divwnd.ltm.mon.nc", "c")
fout->ud = ud
fout->vd = vd
fout->chi = chi
fout->sp = sp
end if

end

```
Last update: 02/15/2021