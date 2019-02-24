---
layout: post
title:  深度学习（二十九）——Normalization进阶（2）, Graph NN
category: DL 
---

# 花式卷积进阶

## 3D卷积（续）

https://zhuanlan.zhihu.com/p/25912625

C3D network: 用于视频特征提取的3维卷积网络

https://zhuanlan.zhihu.com/p/26350774

SCNN-用于时序动作定位的多阶段3D卷积网络

https://www.jiqizhixin.com/articles/2016-08-03

FusionNet融合三个卷积网络：识别对象从二维升级到三维

http://blog.csdn.net/zouxy09/article/details/9002508

基于3D卷积神经网络的人体行为理解

https://mp.weixin.qq.com/s/YdON6Yzddq2f_QGbQsOY8w

深度三维残差神经网络：视频理解新突破

## 参考

https://mp.weixin.qq.com/s/1gBC-bp4Q4dPr0XMYPStXA

万字长文带你看尽深度学习中的各种卷积网络

https://mp.weixin.qq.com/s/qReN6z8s45870HSMCMNatw

微软亚洲研究院：逐层集中Attention的卷积模型

http://blog.csdn.net/shuzfan/article/details/77964370

不规则卷积神经网络

https://mp.weixin.qq.com/s/rXr_XBc2Psh3NSA0pj4ptQ

常建龙：深度卷积网络中的卷积算子研究进展

# Normalization进阶

和Batch Normalization类似的概念还有Weight Normalization和Layer Normalization。

### Batch Normalization

![](/images/img2/BN.jpg)

从上图可以看出，**BN是对input tensor的每个通道进行mini-batch级别的Normalization。而LN则是对所有通道的input tensor进行Normalization**。

BN的特点：

对于batch size比较小的时候，效果非常不好，而batch size越大，那么效果则越好，因为其本质上是要通过mini-batch得到对整个数据集的无偏估计；

在训练阶段和推理阶段的计算过程是不一样的；

在CNN上表现较好，而不适用于RNN甚至LSTM。

### Layer Normalization

LN的特点：

不依赖于batch size的大小，即使对于batch size为1的在线学习，也可以完美适应；

训练阶段和推理阶段的计算过程完全一样。

适用于RNN或LSTM，而在CNN上表现一般。

### Weight Normalization

WN的公式如下：

$$w=\frac{g}{\|v\|}v$$

**WN将权重分为模和方向两个分量，并分别进行训练。**

论文：

《Weight Normalization: A Simple Reparameterization to Accelerate Training of Deep Neural Networks》

WN的特点：

计算简单，易于理解。

相比于其他两种方法，其训练起来不太稳定，非常依赖于输入数据的分布。

### Cosine Normalization

Normalization还能怎么做？

我们再来看看神经元的经典变换$$f_w(x)=w\cdot x$$。

对输入数据x的变换已经做过了，横着来是LN，纵着来是BN。

对模型参数w的变换也已经做过了，就是WN。

好像没啥可做的了。然而天才的研究员们盯上了中间的那个点，对，就是$$\cdot$$。

$$f_w(x)=\cos \theta=\frac{w\cdot x}{\|w\|\cdot\|x\|}$$

参考：

https://mp.weixin.qq.com/s/EBRYlCoj9rwf0NQY0B4nhQ

Layer Normalization原理及其TensorFlow实现

http://mlexplained.com/2018/01/10/an-intuitive-explanation-of-why-batch-normalization-really-works-normalization-in-deep-learning-part-1/

An Intuitive Explanation of Why Batch Normalization Really Works

http://mlexplained.com/2018/01/13/weight-normalization-and-layer-normalization-explained-normalization-in-deep-learning-part-2/

Weight Normalization and Layer Normalization Explained

https://mp.weixin.qq.com/s/KnmQTKneSimuOGqGSPy58w

详解深度学习中的Normalization，不只是BN（1）

https://mp.weixin.qq.com/s/nSQvjBRMaBeoOjdHbyrbuw

详解深度学习中的Normalization，不只是BN（2）

https://mp.weixin.qq.com/s/Z119_EpLKDz1TiLXGbygJQ

MIT新研究参透批归一化原理

https://mp.weixin.qq.com/s/Lp2pq95woQ5-E3RemdRnyw

