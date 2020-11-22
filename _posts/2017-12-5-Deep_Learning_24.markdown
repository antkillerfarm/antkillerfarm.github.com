---
layout: post
title:  深度学习（二十四）——深度目标跟踪, L2 Normalization
category: DL 
---

* toc
{:toc}

# 深度目标跟踪

![](/images/img3/recent_Tracker_development.png)

原图地址：

https://github.com/foolwood/benchmark_results

这也是一个Visual Tracking的Paper List。

参考：

https://mp.weixin.qq.com/s/i5TyVvaRuTZ8c54Iiu4VHg

跟踪综述推荐：目标跟踪40年

https://zhuanlan.zhihu.com/p/22334661

深度学习在目标跟踪中的应用

https://mp.weixin.qq.com/s/u2UyeZwWKCQX5iOe6ZUA9w

目标跟踪算法综述

https://mp.weixin.qq.com/s/yunmBZ_5acDZlIN0mLf66g

《视觉跟踪最新方法与趋势》，44页最新综述带你全面了解视觉跟踪领域发展方向

https://zhuanlan.zhihu.com/p/76153871

一文带你了解视觉目标跟踪

https://zhuanlan.zhihu.com/p/96631118

单目标跟踪paper小综述

https://zhuanlan.zhihu.com/p/27293523

目标跟踪领域进展报告

https://mp.weixin.qq.com/s/3R8zaFUKFTvp0uE8lY44WA

首个应用残差学习的深度目标跟踪算法

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

https://zhuanlan.zhihu.com/p/26654891

EBT：Proposal与Tracking不得不说的秘密

https://mp.weixin.qq.com/s/Pp3vnD2DMDYuiatI8drJrw

PTAV：实时高精度目标追踪框架

https://mp.weixin.qq.com/s/NCtgHHpiG473gixu1tZeJA

哈工大提出STRCF：克服遮挡和大幅形变的实时视觉追踪算法

https://mp.weixin.qq.com/s/2RNLG4KU81z7thk3CLwD9Q

商汤科技 & 中科院自动化所：视觉跟踪之端到端的光流相关滤波

https://zhuanlan.zhihu.com/p/36463844

目标跟踪新高度ECO+：解除深度特征被封印的力量

https://zhuanlan.zhihu.com/p/37897158

SiameseRPN阅读笔记

https://zhuanlan.zhihu.com/p/39614034

基于孪生区域推荐网络的高性能单目标跟踪

https://mp.weixin.qq.com/s/tr1oWpaRZair9zG5CHWvbg

视觉目标跟踪之DaSiamRPN

https://mp.weixin.qq.com/s/aRUmQ7SMlVczuifjSrUzFg

商汤开源目标跟踪最强算法SiamRPN系列

https://mp.weixin.qq.com/s/Nd5XLnncXRawu4V8TOzHxg

视觉物体跟踪新进展：让跟踪器读懂目标语义信息

https://zhuanlan.zhihu.com/p/46669238

VOT2018：SiamNet大崛起

https://mp.weixin.qq.com/s/dB5u2No8eakLnrjto0kvyQ

DaSiamRPN的升级版，视觉目标跟踪之SiamRPN++

https://mp.weixin.qq.com/s/hAdMfwTqfuMcF07RhhKvGg

SiamRPN++论文笔记

https://mp.weixin.qq.com/s/7hScDZQfv42nBKdYu4RA0Q

惊艳的SiamMask：开源快速同时进行目标跟踪与分割算法

https://mp.weixin.qq.com/s/yU7_j6hmWKv6FV1pWGq8qA

京东开源姿态跟踪新框架LightTrack！

https://mp.weixin.qq.com/s/Tlac-tXNlbACm17EFUsn8Q

ASRCF：基于自适应空间加权相关滤波的视觉跟踪研究

https://mp.weixin.qq.com/s/dcrVoGBU7SrCGQysJwTGcw

基于上下文信息分离的无监督运动目标检测

https://mp.weixin.qq.com/s/wK77YucINmQtvnH1nJX8vw

张志鹏：SiamDW Real-Time Visual Tracking

https://zhuanlan.zhihu.com/p/72988881

基于深度学习的光流估计

https://zhuanlan.zhihu.com/p/74460341

光流估计——从传统方法到深度学习

https://zhuanlan.zhihu.com/p/261046950

光流估计

https://mp.weixin.qq.com/s?__biz=MzI5MDUyMDIxNA==&mid=2247491156&idx=2&sn=e088a817c9c6b4f52527cfcf51dc7ffe

首个siamese网络中训练GCNs的视觉追踪方法《Graph Convolutional Tracking》

https://zhuanlan.zhihu.com/p/95842960

