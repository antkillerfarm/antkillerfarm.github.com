---
layout: post
title:  深度学习（十四）——语义分割, FCN
category: theory 
---

# YOLOv2（续）

## Stronger

### 联合训练

作者提出了一种在分类数据集和检测数据集上联合训练的机制：

1.使用检测数据集的图片去学习检测相关的信息，例如bounding box坐标预测，是否包含物体以及属于各个物体的概率。

2.使用仅有类别标签的分类数据集图片去扩展可以检测的种类。

这种方法有一些难点需要解决。检测数据集只有常见物体和抽象标签（不具体），例如 “狗”，“船”。分类数据集拥有广而深的标签范围（例如ImageNet就有一百多类狗的品种，包括 “Norfolk terrier”, “Yorkshire terrier”, and “Bedlington terrier”等. ）。必须按照某种一致的方式来整合两类标签。

大多数分类的方法采用softmax层，考虑所有可能的种类计算最终的概率分布。但是softmax假设类别之间互不包含，但是整合之后的数据是类别是有包含关系的，例如 “Norfolk terrier” 和 “dog”。所以整合数据集没法使用这种方式（softmax 模型），

作者最后采用一种不要求互不包含的多标签模型（multi-label model）来整合数据集。

### Hierarchical classiﬁcation（层次式分类）

ImageNet的标签参考WordNet（一种结构化概念及概念之间关系的语言数据库）。例如：

![](/images/article/WordNet.png)

很多分类数据集采用扁平化的标签。而整合数据集则需要结构化标签。

WordNet是一个有向图结构（而非树结构），因为语言是复杂的（例如“dog”既是“canine”又是“domestic animal”），为了简化问题，作者从ImageNet的概念中构建了一个层次树结构（hierarchical tree）来代替图结构方案。这也就是作者论文中提到的WordTree。

WordTree的细节，更偏NLP一些，这里不再赘述。

## 参考

https://zhuanlan.zhihu.com/p/25167153

YOLO2

http://blog.csdn.net/jesse_mx/article/details/53925356

YOLOv2论文笔记

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

## 概述

Semantic segmentation是图像理解的基石性技术，在自动驾驶系统（具体为街景识别与理解）、无人机应用（着陆点判断）以及穿戴式设备应用中举足轻重。

我们都知道，图像是由许多像素（Pixel）组成，而「语义分割」顾名思义就是将像素按照图像中表达语义含义的不同进行分组（Grouping）／分割（Segmentation）。

![](/images/article/image_enet.png)

上图是语义分割网络ENet的实际效果图。

## 参考

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

https://mp.weixin.qq.com/s/4BvvwV11f9MrrYyLwUrX9w

还在用ps抠图抠瞎眼？机器学习通用背景去除产品诞生记

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

# 视频目标分割

http://mp.weixin.qq.com/s/pGrzmq5aGoLb2uiJRYAXVw

一文概览视频目标分割


