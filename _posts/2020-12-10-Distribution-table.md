---
layout: post
title: "Print a Statistical Distribution Table in Python"
date: 2020-12-10
description: How to print a statistical distribution table in Python
share: true
tags:
 - python
---

```python
import numpy as np
from scipy.stats import chi2
from scipy.stats import t
from scipy.stats import f
from scipy.stats import norm
 
# chi square distribution
percents = [0.995, 0.990, 0.975, 0.950, 0.900, 0.100, 0.050, 0.025, 0.010, 0.005]
print np.array([chi2.isf(percents, df=i) for i in range(1, 47)])
# t distribution
percents = [0.100, 0.050, 0.025, 0.010, 0.005, 0.001, 0.0005]
print np.array([t.isf(percents, df=i) for i in range(1, 46)])
# F distribution
alpha = 0.05
print np.array([f.isf(alpha, df1, df2) for df1 in range(1, 11) for df2 in range(1, 46)]).reshape(10, -1).T
# normal distribution
print norm.ppf(np.arange(0, 0.99, 0.001).reshape(-1, 10))
```
 Last update: 12/10/2020