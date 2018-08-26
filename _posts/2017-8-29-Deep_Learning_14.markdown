---
layout: post
title:  深度学习（十四）——Instance Normalization, IBN-Net, Softmax详解, 目标检测
category: DL 
---

# Instance Normalization

Instance Normalization主要用于CV领域。

论文：

《Instance Normalization: The Missing Ingredient for Fast Stylization》

首先我们列出对图片Batch Normalization的公式：

$$y_{tijk}=\frac{x_{tijk}-\mu_i}{\sqrt{\sigma_i^2+\epsilon}}, \mu_i=\frac{1}{HWT}\sum_{t=1}^T \sum_{l=1}^W \sum_{m=1}^Hx_{tilm}, \sigma_i^2=\frac{1}{HWT}\sum_{t=1}^T \sum_{l=1}^W \sum_{m=1}^H(x_{tilm}-m\mu_i)^2$$

其中，T为图片数量，i为通道，j、k为图片的宽、高。

Instance Normalization的公式：

$$y_{tijk}=\frac{x_{tijk}-\mu_{ti}}{\sqrt{\sigma_{ti}^2+\epsilon}}, \mu_{ti}=\frac{1}{HW} \sum_{l=1}^W \sum_{m=1}^Hx_{tilm}, \sigma_{ti}^2=\frac{1}{HW} \sum_{l=1}^W \sum_{m=1}^H(x_{tilm}-m\mu_{ti})^2$$

从中可以看出Instance Normalization实际上就是对一张图片的一个通道内的值进行归一化，因此又叫做对比度归一化（contrast normalization）。

参考：

http://www.jianshu.com/p/d77b6273b990

论文中文版

# IBN-Net

IBN-Net是汤晓鸥小组的新作（2018.7）。

![](/images/article/rcnn_2.png)

与BN相比，IN有两个主要的特点：第一，它不是用训练批次来将图像特征标准化，而是用单个样本的统计信息；第二，IN能将同样的标准化步骤既用于训练，又用于推断。

潘新钢等发现，IN和BN的核心区别在于，IN学习到的是不随着颜色、风格、虚拟性/现实性等外观变化而改变的特征，而要保留与内容相关的信息，就要用到BN。

论文：

《Two at Once: Enhancing Learning and Generalization Capacities via IBN-Net》

参考：

https://mp.weixin.qq.com/s/LVL90n4--WPgFLMQ-Gnf6g

汤晓鸥为CNN搓了一颗大力丸

https://mp.weixin.qq.com/s/6hNpgffEnUTkNAfrPgKHkA

IBN-Net：打开Domain Generalization的新方式

# Softmax详解

首先给出Softmax function的定义:

$$y_c=\zeta(\textbf{z})_c = \dfrac{e^{z_c}}{\sum_{d=1}^C{e^{z_d}}} \text{  for } c=1, \dots, C$$

从中可以很容易的发现，如果$$z_c$$的值过大，朴素的直接计算会上溢出或下溢出。

解决办法：

$$z_c\leftarrow z_c-a,a=\max\{z_1,\dots,z_C\}$$

证明：

$$\zeta(\textbf{z-a})_c = \dfrac{e^{z_c}\cdot e^{-a}}{\sum_{d=1}^C{e^{z_d}\cdot e^{-a}}} = \dfrac{e^{z_c}}{\sum_{d=1}^C{e^{z_d}}} = \zeta(\textbf{z})_c$$

Softmax的损失函数是cross entropy loss function：

$$\xi(X, Y) = \sum_{i=1}^n \xi(\textbf{t}_i, \textbf{y}_i) = - \sum_{i=1}^n \sum_{i=c}^C t_{ic} \cdot \log(y_{ic})$$

Softmax的反向传播算法：

$$\begin{align}
\dfrac{\partial\xi}{\partial z_i} &= - \sum_{j=1}^C \dfrac{\partial t_j \log(y_j)}{\partial z_i} \\
&= - \sum_{j=1}^C t_j \dfrac{\partial \log(y_j)}{\partial z_i} \\
&= - \sum_{j=1}^C t_j \dfrac{1}{y_j} \dfrac{\partial y_j}{\partial z_i} \\
&= - \dfrac{t_i}{y_i} \dfrac{\partial y_i}{\partial z_i} - \sum_{j \neq i}^C \dfrac{t_j}{y_j} \dfrac{\partial y_j}{\partial z_i} \\
&= - \dfrac{t_i}{y_i} y_i(1-y_i) - \sum_{j \neq i}^{C} \dfrac{t_j}{y_j}(-y_jy_j) \\
&= -t_i + t_iy_i + \sum_{j \neq i}^{C} t_jy_i \\
&= -t_i + \sum_{j=1}^C t_jy_i \\
&= -t_i + y_i \sum_{j=1}^C t_j \\
&= y_i - t_i
\end{align}$$

