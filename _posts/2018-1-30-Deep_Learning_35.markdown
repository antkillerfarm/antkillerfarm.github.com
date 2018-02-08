---
layout: post
title:  深度学习（三十五）——OpenPose, 深度目标跟踪, Mask R-CNN, Fast Image Processing
category: DL 
---

# OpenPose

OpenPose是一个实时多人关键点检测的库，基于OpenCV和Caffe编写。它是CMU的Yaser Sheikh小组的作品。

>Yaser Ajmal Sheikh，巴基斯坦信德省易司哈克工程科学与技术学院本科（2001年）+中佛罗里达大学博士（2006年）。现为CMU副教授。

![](/images/article/openpose.png)

OpenPose的使用效果如上图所示。

论文：

《Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields》

《Hand Keypoint Detection in Single Images using Multiview Bootstrapping》

《Convolutional pose machines》

官方代码（caffe）：

https://github.com/CMU-Perceptual-Computing-Lab/openpose

Tensorflow版本：

https://github.com/ildoonet/tf-pose-estimation

参考：

https://mp.weixin.qq.com/s/-EU4jTElNll9MQomjuqFXA

姿态估计相比Mask-RCNN提高8.2%，上海交大卢策吾团队开源AlphaPose

# 深度目标跟踪

## 概述

目标跟踪（object tracking）就是在连续的视频序列中，建立所要跟踪物体的位置关系，得到物体完整的运动轨迹。

目标跟踪分为单目标跟踪和多目标跟踪。本文如无特别指出，均指单目标跟踪。

通常的做法是：

1.在第1帧给一个bbox框住需要跟踪的物体。

2.在不借助重检测（re-detection）的情况下，尽可能长时间的跟住物体。

3.不能使用依赖外部特征的姿态估计（pose estimation）。

当然这是针对目标跟踪算法的要求，至于实际产品中，对象的重检测以及依赖外部特征的姿态估计都是必不可少的。

比如，自动驾驶领域的车辆跟踪，一般都会针对车辆的运动特点建立模型，以辅助目标跟踪。

## TB50 & TB100

这个领域最著名的数据集是吴毅提出的TB50 & TB100，50和100分别代表视频数量。有的论文也把它们称作OTB-2013和OTB-2015。

>吴毅，中国科学院自动化研究所博士（2009年）。南京审计大学副教授。

官网：

http://cvlab.hanyang.ac.kr/tracker_benchmark/

论文：

《Online object tracking: A benchmark》

《Object tracking benchmark》

上面的论文在TB数据集上比较了包括2012年及之前的29个顶尖的tracker，基本解决了统一标准的问题，因此也成为了目标跟踪领域的权威数据集。

《Object tracking: A survey》

这篇论文总结了2006年以前的目标跟踪算法。

## VOT

VOT竞赛数据集是另一个常用数据集。官网：

http://votchallenge.net/challenges.html

OTB包括25%的灰度序列，但VOT都是彩色序列，这也是造成很多颜色特征算法性能差异的原因。

## 目标跟踪的难点

![](/images/article/tracker_hard.png)

![](/images/article/tracker_hard_2.png)

## 参考

https://www.zhihu.com/question/26493945

计算机视觉中，目前有哪些经典的目标跟踪算法？

https://mp.weixin.qq.com/s/Wz-loMz1oOlxtm10gazQRg

目标检测（Object Detection）和目标跟踪（Object Tracking）的区别

https://zhuanlan.zhihu.com/p/22334661

深度学习在目标跟踪中的应用

https://mp.weixin.qq.com/s/3R8zaFUKFTvp0uE8lY44WA

首个应用残差学习的深度目标跟踪算法

https://zhuanlan.zhihu.com/p/27293523

目标跟踪领域进展报告

https://zhuanlan.zhihu.com/p/27335895

CVPR 2017目标跟踪相关论文

https://www.zhihu.com/question/59623472

基于深度学习的目标跟踪算法是否可能做到实时？

http://acsweb.ucsd.edu/~yuw176/report/vehicle.pdf

Monocular Vehicle Detection and Tracking

https://mp.weixin.qq.com/s/x7LoIN7mOJcZDY70GB_rLg

VOT Challenge 2017亚军北邮团队技术分享

https://zhuanlan.zhihu.com/p/32489557

深度学习的快速目标跟踪

# FlowNet

论文：

《FlowNet: Learning Optical Flow with Convolutional Networks》

参考：

http://www.cnblogs.com/zhang-yd/p/6511475.html

FlowNet

http://blog.csdn.net/hysteric314/article/details/50529804

神经光流网络——用卷积网络实现光流预测

http://geek.csdn.net/news/detail/129128

