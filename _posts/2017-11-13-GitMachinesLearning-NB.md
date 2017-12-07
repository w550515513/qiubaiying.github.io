---
layout:     post
title:      NB朴素贝叶斯
subtitle:   naive bayes
date:       2017-11-25
author:     WSS
header-img: img/AI4.jpg
catalog: true
tags:
    - Machine Learning
    - NB
---
>读《机器学习与R语言》

>[台湾大学机器学习课程](https://www.youtube.com/watch?v=A-GxGCCAIrg&index=1&list=PLXVfgk9fNX2IQOYPmqjqWsNUFl2kpk1U2)
## 算法简介 ##

>本算法适合看《机器学习与R语言》的一个例子学习

贝叶斯分类是一类分类算法的总称，这类算法均以贝叶斯定理为基础，故统称为贝叶斯分类。而朴素朴素贝叶斯分类是贝叶斯分类中最简单，也是常见的一种分类方法。 

叫它朴素贝叶斯分类是因为这种方法的思想真的很朴素，朴素贝叶斯的思想基础是这样的：对于给出的待分类项，求解在此项出现的条件下各个类别出现的概率，哪个最大，就认为此待分类项属于哪个类别。通俗来说，就好比这么个道理，你在街上看到一个黑人，我问你你猜这哥们哪里来的，你十有八九猜非洲。为什么呢？因为黑人中非洲人的比率最高，当然人家也可能是美洲人或亚洲人，但在没有其它可用信息下，我们会选择条件概率最大的类别，这就是朴素贝叶斯的思想基础。


## 贝叶斯分类的基础——贝叶斯定理 ##

P(A|B)表示事件B已经发生的前提下，事件A发生的概率，叫做事件B发生下事件A的条件概率。

`P(A|B)=P(AB)/P(B)`

我们在生活中经常遇到这种情况：我们可以很容易直接得出P(A|B)，P(B|A)则很难直接得出，但我们更关心P(B|A)，贝叶斯定理就为我们打通从P(A|B)获得P(B|A)的道路。

**`P(B|A)=P(A|B)P(B)/P(A)`**

## 适用 ##

优点：学习和预测的效率高，且易于实现；在数据较少的情况下仍然有效，可以处理多分类问题。

缺点：分类效果不一定很高，特征独立性假设会是朴素贝叶斯变得简单，但是会牺牲一定的分类准确率。

典型应用：垃圾邮件分类。

## 例子 ##

垃圾短信分类（《机器学习与R语言》P71）
[数据与程序]()

数据准备

```python

#本次数据有两列，一列是 spam|ham(垃圾|非垃圾)，另一列是数据
sms_raw <- read.csv("sms_spam.csv",stringsAsFactors = FALSE)
str(sms_raw)
table(sms_raw$type)
#对数据进行编码
sms_raw$type <- factor(sms_raw$type)
str(sms_raw$type)
```

短信是由词、空格、数字、标点组成的文本字符串，处理这种类型的复杂数据需要大量的工作，要考虑除去标点与数字，还要除去停用词，还要将句子进行分词。

可以采用`tm`文本挖掘包来完成这些工作

[tm基础教程](http://yphuang.github.io/blog/2016/03/04/text-mining-tm-package/)

数据清洗如下：

```r

#转小写
corpus_clean <- tm_map(sms_corpus, tolower)

#移除数字
corpus_clean <- tm_map(corpus_clean, removeNumbers)

#移除停用词,第三个参数是字典
corpus_clean <- tm_map(corpus_clean,removeWords,stopwords())

#去除标点
corpus_clean <- tm_map(corpus_clean,removePunctuation)

#做完这些后会有很多空格，去空格
corpus_clean <- tm_map(corpus_clean,stripWhitespace)

inspect(sms_corpus[1:10])


```

数据清洗完成
进行标记，使数据适合贝叶斯算法,此处需要构建一个表格

![](http://oyug2kd6x.bkt.clouddn.com//MachineLearning/NBNB_excel.png)

然后进行数据准备

```r

#建立稀疏矩阵
sms_dtm <- DocumentTermMatrix(corpus_clean)
#建立训练集与测试集
sms_raw_train <- sms_raw[1:4200]
sms_raw_test <- sms_raw[4201:5560]
#输出文档-单词矩阵
sms_dtm_train <- sms_dtm[1:4200,]
sms_dtm_test<- sms_dtm[4201:5560,]
#得到语料库：
sms_corpus_train <- corpus_clean[1:4200]
sms_corpus_test <-corpus_clean[4201:5560]

```

>未完待续

