---
layout: post
title:  机器学习（三十七）——KNN, Optimizer进阶, 聚类算法, 特征工程, 模仿学习, 三门问题, 推荐系统进阶（2）
category: ML 
---

# KNN

K最近邻(k-Nearest Neighbor，KNN)分类算法，是一个理论上比较成熟的方法，也是最简单的机器学习算法之一。

该方法的思路是：如果一个样本在特征空间中的k个最相似(即特征空间中最邻近)的样本中的大多数属于某一个类别，则该样本也属于这个类别。

KNN算法中，所选择的邻居都是已经正确分类的对象。该方法在定类决策上只依据最邻近的一个或者几个样本的类别来决定待分样本所属的类别。

KNN方法虽然从原理上也依赖于极限定理，但在类别决策时，只与极少量的相邻样本有关。由于KNN方法主要靠周围有限的邻近的样本，而不是靠判别类域的方法来确定所属类别的，因此对于类域的交叉或重叠较多的待分样本集来说，KNN方法较其他方法更为适合。

## 和K-means的区别

虽然K-means和KNN都有计算点之间最近距离的步骤，然而两者的目的是不同的：K-means是聚类算法，而KNN是分类算法。

一个常见的应用是：使用K-means对训练样本进行聚类，然后使用KNN对预测样本进行分类。

## KNN在时间序列分析上的应用

KNN虽然主要是个分类算法，但通过构建特殊的模型，亦可应用于其他领域。其中，KNN在时间序列分析上的应用，就是一个很有技巧性的事情。

假设已知时间序列$$X:\{x_1,\dots,x_n\}$$，来预测$$x_{n+1}$$。

首先，我们选取$$x_{n+1}$$之前的最近m个序列值，作为预测值的特征向量$$X_{m\{n+1\}}$$。这里的m一般根据时间序列的周期来选择，比如商场客流的周期一般为一周。

$$X_{m\{n+1\}}$$和预测值$$x_{n+1}$$组成了扩展向量$$[X_{m\{n+1\}},x_{n+1}]$$。为了表明$$x_{n+1}$$是预测值的事实，上述向量又写作$$[X_{m\{n+1\}},y_{n+1}]$$。

依此类推，对于X中的任意$$x_i$$，我们都可以构建扩展向量$$[X_{m\{i\}},y_{i}]$$。即我们假定，$$x_i$$的值由它之前的m个序列值唯一确定。显然，由于是已经发生了的事件，这里的$$y_{i}$$都是已知的。

在X中，这样的m维特征向量共有$$n-m$$个。使用KNN算法，获得与$$X_{m\{n+1\}}$$最邻近的k个特征向量$$X_{m\{i\}}$$。然后根据这k个特征向量的时间和相似度，对k个$$y_{i}$$值进行加权平均，以获得最终的预测值$$y_{n+1}$$。

参考：

http://www.doc88.com/p-1416660147532.html

KNN算法在股票预测中的应用

https://zhuanlan.zhihu.com/p/29838009

K近邻算法

https://mp.weixin.qq.com/s?__biz=MzI4ODU5NjQ3OQ==&mid=2247483791&idx=1&sn=0fafce03d0c20a14020e193b9b5b64e6

机器学习分类算法之k-近邻算法

https://mp.weixin.qq.com/s/HYNHkk9KSxuWZEfmPXAJKA

KNN简明教程

https://mp.weixin.qq.com/s/7ohIh_dVfzNyt7TpBlCFYw

机器学习算法KNN简介及实现

https://mp.weixin.qq.com/s/NukVEGgbqVx0S-LlBNrb-Q

以前你可能一直用错“K均值聚类”？

# Optimizer进阶

## AdaSecant

《ADASECANT: Robust Adaptive Secant Method for Stochastic Gradient》

## 二阶Optimizer

虽然二阶Optimizer的收敛效果优于一阶Optimizer，但由于计算量较大，通常用的较少。

常用的算法有BGFS和L-BFGS。

http://www.cnblogs.com/kemaswill/p/3352898.html

优化算法-BFGS

http://blog.csdn.net/acdreamers/article/details/44728041

L-BFGS算法

https://mp.weixin.qq.com/s/lGrTUYALmKOQkO70DZpbPQ

小改进，大飞跃：深度学习中的最小牛顿求解器

## 参考

https://mp.weixin.qq.com/s/n1Ks8I3Ldgb-u-kVbGBZ5Q

机器学习中的优化方法

https://mp.weixin.qq.com/s/4XOI8Dq6fqe8rhtJjeyxeA

超级收敛：使用超大学习率超快速训练残差网络

http://mp.weixin.qq.com/s/Q5kBCNZs3a6oiznC9-2bVg

Michael Jordan新研究官方解读：如何有效地避开鞍点

https://mp.weixin.qq.com/s/idmt0F49tOCh-ghWdHLdUw

吴恩达导师Michael I.Jordan学术演讲：如何有效避开鞍点。这是Jordan半年后的另一个演讲，有些新内容。

https://mp.weixin.qq.com/s/jVjemfcLzIWOdWdxMgoxsA

超越Adam，从适应性学习率家族出发解读ICLR 2018高分论文

https://mp.weixin.qq.com/s/B9nUwPtgpsLkEyCOlSAO5A

1cycle策略：实践中的学习率设定应该是先增再降

