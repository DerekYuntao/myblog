---
layout: post
title: "Matplotlib notes and tips"
date: 2021-02-15
description: Some notes and tips about python matplotlib
share: true
tags:
 - python
---

### Self-defined Colormap in Matplotlib

```python
import matplotlib.colors as mcolor
from matplotlib import cm

#load color file
color = np.loadtxt('/need_13colors.dat',delimiter=' ')
cc=list(color/255)
colormap = cm.colors.LinearSegmentedColormap.from_list('mycolors', cc, N=np.shape(color)[0])

im = ax.contourf(XX,YY,data,clevs,extend='both',cmap=colormap,transform= proj)   
```

### A secondary axis for the same plot
Reference:
<https://matplotlib.org/stable/gallery/subplots_axes_and_figures/secondary_axis.html>
<https://stackoverflow.com/questions/43149703/adding-a-second-y-axis-related-to-the-first-y-axis>

**Plot both pressure and height (Z) coordinates in a vertical profile**
```python
def Zlev2pressure(h):
    return 100000*(1+(-0.0065/293.5)*(h-0))**(-9.8*0.0289644/(8.31432*(-0.0065)))/100  # hPa

def pressure2Zlev(press):
    press = press*100
    return 0+(293.5/(-0.0065))*((press/100000)**(-(8.31432*(-0.0065)/(9.8*0.0289644)))-1)/1000  #km

......

fig = plt.figure(figsize=(9,15))

ax1 = fig.add_subplot(1,1,1)
# the left axis
ax1.invert_yaxis()
ax1.plot(R_pass_TES_seas, lev_TES_need, color='grey', alpha=0.2)
ax1.errorbar(R_ensumb_pass_TES_seas, lev_TES_need, xerr=R_std_pass_TES_seas, fmt='o-', color='black', ecolor='blue', capsize=4)
ax1.axvline(x=0, color='black', linestyle='--')
ax1.axhline(y=690, color='red', linestyle='--')
ax1.set_xlim([-1,1])
ax1.set_ylim([1000,100])
# the right axis
axt1 = ax1.secondary_yaxis('right', functions=(pressure2Zlev, Zlev2pressure))
axt1.set_yticks([0,2,4,6,8,10,12,15])
axt1.set_yticklabels('') 
ax1.set_ylabel('Pressure (hPa)', fontweight='bold')
ax1.set_title('(a) TES Interseas', fontweight='bold')
```

Check relationship between altitude and pressure here:
<https://www.mide.com/air-pressure-at-altitude-calculator>


Last update: 11/04/2022