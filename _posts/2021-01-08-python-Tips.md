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
>>> np.finfo(np.float16).precision
3
>>> np.finfo(np.float32).precision
6
>>> np.finfo(np.float64).precision
15
>>> np.finfo(np.float128).precision
```

## Numpy slicing array
***Note*: In numpy and NCL, rule of slicing array is: **

aa[i_start: i_end: slicing_step]

aa(i_start: i_end: slicing_step)

** But in MATLAB, it is different: **

aa(i_start: slicing_step: i_end)

** Do remeber it clearly or it will make mistakes!! **

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



