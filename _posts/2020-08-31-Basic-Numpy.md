---
layout: post
title: "Basic Tips of Numpy"
date: 2020-08-31
description: Array, Matrix, Broadcasting...
share: true
tags:
 - python
---

## e.g. 1 Print arrays
```python
x1 = np.arange(10)   # numbers from 0 to 9 (note 10 is NOT included)
x2 = np.arange(2,7)  # numbers from 2 to 6 (note 7 is NOT included)
print("x1 = "+str(x1))
print("x2 = "+str(x2))    #!!Note: should transfer to string to print!

#output:
x1 = [0 1 2 3 4 5 6 7 8 9]
x2 = [2 3 4 5 6]
```

## e.g. 2 Array increasement 
```python
# Increments of 2.5.  To ensure it includes the value 10, we make the endpoint slightly >10.
step = 2.5
x1 = np.arange(0,10+step/2,step)
print(x1)

x2 = np.linspace(0,10,5)
print(x2)

#output:
[ 0.   2.5  5.   7.5 10. ]
[ 0.   2.5  5.   7.5 10. ]
```

## e.g. 3  The power of *range*
```python
z1 = np.array(range(2,21,2))
print("z1=" + str(z1))
z2 = np.abs(np.array(range(50, -51, -10)))
print("z2=" + str(z2))
#output:
z1=[ 2  4  6  8 10 12 14 16 18 20]
z2=[50 40 30 20 10  0 10 20 30 40 50]
```

**Note: Arrays retain their datatype!**
Unlike Matlab, arrays retain their originally declared datatype, despite any operations that may be performed on them. We can't replace one element of an integer array with a float, and can't divide integer elements and expect floats. Thus, **we would better declare the tpye when making an array**. 

## e.g. 4 
```python
x = np.array(range(2,6)) # declare an integer array
print("x="+str(x))
r = np.random.rand(1) # generate random float
print("r="+str(r))
x[0] = r # try assigning float as element
x[1] = x[1]/2 # try dividing odd integer by two
print("x="+str(x)) # the array stays integer!
print(x.dtype)

#output
x=[2 3 4 5]
r=[0.98797611]
x=[0 1 4 5]
int32

print("r="+str(r))
x = np.array(range(2,6), dtype=float) # declare an array of floats
print("x="+str(x))
x[0] = r # try assigning float as element
x[1] = x[1]/2 # try dividing odd integer by two
print("x="+str(x)) # this works fine!

#output
r=[0.08604006]
x=[2. 3. 4. 5.]
x=[0.08604006 1.5        4.         5.        ]

x = np.array([2,3,4,5.]) # this also declares an array of floats
print("x="+str(x))
x[0] = r # try assigning float as element
x[1] = x[1]/2 # try dividing odd integer by two
print("x="+str(x)) # this works fine!

#output
x=[2. 3. 4. 5.]
x=[0.08604006 1.5        4.         5.        ]
```

## e.g. 5 Indexing and slicing vectors (important!)
```python
x = np.random.rand(10)  # 10 random elements uniformly distributed in [0,1]
x1 = x[2:5]             # Elements 2 thru 5 (not including 5, indexing starts at 0)
x2 = x[:4]              # Elements thru 4 (not including 4, indexing starts at 0)
x3 = x[7:]              # Elements 7 to end (indexing starts at 0)
xlast = x[-1]           # The last element (i.e., index=length-1)
xslice = x[0:10:2]      # Slicing the array every 2 elements
xslice2 = x[0::2]       # Slicing the array every 2 elements

print("x=      "+np.array_str(x,precision=3))    #!!control the output array precision!
print("x[2:5]= "+np.array_str(x1,precision=3))
print("x[:4]=  "+np.array_str(x2,precision=3))
print("x[7:]=  "+np.array_str(x3,precision=3))
print("xlast=  {0:5.3f}".format(xlast))
print("xslice" + np.array_str(xslice,precision=3))
print("xslice2" + np.array_str(xslice2,precision=3))

#output
x=      [0.979 0.816 0.135 0.315 0.557 0.672 0.403 0.279 0.113 0.326]
x[2:5]= [0.135 0.315 0.557]
x[:4]=  [0.979 0.816 0.135 0.315]
x[7:]=  [0.279 0.113 0.326]
xlast=  0.326
xslice = [0.979 0.135 0.557 0.403 0.113]
xslice2 = [0.979 0.135 0.557 0.403 0.113]
```

**Note: Array slicing does not copy!**
Slices of an array are merely pointers to the original array. So, if you modify the slice, you modify the original array! If you want to manipulate a slice independently of the original array, you must explicity create a new copy of that slice.
## e.g. 6
```python
r = np.random.rand(1)
a = np.array([1, 2, 3, 4],dtype=float)
b = a[0:2] # slice of a
b[0] = r # this will change both a and b!
print("a=" + str(a))
print("b=" + str(b))

a = np.array([1, 2, 3, 4],dtype=float)
c = np.array(a[0:2]) # this creates a new copy from the slice
c[0] = r # this only changes c
print("a="+str(a))
print("c=" + str(c))

#output
a=[0.75570136 2.         3.         4.        ]
b=[0.75570136 2.        ]
a=[1. 2. 3. 4.]
c=[0.75570136 2.        ]
```

## e.g. 7 Loops
```python
mylist  = ['a', 'b', 'c']
for k in mylist:
    print(k)

#access both the index into the list and the list elements:
for k, element in enumerate(mylist):   
    print(k,element)

#output
 a b c

0 a
1 b
2 c
```

