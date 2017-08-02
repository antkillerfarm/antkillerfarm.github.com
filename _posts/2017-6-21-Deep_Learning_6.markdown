---
layout: post
title:  深度学习（六）——目标检测, RCNN
category: theory 
---

# CNN进化史

## SqueezeNet（续）

SqueezeNet最大的创新点在于使用Fire Module替换大尺寸的卷积层。

![](/images/article/SqueezeNet.png)

上图是Fire Module的结构示意图。它采用squeeze层+expand层两个小卷积层，替换了AlexNet的大尺寸卷积层。其中，$$N_{squeeze}<N_{expand}$$，N表示每层的卷积个数。

这里需要特别指出的是：expand层采用了2种不同尺寸的卷积，这也是当前设计的一个趋势。

这个趋势在GoogleNet中已经有所体现，在ResNet中也间接隐含。

![](/images/article/expand_ResNet.png)

上图是ResNet的展开图，可见展开之后的ResNet，实际上等效于一个多尺寸交错混编的复杂卷积网。其思路和GoogleNet实际上是一致的。

参见：

http://blog.csdn.net/xbinworld/article/details/50897870

最新SqueezeNet模型详解，CNN模型参数降低50倍，压缩461倍！

http://www.jianshu.com/p/8e269451795d

神经网络瘦身：SqueezeNet

http://blog.csdn.net/shenxiaolu1984/article/details/51444525

超轻量级网络SqueezeNet算法详解

## 参考

http://mp.weixin.qq.com/s/ZKMi4gRfDRcTxzKlTQb-Mw

计算机视觉识别简史：从AlexNet、ResNet到Mask RCNN

http://mp.weixin.qq.com/s/kbHzA3h-CfTRcnkViY37MQ

详解CNN五大经典模型:Lenet，Alexnet，Googlenet，VGG，DRL

https://zhuanlan.zhihu.com/p/22094600

Deep Learning回顾之LeNet、AlexNet、GoogLeNet、VGG、ResNet

http://www.leiphone.com/news/201609/303vE8MIwFC7E3DB.html

Google最新开源Inception-ResNet-v2，借助残差网络进一步提升图像分类水准

https://mp.weixin.qq.com/s/x3bSu9ecl3dldCbvS1rT1g

站在巨人的肩膀上，深度学习的9篇开山之作

http://mp.weixin.qq.com/s/2TUw_2d36uFAiJTkvaaqpA

解读Keras在ImageNet中的应用：详解5种主要的图像识别模型

https://zhuanlan.zhihu.com/p/27642620

YJango的卷积神经网络——介绍

https://www.zybuluo.com/coolwyj/note/202469

ImageNet Classification with Deep Convolutional Neural Networks

http://simtalk.cn/2016/09/20/AlexNet/

AlexNet简介

http://simtalk.cn/2016/09/12/CNNs/

CNN简介

http://www.cnblogs.com/Allen-rg/p/5833919.html

GoogLeNet学习心得

# 目标检测

## 概述

object detection是计算机视觉的一个重要的分支。类似的分支还有目标分割、目标识别和目标跟踪。

以下摘录自Sensetime CTO曹旭东的解读：

传统方法使用滑动窗口的框架，把一张图分解成几百万个不同位置不同尺度的子窗口，针对每一个窗口使用分类器判断是否包含目标物体。传统方法针对不同的类别的物体，一般会设计不同的特征和分类算法，比如人脸检测的经典算法是**Harr特征+Adaboosting分类器**；行人检测的经典算法是**HOG(histogram of gradients)+Support Vector Machine**；一般性物体的检测的话是**HOG特征+DPM(deformable part model)的算法**。

基于深度学习的物体检测的经典算法是RCNN系列：RCNN，fast RCNN(Ross Girshick)，faster RCNN(少卿、凯明、孙剑、Ross)。这三个工作的核心思想是分别是：使用更好的CNN模型判断候选区域的类别；复用预计算的sharing feature map加快模型训练和物体检测的速度；进一步使用sharing feature map大幅提高计算候选区域的速度。其实基于深度学习的物体检测也可以看成对海量滑动窗口分类，只是用全卷积的方式。

RCNN系列算法还是将物体检测分为两个步骤。现在还有一些工作是端到端(end-to-end)的物体检测，比如说YOLO(You Only Look Once: Unified, Real-Time Object Detection)和SSD(SSD: Single Shot MultiBox Detector)这样的算法。这两个算法号称和faster RCNN精度相似但速度更快。物体检测正负样本极端非均衡，two-stage cascade可以更好的应对非均衡。端到端学习是否可以超越faster RCNN还需要更多研究实验。

参考：

https://www.zhihu.com/question/34223049

从近两年的CVPR会议来看，目标检测的研究方向是怎么样的？

https://zhuanlan.zhihu.com/p/21533724

对话CVPR2016：目标检测新进展

https://mp.weixin.qq.com/s/r9tXvKIN-eqKW_65yFyOew

谷歌开源TensorFlow Object Detection API

https://mp.weixin.qq.com/s/-PeXMU_gkcT5YnMcLoaKag

CVPR清华大学研究，高效视觉目标检测框架RON

https://mp.weixin.qq.com/s/_cOuhToH8KvZldNfraumSQ

什么促使了候选目标的有效检测？

## 进化史

DPM(2007)->RCNN(2014)->Fast RCNN->Faster RCNN

![](/images/article/rcnn_2.png)

参考：

http://blog.csdn.net/ttransposition/article/details/12966521

DPM(Deformable Parts Model)--原理

# RCNN

