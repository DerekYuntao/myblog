---
layout: post
title: "Calculate local mass stream function"
date: 2021-02-15
description: Derivation and calculation of local mass stream function
share: true
tags:
 - Research
 - python
---

## Regional partitioning of the overturning circulation

Keyser et al. (1989) et al. developed a method to partition a three-dimensional divergent flow into an independent pair of orthogonal, two-dimensional circulations, with both satisfying continuities. This method is possible to uniquely attribute part of the vertical motion to the circulation in the zonal direction and part to that in the meridional direction. Schwendike et al. (2014) decomposed the divergent overturning circulation into the meridional and zonal components. The stream function of the Hadley circulation is derived from the divergent meridional wind and the stream function for the Walker circulation was derived from the divergent zonal wind. This approach is proved to be useful for defining regionally averaged overturning circulations.

## Derivation of the regional mass stream function

Since only the divergent component of wind produce the vertical motion, the local mass continuity equation can be written as:
$\frac{∂u_D}{∂x}\quad + \frac{∂v_D}{∂x}\quad + \frac{∂ω}{∂p}\quad = 0$   (1)
Subscript D represents for divergent wind. 
The continuity equation can be decomposed into zonal and meridional parts:
(∂u_D)/∂x+(∂ω_x)/∂p=0   (2.1)
(∂v_D)/∂y+(∂ω_y)/∂p=0   (2.2)
Subscripts x and y represents for vertical wind deduced by zonal and meridional circulations. 
The zonal continuity equation indicates a stream function on x-z plan:
u_D=(∂ψ_x)/∂p and ω_x=-(∂ψ_x)/∂x   (3.1)
The meridional continuity equation indicates a stream function on y-z plan:
v_D=(∂ψ_y)/∂p and ω_y=-(∂ψ_y)/∂y   (3.2)
The corresponding vectors form of Eq.3.1 and Eq.3.2 is:
v_D=∂ψ/∂p and ω=-∇ψ   (3.3)
where ψ=(ψ_x, ψ_y) and ∇=(∂/∂x, ∂/∂y).

***Note:*** There is a problem that either u_D=(∂ψ_x)/∂p;  ω_x=-(∂ψ_x)/∂x and u_D=-(∂ψ_x)/∂p;  ω_x=(∂ψ_x)/∂x are solutions for Eq 2.1. **How to check which is the solution?? I don't know yet...**

Meridional mass stream function ψ_y is defined as the northward mass flux above a particular pressure P. In sphere coordinate, the zonal mean meridional mass stream function ψ_ϕ is:
ψ_ϕ=∫_z^∞▒dz ∫_(λ_1)^(λ_2)▒〖ρ[v_D]acosϕdλ〗
=acosϕ/g ∫_0^p▒dp ∫_(λ_1)^(λ_2)▒[v_D ]dλ
=acosϕ∆λ/g ∫_0^p▒[v_D ]dp     (4)
where [v_D] is zonal mean meridional divergent wind; ϕ is latitude in radian and λ is longitude in radian; ∆λ is the width of the regional zonal belt in radian.

Zonal mass stream function ψ_y is defined as the eastward mass flux above a particular pressure P. In sphere coordinate, the meridional mean zonal mass stream function ψ_λ is:
ψ_λ=∫_z^∞▒dz ∫_(ϕ_1)^(ϕ_2)▒ρ〈u_D 〉adϕ
=a∆ϕ/g ∫_0^p▒〈u_D 〉dp    (5)
where 〈u_D 〉 is meridional mean zonal divergent wind, ∆ϕ is the width of the regional meridional belt in radian.

The vertical velocity in sphere coordinate is:
(ω_λ,〖 ω〗_ϕ )=-((∂ψ_λ)/acosϕ∂λ,(∂(ψ_ϕ cosϕ))/acosϕ∂ϕ)   (6)
Regional mass stream function (MSF) can depict the thermodynamic circulation. The vertical motion of the partitioned zonal and meridional overturning circulations can be retrieved from the gradient of the partitioned MSF (Eq.6).   

## Part of code
```python
#%% find the start pressure level above the surface
st_lev_uall = []
for k in range(N):
    st_lev_v = np.zeros((12, ny_v[k], nx_v[k]), dtype="float")
    for t in range(12):   #time in month
        for j in range(ny_v[k]):   # lat
            for i in range(nx_v[k]):   #lon
                if spv_all[k][t,j,i] >= 1000.:
                    st_lev_v[t,j,i] = nz-1
                else:    
                    st_lev_v[t,j,i] = np.where(lev > spv_all[k][t,j,i])[0][0]-1      
    st_lev_vall.append(st_lev_v)       

#%% calculate mass stream function   
# length in radius
delta_theta = np.zeros(3, "float")
delta_phi = np.zeros(3, "float")
vconst = []
for k in range(N):
    # y direction
    delta_phi[k] = angle2rad(xrange[k][1] - xrange[k][0])
    delta_theta[k] = angle2rad(yrange[k][1] - yrange[k][0])
    vconst.append(a*np.cos(angle2rad(lat[30:151]))*delta_phi[k]/g)

# calculate vertical integral       
vmass_all = []
for k in range(N):
    vmass = np.zeros((12, nz, ny_v[k], nx_v[k]), dtype="float")
    for t in range(12):
        for j in range(ny_v[k]):
            for i in range(nx_v[k]):
                z = 0
                vmass[t,z,j,i] = vd_all[k][t,z,j,i]
                while z < st_lev_vall[k][t,j,i]:
                    z = z+1
                    vmass[t,z,j,i] = vmass[t,z-1,j,i] + \
                        (vd_all[k][t,z,j,i] + vd_all[k][t,z-1,j,i])*(lev[z]-lev[z-1])*100./2                                  
    vmass_all.append(vmass)                    
    
# calculate zonal or meridional mean
mt, mz, my, _ = np.shape(vmass_all[0])   
vmass_m = np.zeros((3, mt, mz, my), dtype="float")
for k in range(N):
    vmass_m[k,:,:,:] = np.nanmean(vmass_all[k],3)
```        

## Reference
Keyser, D., Schmidt, B. D., & Duffy, D. G. (1989). A technique for representing three-dimensional vertical circulations in baroclinic disturbances. Monthly weather review, 117(11), 2463-2494.
Schwendike, J., Govekar, P., Reeder, M. J., Wardle, R., Berry, G. J., & Jakob, C. (2014). Local partitioning of the overturning circulation in the tropics and the connection to the Hadley and Walker circulations. Journal of Geophysical Research: Atmospheres, 119(3), 1322-1339.