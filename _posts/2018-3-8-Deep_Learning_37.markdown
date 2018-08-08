---
layout: post
title:  深度学习（三十七）——人脸识别
category: DL 
---

# 人脸识别

## Cascade CNN

论文：

《A Convolutional Neural Network Cascade for Face Detection》

![](/images/img2/CascadeCNN.jpg)

这篇可以说是对经典的Viola-Jones方法的深度卷积网络实现，可以明显看出是3阶级联（12-net、24-net、48-net）。

前2阶的网络都非常简单，只有第3阶才比较复杂。这不是重点，重点是我们要从上图中学习多尺度特征组合。

以第2阶段的24-net为例，首先把上一阶段剩下的窗口resize为24*24大小，然后送入网络，得到全连接层的特征。同时，将之前12-net的全连接层特征取出与之拼接在一起。最后对组合后的特征进行softmax分类。

除了分类网络之外，Cascade CNN还包含了3个修正bounding box的CNN网络，分别叫做12-calibration-net，24-calibration-net和48-calibration-net。他们的结构与12-net等类似。

网络结构方面也就这样了，该论文最牛之处在于给出了这类级联网络的训练方法。

![](/images/img2/CascadeCNN_2.jpg)

1、按照一般的方法组织正负样本训练第一阶段的12-net和12-calibration-net网络；

2、 利用上述的1层网络在AFLW数据集上作人脸检测，在保证99%的召回率的基础上确定判别阈值T1。

3、将在AFLW上判为人脸的非人脸窗口作为负样本，将所有真实人脸作为正样本，训练第二阶段的24-net和24-calibration-net网络；

4、重复2和3，完成最后阶段的训练。

参考：

http://blog.csdn.net/shuzfan/article/details/50358809

人脸检测——CascadeCNN

## MTCNN

论文：

《Joint Face Detection and Alignment using Multi-task Cascaded Convolutional Networks》

![](/images/img2/MTCNN.png)

上面是该方法的流程图，可以看出也是三阶级联，和CascadeCNN很像。

stage1: 在构建图像金字塔的基础上，利用fully convolutional network来进行检测，同时利用boundingbox regression和NMS来进行修正。

stage2: 将通过stage1的所有窗口输入作进一步判断，同时也要做boundingbox regression和NMS。

stage3: 和stage2相似，只不过增加了更强的约束：5个人脸关键点（landmark）。

参考：

http://blog.csdn.net/qq_14845119/article/details/52680940

MTCNN（Multi-task convolutional neural networks）人脸对齐

http://blog.csdn.net/shuzfan/article/details/52668935

人脸检测——MTCNN

https://mp.weixin.qq.com/s/IrZEQ69RNUdcs0Fl8fHmmQ

如何应用MTCNN和FaceNet模型实现人脸检测及识别

## Triplet Loss

Triplet loss通常是在个体级别的细粒度识别上使用，传统的分类是花鸟狗的大类别的识别，但是有些需求是要精确到个体级别，比如精确到哪个人的人脸识别，所以triplet loss的最主要应用也就是face identification，person re-identification，vehicle re-identification的各种identification识别问题上。

当然你可以把每个人当做一个类别来进行分类训练，但是往往最后会造成softmax的维数远大于feature的维数。

![](/images/img2/Triplet_Loss.png)

如上图所示，triplet是一个三元组，这个三元组是这样构成的：从训练数据集中随机选一个样本，该样本称为Anchor，然后再随机选取一个和Anchor(记为$$x^a$$)属于同一类的样本和不同类的样本,这两个样本对应的称为Positive(记为$$x^p$$)和Negative(记为$$x^n$$)，由此构成一个（Anchor，Positive，Negative）三元组。

针对每个样本$$x_i$$，训练一个参数共享或者不共享的网络，得到三个元素的特征表达，分别记为：$$f(x_i^a), f(x_i^p), f(x_i^n)$$。

triplet loss的目的就是通过学习，让$$x^a$$和$$x^p$$特征表达之间的距离尽可能小，而$$x^a$$和$$x^n$$的特征表达之间的距离尽可能大。公式化的表示就是：

