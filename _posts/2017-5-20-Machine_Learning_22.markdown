---
layout: post
title:  机器学习（二十二）——时间序列分析, NLP机器翻译常用评价度量, Tri-training
category: theory 
---

# 时间序列分析

## 概述（续）

下图所示，左边非稳定，右边稳定。

![](/images/article/time_series.png)

非平稳时间序列分析时，若导致非平稳的原因是确定的，可以用的方法主要有趋势拟合模型、季节调整模型、移动平均、指数平滑等方法。

若导致非平稳的原因是随机的，方法主要有ARIMA及自回归条件异方差模型等。

## ARIMA

ARIMA模型全称为差分自回归移动平均模型(Autoregressive Integrated Moving Average Model,简记ARIMA)，也叫求和自回归移动平均模型，是由George Edward Pelham Box和Gwilym Meirion Jenkins于70年代初提出的一著名时间序列预测方法，所以又称为box-jenkins模型、博克思-詹金斯法。

>注：Gwilym Meirion Jenkins，1932～1982，英国统计学家。伦敦大学学院博士，兰卡斯特大学教授。

同《数学狂想曲（三）》中的PID算法一样，ARIMA模型实际上是三个简单模型的组合。

### AR模型

$$X_t = c + \sum_{i=1}^p \varphi_i X_{t-i}+ \varepsilon_t$$

其中，p为阶数，$$\varepsilon_t$$为白噪声。上式又记作AR(p)。显然，AR模型是一个系统状态模型。

### MA模型

$$X_t = \mu + \varepsilon_t + \sum_{i=1}^q \theta_i \varepsilon_{t-i}$$

上式记作MA(q)，其中q和$$\varepsilon_t$$的含义与上同。MA模型是一个噪声模型。

### ARMA模型

AR模型和MA模型合起来，就是ARMA模型：

$$X_t = c + \varepsilon_t +  \sum_{i=1}^p \varphi_i X_{t-i} + \sum_{i=1}^q \theta_i \varepsilon_{t-i}$$

### Lag operator

在继续下面的描述之前，我们先来定义一下Lag operator--L。

$$L X_t = X_{t-1} \; \text{or} \; X_t = L X_{t+1}$$

### I模型

$$(1-L)^d X_t$$

上式中d为阶数，因此上式也记作I(d)。显然$$I(0)=X_t$$。

I模型有什么用呢？我们观察一下I(1)：

$$(1-L) X_t = X_t - X_{t-1} = \Delta X$$

有的时候，虽然I(0)不是平稳序列，但I(1)是平稳序列，这时我们称该序列是**1阶平稳序列**。n阶的情况，可依此类推。

### ARIMA模型

$$Y_t = (1-L)^d X_t$$

$$\left( 1 - \sum_{i=1}^p \phi_i L^i \right) Y_t = \left( 1 + \sum_{i=1}^q \theta_i L^i \right) \varepsilon_t$$

从上式可以看出，ARIMA模型实际上就是利用I模型，将时间序列转化为平稳序列之后的ARMA模型。

>注：上面的内容只是对ARIMA模型给出一个简单的定义。实际的假设检验、参数估计的步骤，还是比较复杂的，完全可以写本书来说。

参考：

https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average

https://en.wikipedia.org/wiki/Autoregressive%E2%80%93moving-average_model

https://zhuanlan.zhihu.com/p/23534595

时间序列分析：结合ARMA的卡尔曼滤波算法（该文的参考文献中有不少好文）

http://blog.csdn.net/aliceyangxi1987/article/details/71079522

用ARIMA模型做需求预测

# 推荐算法中的常用排序算法

## Pointwise方法

Pranking (NIPS 2002), OAP-BPM (EMCL 2003), Ranking with Large Margin Principles (NIPS 2002), Constraint Ordinal Regression (ICML 2005)。

## Pairwise方法

Learning to Retrieve Information (SCC 1995), Learning to Order Things (NIPS 1998), Ranking SVM (ICANN 1999), RankBoost (JMLR 2003), LDM (SIGIR 2005), RankNet (ICML 2005), Frank (SIGIR 2007), MHR(SIGIR 2007), Round Robin Ranking (ECML 2003), GBRank (SIGIR 2007), QBRank (NIPS 2007), MPRank (ICML 2007), IRSVM (SIGIR 2006)。

## Listwise方法

LambdaRank (NIPS 2006), AdaRank (SIGIR 2007), SVM-MAP (SIGIR 2007), SoftRank (LR4IR 2007), GPRank (LR4IR 2007), CCA (SIGIR 2007), RankCosine (IP&M 2007), ListNet (ICML 2007), ListMLE (ICML 2008) 。

# NLP机器翻译常用评价度量

机器翻译的评价指标主要有：BLEU、NIST、Rouge、METEOR等。

参考：

http://blog.csdn.net/joshuaxx316/article/details/58696552

BLEU，ROUGE，METEOR，ROUGE-浅述自然语言处理机器翻译常用评价度量

http://blog.csdn.net/guolindonggld/article/details/56966200

机器翻译评价指标之BLEU

http://blog.csdn.net/han_xiaoyang/article/details/10118517

机器翻译评估标准介绍和计算方法

http://blog.csdn.net/lcj369387335/article/details/69845385

自动文档摘要评价方法---Edmundson和ROUGE

# Tri-training

## 半监督学习

之前提到的算法，多数都属于监督学习算法。其特点在于，构建一个包含标记数据的训练集，用来训练算法模型。

然而，获得标记数据是一个费时费力的高成本过程，实际工作中，更有可能的情况是：少量标记数据+大量未标记数据。

未标记数据的处理方式，一般有如下三种：

![](/images/article/Semi_supervised_Learning.png)

1.主动学习。

2.

## 协同训练算法

http://www.cnblogs.com/liqizhou/archive/2012/05/11/2496162.html

Tri-training, 协同训练算法