参考：

https://mp.weixin.qq.com/s/2xYgaeLlmmUfxiHCbCa8dQ

softmax函数计算时候为什么要减去一个最大值？

http://shuokay.com/2016/07/20/softmax-loss/

Softmax输出及其反向传播推导

https://mp.weixin.qq.com/s/HTIgKm8HuZZ_-lIQ3nIFhQ

浅入深出之大话SoftMax

https://mp.weixin.qq.com/s/XBK7T1P7z3rm3o-3BDNeOA

三分钟带你对Softmax划重点

https://mp.weixin.qq.com/s/vhvXsSsEHPVjJGqtCOOwLw

Softmax和交叉熵的深度解析和Python实现

# 目标检测

## 概述

object detection是计算机视觉的一个重要的分支。类似的分支还有目标分割、目标识别和目标跟踪。

以下摘录自Sensetime CTO曹旭东的解读：

传统方法使用滑动窗口的框架，把一张图分解成几百万个不同位置不同尺度的子窗口，针对每一个窗口使用分类器判断是否包含目标物体。传统方法针对不同的类别的物体，一般会设计不同的特征和分类算法，比如人脸检测的经典算法是**Harr特征+Adaboosting分类器**；行人检测的经典算法是**HOG(histogram of gradients)+Support Vector Machine**；一般性物体的检测的话是**HOG特征+DPM(deformable part model)的算法**。

基于深度学习的物体检测的经典算法是RCNN系列：RCNN，fast RCNN(Ross Girshick)，faster RCNN(少卿、凯明、孙剑、Ross)。这三个工作的核心思想是分别是：使用更好的CNN模型判断候选区域的类别；复用预计算的sharing feature map加快模型训练和物体检测的速度；进一步使用sharing feature map大幅提高计算候选区域的速度。其实基于深度学习的物体检测也可以看成对海量滑动窗口分类，只是用全卷积的方式。

RCNN系列算法还是将物体检测分为两个步骤。现在还有一些工作是端到端(end-to-end)的物体检测，比如说YOLO(You Only Look Once: Unified, Real-Time Object Detection)和SSD(SSD: Single Shot MultiBox Detector)这样的算法。这两个算法号称和faster RCNN精度相似但速度更快。物体检测正负样本极端非均衡，two-stage cascade可以更好的应对非均衡。端到端学习是否可以超越faster RCNN还需要更多研究实验。

## 进化史

DPM(2007)->RCNN(2014)->Fast RCNN->Faster RCNN

![](/images/article/rcnn_2.png)

![](/images/article/rcnn_4.jpg)

参考：

http://blog.csdn.net/ttransposition/article/details/12966521

DPM(Deformable Parts Model)--原理

## CV实践的难点

从理论上说，无论是传统CV，还是新近崛起的DL CV，其本质都是通过比对目标图片和训练图片的相似度，从而得到识别的结果。

CV的输入一般是由像素组成的矩阵。相比其他领域的数据挖掘而言，CV的实践难点主要包括：

1.视角的改变。照相机的移动会导致像素矩阵发生平移、旋转等变换。

2.光照影响。同一物体在不同光照条件下的影像有所不同。

3.形变。典型的例子是动物的运动，会导致外观的改变。

4.遮挡。待识别物体通常不是完全可见的。

5.背景。雪地、沙滩等不同场景，会影响物体的识别。

6.同类差异。比如各种猫都是猫，但它们的外观有细微的差异。

![](/images/article/cv_problem.png)

## 参考

https://github.com/amusi/awesome-object-detection

目标检测最全论文集锦

https://www.zhihu.com/question/34223049

从近两年的CVPR会议来看，目标检测的研究方向是怎么样的？

https://zhuanlan.zhihu.com/p/21533724

对话CVPR2016：目标检测新进展

https://zhuanlan.zhihu.com/p/36088972

基于深度学习的目标检测算法综述

https://mp.weixin.qq.com/s/r9tXvKIN-eqKW_65yFyOew

谷歌开源TensorFlow Object Detection API

https://mp.weixin.qq.com/s/_cOuhToH8KvZldNfraumSQ

什么促使了候选目标的有效检测？

https://mp.weixin.qq.com/s/LAy1LKGj5HOh_e9jPgvfQw

