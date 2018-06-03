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

# Mel-Frequency Analysis（续）

## 参考

http://blog.csdn.net/zouxy09/article/details/9156785

梅尔频率倒谱系数（MFCC）

https://my.oschina.net/jamesju/blog/193343

语音特征参数MFCC提取过程详解

https://liuyanfeier.github.io/2017/10/26/2017-10-27-Kaldi%E4%B9%8Bfbank%E5%92%8Cmfcc%E7%89%B9%E5%BE%81%E6%8F%90%E5%8F%96/

kaldi之fbank和mfcc特征提取

https://zhuanlan.zhihu.com/p/26680599

语音信号预处理及特征参数提取

# FBank

FBank和MFCC的计算步骤基本一致，只是没有做IDST而已。

FBank与MFCC对比：

1.计算量：MFCC是在FBank的基础上进行的，所以MFCC的计算量更大

2.特征区分度：FBank特征相关性较高（相邻滤波器组有重叠），MFCC具有更好的判别度，这也是在大多数语音识别论文中用的是MFCC，而不是FBank的原因

3.使用对角协方差矩阵的GMM由于忽略了不同特征维度的相关性，MFCC更适合用来做特征。

4.DNN/CNN可以更好的利用这些相关性，使用fbank特征可以更多地降低WER。

参考：

http://blog.csdn.net/wxb1553725576/article/details/78048546

Kaldi特征提取之-FBank

# Pitch Detection

http://blog.csdn.net/zouxy09/article/details/9141875

基音周期估计（Pitch Detection）

# Vector Quantization

http://blog.csdn.net/zouxy09/article/details/9153255

矢量量化（Vector Quantization）

# fMLLR

https://blog.csdn.net/xmdxcsj/article/details/78512645

声学特征变换fMLLR

# SGMM

https://blog.csdn.net/quhediegooo/article/details/68946100

子空间高斯混合模型-SGMM

# VTLN

https://blog.csdn.net/jiangyangbo/article/details/6535928

VTLN(Vocal Tract Length Normalisation)

# HMM与语音识别

HMM的基本概念参见《机器学习（二十二）》，这里谈一下HMM在语音识别领域的应用。

从概率的角度来说，语音识别的目标是寻找最可能的$$P(W\mid O)$$。这里的W表示word，O表示observation。

直接找显然没这么容易，所以要用到Bayes公式：

$$\frac{P(W)P(O\mid W)}{P(O)}$$

这里只有$$P(O)$$已知，剩下的两个参数都需要额外提供。其中HMM提供$$P(O\mid W)$$，LM提供$$P(W)$$。

由于HMM的path上的概率是各个transition probability的乘积，而这些概率都小于1，因此他们的乘积必然是更小的数。这时可以考虑使用对数，不仅可将乘法变为加法，同时数值的范围也得到了改善。

# 语音识别的评价指标

语音识别的评价指标主要是Word Error Rate（WER）。

错误的情况包括三种：

1.Substitutions：错词。

2.Deletions：漏词。

3.Insertions：多词。

$$WER=100\%\times \frac{Subs+Dels+Ins}{\text{word in correct sentence}}$$

类似的还有CER/PER：Character/Phoneme Error Rate。

需要注意的是，评价WER时，需要在ASR output和Label之间进行对齐操作，而不是简单的从左往右匹配，否则将无法正确处理Deletions和Insertions的情形。

还有根据公式可知，WER是可以大于100%的。

参考：

https://blog.csdn.net/quhediegooo/article/details/56834417

语音识别评估标准-WER

# 声学模型进阶

## 语音质量

更高的采样率可以降低WER。一般来说，16KHz相比8KHz的WER要小10%左右。

## Voice Detection

长时间的silence会增加WER，因此我们需要判断当前是否在说话。

Voice Detection包括两个方面：

1.Beginning-Point Detection。也叫做Voice Activity Detection（VAD）。有些类似于唤醒检测，但并不局限于设备的开机时刻。

2.End-Point Detection。

## Feature normalization

有时候需要对Feature进行normalization。例如，对MFCC特征减去均值，可以有效提升在噪声环境下的识别率。


