---
layout:     post
title:      SVM支持向量机
subtitle:   SVM分类
date:       2017-11-18
author:     WSS
header-img: img/AI4.jpg
catalog: true
tags:
    - Machine Learning
    - SVM
---
>读《机器学习与R语言》

>[台湾大学机器学习课程](https://www.youtube.com/watch?v=A-GxGCCAIrg&index=1&list=PLXVfgk9fNX2IQOYPmqjqWsNUFl2kpk1U2)
## 算法简介 ##

支持向量机（Support Vector Machine，SVM），是一种二分类模型，它的基本模型是定义在特征空间上的间隔最大的线性分类器，支持向量机的学习策略就是间隔最大化。可形式化为一个求解**凸二次规划**的问题，也等价于正则化的合页损失函数最小化问题，支持向量机的学习算法是求解凸二次规划的最后花算法。

当训练数据线性可分，    通过硬间隔最大化，学习结果称为硬间隔支持向量机（线性可分支持向量机）
当训练数据近似线性可分，通过软间隔最大化，学习结果称为软间隔支持向量机（线性支持向量机）
当训练数据线性不可分，通过核技巧及软间隔最大化，学习结果称为非线性支持向量机

可以想象成一个平面，该平面定义了各个数据点之间的界限，而这些数据店代表根据它们的特征值，会旨在，多维空间中的案例，支持向量机的目标是创建一个平面边界，称为超平面，使得任何一边的数据划分都是相当均匀的，通过这种方式，支持向量机学习，结合了基于实例的近邻学习和线性回归建模的两个方面，允许支持向量机，对非常复杂的关系进行建模。

虽然推动支持向量机的数学基础已经存在了几十年，但是他们今年来才人气暴涨，这当然根植于他们最先进的性能，但或许也是因为这个事实屡获殊荣的，支持向量机算法已经通过许多程序员在一些受到欢迎和大力支持的库中得到实现，这使得支持向量机得到更广泛的用户接受，而这些用户之前可能因为支持向量机的实现设计比较复杂的数学而不去用它，然而好消息是，尽管数学可能很难，但是基本的概念是可以理解的。

当支持向量机用于二元分类时，它最容易理解，这就是为什么该方法已经被习惯应用的原因，因此在剩下的部分，我们只将专注于支持向量机分类器，然而不必担心，当支持向量机适用于其他学习任务，比如数值预测，这里学习的原则同样适用。


## 适用 ##

支持向量机几乎可以适用于所有的学习任务，包括分类和数值预测两个方面，许多算法成功的关键都是来自于模式识别，著名的应用包括：

·用于文本和超文本的分类，在归纳和直推方法中都可以显著减少所需要的有类标的样本数

·用于图像分类。实验结果显示：在经过三到四轮相关反馈之后，比起传统的查询优化方案，支持向量机能够获取明显更高的搜索准确度。这同样也适用于图像分区系统，比如使用Vapnik所建议的使用特权方法的修改版本SVM的那些图像分区系统。

·用于手写字体识别。

·用于医学中分类蛋白质，超过90%的化合物能够被正确分类。基于支持向量机权重的置换测试已被建议作为一种机制，用于解释的支持向量机模型。[6][7] 支持向量机权重也被用来解释过去的SVM模型。[8] 为识别模型用于进行预测的特征而对支持向量机模型做出事后解释是在生物科学中具有特殊意义的相对较新的研究领域。


## 用超平面分类 ##

可参见[感知机](http://wangsai.top/2017/11/16/GitMachinesLearning-%E6%84%9F%E7%9F%A5%E6%9C%BA/)

## 线性可分支持向量机与硬间隔最大化 ##

### 函数间隔与几何间隔 ###

### 间隔最大化 ###

支持向量机学习的基本想法是求解能够正确划分训练数据集并且**几何间隔**最大的分离超平面

## 线性支持向量机与软间隔最大化 ##

加入一个惩罚参数C，C大时对误分类的惩罚增大，C小时对误分类的惩罚减小。

以下几个图展示了在不同的 C 值中分类器和间隔的变化（未显示支持向量）:
![](http://oyug2kd6x.bkt.clouddn.com//MachineLearning/SVMC.png)
[https://www.jiqizhixin.com/articles/2017-10-08☆☆](https://www.jiqizhixin.com/articles/2017-10-08)

此时的最小化目标函数有两部分组成，一部分是原有的w的L2范数的平方，令一部分是C与松弛变量的乘积。

## 非线性支持向量机与核函数 ##

非线性问题往往不好求解。所以希望能用解线性分类问题的方法去解决这个问题。所采取的方法是进行一个非线性变化。将非线性问题变换为线性问题。通过解变换后的线性问题的方法，求解原来的非线性问题。

### 核技巧 ###

核技巧运用到支持向量机，其基本想法就是通过一个非线性变换，将输入空间对应于一个（更高维）特征空间，使得在输入空间Rn中的超曲面模型对应于特征空间中的平面模型（支持向量机）。

 
在特征空间中，我们对线性可分的新样本使用前面提到过的求解线性可分的情况下的分类问题的方法时，需要计算样本内积，但是因为样本维数很高，容易造成“维数灾难”，所以这里我们就引入了核函数，把高维向量的内积转变成了求低维向量的内积问题。
[http://blog.csdn.net/jiangjieqazwsx/article/details/51418681](http://blog.csdn.net/jiangjieqazwsx/article/details/51418681)

如何确定要将数据映射到什么样的空间？
 
文件：E/x学习/机器

[https://www.jiqizhixin.com/articles/2017-10-08☆☆](https://www.jiqizhixin.com/articles/2017-10-08)

## 例子 ##



```R

library(kernlab)

letters <- read.csv("letterdata.csv")
str(letters)

letters_train <- letters[1:16000,]
letters_test <- letters[16001:20000,]


letter_classifier <- ksvm(letter ~ .,data = letters_train,kernel = "vanilladot")
letter_classifier

letter_predictions <- predict(letter_classifier,letters_test)
head(letter_predictions)
table(letter_predictions,letters_test$letter)


agreement <- letter_predictions == letters_test$letter

prop.table(table(agreement))

```
```R

#原型：m <-  ksvm(target ~ predictors, data = iris,type = 'C-bsvc', kernel = 'rbfdot',kpar = list(sigma = 0.1), C = 10,prob.model = TRUE) 
#target:数据框mydata中需要建模的输出变量
#predictors：是给出数据框mydata中用于预测的一个R公式，用“.”代替其他列数据
#type:表示是用于分类还是回归，还是检测，取决于y是否是一个因子。缺省取C-svc或eps-svr可取值有:
	C-svc 　C classification 分类
	nu-svc 　nu classification
	C-bsvc　 bound-constraint svm classification
	spoc-svc　 Crammer, Singer native multi-class
	kbb-svc　 Weston, Watkins native multi-class
	one-svc 　novelty detection 
	eps-svr 　epsilon regression   回归
	nu-svr 　nu regression
	eps-bsvr bound-constrain t svm regression
kernel：核函数，也可自己写
kpar：没见有用过的
C：惩罚的大小，c越大边界越窄

```