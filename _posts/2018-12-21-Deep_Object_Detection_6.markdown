---
layout: post
title:  深度目标检测（六）——Anchor-Free, 其它目标检测网络, 目标检测进阶
category: Deep Object Detection 
---

# FPN（续）

- 图（d）则是FPN的网络结构，该网络结构大体上分为三个部分，即buttom-up自底向上的特征抽取，自顶向下的upsampling，以及侧边融合通道（lateral coonnection）。通过这三个结构网络的每一层均会具有较强的语义信息，且能很好的满足速度和内存的要求。

![](/images/img3/FPN.jpg)

上图是加了FPN之后的ResNet，其中的虚线框表示的是通道融合的方法。U-Net采用了concat模式融合下采样和上采样通道，而这里则是沿用了ResNet的融合方法：Tensor Add。

![](/images/img3/FPN_2.jpg)

上图是Faster R-CNN+FPN。原始的Faster R-CNN的RoI pooling是从同一个feature map中获得ROI，而这里是根据目标尺度大小，从不同尺度的feature map中获得ROI。

参考：

https://mp.weixin.qq.com/s/mY_QHvKmJ0IH_Rpp2ic1ig

目标检测FPN

https://mp.weixin.qq.com/s/TelGG-uVQyxwQjiDGE1pqA

特征金字塔网络FPN

https://zhuanlan.zhihu.com/p/58603276

FPN-目标检测

# Anchor-Free

https://zhuanlan.zhihu.com/p/63024247

锚框：Anchor box综述

https://mp.weixin.qq.com/s/dYV446meJXtCQVFrLzWV8A

目标检测中Anchor的认识及理解

https://mp.weixin.qq.com/s/WAx3Zazx9Pq7Lb3vKa510w

目标检测最新方向：推翻固有设置，不再一成不变Anchor

https://zhuanlan.zhihu.com/p/64563186

Anchor free深度学习的目标检测方法

https://mp.weixin.qq.com/s/DoN-vha1H-2lHhbFOaVS8w

FoveaBox：目标检测新纪元，无Anchor时代来临！

https://zhuanlan.zhihu.com/p/62198865

最新的Anchor-Free目标检测模型FCOS，现已开源！

https://mp.weixin.qq.com/s/N93TrVnUuvAgfcoHXevTHw

FCOS: 最新的one-stage逐像素目标检测算法

https://mp.weixin.qq.com/s/04h80ubIxjJbT9BxQy5FSw

目标检测：Anchor-Free时代

https://mp.weixin.qq.com/s/wWqdjsJ6U86lML0rSohz4A

CenterNet：将目标视为点

https://zhuanlan.zhihu.com/p/62789701

中科院牛津华为诺亚提出CenterNet，one-stage detector可达47AP，已开源！

https://zhuanlan.zhihu.com/p/66156431

从Densebox到Dubox：更快、性能更优、更易部署的anchor-free目标检测

# 其它目标检测网络

## A-Fast-RCNN

A-Fast-RCNN首次将对抗学习引入到了目标检测领域，idea是非常创新的。

http://blog.csdn.net/jesse_mx/article/details/72955981

A-Fast-RCNN论文笔记

## R-FCN

R-FCN是何恺明/孙剑小组的Jifeng Dai于2016年提出的。

论文：

《R-FCN: Object Detection via Region-based Fully Convolutional Networks》

代码：

https://github.com/PureDiors/pytorch_RFCN

![](/images/img3/R-FCN.png)

FCN在目标检测领域的应用。

参考：

http://blog.csdn.net/zijin0802034/article/details/53411041

R-FCN: Object Detection via Region-based Fully Convolutional Networks

https://blog.csdn.net/App_12062011/article/details/79737363

R-FCN

https://mp.weixin.qq.com/s/HPzQST8cq5lBhU3wnz7-cg

R-FCN每秒30帧实时检测3000类物体，马里兰大学Larry Davis组最新目标检测工作

https://mp.weixin.qq.com/s/AddHG_I00uaDov0le4vdvA

R-FCN和FPN

