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


### Pandas介绍

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


### Series

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

### DataFrame

一个dataframe是一个二维的表结构。Pandas的dataframe可以存储许多种不同的数据类型，并且每一个坐标轴都有自己的标签。你可以把它想象成一个series的字典项。

创建一个 DateFrame：

```python
#创建日期索引序列 
dates = pd.date_range('20130101', periods=6)
#创建Dataframe，其中 index 决定索引序列，columns 决定列名
df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=list('ABCD'))
print df
```





#### String 操作简化了

`String` 许多要通过 `.characters` 进行的操作，可以直接用 String 进行操作了。

例如：

```swift
let greeting = "Hello, 😜!"
// No need to drill down to .characters
let n = greeting.count
let endOfSentence = greeting.index(of: "!")!

```

#### Series

一个series是一个一维的数据类型，其中每一个元素都有一个标签。类似于Numpy中元素带标签的数组。其中，标签可以是数字或者字符串。

swift 4 为字符串片段新增了一个叫 `Substring` 的类型。

当你创建一个字符串的片段时，会产生一个 `Substring` 实例。`Substring` 与 `String` 用法相同， 因为子串和原字符串共享内存，所以对子串的操作快速而且高效。

```swift
let greeting = "Hi there! It's nice to meet you! 👋"
let endOfSentence = greeting.index(of: "!")! 

// 产生 Substring 实例
let firstSentence = greeting[...endOfSentence]
// firstSentence == "Hi there!"

// `Substring` 与 `String` 用法相同
let shoutingSentence = firstSentence.uppercased()
// shoutingSentence == "HI THERE!" 
```

但是要注意一个 `Substring` 保留从其生成的完整的 `String`值。 当您传递一个看似很小的 `Substring` 时，这可能导致意外的高内存开销。所以使用 `Substring`时，最好转化为 `String`.

```swift
let newString = String(substring)
```


#### 换行可以不用 `\n`了！

Swift 3，字符串换行要插入 `\n`。
例如：

![](https://ws4.sinaimg.cn/large/006tNc79gy1fjdam0wvhhj305d0283yf.jpg)

在 Swift 4 可以这样操作:

![](https://ws2.sinaimg.cn/large/006tNc79gy1fjdas2yri4j303q0260sm.jpg)

用两个 `“”“` 包裹起来的字符串会自动添加 `\n` 换行，更加直观了。注意：换行与缩进参照的是第二个 `“”“` 号的位置。

嗯，我觉得OK！

#### 支持 Unicode 9

Swift 4 支持 Unicode 9，[为现代表情符号修正了一些问题](https://oleb.net/blog/2016/12/emoji-4-0/)。


```swift
let family1 = "👨‍👩‍👧‍👦"
let family2 = "👨\u{200D}👩\u{200D}👧\u{200D}👦"
family1 == family2 // → true
```

居然还有这种操作~

### 新增 KeyPath 数据类型

KeyPath 是 Swift 4 新增加的数据类型。

定义两个结构体 `Person`与`Book` 

```swift
struct Person {
    var name: String
}

struct Book {
    var title: String
    var authors: [Person]
    var primaryAuthor: Person {
        return authors.first!
    }
}

let abelson = Person(name: "Harold Abelson")
let sussman = Person(name: "Gerald Jay Sussman")
let book = Book(title: "Structure and Interpretation of Computer Programs", authors: [abelson, sussman])
```
```swift
book[keyPath: \Book.title]
book[keyPath: \Book.primaryAuthor.name]
// 相当与
book.title
book.primaryAuthor.name
```

这里 `\Book.title` 与 `\Book.primaryAuthor.name` 就是 KeyPath.

KeyPath 可以用 `.appending` 拼接

```swift
let authorKeyPath = \Book.primaryAuthor
let nameKeyPath = authorKeyPath.appending(path: \.name)
// nameKeyPath = \Book.primaryAuthor.name
```

### 新增  `swapAt()` 函数
Swift 4 引入了一种在集合中交换两个元素的新方法: `swapAt()`

Swift 3 交换集合中的元素的用 `swap()`

```swift
var numbers = [1,2,3,4,5]
swap(&numbers[0], &numbers[1])
// numbers = [2,1,3,4,5]
```

Swift 4 中可以直接用 

```swift
var numbers = [1,2,3,4,5]
numbers.swapAt(0,1)
// numbers = [2,1,3,4,5]
```



### 其他改动

其他改动如：**新的整数协议**、**泛型下标**、**NSNumber bridging**等

可以参考：[whats new in swift4](https://github.com/ole/whats-new-in-swift-4)