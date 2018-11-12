---
layout: post
title:  深度学习（三十五）——姿态/行为检测, 深度目标跟踪, VAE进阶
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

https://mp.weixin.qq.com/s/HY9fRbiQrFARPMUsNANndA

最高比Mask-RCNN快3倍！上交大实时姿态估计AlphaPose升级

https://zhuanlan.zhihu.com/p/37526892

OpenPose：实时多人2D姿态估计

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

https://mp.weixin.qq.com/s/4oA-x2sofqR3GBEHlmeQrg

人体姿态估计资源大列表

https://mp.weixin.qq.com/s/GSUfhLxx0l-w04Et5b3SNg

人体骨骼关键点检测综述

https://mp.weixin.qq.com/s?__biz=MzU2OTA0NzE2NA==&mid=2247500351&idx=2&sn=aaaecdca274c6322fb06c7ee0e2696a2

157页基于相似度的动作捕捉数据处理教程

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

https://mp.weixin.qq.com/s/NuWP9v_5b2-V7rPdyqtM0A

DanceNet：帮你生成会跳舞的小姐姐

https://mp.weixin.qq.com/s/UvwUu0cwpk_gvMmBGX1vfQ

嘿嘿，想变成会跳舞的小哥哥或小姐姐吗？超简单！

https://mp.weixin.qq.com/s/7YzkgpXRCgYqguTng4nf7g

超越CycleGAN：这个人体动态迁移技术让白痴变舞王

https://mp.weixin.qq.com/s/naD6hVwDg3QSA76sXy2GZA

上海交通大学CVPR Spotlight论文：利用形态相似性生成人体部位解析数据

https://mp.weixin.qq.com/s/L8TlPWBfirF09xKLT3bWVg

最佳学生论文：EPFL&FAIR提出QuaterNet，更好地解决人类动作建模问题

https://mp.weixin.qq.com/s/AmJ4SRgNG8XHNpxPfDOjBg   

Facebook开发姿态转换模型，只需一张照片就能让它跳舞

https://mp.weixin.qq.com/s/hfnx7dLomPlQz-7OLl66iQ

YOLO遇上OpenPose，近200FPS的高帧数多人姿态检测

https://mp.weixin.qq.com/s/hpWDqyLclqEfWwakFZtBIQ

用DensePose，教照片里的人学跳舞，系群体鬼畜

https://mp.weixin.qq.com/s/OUFGRHa-uYKUg_giGmL6yQ

机器人读懂人心的九大模型。一个动作识别的blog

https://mp.weixin.qq.com/s/JkbM8Af8ApTO4fkxCUTOoA

浅谈动作识别TSN, TRN, ECO

https://mp.weixin.qq.com/s/DCTxmHg9aX3BqCAckv_S0g

不再需要动作捕捉，伯克利推出“看视频学杂技”的AI智能体

https://mp.weixin.qq.com/s/sZskFg2KbUbt88O0MaEM_g

检测与识别人与目标之间的互动

https://mp.weixin.qq.com/s?__biz=MzI5MDUyMDIxNA==&mid=2247486245&idx=1&sn=619f782d1758ee9daebd38637c0faa7f

行为识别论文笔记之多纤维网络

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

https://mp.weixin.qq.com/s/tr1oWpaRZair9zG5CHWvbg

视觉目标跟踪之DaSiamRPN

https://mp.weixin.qq.com/s/Nd5XLnncXRawu4V8TOzHxg

视觉物体跟踪新进展：让跟踪器读懂目标语义信息

https://zhuanlan.zhihu.com/p/46669238

VOT2018：SiamNet大崛起

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

https://zhuanlan.zhihu.com/p/36462982

多目标追踪算法：条件随机场算法

https://mp.weixin.qq.com/s/Lmvx_KurxG3nfmvhXAD_tQ

一种轻量级在线多目标车辆跟踪方法

https://mp.weixin.qq.com/s/zlo59TVpSq5w6zTwJLjbXg

视觉多目标跟踪算法综述（上）

https://mp.weixin.qq.com/s/XwMXrsmSnImgD1vNSVErLg

深度多目标跟踪算法综述

# VAE进阶

https://mp.weixin.qq.com/s/6G1y2xMclUyzz_GQzKDrIw

变分U-Net，可按条件独立变换目标的外观和形状

https://mp.weixin.qq.com/s/ZlLuhu08m_RnD-h86df8sA

清华大学提出SA-VAE框架，通过单样本/少样本学习生成任意风格的汉字

https://mp.weixin.qq.com/s/t4YYIl4o_TAPG7737ZfiaA

面向无监督任务：DeepMind提出神经离散表示学习生成模型VQ-VAE

https://mp.weixin.qq.com/s/51Xu7osdVa-fCV-IZbHdCA

Wasserstein自编码器

https://mp.weixin.qq.com/s/0HK026K6jru10VscvT2rOQ

哈佛大学提出变分注意力：用VAE重建注意力机制

https://mp.weixin.qq.com/s/790wbFnxkNbNRampiV-0MQ

谷歌大脑提出对抗正则化方法，显著改善自编码器的泛化和表征学习能力

https://mp.weixin.qq.com/s/iOdh1iIP0GIYe4gRDE0z-g

漫谈概率PCA和变分自编码器

https://mp.weixin.qq.com/s/pBnKNRc56HhBWvrYaZjGdw

稳定、表征丰富的球面变分自编码器

https://mp.weixin.qq.com/s/QOdQKdLolR-YTihzaA81yw

黄怀波 ：自省变分自编码器理论及其在图像生成上的应用

https://mp.weixin.qq.com/s/FqY9I02blg3S8_K50B7czQ

UC伯克利提出小批量MH测试：令MCMC方法在自编码器中更强劲
