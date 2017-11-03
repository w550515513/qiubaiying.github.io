---
layout:     post
title:      Python中的Pandas
subtitle:   如何使用熊猫
date:       2017-11-03
author:     WSS
header-img: img/post-bg-pandas.jpg
catalog: true
tags:
    - Pandas
    - Python
---


## Pandas介绍 ##

pandas是为了解决数据分析任务而创建的，纳入了大量的库和标准数据模型，提供了高效地操作大型数据集所需的工具。

pandas中的数据结构 :

	Series: 一维数组，类似于python中的基本数据结构list，区别是series只允许存储相同的数据类型，这样可以更有效的使用内存，提高运算效率。就像数据库中的列数据。

	DataFrame: 二维的表格型数据结构。很多功能与R中的data.frame类似。可以将DataFrame理解为Series的容器。

	Panel：三维的数组，可以理解为DataFrame的容器。
	
引入的包：

```python
import pandas as pd	
import numpy as np  #python实现的科学计算包
import matplotlib.pyplot as plt #python的2D绘图库
```

## Series##

一个series是一个一维的数据类型，其中每一个元素都有一个标签。类似于Numpy中元素带标签的数组。其中，标签可以是数字或者字符串。

```python
# coding: utf-8
import numpy as np
import pandas as pd

s = pd.Series([1, 2, 5, np.nan, 6, 8])#nan
print s
```
输出：
```python
0 1.0
1 2.0
2 5.0
3 NaN
4 6.0
5 8.0
```

## DataFrame##

一个dataframe是一个二维的表结构。Pandas的dataframe可以存储许多种不同的数据类型，并且每一个坐标轴都有自己的标签。你可以把它想象成一个series的字典项。

####创建一个 DateFrame ####

```python
#创建日期索引序列 
dates = pd.date_range('20130101', periods=6)
#创建Dataframe，其中 index 决定索引序列，columns 决定列名
df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=list('ABCD'))
print df
```

输出：
```python
  				A  		B  			C  			D
2013-01-01 -0.334482 0.746019 -2.205026 -0.803878
2013-01-02 2.007879 1.559073 -0.527997 0.950946
2013-01-03 -1.053796 0.438214 -0.027664 0.018537
2013-01-04 -0.208744 -0.725155 -0.395226 -0.268529
2013-01-05 0.080822 -1.215433 -0.785030 0.977654
2013-01-06 -0.126459 0.426328 -0.474553 -1.968056
```

#### 字典创建 DataFrame ####

```python
df2 = pd.DataFrame({ 'A' : 1,
   'B' : pd.Timestamp('20130102'),
   'C' : pd.Series(1,index=list(range(4)),dtype='float32'),
   'D' : np.array([3] * 4,dtype='int32'),
   'E' : pd.Categorical(["test","train","test","train"]),
   'F' : 'foo' })
```
输出：
```python
  A  B 		   C D  E    F
0 1 2013-01-02 1 3 test foo
1 1 2013-01-02 1 3 train foo
2 1 2013-01-02 1 3 test foo
3 1 2013-01-02 1 3 train foo
```

####将csv文件数据导入Pandas####

```python
df = pd.read_csv("Average_Daily_Traffic_Counts.csv", header = 0)
df.head()
```

##选择/切片##

```python
# 选择单独的一列，返回 Serires，与 df.A 效果相当。
df['A']

# 位置切片
df[0:3]

# 索引切片
df['20130102':'20130104']

#　通过标签选择
df.loc[dates[0]] #dates = pd.date_range('20130101', periods=6) 选行

# 对多个轴同时通过标签进行选择
df.loc[:,['A','B']]

# 获得某一个单元的数据
**df.loc[dates[0],'A']** #先行后列

# 通过位置进行选择
df.iloc[3]

# 切片
df.iloc[3:5,0:2]

# 列表选择
df.iloc[[1,2,4],[0,2]]

# 获得某一个单元的数据
df.iloc[1,1]
# 或者
df.iat[1,1] # 更快的做法

# isin 过滤
df2[df2['E'].isin(['two','four'])]

#过滤
final[final.VIS>0]
```