卷积神经网络（CNN）在无人驾驶中的应用

https://zhuanlan.zhihu.com/p/32663227

重新认识two stream的光流算法

http://blog.csdn.net/bea_tree/article/details/67049373

几分钟走进神奇的光流：FlowNet 2.0: Evolution of Optical Flow Estimation with Deep Networks

# SpyNet

论文：

《Optical Flow Estimation using a Spatial Pyramid Network》

代码：

https://github.com/anuragranj/spynet

参考：

http://www.cnblogs.com/wangxiaocvpr/p/7058617.html

论文笔记之：Optical Flow Estimation using a Spatial Pyramid Network

# Mask R-CNN

Mask R-CNN虽然挂着R-CNN的名头，但却是一个对象实例分割（不仅要分出对象的类别，连同一类对象的不同实例也要分出来）的NN。它是何恺明2017年的新作。

论文：

《Mask R-CNN》

只有非官方的代码：

Caffe版本：

https://github.com/jasjeetIM/Mask-RCNN

TensorFlow版本：

https://github.com/hillox/TFMaskRCNN

MXNet版本：

https://github.com/TuSimple/mx-maskrcnn

![](/images/img2/mask_rcnn.jpg)

参考：

https://zhuanlan.zhihu.com/p/25954683

Mask R-CNN个人理解

https://mp.weixin.qq.com/s/E0P2B798pukbtRarWooUkg

Mask R-CNN的Keras/TensorFlow/Pytorch代码实现

https://zhuanlan.zhihu.com/p/30967656

从R-CNN到Mask R-CNN

http://zh.gluon.ai/chapter_computer-vision/object-detection.html

使用卷积神经网络的物体检测

https://mp.weixin.qq.com/s/4BRwMEr6rFYvkmKXM7rYLg

效果惊艳！FAIR提出人体姿势估计新模型，升级版Mask-RCNN

https://mp.weixin.qq.com/s/6WmXY1-BmdOHkSRA_Ds9IQ

密集人体姿态估计：2D图像帧可实时生成UV贴图

# Fast Image Processing

![](/images/article/FIP.png)

上图是照片界常用的几种修图方式之一。一般将这些图片风格转换的算法，称为图像处理算子（image processing operators）。如何加速image processing operators的计算，就成为了学界研究的课题之一。

本文提出的模型就是用来加速image processing operators计算的。它是Intel Lab的Qifeng Chen和Jia Xu于2017年提出的。

论文：

《Fast Image Processing with Fully-Convolutional Networks》

代码：

https://github.com/CQFIO/FastImageProcessing

Demo网站：

http://cqf.io/ImageProcessing/

这个课题一般使用MIT-Adobe FiveK Dataset作为基准数据集。网址：

http://groups.csail.mit.edu/graphics/fivek_dataset/

这个数据集包含了5K张原始照片，并雇用了5个专业修图师，对每张图片进行修图。

众所周知，多层神经网络只要有足够的深度和宽度，就可以任意逼近任意连续函数。然而从Fast Image Processing的目的来说，神经网络的深度和宽度注定是有限的，否则肯定快不了。而这也是该课题的研究意义所在。

本文只使用了MIT-Adobe数据集中的原始图片，并使用了10种常用的算子对图片进行处理。因此，该网络训练时的输入是原始图片，而输出是处理后的图片。

![](/images/article/MCA.png)

上图是本文模型的网络结构图。它的设计特点如下：

1.采用Multi-Scale Context Aggregation作为基础网络。MCA的内容参见《深度学习（九）》。

2.传统MCA一般有下采样的过程，但这里由于网络输入和输出的尺寸维度是一样的，因此，所有的feature maps都是等大的。

3.借鉴FCN的思想，去掉了池化层和全连接层。

4.L1~L3主要用于图片的特征提取和升维，而L4~L5则用于特征的聚合和降维，并最终和输出数据的尺寸维度相匹配。

在normalization方面，作者发现有的operators经过normalization之后，精度会上升，而有的精度反而会下降，因此为了统一模型，定义如下的normalization运算：

$$\Psi^s(x)=\lambda_sx+\mu_sBN(x)$$

Loss函数为：

$$\mathcal{l(K,B)}=\sum_i\frac{1}{N_i}\|\hat f (I_i;\mathcal{K,B})-f(I_i)\|^2$$

这实际上就是RGB颜色空间的MSE误差。

为了检验模型的泛化能力，本文还使用RAISE数据集作为交叉验证的数据集。该数据集的网址：

http://mmlab.science.unitn.it/RAISE/

RAISE数据集包含了8156张高分辨率原始照片，由3台不同的相机拍摄，并给出了相机的型号和参数。

