---
layout: post
title:  深度学习（三十二）——点云, AutoDL（1）, 图像超分辨率进阶
category: DL 
---

* toc
{:toc}

# 点云

## PCL

The Point Cloud Library是一个点云处理的库。

官网：

http://www.pointclouds.org/

## 参考

https://github.com/Yochengliu/awesome-point-cloud-analysis

三维点云论文和数据集

https://mp.weixin.qq.com/s/f44TkWXklGONCuXnIuTbXg

无人驾驶汽车系统入门：基于VoxelNet的激光雷达点云车辆检测及ROS实现

https://mp.weixin.qq.com/s/_lK-DPldCYGcicV7O0njfg

上海交大卢策吾团队开源PointSIFT刷新点云语义分割记录

https://zhuanlan.zhihu.com/p/41287237

点云感知CVPR 2018论文总结

https://mp.weixin.qq.com/s/fPj4wMtiFX55c_UjNsQnBg

PointCNN全面刷新测试记录：山东大学提出通用点云卷积框架

http://mp.weixin.qq.com/s/5ozeLeF6IyKTOqAxYEdaog

山东大学提出PointCNN：让CNN更好地处理不规则和无序的点云数据

https://mp.weixin.qq.com/s/QHxuYWb39BktuTomiwG2rg

点云配准国内外研究现状

https://mp.weixin.qq.com/s/ki0F3zjbB_X84m9NsYYlow

旷视Oral论文提出GeoNet：基于测地距离的点云分析深度网络

https://mp.weixin.qq.com/s/X7ijhqXjICjIJh3nUNsuBw

百度无人车实现全球首个基于深度学习的激光点云自定位技术

https://mp.weixin.qq.com/s/iQwvqMfQgVZbOHeK-fVBIQ

PointConv：在点云上高效实现卷积操作

https://mp.weixin.qq.com/s/MQU98bk6tFRSJGDfRUFkOQ

Relation-Shape CNN：以几何关系卷积推理点云3D形状

https://mp.weixin.qq.com/s/u9BNyA06P96yIvA0uXUqAw

PointNet系列论文解读

https://mp.weixin.qq.com/s/KuoHFPpUYHPFacsBWZWVSg

Relation-Shape CNN：以几何关系卷积推理点云3D形状

https://mp.weixin.qq.com/s/Jso2YZs2NEtMORZsLkrJ5w

Geometric Relation Learning in 3D Point Cloud Analysis

https://mp.weixin.qq.com/s/sXxUh_9Kd_2BkISHbxL2CA

基于深度学习的实时激光雷达点云目标检测及ROS实现

https://mp.weixin.qq.com/s/dgDBmjavB0-Rz9VrsJ8vkQ

PointNet

https://zhuanlan.zhihu.com/p/71564244

PointRCNN第一个基于原始点云的3D目标检测

https://mp.weixin.qq.com/s/ZujYVfpM7ccGPqZnh1YsuQ

基于点云/RGBD的3D视觉检测技术

https://mp.weixin.qq.com/s/vewENL7bakutXraGkyjCMA

3D目标检测于RGB-D（Object detection in RGB-D images）

https://mp.weixin.qq.com/s/d1eGpZUVR25tusL4VtwHug

激光雷达深度补全

https://mp.weixin.qq.com/s/AOtC1PpTZM9zWPDuK4Vulg

贾佳亚等提出Fast Point R-CNN，利用点云快速高效检测3D目标

https://mp.weixin.qq.com/s/CN1vJ9to14QscgOVbMs3zg

​端到端传感器建模生成激光雷达点云

https://mp.weixin.qq.com/s/vJ-BlxdY8rTwz7ygno6uWQ

DeepVCP：百度无人车提出首个端到端的高精度点云配准网络

https://zhuanlan.zhihu.com/p/91275450

点云配准综述

https://mp.weixin.qq.com/s/5RJAv_cOlhee1R9uZzkmHQ

国防科技大学发布最新“3D点云深度学习”综述论文，带你全面了解最新点云学习方法

https://mp.weixin.qq.com/s/JVFHgrctp-Mi9k7GtFQpFg

深度学习3D点云分割，22页综述全面了解领域最新进展

https://mp.weixin.qq.com/s/gybhVw3D4ykAHsVGzazWNw

3D-BoNet：一种简单高效的3D点云实例分割框架

https://mp.weixin.qq.com/s/xvAF_-rRlU5-hYmClFvTjA

一文览尽“基于激光雷达点云（lidar）的目标检测方法”

