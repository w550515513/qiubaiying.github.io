---
layout:     post
title:      K近邻
subtitle:   KNN
date:       2017-11-13
author:     WSS
header-img: img/AI4.jpg
catalog: true
tags:
    - Machine Learning
    - sum
---
>读《机器学习与R语言》

## 算法简介 ##

k-近邻算法是用于分类。分类问题是监督学习算法的一个研究方向。既然是属于监督学习的，就需要1个训练数据集作为判断的基础。

kNN的学习方法很朴素，简单说就是站队，对于已有的训练集，当传入未知数据待分类时，分别计算未知数据和训练集所有数据点的距离，将距离从小到大取前k个。根据这k个点的分类情况判断未知数据的分类。通常哪个类别在前k个数据点中数量最多，我们就认为未知数据是哪个类别的。

![](http://oyug2kd6x.bkt.clouddn.com//MachineLearning/KNNMachineLearingKNN.jpg)

如图，在拥有三个分类的若干样本点的训练集中，对未知元素Xu，我们取k=5，选取距离其最近的5个训练集的点，其中4个是红色圆圈，1个是绿色方块，0个蓝色三角，那么我们认为，其最可能所属的分类是红色圆圈。

## 适用 ##

一般来说，近邻分类器比较适合：特征和目标之间的关系众多、复杂。用其他方式难以理解，但是具有相似类的项目又是非常近似，也就是说，一个概念很难理解，但是当你看到它时你知道他是什么，那么近邻分类就是合适的方法。但是如果组与组之间没有明确的界限，那么该算法就不太合适。

目前，近邻分类已经成功应用在：

·计算机视觉应用，包括在静止图像和视频中的光学字符识别和面部识别。

·预测一个人是否喜欢推荐给他们的电影。

·识别基因数据的模式，用于发现特定的蛋白质或疾病。

优点 | 缺点
---- | ---
简单有效 | 不产生模型，在发现特征之间关系上的能力有限
对数据的分布没有要求 |  分类阶段很慢，需要大量内存
训练阶段很快 |  **名义变量（特征）**和缺失数据需要额外处理

其他缺点：

对训练集要求苛刻，kNN要求训练集不同类别的数据不能相差太多，对上图的数据，可以预见，因为样本过少很有可能很少有数据被分类成蓝色圆圈—-即使它可能真的是蓝色圆圈

![](http://oyug2kd6x.bkt.clouddn.com//MachineLearning/KNNKNNquedian.jpg)

计算量大。慢是kNN的致命伤，每一次都要计算未知数据和所有训练集的距离，当训练集非常大的时候，这会变得很慢；而训练集小的话，又会显得精确度不够。

![](http://oyug2kd6x.bkt.clouddn.com//MachineLearning/KNNKNNyabianliang.gif)


## K近邻推导 ##

《统计学》(李航)
[基于K近邻的分类算法研究](http://pan.baidu.com/s/1o79A7vc)

## 计算距离 ##

最常用的是采用欧式距离(Euclidean distance),欧式距离是通过直线距离(as the crow flies)来度量,即最短的直线路线.另一种常见的距离度量是曼哈顿距离(anhattan distance),即两点在南北方向上的距离加上在东西方向上的距离.

下图中红线代表曼哈顿距离，绿色`代表欧氏距离，蓝色和黄色代表等价的曼哈顿距离.

![](http://oyug2kd6x.bkt.clouddn.com//MachineLearning/KNNKNNjuli.png)

## 选择一个合适的K ##

确定用于KNN算法的邻居数量（K）将决定把模型推广到未来数据时模型的好坏，过度拟合和低度拟合训练数据之间的平衡问题称为**偏差-方差权衡**。选择一个合适的k会减少噪声数据对模型的影响或减少噪声导致的模型波动，但是它会使分类器产生偏差，比如，它有忽视不易察觉但却很重要模式的风险。

一个极端的情况，k取得非常大，它等于将训练数据中所有观测值的数量，训练类总会获得大多数的票，即模型总会预测为数量最多的那个训练类，而不管哪个邻居最近

相反，k=1，这时，假如一些训练案例被意外贴错了标签，而另一些未标记的案例恰好是最接近于被错误标记的训练案例，那么，它就会被预测到错误的类中。

在实际中，k的选择取决于要学习概念的难度和训练数据中案例的数量。通常，k为3-10。一种常见的做法就是设置k=案例数量的开根号。当然，这样的规则可能并不总是产生最好的k值，另一种方法是基于各种测试数据集来测试读个K值，并选择一个可以提供最好分类性能的k值。

从一个方面说，除非数据的噪声非常大，否则更大、更具代表性的训练数据集可以使K值的选择不那么重要。这是因为即使是微小的概念，也将有一个足够大的案例池来进行投票，以便选举出近邻。

>一个不太常见，但有趣的解决方法是选择一个较大的k，但是同时应用一个**权值投票**过程，认为较近邻的投票比远邻的投票更加有权威。

## 准备kNN算法使用的数据 ##

在应用kNN算法之前，通常将特征转化为一个标准的范围内。即使得不同的特征有相同的度量尺度。

对kNN算法的特征进行重新调整的传统方法是**min-max标准化**(min-max normalization)，该过程对特征转化，使它所有的值都落在0~1的范围。公式如下

![](http://oyug2kd6x.bkt.clouddn.com//MachineLearning/KNNKNNmin-max.jpg)

![](http://oyug2kd6x.bkt.clouddn.com//MachineLearning/KNNKNNz_score.jpg)

z-score落在一个无界的负数和正数构成的范围内，没有最大值与最小值。

欧式公式不是为名义数据定义的，为例计算名义特征（变量）之间的距离，需要将他们转化成数值型格式，一种典型的解决方案是利用哑变量编码：

对于名义特征，如 性别，类别或其他属性需要将其转换为数值型格式。一种典型的解决方法就是利用哑变量编码（dummy coding）,其中1表示一个类别,0表示其他类别.

## 手写数字识别 ##

### 任务 ###

手写识别的概念：是指将在手写设备上书写时产生的轨迹信息转化为具体字码。
手写识别系统是个很大的项目，识别汉字、英语、数字、其他字符。本文只是个小demo，重点不在手写识别而在于理解kNN，因此只识别0～9单个数字。

输入格式：每个手写数字已经事先处理成32*32的二进制文本，存储为txt文件。0～9每个数字都有10个训练样本，5个测试样本。

[数据及程序](https://github.com/w550515513/MachineLearning)

### 算法步骤 ###

1. step.1—初始化距离为最大值

2. step.2—计算未知样本和每个训练样本的距离dist

3. step.3—得到目前K个最临近样本中的最大距离maxdist

4. step.4—如果dist小于maxdist，则将该训练样本作为K-最近邻样本

5. step.5—重复步骤2、3、4，直到未知样本和所有训练样本的距离都算完

6. step.6—统计K-最近邻样本中每个类标号出现的次数

7. step.7—选择出现频率最大的类标号作为未知样本的类标号

现在编程实现三个步骤：
（1）将每个图片（即txt文本，以下提到图片都指txt文本）转化为一个向量，即32*32的数组转化为1*1024的数组，这个1*1024的数组用机器学习的术语来说就是特征向量。

（2）训练样本中有10*10个图片，可以合并成一个100*1024的矩阵，每一行对应一个图片。（这是为了方便计算，很多机器学习算法在计算的时候采用矩阵运算，可以简化代码，有时还可以减少计算复杂度）。

（3）测试样本中有10*5个图片，我们要让程序自动判断每个图片所表示的数字。同样的，对于测试图片，将其转化为1*1024的向量，然后计算它与训练样本中各个图片的“距离”（这里两个向量的距离采用欧式距离），然后对距离排序，选出较小的前k个，因为这k个样本来自训练集，是已知其代表的数字的，所以被测试图片所代表的数字就可以确定为这k个中出现次数最多的那个数字。

第一步：转化为1*1024的特征向量。程序中的filename是文件名，比如3_3.txt

```python
def img2vector(filename):#将32行的数据转为1行数据
    returnVect = zeros((1,1024))
    fr = open(filename)
    for i in range(32):
        lineStr = fr.readline()
        for j in range(32):
            returnVect[0,32*i+j] = int(lineStr[j])
    return returnVect
```

第二步、第三步：将训练集图片合并成100*1024的大矩阵，同时逐一对测试集中的样本分类

```python
def handwritingClassTest():

    hwLabels = []
    trainingFileList = listdir('trainingDigits')#得到训练文件夹下所有文件列表         
    m = len(trainingFileList) #一共m个文件
    trainingMat = zeros((m,1024))#建立m*1024的数组
    for i in range(m):
        fileNameStr = trainingFileList[i]            
        fileStr = fileNameStr.split('.')[0] #得到0_1                
        classNumStr = int(fileStr.split('_')[0])#得到0          
        hwLabels.append(classNumStr)#得到真实结果
        trainingMat[i,:] = img2vector('trainingDigits/%s' % fileNameStr)
     
    testFileList = listdir('testDigits')       
    errorCount = 0.0
    mTest = len(testFileList)
    for i in range(mTest):
        fileNameStr = testFileList[i]
        fileStr = fileNameStr.split('.')[0]     
        classNumStr = int(fileStr.split('_')[0])
        vectorUnderTest = img2vector('testDigits/%s' % fileNameStr)
        classifierResult = classify0(vectorUnderTest, trainingMat, hwLabels, 3)
        print("the classifier came back with: %d, the real answer is: %d" % (classifierResult, classNumStr))
        if (classifierResult != classNumStr): errorCount += 1.0
    print("\nthe total number of errors is: %d" % errorCount)
    print("\nthe total error rate is: %f" % (errorCount/float(mTest)))
```

这里面的函数classify()为分类主体函数，计算欧式距离，并最终返回测试图片类别：

#分类主体程序，计算欧式距离，选择距离最小的k个，返回k个中出现频率最高的类别 
```python 
def classify0(inX, dataSet, labels, k):#inX一个测试数据，dataSet所有的训练数据，labels标签 ， k=k
    dataSetSize = dataSet.shape[0]    #shape[0]得出dataSet的行数，即样本个数              
    diffMat = tile(inX, (dataSetSize,1)) - dataSet   #将数组inX按行重复m遍
    sqDiffMat = diffMat**2   #幂 - 返回x的y次幂
    sqDistances = sqDiffMat.sum(axis=1) #axis＝1表示按照行的方向相加           
    distances = sqDistances**0.5
    sortedDistIndicies = distances.argsort()  #array.argsort()，得到每个元素的排序序号           
    classCount={}                                      
    for i in range(k):
        voteIlabel = labels[sortedDistIndicies[i]]
        classCount[voteIlabel] = classCount.get(voteIlabel,0) + 1
    sortedClassCount = sorted(classCount.iteritems(), key=operator.itemgetter(1), reverse=True)
    return sortedClassCount[0][0]
```
>[资料](http://blog.csdn.net/u012162613/article/details/41768407)
