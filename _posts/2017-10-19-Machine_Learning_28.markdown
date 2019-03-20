---
layout: post
title:  机器学习（二十八）——异常检测, AutoML
category: ML 
---

# 异常检测

1.基于聚类算法的异常检测。

2.基于孤立森林算法的异常检测。

3.基于支持向量机算法的异常检测。

4.基于高斯分布的异常检测。

5.基于马尔可夫链的异常检测。

参考：

http://chuansong.me/n/377440751130

异常点检测算法（一）

http://jiangshuxia.9.blog.163.com/blog/static/3487586020083662621887/

异常(Outlier)检测算法综述

http://www.cnblogs.com/fengfenggirl/p/iForest.html

异常检测算法--Isolation Forest

https://mp.weixin.qq.com/s/xsuLIMPJVCThBGMRlz09Hg

Isolation Forest算法原理详解

http://www.bigdata8.top/front/article/466

异常值检测-滑动均值实现智能告警

https://mp.weixin.qq.com/s/ujG6Fr161kZh3S-lEiTJUg

异常检测（Anomaly Detection）

https://mp.weixin.qq.com/s/_Sds7O1wonVARKkb7qscww

腾讯：机器学习构建通用的数据异常检测平台

https://mp.weixin.qq.com/s/UcMPIf6ZRAhPjn79H2n1ig

异常点检测算法小结

https://mp.weixin.qq.com/s?__biz=MzIxODM4MjA5MA==&mid=2247487342&idx=3&sn=1981a6caec4591a19043d7a6176d359f

异常检测的阈值，你怎么选？给你整理好了...

https://mp.weixin.qq.com/s/ReQpT9KT6_tE8vXM-F_Ejw

从“马蜂窝事件”看，投资人如何避免数据尽职调查背后的交易风险？新时代数据造假特征及应对方法

https://www.zhihu.com/question/30508773

反欺诈(Fraud Detection)中所用到的机器学习模型有哪些？

https://mp.weixin.qq.com/s/jB5o4a6O4rrhMYD0GhKclw

基于机器学习算法的时间序列价格异常检测

https://mp.weixin.qq.com/s/PKX13Fv5fWmgX5IHLYgjmQ

基于无监督学习的期权定价异常检测

https://mp.weixin.qq.com/s/l1OKsahXHrFGr4YX441c9A

时序数据异常检测工具/数据集大列表

# AutoML

## 概述

尽管现在已经有许多成熟的ML算法，然而大多数ML任务仍依赖于专业人员的手工编程实现。

然而但凡做过若干同类项目的人都明白，在算法选择和参数调优的过程中，有大量的套路可以遵循。

比如有人就总结出参加kaggle比赛的套路：

http://www.jianshu.com/p/63ef4b87e197

一个框架解决几乎所有机器学习问题

https://mlwave.com/kaggle-ensembling-guide/

Kaggle Ensembling Guide

既然是套路，那么就有将之自动化的可能，比如下面网页中，就有好几个AutoML的框架：

https://mp.weixin.qq.com/s/QIR_l8OqvCQzXXXVY2WA1w

十大你不可忽视的机器学习项目

下面给几个套路图：

![](/images/article/ML.png)

![](/images/article/AutoML.jpg)

![](/images/img2/AutoNLP.png)

## 超参数

所谓hyper-parameters，就是机器学习模型里面的框架参数，比如聚类方法里面类的个数，或者话题模型里面话题的个数等等，都称为超参数。它们跟训练过程中学习的参数（权重）是不一样的，通常是手工设定，不断试错调整，或者对一系列穷举出来的参数组合一通枚举（叫做网格搜索）。

AutoML很大程度上就是自动化寻找合适的hyper-parameters的方案或方法。

参见：

http://blog.csdn.net/xiewenbo/article/details/51585054

什么是超参数

http://www.cnblogs.com/fhsy9373/p/6993675.html

如何选取一个神经网络中的超参数hyper-parameters

https://mp.weixin.qq.com/s/Q7Xqb-GZXktFIM5yW8moPg

机器学习中的超参数的选择与交叉验证

## 参考

https://mp.weixin.qq.com/s/-0--sZXjMvFKxxo87w4udg

自动机器学习工具全景图：精选22种框架，解放炼丹师

http://blog.csdn.net/aliceyangxi1987/article/details/71079448

一个框架解决几乎所有机器学习问题

https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-algorithm-cheat-sheet

MS提供的ML算法选择指南

https://mp.weixin.qq.com/s/53AcAZcCKBZI-i1CORl0bQ

分分钟带你杀入Kaggle Top 1%

https://mp.weixin.qq.com/s/NwVGkAcoDmyXKrYFUaK2Bw

如何在机器学习竞赛中更胜一筹？

https://mp.weixin.qq.com/s/5v80Qz2nEfoAig0ft_HzaA

Kaggle求生

https://mp.weixin.qq.com/s/K3EVwRFBJufXK5QKSQsPbQ

这是一份为数据科学初学者准备的Kaggle竞赛指南

https://mp.weixin.qq.com/s/hf4IOAayS29i6GB9m4GHcA

全自动机器学习：ML工程师屠龙利器

https://mp.weixin.qq.com/s/h2QQhoBfnEhU12RgatT3EA

机器学习都能自动化了？

https://mp.weixin.qq.com/s/-n-5Cp_hgkvdmsHGWEIpWw

自动化机器学习第一步：使用Hyperopt自动选择超参数

https://mp.weixin.qq.com/s/Nbwii7Di_h5Ewy5p5xzBdQ

解决机器学习问题有通法

http://automl.info/

某牛的blog

https://mp.weixin.qq.com/s/gXkD2PPNRhZGcXDxDXRAiQ

由0到1走入Kaggle-入门指导

https://mp.weixin.qq.com/s/2ZwhNN7kqigwRLqRnUOENw

谷歌做了45万次不同类型的文本分类后，总结出一个通用的“模型选择算法”

https://mp.weixin.qq.com/s/WQ-8OvF9-fRpRf5lgr5_iw

一种简单有效的网络结构搜索

https://mp.weixin.qq.com/s/g8U2C9bi75mE5OqWLPkgQw

自动化学习框架（AutoML）的性能比较

https://mp.weixin.qq.com/s/DkZGkI-CnEHfhXTDyp2nHQ

超参数搜索不够高效？这几大策略了解一下

https://zhuanlan.zhihu.com/p/48642938

分享一篇比较全面的AutoML综述

https://mp.weixin.qq.com/s/zE8N5snKK2EoM9WgAhI-_g

NeurIPS 2018 AutoML Phase1 冠军队伍 DeepSmart 团队解决方案分享

https://mp.weixin.qq.com/s/z6CaHP7I4WkJAu-eAJPwAg

自动机器学习计算量大！这种多保真度优化技术是走向应用的关键

https://mp.weixin.qq.com/s/JPAZTdvcxY3sgWukbn3ScQ

AutoML在推荐系统中的应用

https://mp.weixin.qq.com/s/95FH-_L5smx7WoNnfucWVg

为什么说自动化特征工程将改变机器学习的方式