D3S

https://mp.weixin.qq.com/s/3NiCkJoTBt2qLaOsMAkCNg

MAML-Tracker:用目标检测思路做目标跟踪？小样本即可得高准确率

https://mp.weixin.qq.com/s/vFEHZv6qh1dWhQ27WUEDEQ

目标跟踪系列--C-COT算法

https://mp.weixin.qq.com/s/nDkaOb4q9cPSqW4w76kcNg

码隆科技提出SiamAttn，将孪生网络跟踪器的性能提至最优水平

https://mp.weixin.qq.com/s/DWaKFhK-dv43ap99fRnpAw

概率回归的视觉追踪

## FlowNet

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

https://mp.weixin.qq.com/s/Z3Ra-XcUGO5BcYGLeKBClg

港中大等打造光流预测新模型SelFlow，自监督学习攻克遮挡难题

https://mp.weixin.qq.com/s/hWtiRPyeuk7VNoZhOwEUCQ

FlowNet

https://mp.weixin.qq.com/s/utlOnJ08ksu5BPOwqSQT-A

MaskFlownet：基于可学习遮挡掩模的非对称特征匹配

## SpyNet

论文：

《Optical Flow Estimation using a Spatial Pyramid Network》

代码：

https://github.com/anuragranj/spynet

参考：

http://www.cnblogs.com/wangxiaocvpr/p/7058617.html

论文笔记之：Optical Flow Estimation using a Spatial Pyramid Network

## 多目标追踪

Multiple Object Tracking

https://zhuanlan.zhihu.com/p/34730368

多目标追踪

https://mp.weixin.qq.com/s/5Kcbnme7151Ln6KiTyVaBw

多目标追踪资源列表

https://mp.weixin.qq.com/s/laaLCZZIwNaEsy-Au9Yh7Q

多目标跟踪算法汇总，含算法介绍/论文/源码

https://zhuanlan.zhihu.com/p/65177442

多目标跟踪：近年论文及开源代码汇总

https://zhuanlan.zhihu.com/p/97449724

多目标跟踪（MOT）入门

https://mp.weixin.qq.com/s/hCg0kn2x8q-K9n_96-7p8A

基于深度学习的多目标跟踪（MOT）技术一览

https://zhuanlan.zhihu.com/p/36462982

多目标追踪算法：条件随机场算法

https://mp.weixin.qq.com/s/Lmvx_KurxG3nfmvhXAD_tQ

一种轻量级在线多目标车辆跟踪方法

https://mp.weixin.qq.com/s/zlo59TVpSq5w6zTwJLjbXg

视觉多目标跟踪算法综述（上）

https://mp.weixin.qq.com/s/XwMXrsmSnImgD1vNSVErLg

深度多目标跟踪算法综述

https://mp.weixin.qq.com/s/IlFxY-cXyOg2xmaGdJbOVQ

多目标跟踪算法

https://mp.weixin.qq.com/s/KLTSUqprwfFBVeVIP7HJRw

视频中的多目标跟踪

https://zhuanlan.zhihu.com/p/55738110

商汤等提出：统一多目标跟踪框架

https://mp.weixin.qq.com/s/PGs3M5-vdQf4deE6hJuqng

多目标跟踪：SORT和Deep SORT

https://mp.weixin.qq.com/s/R2OE76ipTX3r_vvj0lYYYQ

精度优秀，速度214.7fps！卡内基梅隆大学开源强大的3D多目标跟踪系统

https://mp.weixin.qq.com/s/Siy_FaRmMRGPn4VBZuVRUQ

基于深度学习的多目标跟踪算法（上）：端到端的数据关联

https://mp.weixin.qq.com/s/7IzqglD0e89Y2Oai9Jv2XQ

打遍天下无敌手，却说它只是个baseline！多目标跟踪FairMOT的烦恼

https://mp.weixin.qq.com/s/eN9PgGBbM-5R3xQ6zSFiYA

多目标跟踪（MOT）领域近期值得读的几篇论文

https://zhuanlan.zhihu.com/p/138443415

从UMA Tracker(CVPR2020)出发谈谈SOT类MOT算法

https://mp.weixin.qq.com/s/lj926JcCYX8qbUOBTRsQIw

实时多人追踪论文--MOTDT

https://zhuanlan.zhihu.com/p/143798072

One Shot MOT Overview

https://mp.weixin.qq.com/s/rIMOKYgXaFEbvRZ14brrjw

Deep SORT论文阅读总结

https://mp.weixin.qq.com/s/wjKbKF8McGOjbjudi3cOVQ

FairMOT：统一检测、重识别的多目标跟踪框架，全新Baseline

https://zhuanlan.zhihu.com/p/151105050

