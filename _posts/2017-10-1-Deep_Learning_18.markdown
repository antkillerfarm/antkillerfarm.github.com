---
layout: post
title:  深度学习（十八）——Mask R-CNN, 目标跟踪, 图像超分辨率算法
category: DL 
---

# Mask R-CNN

Mask R-CNN虽然挂着R-CNN的名头，但却是一个对象实例分割（不仅要分出对象的类别，连同一类对象的不同实例也要分出来）的NN。它是何恺明2017年的新作。

论文：

《Mask R-CNN》

只有非官方的代码：

Caffe版本：

https://github.com/jasjeetIM/Mask-RCNN

TensorFlow版本：

https://github.com/hillox/TFMaskRCNN

MXNet版本：

https://github.com/TuSimple/mx-maskrcnn

参考：

https://zhuanlan.zhihu.com/p/25954683

Mask R-CNN个人理解

https://mp.weixin.qq.com/s/E0P2B798pukbtRarWooUkg

Mask R-CNN的Keras/TensorFlow/Pytorch代码实现

https://zhuanlan.zhihu.com/p/30967656

从R-CNN到Mask R-CNN

# 目标跟踪

## 概述

目标跟踪（object tracking）就是在连续的视频序列中，建立所要跟踪物体的位置关系，得到物体完整的运动轨迹。

目标跟踪分为单目标跟踪和多目标跟踪。本文如无特别指出，均指单目标跟踪。

通常的做法是：

1.在第1帧给一个bbox框住需要跟踪的物体。

2.在不借助重检测（re-detection）的情况下，尽可能长时间的跟住物体。

3.不能使用依赖外部特征的姿态估计（pose estimation）。

当然这是针对目标跟踪算法的要求，至于实际产品中，对象的重检测以及依赖外部特征的姿态估计都是必不可少的。

比如，自动驾驶领域的车辆跟踪，一般都会针对车辆的运动特点建立模型，以辅助目标跟踪。

## TB50 & TB100

这个领域最著名的数据集是吴毅提出的TB50 & TB100，50和100分别代表视频数量。有的论文也把它们称作OTB-2013和OTB-2015。

>吴毅，中国科学院自动化研究所博士（2009年）。南京审计大学副教授。

官网：

http://cvlab.hanyang.ac.kr/tracker_benchmark/

论文：

《Online object tracking: A benchmark》

《Object tracking benchmark》

上面的论文在TB数据集上比较了包括2012年及之前的29个顶尖的tracker，基本解决了统一标准的问题，因此也成为了目标跟踪领域的权威数据集。

《Object tracking: A survey》

这篇论文总结了2006年以前的目标跟踪算法。

## VOT

VOT竞赛数据集是另一个常用数据集。官网：

http://votchallenge.net/challenges.html

OTB包括25%的灰度序列，但VOT都是彩色序列，这也是造成很多颜色特征算法性能差异的原因。

## 目标跟踪的难点

![](/images/article/tracker_hard.png)

![](/images/article/tracker_hard_2.png)

## 参考

https://www.zhihu.com/question/26493945

计算机视觉中，目前有哪些经典的目标跟踪算法？

https://zhuanlan.zhihu.com/p/22334661

深度学习在目标跟踪中的应用

https://mp.weixin.qq.com/s/3R8zaFUKFTvp0uE8lY44WA

首个应用残差学习的深度目标跟踪算法

https://zhuanlan.zhihu.com/p/27293523

目标跟踪领域进展报告

https://zhuanlan.zhihu.com/p/27335895

CVPR 2017目标跟踪相关论文

https://www.zhihu.com/question/59623472

基于深度学习的目标跟踪算法是否可能做到实时？

http://acsweb.ucsd.edu/~yuw176/report/vehicle.pdf

Monocular Vehicle Detection and Tracking

https://mp.weixin.qq.com/s/x7LoIN7mOJcZDY70GB_rLg

VOT Challenge 2017亚军北邮团队技术分享

# 图像超分辨率算法

Super Resolution（SR）的目标是：给你一张低分辨率的小图（Low Resolution，LR），你通过算法将这张LR放大成一张高分辨率的大图（High Resolution，HR）。