## e.g. 8 Reshape
```python
X = np.arange(24).reshape(2,3,4)  # shape = (2,3,4)
print(X)

#output
X=[[[ 0  1  2  3]
  [ 4  5  6  7]
  [ 8  9 10 11]]

 [[12 13 14 15]
  [16 17 18 19]
  [20 21 22 23]]]
```

## e.g. 9 Broadcasting: mean removal
Suppose that X is a data matrix of shape (n,p). That is, there are n data points and p features per point. Often, we have to remove the mean from each feature. 
```python
# Generate some random data
n = 100
p = 5
X = np.random.rand(n,p)
print(np.shape(X))

# Compute the mean per column using the axis command
Xm = np.mean(X,axis=0)  # This is a p-dim matrix
print(np.shape(Xm))
print(np.shape(Xm[None,:]))

# Subtract the mean
X_demean = X - Xm[None,:]
print(np.shape(X_demean))
X_demean = X - Xm
print(np.shape(X_demean))

#output
(100, 5)
(5,)
(1, 5)
(100, 5)
(100, 5)
```

## e.g. 9 Broadcasting: distance
Here is a more complicated example. Suppose we have a data matrix X of shape (nx,p) and a second set of points, Y of shape (ny,p). For each i and j, we want to compute the distances,
 d[i,j] = np.sum((X[i,:] - Y[j,:])**2)
This represents the distances between the vectors X[i,:] and Y[j,:]. We can do this without a for loop as follows.

```python
# Some random data
nx = 100
ny = 10
p = 2
X = np.random.rand(nx,p)
print('X shape = ' +  str(np.shape(X)))
Y = np.random.rand(ny,p)
print('Y shape = ' +  str(np.shape(Y)))
      
# Computing the distances in two lines.  No for loop!
DXY = X[:,None,:]-Y[None,:,:]
print('X[:,None,:] shape = ' +  str(np.shape(X[:,None,:])))
print('Y[:,None,:] shape = ' +  str(np.shape(Y[None,:,:])))
print('DXY shape = ' +  str(np.shape(DXY)))
d = np.sum(DXY**2,axis=2)
print('d shape = ' +  str(np.shape(d)))

#output
X shape = (100, 2)
Y shape = (10, 2)
X[:,None,:] shape = (100, 1, 2)
Y[:,None,:] shape = (1, 10, 2)
DXY shape = (100, 10, 2)
d shape = (100, 10)
```

## e.g.10 Broadcasting: outer product
The outer product of vectors x and y is the matrix Z[i,j] = x[i]y[j]. 
```python
# Some random data
nx = 100
ny = 10
x = np.random.rand(nx)
y = np.random.rand(ny)

# Compute the outer product in one line
Z = x[:,None]*y[None,:]     # x[:,None] has shape (nx,  1);  y[None,:] has shape ( 1, ny)
print('Z shape = '+ str(np.shape(Z)))

#output
Z shape = (100, 10)
```

**e.g. 11 The work space**
```python
whos

#output
Variable   Type       Data/Info
-------------------------------
a          ndarray    4: 4 elems, type `float64`, 32 bytes
b          ndarray    2: 2 elems, type `float64`, 16 bytes
c          ndarray    2: 2 elems, type `float64`, 16 bytes
element    str        cranberry
f          int        3
k          int        2
mylist     list       n=3
np         module     <module 'numpy' from '/Us<...>kages/numpy/__init__.py'>
plt        module     <module 'matplotlib.pyplo<...>es/matplotlib/pyplot.py'>
r          ndarray    1: 1 elems, type `float64`, 8 bytes
step       float      2.5
t          ndarray    100: 100 elems, type `float64`, 800 bytes
x          ndarray    11: 11 elems, type `float64`, 88 bytes
x1         ndarray    3: 3 elems, type `float64`, 24 bytes
x2         ndarray    4: 4 elems, type `float64`, 32 bytes
x3         ndarray    4: 4 elems, type `float64`, 32 bytes
xlast      float64    0.27141830443616755
y          ndarray    100: 100 elems, type `float64`, 800 bytes
ysq        ndarray    100: 100 elems, type `float64`, 800 bytes

del a, b, c    #delete variables
whos 

#output
Variable   Type       Data/Info
-------------------------------
element    str        c
k          int        2
mylist     list       n=3
np         module     <module 'numpy' from 'D:\<...>ges\\numpy\\__init__.py'>
r          ndarray    1: 1 elems, type `float64`, 8 bytes
step       float      2.5
x          ndarray    4: 4 elems, type `int32`, 16 bytes
x1         ndarray    3: 3 elems, type `int32`, 12 bytes
x2         ndarray    1: 1 elems, type `int32`, 4 bytes
x3         ndarray    3: 3 elems, type `int32`, 12 bytes
xlast      float64    0.32608577784906123
z1         ndarray    10: 10 elems, type `int32`, 40 bytes
z2         ndarray    11: 11 elems, type `int32`, 44 bytes
```

**e.g. 12 Initializing a numpy array of string data** 
The numpy string array is limited by its fixed length (default: length of 1). If the length of the strings are unsure or not equal, we can initialize the numpy string array by setting the data type as `dtype=object`:
```python
    newstr_array = numpy.empty((N, M), dtype=object)
```

Last update: 08/31/2020
Last update: 02/27/2021