基于图卷积GNN的多目标跟踪算法解析

https://mp.weixin.qq.com/s/bMBBYWk5-o5XEOSzI7DyDg

从FairMOT到VoxelPose，揭秘微软以“人”为中心的最新视觉理解成果

https://mp.weixin.qq.com/s/qpNbHKZ0V5do77jQlZDBBw

从零开始学习Deep SORT+YOLO V3进行多目标跟踪

https://mp.weixin.qq.com/s/mvx_LyGVzn_H3XpwTsuFMA

多目标跟踪：监控领域你必须要了解的算法

# 目标检测进阶++

https://mp.weixin.qq.com/s/k8msLl6c2Cp_5h-4xBD6Zw

CVPR2019-目标检测分割技术进展

https://mp.weixin.qq.com/s/uzG8sic5Y6LVqBS6iKQDhw

目标检测中图像增强，mixup如何操作？

https://mp.weixin.qq.com/s/pkFcmm15gnuRJtngFX7f0w

目标检测训练trick超级大礼包—不改模型提升精度，值得拥有

https://mp.weixin.qq.com/s/flXzhQ-Ypf3fwTqLelLzOQ

李沐等将目标检测绝对精度提升5%，不牺牲推理速度

https://mp.weixin.qq.com/s/6QsyYtEVjavoLfU_lQF1pw

目标检测新文：Generalized Intersection over Union

https://mp.weixin.qq.com/s/Xs3nThAcUOq62bO2p61YFA

论文解读 Receptive Field Block Net for Accurate and Fast

https://mp.weixin.qq.com/s/dcrBQ-t3tLOTouEyofOBxg

间谍卫星：利用卷积神经网络对卫星影像进行多尺度目标检测

https://mp.weixin.qq.com/s/LtXylKTKsHdjMPw9Q1HyXA

优于MobileNet、YOLOv2：移动设备上的实时目标检测系统Pelee

https://mp.weixin.qq.com/s/xpk9LhsZ3dRMvqR6Uc5jeg

Pelee：移动端实时检测骨干网络

https://mp.weixin.qq.com/s/Gq3bflJq59Tx-nDCvbweNA

无需预训练分类器，清华&旷视提出专用于目标检测的骨干网络DetNet

https://mp.weixin.qq.com/s/u3eXhoFvo7vZujc0XoQQWQ

旷视研究院解读Light-Head R-CNN：平衡精准度和速度

https://mp.weixin.qq.com/s/6cUP9vvfcuv8rIEnGnAFiA

NCSU&阿里巴巴论文：可解释的R-CNN

https://mp.weixin.qq.com/s/1vOdOMyByBacSBMVrscq5Q

黄畅：基于DenesBox的目标检测在自动驾驶中的应用

https://mp.weixin.qq.com/s/-PeXMU_gkcT5YnMcLoaKag

CVPR清华大学研究，高效视觉目标检测框架RON

https://mp.weixin.qq.com/s/XoKdsQKyaI3LsDxF7uyKuQ

聊聊目标检测中的多尺度检测（Multi-Scale），从YOLO到FPN，SNIPER，SSD填坑贴和极大极小目标识别

https://mp.weixin.qq.com/s/GpZHGksl0elxMcaQYosK-A

SNIP的升级版SNIPER（效果比Mosaic更佳）

https://mp.weixin.qq.com/s/XdH54ImSfgadCoISmVyyVg

基于单目摄像头的物体检测

https://mp.weixin.qq.com/s/h_ENriEXr7WI_XR_DtxpMQ

这样可以更精确的目标检测——超网络

https://mp.weixin.qq.com/s/dFoUO4xArZpmtbKg1Kx6Zg

COCO mAP 53.3！骨干网合成算法CBNet带来目标检测精度新突破

https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247499933&idx=1&sn=b9fe7d6714c44acedd12a60cfe6b1c60

小样本域适应的目标检测

https://zhuanlan.zhihu.com/p/84890413

PolarMask：一阶段实例分割新思路

https://mp.weixin.qq.com/s/t8pVNeW2Y-QQwD8H9Nk83w

定向和密集的目标检测怎么办？动态优化网络来解决

https://zhuanlan.zhihu.com/p/158907507

From VanillaDet to AutoAssign

https://mp.weixin.qq.com/s/GL_q4VLCgbjTZq_zpTLq0w

Label Assign：提升目标检测上限

https://mp.weixin.qq.com/s/EO5h1zW4yrToqwTCnXtz4g

TTFNet: 最大程度提高训练效率的实时目标检测

https://mp.weixin.qq.com/s/6R3myLyAeMMS0lR1GSlvcA

细说物体检测中的Anchors
