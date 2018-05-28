---
layout: post
title:  图像处理理论（七）——经典目标跟踪算法, 从BOW到SPM, ILSVRC 2010考古
category: graphics 
---

# Viola-Jones（续）

## 概述

和之前的方法不同，Viola-Jones不仅是一个算法，更是一个框架，前DL时代的人脸检测一般都采用该框架。其准确度也由Fisherface时代的不到70%，上升到90%以上。当然，这里所用的数据集以今天的眼光来看，只能算作玩具了——基本都是正面、无遮挡的标准照，光照也比较理想。但不管怎么说，这也是第一个进入商业实用阶段的目标检测框架，目前大多数的商业化产品仍然基于该框架。

Viola-Jones框架主要有三个要点：

1.Haar-like特征，AdaBoost算法和Cascade结构。Haar-like特征利用积分图像（Integral Image）快速的计算矩形区域的差分信号；

2.AdaBoost算法选择区分能力强的特征结合Stump函数做弱分类器，然后把若干这些弱分类器线性组合在一起增强分类性能；

3.Cascade结构做Early decision快速抛弃明显不是人脸的扫描窗口。

下面我们分别描述一下这几个要点。

## Integral image



![](/images/img2/integral_image_a.png)

![](/images/img2/integral_image_b.png)

$$46 – 22 – 20 + 10 = 14$$



参考：

http://www.mathworks.com/help/vision/ref/integralimage.html

Integral image

## 参考

https://www.jianshu.com/p/024ad859c8de

人脸检测的Viola-Jones方法

http://c.blog.sina.com.cn/profile.php?blogid=ab0aa22c890006v0

从Viola&Jones的人脸检测说起

http://www.cnblogs.com/hrlnw/archive/2013/10/23/3374707.html

Viola Jones Face Detector

# 直方图反向投影

http://www.cnblogs.com/zsb517/archive/2012/06/20/2556508.html

opencv直方图反向投影

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

# DTW（续）

结合连续性和单调性约束，每一个格点的路径就只有三个方向了。例如如果路径已经通过了格点$$(i, j)$$，那么下一个通过的格点只可能是下列三种情况之一：$$(i+1, j)$$，$$(i, j+1)$$或者$$(i+1, j+1)$$。

归整路径实际上就是满足上述约束的所有路径中，cumulative distances最小的那条路径，即：

$$D(i,j)=Dist(i,j)+\min(D(i-1,j),D(i,j-1),D(i-1,j-1)), D(1,1) = 0$$

这里的距离可以使用欧氏距离，也可以使用马氏距离。

DTW实例的具体计算过程可参见：

http://www.cnblogs.com/tornadomeet/archive/2012/03/23/2413363.html

从一个实例中学习DTW算法

从中可以看出，DTW实际上是一个动态规划问题。

更一般的，DTW也可用于计算两个离散的序列(不一定要与时间有关)的相似度。和《机器学习（二十二）》的EMD距离相比，DTW距离能够**保持序列的形状信息**。

除此之外，我们还可以增加别的约束：

**全局路径窗口(Warping Window)**：$$\mid \phi_x(s)-\phi_y(s)\mid \leq r$$。比较好的匹配路径往往在对角线附近，所以我们可以只考虑在对角线附近的一个区域寻找合适路径(r就是这个区域的宽度);

**斜率约束(Slope Constrain)**：$$\dfrac{\phi_x(m)-\phi_x(n)}{\phi_y(m)-\phi_y(n)}\leq p$$和$$\dfrac{\phi_y(m)-\phi_y(n)}{\phi_x(m)-\phi_x(n)}\leq q$$，这个可以看做是局部的Warping Window，用于避免路径太过平缓或陡峭，导致短的序列匹配到太长的序列或者太长的序列匹配到太短的序列。

![](/images/img2/DTW.png)

上图是两种常见的约束搜索空间的方法。

DTW的缺点：

1.运算量大；

2.识别性能过分依赖于端点检测；

3.太依赖于说话人的原来发音；

4.不能对样本作动态训练；

5.没有充分利用语音信号的时序动态特性；

DTW适合于特定人基元较小的场合，多用于孤立词识别；

参考：

http://blog.csdn.net/zouxy09/article/details/9140207

动态时间规整（DTW）

https://blog.csdn.net/raym0ndkwan/article/details/45614813

DTW动态时间规整

http://www.cnblogs.com/luxiaoxun/archive/2013/05/09/3069036.html

Dynamic Time Warping动态时间规整算法

# Spectrogram

DTW是一种时域方法，作为信号处理自然少不了频域方法。这里我们先来了解一个叫声谱图的东西。

![](/images/img2/Spectrogram.png)

这段语音被分为很多帧，每帧语音都对应于一个频谱（通过短时FFT计算），频谱表示频率与能量的关系。在实际使用中，频谱图有三种，即线性振幅谱、对数振幅谱、自功率谱（对数振幅谱中各谱线的振幅都作了对数计算，所以其纵坐标的单位是dB（分贝）。这个变换的目的是使那些振幅较低的成分相对高振幅成分得以拉高，以便观察掩盖在低幅噪声中的周期信号）。

![](/images/img2/Spectrogram_2.png)

我们先将其中一帧语音的频谱通过坐标表示出来。

![](/images/img2/Spectrogram_3.png)

再将左边的频谱旋转90度。

![](/images/img2/Spectrogram_4.png)

然后把这些幅度映射到一个灰度级表示的直方图。0表示白色，255表示黑色。幅度值越大，相应的区域越黑。

![](/images/img2/Spectrogram_5.png)

这样我们会得到一个随着时间变化的频谱图，这个就是描述语音信号的spectrogram声谱图。

# Cepstrum Analysis

![](/images/img2/Cepstral.png)

上图是一个语音的频谱图。峰值就表示语音的主要频率成分，我们把这些峰值称为共振峰（formants），而共振峰就是携带了声音的辨识属性（就是个人身份证一样）。所以它特别重要。用它就可以识别不同的声音。

![](/images/img2/Cepstral_2.png)

既然它那么重要，那我们就是需要把它提取出来！我们要提取的不仅仅是共振峰的位置，还得提取它们转变的过程。所以我们提取的是频谱的包络（Spectral Envelope）。这包络就是一条连接这些共振峰点的平滑曲线。

![](/images/img2/Cepstral_3.png)

原始的频谱由两部分组成：包络和频谱的细节。这里用到的是对数频谱，所以单位是dB。

![](/images/img2/Cepstral_4.png)

怎么把他们分离开呢？也就是，怎么在给定$$\log X[k]$$的基础上，求得$$\log H[k]$$和$$\log E[k]$$以满足$$\log X[k] = \log H[k] + \log E[k]$$呢？

![](/images/img2/Cepstral_5.png)

为了达到这个目标，我们需要Play a Mathematical Trick。这个Trick是什么呢？就是对频谱做FFT。