https://mp.weixin.qq.com/s/fn0EAanAEFT-Aupl4Skpcw

基于深度学习处理点云数据入门经典：PointNet、PointNet++

https://mp.weixin.qq.com/s/1M22-5ns3Mbd9_VJjTdGBQ

3D物体检测、行为预测和运动检测，一文解析激光雷达中时序融合的研究现状和发展方向

# AutoDL

DL领域目前存在的主要问题之一是：如何设计网络结构和调整超参数。目前的做法，通常依赖于作者的直觉，属于典型的拍脑袋想点子。

既然AI已经能够做很多事了，那么有没有可能，使用AI自动生成网络结构呢？

Google的这两篇论文在这里做了一些尝试：

《Neural Architecture Search With Reinforcement Learning》

《Learning Transferable Architectures for Scalable Image Recognition》

这里主要采用强化学习的方法，在一个广阔的搜索空间中，寻找最合适的网络结构。但对于计算能力提出了很高的要求。论文中提到，他们使用了500块GPU。**有钱真的是可以为所欲为的。**

这里学到的模型，一般被称为NASNet。

![](/images/img4/NAS.png)

NAS的工作可以根据三个方面进行划分：search space(搜索空间)、search strategy(搜索策略)、performance estimation strategy(性能评估策略)。

AutoDL常用的套路主要有：

- random search/gird search。这是最古老的办法。
- reinforcement learning
- gradient-based
- weight-sharing
- evolutionary
- random graph

## 工具

https://mp.weixin.qq.com/s/9OZiFgziY8fMn-lkmdtiqQ

终结谷歌AutoML的真正杀手！Saleforce开源TransmogrifAI

https://mp.weixin.qq.com/s/GFjYzxf6IdKPvkij8-i_Dg

调参工要凉？微软重磅开源AutoML工具包NNI

https://mp.weixin.qq.com/s/7_KRT-rRojQbNuJzkjFMuA

微软开源自动机器学习工具NNI概览及新功能详解

https://mp.weixin.qq.com/s/L1nkhc4I6VX2s6S5Tiv0Zw

小白也能搭建深度模型，百度EasyDL的背后你知多少

![](/images/img2/AdaNet.gif)

https://mp.weixin.qq.com/s/HiD-OqAz67cwwchjSyIjWA

AutoML又一利器来了，谷歌宣布开源AdaNet

https://mp.weixin.qq.com/s/o3WqVIxlsukt3_JcY-10LA

AdaNet简介：快速灵活的AutoML，提供学习保证

https://mp.weixin.qq.com/s/2ehni5O__XnPaLjBRmAiIQ

利用AdaNet将多个TensorFlow Hub模块组合成一个集成网络

https://mp.weixin.qq.com/s/mEYFnP1q131-4hdgjOQ2cQ

“超参数”与“网络结构”自动化设置方法---DeepHyper

https://mp.weixin.qq.com/s/uF3R3FoERAKvIZLpgWp13A

BTB: 用于开发AutoML自动化机器学习系统的简单、可扩展库

## 书籍

https://mp.weixin.qq.com/s/wTwjbripELzqdCjs64Dzzw

告别调参，AutoML新书发布

## ENAS

https://mp.weixin.qq.com/s/SwJs0OUzBlhiIOyFHFBK_g

一文看懂Jeff Dean等提出的ENAS到底好在哪？

https://mp.weixin.qq.com/s/Jw2kzCf2uFibhIpREnjO_A

图解高效神经网络结构搜索（ENAS）

https://mp.weixin.qq.com/s/RfONzym6-FQgF1J8wru35A

ENAS：首个权值共享的神经网络搜索方法，千倍加速

## DARTS

论文：

《DARTS: Differentiable Architecture Search》

代码：

https://github.com/quark0/darts

参考：

https://mp.weixin.qq.com/s/bjVTpdaKFx4fFtT8BjMNew

指数级加速架构搜索：CMU提出基于梯度下降的可微架构搜索方法（DARTS）

https://mp.weixin.qq.com/s/Ihn-faO0XvfDDqtEwfWDzg

DARTS：基于梯度下降的经典网络搜索方法，开启端到端的网络搜索

# 图像超分辨率进阶

亚像素尺度上对物体进行计数，是一类和超分辨率很相关的CV任务。

https://mp.weixin.qq.com/s/Zp5jlUspiocEZ6CI1cWJZw

深度学习能看到的比你更多，亚像素物体计数方法介绍

----

![](/images/img4/SR.jpg)

https://zhuanlan.zhihu.com/p/266472242

