---
layout: post
title:  机器学习（三十七）——时间序列分析（1）, 辛普森悖论
category: ML 
---

# 时间序列分析

## 书籍和教程

http://www.stat.berkeley.edu/~bartlett/courses/153-fall2010/

berkeley的时间序列分析课程

http://people.duke.edu/%7Ernau/411home.htm

回归和时间序列分析

《应用时间序列分析》，王燕著。

## 概述

时间序列，就是按时间顺序排列的，随时间变化的数据序列。

生活中各领域各行业太多时间序列的数据了，销售额，顾客数，访问量，股价，油价，GDP，气温...

随机过程的特征有均值、方差、协方差等。

如果随机过程的特征随着时间变化，则此过程是非平稳的；相反，如果随机过程的特征不随时间而变化，就称此过程是平稳的。

下图所示，左边非稳定，右边稳定。

![](/images/article/time_series.png)

非平稳时间序列分析时，若导致非平稳的原因是确定的，可以用的方法主要有趋势拟合模型、季节调整模型、移动平均、指数平滑等方法。

若导致非平稳的原因是随机的，方法主要有ARIMA及自回归条件异方差模型等。

## ARIMA

ARIMA模型全称为差分自回归移动平均模型(Autoregressive Integrated Moving Average Model,简记ARIMA)，也叫求和自回归移动平均模型，是由George Edward Pelham Box和Gwilym Meirion Jenkins于70年代初提出的一著名时间序列预测方法，所以又称为box-jenkins模型、博克思-詹金斯法。

>注：Gwilym Meirion Jenkins，1932～1982，英国统计学家。伦敦大学学院博士，兰卡斯特大学教授。

同《数学狂想曲（十一）》中的PID算法一样，ARIMA模型实际上是三个简单模型的组合。

### AR模型

$$X_t = c + \sum_{i=1}^p \varphi_i X_{t-i}+ \varepsilon_t$$

其中，p为阶数，$$\varepsilon_t$$为白噪声。上式又记作**AR(p)**。显然，AR模型是一个系统状态模型。

### MA模型

$$X_t = \mu + \varepsilon_t + \sum_{i=1}^q \theta_i \varepsilon_{t-i}$$

上式记作**MA(q)**，其中q和$$\varepsilon_t$$的含义与上同。MA模型是一个噪声模型。

### ARMA模型

AR模型和MA模型合起来，就是ARMA模型：

$$X_t = c + \varepsilon_t +  \sum_{i=1}^p \varphi_i X_{t-i} + \sum_{i=1}^q \theta_i \varepsilon_{t-i}$$

同理，上式也被记作**ARMA(p,q)**。

### Lag operator

在继续下面的描述之前，我们先来定义一下Lag operator--L。

$$L X_t = X_{t-1} \; \text{or} \; X_t = L X_{t+1}$$

### I模型

$$(1-L)^d X_t$$

上式中d为阶数，因此上式也记作**I(d)**。显然$$I(0)=X_t$$。

I模型有什么用呢？我们观察一下I(1)：

$$(1-L) X_t = X_t - X_{t-1} = \Delta X$$

有的时候，虽然I(0)不是平稳序列，但I(1)是平稳序列，这时我们称该序列是**1阶平稳序列**。n阶的情况，可依此类推。

### ARIMA模型

ARIMA模型可以看作是两个随机过程的组合。

首先是非平稳过程：

$$Y_t = (1-L)^d X_t$$

接着是一个广义平稳过程：

$$\left( 1 - \sum_{i=1}^p \phi_i L^i \right) Y_t = \left( 1 + \sum_{i=1}^q \theta_i L^i \right) \varepsilon_t$$

最后得到ARIMA模型的公式：

$$\left( 1 - \sum_{i=1}^p \phi_i L^i\right)
(1-L)^d X_t = \delta + \left( 1 + \sum_{i=1}^q \theta_i L^i \right) \varepsilon_t$$

上式也被记作**ARIMA(p,d,q)**。从上式可以看出，ARIMA模型实际上就是利用I模型，将时间序列转化为平稳序列之后的ARMA模型。

>注：上面的内容只是对ARIMA模型给出一个简单的定义。实际的假设检验、参数估计的步骤，还是比较复杂的，完全可以写本书来说。

## 其它

除了ARIMA系列模型之外，ARCH系列模型也用的比较多：

autoregressive conditional heteroskedasticity, ARCH

