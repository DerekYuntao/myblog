---
layout: post
title: "Outputed Energy Variables from CAM"
date: 2020-08-24
description: Atmospheric energy variables e.g. ASR, OLR...
share: true
tags:
 - Research
 - CESM
---

**Concepts**
A budget for the energy content of the global atmosphere-ocean system:
𝑑𝐸/𝑑𝑡 = net energy flux into system = Net Flux in – Flux out
where 𝐸 is the enthalpy or heat content of the total system. 
Note: any internal exchanges of energy between different reservoirs do not appear in this budget – because 𝐸 is the sum of all reservoirs.

Specifically,
**Flux in**:
The radiative fluxes to and from space, the incoming solar radiation Q. 

**Flux-out**:
Including reflected solar radiation and emitted terrestrial (longwave) radiation.
*Note that OLR = outgoing longwave radiation or terrestrial emissions to space

ASR: absorbed solar radiation
ASR: incoming flux – reflected flux=Q−αQ=(1−α)Q
where α is the Planetary albedo. Globally, α = reflected solar flux / incoming solar flux ≈ 0.3.

Finally gives the climate system energy budget equation, the starting point for every climate model.
𝑑𝐸/𝑑𝑡 = (1-α)Q-OLR

**CAM variables**
```powershell
    # OLR:
	float FLUT(time, lat, lon) ;
		FLUT:Sampling_Sequence = "rad_lwsw" ;
		FLUT:units = "W/m2" ;
		FLUT:long_name = "Upwelling longwave flux at top of model" ;
		FLUT:cell_methods = "time: mean" 

    # Incoming solar flux:
    float SOLIN(time, lat, lon) ;
        SOLIN:Sampling_Sequence = "rad_lwsw" ;
        SOLIN:units = "W/m2" ;
        SOLIN:long_name = "Solar insolation" ;
        SOLIN:cell_methods = "time: mean" ;

    # Reflected shortwave flux:
	float FSUTOA(time, lat, lon) ;
		FSUTOA:Sampling_Sequence = "rad_lwsw" ;
		FSUTOA:units = "W/m2" ;
		FSUTOA:long_name = "Upwelling solar flux at top of atmosphere" ;
		FSUTOA:cell_methods = "time: mean" ;
```
Based on CAM output, the energy budget of the climate system is calculated as follows.
ASR = SOLIN - FSUTOA
OLR = FLUT
𝑑𝐸/𝑑𝑡 = ASR - OLR
*Note that all variables are in positive values.

Reference:
"We focus on the radiative fluxes at the top of the atmosphere, which provide the energy sources and sinks for the climate system: the upward longwave flux FLUT (often called outgoing longwave radiation or OLR), and the upward shortwave flux FSUTOA (solar energy reflected back to space by the atmosphere and surface)."

Ascough, J., H. Maier, J. Ravalico, and M. Strudley (2008), Future research challenges for incorporation of uncertainty in environmental and ecological decision‐making, Ecol. Modell., 219(3–4), 383–399.
https://agupubs.onlinelibrary.wiley.com/doi/full/10.1002/jame.20040

Last update: 08/25/2020

