---
layout: post
title:  深度学习（十一）——YOLO, SSD
category: theory 
---

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