动态层归一化（Dynamic Layer Normalization）

# Graph NN

https://github.com/thunlp/GNNPapers

相关论文列表

https://mp.weixin.qq.com/s/xc_TnMLs3o2LQ8eM4naZDw

AAAI2019 Tutorial《图表示学习》, 180页PPT带你从入门到精通

https://mp.weixin.qq.com/s/_aydey5ZVwrObmoFXXIYcw

Bengio等人提出图注意网络架构GAT，可处理复杂结构图

https://zhuanlan.zhihu.com/p/34232818

《Graph Attention Networks》阅读笔记

https://zhuanlan.zhihu.com/p/28170197

《Gated Graph Sequence Neural Networks》阅读笔记

https://mp.weixin.qq.com/s/Pm1HiEQOBnbo_GQ_v6Y_zw

腾讯提出自适应图卷积神经网络，接受不同图结构和规模的数据

https://mp.weixin.qq.com/s/bMpugd2Lp35VPr8fQAPzsg

一文概览图卷积网络基本结构和最新进展

https://zhuanlan.zhihu.com/p/31067515

《Semi-Supervised Classification with Graph Convolutional Networks》阅读笔记

https://mp.weixin.qq.com/s/6viSk0Ts_7eTfYrWYi_HDQ

基于图结构的实体和关系联合抽取模型简介

https://mp.weixin.qq.com/s/w5ldyp00CqkX8Kp-8Aw0nQ

图深度学习(GraphDL)，下一个人工智能算法热点？一文了解最新GDL相关文章

https://mp.weixin.qq.com/s/Jt6CjMqNFEXWoL5pkLeVyw

洛桑理工：Graph上的深度学习报告

https://zhuanlan.zhihu.com/p/36117802

《Learn to Represent Programs with Graphs》阅读笔记。这篇论文讲述了DL在程序代码纠错方面的应用。

https://zhuanlan.zhihu.com/p/37278426

Graph2Seq: Graph to Sequence Learning with Attention-based Neural Networks

https://mp.weixin.qq.com/s/iQYVyo2PHuGbEsYgdIf_oQ

DeepMind等机构提出“图网络”：面向关系推理

https://mp.weixin.qq.com/s/TAccHagxXQ82lfE91Y6xWg

CNN已老，GNN来了：重磅论文讲述深度学习的因果推理

https://mp.weixin.qq.com/s/UONtTJJgDawRPWtatAVKkg

如何利用高效的搜索算法来搜索网络的拓扑结构

https://mp.weixin.qq.com/s/lt9lZbulkW0C8A_xi6hodQ

浅析图卷积神经网络

https://mp.weixin.qq.com/s/SGCtwYWfnxjcpMJeeH1b4w

图神经网络+池化模块，斯坦福等提出层级图表征学习

https://mp.weixin.qq.com/s/DOau_vTbwCauQ8mrHkGu9Q

首个面向Facebook、arXiv网络图类的对抗攻击研究

https://mp.weixin.qq.com/s/XSug_qOqq_QaphkiRlGkIg

图卷积GCN前沿方法介绍

https://mp.weixin.qq.com/s/aeQyZ8cpz81cK8Dg-84mjA

网络表征学习综述

https://mp.weixin.qq.com/s/_0quf0IRe8mn4dnsBwf6Aw

基于路径的实体图关系抽取模型

https://mp.weixin.qq.com/s/jCgbBldpw4TGHUvN9WkJZg

在对抗中学习网络结构——87页PPT带你学习Graph中的GAN

https://mp.weixin.qq.com/s/xTZbfiLYHB64AJJRcw04qQ

知识图和神经网络：如何有效读取图节点属性

https://mp.weixin.qq.com/s/9fFjVSiMg-LwddXfNJuKuw

DeepMind开源图网络库，一种结合图和神经网络的新方法

https://mp.weixin.qq.com/s/5DmpgPN4t3p3H53Xu7_-3A

北大、微软亚洲研究院：高效的大规模图神经网络计算

https://mp.weixin.qq.com/s/BFJD8i_yg1Y6fxZS5or-rw

Bengio最新论文提出GibbsNet：深度图模型中的迭代性对抗推断

https://zhuanlan.zhihu.com/p/48834333

GCN in 2018：2018年顶会论文中的图卷积神经网络的理论与应用

