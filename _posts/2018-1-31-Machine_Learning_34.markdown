---
layout: post
title:  机器学习（三十四）——概率图模型, 机器学习语录, CRF
category: ML 
---

# 概率图模型

## 资料

probabilistic graphical model（PGM）最早由Judea Pearl发明。

这方面比较重要的文章和书籍有：

http://www.cis.upenn.edu/~mkearns/papers/barbados/jordan-tut.pdf

Michael Irwin Jordan著。

《Probabilistic Graphical Models: Principles and Techniques》，Daphne Koller，Nir Friedman著（2009年）。

>注：Judea Pearl，1936年生，以色列-美国计算机科学家，UCLA教授。2011年获得图灵奖。

>Michael Irwin Jordan，1956年生，美国计算机科学家。UCSD博士，先后执教于MIT和UCB。吴恩达的导师。

>Daphne Koller，女，1968年生，以色列-美国计算机科学家。斯坦福大学博士及教授。和吴恩达共同创立在线教育平台Coursera。

>Nir Friedman，1967年生，以色列计算机科学家。斯坦福大学博士，耶路撒冷希伯来大学教授。

http://www.cs.cmu.edu/~epxing/Class/10708-14/lectures/

CMU的邢波（Eric Xing）所开的概率图模型课程。

## 概述

概率图模型的三要素：Graph：$$\mathcal{G}$$、Model：$$\mathcal{M}$$和Data：$$\mathcal{D}\equiv\{X^{(i)}_1,\dots,X^{(i)}_m\}^N_{i=1}$$。

它要解决的三大问题：

1.**表示**。如何获取或定义真实世界的不确定度？如何对领域知识/假设/约束编码？

2.**推断**。根据模型/数据，推断答案。

$$\text{e.g.}:P(x_i\mid \mathcal{D})$$

3.**学习**。根据数据确定哪个模型是正确的。

$$\text{e.g.}:\mathcal{M}=\arg\max_{\mathcal{M}\in M}F(\mathcal{D};\mathcal{M})$$

![](/images/article/PGM.png)

上图是PGM的一个示例。其中$$X_i$$表示随机变量，图中共有8个随机变量，假设它们均为二值变量，则整个状态空间共有$$2^8$$种组合。遍历这样大的状态空间无疑是一件极为费力的事情。

如果$$X_i$$是条件独立的话，则由上图可得：

$$P(X_1,\dots,X_8)=P(X_2)P(X_4\mid X_2)P(X_5\mid X_2)P(X_1)P(X_3\mid X_1)\\P(X_6\mid X_3,X_4)P(X_7\mid X_6)P(X_8\mid X_5,X_6)$$

这样，状态空间就缩小为$$2+4+4+2+4+8+4+8=36$$种组合了。

根据边的类型，PGM可分为两类：

1.有向边表示变量间的**因果**关系。这样的PGM，常称为Bayesia Network（BN）或Directed Graphical Model（DGM）。

2.无向边表示变量间的**相关**关系。这样的PGM，常称为Markov Random Field（MRF）或Undirected Graphical Model（UGM）。

>注：因果关系是一种强逻辑关系，需要变量间有深刻的内在联系。而相关关系要弱的多，典型的例子就是《机器学习（十七）》中的尿布和啤酒的故事。尿布和啤酒虽然正相关，然而它们本身却没有多大的联系。

根据模型的不同，PGM又可分为生成模型（Generative Model, GM）和判别模型（Discriminative Model, DM）。两者的区别在《机器学习（二）》中已经简单提到过，这里做一个扩展。

之前提到的机器学习算法，主要是建立特征向量X和标签Y之间的联系。但是实际情况下，X中的状态不一定都能得到，因此可以根据可见性，将X分为可观测变量集合O和其他变量集合R，Y也不一定是一个标签，而可能是一个变量集合。即：

$$GM:P(Y,R,O)\to P(Y\mid O)$$

$$DM:P(Y,R\mid O)\to P(Y\mid O)$$

注意，在贝叶斯学派的观点中，模型的参数也是随机变量，因此，R在某些情况下，不仅包含不可观测的变量，也包含模型参数。