## G-CNN

G-CNN是MaryLand大学的工作，论文主要的思路也是消除region proposal，和YOLO，SSD不同，G-CNN的工作借鉴了迭代的想法，把边框检测等价于找到初始边框到最终目标的一个路径。但是使用one-step regression不能处理这个非线性的过程，所以作者采用迭代的方法逐步接近最终的目标。

http://blog.csdn.net/zijin0802034/article/details/53535647

G-CNN: an Iterative Grid Based Object Detector

## CornerNet

https://mp.weixin.qq.com/s/e74-zFcMZzn67KaFXb_fdQ

CornerNet目标检测开启预测“边界框”到预测“点对”的新思路

https://zhuanlan.zhihu.com/p/41865617

CornerNet：目标检测算法新思路

https://mp.weixin.qq.com/s/e6B22xpue_xZwrXmIlZodw

ECCV-2018最佼佼者CornerNet的目标检测算法

https://mp.weixin.qq.com/s/9ldLaYKGkgq-MnJZw7CrDQ

CornerNet为什么有别于其他目标检测领域的主流算法？

https://mp.weixin.qq.com/s/ZhfnZ4IwOnTQlqeB6Ilr3A

CornerNet: Detecting Objects as Paired Keypoints解读

https://zhuanlan.zhihu.com/p/63134919

普林斯顿大学提出：CornerNet-Lite，基于关键点的目标检测算法，已开源！

# 目标检测进阶

https://mp.weixin.qq.com/s/dcrBQ-t3tLOTouEyofOBxg

间谍卫星：利用卷积神经网络对卫星影像进行多尺度目标检测

https://mp.weixin.qq.com/s/LtXylKTKsHdjMPw9Q1HyXA

优于MobileNet、YOLOv2：移动设备上的实时目标检测系统Pelee

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

https://mp.weixin.qq.com/s/AupXIoVmhcOBrX1z1vgdtw

弱监督实现精确目标检测，上交大提出协同学习框架

https://mp.weixin.qq.com/s/Lt00ASVSb_fDDJdtCO0-tQ

物体检测中的结构推理网络

https://mp.weixin.qq.com/s/f0Ynln-27z5A6LXt8j5qKQ

据说以后在探头下面用帽子挡脸没用了：SymmNet遮挡物检测的对称卷积神经网络

https://mp.weixin.qq.com/s/cEg6HmS651riJVAtHdPafg

基于域适应弱监督学习的目标检测

https://mp.weixin.qq.com/s/PpT-NmTVjRi0_SEq0lISXw

旷视科技Oral论文解读：IoU-Net让目标检测用上定位置信度

https://mp.weixin.qq.com/s/OqlZ2TRGbHURYW00440lgQ

微软亚洲研究院与北京大学共同提出用于物体检测的可学习区域特征提取模块

https://mp.weixin.qq.com/s/5I9uzGCNFD93L1mzakTl0Q

目标检测网络学习总结（RCNN-->YOLO V3）

https://mp.weixin.qq.com/s/ZQqcsJenqkXtH1czOe5WnA

阿里巴巴提出Auto-Context R-CNN算法，刷出Faster RCNN目标检测新高度

https://mp.weixin.qq.com/s/aLYQepnr_BjS27Fb-zoZ_g

迈向完全可学习的物体检测器：可学习区域特征提取方法

https://zhuanlan.zhihu.com/p/43655912

“别挡我，我要C位出道！”谈谈深度学习目标检测中的遮挡问题

https://mp.weixin.qq.com/s/VtlSVF4d9LwPJhDEYSbgTg

无监督难分样本挖掘改进目标检测

https://mp.weixin.qq.com/s/h_ENriEXr7WI_XR_DtxpMQ

这样可以更精确的目标检测——超网络

https://mp.weixin.qq.com/s?__biz=MzI5MDUyMDIxNA==&mid=2247486104&idx=1&sn=5580a4680f3190adb98638471e9b5982

百度视觉团队斩获 ECCV Google AI 目标检测竞赛冠军，获奖方案全解读

