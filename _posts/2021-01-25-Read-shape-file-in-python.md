---
layout: post
title: "Read Shape Files in Python"
date: 2021-01-25
description: How to read shape file in python
share: true
tags:
 - python
---

```python
import shapefile 
#read shape file
sf = shapefile.Reader("F:\itrace\shape\gshhg-shp-2.3.7\GSHHS_shp\l\GSHHS_l_L1.shp")
border = sf.shapes()
## .shapes() will read the geomeray data.
border_points = []
shp_x = []
shp_y = []
for i in range(len(border)):
     border_points.append(border[i].points)
     x,y = zip(*border_points[i])
     x0 = np.array(x)
     idx = np.where(x0<0)[0]
     if len(idx)>0:
         x0[idx] = x0[idx] + 360
     shp_x.append(list(x0))
     shp_y.append(list(y))
```     

reference:
<https://blog.csdn.net/qq_41034838/article/details/100034244>


**Zip() usage
zip() 函数用于将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象，这样做的好处是节约了不少的内存。我们可以使用 list() 转换来输出列表。
如果各个迭代器的元素个数不一致，则返回列表长度与最短的对象相同，利用 * 号操作符，可以将元组反向解压为列表。

exmaple:
```python
import random
X = [1, 2, 3, 4, 5, 6]
y = [0, 1, 0, 0, 1, 1]
zipped_data = list(zip(X, y))  
# 将样本和标签一 一对应组合起来,并转换成list类型方便后续打乱操作

random.shuffle(zipped_data)  
# 使用random模块中的shuffle函数打乱列表，原地操作，没有返回值

#list(zip(*zipped_data))   #反向解压
new_zipped_data = list(map(list, zip(*zipped_data)))  
# zip(*)反向解压，map()逐项转换类型，list()做最后转换

new_X, new_y = new_zipped_data[0], new_zipped_data[1]  
# 返回打乱后的新数据

print('X:',X,'\n','y:',y)
print('new_X:',new_X, '\n', 'new_y:',new_y)
```

reference:
<https://www.runoob.com/python3/python3-func-zip.html>

Last update: 01/25/2021