## 贝叶斯网络

贝叶斯网络是最简单的有向图模型。

首先给出几个术语的定义：

**有向无环图(Directed Acyclic Graph, DAG)**：这个术语的字面意思很清楚，不解释。

**马尔可夫毯(Markov Blanket, MB)**：有向图——结点A的父结点+A的子结点+A的子结点的其他父结点。如下图所示：

![](/images/article/Markov_blanket.png)

无向图——结点A的邻近结点。

下图是图模型的部分变种之间的关系图。

![](/images/article/Generative_Models.png)

## 参考

https://mp.weixin.qq.com/s/srLs_88QOFkBiKMiLZH9HA

想了解概率图模型？你要先理解图论的基本定义与形式

https://mp.weixin.qq.com/s/S-6Mb6zNzVPpxR8DbWdT-A

读懂概率图模型：你需要从基本概念和参数估计开始

https://mp.weixin.qq.com/s/b_0sxD0noWnuIkCxxUBlDA

终极入门：马尔可夫网络 (Markov Networks)

https://mp.weixin.qq.com/s/QvD97vfnv1x8w-e33WymRA

从贝叶斯理论到图像马尔科夫随机场

# 机器学习语录

这里收录一些网上的只言片语式的心得，以区别于一般的教程。

>首先要考虑你的数据维度是线性相关的还是非线性相关的，数据是稀疏的还是稠密的，正例反例比例是多少，数据量是否充足。数据是否具有可分类性。是否需要降维。是否有噪音，是否有异常点等，然后去选择分类策略。通常包括数据采集，预处理，分类训练，预测，后处理等过程。

## 角度值的特征化

角度值是数据分析中常见的值，然而它不是线性的，比如0度和359度之间只相差1度，然而数值上却差了359度，因此无法将角度值直接代入线性回归等模型。因为后者的loss函数是用线性的欧氏距离定义的，角度显然不满足要求。

既然角度在一维上不是线性的，那么二维呢？没错，可以采用复数坐标(x,y)来表示角度，这样角度就是线性的了。

## 数值计算中的小技巧

1.两个数值趋近于无穷小且相近的数字相除的结果应该大约为1，但却因为分母接近为0而变成无穷。这时可以直接取对数执行运算，其结果在数值上会较为稳定。

## 相关关系不等同于因果关系

我们可以通过一句调侃的话来解释：“地球变暖、地震、龙卷风，以及其他自然灾害，都和18世纪以来全球海盗数量的减少有直接关系”。这两个变量的变化有相关性，但是并不能说存在因果关系，因为往往存在第三类（甚至第4、5类）未被观察到的变量在起作用。相关关系应该看作是潜在的因果关系的一定程度的体现，但需要进一步研究。

# MEMM

Maximum Entropy Markov Model

http://www.cnblogs.com/en-heng/p/6201893.html

中文分词：最大熵马尔可夫模型MEMM

http://www.cnblogs.com/549294286/archive/2013/06/06/3121761.html

统计模型之间的比较，HMM，最大熵模型，CRF条件随机场

http://blog.csdn.net/caohao2008/article/details/4242308

HMM,MEMM,CRF模型的比较

http://blog.csdn.net/zhoubl668/article/details/7787690

标注偏置问题(Label Bias Problem)和HMM、MEMM、CRF模型比较

http://tripleday.cn/2016/07/14/hmm-memm-crf/

HMM、MEMM和CRF的学习总结

https://zhuanlan.zhihu.com/p/33397147

概率图模型体系：HMM、MEMM、CRF

# CRF

条件随机场(Conditional Random Field)由Lafferty等人于2001年提出，结合了最大熵模型和隐马尔可夫模型的特点，是一种无向图模型，近年来在分词、词性标注和命名实体识别等序列标注任务中取得了很好的效果。

https://mp.weixin.qq.com/s/heYpeVLrZBRjXtMQou2BAA

如何轻松愉快的理解条件随机场（CRF）？

http://www.chokkan.org/software/crfsuite/

CRFsuite: A fast implementation of Conditional Random Fields (CRFs)