一文带你入门超分网络及其渐进式上采样方法

https://mp.weixin.qq.com/s/xpvGz1HVo9eLNDMv9v7vqg

NTIRE2017夺冠论文：用于单一图像超分辨率的增强型深度残差网络

https://www.zhihu.com/question/25401250

如何通过多帧影像进行超分辨率重构？

https://www.zhihu.com/question/38637977

超分辨率重建还有什么可以研究的吗？

https://zhuanlan.zhihu.com/p/25912465

胎儿MRI高分辨率重建技术：现状与趋势

https://mp.weixin.qq.com/s/i-im1sy6MNWP1Fmi5oWMZg

华为推出新型HiSR：移动端的超分辨率算法

https://mp.weixin.qq.com/s/h4Xzt-aS1_-5zjTB0ypTLg

普通视频转高清：10个基于深度学习的超分辨率神经网络

https://mp.weixin.qq.com/s/WmqagSGRy98USgnz21W3Pg

WDSR

https://mp.weixin.qq.com/s/oNavFLOPskHNxWIyBbFzHw

WDSR (NTIRE2018 超分辨率冠军)

https://mp.weixin.qq.com/s/AxHTaT-G5_Y6Iw_3aIIxCg

超分辨率技术如何发展？这6篇ECCV 18论文带你一次尽览

https://mp.weixin.qq.com/s/zWoQCKbZNz2td3cZxEsqKQ

腾讯优图提出SRN-DeblurNet：高效高质量去除复杂图像模糊

https://mp.weixin.qq.com/s/eJkkbGBYxWlngkT5gjjW7g

效果惊人：上古卷轴III等经典游戏也能使用超分辨率GAN重制了

https://mp.weixin.qq.com/s/M8gCrQDtjT1lszsxV2QQKg

FSRNet：端到端深度可训练人脸超分辨网络

https://mp.weixin.qq.com/s/fkHfzkHRFpEGc-L4uGlqcg

从网络设计到实际应用，深度学习图像超分辨率综述

https://mp.weixin.qq.com/s/C9Ku4MvU2QuZ_GJcs8FelA

基于深度学习的图像超分辨率最新进展与趋势

https://mp.weixin.qq.com/s/qNHbs2Bd4buVlu0j3Rtj0g

Adobe提出新型超分辨率方法：用神经网络迁移参照图像纹理

https://mp.weixin.qq.com/s/N-kAIK_qMowsFqEtWTqt4w

图像超分辨率重建--工程应用

https://mp.weixin.qq.com/s/JaPYUWvh7RBHVUYgKlTeHw

旷视提出超分辨率新方法Meta-SR：单一模型实现任意缩放因子

https://mp.weixin.qq.com/s/aEu0q04pgXUE1mqAS5kewQ

不用P30 Pro，普通手机也能变身望远镜：陈启峰团队新作，登上CVPR 2019

https://mp.weixin.qq.com/s/CXDzPSSyObPHYc40YictKQ

CVPR 2019 神奇的超分辨率算法DPSR：应对图像模糊降质

https://mp.weixin.qq.com/s/Rr4AKGjZNyV3PDoRBVH-lw

低清视频也能快速转高清：超分辨率算法TecoGAN

https://mp.weixin.qq.com/s/PIq78vhNQxAKntnZilL4pQ

分割、检测与定位，高分辨率网络显神威！这会是席卷深度学习的通用结构吗？

https://mp.weixin.qq.com/s/G55dxHfMYxWzjz4_8YnUaw

深度学习超分辨率最新综述：一文道尽技术分类与效果评测

https://zhuanlan.zhihu.com/p/67613641

基于多级神经纹理迁移的图像超分辨方法(Adobe Research)

https://mp.weixin.qq.com/s/IStOD22WgZ6EBFCvIHkNSg

图像超分辨率网络：RCAN

https://mp.weixin.qq.com/s/4LIq3kZaXgaoEEyb8TQvwg

图像超分辨率重建--AI研究

https://mp.weixin.qq.com/s/ETGPXVDGRHOq0W-ef_q2_A

RankSRGAN:排序学习+GAN用于超分辨率

https://zhuanlan.zhihu.com/p/140507840

图像超分：USRNet

https://mp.weixin.qq.com/s/sju4SYFxzDkJevk3mi68Rw

图像超分辨率网络：EDSR

https://mp.weixin.qq.com/s/uZQK0oQbV7wm0WVz_QxVQg

DRN：用于单图像超分辨率的对偶回归网络
