---
layout: post
title:  深度学习（十一）——YOLO, SSD, Ultra Deep Network
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

4.4-Step Alternating Training

这是作者发布的源代码中采用的方法。

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

**Step 2**：CNN提取特征和预测：卷积部分负责提特征。全连接部分负责预测：

a) $$7\times 7\times 2=98$$个bounding box(bbox) 的坐标$$x_{center},y_{center},w,h$$和是否有物体的confidence。

b) $$7\times 7=49$$个cell所属20个物体的概率。

![](/images/article/yolo_2.png)

**Step 3**：过滤bbox（通过NMS）。

![](/images/article/yolo_3.png)

上图是YOLO的网络结构图。从表面看，YOLO的输出只有一个，似乎比Faster RCNN两个输出少一个，然而这个输出本身，实际上要复杂的多。

YOLO的输出是一个$$7\times 7\times 30$$的tensor，其中$$7\times 7$$对应图片分割的7*7网格。30表明每个网格对应一个30维的向量。

![](/images/article/yolo_4.png)

上图是这个30维向量的编码方式。

## YOLOv2

YOLOv2又名YOLO9000，是Redmon 2016年12月提出的一个YOLO改进版本。

论文：

《YOLO9000: Better, Faster, Stronger》

YOLOv2对于输出向量的编码方式进行了改进，如下图所示：

![](/images/article/yolov2.png)

参考：

https://zhuanlan.zhihu.com/p/24916786

图解YOLO

https://mp.weixin.qq.com/s/n51XtGAsaDDAatXYychXrg

YOLO比R-CNN快1000倍，比Fast R-CNN快100倍的实时对象检测！

http://blog.csdn.net/tangwei2014/article/details/50915317

论文阅读笔记：You Only Look Once: Unified, Real-Time Object Detection

https://mp.weixin.qq.com/s/Wqj6EM33p-rjPIHnFKtmCw

计算机是怎样快速看懂图片的：比R-CNN快1000倍的YOLO算法

https://zhuanlan.zhihu.com/p/25167153

YOLO2

http://lanbing510.info/2017/08/28/YOLO-SSD.html

目标检测之YOLO，SSD

http://lanbing510.info/2017/09/04/YOLOV2.html

目标检测之YOLOv2

# SSD

SSD是Wei Liu于2016年提出的算法。

论文：

《SSD: Single Shot MultiBox Detector》

代码：

https://github.com/weiliu89/caffe

>Wei Liu，南京大学本科（2009）+北卡罗莱娜大学博士（在读）。   
>个人主页：   
>http://www.cs.unc.edu/~wliu/

参考：

http://www.jianshu.com/p/ebebfcd274e6

Caffe-SSD训练自己的数据集教程

https://zhuanlan.zhihu.com/p/24954433

SSD

http://blog.csdn.net/zy1034092330/article/details/72862030

SSD详解

http://blog.csdn.net/jesse_mx/article/details/74011886

SSD: Single Shot MultiBox Detector模型fine-tune和网络架构

## ENet

https://github.com/TimoSaemann/ENet

《ENet: A Deep Neural Network Architecture for Real-Time Semantic Segmentation》

http://blog.csdn.net/zijinxuxu/article/details/67638290

# Ultra Deep Network

## FractalNet

论文：

《FractalNet: Ultra-Deep Neural Networks without Residuals》

![](/images/article/FractalNet.png)

## Resnet in Resnet

论文：

《Resnet in Resnet: Generalizing Residual Architectures》

![](/images/article/RiR.png)

## Highway

论文：

《Training Very Deep Networks》

![](/images/article/highway.png)

Resnet对于残差的跨层传递是无条件的，而Highway则是有条件的。这种条件开关被称为gate，它也是由网络训练得到的。