https://mp.weixin.qq.com/s/TGuEvNXw_9S5-9a3KyDvvw

基于图卷积网络的图深度学习

https://mp.weixin.qq.com/s/zg3yW7e4UKIs9-m6WmcbvA

GraphWave：一种全新的无监督网络嵌入方法

https://mp.weixin.qq.com/s/rGC8O2Pyq8WL8D8ATMbH0Q

NYU、AWS联合推出：全新图神经网络框架DGL正式发布

https://mp.weixin.qq.com/s/mamet6l_lA7fhoYkysZ7PQ

华为联合LSE提出KONG：有序近邻图的核函数

https://mp.weixin.qq.com/s/WW-URKk-fNct9sC4bJ22eg

深度学习时代的图模型，清华发文综述图网络

https://mp.weixin.qq.com/s/Rr6SC-se_0q8dfEz0oUwlA

清华大学孙茂松课题组:《图神经网络: 方法与应用》综述论文

https://mp.weixin.qq.com/s/OnRB44tliuTFcjlmuRG3Xw

图神经网络“理论在哪里“？

https://mp.weixin.qq.com/s/bsNDI9YxFdaB2Q5aRz9ECw

图卷积神经网络的变种与挑战

https://mp.weixin.qq.com/s/Uy2ekBiwkI2sIo637b-16g

北大、微软提出NGra：高效大规模图神经网络计算

https://mp.weixin.qq.com/s/kcXp-uWcmIsAVfa63mor4g

图卷积网络介绍及进展

https://mp.weixin.qq.com/s/eelcT5x_kWC0dDt0_Ph4qg

清华朱文武组一文综述GraphDL五类模型

https://mp.weixin.qq.com/s/0rs8Wur7Iv6jSpFz5C-KNg

来自IEEE Fellow的GNN综述

https://mp.weixin.qq.com/s/I8pGqpKnRJp9HRglHfMZCw

手把手教你用DGL框架进行批量图分类

https://mp.weixin.qq.com/s/diIzbc0tpCW4xhbIQu8mCw

阿里凑单算法首次公开！基于Graph Embedding的打包购商品挖掘系统解析

https://mp.weixin.qq.com/s/chiHw5gKnJyTJTQeF6gViw

在向量空间中启用网络分析和推理，清华大学崔鹏博士最新分享

https://mp.weixin.qq.com/s/oKwxWbCkH-xqYSJIBdb92A

2018超网络节点表示学习

https://mp.weixin.qq.com/s/kQlxLDHLI6xxFzwJVjFj7w

GraRep: 基于全局结构信息的图结点表示学习

https://mp.weixin.qq.com/s/WQlSghxG89JCroNZSmop8w

朱军：关于图的表达学习

https://mp.weixin.qq.com/s/WMpcamrHjUDnYwqyISdooA

斯坦福Jure Leskovec图表示学习：无监督和有监督方法

https://mp.weixin.qq.com/s/mTCrTPzyeogwRHfgitfK6Q

为什么说图网络是AI的未来？

https://mp.weixin.qq.com/s/cdbHoR_E_mpIdcvmNGWfDA

掌握图神经网络GNN基本，看这篇文章就够了

https://mp.weixin.qq.com/s/043sK8IDmdTYDpbCfPLIxw

图神经网络综述：方法及应用

https://mp.weixin.qq.com/s/c6ZhSk4r3pvnjHsvpwkkSw

用图卷积网络(GCN)来做语义角色标注

https://mp.weixin.qq.com/s/B8rJRlnwGJKUSI17Ot66Xw

从CNN到GCN的联系与区别——GCN从入门到精（fang）通（qi）

https://mp.weixin.qq.com/s/DUv5c6ce-dgLOBAE4ChiQg

图神经网络为何如此强大？看完这份斯坦福31页PPT就懂了！

https://mp.weixin.qq.com/s/2SaBiMJzhSRw1G0giL9TAw

深入理解图注意力机制

https://mp.weixin.qq.com/s/sg9O761F0KHAmCPOfMW_kQ

图卷积网络到底怎么做，这是一份极简的Numpy实现

https://mp.weixin.qq.com/s/s6E2vV1KrQDI4SeAnkYTKw

图神经网络将成AI下一拐点！MIT斯坦福一文综述GNN到底有多强
