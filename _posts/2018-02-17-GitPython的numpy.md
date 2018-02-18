---
layout:     post
title:      	Python的numpy
subtitle:   numpy使用
date:       2018-02-17
author:     WSS
header-img: img/AI4.jpg
catalog: true
tags:
    - Python
    - Numpy
---


`import numpy as np`

## 创建数据##

array：创建数组
dtype：指定数据类型
zeros：创建数据全为0
ones：创建数据全为1
empty：创建数据接近0
arrange：按指定范围创建数据
linspace：创建线段


```python
import numpy as np
```


```python
np.array([[1,2,3],[4,5,6]])
#指定数据 dtype 
a = np.array([[1,2,3],[4,5,6]],dtype=np.int32)
a.dtype
```




    array([[1, 2, 3],
           [4, 5, 6]])
		   
    dtype('int32')



## 创建特定数据 ##

array：创建数组
dtype：指定数据类型
zeros：创建数据全为0
ones：创建数据全为1
empty：创建数据接近0
arrange：按指定范围创建数据
linspace：创建线段


```python
np.array([[1,2,3],[4,5,6]])  # 2d 矩阵 2行3列
```




    array([[1, 2, 3],
           [4, 5, 6]])

```python
np.zeros((3,4),dtype=np.int32)#创建一个3X4的零矩阵
```




    array([[0, 0, 0, 0],
           [0, 0, 0, 0],
           [0, 0, 0, 0]])

```python
np.ones((3,4),dtype=np.int32)#创建一个3X4的1矩阵
```




    array([[1, 1, 1, 1],
           [1, 1, 1, 1],
           [1, 1, 1, 1]])

```python
np.empty((3,4)) # 创建全空数组, 其实每个值都是接近于零的数,数据为empty
```




    array([[  7.45436714e-312,   2.47032823e-322,   0.00000000e+000,
              0.00000000e+000],
           [  0.00000000e+000,   3.76231868e+174,   7.17593878e-091,
              7.24744758e+169],
           [  9.98865099e-048,   5.98943391e+174,   3.99910963e+252,
              4.76327670e-038]])