$$\|f(x_i^a)-f(x_i^p)\|_2^2 + \alpha < \|f(x_i^a)-f(x_i^n)\|_2^2$$

其中，$$\alpha$$表示两个距离之间的间隔。因此，对应的目标函数也就很清楚了：

$$\sum_i^N\left[\|f(x_i^a)-f(x_i^p)\|_2^2 - \|f(x_i^a)-f(x_i^n)\|_2^2 + \alpha \right]_+$$

这里距离用欧式距离度量，+表示[]内的值大于零的时候，取该值为损失，小于零的时候，损失为零。 

参考：

https://blog.csdn.net/u010167269/article/details/52027378

Triplet Loss、Coupled Cluster Loss探究

https://blog.csdn.net/tangwei2014/article/details/46788025

triplet loss原理以及梯度推导

https://www.zhihu.com/question/62486208

triplet loss在深度学习中主要应用在什么地方？有什么明显的优势？

https://mp.weixin.qq.com/s/XB9VsW3NRwHua6AdRL3n8w

Lossless Triplet Loss:一种高效的Siamese网络损失函数

## Coupled Cluster Loss

论文：

《Deep Relative Distance Learning: Tell the Difference Between Similar Vehicles》



参考：

https://blog.csdn.net/u010167269/article/details/51783446

论文中文笔记

## FaceNet

论文：

《FaceNet: A Unified Embedding for Face Recognition and Clustering》

https://blog.csdn.net/stdcoutzyx/article/details/46687471

FaceNet--Google的人脸识别

## OpenFace

OpenFace是一款开源的人脸识别软件。它的原理基于CVPR 2015年的论文：FaceNet。由于采用了深度学习技术，OpenFace对人脸识别的准确率，大大超过了OpenCV。

OpenFace是用Python和Torch编写的。

官网：

https://cmusatyalab.github.io/openface/

参考：

http://www.cnblogs.com/pandaroll/p/6590339.html

开源人脸识别openface

https://mp.weixin.qq.com/s/RSCrkeIToeNKrFvMITxzDg

通过OpenFace来理解人脸识别

## SeetaFace

SeetaFace人脸识别引擎由中科院计算所山世光研究员带领的人脸识别研究组研发。代码基于C++实现，且不依赖于任何第三方的库函数，开源协议为BSD-2，可供学术界和工业界免费使用。

代码：

https://github.com/seetaface

论文：

《Coarse-to-Fine Auto-Encoder Networks (CFAN) for Real-Time Face Alignment》

参考：

https://zhuanlan.zhihu.com/p/22451474

SeetaFace开源人脸识别引擎介绍

http://www.cnblogs.com/nenya33/p/6801045.html

CFAN

## Siamese network

https://www.jianshu.com/p/92d7f6eaacf5

Siamese network孪生神经网络--一个简单神奇的结构

https://blog.csdn.net/sxf1061926959/article/details/54836696

Siamese Network理解

https://vra.github.io/2016/12/13/siamese-caffe/

Caffe中的Siamese网络

## 人脸年龄识别

https://www.openu.ac.il/home/hassner/projects/cnn_agegender/

Age and Gender Classification using Convolutional Neural Networks

## DeepFace

《DeepFace: Closing the Gap to Human-Level Performance in Face Verification》

DeepFace先进行了两次全卷积＋一次池化，提取了低层次的边缘／纹理等特征。

后接了3个Local-Conv层，这里是用Local-Conv的原因是，人脸在不同的区域存在不同的特征（眼睛／鼻子／嘴的分布位置相对固定），当不存在全局的局部特征分布时，Local-Conv更适合特征的提取。

## 参考

https://mp.weixin.qq.com/s/eZ78biXN-mVw3s9Ky_LBZg

如何走近深度学习人脸识别？你需要这篇超长综述

https://zhuanlan.zhihu.com/p/32702868

人脸检测背景介绍和发展现状

http://mp.weixin.qq.com/s/KQxGQdLa3XzKVIFYqlrV7g

人脸检测与识别的趋势和分析

