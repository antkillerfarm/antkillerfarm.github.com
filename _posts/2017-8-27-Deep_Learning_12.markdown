---
layout: post
title:  深度学习（十二）——YOLO, SSD, YOLOv2
category: theory 
---

# Faster R-CNN（续）

## 定义损失函数

对于每个anchor，首先在后面接上一个二分类softmax，有2个score 输出用以表示其是一个物体的概率与不是一个物体的概率 ($$p_i$$)。这个概率也可以理解为前景与后景，或者物体和背景的概率。

然后再接上一个bounding box的regressor 输出代表这个anchor的4个坐标位置（$$t_i$$），因此RPN的总体Loss函数可以定义为 ：

$$L(\{p_i\},\{t_i\})=\frac{1}{N_{cls}}\sum_iL_{cls}(p_i,p_i^*)+\lambda \frac{1}{N_{reg}}\sum_ip_i^*L_{reg}(t_i,t_i^*)$$

该公式的含义和计算都比较复杂，这里不再赘述。

## RPN和Fast R-CNN协同训练

我们知道，如果是分别训练两种不同任务的网络模型，即使它们的结构、参数完全一致，但各自的卷积层内的卷积核也会向着不同的方向改变，导致无法共享网络权重，论文作者提出了几种可能的方式。

### Alternating training

此方法其实就是一个不断迭代的训练过程，既然分别训练RPN和Fast-RCNN可能让网络朝不同的方向收敛，a)那么我们可以先独立训练RPN，然后用这个RPN的网络权重对Fast-RCNN网络进行初始化并且用之前RPN输出proposal作为此时Fast-RCNN的输入训练Fast R-CNN。b) 用Fast R-CNN的网络参数去初始化RPN。之后不断迭代这个过程，即循环训练RPN、Fast-RCNN。

![](/images/article/alternating_training.png)

### Approximate joint training or Non-approximate training

这两种方式，不再是串行训练RPN和Fast-RCNN，而是尝试把二者融入到一个网络内训练。融合方式和上面的Faster R-CNN结构图类似。细节不再赘述。

### 4-Step Alternating Training

这是作者发布的源代码中采用的方法。

第一步：用ImageNet模型初始化，独立训练一个RPN网络；

第二步：仍然用ImageNet模型初始化，但是使用上一步RPN网络产生的proposal作为输入，训练一个Fast-RCNN网络，至此，两个网络每一层的参数完全不共享；

第三步：使用第二步的Fast-RCNN网络参数初始化一个新的RPN网络，但是把RPN、Fast-RCNN共享的那些卷积层的learning rate设置为0，也就是不更新，仅仅更新RPN特有的那些网络层，重新训练，此时，两个网络已经共享了所有公共的卷积层；

第四步：仍然固定共享的那些网络层，把Fast-RCNN特有的网络层也加入进来，形成一个unified network，继续训练，fine tune Fast-RCNN特有的网络层，此时，该网络已经实现我们设想的目标，即网络内部预测proposal并实现检测的功能。

![](/images/article/4_Step_Alternating_Training.png)

## 总结

![](/images/article/faster_rcnn_p.png)

参考：

https://zhuanlan.zhihu.com/p/24916624

Faster R-CNN

http://blog.csdn.net/shenxiaolu1984/article/details/51152614

Faster RCNN算法详解

https://mp.weixin.qq.com/s/VKQufVUQ3TP5m7_2vOxnEQ

通过Faster R-CNN实现当前最佳的目标计数

# YOLO

YOLO: Real-Time Object Detection，是一个基于神经网络的实时对象检测软件。它的原理基于Joseph Chet Redmon 2016年的论文：

《You Only Look Once: Unified, Real-Time Object Detection》

这也是Ross Girshick去Facebook之后，参与的又一力作。

官网：

https://pjreddie.com/darknet/yolo/

>注：Joseph Chet Redmon，Middlebury College本科+华盛顿大学博士（在读）。网名：pjreddie。

## 概述

从R-CNN到Fast R-CNN一直采用的思路是proposal+分类（proposal提供位置信息，分类提供类别信息），这也被称作two-stage cascade。

YOLO不仅是end-to-end，而且还提供了另一种更为直接的思路：直接在输出层回归bounding box的位置和bounding box所属的类别(整张图作为网络的输入，把Object Detection的问题转化成一个Regression问题)。

![](/images/article/yolo.png)

上图是YOLO的大致流程：

**Step 1**：Resize成448*448，图片分割得到7*7网格(cell)。

**Step 2**：CNN提取特征和预测：卷积部分负责提特征。全连接部分负责预测。

a) $$7\times 7\times 2=98$$个bounding box(bbox) 的坐标$$x_{center},y_{center},w,h$$和是否有物体的confidence。

b) $$7\times 7=49$$个cell所属20个物体分类的概率。

![](/images/article/yolo_2.png)

![](/images/article/yolo_3.png)

上图是YOLO的网络结构图。从表面看，YOLO的输出只有一个，似乎比Faster RCNN两个输出少一个，然而这个输出本身，实际上要复杂的多。

YOLO的输出是一个$$7\times 7\times 30$$的tensor，其中$$7\times 7$$对应图片分割的7*7网格。30表明每个网格对应一个30维的向量。

![](/images/article/yolo_4.png)

上图是这个30维向量的编码方式：2个bbox+confidence是$$5\times 2=10$$维，20个物体分类的概率占其余20维。