视觉目标检测和识别之过去，现在及可能

https://mp.weixin.qq.com/s/ZHRP5xnQxex7lQJsCxwblA

深度学习目标检测的主要问题和挑战！

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

https://mp.weixin.qq.com/s/JPCQqyzR8xIUyAdk_RI5dA

RCNN, Fast-RCNN, Faster-RCNN那些你必须知道的事！

https://www.zhihu.com/question/35887527

如何评价rcnn、fast-rcnn和faster-rcnn这一系列方法？

http://blog.csdn.net/messiran10/article/details/49132053

Caffe matlab之基于Alex network的特征提取

https://mp.weixin.qq.com/s/YovhKYeGGLqSxxSqMNsbKg

基于深度学习的目标检测学习总结

https://mp.weixin.qq.com/s/nGSaQXm8AczYodtmHD1qNA

深度学习目标检测模型全面综述：Faster R-CNN、R-FCN和SSD

https://mp.weixin.qq.com/s/c2oMJfE95I1ciEtvdTlb4A

完全脱离预训练模型的目标检测方法

https://mp.weixin.qq.com/s/NV2hWofOCractLt45-wI1A

山世光：基于深度学习的目标检测技术进展与展望

https://mp.weixin.qq.com/s/zJ3EN175_9num2OknVvnyA

邬书哲：物体检测算法的革新与传承

https://mp.weixin.qq.com/s/6rSeJOqbKyrDj3FpS8J5eg

黄李超讲物体检测

https://mp.weixin.qq.com/s/JjsAnB_OxKS1Af9XAtw5sA

一文带你读懂深度学习框架下的目标检测

https://mp.weixin.qq.com/s/dcrBQ-t3tLOTouEyofOBxg

间谍卫星：利用卷积神经网络对卫星影像进行多尺度目标检测

https://mp.weixin.qq.com/s/66yXsRIeZdHfoZkJkci24w

地平线黄李超开讲：深度学习和物体检测！

http://blog.csdn.net/zhang11wu4/article/details/53967688

目标检测最新方法介绍

https://mp.weixin.qq.com/s/ZrZtGBxVOZmexDMw_S_Orw

TensorFlow深度学习目标检测模型及源码架构解析

https://mp.weixin.qq.com/s/HPzQST8cq5lBhU3wnz7-cg

R-FCN每秒30帧实时检测3000类物体，马里兰大学Larry Davis组最新目标检测工作

https://mp.weixin.qq.com/s/A9F3UsITUaNvR69roHa0mg

结合单阶段和两阶段目标检测的优势：基于单次精化神经网络的目标检测方法

https://mp.weixin.qq.com/s/xNIPdmIJiqDg_ruwHPLEDg

深度学习目标检测从入门到精通：第一篇

http://mp.weixin.qq.com/s/kDoogc5yIeX6D4_YWC5YQA

后RCNN时代的物体检测及实例分割进展

https://mp.weixin.qq.com/s/rUMAaIJ14eEXbRmssnUK4w

基于区域的目标检测——细粒度

https://mp.weixin.qq.com/s/Oy7YAp1NoVICQZqXRz0uVg

基于深度学习的目标检测综述(一）

https://mp.weixin.qq.com/s/5zE78EU_NdV5ZeW5t1yV7A

从RCNN到SSD，这应该是最全的一份目标检测算法盘点

https://mp.weixin.qq.com/s/a1GFDzcR98b7pyJR1Gn-TA

深度学习之目标检测网络学习总结（from RCNN to YOLO V3）

https://mp.weixin.qq.com/s/_kzPi1QgwqvT4glRI-FCzg

深度学习目标检测指南：如何过滤不感兴趣的分类及添加新分类？

# RCNN

《深度学习（五）》中提到的AlexNet、VGG、GoogleNet主要用于图片分类。而这里介绍的RCNN(Regions with CNN)主要用于目标检测。

## 车牌识别的另一种思路

在介绍RCNN之前，我首先介绍一下2013年的一个车牌识别项目的解决思路。

车牌识别差不多是深度学习应用到CV领域之前，CV领域少数几个达到实用价值的应用之一。国内在2010～2015年前后，有许多公司都做过类似的项目。其产品更是随处可见，很多停车场已经利用该技术，自动识别车辆信息。

车牌识别的难度不高——无论是目标字符集，还是目标字体，都很有限。但也有它的技术难点：

1.计算资源有限。通常就是PC，甚至嵌入式设备，不可能用大规模集群来计算。

2.有实时性的要求，通常处理时间不超过3s。