https://mp.weixin.qq.com/s/dseeCB-CRtZnzC3d4_8pYw

AMSGrad能够取代Adam吗

https://mp.weixin.qq.com/s/7E8o1TnvmAvZgB7_AWCunQ

2018值得尝试的无参数全局优化新算法

https://mp.weixin.qq.com/s/hK6BJGAPyg_RqqilCcf2NA

全局自动优化：C++机器学习库dlib引入自动调参算法

https://mp.weixin.qq.com/s/T-v9OTcJa5OQ71QmYrFtbg

斯坦福大学提出SGD动量自调节器YellowFin

https://mp.weixin.qq.com/s/6u5W7Lm81Wtczdzp5WCJWw

DeepMind提出新型超参数最优化方法：性能超越手动调参和贝叶斯优化

https://mp.weixin.qq.com/s/0V8B-u5_bRM5Fu9oOAYjqw

清华大学：通过在单纯形上软门限投影的加速随机贪心坐标下降

https://mp.weixin.qq.com/s/LuuvvL9yZ3ucXxRq0pZfsg

优化策略：Label Smoothing Regularization_LSR原理分析

https://zhuanlan.zhihu.com/p/23866364

从梯度下降到Hessian-Free优化

https://mp.weixin.qq.com/s/HPrjEdszBSvVoVS66W-Fjw

2017年深度学习优化算法研究亮点最新综述

https://mp.weixin.qq.com/s/W06YcuGWalDbyUaZa_kZnQ

2017年深度学习优化算法最新综述

https://mp.weixin.qq.com/s/WQ6CxRS-v_y-7PnYY-1ffg

在局部误差边界条件下的随机子梯度方法的加速

https://mp.weixin.qq.com/s/rOltA6fDzWmxcSyoHYqeSg

谷歌提出最新参数优化方法Adafactor，已在TensorFlow中开源

https://mp.weixin.qq.com/s/eTVPLSpZir4A49bhmWAibQ

深度学习中的各种优化算法

https://mp.weixin.qq.com/s/CQpIhVinDPhXpp70WhyYww

当前训练神经网络最快的方式：AdamW优化算法+超级收敛

https://mp.weixin.qq.com/s/y3ThoC2A04q4uWiOsuhUJw

腾讯AI Lab提出误差补偿式量化SGD：显著降低分布式机器学习的通信成本

https://mp.weixin.qq.com/s/aBt0qXPHFvwSs-0MtJYKjQ

一文告诉你Adam、AdamW、Amsgrad区别和联系，助你实现Super-convergence的终极目标

https://mp.weixin.qq.com/s/aZlJNZsSv60ZZi2heGo_Mw

一文简述深度学习优化方法——梯度下降

https://mp.weixin.qq.com/s/DsmjjfInV_yPFWB2oSq-dA

取代学习率衰减的新方法：谷歌大脑提出增加Batch Size

https://mp.weixin.qq.com/s/jgOQGDqDKtbJXbAj3EpI9A

别用大批量mini-batch训练神经网络，用局部SGD！

https://zhuanlan.zhihu.com/p/45298186

Matrix Factorization方法证明总结

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

# 特征工程

https://mp.weixin.qq.com/s/ibiElLIgrT3wYx3tDYMMTw

理解特征工程

https://mp.weixin.qq.com/s/3Ce8uMf_Kyt-hEZUYfdh3g

特征工程之特征选择

https://mp.weixin.qq.com/s/tOcyfK68jW7Tr-PGCvdXMA

特征工程最后一个要点:特征预处理

https://mp.weixin.qq.com/s/c9iHdgtErVd_iitwny7_zw

Kaggle前1%参赛者经验：特征工程为何如此重要？

# 模仿学习

https://zhuanlan.zhihu.com/p/27935902

机器人学习Robot Learning之模仿学习Imitation Learning的发展

https://zhuanlan.zhihu.com/p/25688750

模仿学习（Imitation Learning）完全介绍

https://mp.weixin.qq.com/s/naq73D27vsCOUBperKto8A

从监督式到DAgger，综述论文描绘模仿学习全貌

https://mp.weixin.qq.com/s/LNNqp2KsEAljG26hY43mUw

ICML2018 模仿学习教程

# 三门问题

https://www.zhihu.com/question/26709273/

蒙提霍尔问题（又称三门问题、山羊汽车问题）的正解是什么？

https://zhuanlan.zhihu.com/p/21461266

数学杂谈——“三门问题”：Monty Hall Problem

https://zhuanlan.zhihu.com/p/23338174

蒙提霍尔问题/三门问题（Monty Hall problem）

# 推荐系统进阶

https://mp.weixin.qq.com/s/qmkMJJkMqumbtynM4cLatw

计算广告CTR预估系列

https://zhuanlan.zhihu.com/p/38117606

早期购买行为的分析和预测建模

https://mp.weixin.qq.com/s/nQAqEZW_TJSsbVp-kVcKGA

简谈马尔可夫模型在个性化推荐中的应用

https://mp.weixin.qq.com/s/Zalby_gZsxzrmBq6ZePsiQ

短视频如何做到千人千面？FM+GBM排序模型深度解析

https://mp.weixin.qq.com/s/hkXyqe6tDdgNuLgk3e90JQ

如何发现品牌潜客？目标人群优选算法模型及实践解析

