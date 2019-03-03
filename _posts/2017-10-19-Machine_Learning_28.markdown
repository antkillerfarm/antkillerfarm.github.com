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

# Temporal-Difference Learning

## TD

时序差分学习和蒙特卡洛学习一样，它也从Episode学习，不需要了解模型本身；但是它可以学习不完整的Episode，通过bootstrapping，猜测Episode的结果，同时持续更新这个猜测。

最简单的TD算法——TD(0)的更新公式如下：

$$V(S_t)\leftarrow V(S_t)+\alpha(R_{t+1}+\gamma V(S_{t+1})-V(S_t))$$

其中，$$R_{t+1}+\gamma V(S_{t+1})$$被称作TD target，而$$\delta_t=R_{t+1}+\gamma V(S_{t+1})-V(S_t)$$被称作TD error。

下面用驾车返家的例子直观解释蒙特卡洛策略评估和TD策略评估的差别。

想象一下你下班后开车回家，需要预估整个行程花费的时间。假如一个人在驾车回家的路上突然碰到险情：对面迎来一辆车感觉要和你相撞，严重的话他可能面临死亡威胁，但是最后双方都采取了措施没有实际发生碰撞。如果使用蒙特卡洛学习，路上发生的这一险情可能引发的负向奖励不会被考虑进去，不会影响总的预测耗时；但是在TD学习时，碰到这样的险情，这个人会立即更新这个状态的价值，随后会发现这比之前的状态要糟糕，会立即考虑决策降低速度赢得时间，也就是说你不必像蒙特卡洛学习那样直到他死亡后才更新状态价值，那种情况下也无法更新状态价值。

TD算法相当于在整个返家的过程中（一个Episode），根据已经消耗的时间和预期还需要的时间来不断更新最终回家需要消耗的时间。

| 状态 | 已消耗时间 | 预计仍需耗时 | 预测总耗时 |
|:--:|:--:|:--:|:--:|
| 离开办公室 | 0 | 30 | 30 |
| 取车，发现下雨 | 5 | 35 | 40 |
| 离开高速公路 | 20 | 15 | 35 |
| 被迫跟在卡车后面 | 30 | 10 | 40 |
| 到达家所在街区 | 40 | 3 | 43 |
| 到家 | 43 | 0 | 43 |

基于上表所示的数据，下图展示了蒙特卡洛学习和TD学习两种不同的学习策略来更新价值函数（各个状态的价值）。这里使用的是从某个状态预估的到家还需耗时来间接反映某状态的价值：某位置预估的到家时间越长，该位置价值越低，在优化决策时需要避免进入该状态。对于蒙特卡洛学习过程，驾驶员在路面上碰到各种情况时，他不会更新对于回家的预估时间，等他回到家得到了真实回家耗时后，他会重新估计在返家的路上着每一个主要节点状态到家的时间，在下一次返家的时候用新估计的时间来帮助决策。

而对于TD学习，在一开始离开办公室的时候你可能会预估总耗时30分钟，但是当你取到车发现下雨的时候，你会立刻想到原来的预计过于乐观，因为既往的经验告诉你下雨会延长你的返家总时间，此时你会更新目前的状态价值估计，从原来的30分钟提高到40分钟。同样当你驾车离开高速公路时，会一路根据当前的状态（位置、路况等）对应的预估返家剩余时间，直到返回家门得到实际的返家总耗时。这一过程中，你会根据状态的变化实时更新该状态的价值。

![](/images/img2/TD.png)