generalized autoregressive conditional heteroskedasticity, GARCH

上面介绍的序列建模方法主要针对的是：预测未来节点，即所谓的走势分析问题。

时间序列的常见问题还包括：

- 判断不同序列类别，即序列分类问题。

- 不同时序对应的状态的分析，即序列标注问题。

这些问题的常见工具包括HMM、CRF、RNN等，可参见其他相关章节。

## Prophet

Prophet是FaceBook提出的时间序列算法。同时，也是该算法的工具包的名字。

官网：

https://facebook.github.io/prophet/

参考：

https://mp.weixin.qq.com/s/ven_4JbWYFswIkGyhjTcww

Prophet：教你如何用加法模型探索时间序列数据

https://mp.weixin.qq.com/s/PMsAjk7WbGRu2n3s6Q8prQ

Facebook时间序列预测算法Prophet的研究

https://mp.weixin.qq.com/s/bf_CHcoZMjqP6Is4ebD58g

使用Prophet预测股价并进行多策略交易

https://mp.weixin.qq.com/s/pJTDJrMCfv5y4LQ2itt1tQ

Facebook的 Prophet 算法简介与使用

https://mp.weixin.qq.com/s/675ASxDSVH_8BX6W8WRRqg

基于Prophet的时间序列预测

## Lebesgue积分

![](/images/img3/Riemann_Lebesgue.jpg)

蓝色的是Riemann积分，红色的是Lebesgue积分。

>Henri Léon Lebesgue，1875～1941，法国数学家。

参考：

https://zhuanlan.zhihu.com/p/34407471

如何理解时间序列？—从Riemann积分和Lebesgue积分谈起

https://zhuanlan.zhihu.com/p/49262150

从Riemann积分到Lebesgue积分

https://zhuanlan.zhihu.com/p/90607361

Quadrature求积法

https://zhuanlan.zhihu.com/p/91709767

ODE's Initial value problem (IVP)

## 参考

https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average

https://en.wikipedia.org/wiki/Autoregressive%E2%80%93moving-average_model

https://zhuanlan.zhihu.com/p/23534595

时间序列分析：结合ARMA的卡尔曼滤波算法（该文的参考文献中有不少好文）

http://blog.csdn.net/aliceyangxi1987/article/details/71079522

用ARIMA模型做需求预测

http://blog.csdn.net/kicilove/article/details/78315335

时间序列初级理论篇

https://mp.weixin.qq.com/s/K-XGuaWTcF6BDPJagaJDPQ

时序数据与事件的关联分析

https://mp.weixin.qq.com/s/JR-GIXwHF45OysoE0qvwzw

时间序列异常检测机制的研究

https://mp.weixin.qq.com/s/2hpQ_7Ih58d1RKYb1oW_Sg

时间序列简介（一）

https://zhuanlan.zhihu.com/p/35093835

时间序列的自回归模型—从线性代数的角度来看

https://zhuanlan.zhihu.com/p/39105270

时间序列的表示与信息提取

https://mp.weixin.qq.com/s/iah8PvIC0oZngSaNHw7gJw

从上帝视角看透时间序列和数据挖掘

https://zhuanlan.zhihu.com/p/38130622

时间序列的相似性

https://mp.weixin.qq.com/s/DGGuAYsoa6DPD6FBf2Hc4g

时间序列分析之理论篇

https://zhuanlan.zhihu.com/p/50698719

两篇关于时间序列的论文

https://zhuanlan.zhihu.com/p/55129654

时间序列的单调性

https://zhuanlan.zhihu.com/p/55903495

时间序列的聚类

https://mp.weixin.qq.com/s/2teyejpbpM6x5UCiYL8s-Q

关于时间序列你需要了解的一切

https://mp.weixin.qq.com/s/Aqh9lZvyDncyCdXgxH1lSQ

短小时序，如何预测？——基于特征重构的张量ARIMA

# 辛普森悖论

![](/images/img2/Simpson.png)

如果分专业来看，你就会发现：在各个专业女生的录取率其实都是更高的。之所以会产生“总体录取率女生偏低”这一结果，是因为女生大部分都报考了那些本身就难以录取的学院，而男生则大部分报考了那些录取率本身就偏高的学院。

参考：

https://mp.weixin.qq.com/s/5jZ2dzLInLtUw7rWZF4mtg

张忠元：渣男受女生欢迎？当心统计陷阱

