---
layout: post
title:  机器学习（三十三）——机器学习的算法体系&相关术语表, ACBM算法, 高斯过程回归
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

# ACBM算法

ACBM算法是在AC（Aho-Corasick）自动机（UNIX上的fgrep命令使用的就是AC算法）的基础之上，引入了BM（Boyer-Moore）算法的多模扩展，实现的高效的多模匹配。和AC自动机不同的是，ACBM算法不需要扫描目标文本串中的每一个字符，可以利用本次匹配不成功的信息，跳过尽可能多的字符，实现高效匹配。

>注： Alfred Vaino Aho，1941年生，加拿大计算机科学家。普林斯顿大学博士，长期供职于贝尔实验室，后为哥伦比亚大学教授。egrep和fgrep的发明人，AWK语言的联合发明人。2003年获IEEE John von Neumann Medal。

>Margaret John Corasick，贝尔实验室研究员。

>Robert Stephen Boyer，德克萨斯大学教授。

>J Strother Moore，德克萨斯大学教授。Boyer的好朋友，两人的绝大多数成就都是合作完成的。

参见：

http://blog.csdn.net/sealyao/article/details/6817944

ACBM算法

https://mp.weixin.qq.com/s/yVOgAuD9hEYAMdLyvae2VA

最长公共子序列与最长公共子串

https://mp.weixin.qq.com/s/8rP3fF9iVnplhkFmxuj6fw

一文读懂KMP算法

https://mp.weixin.qq.com/s/if7P0yq59DbBEjW15vfaQA

推荐一个高效算法wumanber：每秒680万匹配！

https://mp.weixin.qq.com/s/4m1O5ZHsZTRc-JuBF97_3w

字符串匹配的Boyer-Moore算法

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

# 热传导推荐算法

https://www.zhihu.com/question/20184666

推荐系统中用到的热传导算法和物质扩散是怎么用的？

http://tis.hrbeu.edu.cn/oa/pdfdow.aspx?Sid=20160307

基于影响力控制的热传导算法

http://www.doc88.com/p-7082821463697.html

改进的热传导和物质扩散混合推荐算法

# 目标检测进阶++

https://mp.weixin.qq.com/s/k8msLl6c2Cp_5h-4xBD6Zw

CVPR2019-目标检测分割技术进展

https://mp.weixin.qq.com/s/uzG8sic5Y6LVqBS6iKQDhw

目标检测中图像增强，mixup如何操作？

https://mp.weixin.qq.com/s/pkFcmm15gnuRJtngFX7f0w

目标检测训练trick超级大礼包—不改模型提升精度，值得拥有

https://mp.weixin.qq.com/s/flXzhQ-Ypf3fwTqLelLzOQ

李沐等将目标检测绝对精度提升5%，不牺牲推理速度

https://mp.weixin.qq.com/s/6QsyYtEVjavoLfU_lQF1pw

目标检测新文：Generalized Intersection over Union

https://mp.weixin.qq.com/s/Xs3nThAcUOq62bO2p61YFA

论文解读 Receptive Field Block Net for Accurate and Fast

https://mp.weixin.qq.com/s/dcrBQ-t3tLOTouEyofOBxg

间谍卫星：利用卷积神经网络对卫星影像进行多尺度目标检测

https://mp.weixin.qq.com/s/LtXylKTKsHdjMPw9Q1HyXA

优于MobileNet、YOLOv2：移动设备上的实时目标检测系统Pelee

https://mp.weixin.qq.com/s/xpk9LhsZ3dRMvqR6Uc5jeg

Pelee：移动端实时检测骨干网络

https://mp.weixin.qq.com/s/Gq3bflJq59Tx-nDCvbweNA

无需预训练分类器，清华&旷视提出专用于目标检测的骨干网络DetNet

https://mp.weixin.qq.com/s/u3eXhoFvo7vZujc0XoQQWQ

旷视研究院解读Light-Head R-CNN：平衡精准度和速度

https://mp.weixin.qq.com/s/6cUP9vvfcuv8rIEnGnAFiA

NCSU&阿里巴巴论文：可解释的R-CNN

https://mp.weixin.qq.com/s/1vOdOMyByBacSBMVrscq5Q

黄畅：基于DenesBox的目标检测在自动驾驶中的应用

https://mp.weixin.qq.com/s/-PeXMU_gkcT5YnMcLoaKag

CVPR清华大学研究，高效视觉目标检测框架RON

https://mp.weixin.qq.com/s/XoKdsQKyaI3LsDxF7uyKuQ

聊聊目标检测中的多尺度检测（Multi-Scale），从YOLO到FPN，SNIPER，SSD填坑贴和极大极小目标识别

https://mp.weixin.qq.com/s/GpZHGksl0elxMcaQYosK-A

SNIP的升级版SNIPER（效果比Mosaic更佳）

https://mp.weixin.qq.com/s/XdH54ImSfgadCoISmVyyVg

基于单目摄像头的物体检测

https://mp.weixin.qq.com/s/h_ENriEXr7WI_XR_DtxpMQ

这样可以更精确的目标检测——超网络

https://mp.weixin.qq.com/s/dFoUO4xArZpmtbKg1Kx6Zg

COCO mAP 53.3！骨干网合成算法CBNet带来目标检测精度新突破

https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247499933&idx=1&sn=b9fe7d6714c44acedd12a60cfe6b1c60

小样本域适应的目标检测

https://zhuanlan.zhihu.com/p/84890413

PolarMask：一阶段实例分割新思路

https://mp.weixin.qq.com/s/t8pVNeW2Y-QQwD8H9Nk83w

定向和密集的目标检测怎么办？动态优化网络来解决

https://zhuanlan.zhihu.com/p/158907507

From VanillaDet to AutoAssign

https://mp.weixin.qq.com/s/GL_q4VLCgbjTZq_zpTLq0w

Label Assign：提升目标检测上限

https://mp.weixin.qq.com/s/EO5h1zW4yrToqwTCnXtz4g

TTFNet: 最大程度提高训练效率的实时目标检测

https://mp.weixin.qq.com/s/6R3myLyAeMMS0lR1GSlvcA

细说物体检测中的Anchors

https://mp.weixin.qq.com/s/BsoqlaOlhXc9irSuBc6vGg

在物体检测中搞定小目标

https://zhuanlan.zhihu.com/p/200924181

计算机视觉中低延迟检测的相关理论和应用（上）

https://zhuanlan.zhihu.com/p/212842916

计算机视觉中低延迟检测的相关理论和应用（下）

https://mp.weixin.qq.com/s/Z5zFWr04Z2LBpf-6EXIgRg

OpenImage冠军方案：在物体检测中为分类和回归任务使用各自独立的特征图

https://mp.weixin.qq.com/s/_2DwSY6olj3wKy2xKukEGg

商汤开源Grid R-CNN Plus：相比Grid RCNN，速度更快，精度更高

https://mp.weixin.qq.com/s/baPfFVi7deEsCAFu3ColoQ

CVPR2018目标检测算法总览

https://mp.weixin.qq.com/s/t5p1xGKVnwd7wbiOzucFqQ

基于深度学习的目标检测算法剖析与实现

https://mp.weixin.qq.com/s/-zQZjHVs7bYyGkGuMUf3qg

目标检测领域还有什么可做的？19个方向给你建议