《深度学习（五）》中提到的AlexNet、VGG、GoogleNet主要用于图片分类。而这里介绍的RCNN(Regions with CNN)主要用于目标检测。

## 车牌识别的另一种思路

在介绍RCNN之前，我首先介绍一下2013年的一个车牌识别项目的解决思路。

车牌识别差不多是深度学习应用到CV领域之前，CV领域少数几个达到实用价值的应用之一。国内在2010～2015年前后，有许多公司都做过类似的项目。其产品更是随处可见，很多停车场已经利用该技术，自动识别车辆信息。

车牌识别的难度不高——无论是目标字符集，还是目标字体，都很有限。但也有它的技术难点：

1.计算资源有限。通常就是PC，甚至嵌入式设备，不可能用大规模集群来计算。

2.有实时性的要求，通常处理时间不超过3s。

因此，如何快速的在图片中找到车牌所在区域，就成为了关键问题。

常规的做法，通常是根据颜色、形状找到车牌所在区域，但鲁棒性不佳。后来，有个同事提出了改进方法：

1.在整个图片中，基于haar算子，寻找疑似数字的区域。

2.将数字聚集的区域设定为疑似车牌所在区域。

3.投入更大运算量，以识别车牌上的文字。（这一步是常规做法。）

## RCNN的基本原理

RCNN是Ross Girshick于2014年提出的深度模型。

>注：Ross Girshick（网名：rbg），芝加哥大学博士（2012），Facebook研究员。他和何恺明被誉为CV界深度学习的**双子新星**。   
>个人主页：http://www.rossgirshick.info/

论文：

《Rich feature hierarchies for accurate object detection and semantic segmentation》

RCNN相对传统方法的改进：

**速度**：经典的目标检测算法使用滑动窗法依次判断所有可能的区域。RCNN则(采用Selective Search方法)预先提取一系列较可能是物体的候选区域，之后仅在这些候选区域上(采用CNN)提取特征，进行判断。

**训练集**：经典的目标检测算法在区域中提取人工设定的特征。RCNN则采用深度网络进行特征提取。

使用两个数据库：

一个较大的识别库（ImageNet ILSVC 2012）：标定每张图片中物体的类别。一千万图像，1000类。

一个较小的检测库（PASCAL VOC 2007）：标定每张图片中，物体的类别和位置，一万图像，20类。

RCNN使用识别库进行预训练得到CNN（有监督预训练），而后用检测库调优参数，最后在检测库上评测。

## RCNN算法的基本流程

![](/images/article/rcnn.png)

候选区域生成：一张图像生成1K~2K个候选区域 （采用Selective Search 方法）。

特征提取：对每个候选区域，使用深度卷积网络提取特征（CNN）。

类别判断：特征送入每一类的SVM 分类器，判别是否属于该类。

位置精修：使用回归器精细修正候选框位置。

参考：

https://zhuanlan.zhihu.com/p/23006190

RCNN-将CNN引入目标检测的开山之作

https://zhuanlan.zhihu.com/p/24774302

SPPNet-引入空间金字塔池化改进RCNN

https://zhuanlan.zhihu.com/p/24780395

Fast R-CNN

https://zhuanlan.zhihu.com/p/24916624

Faster R-CNN

https://zhuanlan.zhihu.com/p/24916786

图解YOLO

https://zhuanlan.zhihu.com/p/24954433

SSD

https://zhuanlan.zhihu.com/p/25167153

YOLO2

https://www.zhihu.com/question/35887527

如何评价rcnn、fast-rcnn和faster-rcnn这一系列方法？

http://blog.csdn.net/tangwei2014/article/details/50915317

论文阅读笔记：You Only Look Once: Unified, Real-Time Object Detection

http://blog.csdn.net/shenxiaolu1984/article/details/51066975

RCNN算法详解

http://blog.csdn.net/shenxiaolu1984/article/details/51036677

Fast RCNN算法详解

http://blog.csdn.net/shenxiaolu1984/article/details/51152614

Faster RCNN算法详解

https://mp.weixin.qq.com/s/XorPkuIdhRNI1zGLwg-55A

斯坦福新深度学习系统 NoScope：视频对象检测快1000倍

https://mp.weixin.qq.com/s/XbgmLmlt5X4TX5CP59gyoA

目标检测算法精彩集锦

https://mp.weixin.qq.com/s/BgTc1SE2IzNH27OC2P2CFg

CVPR-I

https://mp.weixin.qq.com/s/qMdnp9ZdlYIja2vNEKuRNQ

CVPR—II

https://mp.weixin.qq.com/s/tc1PsIoF1RN1sx_IFPmtWQ

CVPR—III

https://mp.weixin.qq.com/s/bpCn2nREHzazJYq6B9vMHg

目标识别算法的进展

https://mp.weixin.qq.com/s/YzxaS4KQmpbUSnyOwccn4A

基于深度学习的目标检测技术进展与展望

https://mp.weixin.qq.com/s/VKQufVUQ3TP5m7_2vOxnEQ

通过Faster R-CNN实现当前最佳的目标计数

## YOLO

YOLO: Real-Time Object Detection，是一个基于神经网络的实时对象检测软件。

官网：

https://pjreddie.com/darknet/yolo/

参考：

https://mp.weixin.qq.com/s/n51XtGAsaDDAatXYychXrg

YOLO比R-CNN快1000倍，比Fast R-CNN快100倍的实时对象检测！

## SSD

论文：

《SSD: Single Shot MultiBox Detector》

参考：

http://www.jianshu.com/p/ebebfcd274e6

Caffe-SSD 训练自己的数据集教程



