---
layout: post
title:  深度学习（三十五）——姿态/行为检测, 深度目标跟踪, Mask R-CNN, DNC, Graph NN
category: DL 
---

# 姿态/行为检测

## OpenPose

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

## DensePose

与OpenPose类似的还有Facebook提出的DensePose。

论文：

《DensePose: Dense Human Pose Estimation In The Wild》

数据集：

http://densepose.org/

这里包含了一个名为DensePose-COCO的姿态数据集。

参考：

https://mp.weixin.qq.com/s/sFd9hrMrKDl5UJwlY6N7mw

Facebook提出DensePose数据集和网络架构：可实现实时的人体姿态估计

https://mp.weixin.qq.com/s/t29ITfRPD3yCmRD5wJyq7g

ICCV2017 PoseTrack challenge优异方法：基于检测和跟踪的视频中人体姿态估计

https://mp.weixin.qq.com/s/mGcKpu3BXlAGO-t2FUCxAg

基于深度模型的人脸对齐和姿态标准化

https://mp.weixin.qq.com/s/gwRD3SzTof349V8W0_lRfg

实时评估世界杯球员的正确姿势：FAIR开源DensePose

https://zhuanlan.zhihu.com/p/39219404

Dense Pose

https://blog.csdn.net/sinat_26917383/article/details/79704097

关键点定位：四款人体姿势关键点估计论文笔记

## 参考

https://mp.weixin.qq.com/s/6pNZ8Crs4Lel2C0TlFAc4Q

真能“穿墙识人”，MIT人体姿态估计系统创历史最高精度！

https://mp.weixin.qq.com/s/B0G-OLKXL7WZQgwOotxh2g

CVPR大规模行为识别竞赛连续两年夺冠，上交大详细技术分享

https://mp.weixin.qq.com/s/X-0txgjl129MLv6Dnv_t8g

剑桥大学等研发“暴力行为”检测系统，用无人机精准识别人群暴力

https://mp.weixin.qq.com/s/-kXWmaIfw67rvb8IRi5mCQ

视频行为理解新边界

https://mp.weixin.qq.com/s/Ns8oD_HBpFoumMMDbMq8-g

DeepMind视频行为分类竞赛，百度IDL获第一

https://mp.weixin.qq.com/s/4baaoCCdGX4iTw2MO_Y9rA

熊元骏：人类行为理解研究进展

https://mp.weixin.qq.com/s/pTlELRhUL9Zg50HXDWjbqw

基于双流递归神经网络的人体骨架行为识别！

https://mp.weixin.qq.com/s/pnCdKcdRCxhFEcXyWb9xlQ

深度学习助力实现智能行为分析和事件识别

https://mp.weixin.qq.com/s/uxawHWsVXMNOTLNthAL0vg

时空图卷积网络：港中文提出基于动态骨骼的行为识别新方案

https://mp.weixin.qq.com/s/g6032xTGEtvbsfwXboMJ4A

大阪大学副校长Yasushi Yagi：步态分析

http://mp.weixin.qq.com/s/Y-PvMz_Vz8nBGRZo9dwUCA

中科院步态识别技术：不看脸50米内在人群中认出你！

https://mp.weixin.qq.com/s/0KaSWHi8r9kSXxlwswIY5g

乔宇：深度模型让机器理解场景

https://mp.weixin.qq.com/s/CSSN0K_9CYm5MnplUqU5xg

乔宇: 面向复杂行为理解的深度学习模型及应用

https://mp.weixin.qq.com/s/3Pe5wJ0VomzwKMF84OqcMg

步态识别的深度学习综述

https://mp.weixin.qq.com/s/ZRS58dqdfeNczVNufoUXgQ

基于交互感知的时空金字塔注意力机制神经网络的行为分类

https://mp.weixin.qq.com/s/6WmXY1-BmdOHkSRA_Ds9IQ

密集人体姿态估计：2D图像帧可实时生成UV贴图

https://mp.weixin.qq.com/s/1yV8qz-9-mV8Loz2b2iJNQ

一文看懂如何将深度学习应用于视频动作识别

https://mp.weixin.qq.com/s/_LXH2-il65wkpRUQZpfU2g

检测与跟踪：快速视频姿态估计

https://mp.weixin.qq.com/s/daGdfe8c5RACq6hjh1ORVw

MultiPoseNet:人体检测、姿态估计、语义分割一“网”打尽

https://mp.weixin.qq.com/s/aJvnzck__8z4DaPPgzT_sA

美图云联合中科院提出基于交互感知注意力机制神经网络的行为分类技术

https://mp.weixin.qq.com/s/WtDZs3rsEBWrTqTUCHDFxg

中山大学&商汤提出部分分组网络PGN，解决实例级人体解析难题

# 深度目标跟踪

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

Pytorch版本：

https://github.com/wannabeOG/Mask-RCNN

![](/images/img2/mask_rcnn.png)

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

https://mp.weixin.qq.com/s/UXzhMkGIwqek4zHVNPgRbA

