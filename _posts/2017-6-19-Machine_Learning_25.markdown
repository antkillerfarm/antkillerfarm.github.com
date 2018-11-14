---
layout: post
title:  机器学习（二十五）——Tri-training, 聚类算法, 模仿学习, 强化学习
category: ML 
---

# 时间序列分析（续）

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

https://zhuanlan.zhihu.com/p/34407471

如何理解时间序列？—从Riemann积分和Lebesgue积分谈起

https://zhuanlan.zhihu.com/p/35093835

时间序列的自回归模型—从线性代数的角度来看

https://mp.weixin.qq.com/s/lmJk-iIzxxPmnZa6D8i_nw

一文简述如何使用嵌套交叉验证方法处理时序数据

https://zhuanlan.zhihu.com/p/39105270

时间序列的表示与信息提取

https://mp.weixin.qq.com/s/iah8PvIC0oZngSaNHw7gJw

从上帝视角看透时间序列和数据挖掘

https://zhuanlan.zhihu.com/p/38130622

时间序列的相似性

https://mp.weixin.qq.com/s/p8oN4xh-FHnay2eTsk6Gng

基于高阶模糊认知图与小波变换的时间序列预测

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

# 聚类算法

https://mp.weixin.qq.com/s/xGPiaXTnQad3RcMwIELP4w

流式聚类算法

https://wenku.baidu.com/view/4169e77f27284b73f2425047.html

层次聚类

https://mp.weixin.qq.com/s/uSHLJKB0knVcCY759Ul25w

什么是聚类和降维？

https://mp.weixin.qq.com/s/ORLOOhufrInyPdS6fbywOw

深入浅出——基于密度的聚类方法

https://mp.weixin.qq.com/s/Vi4Yb8TOJydj9yL078iNHw

BIRCH层次聚类详解

https://mp.weixin.qq.com/s/dIsq3RKAU_K6vS0-MPKbzA

BIRCH聚类算法原理

https://mp.weixin.qq.com/s/_5A3DuVyN6aE9n5OEc19kA

数据科学家必须了解的六大聚类算法：带你发现数据之美

https://mp.weixin.qq.com/s/pDiZt4ydWw-4cYeE4gpjiw

数据科学家需要了解的5大聚类算法

https://www.zhihu.com/question/34554321

用于数据挖掘的聚类算法有哪些，各有何优势？

https://mp.weixin.qq.com/s/7zV370J7nv5wSWxdYa5Plg

DBSCAN聚类算法原理介绍，以及代码实现

https://mp.weixin.qq.com/s/8dB2OQCoZ7_kSxExm32WSA

于剑：聚类理论与算法选讲

https://mp.weixin.qq.com/s/1SOQZ3fsiYtT4emt4jvMxQ

深入浅出聚类算法

https://mp.weixin.qq.com/s/GlwJhvdzxaRbRE2DhMpBSg

基于动态局部搜索的免疫自动聚类算法

# 模仿学习

https://zhuanlan.zhihu.com/p/27935902

机器人学习Robot Learning之模仿学习Imitation Learning的发展

https://zhuanlan.zhihu.com/p/25688750

模仿学习（Imitation Learning）完全介绍

https://mp.weixin.qq.com/s/naq73D27vsCOUBperKto8A

从监督式到DAgger，综述论文描绘模仿学习全貌

https://mp.weixin.qq.com/s/LNNqp2KsEAljG26hY43mUw

ICML2018 模仿学习教程

# 强化学习

## 监督学习的局限

举个例子，如果我们想让机器学会开车，一个很直接的想法是观察人类行为，并且模仿人类，在相应观测下做出人类所做行为。这种方法也叫作模仿学习（Imitation Learning）。

将这个想法实现起来也很简单，只需要收集该任务的一些观测（路面的画面），以及每个观测人类会做出的反应（转动方向盘），然后像监督学习一样训练一个神经网络，以观测为输入，人类行为为标签，其中行为是离散时是分类任务，连续时是回归任务。

然而这简单的监督学习理论上并不可行，一个直观的原因是由于现实的随机性或者复杂性，使得机器所采用的动作和人类的动作有偏差或者动作所产生的结果有偏差，这样在有偏差的下一状态，机器还会做出有偏差的动作，使得之后状态的偏差积累，导致机器遇到监督学习时没有碰到过的状态，那机器就完全不知道该怎么做了，也就是如下图所示：

![](/images/img2/RL.png)

即使我们可以使用一些算法来改善模仿学习的效果，但模仿学习始终有如下问题：

1.需要人类提供的大量数据（尤其是深度学习，需要大量样本）。

2.人类对一些任务也做的不太好，对于一些复杂任务，人类能做出的动作有限。

3.我们希望机器能自动学习，即能不断地在错误中自我完善，而不需要人类的指导。

## 概述

强化学习是一个多学科交叉的领域。它的主要组成以及和其他学科的关系如下图所示：

![](/images/article/RL_2.png)

![](/images/article/RL.png)

上图是Reinforcement Learning和其他类型算法的关系图。

![](/images/img2/RL_2.png)

不像监督学习，对于每一个样本，都有一个确定的标签与之对应，而强化学习没有标签，只有一个时间延迟的奖励，而且游戏中我们往往牺牲当前的奖励来获取将来更大的奖励。这就是**信用分配问题（Credit Assignment Problem）**，即当前的动作要为将来获得更多的奖励负责。

而且在我们找到一个策略，让游戏获得不错的奖励时，我们是选择继续坚持当前的策略，还是探索新的策略以求更多的奖励？这就是**探索与开发（Explore-exploit Dilemma）**的问题。

因此，**强化学习某种意义上可看做具有延迟标记信息的监督学习**。

