---
layout: post
title: "Fundational Tips for Python"
date: 2021-01-08
description: Fundational tips for python
share: true
tags:
 - python
---

[数据格式汇总及type, astype, dtype区别](https://blog.csdn.net/sinat_36458870/article/details/78946053)

[python-float8,float16,float32,float64和float128可以包含多少位数？](https://bugjia.net/200527/779405.html)

```python
>>> np.finfo(np.float16).precision
3
>>> np.finfo(np.float32).precision
6
>>> np.finfo(np.float64).precision
15
>>> np.finfo(np.float128).precision
```

## Numpy slicing array ##
***Note*: In numpy and NCL, rule of slicing array is: **

aa[i_start: i_end: slicing_step]

aa(i_start: i_end: slicing_step)

** But in MATLAB, it is different: **

aa(i_start: slicing_step: i_end)

** Do remeber it clearly or it will make mistakes!! **