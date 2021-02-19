---
layout: post
title: "Calculate Local Mass Stream Function"
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
Meridional mass stream function $ψ_y$ is defined as the northward mass flux above a particular pressure P. In sphere coordinate, the zonal mean meridional mass stream function $ψ_ϕ$ is:

$$
\begin{align}
ψ_ϕ = \int_z^\infty dz \int_{λ_1}^{λ_2}ρ[v_D]acosϕdλ\\
=acosϕ/g \int_0^p dp \int_{λ_1}^{λ_2}[v_D]dλ\\
=acosϕ∆λ/g \int_0^p[v_D] dp\qquad     (1)
\end{align}
$$     

where [$v_D$] is zonal mean meridional divergent wind; ϕ is latitude in radian and λ is longitude in radian; ∆λ is the width of the regional zonal belt in radian.

Zonal mass stream function $ψ_x$ is defined as the eastward mass flux above a particular pressure P. In sphere coordinate, the meridional mean zonal mass stream function $ψ_λ$ is:

$$
\begin{align}
ψ_λ=\int_z^\infty dz \int_{λ_1}^{λ_2}ρ[u_D]adλ\\
=a∆ϕ/g \int_0^p[u_D] dp\qquad      (2)
\end{align}
$$    

where <[$u_D$]> is meridional mean zonal divergent wind, ∆ϕ is the width of the regional meridional belt in radian.

The vertical velocity in sphere coordinate is:
$(ω_λ, ω_ϕ )=-(\frac{∂ψ_λ}{acosϕ∂λ}\quad,\frac{∂(ψ_ϕ cosϕ)}{acosϕ∂ϕ}\quad)$   (3)
Regional mass stream function (MSF) can depict the thermodynamic circulation. The vertical motion of the partitioned zonal and meridional overturning circulations can be retrieved from the gradient of the partitioned MSF (Eq.3).   

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