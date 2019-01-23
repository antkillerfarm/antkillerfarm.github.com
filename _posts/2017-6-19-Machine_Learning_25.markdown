---
layout: post
title:  机器学习（二十五）——Tri-training, 聚类算法, 压缩感知, 元胞自动机, 线性规划
category: ML 
---

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

http://www.cnblogs.com/pinard/p/6221564.html

谱聚类（spectral clustering）原理总结

# 压缩感知

http://blog.csdn.net/abcjennifer/article/details/7721834

初识压缩感知Compressive Sensing

http://blog.csdn.net/abcjennifer/article/details/7724360

中国压缩传感资源（China Compressive Sensing Resources）

http://blog.csdn.net/xiahouzuoxin/article/details/38820925

白话压缩感知（含Matlab代码）

http://blog.csdn.net/abcjennifer/article/details/7748833

压缩感知进阶——有关稀疏矩阵

# 元胞自动机

http://www.doc88.com/p-8939046003005.html

元胞自动机理论及其在计算机仿真模拟中的应用

http://blog.csdn.net/chl033/article/details/5367861

元胞自动机与相关理论和方法

http://blog.csdn.net/deepfuture/article/details/5065284

元胞自动机

http://www.cnblogs.com/Firefly727/articles/1856328.html

元胞自动机

# 线性规划

http://www.cnblogs.com/6DAN_HUST/archive/2010/11/11/1874681.html

运筹学——线性规划及单纯形法求解

https://mp.weixin.qq.com/s/2qOjE0B6x9xuyoBw94NqQA

线性规划基础

https://mp.weixin.qq.com/s/zV6zi79c1Q2dfCaywuK6Pw

内点法六十年再回首

https://mp.weixin.qq.com/s/aryMyP0r6vov0pUvkoFYng

过去，现在和未来：运筹学为航空业保驾护航

https://mp.weixin.qq.com/s?__biz=MzUxMTYwMzI0OQ==&mid=2247486937&idx=1&sn=6be69679390f59516ee5f077adb8ccfa

运筹优化的剖析与应用

# 金融模型

Capital asset pricing model

Fama–French three-factor model

Carhart four-factor model

# DL参考资源+

https://mp.weixin.qq.com/s/gPI6Vq6smDn8IiKhGe61bA

用深度学习DIY自动化监控系统

https://mp.weixin.qq.com/s/JkYFx4tN2x8y5tYPyonE9g

建模任务相关注视点转移，实现第一人称视频注视点的准确估计

https://mp.weixin.qq.com/s/OCSe_o6bJjOYNEpYaOQ_Cg

最大化互信息来学习深度表示，Bengio等提出Deep INFOMAX

https://mp.weixin.qq.com/s/VvU8OfXKJiyEKaujWfCrWw

上海交通大学ECCV 2018四篇入选论文解读

https://mp.weixin.qq.com/s/_IkE5NPYjpT2QGVo9mfWvA

MIT&谷歌视频运动放大让计算机辅助人眼“明察秋毫” 

https://mp.weixin.qq.com/s/yOuK39WdkfvyV36c1_SOnQ

社会媒体数据挖掘与信息传播预测

https://mp.weixin.qq.com/s/jMeTSr1t2Uc-qibNom4AZg

LeCun提出错误编码网络，可在不确定环境中执行时间预测

https://mp.weixin.qq.com/s/LQ3tn4Zxg5obDD_oyEugKQ

为数据集自动生成神经网络：普林斯顿大学提出NeST

https://mp.weixin.qq.com/s/LUAcYoxoYBcDzTqi5bVJTQ

非局部神经网络，打造未来神经网络基本组件

https://mp.weixin.qq.com/s/lrl4si8Aeh7Pl76leBSmeA

Nature论文解读：用于改善加权生物网络信噪比的网络增强方法

https://mp.weixin.qq.com/s/0AOOiyWQqpGIdG4-9UmF8Q

DeepMind提出视觉问题回答新模型，CLEVR准确率达98.8％

https://mp.weixin.qq.com/s/8dVronQoeZLJI2g49AljLg

技术讲解概率机器学习——深度学习革命之后AI道路

https://mp.weixin.qq.com/s/qof4GEPQYb9rO3Do_oL7BQ

如果给猫披上象皮，神经网络将作何判断？

https://mp.weixin.qq.com/s/l1Bc5l5cr3y9jQbh-bEQeQ

Deep Learning of Graph Matching论文解读

https://mp.weixin.qq.com/s/494I6XHrk9fB2DvhiMM9iQ

第四范式联合港科大提出样本自适应在线卷积稀疏编码

https://mp.weixin.qq.com/s/p8u2aTJL2gfPIXLsLfoJyg

谷歌大脑提出MAPO：用于程序合成的策略优化方法

https://mp.weixin.qq.com/s/zC5iGXVn7xH8TYBXtkntzA

让计算机一眼认出“猫”：哈佛提出新高维数据分析法

http://mp.weixin.qq.com/s/vhBOrR6uTL2vGXnYC8BS1w

Windows版深度学习软件安装指南

https://mp.weixin.qq.com/s/FwqKYNpIuB6cUAVjSey7mw

Yoshua Bengio首次中国演讲：深度学习通往人类水平AI的挑战

https://mp.weixin.qq.com/s/aqfwn0kiXbZwVDVhHXBAXQ

谷歌大脑研究员玩转汉字RNN：神经网络生成新华字典

https://mp.weixin.qq.com/s/GxWOuIf25JQnQoCstVyGLQ

李飞飞团队提出OpenTag模型：减少人工标注，自动提取产品属性值

https://mp.weixin.qq.com/s/qos7VRFP7uYZ6Qt83KiPhw

用机器学习构建O(N)复杂度的排序算法，可在GPU和TPU上加速计算

https://mp.weixin.qq.com/s/_w4IIRY6LwqehpDlEXt90A

牛津大学提出神经网络新训练法：用低秩结构增强网络压缩和对抗稳健性

https://mp.weixin.qq.com/s/w-6z8B60cIHUF5tU-RoQ3w

旷视科技提出统一感知解析网络UPerNet，优化场景理解

https://mp.weixin.qq.com/s/Jty0eHlQ7gmihKkdp76XyQ

北大：一种基于新闻特征抽取和循环神经网络的股票预测方法

https://mp.weixin.qq.com/s/_QPzoFyhCxnIxDWKzzsvAQ

智能交通大数据最新论文综述

https://mp.weixin.qq.com/s/eYTDwSqgj-r7yV7w9tY_MA

融合质量不理想数据

https://mp.weixin.qq.com/s/tduZoZecbOM0gC9Ugerbwg

用AI设计微波集成电路，清华大学等提出深度强化学习方法RINN
