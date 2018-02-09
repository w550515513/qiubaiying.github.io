---
layout:     post
title:        PyMyql地使用
subtitle:   python与mysql初步总结
date:       2018-02-03
author:     WSS
header-img: img/AI2.jpg
catalog: true
tags:
    - Mysql
    - Python
---


## Mysql的python包 ##

名字`mysqlclient`

建议手动安装，下载位置:[https://pypi.python.org/pypi/mysqlclient/1.3.12](https://pypi.python.org/pypi/mysqlclient/1.3.12)，查找适合自己电脑与python版本的轮子

使用cmd定位到路径

`pip install packagename(wholename)`


## 连接数据库 ##

```python

import MySQLdb

# 打开数据库连接eg:(host='localhost', user='root', password='888888', db='tianqi', charset='utf8')
conn = MySQLdb.connect("localhost","testuser","test123","TESTDB" )

# 使用cursor()方法获取操作游标 
cursor = db.cursor()

```
## DataFrame ##

一个dataframe是一个二维的表结构。Pandas的dataframe可以存储许多种不同的数据类型，并且每一个坐标轴都有自己的标签。你可以把它想象成一个series的字典项。

####  创建一个 DateFrame  ####

```python
#创建日期索引序列 
dates = pd.date_range('20130101', periods=6)
#创建Dataframe，其中 index 决定索引序列，columns 决定列名
df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=list('ABCD'))
print df
```

输出：
```python
  	         A         B        C         D
2013-01-01 -0.334482 0.746019 -2.205026 -0.803878
2013-01-02 2.007879 1.559073 -0.527997 0.950946
2013-01-03 -1.053796 0.438214 -0.027664 0.018537
2013-01-04 -0.208744 -0.725155 -0.395226 -0.268529
2013-01-05 0.080822 -1.215433 -0.785030 0.977654
2013-01-06 -0.126459 0.426328 -0.474553 -1.968056
```

####  字典创建 DataFrame  ####

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
  A  B         C D  E    F
0 1 2013-01-02 1 3 test foo
1 1 2013-01-02 1 3 train foo
2 1 2013-01-02 1 3 test foo
3 1 2013-01-02 1 3 train foo
```

#### 将csv文件数据导入Pandas ####

```python
df = pd.read_csv("Average_Daily_Traffic_Counts.csv", header = 0)
df.head()
```

## 选择/切片 ##

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

# 通过位置进行选择   **选择行**
df.iloc[3]

# 切片
df.iloc[3:5,0:2]

# 列表选择  选择多行与多列
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

## 赋值 ##

```python
# 新增一列，根据索引排列
s1 = pd.Series([1,2,3,4,5,6], index=pd.date_range('20130102', periods=6))
df['F'] = s1

# 赋值
df.loc[1,'A'] = nan

#新增一行
df.loc[i]={'a':1,'b':2}

#新增一行2
df = pd.DataFrame(np.random.randn(8, 4), columns=['A','B','C','D'])
s = df.iloc[3]
df.append(s, ignore_index=True)
```

## 缺省操作 ##

```python
# 在 pandas 中使用 np.nan 作为缺省项的值。
df1 = df.reindex(index=dates[0:4], columns=list(df.columns) + ['E'])
df1.loc[dates[0]:dates[1],'E'] = 1

# 删除所有带有缺省项的行
df1.dropna(how='any')

# 填充缺省项
df1.fillna(value=5)

# 获得缺省项的布尔掩码
pd.isnull(df1)
```
## 观察操作 ##

```python
# 观察开头的数据
df.head()

# 观察末尾的数据
df.tail(3)

# 显示索引,行号
df.index

# 显示列
df.columns

# 显示底层 numpy 结构
df.values

# 转置，行变列
df.T

# 根据某一轴的索引进行排序
df.sort_index(axis=1, ascending=False)

# 根据某一列的数值进行排序
df.sort(columns='B')

# DataFrame 的基本统计学属性预览
df.describe()

#out
        A      B       C       D
count 6.000000 6.000000 6.000000 6.000000 #数量
mean 0.073711 -0.431125 -0.687758 -0.233103 #平均值
std 0.843157 0.922818 0.779887 0.973118 #标准差
min -0.861849 -2.104569 -1.509059 -1.135632 #最小值
25% -0.611510 -0.600794 -1.368714 -1.076610 #正态分布 25%
50% 0.022070 -0.228039 -0.767252 -0.386188 #正态分布 50%
75% 0.658444 0.041933 -0.034326 0.461706 #正态分布 75%
max 1.212112 0.567020 0.276232 1.071804 #最大值

```

## 合并 ##

使用 concat() 连接 pandas 对象:

```python
df = pd.DataFrame(np.random.randn(10, 4))

     0       1         2         3
0 -0.548702 1.467327 -1.015962 -0.483075
1 1.637550 -1.217659 -0.291519 -1.745505
2 -0.263952 0.991460 -0.919069 0.266046
3 -0.709661 1.669052 1.037882 -1.705775
4 -0.919854 -0.042379 1.247642 -0.009920
5 0.290213 0.495767 0.362949 1.548106
6 -1.131345 -0.089329 0.337863 -0.945867
7 -0.932132 1.956030 0.017587 -0.016692
8 -0.575247 0.254161 -1.143704 0.215897
9 1.193555 -0.077118 -0.408530 -0.862495


pieces = [df[:3], df[3:7], df[7:]]#前3行+4到7行+8到最后
pd.concat(pieces)

    0           1         2         3
0 -0.548702 1.467327 -1.015962 -0.483075
1 1.637550 -1.217659 -0.291519 -1.745505
2 -0.263952 0.991460 -0.919069 0.266046
3 -0.709661 1.669052 1.037882 -1.705775
4 -0.919854 -0.042379 1.247642 -0.009920
5 0.290213 0.495767 0.362949 1.548106
6 -1.131345 -0.089329 0.337863 -0.945867
7 -0.932132 1.956030 0.017587 -0.016692
8 -0.575247 0.254161 -1.143704 0.215897
9 1.193555 -0.077118 -0.408530 -0.862495

```

## 统计 ##

```python
# 求平均值
df.mean()

A -0.004474
B -0.383981
C -0.687758
D 5.000000
F 3.000000
dtype: float64


# 指定轴上的平均值
df.mean(1)

# 不同维度的 pandas 对象也可以做运算，它会自动进行对应，shift 用来做对齐操作。
s = pd.Series([1,3,5,np.nan,6,8], index=dates).shift(2)

2013-01-01 NaN
2013-01-02 NaN
2013-01-03 1
2013-01-04 3
2013-01-05 5
2013-01-06 NaN
Freq: D, dtype: float64


# 对不同维度的 pandas 对象进行减法操作
df.sub(s, axis='index')

           A   B   C   D   F
2013-01-01 NaN NaN NaN NaN NaN
2013-01-02 NaN NaN NaN NaN NaN
2013-01-03 -1.861849 -3.104569 -1.494929 4 1
2013-01-04 -2.278445 -3.706771 -4.039575 2 0
2013-01-05 -5.424972 -4.432980 -4.723768 0 -1
2013-01-06 NaN NaN NaN NaN NaN

```

## 直方图 ##

```python
s = pd.Series(np.random.randint(0, 7, size=10))
s.value_counts()
"""
4 5
6 2
2 2
1 1
dtype: int64
String Methods
"""
```

## 字符处理 ##

```python 
s = pd.Series(['A', 'B', 'C', 'Aaba', 'Baca', np.nan, 'CABA', 'dog', 'cat'])
s.str.lower()#小写
"""
0 a
1 b
2 c
3 aaba
4 baca
5 NaN
6 caba
7 dog
8 cat
dtype: object
"""
```

## 分组? ##

对于”group by”操作，我们通常是指以下一个或多个操作步骤：

1. （Splitting）按照一些规则将数据分为不同的组；

2. （Applying）对于每组数据分别执行一个函数；

3. （Combining）将结果组合到一个数据结构中；

**1、（Splitting）按照一些规则将数据分为不同的组；**

```python 
#1、（Splitting）按照一些规则将数据分为不同的组；
>>> df = pd.DataFrame({'A' : ['foo', 'bar', 'foo', 'bar','foo', 'bar', 'foo', 'foo'],
                       'B' : ['one', 'one', 'two', 'three','two', 'two', 'one', 'three'],
                       'C' : np.random.randn(8),
                       'D' : np.random.randn(8)})
>>> df
         A      B         C         D
    0  foo    one -0.655020 -0.671592
    1  bar    one  0.846428  1.884603
    2  foo    two -2.280466  0.725070
    3  bar  three  1.166448 -0.208171
    4  foo    two -0.257124 -0.850319
    5  bar    two -0.654609  1.258091
    6  foo    one -1.624213 -0.383978
    7  foo  three -0.523944  0.114338
```

分组后sum求和

```python
>>>df.groupby('A').sum()
                C         D
    A
    bar  1.358267  2.934523
    foo -5.340766 -1.066481
```
对多列分组后sum:

```python
>>> df.groupby(['A','B']).sum()
                               
    A   B			C		D
    bar one    0.846428  1.884603
        three  1.166448 -0.208171
        two   -0.654609  1.258091
    foo one   -2.279233 -1.055570
        three -0.523944  0.114338
        two   -2.537589 -0.125249
```

··············································································

```python
>>> df = pd.DataFrame({"id":[1,2,3,4,5,6], "raw_grade":['a', 'b', 'b', 'a', 'a', 'e']})
```
1.将原始成绩转换为分类数据:

```python
>>> df["grade"] = df["raw_grade"].astype("category")
>>> df["grade"]
    0    a
    1    b
    2    b
    3    a
    4    a
    5    e
    Name: grade, dtype: category
    Categories (3, object): [a, b, e]
```

2.重命名分类使其更有意义:

```python
>>> df["grade"].cat.categories = ["very good", "good", "very bad"]
```

3.重新整理类别，并添加缺少的类别:

```python
>>> df["grade"] = df["grade"].cat.set_categories(["very bad", "bad", "medium", "good", "very good"])
>>> df["grade"]
    0    very good
    1         good
    2         good
    3    very good
    4    very good
    5     very bad
    Name: grade, dtype: category
    Categories (5, object): [very bad, bad, medium, good, very good]
```

4.按整理后的类别排序(并非词汇的顺序):

```python
>>> df.sort_values(by="grade")
       id raw_grade      grade
    5   6         e   very bad
    1   2         b       good
    2   3         b       good
    0   1         a  very good
    3   4         a  very good
    4   5         a  very good
```

5.按类别分组也包括空类别:

```python
>>> df.groupby("grade").size()
    grade
    very bad     1
    bad          0
    medium       0
    good         2
    very good    3
    dtype: int64
```

## 数据IO ##

```python
# 从 csv 文件读取数据
pd.read_csv('foo.csv')

# 保存到 csv 文件
df.to_csv('foo.csv')

# 读取 excel 文件
pd.read_excel('foo.xlsx', 'Sheet1', index_col=None, na_values=['NA'])

# 保存到 excel 文件
df.to_excel('foo.xlsx', sheet_name='Sheet1')
```

>资料来源于：[Python科学计算之Pandas详解，pythonpandas](http://www.bkjia.com/Pythonjc/1189627.html)
>          [十分钟搞定pandas](http://python.jobbole.com/84416/)