总结一下，输出tersor的维度是：$$S\times S \times (B \times 5 + C)$$

这里的confidence代表了所预测的box中含有object的置信度和这个box预测的有多准两重信息：

$$\text{confidence} = \text{Pr}(Object) ∗ \text{IOU}_{pred}^{truth}$$

在loss函数设计方面，简单的把结果堆在一起，然后认为它们的重要程度都一样，这显然是不合理的，每个loss项之前的参数$$\lambda$$就是用来设定权重的。

**Step 3**：过滤bbox（通过NMS）。

![](/images/article/yolo_5.png)

上图是Test阶段的NMS的过程示意图。

## 参考

https://zhuanlan.zhihu.com/p/24916786

图解YOLO

https://mp.weixin.qq.com/s/n51XtGAsaDDAatXYychXrg

YOLO比R-CNN快1000倍，比Fast R-CNN快100倍的实时对象检测！

http://blog.csdn.net/tangwei2014/article/details/50915317

论文阅读笔记：You Only Look Once: Unified, Real-Time Object Detection

https://mp.weixin.qq.com/s/Wqj6EM33p-rjPIHnFKtmCw

计算机是怎样快速看懂图片的：比R-CNN快1000倍的YOLO算法

http://lanbing510.info/2017/08/28/YOLO-SSD.html

目标检测之YOLO，SSD

# SSD

SSD是Wei Liu于2016年提出的算法。

论文：

《SSD: Single Shot MultiBox Detector》

代码：

https://github.com/weiliu89/caffe

>Wei Liu，南京大学本科（2009）+北卡罗莱娜大学博士（在读）。   
>个人主页：   
>http://www.cs.unc.edu/~wliu/

YOLO有一些缺陷：每个网格只预测一个物体，容易造成漏检；对于物体的尺度相对比较敏感，对于尺度变化较大的物体泛化能力较差。

针对YOLO中的这些不足，SSD在这两方面都有所改进，同时兼顾了mAP和实时性的要求。其思路就是Faster R-CNN+YOLO，利用YOLO的思路和Faster R-CNN的anchor box的思想。

![](/images/article/ssd.png)

上图是SSD的网络结构图。其特点为：

1.采用VGG16的基础网络结构，使用前面的前5层。

2.使用Dilated convolution将fc6和fc7层转化成两个卷积层。

3.再额外增加了3个卷积层，和一个average pool层。不同层次的feature map分别用于default box的偏移以及不同类别得分的预测。

4.通过NMS得到最终的检测结果。

这些增加的卷积层的feature map的大小变化比较大，允许能够检测出不同尺度下的物体：在低层的feature map，感受野比较小，高层的感受野比较大，在不同的feature map进行卷积，可以达到多尺度的目的。

![](/images/article/ssd_2.png)

上图是从另一个角度观察SSD，可以看出SSD可检出8372个候选box。

![](/images/article/ssd_3.png)

和YOLO一样，卷积层的每个点都是一个vector，含义也和YOLO类似，只是分类的时候，多了一个背景的类别，所以就成了20+1类。

参考：

http://www.jianshu.com/p/ebebfcd274e6

Caffe-SSD训练自己的数据集教程

https://zhuanlan.zhihu.com/p/24954433

SSD

http://blog.csdn.net/zy1034092330/article/details/72862030

SSD详解

http://blog.csdn.net/jesse_mx/article/details/74011886

SSD模型fine-tune和网络架构

http://blog.csdn.net/u010167269/article/details/52563573

SSD论文阅读

http://blog.csdn.net/zijin0802034/article/details/53288773

另一个SSD论文阅读

http://www.lai18.com/content/24600342.html

还是一个SSD论文阅读

# YOLOv2

面对SSD的攻势，Redmon不甘示弱，于2016年12月提出了YOLOv2（又名YOLO9000）。YOLOv2对YOLO做了较多改进，实际上更像是SSD的升级版。

论文：

《YOLO9000: Better, Faster, Stronger》

实际上，论文的内容也正如标题所言，主要分为Better, Faster, Stronger三个部分。

## Better

### batch normalization

YOLOv2网络通过在每一个卷积层后添加batch normalization，极大的改善了收敛速度同时减少了对其它regularization方法的依赖（舍弃了dropout优化后依然没有过拟合），使得mAP获得了2%的提升。

### High Resolution Classifier

所有state-of-the-art的检测方法基本上都会使用ImageNet预训练过的模型（classifier）来提取特征，例如AlexNet输入图片会被resize到不足256 * 256，这导致分辨率不够高，给检测带来困难。所以YOLO(v1)先以分辨率224*224训练分类网络，然后需要增加分辨率到448*448，这样做不仅切换为检测算法也改变了分辨率。所以作者想能不能在预训练的时候就把分辨率提高了，训练的时候只是由分类算法切换为检测算法。

YOLOv2首先修改预训练分类网络的分辨率为448*448，在ImageNet数据集上训练10轮（10 epochs）。这个过程让网络有足够的时间调整filter去适应高分辨率的输入。然后fine tune为检测网络。mAP获得了4%的提升。

## Faster



## Stronger

YOLOv2对于输出向量的编码方式进行了改进，如下图所示：

![](/images/article/yolov2.png)

## 参考

https://zhuanlan.zhihu.com/p/25167153

YOLO2

http://blog.csdn.net/jesse_mx/article/details/53925356

YOLOv2 论文笔记

http://lanbing510.info/2017/09/04/YOLOV2.html

目标检测之YOLOv2