https://github.com/scrapinghub/python-crfsuite

A python binding for crfsuite

http://taku910.github.io/crfpp/

CRF++: Yet Another CRF toolkit

https://mp.weixin.qq.com/s/1rx_R1BGRVAIDqKixNLMQA

终极入门 马尔可夫网络 (Markov Networks)——概率图模型

https://mp.weixin.qq.com/s/GXbFxlExDtjtQe-OPwfokA

一文轻松搞懂-条件随机场CRF

https://zhuanlan.zhihu.com/p/35969159

CRF条件随机场

https://mp.weixin.qq.com/s/0FIns5Xt2G1seqFbpGvzTQ

长文详解基于并行计算的条件随机场

https://blog.csdn.net/liuyuemaicha/article/details/73147548

从PGM到HMM再到CRF

https://mp.weixin.qq.com/s/4r4k6JIj4xvHHmt3QqmbuA

以RNN形式做CRF后处理—CRFasRNN

https://mp.weixin.qq.com/s/JsqhwwJ7wnNcgOuAR6ekxw

理解条件随机场

https://mp.weixin.qq.com/s/79M6ehrQTiUc0l_sO9fUqA

用于序列标注问题的条件随机场（Conditional Random Field, CRF）

https://zhuanlan.zhihu.com/p/78006020

NCRF++学习笔记

https://zhuanlan.zhihu.com/p/91031332

用腻了CRF，试试LAN吧？

https://zhuanlan.zhihu.com/p/100576406

条件随机场及Mininum Risk Training

https://www.jianshu.com/p/55755fc649b1

如何轻松愉快地理解条件随机场（CRF）？

## BiLSTM+CRF

![](/images/img2/BiLSTM_CRF.jpg)

https://mp.weixin.qq.com/s/vbBNYzKq6AnsDTy8lFsKAw

TensorFlow RNN深度学习BiLSTM+CRF实现sequence labeling序列标注

https://www.jianshu.com/p/97cb3b6db573

BiLSTM模型中CRF层的运行原理-1

https://www.jianshu.com/p/7c83478eeb56

BiLSTM模型中CRF层的运行原理-2

https://www.zhihu.com/question/62399257

如何理解LSTM后接CRF？

https://mp.weixin.qq.com/s/1FCWMRapGMXjxTLoA2fYCg

CRF和LSTM 模型在序列标注上的优劣？

https://zhuanlan.zhihu.com/p/97676647

手撕BiLSTM-CRF

https://zhuanlan.zhihu.com/p/44042528

最通俗易懂的BiLSTM-CRF模型中的CRF层介绍

# 自适应滤波器

《自适应滤波器原理（Adaptive Filter Theory）》，Simon Haykin著。

## 基本估计

三种基本的信息处理运算：

**滤波（Filter）**：利用$$[0,t]$$的数据，来估计t时刻信息的运算过程。

**平滑（Smoothing）**：利用$$[0,t]$$的数据，来估计$$t'(t'<t)$$时刻信息的运算过程。

**预测（Prediction）**：利用$$[0,t]$$的数据，来估计$$t+\tau(\tau>0)$$时刻信息的运算过程。

可见，滤波和预测是实时运算，而平滑是非实时运算。

## Optimal Wiener Filters

>Norbert Wiener，1894～1964，美国数学家，控制论之父。18岁获得Harvard博士。MIT教授。

Wiener Filter主要用于**平稳状态下的线性滤波**。该滤波器在均方误差意义上是最优的。误差信号均方值相对于线性滤波器可调参数的曲线，称为**误差性能曲面**。该曲面的极小点即为Wiener solutions。

根据滤波器的记忆能力，可将线性滤波器分为Finite Impulse Response和Infinite Impulse Response，分别代表有限记忆和无限长但衰减的记忆。

在非平稳环境下，误差性能曲面随时间变化而变化，但只要输入数据的变化与算法学习率相比是慢的，则滤波器就能跟踪住。

参考：

https://blog.csdn.net/bluecol/article/details/46242355

图像去模糊（维纳滤波）
