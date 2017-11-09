---
layout:     post
title:      matlab中的S函数
subtitle:   智能控制作业
date:       2017-11-03
author:     WSS
header-img: img/post-bg-beu.jpg
catalog: true
tags:
    - matlab
    - S函数
---


## S函数仿真阶段 ##

![](http://img.blog.csdn.net/20170301120330934)

```matlab
function[sys,x0,str,ts,simStateCompliance]=mdlInitializeSizes  
sizes = simsizes;  
sizes.NumContStates  = 0;  %连续状态个数  
sizes.NumDiscStates  = 0;  %离散状态个数  
sizes.NumOutputs     = 0;  %输出个数  
sizes.NumInputs      = 0;  %输入个数  
sizes.DirFeedthrough = 1;  %是否直接馈通  
sizes.NumSampleTimes = 1;  %采样时间个数，至少一个  
sys = simsizes(sizes);     %将size结构传到sys中  
x0  = [];                     %初始状态向量，由传入的参数决定，没有为空  
str = [];  
ts  = [0 0];                  %设置采样时间，这里是连续采样，偏移量为0  
% Specify the blocksimStateCompliance. The allowed values are:  
%    'UnknownSimState', < The defaultsetting; warn and assume DefaultSimState  
%    'DefaultSimState', < Same sim state as abuilt-in block  
%    'HasNoSimState',   < No sim state  
%    'DisallowSimState' < Error out whensaving or restoring the model sim state  
simStateCompliance = 'UnknownSimState';  
```

## 参数意义 ##

![](http://img.blog.csdn.net/20170301120256261)

## 仿真过程 ##

![](http://img.blog.csdn.net/20170301120004975)

Simulnk仿真分为两步：初始化、仿真循环。仿真是由求解器控制的，求解器主要作用是：计算模块输出、更新模块离散状态、计算连续状态。求解器传递给系统的信息包括：时间、输入和当前状态。系统的作用：计算模块的输出、更新状态、计算状态导数，然后将这些信息传递给求解器。求解器和系统之间的信息传递是通过不同标志来控制的。

>[资料](http://blog.csdn.net/acelit/article/details/59082349)