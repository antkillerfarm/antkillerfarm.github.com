---
layout: post
title:  深度学习（十三）——YOLOv2, 语义分割
category: theory 
---

# YOLOv2

面对SSD的攻势，pjreddie不甘示弱，于2016年12月提出了YOLOv2（又名YOLO9000）。YOLOv2对YOLO做了较多改进，实际上更像是SSD的升级版。

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

# 其它目标检测网络

## A-Fast-RCNN

A-Fast-RCNN首次将对抗学习引入到了目标检测领域，idea是非常创新的。

http://blog.csdn.net/jesse_mx/article/details/72955981

A-Fast-RCNN论文笔记

## R-FCN

FCN在目标检测领域的应用。

http://blog.csdn.net/zijin0802034/article/details/53411041

R-FCN: Object Detection via Region-based Fully Convolutional Networks

## G-CNN

G-CNN是MaryLand大学的工作，论文主要的思路也是消除region proposal，和YOLO，SSD不同，G-CNN的工作借鉴了迭代的想法，把边框检测等价于找到初始边框到最终目标的一个路径。但是使用one-step regression不能处理这个非线性的过程，所以作者采用迭代的方法逐步接近最终的目标。

http://blog.csdn.net/zijin0802034/article/details/53535647

G-CNN: an Iterative Grid Based Object Detector

# 语义分割

https://zhuanlan.zhihu.com/p/21824299

从特斯拉到计算机视觉之「图像语义分割」

https://zhuanlan.zhihu.com/SemanticSegmentation

一个语义分割的专栏

https://zhuanlan.zhihu.com/p/22308032

图像语义分割之FCN和CRF

https://zhuanlan.zhihu.com/p/25515361

图像语义分割之特征整合和结构预测

https://zhuanlan.zhihu.com/p/27794982

语义分割中的深度学习方法全解：从FCN、SegNet到各代DeepLab

https://mp.weixin.qq.com/s/BWc8nRbOF1IfwbBtmLnhnA

从全连接层到大型卷积核：深度学习语义分割全指南

https://mp.weixin.qq.com/s/cANlqQAI-A2mC9vnd3imQA

Instance-Aware图像语义分割

https://mp.weixin.qq.com/s/v_TLYYq6cFWuwR9tXM8m-A

如何通过CRF-RNN模型实现图像语义分割任务

https://mp.weixin.qq.com/s/ceCC7Q6yr0QKESeZXi6lWQ

堆叠解卷积网络实现图像语义分割顶尖效果

# FCN

http://www.cnblogs.com/gujianhan/p/6030639.html

全卷积网络FCN详解

# DeepLab

《Semantic Image Segmentation with Deep Convolutional Nets and Fully Connected CRFs》

《DeepLab: Semantic Image Segmentation with Deep Convolutional Nets, Atrous Convolution, and Fully Connected CRFs》

《Rethinking Atrous Convolution for Semantic Image Segmentation》

>Liang-Chieh (Jay) Chen，台湾国立交通大学本科（2004年）+密歇根大学硕士（2010年）+UCLA博士（2015年）。现为Google研究员。   
>个人主页：   
>http://liangchiehchen.com/

# ENet

![](/images/article/image_enet.png)

论文：

《ENet: A Deep Neural Network Architecture for Real-Time Semantic Segmentation》

代码：

https://github.com/TimoSaemann/ENet

参考：

http://blog.csdn.net/zijinxuxu/article/details/67638290

论文中文版blog

# OpenPose

![](/images/article/openpose.png)

论文：

《Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields》

《Hand Keypoint Detection in Single Images using Multiview Bootstrapping》

《Convolutional pose machines》

代码：

https://github.com/CMU-Perceptual-Computing-Lab/openpose




