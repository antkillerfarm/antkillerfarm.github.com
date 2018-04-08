---
layout: post
title:  图像处理理论（七）——经典目标跟踪算法, 从BOW到SPM, ILSVRC 2010考古
category: graphics 
---

# 经典目标跟踪算法

camshift、meanshift、Kalman filter、particle filter、Optical flow、TLD、KCF、Struck

## Meanshift

参考：

http://www.cnblogs.com/liqizhou/archive/2012/05/12/2497220.html

Meanshift，聚类算法

https://wenku.baidu.com/view/0d9eb876a417866fb84a8eb2.html

mean-shift算法概述

http://www.cnblogs.com/cfantaisie/archive/2011/06/10/2077188.html

meanshift聚类

## Camshift

参考：

http://blog.sina.com.cn/s/blog_5d1476580101a57j.html

Camshift算法

http://blog.163.com/thomaskjh@126/blog/static/370829982010113133152722/

CAMSHIFT原理

https://wenku.baidu.com/view/59596ac42cc58bd63186bd37.html

Camshift算法原理

## Optical flow

http://www.cnblogs.com/walccott/p/4956858.html

Horn-Schunck光流法

http://blog.csdn.net/u014568921/article/details/46638557

目标跟踪之Lukas-Kanade光流法

http://blog.csdn.net/zouxy09/article/details/8683859

光流Optical Flow介绍与OpenCV实现

http://www.cnblogs.com/gnuhpc/archive/2012/12/04/2802124.html

Lucas–Kanade光流算法

http://www.cnblogs.com/dzyBK/p/5096860.html

光流算法：Brox算法

http://www.cnblogs.com/quarryman/p/optical_flow.html

图像分析之光流之经典

https://zhuanlan.zhihu.com/p/31726032

走进光流的世界

## Particle filter

http://www.cvvision.cn/6002.html

基于粒子滤波器的目标跟踪算法及实现

http://www.cnblogs.com/zjb0823/p/3806333.html

运动目标跟踪算法综述

https://wenku.baidu.com/view/6554ba7402768e9951e73864.html

基于粒子滤波的视频目标追踪

http://www.cnblogs.com/feisky/archive/2009/11/10/1600086.html

粒子滤波概述

http://www.cnblogs.com/yangyangcv/archive/2010/05/23/1742263.html

基于粒子滤波的物体跟踪

https://www.zhihu.com/question/25371476

怎样从实际场景上理解粒子滤波（Particle Filter）？

http://freemind.pluskid.org/machine-learning/hmm-kalman-particle-filtering

漫谈HMM：Kalman/Particle Filtering

## TLD

http://blog.csdn.net/zouxy09/article/details/7893011

TLD（Tracking-Learning-Detection）学习与源码理解系列文章

http://blog.csdn.net/carson2005/article/details/7647500

比微软kinect更强的视频跟踪算法--TLD跟踪算法介绍

http://www.cnblogs.com/lxy2017/p/3927456.html

TLD（Tracking-Learning-Detection）一种目标跟踪算法

# Harris

http://blog.csdn.net/lwzkiller/article/details/54633670

Harris角点检测原理详解

http://www.cnblogs.com/king-lps/p/6375424.html

Harris角点检测

# 从BOW到SPM

## BOW

Bag-of-words模型是信息检索领域常用的文档表示方法。在信息检索中，BOW模型假定对于一个文档，忽略它的单词顺序和语法、句法等要素，将其仅仅看作是若干个词汇的集合，文档中每个单词的出现都是独立的，不依赖于其它单词是否出现。

为了表示一幅图像，我们可以将图像看作文档，即若干个“视觉词汇”的集合，同样的，视觉词汇相互之间没有顺序。

![](/images/article/cv_bow.jpg)

由于图像中的词汇不像文本文档中的那样是现成的，我们需要首先从图像中提取出相互独立的视觉词汇，这通常需要经过三个步骤：

（1）特征检测。

（2）特征表示。

（3）单词本的生成。

而SIFT算法是提取图像中局部不变特征的应用最广泛的算法，因此我们可以用SIFT算法从图像中提取不变特征点，作为视觉词汇，并构造单词表，用单词表中的单词表示一幅图像。

参考：

http://blog.csdn.net/v_JULY_v/article/details/6555899

SIFT算法的应用--目标识别之Bag-of-words模型

https://zhuanlan.zhihu.com/p/25999669

BOW算法，被CNN打爆之前的王者

## SPM

http://blog.csdn.net/chlele0105/article/details/16972695

SPM:Spatial Pyramid Matching for Recognizing Natural Scene Categories空间金字塔匹配

http://blog.csdn.net/jwh_bupt/article/details/9625469

Spatial Pyramid Matching 小结

# ILSVRC 2010考古

ILSVRC 2010的冠军是NEC和UIUC的联合队伍。这也是DL于2012年大放光彩之前比较杰出的成果。虽然现在它通常作为反面教材，出现在与DL的对比场景中，然而不可否认的是，它仍然是一个算法的杰作。

>林元庆，清华大学硕士+宾夕法尼亚大学博士（2008年）。原百度研究院院长。

![](/images/article/ILSVRC_2010.png)

上图是NEC算法的基本流程图。这里不打算描述整个算法，而仅对其中涉及的术语做一个解释。

