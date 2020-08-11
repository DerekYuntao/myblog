---
layout: post
title: "Calclulation of d18Op"
date: 2020-08-11
description: How to calculate precipitation weighted d18Op
share: true
tags:
 - research
---

Isotopic ratios of the most of the elements for light stable isotope geochemistry are written conventionally as the ratio of the heavy (and rare) isotope to the light (and more abundant) isotope as in 18O/16O, 34S/32S, etc.

Since heavier water has a lower vapor pressure so they evaporate more slowly and condense more readily; heavier water has higher freezing and boiling point, which means it solidify easier and gasify slower. 

Isotopically light molecules will preferentially diffuse out of a system and leave the reservoir enriched in the heavy isotope. In the case of evaporation, the greater average translational velocities of isotopically lighter water molecules allow them to break through the liquid surface preferentially and diffuse across a boundary layer, resulting in an isotopic fractionation between vapor and liquid. It is the fact that water vapor in atmosphere over the oceans or over a large lake has 18O/16O and D/H ratios that are significantly lower than the ratios that would obtain at equilibrium due to evaporation.

Relative differences in isotopic ratios, the delta value is given by (unit: permil or ‰):
$δ = [(R_x-R_{std})/R_{std}]×1000$
$δO^{18} = [(O^{18}/O^{16} )/(O^{18}/O^{16} )_{SMOW} -1]×1000$

Based on model output, the d18Op is calculated as this :
P16 = PRECRC_H216Or + PRECSC_H216Os + PRECRL_H216OR + PRECSL_H216OS
P18 = PRECRC_H218Or + PRECSC_H218Os + PRECRL_H218OR + PRECSL_H218OS
The pure d18Op is,
d18Op = (P18 / P16 - 1) * 1e3     

Precipitation weighted d18Op is,
d18Op_precp = sum(p*d18Op)/sum(p)
where p is precipitation. Sum is along time coordinates, sum of 12 month is for annual.

Variale info:
```powershell
float PRECRC_H216Or(time, lat, lon) ;
    PRECRC_H216Or:units = "m/s" ;
    PRECRC_H216Or:long_name = "Convective rain rate for H216Or" ;
    PRECRC_H216Or:cell_methods = "time: mean" ;
float PRECSC_H216Os(time, lat, lon) ;
    PRECSC_H216Os:units = "m/s" ;
    PRECSC_H216Os:long_name = "Convective snow rate (water equivalent) for H216Os" ;
    PRECSC_H216Os:cell_methods = "time: mean" ;    
float PRECRL_H216OR(time, lat, lon) ;
    PRECRL_H216OR:units = "m/s" ;
    PRECRL_H216OR:long_name = "Large-scale (stable) rain rate for H216OR" ;
    PRECRL_H216OR:cell_methods = "time: mean" ;    
float PRECSL_H216OS(time, lat, lon) ;
    PRECSL_H216OS:units = "m/s" ;
    PRECSL_H216OS:long_name = "Large-scale (stable) snow rate (water equivalent) for H216OS" ;
    PRECSL_H216OS:cell_methods = "time: mean" ;    
```