参考：

https://zhuanlan.zhihu.com/p/25532538

深度学习在图像超分辨率重建中的应用

https://zhuanlan.zhihu.com/p/25201511

深度对抗学习在图像分割和超分辨率中的应用

https://mp.weixin.qq.com/s/uK0L5RV0bB2Jnr5WCZasfw

深度学习在单图像超分辨率上的应用：SRCNN、Perceptual loss、SRResNet

https://mp.weixin.qq.com/s/KxQ-GRnEYEdmS2H-DHIHOg

南京理工大学ICCV 2017论文：图像超分辨率模型MemNet

https://mp.weixin.qq.com/s/xpvGz1HVo9eLNDMv9v7vqg

NTIRE2017夺冠论文：用于单一图像超分辨率的增强型深度残差网络

https://www.zhihu.com/question/25401250

如何通过多帧影像进行超分辨率重构？

https://www.zhihu.com/question/38637977

超分辨率重建还有什么可以研究的吗？

https://zhuanlan.zhihu.com/p/25912465

胎儿MRI高分辨率重建技术：现状与趋势

# ACBM算法

ACBM算法是在AC（Aho-Corasick）自动机（UNIX上的fgrep命令使用的就是AC算法）的基础之上，引入了BM（Boyer-Moore）算法的多模扩展，实现的高效的多模匹配。和AC自动机不同的是，ACBM算法不需要扫描目标文本串中的每一个字符，可以利用本次匹配不成功的信息，跳过尽可能多的字符，实现高效匹配。

>注： Alfred Vaino Aho，1941年生，加拿大计算机科学家。普林斯顿大学博士，长期供职于贝尔实验室，后为哥伦比亚大学教授。egrep和fgrep的发明人，AWK语言的联合发明人。著有《Principles of Compiler Design Compilers: Principles, Techniques, and Tools》。该书由于封面上有龙图案，又被称为“龙书”，是编译原理方面的权威书籍。2003年获IEEE John von Neumann Medal。

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

# 高斯过程回归

从大的分类来说，机器学习的算法可分为两类：

1.定义一个模型，用训练数据训练模型的参数，然后用训练好的模型进行预测。这种方法的缺点在于，预测效果和模型与样本的匹配程度有关。比如对非线性样本采用线性模型，其预测效果通常不会太好。但是增加模型的复杂度，又会导致过拟合。

2.定义一个函数分布，赋予每一种可能的函数一个先验概率，可能性越大的函数，其先验概率越大。但是可能的函数往往为一个不可数集，即有无限个可能的函数，随之引入一个新的问题：如何在有限的时间内对这些无限的函数进行选择？一种有效解决方法就是高斯过程回归(Gaussian process regression，GPR)。

>注：Radford M. Neal，1956年生，加拿大科学家。多伦多大学博士（1995）和教授。贝叶斯神经网络的发明人。导师为Geoffrey Hinton。   
>个人主页：http://www.cs.toronto.edu/~radford/

>Danie G. Krige，1919～2013，南非矿业工程师和统计学家，威特沃特斯兰德大学教授。地理统计学早期的代表人物之一。

http://www.cnblogs.com/hxsyl/p/5229746.html

https://mqshen.gitbooks.io/prml/content/Chapter6/gaussian/gaussian_processes_regression.html

http://www.gaussianprocess.org/gpml/chapters/RW.pdf

http://people.cs.umass.edu/~wallach/talks/gp_intro.pdf

http://wenku.baidu.com/view/72f80113915f804d2b16c173.html

# 压缩感知

http://blog.csdn.net/abcjennifer/article/details/7721834

初识压缩感知Compressive Sensing

http://blog.csdn.net/abcjennifer/article/details/7724360

中国压缩传感资源（China Compressive Sensing Resources）

http://blog.csdn.net/xiahouzuoxin/article/details/38820925

白话压缩感知（含Matlab代码）

http://blog.csdn.net/abcjennifer/article/details/7748833

压缩感知进阶——有关稀疏矩阵
