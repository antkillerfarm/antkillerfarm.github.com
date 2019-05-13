---
layout: post
title:  机器学习（二十五）——Tri-training, 聚类算法, 压缩感知, 元胞自动机, 运筹学, simhash
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

## 谱聚类

http://www.cnblogs.com/pinard/p/6221564.html

谱聚类（spectral clustering）原理总结

https://mp.weixin.qq.com/s/DrD7aONVfN3Ibx4x6z-e3Q

理解谱聚类

## 参考

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

https://mp.weixin.qq.com/s/IS4eXECKx2u41VFtmTsBhA

全面理解无监督学习基础知识

https://mp.weixin.qq.com/s/P9bxQobyEvMHY8umGkAz8Q

无监督机器学习中，最常见4类聚类算法总结

https://mp.weixin.qq.com/s/qTZZVBlZF7_oR8QFGq6hmQ

Fuzzy c-means聚类算法简介

# 压缩感知

https://blog.csdn.net/jbb0523

一个压缩感知+贝叶斯网络方面的blog

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

# 运筹学

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

https://mp.weixin.qq.com/s/Ofn-p8NnnO4kLYqTlL0bJg

二次规划（QP）样条路径

https://mp.weixin.qq.com/s/zqesjLOeWIkiq68tLVVmJQ

混合整数规划模型在页岩气开采中的应用：EQT公司的案例

https://mp.weixin.qq.com/s/Ve_Gvp1Y0nIX4DV9_w0S3g

半正定规划(SDP)的形象理解和基本原理

https://mp.weixin.qq.com/s/wspfngdFNq-GeCTUJAse0A

在单纯形法之前

https://mp.weixin.qq.com/s/sGRNiAl7tvPc1tl0Xk2Idw

深度学习和强化学习在组合优化方面有哪些应用？

# 金融模型

Capital asset pricing model

Fama–French three-factor model

Carhart four-factor model

# simhash

simhash是一种能计算文档相似度的hash算法。

论文：

《Detecting Near-Duplicates for Web Crawling》

代码：

https://github.com/yanyiwu/simhash

类似的相似度Hash算法，还有用于图像搜索的aHash、pHash、dHash。

参考：

https://www.biaodianfu.com/simhash.html

文本内容相似度计算方法：simhash

http://www.cnblogs.com/chenwenbiao/archive/2011/09/12/2174139.html

simhash算法的原理

http://www.cnblogs.com/chenwenbiao/archive/2011/09/12/2174137.html

simhash与Google的网页去重

http://blog.jobbole.com/21928/

Simhash算法原理和网页查重应用

https://blog.csdn.net/luoweifu/article/details/8220992

看起来像它——图像搜索其实也不难

https://www.jianshu.com/p/193f0089b7a2

相似图片检测：感知哈希算法之dHash的Python实现

https://mp.weixin.qq.com/s/vwhetMpQllczILptBNcoWg

基于快速GeoHash，如何实现海量商品与商圈的高效匹配？

# TensorFlow+

https://mp.weixin.qq.com/s/nnjyR4XGVZQ1zXCIPzTNlg

基于TensorFlow的变分自编码器实现

https://mp.weixin.qq.com/s/iMgesGmdb7Jq4muCxb-nFA

Tensorflow实战：Discuz验证码识别

https://mp.weixin.qq.com/s/4aJUGBpPG_6Oc5EqOmM0Iw

作为TensorFlow的底层语言，你会用C++构建深度神经网络吗？

https://mp.weixin.qq.com/s/5_oCSK6MtsK_4xNU6T76UA

计算机图形学遇上深度学习，针对3D图像的TensorFlow Graphics面世

https://mp.weixin.qq.com/s/ndnbTCr51k_FSDRCLi0UOg

添加新操作

# 知识图谱参考资源+

https://mp.weixin.qq.com/s/7LFkOzkD1-PsNNAr5DAETg

TechKG：一个面向中文学术领域的大型知识图谱

https://mp.weixin.qq.com/s/QNTJngwEDtxufEpIGTngCw

事理图谱，下一代知识图谱

https://mp.weixin.qq.com/s/0bTmzKiC7WDbLRcxfAAGqw

提取计数量词丰富知识库

https://mp.weixin.qq.com/s/Pqm5uhC9iMLuPk6mDCtPuQ

动态知识图谱补全论文合集

https://zhuanlan.zhihu.com/p/51000072

基于知识图谱路径推理的可解释推荐

https://mp.weixin.qq.com/s/qj5sFIqCUkW38XQ6hRao1w

KG Embedding with Iterative Guidance from Soft Rules

https://mp.weixin.qq.com/s/PrugbfsWIt0LWGJRw_9i1Q

基于知识库的类型实体和关系的联合抽取

https://mp.weixin.qq.com/s/_pNDalO6D8kGiCBYi1jnIA

对于知识图谱嵌入表示的几何形状理解

https://mp.weixin.qq.com/s/3QjLFzMvH9qR05kmUyawQg

知识图谱如何用于可解释人工智能中？