Mask-RCNN论文解读

https://mp.weixin.qq.com/s/_ohsx7kzgU-szP-K9_Yv1w

优于Mask R-CNN，港中文&腾讯优图提出PANet实例分割框架

https://mp.weixin.qq.com/s/uJpVqRpWWaK2cY8fYGlRag

先理解Mask R-CNN的工作原理，然后构建颜色填充器应用

https://mp.weixin.qq.com/s/x_9klKK_hIiFV1fGhxZIVA

Mask R-CNN神应用：像英剧《黑镜》一样屏蔽人像

https://mp.weixin.qq.com/s/V6m1xBS2vZQ6VRlAg5zOSA

干掉照片中那些讨厌的家伙！Mask R-CNN助你一键“除”人！

https://mp.weixin.qq.com/s/48eIhnBdYzgEiV_wESHsJA

如何使用Mask RCNN模型进行图像实体分割？

# LSM

liquid state machine (LSM)

http://www.docin.com/p-390935406.html

基于液体状态机的脑运动神经系统的建模研究

# DNC

https://zhuanlan.zhihu.com/p/27773709

浅析至强RNN可微分神经计算机(DNC)

https://zhuanlan.zhihu.com/p/27964341

浅析至强RNN可微分神经计算机(DNC)-2

https://zhuanlan.zhihu.com/p/28209628

DNC-3滚动分类的模式识别

https://zhuanlan.zhihu.com/p/28433712

DNC4广义线性回归

# Graph NN

https://mp.weixin.qq.com/s/_aydey5ZVwrObmoFXXIYcw

Bengio等人提出图注意网络架构GAT，可处理复杂结构图

https://zhuanlan.zhihu.com/p/34232818

《Graph Attention Networks》阅读笔记

https://zhuanlan.zhihu.com/p/28170197

《Gated Graph Sequence Neural Networks》阅读笔记

https://mp.weixin.qq.com/s/Pm1HiEQOBnbo_GQ_v6Y_zw

腾讯提出自适应图卷积神经网络，接受不同图结构和规模的数据

https://mp.weixin.qq.com/s/bMpugd2Lp35VPr8fQAPzsg

一文概览图卷积网络基本结构和最新进展

https://zhuanlan.zhihu.com/p/31067515

《Semi-Supervised Classification with Graph Convolutional Networks》阅读笔记

https://mp.weixin.qq.com/s/6viSk0Ts_7eTfYrWYi_HDQ

基于图结构的实体和关系联合抽取模型简介

https://mp.weixin.qq.com/s/w5ldyp00CqkX8Kp-8Aw0nQ

图深度学习(GraphDL)，下一个人工智能算法热点？一文了解最新GDL相关文章

https://mp.weixin.qq.com/s/Jt6CjMqNFEXWoL5pkLeVyw

洛桑理工：Graph上的深度学习报告

https://zhuanlan.zhihu.com/p/36117802

《Learn to Represent Programs with Graphs》阅读笔记。这篇论文讲述了DL在程序代码纠错方面的应用。

https://zhuanlan.zhihu.com/p/37278426

Graph2Seq: Graph to Sequence Learning with Attention-based Neural Networks

https://mp.weixin.qq.com/s/iQYVyo2PHuGbEsYgdIf_oQ

DeepMind等机构提出“图网络”：面向关系推理

https://mp.weixin.qq.com/s/TAccHagxXQ82lfE91Y6xWg

CNN已老，GNN来了：重磅论文讲述深度学习的因果推理

https://mp.weixin.qq.com/s/UONtTJJgDawRPWtatAVKkg

如何利用高效的搜索算法来搜索网络的拓扑结构

https://mp.weixin.qq.com/s/lt9lZbulkW0C8A_xi6hodQ

浅析图卷积神经网络

https://mp.weixin.qq.com/s/SGCtwYWfnxjcpMJeeH1b4w

图神经网络+池化模块，斯坦福等提出层级图表征学习

# 垃圾筐

http://www.matrix67.com/

一个数学blog

http://blog.huajh7.com/2013/03/06/variational-bayes/

变分贝叶斯算法理解与推导

https://www.zhihu.com/question/267166775

真空中有两个位置固定的引力源(恒星)，一个质量较小的小球(行星)有哪些典型的稳定绕转方式?

http://www.matrix67.com/blog/archives/6750

趣题：两个方阵是怎样互相穿过对方的？

https://mp.weixin.qq.com/s/r95iMIKMctgjko1GRn9-5A

史上最贱的数学题

https://mp.weixin.qq.com/s/BkKw2C3n9WsmIchJkkZxUw

从七桥问题开始：全面介绍图论及其应用

https://mp.weixin.qq.com/s/ZDY3Yt67eXK5pjXgvJkkyQ

图论的各种基本算法

https://mp.weixin.qq.com/s/2h1dgvPbYKBOYZPiixg9iw

手把手：四色猜想、七桥问题…程序员眼里的图论，了解下？

