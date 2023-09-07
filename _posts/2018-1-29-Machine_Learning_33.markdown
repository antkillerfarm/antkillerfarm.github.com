---
layout: post
title:  机器学习（三十三）——机器学习的算法体系&相关术语表, ACBM算法, 高斯过程回归, Parameter Server
category: ML 
---

* toc
{:toc}

# t-SNE

## t-SNE（续）

![](/images/img2/t-SNE.png)

![](/images/img2/Sammon.png)

上图分别是使用t-SNE和Sammon mapping可视化MNIST数据集后的效果图。从中可以看出t-SNE图中，数据更成团状，可视化效果更好。

t-SNE的不足主要有四个:

>主要用于可视化，很难用于其他目的。比如测试集合降维，因为他没有显式的预估部分，不能在测试集合直接降维；比如降维到10维，因为t分布偏重长尾，1个自由度的t分布很难保存好局部特征，可能需要设置成更高的自由度。

>t-SNE倾向于保存局部特征，对于本征维数(intrinsic dimensionality)本身就很高的数据集，是不可能完整的映射到2-3维的空间

>t-SNE没有唯一最优解，且没有预估部分。如果想要做预估，可以考虑降维之后，再构建一个回归方程之类的模型去做。但是要注意，t-sne中距离本身是没有意义，都是概率分布问题。

>训练太慢。有很多基于树的算法在t-sne上做一些改进。

## FastTSNE

该工具包提供了两种快速实现tSNE的方法：

Barnes-hut tsne：源于Multicore tSNE，适用于小规模数据集，时间复杂度为O(nlogn)。

Fit-SNE：源于Fit-SNE的C++实现方法，适用于样本量在10,000以上的大规模数据集，时间复杂度为O(n)。

代码：

https://github.com/pavlin-policar/fastTSNE

## 参考

https://www.zhihu.com/question/52022955

t-sne数据可视化算法的作用是啥？为了降维还是认识数据？

https://mp.weixin.qq.com/s/Rs9ri6Xs5R-yitrda8pJMg

详解可视化利器t-SNE算法：数无形时少直觉

https://mp.weixin.qq.com/s/_DXMlNZHVKm2jMnLGQFM_Q

还在用PCA降维？快学学大牛最爱的t-SNE算法吧

http://www.datakit.cn/blog/2017/02/05/t_sne_full.html

t-SNE完整笔记

https://yq.aliyun.com/articles/70733

比PCA降维更高级——（R/Python）t-SNE聚类算法实践指南

https://mp.weixin.qq.com/s/7Vy7l1YyBT7rMYW2i1AsuA

线性判别分析(LDA)原理详解

https://mp.weixin.qq.com/s/cnzQ7XepftDOZXslCf1MUA

你真的会用t-SNE么？有关t-SNE的小技巧

https://mp.weixin.qq.com/s/lbpe2NO1m8S38wpnp47BEg

通过可视化隐藏表示，更好地理解神经网络

https://mp.weixin.qq.com/s/F08aOjKsVdRInN6GPNJ7cA

t-SNE：最好的降维方法之一

https://mp.weixin.qq.com/s/qHOmUJgalvxwEBCBiiR7ig

t-SNE

https://mp.weixin.qq.com/s/EaeVywxcQX3goW7WqYiOBA

探究Softmax的替代品：exp(x)的偶次泰勒展开式总是正的

https://mp.weixin.qq.com/s/GEagy8-BwWcuWgzG9e8XbA

t-SNE：可视化效果最好的降维算法

# 机器学习的算法体系&相关术语表

## 算法体系

![](/images/article/ML_2.png)

原文：

https://mp.weixin.qq.com/s/HapJwwmN3-dbQvzp2jzt1w

一文看懂机器学习的算法体系

## 相关术语表

https://mp.weixin.qq.com/s/6KRNawRE1i8de3ctgv6aVg

机器学习词汇表：纵览机器学习基本词汇与概念

https://mp.weixin.qq.com/s/AtImcIBaRIJVXpOP2hruBQ

Google发布机器学习术语表 (包括简体中文)

https://www.zhihu.com/question/469612040

刚进算法团队，大牛们讨论高深的cv术语和算法，如何才能听懂？

# 高斯过程回归

从大的分类来说，机器学习的算法可分为两类：

