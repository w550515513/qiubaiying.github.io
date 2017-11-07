---
layout:     post
title:      Ubantu16的安装
subtitle:   搭建caffe之系统安装
date:       2017-11-07
author:     WSS
header-img: img/post-bg-yinxin.jpg
catalog: true
tags:
    - ubantu
    - caffe
---


## 制作启动U盘 ##

进入UltraISO，打开文件：

![](http://oyug2kd6x.bkt.clouddn.com/ultrISO.png)

打开镜像文件

启动>写入硬盘映像：

![](http://oyug2kd6x.bkt.clouddn.com/ultrISO2.png)

写入硬盘映像

按默认值写入：

![](http://oyug2kd6x.bkt.clouddn.com/ultrISO3.png)



## U盘安装Ubuntu ##

1. AUSU开机按`esc`选择启动方式，选择U盘启动。
2. 选择安装ubantu。
3. 安装类型选择`其他选项`。
 ![](http://oyug2kd6x.bkt.clouddn.com/anzhuangleixing.jpg)
5. 磁盘分区。

![](http://oyug2kd6x.bkt.clouddn.com/fenqu.png)

点击`-`腾出一个空闲磁盘，由于一块硬盘最多容纳4个主分区，或3个主分区外加1个扩展分区，在选择分区类型时，可能会出现“安装系统时空闲分区不可用”状况，为了解决问题，下面一律选择“逻辑分区”。若无法实现，则全部选择“逻辑分区”。



**选择/boot对应的盘符作为“安装启动引导器的设备”，务必保证一致**

![](http://oyug2kd6x.bkt.clouddn.com/yizhi.jpg)

## windows下引导 ##

进入EasyBCD，选择“添加新条目”，选择Linux/BSD操作系统，在“驱动器”栏目选择接近200M的Linux分区：

![](http://oyug2kd6x.bkt.clouddn.com/yindao.png)

## OK ##

- ubantu下可以看到windows盘，但windows下看不到ubantu的盘，可能需要共享。


>资料参考：[Beginners Guide To Install Windows 10 With Ubuntu ](http://itsfoss.com/install-ubuntu-1404-dual-boot-mode-windows-8-81-uefi/)
>          [   Ubuntu安装](http://www.jianshu.com/p/0ccf1778d8ae)