https://zhuanlan.zhihu.com/p/25335957

人脸检测与深度学习

http://www.leiphone.com/news/201608/MPXlWtGaJLPYL7NB.html

人脸检测发展：从VJ到深度学习

https://zhuanlan.zhihu.com/p/22591740

从事人脸识别研究必读的N篇文章

https://mp.weixin.qq.com/s/7cZTbTBttEvN6NvOarSFgw

如何构建自定义人脸识别数据集

https://mp.weixin.qq.com/s/Z06oUe7oExgkKZ8g2_PnPw

科普人脸表情识别技术

https://mp.weixin.qq.com/s/tS_9gS2ADoEdSKCLB-cxpA

刘小明教授为你讲解人脸识别

https://mp.weixin.qq.com/s/CLgyaAslE4hYHi62UhzeGw

韩琥：深度学习让机器给人脸“贴标签”

https://mp.weixin.qq.com/s/pUcVO8Fj-n9-pMwAzBvWuQ

山世光：人脸识别领域的“激荡20年”

https://zhuanlan.zhihu.com/p/23340343

Center Loss及其在人脸识别中的应用

https://mp.weixin.qq.com/s/Gqlo4wU0wtIcJDvQs2kTAw

为了让你分清人脸识别与人脸检测，苹果要亲自给你科普

https://mp.weixin.qq.com/s/ZJmgC8xTruaRLfCocodjqA

人脸检测与识别总结

https://mp.weixin.qq.com/s/lGIkLcglQGvDzdBQmZW0Qw

判别特征学习方法用于人脸识别

https://mp.weixin.qq.com/s/t-vFSkQ7SYlTGGxEEY1mSg

近期人脸对齐的实证性研究

https://mp.weixin.qq.com/s/zhIyZ9MEFXAuIVUxAGkW-w

人脸对齐之GBDT(ERT)算法解读

https://mp.weixin.qq.com/s/1g9PXc_3nhKMf1_-E_cVAA

图像识别的原理、过程、应用前景，精华篇！

https://mp.weixin.qq.com/s/CvdeV5xgUF0kStJQdRst0w

从传统方法到深度学习，人脸关键点检测方法综述

https://zhuanlan.zhihu.com/p/34404607

人脸识别的LOSS（上）

https://mp.weixin.qq.com/s/ZrnAqDJCLtMy_qTQ2RZT0A

级联MobileNet-V2实现人脸关键点检测

https://mp.weixin.qq.com/s/MyA8_yt4YCkFl67AyhpZow

尺度不变人脸检测器（S3FD-Single Shot Scale-invariant Face Detector）

https://mp.weixin.qq.com/s/b-v_hDHJjUxMithBDr3TcQ

实时旋转鲁棒人脸检测算法

https://blog.csdn.net/xiamentingtao/article/details/50908190

Facial Landmark Detection(人脸特征点检测)

https://mp.weixin.qq.com/s/i4HdS-lCrsv9YR39Hja8ow

深度人脸表情识别技术综述，没有比这更全的了

https://mp.weixin.qq.com/s/YlrWHDPIPzN4dQO2vo4DjA

人脸颜值研究综述

https://mp.weixin.qq.com/s/d3aaN4vGAtMfLqOpU9oC6w

香港中文大学助理教授吕健勤：面向人脸分析的深度学习方法

https://mp.weixin.qq.com/s/wcKUURsYiEYVzn7qTMVPag

格灵深瞳：人脸识别最新进展以及工业级大规模人脸识别实践探讨

https://mp.weixin.qq.com/s/Cy5gxHl_Z4v7BX0k8SDNSQ

AI人像美妆算法初识

https://mp.weixin.qq.com/s/Ht8kFTgIWASusfSUQqoaJA

人脸表情识别研究

https://mp.weixin.qq.com/s/qUFjfsDtTQfAEjBCuoWHew

人脸脸型分类研究现状

https://mp.weixin.qq.com/s/5vx1FGgSOhF71FwHeUwaAQ

清华&商汤开源CVPR2018超高精度人脸对齐算法LAB

