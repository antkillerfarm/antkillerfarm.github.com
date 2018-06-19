---
layout: post
title:  图像处理理论（七）——Viola-Jones, 经典目标跟踪算法, 从BOW到SPM, ILSVRC 2010考古
category: graphics 
---

# Viola-Jones

Viola-Jones方法由Paul Viola和Michael Jones于2001年提出。

>Paul Viola，MIT本科（1988）+博士（1995）。先后在微软、Amazon担任研究员。

>Michael Jones，MIT博士（1997）。现为Mitsubishi electric research laboratories研究员。

论文：

《Rapid Object Detection using a Boosted Cascade of Simple Features》

《Robust real-time face detection》

《An Extended Set of Haar-like Features for Rapid Object Detection》

《Learning Multi-scale Block Local Binary Patterns for Face Recognition》

《Implementing the Viola-Jones Face Detection Algorithm》

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

# WFST（续）

## FSM

![](/images/img2/FSM.png)

上图是finite-state machine（也叫finite-state automaton，FSA）的示意图。图中的Node表示State，顾名思义，FSM的State数量是有限的。图中的Edge表示Transition，Edge上的Label表示Input/Event。

FSM的含义是，在某一状态下，获得一个输入，从而产生一个状态转换。例如，上图中在Sleep状态下，如果输入是hungry的话，那么状态就会切换到Eat状态。当然了，输入也可以不改变状态，比如在Sleep状态下，输入是tired的时候。

## FST

![](/images/img2/FST.png)

上图是finite-state transducers的示意图。FST和FSM的差别主要在Edge上的Label。FST收到Input的时候，不仅会发生状态改变，还会产生Output序列。因此，其Label的格式为`input:output`。

## WFST

![](/images/img2/WFST.png)

上图是WFST的示意图。顾名思义，Label上不仅有Input、Output，还有Weight信息，其格式为`input:output/weight`。

在有些图中会碰到$$\epsilon$$. 这个符号在输入时表示不消耗任何输入，在输出位置表示不产生任何输出。

此外，还有格式为`input/weight`的FSM，一般被称为Weighted Finite-State Acceptors。

## 相关的群论知识

WFST是基于半环代数理论的，在介绍半环之前我先简单的说一下群和半群。

**群（Group）**：G为非空集合，如果在G上定义的二元运算*，满足：

（1）封闭性（Closure）：对于任意$$a,b\in G$$,有$$a*b\in G$$;

（2）结合律（Associativity）：对于任意$$a,b,c\in G,(a*b)*c=a*(b*c)$$;

（3）幺元（Identity）：存在幺元e，使得对于任意$$a\in G,e*a=a*e=a$$;

（4）逆元：对于任意$$a\in G$$,存在逆元$$a^{-1}*a=a*a^{-1}=e$$。

则称（G,*）为群。

**半群（Semigroup）**：仅满足封闭性和结合律群称为半群；如果还包含幺元，则成为幺元半群。

**半环（semiring）**：指具有两个二元运算$$+$$和$$\cdot$$的非空集合S，且满足：

（1）$$(S,+),(S,\cdot)$$都是半群；

（2）$$\forall a,b,c\in S,(a+b)c = ac+bc, c(a+b) = ca+cb$$

半环的形式化表示如下：

$$(K, \bigoplus, \bigotimes，0， 1)$$

其中K是一个数集，$$\bigoplus, \bigotimes$$是两个二元操作，’0’和’1’是特定的（designated）零元素和幺元素（不一定是真正的数0和数1）。

常用的半环如下表所示：

| Semiring | Set | $$\oplus$$ | $$\otimes$$ | 0 | 1 |
|:--:|:--:|:--:|:--:|:--:|:--:|
| Boolean | $$\{0,1\}$$ | $$\lor$$ | $$\land$$ | 0 | 1 |
| Probability | $$R_+$$ | $$+$$ | $$\times$$ | 0 | 1 |
| Log | $$R\cup\{-\infty，+\infty\}$$ | $$\oplus_{log}$$ | + | $$+\infty$$ | 0 |
| Tropical | $$R\cup\{-\infty，+\infty\}$$ | min | + | $$+\infty$$ | 0 |
| String | $$\Sigma^*\cup\{\infty\}$$ | $$\land$$ | $$\cdot$$ | $$\infty$$ | $$\epsilon$$ |

在WFST中用的比较多的是log半环和tropical半环。前者对路径概率进行了对数运算，而后者在log半环的基础上，进行了viterbi approximation，也就是用若干路径的概率极值，作为当前概率值，这和动态规划中的viterbi算法是一致的。

接下来定义WFST上的二元运算：

一整条路径的权重$$w[\pi ]=w[e_1]\bigotimes \cdots \bigotimes w[e_k]$$。

多个有限路径集合的权重$$w[R]=\bigoplus_{\pi \in R} w[\pi]$$。

参考：

http://hongjiang.info/semigroup-and-monoid/

半群(semigroup)与幺半群(monoid)

## Sum(Union)

介绍完WFST的定义，再来介绍一下定义在它之上的运算。

![](/images/img2/sum.png)

Sum运算的形式化描述为：

$$[T_1 \oplus T_2](x,y)=[T_1](x,y)\oplus [T_2](x,y)$$

## Product(Concatenation)

![](/images/img2/product.png)

Product运算的形式化描述为：

$$[T_1 \otimes T_2](x,y)=\bigoplus_{x=x_1x_2,y=y_1y_2} [T_1](x_1,y_1)\otimes [T_2](x_2,y_2)$$

## Closure

![](/images/img2/closure.png)

Closure运算的形式化描述为：

$$[T^*](x,y)=\bigoplus_{n=0}^{\infty}[T^n](x,y)$$



