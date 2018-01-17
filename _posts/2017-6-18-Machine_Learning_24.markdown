---
layout: post
title:  机器学习（二十四）——时间序列分析, 推荐算法中的常用排序算法, Tri-training, Beam Search
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

https://mp.weixin.qq.com/s/05WAZcklXnL_hFPLZW9t7Q

时间序列模型之相空间重构模型

# 推荐算法中的常用排序算法

## Pointwise方法

Pranking (NIPS 2002), OAP-BPM (EMCL 2003), Ranking with Large Margin Principles (NIPS 2002), Constraint Ordinal Regression (ICML 2005)。

## Pairwise方法

Learning to Retrieve Information (SCC 1995), Learning to Order Things (NIPS 1998), Ranking SVM (ICANN 1999), RankBoost (JMLR 2003), LDM (SIGIR 2005), RankNet (ICML 2005), Frank (SIGIR 2007), MHR(SIGIR 2007), Round Robin Ranking (ECML 2003), GBRank (SIGIR 2007), QBRank (NIPS 2007), MPRank (ICML 2007), IRSVM (SIGIR 2006)。

## Listwise方法

LambdaRank (NIPS 2006), AdaRank (SIGIR 2007), SVM-MAP (SIGIR 2007), SoftRank (LR4IR 2007), GPRank (LR4IR 2007), CCA (SIGIR 2007), RankCosine (IP&M 2007), ListNet (ICML 2007), ListMLE (ICML 2008) 。

## 参考

https://mp.weixin.qq.com/s/YjYVE6jzySVsZmXSPivB5w

达观数据搜索引擎排序实践（上篇）

https://mp.weixin.qq.com/s/UpN7tAMjbFLSPcDYsWaykg

达观数据搜索引擎排序实践（下篇）

https://mp.weixin.qq.com/s/xigME-griWFwEvvPNqWuvg

美团点评联盟广告的场景化定向排序机制

# Tri-training

## 半监督学习

之前提到的算法，多数都属于监督学习算法。其特点在于，构建一个包含标记数据的训练集，用来训练算法模型。

然而，获得标记数据是一个费时费力的高成本过程，实际工作中，更有可能的情况是：少量标记数据+大量未标记数据。

未标记数据的处理方式，一般有如下三种：

![](/images/article/Semi_supervised_Learning.png)

### 主动学习

1.根据标记数据生成一个简单的模型A。

2.挑出对改善模型性能帮助最大的样本数据B。

3.通过查询行业专家获得B的真实标记。

4.根据B的真实标记，更新模型A。

以SVM为例，对于改善模型性能帮助最大的样本往往是位于分类边界的样本，可将这些样本挑出来，查询它的标记。

### 纯半监督学习和推断学习

纯半监督学习和推断学习，都属于广义上的半监督学习。和主动学习不同，半监督学习无需专家提供的外部信息。

但标记数据总归不能无中生有，半监督学习的实现有赖于若干假设，其中主要有聚类假设和流形假设两种。其本质都是“相似的样本拥有相似的输出”。

纯半监督学习假定未标记数据为训练样本集，而推断学习则认为未标记数据为测试样本集。

虽然多数情况下，半监督学习能有效提升模型的泛化性能，然而这并不是绝对的。当半监督学习之后，模型的泛化性能反而下降时，我们首先需要检查数据是否满足算法所依赖的假设。

参考：

http://www.cnblogs.com/chaosimple/p/3147974.html

半监督学习

## 协同训练算法

协同训练（co-training）算法是一种多学习器的半监督学习算法。它是多视图（multi-view）学习的代表。

以电影为例，它拥有多个属性集：图像、声音、字幕等。每个属性集就构成了一个视图。

协同训练假设不同的视图具有“相容性”。所谓相容性是指，如果一个电影从图像判断，像是动作片，那么从声音判断，也有很大可能是动作片。

协同训练的算法流程：

1.在每个视图上，基于有标记数据，分别训练出一个分类器。

2.每个分类器挑选自己最有把握的未标记数据，赋予伪标记。

3.其他分类器利用伪标记，进一步训练模型，最终达到相互促进的目的。

理论上，如果不同视图之间具有完全相容性，则模型准确度可达到任意上限。而实际中，虽然很难满足完全相容性假设，但该算法仍能有效提升模型的准确度。

原始的协同训练算法的主要局限在于，构造视图在工程上是件很有难度的事情。后续的一些改进算法针对这点，在应用范围上做了不少改进。如基于不同学习算法、不同的数据采样、不同的参数设置的协同训练算法。

理论研究表明，只要弱学习器之间具有显著分歧（或差异），即可通过相互提供伪标记的方式提升泛化性能。因此，协同训练算法又被称为**基于分歧的算法**。

## Tri-training

2005年，周志华提出了Tri-training算法。该算法的流程如下：

1.首先对有标记示例集进行可重复取样(bootstrap sampling)以获得三个有标记训练集，然后从每个训练集产生一个分类器。

2.在协同训练过程中,各分类器所获得的新标记示例都由其余两个分类器协作提供，具体来说，如果两个分类器对同一个未标记示例的预测相同，则该示例就被认为具有较高的标记置信度，并在标记后被加入第三个分类器的有标记训练集。

参考：

http://www.cnblogs.com/liqizhou/archive/2012/05/11/2496162.html

Tri-training, 协同训练算法

## Co-Forest & Co-Trade

Co-Forest & Co-Trade是周志华在Tri-training基础上的改进算法。

参考：

http://lamda.nju.edu.cn/huangsj/dm11/files/gaoy.pdf

半监督学习中的几种协同训练算法

# Beam Search

Beam Search（集束搜索）是一种启发式图搜索算法，通常用在图的解空间比较大的情况下，为了减少搜索所占用的空间和时间，在每一步深度扩展的时候，剪掉一些质量比较差的结点，保留下一些质量较高的结点。

这样减少了空间消耗，并提高了时间效率，但缺点就是有可能存在潜在的最佳方案被丢弃，因此Beam Search算法是不完全的，一般用于解空间较大的系统中。

![](/images/article/beam_search.png)

上图是一个Beam Search的剪枝示意图。

Beam Search主要用于机器翻译、语音识别等系统。这类系统虽然从理论来说，也就是个多分类系统，然而由于分类数等于词汇数，简单的套用softmax之类的多分类方案，明显是计算量过于巨大了。

PS：中文验证码识别估计也可以采用该技术。

参见：

http://people.csail.mit.edu/srush/optbeam.pdf

Optimal Beam Search for Machine Translation

http://www.cnblogs.com/xxey/p/4277181.html

Beam Search（集束搜索/束搜索）

http://blog.csdn.net/girlhpp/article/details/19400731

束搜索算法（Andrew Jungwirth 初稿）BEAM Search


