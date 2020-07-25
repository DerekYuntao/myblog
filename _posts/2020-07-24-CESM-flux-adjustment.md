# CESM flux adjustment
 
Based on fully coplued CESM model, I will restore model SST to observational seasonal climatology. In order to use "restoring" settings in a fully coupled run, the better way is to set pop2 interior forcing namelist instead of surface forcing namelist, which is only for ocean-alone model in regard to "restoring".

In order to perform the experiments, Let's read and understand what is code doing when restoring.

Here are some pre-assignment parameters and variables:

module grid
KMT            ,&! k index of deepest grid cell on T grid
KMU            ,&! k index of deepest grid cell on U grid
dz                ,&! thickness of layer k
DZU, DZT               ! thickness of U,T cell for pbc