https://mp.weixin.qq.com/s/nL9l7hvG3RG7G7LzCzzvug

旷视科技2018 COCO负责人俞刚：如何构建检测与分割的冠军系统

https://mp.weixin.qq.com/s/zeruKQOye_QNWgluVIN0BA

从R-CNN到RFBNet，目标检测架构5年演进全盘点

https://mp.weixin.qq.com/s/sCGNUI-mUSYxD69uBDQNoQ

基于深度学习的目标检测算法综述：算法改进

https://mp.weixin.qq.com/s/XdH54ImSfgadCoISmVyyVg

基于单目摄像头的物体检测

https://mp.weixin.qq.com/s/yswy7VwEapQJ9M5n_Uo93w

目标检测最新进展总结与展望

https://mp.weixin.qq.com/s/s1qmCA8djEEanwCxeLSV2Q

63页《深度CNN-目标检测》综述

https://mp.weixin.qq.com/s/XoKdsQKyaI3LsDxF7uyKuQ

聊聊目标检测中的多尺度检测（Multi-Scale），从YOLO到FPN，SNIPER，SSD填坑贴和极大极小目标识别

https://mp.weixin.qq.com/s/iW-k12CIO0gSx8Y6etTzgA

三分支网络——目前目标检测性能最佳网络框架

https://mp.weixin.qq.com/s/JN1N-IqIL4tAh4rIkZcxpg

Grid R-CNN解读：商汤最新目标检测算法

https://mp.weixin.qq.com/s/baPfFVi7deEsCAFu3ColoQ

CVPR2018目标检测算法总览

https://mp.weixin.qq.com/s/t5p1xGKVnwd7wbiOzucFqQ

基于深度学习的目标检测算法剖析与实现

https://mp.weixin.qq.com/s/-zQZjHVs7bYyGkGuMUf3qg

目标检测领域还有什么可做的？19个方向给你建议

https://zhuanlan.zhihu.com/p/54334986

TridentNet：处理目标检测中尺度变化新思路

https://mp.weixin.qq.com/s/S6sz5dPgGNcJvrIAZ3ZjGg

基于深度学习的通用物体检测算法对比探索

https://mp.weixin.qq.com/s/oF3MAkl1UikRkOhrj3equw

深度学习的目标检测算法是如何解决尺度问题的？

https://mp.weixin.qq.com/s/oxStDMh90jB7_EY4vqja2w

目标检测论文阅读：DetNet

https://zhuanlan.zhihu.com/p/55972055

SimpleDet:一套简单通用的目标检测与物体识别框架

https://mp.weixin.qq.com/s/Sl958JkcJjy-HW9_c-SH4g

Guided Anchoring: 物体检测器也能自己学Anchor

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

https://mp.weixin.qq.com/s/-G47vOGx2iNQCarYRAiNPg

基于区域分解集成的目标检测

https://mp.weixin.qq.com/s/rlmgN0LbUfd2n9MI8OMT2w

性能大幅度提升（速度&遮挡）:基于区域分解&集成的目标检测

https://zhuanlan.zhihu.com/p/59398728

CVPR2019目标检测方法进展综述

https://mp.weixin.qq.com/s/NWILStthG4klkwrYVcGQSQ

ILC：用于自然场景多目标的计数模型

https://mp.weixin.qq.com/s/9BCf0rCp660a5xQ2JNz3AQ

深入理解one-stage目标检测算法（上篇）

https://mp.weixin.qq.com/s/p9XaI8PSG0o1NWlkmCIn7w

深入理解one-stage目标检测算法（下篇）

https://mp.weixin.qq.com/s/reNCLvOyJHkJZimnMpbIig

目标检测算法优化技巧：Bag of Freebies for Training Object Detection

https://mp.weixin.qq.com/s/psXJNlEawZlQ-ZdktDpjOw

目标检测小tricks之样本不均衡处理

https://mp.weixin.qq.com/s/Pl8HABuVN27CZv-lvGROTw

基于深度学习的目标检测算法近5年发展历史
