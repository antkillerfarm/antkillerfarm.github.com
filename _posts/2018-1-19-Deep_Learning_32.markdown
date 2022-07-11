---
layout: post
title:  深度学习（三十二）——点云, AutoDL（1）, PDE
category: DL 
---

* toc
{:toc}

# 点云

## 概述

![](/images/img4/voxel.jpg)

上图是3D数据的不同表示类型:（a）点云（Point clouds）；(b) 体素网格(Voxel grids)； (c) 多边形网格(Polygon meshes)； (d) 多视图表示(Multi-view representations)

a. 点云是三维空间(xyz坐标)点的集合。

b. 体素是3D空间的像素。量化的，大小固定的点云。每个单元都是固定大小和离散坐标。

c. mesh是面片的集合。

d. 多视图表示是从不同模拟视点渲染的2D图像集合。

参考：

https://zhuanlan.zhihu.com/p/348563616

什么是体素(Voxel)?

## PCL

The Point Cloud Library是一个点云处理的库。

官网：

http://www.pointclouds.org/

## 参考

https://github.com/Yochengliu/awesome-point-cloud-analysis

三维点云论文和数据集

https://www.zhihu.com/column/c_1305511326216097792

一个激光雷达点云处理的专栏

https://mp.weixin.qq.com/s/f44TkWXklGONCuXnIuTbXg

无人驾驶汽车系统入门：基于VoxelNet的激光雷达点云车辆检测及ROS实现

https://www.zhihu.com/question/465405015

自动驾驶公司3d点云目标检测算法都基于哪个网络在做？

https://mp.weixin.qq.com/s/_lK-DPldCYGcicV7O0njfg

上海交大卢策吾团队开源PointSIFT刷新点云语义分割记录

https://zhuanlan.zhihu.com/p/41287237

点云感知CVPR 2018论文总结

https://zhuanlan.zhihu.com/p/103640399

Deep Learning for 3D Point Clouds: A Survey论文阅读

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

https://mp.weixin.qq.com/s/0OXtzUoNIyZFs-4PV173LQ

Apollo激光雷达感知技术解析

https://mp.weixin.qq.com/s/blWOcxts_V_0w0g8zFWz7w

自动驾驶之点云与图像融合综述

https://mp.weixin.qq.com/s/CMBNNhUrvfx87DeX0KbdXg

常用三维点云采样方法总结

https://mp.weixin.qq.com/s/GhJnofRLQWCnJNM4_DDtNA

点云配准算法ICP及其各种变体

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

https://mp.weixin.qq.com/s/TPC6ayQqKhlOpB8PrJHhZA

Microsoft NNI入门

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

https://mp.weixin.qq.com/s/WMxPXDabYbDqLwM-9MX4aw

Efficient Neural Architecture Search

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

## 动态神经网络

除了各种网络结构的搜索之外，还有动态神经网络这个流派。动态网络可以根据不同的输入调整其结构或参数，在精度、计算效率和适应性等方面具有显著优势。

https://mp.weixin.qq.com/s/s225jEDZRBggPuu2lOGnNA

动态神经网络最新综述论文

## 集成学习

神经网络集成学习，也是AutoDL的方法之一。

https://mp.weixin.qq.com/s/57Vxc1A5FCVbMdDYShWiDA

单网络内部集成：我要打十个！

# PDE

偏微分方程（Partial differential equation）的求解一直是数学和物理的热点问题。

在前DL时代，主要有以下方法：

有限差分方法（FDM)是计算机数值模拟最早采用的方法，至今仍被广泛运用。该方法将求解域划分为差分网格，用有限个网格节点代替连续的求解域。

有限元方法(FEA)的基础是变分原理和加权余量法，其基本求解思想是把计算域划分为有限个互不重叠的单元，在每个单元内，选择一些合适的节点作为求解函数的插值点，将微分方程中的变量改写成由各变量或其导数的节点值与所选用的插值函数组成的线性表达式，借助于变分原理或加权余量法，将微分方程离散求解。

Physics-informed neural networks：

![](/images/img4/PDE.png)

PINNs的Python库DeepXDE：

https://deepxde.readthedocs.io

参考：

https://blog.csdn.net/qq_38517015/article/details/101468893

《DeepXDE:a deep learning library for solving differential equations》梳理