1.定义一个模型，用训练数据训练模型的参数，然后用训练好的模型进行预测。这种方法的缺点在于，预测效果和模型与样本的匹配程度有关。比如对非线性样本采用线性模型，其预测效果通常不会太好。但是增加模型的复杂度，又会导致过拟合。

2.定义一个函数分布，赋予每一种可能的函数一个先验概率，可能性越大的函数，其先验概率越大。但是可能的函数往往为一个不可数集，即有无限个可能的函数，随之引入一个新的问题：如何在有限的时间内对这些无限的函数进行选择？一种有效解决方法就是高斯过程回归(Gaussian process regression，GPR)。

>注：Radford M. Neal，1956年生，加拿大科学家。多伦多大学博士（1995）和教授。贝叶斯神经网络的发明人。导师为Geoffrey Hinton。   
>个人主页：http://www.cs.toronto.edu/~radford/

>Danie G. Krige，1919～2013，南非矿业工程师和统计学家，威特沃特斯兰德大学教授。地理统计学早期的代表人物之一。

http://www.cnblogs.com/hxsyl/p/5229746.html

浅谈高斯过程回归

https://mqshen.gitbooks.io/prml/content/Chapter6/gaussian/gaussian_processes_regression.html

Gaussian Processes Regression

http://www.gaussianprocess.org/gpml/chapters/RW.pdf

Gaussian Processes for Machine Learning

http://people.cs.umass.edu/~wallach/talks/gp_intro.pdf

Introduction to Gaussian Process Regression

http://wenku.baidu.com/view/72f80113915f804d2b16c173.html

高斯过程回归方法综述

https://mp.weixin.qq.com/s/ZE_Chzlgy_7wl9E9q1ckOA

AlphaGo Zero用它来调参？“高斯过程”到底有何过人之处？

https://mp.weixin.qq.com/s/VzN02XW3yN2peqTD6q-4Cg

从数学到实现，全面回顾高斯过程中的函数最优化

https://zhuanlan.zhihu.com/gpml2016

高斯世界下的Machine Learning

https://mp.weixin.qq.com/s/FJAgpbBgRA2Zk3BuiEwjdw

看得见的高斯过程：这是一份直观的入门解读

https://zhuanlan.zhihu.com/p/58987388

多元高斯分布完全解析

https://zhuanlan.zhihu.com/p/73832253

通俗理解高斯过程及其应用

https://zhuanlan.zhihu.com/p/75072046

Gaussian process classification初介绍——回归与分类点点滴滴

https://zhuanlan.zhihu.com/p/75589452

高斯过程Gaussian Processes原理、可视化及代码实现

https://mp.weixin.qq.com/s/Bw0vD9EEQijjE1gaIAetHw

如何推导高斯过程回归以及深层高斯过程详解

https://mp.weixin.qq.com/s/1HggYumIF2pAomCtA-eu6g

如何知道一个变量的分布是否为高斯分布?

# 热传导推荐算法

https://www.zhihu.com/question/20184666

推荐系统中用到的热传导算法和物质扩散是怎么用的？

http://tis.hrbeu.edu.cn/oa/pdfdow.aspx?Sid=20160307

基于影响力控制的热传导算法

http://www.doc88.com/p-7082821463697.html

改进的热传导和物质扩散混合推荐算法

# 并行 & 框架 & 优化+

https://mp.weixin.qq.com/s/ai_XI8ddP5I2m3ChCqnQsA

高效大规模机器学习训练，198页PDF带你概览领域前沿进展

https://openmlsys.github.io

机器学习系统：设计和实现

https://mp.weixin.qq.com/s/RAjusu-Jyqb8K19N8KZ_3w

一份552页《大规模数据系统：Large-scale Data Systems》硬核课程PPT

https://mp.weixin.qq.com/s/AeCQK2hFy60pq6y1tRcs_A

20页pdf，A Survey on Large-scale Machine

https://mp.weixin.qq.com/s/_1Yr_BbFhlNEW7UtYvAaoA

分布式深度学习，93页ppt概述最新DDL技术发展

https://mp.weixin.qq.com/s/jC5v9BKQvlxa2_6cikXV9w

分布式算法与优化，118页pdf

https://zhuanlan.zhihu.com/p/58806183

深度学习的分布和并行处理系统

https://zhuanlan.zhihu.com/p/56991108

一文说清楚Tensorflow分布式训练必备知识

https://zhuanlan.zhihu.com/p/26552293

Dataflow架构和神经网络加速器

https://zhuanlan.zhihu.com/p/28445511

