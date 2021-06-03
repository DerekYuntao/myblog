---
layout: post
title: "Python Tips"
date: 2021-01-08
description: Fundational tips for python
share: true
tags:
 - python
---

## Data type and precision
[数据格式汇总及type, astype, dtype区别](https://blog.csdn.net/sinat_36458870/article/details/78946053)

[python-float8,float16,float32,float64和float128可以包含多少位数？](https://bugjia.net/200527/779405.html)

```python
np.finfo(np.float16).precision
# 3
np.finfo(np.float32).precision
# 6
np.finfo(np.float64).precision
# 15
np.finfo(np.float128).precision
```

## Numpy slicing array (important!)
***Note*: In numpy and NCL, rule of slicing array is: **

aa[i_start: i_end: slicing_step]

aa(i_start: i_end: slicing_step)

** But in MATLAB, it is different: **

aa(i_start: slicing_step: i_end)

** Do remeber it clearly or it will make mistakes!! **

## Sort N-D numpy array based on one column 
e.g. sorting a 2-cloumn array based on value of the first column by `np.argsort`
```python
Z1=np.transpose(np.vstack((precp_Z1, d18Op_Z1)))
Z2=np.transpose(np.vstack((precp_Z2, d18Op_Z2)))

Z1_sort = HS1_Z1[np.argsort(Z1[:,0])]
Z2_sort = LGM_Z2[np.argsort(Z2[:,0])]
```

## Initializing a numpy array of string data
The numpy string array is limited by its fixed length (default: length of 1). If the length of the strings are unsure or not equal, we can initialize the numpy string array by setting the data type as `dtype=object`:
```python
    newstr_array = numpy.empty((N, M), dtype=object)
```

## Pythonic list comprehension
A short line of code to generate a list
```python
[x * x for x in range(1, 11) if x % 2 == 0]

#which is identical to:
L = []
for x in range(1, 11):
    if x % 2 ==0:
        L.append(x * x)
```        
More reference: 
[列表生成式](https://www.liaoxuefeng.com/wiki/1016959663602400/1017317609699776)

## Read ascii files 
**Way 1: read()** 
**Read()** read all data at once and return a string.
```python
with open("test.txt", "r") as f:
    data = f.read()
    print(data)
```

**Way2: readline()**
**readline()** read data by line and only return the first line. So loop would be used to read all lines of the data. Note that '\n' is included!
```python
with open("test.txt", "r") as f:
    data = f.readline()
    print(data)
```

**Way3: readlines()**
**readlines()** read data by lines and return all lines of data in a list. Note that '\n' is included!
```python
with open("test.txt", "r") as f:
    data = f.readlines()
    print(data)
```

**Read data by lines and remove line break**
**Way1:**
```python
fnames = []    
with open("need_files.txt") as fp:
    #[fnames.append(i) for i in fp.readlines()]
    [fnames.append(i) for i in fp.read().splitlines()]
```

**Way2:**
```python
with open("need_files.txt", "r") as f:
    data = f.readlines()
    print(data)
need_data = []
need_data.append(i) for i in data[i][:-1]
```

## Read and write data in numpy 
```python
Andes_loc = np.zeros((len(Andes_lat),2), dtype='int')
Andes_loc[:,0] = Andes_lon
Andes_loc[:,1] = Andes_lat
# output index
np.savetxt('Andes_locidx.txt', Andes_loc, fmt='%d')

# read data into an array
locIdx = np.loadtxt('Andes_locidx.txt', Andes_loc, fmt='%d')
``` 

## Matplotlib colors
[List of named colors](https://matplotlib.org/stable/gallery/color/named_colors.html)

[Choosing Colormaps in Matplotlib](https://matplotlib.org/stable/tutorials/colors/colormaps.html)

## Matplotlib special characters
[Writing mathematical expressions](https://matplotlib.org/stable/tutorials/text/mathtext.html)

## Matplotlib unaligned subplot (GridSpec) & double Y axis & bar plot
e.g. 
```python
import matplotlib.gridspec as gridspec

bar_width = 0.4
fig = plt.figure(figsize=(10,14))
gs = gridspec.GridSpec(4, 3)   # subplot in 4*3 settings
ax1 = fig.add_subplot(gs[0,0])  
c1 = ax1.bar(x=range(NM-1), height=Total_zone1[:-1], color='blue', alpha=0.6, width=bar_width)
c2 = ax1.bar(x=np.arange(NM-1)+bar_width, height=Total_zone1[:-1], color='red', alpha=0.6, width=bar_width)
ax12 = ax1.twinx()
c12 = ax12.bar(NM-1, height=Total_zone1[-1], color='blue', alpha=0.6, width=bar_width)
c22 = ax12.bar(NM-1+bar_width, height=Total_zone1[-1], color='red', alpha=0.6, width=bar_width)
```    
Reference:
<https://matplotlib.org/stable/gallery/subplots_axes_and_figures/align_labels_demo.html#sphx-glr-gallery-subplots-axes-and-figures-align-labels-demo-py>

## Matplotlib broken Y axis
<https://matplotlib.org/2.0.2/examples/pylab_examples/broken_axis.html>
<https://blog.csdn.net/Forrest97/article/details/113746307>

## seaborn CDF plot interacted with matplotlib
seaborn CDF subplots in a double y axis figrue 

```python
fig = plt.figure()
ax1 = fig.add_subplot(1,2,1)
ax1 = sns.distplot(precp_LGM_Z1,
             kde_kws={'cumulative':'True', 'linestyle':'--', 'color':'blue'}, hist=False, ax=ax1)
ax1 = sns.distplot(precp_HS1_Z1,
             kde_kws={'cumulative':'True', 'linestyle':'--', 'color':'red'}, hist=False, ax=ax1)
ax1.set_xlim([0, 35])
ax1.set_ylim([0, 1])
ax12 = ax1.twinx()
ax12.plot(LGM_Z1_draw[:,0], LGM_Z1_draw[:,1], color='blue')
ax12.plot(HS1_Z1_draw[:,0], HS1_Z1_draw[:,1], color='red')
ax12.set_ylim([-16, 0])
ax1.set_xlabel('mm/day')
ax1.set_ylabel('CDF')
ax1.set_title('(a) E Brazil', fontweight='bold')

ax2 = fig.add_subplot(1,2,2)
ax2 = sns.distplot(precp_LGM_Z2,
             kde_kws={'cumulative':'True', 'linestyle':'--', 'color':'blue'}, hist=False, ax=ax2)
ax2 = sns.distplot(precp_HS1_Z2,
             kde_kws={'cumulative':'True', 'linestyle':'--', 'color':'red'}, hist=False, ax=ax2)
ax2.set_xlim([0, 35])
ax2.set_ylim([0, 1])
ax2.set_ylabel('')
ax2.set_xlabel('mm/day')
ax22 = ax2.twinx()
c1 = ax22.plot(LGM_Z2_draw[:,0], LGM_Z2_draw[:,1], color='blue')
ax22.plot(HS1_Z2_draw[:,0], HS1_Z2_draw[:,1], color='red')
ax22.set_ylim([-16, 0])
ax22.set_ylabel('$\delta^{18}O_p$ (permil)')
ax2.set_title('(b) W Amazon', fontweight='bold')
```
reference:
 <https://www.it-swarm.cn/zh/python/%E4%BD%BF%E7%94%A8seaborn-python%E7%BB%98%E5%88%B6cdf-%E7%B4%AF%E7%A7%AF%E7%9B%B4%E6%96%B9%E5%9B%BE/826499323/>