https://mp.weixin.qq.com/s/o1a2YlYritcOrsLN2YuLmA

神奇的霍特林法则：为什么汉堡王总是开在麦当劳旁边？

https://mp.weixin.qq.com/s/eq4MllJta5NmaLARPpvang

公交车总迟到？你大概掉进了“等待时间悖论”

https://zhuanlan.zhihu.com/p/43934918

诡异的布雷斯悖论：为什么越是修新路，城市反而更堵了！

https://mp.weixin.qq.com/s/-0VMucGBq4Trb_9FnsW6KQ

10大反直觉的数学结论

https://mp.weixin.qq.com/s/FqY19sTQd7GPdGSsB5L9eQ

数学大反例合集

https://mp.weixin.qq.com/s/EICefFM3dfv5A6V9kVqGWw

吸烟致癌的迷思是如何破除的

https://mp.weixin.qq.com/s/NlJ4-b5SjIjPGgvLUuSxFw

孩子，有时候并不是生活欺骗了你，而是你可能还不懂概率统计……

# 模型压缩与加速+

https://mp.weixin.qq.com/s/L49cqZ2PXP-x4Y6xBJG5cQ

旷视研究院提出MetaPruning：基于元学习和AutoML的模型压缩新方法

https://mp.weixin.qq.com/s/F_p414ezQ0RDhPnZj36SUA

网络剪枝中的AutoML方法

https://zhuanlan.zhihu.com/c_151876233

如何Finetune一个小网络到移动端（时空性能分析篇）

https://mp.weixin.qq.com/s/kU0_BuQW8VQihUwBuZ90cA

发布可伸缩超网SCARLET，小米AutoML团队NAS三部曲杀青

https://mp.weixin.qq.com/s/ZfW-jDSo6uaaqdmJtBizOA

从模型精简，硬件实现，到模型剪枝

https://mp.weixin.qq.com/s/5ywMyedmplCSLzWlnoFDSQ

模型剪枝技术原理及其发展现状和展望

https://mp.weixin.qq.com/s/8jyQ_7DYn7lHMcAWokKbcA

超Mask RCNN速度4倍，仅在单个GPU训练的实时实例分割算法

https://mp.weixin.qq.com/s/TC_Ju2vuKDP6d538v2F8CQ

剪枝需有的放矢，快手&罗切斯特大学提出基于能耗建模的模型压缩

https://mp.weixin.qq.com/s/UkqwPBYgYQuIB9_jGMt2QQ

Rocket Training: 一种提升轻量网络性能的训练方法

https://mp.weixin.qq.com/s/xCzS7sYMFmk5K4ClB1I2YQ

Uber提出SBNet：利用激活的稀疏性加速卷积网络

https://mp.weixin.qq.com/s/6Wj0Y4y30BVA75WrU4oZbQ

SBNet: 提高自动驾驶系统的感知效率

https://mp.weixin.qq.com/s/HXxnhMjAchxKSidu45kOeg

网络压缩最新进展：2019年最新文章概览

https://mp.weixin.qq.com/s/Bl7-hGIxZMsHxscqb7DnMA

200～1000+fps！谷歌公布亚毫秒级人脸检测算法BlazeFace，面向移动GPU

https://mp.weixin.qq.com/s/l2_N-PXjDMCqSRwYxU4BEA

模型加速概述与模型裁剪算法技术解析

https://mp.weixin.qq.com/s/af-z73asc-PmpEsI_yEulA

北邮提出新AI模型压缩算法，显著降低计算复杂度

https://mp.weixin.qq.com/s/AOI2LUjiKPUJFE0D7zX0Hw

谷歌新研究：基于数据共享的神经网络快速训练方法

https://mp.weixin.qq.com/s/Q0XyKIrbOIrA3YsYHmwK1Q

移动端高效率的分组网络都发展到什么程度了？

https://mp.weixin.qq.com/s/l3790PuutrOF27RRmVqJhQ

面对千万级推荐，如何压缩模型最高效？这是腾讯看点新框架

https://mp.weixin.qq.com/s/n_LY6mmJRH5k_cubYOTq1A

模型剪枝有哪些关键技术，如何对其进行长期深入学习

https://zhuanlan.zhihu.com/p/138059904

一文看懂深度学习模型压缩和加速

https://mp.weixin.qq.com/s/4aVY9vUBX_Bxht953r00sA

在Keras中利用TensorNetwork加速神经网络
