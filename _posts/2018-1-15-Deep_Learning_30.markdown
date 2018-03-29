---
layout: post
title:  深度学习（三十）——SOM
category: DL 
---

# RBM & DBN & Deep Autoencoder（续）

## 参考

https://deeplearning4j.org/cn/restrictedboltzmannmachine

受限玻尔兹曼机基础教程

http://txshi-mt.com/2018/02/04/UTNN-11-Hopfield-Nets-and-Boltzmann-Machines/

Hopfield网和玻尔兹曼机

http://txshi-mt.com/2018/02/10/UTNN-12-Restricted-Boltzmann-Machines/

受限玻尔兹曼机

http://www.cnblogs.com/kemaswill/p/3269138.html

基于受限玻尔兹曼机(RBM)的协同过滤

http://www.cnblogs.com/kemaswill/p/3203605.html

受限波尔兹曼机简介

https://mp.weixin.qq.com/s/MqnJ39GPrzP4xJWDZi9gnQ

博士生开源深度学习C++库DLL：快速构建卷积受限玻尔兹曼机

http://www.cs.toronto.edu/~fritz/absps/cogscibm.pdf

A Learning Algorithm for Boltzmann Machines

http://www.cs.toronto.edu/~hinton/absps/dbm.pdf

Deep Boltzmann Machines

http://www.cs.toronto.edu/~hinton/absps/guideTR.pdf

A Practical Guide to Training Restricted Boltzmann Machines

http://www.taodocs.com/p-62206829.html

《受限波尔兹曼机简介》张春霞著

# SOM

Self Organizing Maps (SOM)是一种无监督算法，主要用于聚类和可视化。

>Teuvo Kohonen，1934年生，芬兰科学家。Helsinki University of Technology博士和教授。

![](/images/img2/SOM.png)

上图是一个SOM可视化的案例。左边的六角形网格被称作SOM的map space。其中，越相似的国家，在map space上的颜色和位置越接近。

下面来说一下SOM的具体训练方法。

## 构建map space

首先，map space是有拓扑关系的。这个拓扑关系需要我们确定，如果想要一维的模型，那么隐藏节点依次连成一条线；如果想要二维的拓扑关系，那么就组成一个平面网格。换句话说，SOM的目标就是**将任意维度的输入离散化到一维或者二维(更高维度的不常见)的离散空间上。**

网格的形状一般是矩形或六角形。网格中的每个node关联一个weight vector，其中的值表示一个input space中的点，因此weight vector和input vector的维度是相同的。

## 初始化weight vector

![](/images/img2/SOM_2.png)

上图展示了SOM的训练过程。SOM训练的目的就是使map space尽量贴合样本分布。

因此，通常有两种初始化weight vector的方法：

1）用小的随机值初始化。

2）从最大的两个主特征向量上进行采样。

显然，第2种训练起来要快的多。因为它已经是SOM weights的一个很好的近似了。

## 训练SOM



## 参考

http://chem-eng.utoronto.ca/~datamining/Presentations/SOM.pdf

Self-Organizing Maps

http://www.cnblogs.com/sylvanas2012/p/5117056.html

Self Organizing Maps (SOM): 一种基于神经网络的聚类算法

http://blog.csdn.net/Loyal2M/article/details/11225987

聚类算法实践（3）——PCCA、SOM、Affinity Propagation