浅析深度学习框架设计中的关键技术

https://mp.weixin.qq.com/s/wu32LBwrkkBIANMdknHlCA

C++并行实战，592页pdf，C++ Concurrency in Action

https://zhuanlan.zhihu.com/p/79385727

有限元并行计算简介

https://mp.weixin.qq.com/s/heVQ9AIZKxTiCNiAtYKaag

新加坡国立大学最新“大规模深度学习优化”综述论文，带你全面了解最新深度学习准确率和效率的优化方法

https://mp.weixin.qq.com/s/B4aQp_0YvS0jyUHNLQ5rRA

IBM发布新型分布式深度学习系统：结合软硬件实现当前最优性能

http://engineering.skymind.io/distributed-deep-learning-part-1-an-introduction-to-distributed-training-of-neural-networks

神经网络的分布式训练

https://mp.weixin.qq.com/s/nvuflLfOolidDDXJVe2DZA

美团深度学习系统的工程实践

https://mp.weixin.qq.com/s/IE6blClvhYlq3-QAGHo5ww

TensorFlow分布式计算机制解读：以数据并行为重

https://mp.weixin.qq.com/s/4Ii3um3jqfm5yKKxZAFdmA

继1小时训练ImageNet之后，大批量训练扩展到了3万2千个样本

https://mp.weixin.qq.com/s/kOCftzSbHe2mvDmlRp-ihA

Jeff Dean：AI对计算机系统设计的影响

https://mp.weixin.qq.com/s/XjNPaL6PC9LHX1PEGn5UZg

微软实时AI系统“脑波计划”有多牛？看完秒懂！

https://mp.weixin.qq.com/s/OkqUulFYHQSdgAbf9Fi9LA

CoCoA：大规模机器学习的分布式优化通用框架

https://mp.weixin.qq.com/s/ToIDncp9dS_qk47PsdZm5A

杜克大学：分布式深度学习训练算法TernGrad

https://mp.weixin.qq.com/s/rhtrN2qDspGkpJYDAVSX7w

UC Berkeley展示全新并行处理方法

https://mp.weixin.qq.com/s/ASqpPSIgW_bcFPBfRYz7Xg

哈佛大学提出在云、边缘与终端设备上的分布式深度神经网络DDNN

http://blog.sina.com.cn/s/blog_81f72ca70101kuk9.html

《Large Scale Distributed Deep Networks》中译文

https://mp.weixin.qq.com/s/X7XG51yohLnEZ_Jg6XK9oQ

Caffe作者贾扬清教你怎样打造更加优秀的深度学习架构

https://zhuanlan.zhihu.com/p/529388795

训练千亿参数大模型，离不开四种GPU并行策略

https://mp.weixin.qq.com/s/_mrYI7McMBUx0lEh4rNiYQ

百度开源移动端深度学习框架MDL，手机部署CNN支持iOS GPU

https://mp.weixin.qq.com/s/ZCNSq5FC2REoVTKAK2mJQg

分布式深度学习原理、算法详细介绍

https://mp.weixin.qq.com/s/Ewiil56vMkzhO2xDWgo-Wg

苹果发布Turi Create机器学习框架，5行代码开发图像识别

https://mp.weixin.qq.com/s/jOVUPhrCBI9W9vPvD9eKYg

UC Berkeley提出新型分布式框架Ray：实时动态学习的开端

https://mp.weixin.qq.com/s/r951Iasr4dke6MPHsUO0TA

开源DAWN，Stanford的又一力作

https://mp.weixin.qq.com/s/2jrMDeMcb47zpPfFLEcnIA

深度学习平台技术演进

https://mp.weixin.qq.com/s/L4CMKS53pNyvhhqvQhja0g

5种商业AI产品的技术架构设计

https://mp.weixin.qq.com/s/IqjKdAlGYREqCR9XQB5N1A

伯克利AI分布式框架Ray，兼容TensorFlow、PyTorch与MXNet

https://mp.weixin.qq.com/s/aNX_8UDYI_0u-MwMTYeqdQ

开发易、通用难，深度学习框架何时才能飞入寻常百姓家？

https://mp.weixin.qq.com/s/UbAHB-uEIvqYZCB7xIAJTg

机器学习新框架Propel：使用JavaScript做可微分编程

https://mp.weixin.qq.com/s/Ctl65r4iZNEOBxiiX2I2eQ

Momenta王晋玮：让深度学习更高效运行的两个视角
