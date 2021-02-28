---
layout: post
title:  深度学习（三十二）——点云, AutoDL（1）
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

https://mp.weixin.qq.com/s/0OXtzUoNIyZFs-4PV173LQ

Apollo激光雷达感知技术解析

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

# Graph NN+

https://mp.weixin.qq.com/s/soH6jX7_fjq0iBBH-5bMsA

Node2Vec论文+代码笔记

https://mp.weixin.qq.com/s/VQON_WM_wLjGgbY2_UqzSQ

Node2Vec: 可扩展的网络特征学习

https://mp.weixin.qq.com/s/zxFgW6ofimYQ9wlChTK8cw

Recommender System with GNN

https://mp.weixin.qq.com/s/_vbcLre5HIrOGbAjTzLFjA

动态网络表征学习在推荐领域的创新与实践

https://mp.weixin.qq.com/s/17ozbpr2IIoq36VpBl3Crw

基于知识图谱和图卷积神经网络的应用和开发

https://mp.weixin.qq.com/s/mbr3u9SiYVPysBT9xuX0vg

Hyperbolic Graph Convolutional Neural Networks

https://mp.weixin.qq.com/s/nGPUcDHTrG6KwAqDCkfA1w

基于超图网络模型的图网络进化算法

https://mp.weixin.qq.com/s/XoVUJln3oyhk2jMOtWwfog

基于图神经网络的协同过滤算法

https://zhuanlan.zhihu.com/p/86216369

从3/4层拓展到56层，如何训练超级深层的图卷积神经网络

https://mp.weixin.qq.com/s/ceC1_39cmpqYOoHsu-4sCQ

HEC-Montreal唐建博士：图神经网络推理

https://mp.weixin.qq.com/s/aIU8mP5nlnqR4Qt-4RfMgQ

孙付伟：Graph Embedding在知乎的应用实践

https://zhuanlan.zhihu.com/p/111945052

TextGCN

https://blog.csdn.net/weixin_43269174/article/details/98492487

从图嵌入算法到图神经网络

https://mp.weixin.qq.com/s/LxdZ5xRuqQjNbG9FhhD0Aw

图神经网络越深，表现就一定越好吗？

https://mp.weixin.qq.com/s/d_gf12bzVySHkXFl5rqVTQ

图神经网络前沿综述：动态图网络

https://mp.weixin.qq.com/s/ea3sOkabOmaymlkeQJlZ1A

图算法工程师 面试基础

https://mp.weixin.qq.com/s/3xNRyEsw7QPCzvpQU6chDw

图神经网络的重要分支：时间图网络

https://mp.weixin.qq.com/s/GibBqURtF_Oak8NOKDjEPg

时序图神经网络

https://mp.weixin.qq.com/s/k3HTFY3QLdiY_zj81NePAg

腾讯AI Lab联合清华、港中文，万字解读图深度学习历史、最新进展与应用

https://mp.weixin.qq.com/s/TSsKeADBuamrTPrRKUBpBQ

Graph Normalization (GN)：为图神经网络学习一个有效的图归一化

https://mp.weixin.qq.com/s/oBgPS0s_Fe2w4bmT__664A

动态图上的深度学习-动态时间图网络建模技术综述

https://mp.weixin.qq.com/s/N3A8N4DZC-x8YavEXdpfKA

使用图进行特征提取：最有用的图特征机器学习模型介绍

https://mp.weixin.qq.com/s/3XKrFGkhxxXdg-XGNkvXdg

LINE：不得不看的大规模信息网络嵌入

https://mp.weixin.qq.com/s/BPbUiIl9yU6DJbjKQmrzgw

结合图的结构信息和节点特征的图对比学习

https://mp.weixin.qq.com/s/FwIbHiyZa4EcnMunWoAR1Q

北大发布最新《图神经网络推荐系统》2020综述论文，27页pdf

https://blog.csdn.net/leviopku/article/details/106949616

GNN中的Graph Pooling

https://mp.weixin.qq.com/s/5vgN2Z-FVMQ9ofPAy1Vf-g

图表示学习中的Encoder-Decoder框架

https://mp.weixin.qq.com/s/2HwszqbC_I2zdTCiQirxPQ

图注意力网络入门：从数学理论到到NumPy实现

https://mp.weixin.qq.com/s/oRWMAeKcftmCL1W1SW823g

推荐系统与GNN擦出的火花竟如此绚丽多彩
