---
layout:     post
title:      Perceptron感知机
subtitle:   感知机
date:       2017-11-16
author:     WSS
header-img: img/AI4.jpg
catalog: true
tags:
    - Machine Learning
    - 感知机
---
>读《机器学习与R语言》

## 算法简介 ##

感知机是二类分类的线性分类模型，其输入实例的特征向量，输出为实例的类别，取+1和-1两类，感知机对应于输入空间中，将实例划分为正负两类的分离超平面，属于判别模型。感知机学习旨在求出将训练数据进行线性划的分离超平面，为此导入基于误分类的损失函数，利用梯度下降法对损失函数进行极小化，求的感知机模型，感知机学习算法，具有简单而易于实现的优点，分为原始形式和对偶欧形式，感知机预测是用学习的感知器模型最新的输入实例，是神经网络与支持向量机的基础

## 感知机学习策略 ##

[《统计学》](https://book.douban.com/subject/10590856/) P26

## 支持向量机与感知机 ##

感知器（英语：Perceptron）一种人工神经网络。它可以被视为一种最简单形式的前馈神经网络，是一种二元线性分类器。它不能处理线性不可分问题。

观点1、支持向量机与感知机是一种东西，SVM ≈ L2 regularized Perceptron

观点2、他们不是一种。

[备忘](https://www.zhihu.com/question/51500780)

等看完支持向量机在理解

## 算法实现 ##

[《统计学》](https://book.douban.com/subject/10590856/) P29例2.1题目

```python
import os

# An example in that book, the training set and parameters' sizes are fixed
training_set = [[(3, 3), 1], [(4, 3), 1], [(1, 1), -1]]

w = [0, 0]
b = 0

# update parameters using stochastic gradient descent
def update(item):
    global w, b
    w[0] = w[0] + 1 * item[1] * item[0][0]
    w[1] = w[1] + 1 * item[1] * item[0][1]
    b = b + 1 * item[1]
    # print w, b # you can uncomment this line to check the process of stochastic gradient descent

# calculate the functional distance between 'item' an the dicision surface
def cal(item):
    global w, b
    res = 0
    for i in range(len(item[0])):
        res += item[0][i] * w[i]
    res += b
    res *= item[1]
    return res

# check if the hyperplane can classify the examples correctly
def check():
    flag = False
    for item in training_set:
        if cal(item) <= 0:
            flag = True
            update(item)
    if not flag:
        print "RESULT: w: " + str(w) + " b: "+ str(b)
        os._exit(0)
    flag = False

if __name__=="__main__":
    for i in range(1000):
        check()
    print "The training_set is not linear separable. "
```
>[参考](http://www.cnblogs.com/OldPanda/archive/2013/04/12/3017100.html)

## 感知机的对偶形式 ##

![](http://oyug2kd6x.bkt.clouddn.com//MachineLearning/ganzhiji%E6%84%9F%E7%9F%A5%E6%9C%BA%E5%AF%B9%E5%81%B6.jpg)

假设按照感知机原始形式所有的参数更新一共需要n次，对偶形式就是把这n次分摊到i个样本中去，这样最终的参数可以展开使用每个样本点进行表示，这样在判断误分类的时候的计算就都可以展开成样本之间的点乘形式，这样就可以通过提前算好的Gram矩阵来大大降低计算量，因为仅仅计算了一次，后续全部通过查表就可以了。而反观原始形式，每次参数改变，所有的矩阵计算全部需要计算，导致计算量比对偶形式要大很多。以上是我个人的理解，我也是刚看这本书，希望可以多交流一起学习


[资料](https://www.zhihu.com/question/26526858/answer/134536398)
[资料](https://www.zhihu.com/question/26526858/answer/136577337)


## 从神经网络理解感知机 ##

感知机的另一种理解是从神经网络出发。 [维基百科](https://zh.wikipedia.org/zh-sg/%E6%84%9F%E7%9F%A5%E5%99%A8)

![](http://oyug2kd6x.bkt.clouddn.com//MachineLearning/ganzhiji%E6%84%9F%E7%9F%A5%E5%99%A8%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C.png)

![](http://oyug2kd6x.bkt.clouddn.com//MachineLearning/ganzhiji%E6%84%9F%E7%9F%A5%E5%99%A8%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C2.png)


