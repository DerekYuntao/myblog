---
layout: post
title: "Self-defined colormap in matplotlib"
date: 2021-02-15
description: Define a self-made colormap in matplotlib
share: true
tags:
 - python
---

```python
import matplotlib.colors as mcolor
from matplotlib import cm

#load color file
color = np.loadtxt('/need_13colors.dat',delimiter=' ')
cc=list(color/255)
colormap = cm.colors.LinearSegmentedColormap.from_list('mycolors', cc, N=np.shape(color)[0])

im = ax.contourf(XX,YY,data,clevs,extend='both',cmap=colormap,transform= proj)   
```

Last update: 02/15/2021