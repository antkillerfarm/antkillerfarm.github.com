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

# 世说新语++

## 2020.9（续）

有一天，萨镇冰将军因通宵达旦的批改文件，过度劳累的他就趴在桌子上睡着了。等他醒来之后，却发现自己身上盖了一条被子。谁盖的被子？他很好奇，就询问是何人所为，身旁的亲卫们也说不知道。后来，经过查询后，萨将军才知道，这是一个名叫陈兆汉的普通士兵帮他盖上的。

其后，萨镇冰将军经过认真考察，看中了这个当兵的是块料，就将自己年仅20岁的女儿萨淑端嫁给了陈兆汉。

陈兆汉有个哥哥叫陈兆雄，兆雄有个儿子叫陈绍宽。1906年，陈绍宽在叔叔陈兆汉的推荐下，在江南水师学堂学习，攻读航海技术。民国后，陈绍宽在北洋政府的海军之中受到重用。后来，陈绍宽更是担任了中华民国的海军总司令，国民革命军陆军、海军一级上将。

----

苏格兰传教士马礼逊在1820年发表于《察世俗每月统记传》上的《法兰西国作变平复略传》里将他译成“拿破戾翁·破拿霸地”，不光“破”没少，还自带了戾气老翁属性，且有影射妄图称霸之意，行文中固然承认其赫赫武功，含沙射影之处却也不少，甚至给拿皇主动寻找野爹。

https://www.zhihu.com/question/26989877

为什么Napoleon要翻译成“拿破仑”这样具有贬义的名字？

----

https://www.sohu.com/a/144246014_349972

1906年大清“彰德秋操”汇聚五位民国总统！

----

在民国以前，河州诸马军事力量为 “西军”时期，仍延续马占鳌降清时的格局，即以马占鳌子系（子马安良）为首，辖制马海宴子系（子马麒，马麟）和马千龄子系（子马福禄，马福祥）。

民国后，马占鳌子系在政争中淡出历史，马海宴子系和马千龄子系分据青海（青马）和宁夏（宁马）。

他们以“甘、河、回、马”（即甘肃人、河州人-今甘肃省临夏人，解放前临夏称河州、回族、马姓）这四条为用人标准，核心权力采取父死子继，兄终弟及的封建继承方式，经数十年的发展，逐渐成为左右西北局势的军阀武装。

马步芳为人荒淫无耻，在国民党上层中少见。在大陆时，他曾公开说：“生我、我生者外无不奸。”部属的妻女，自己家族的胞妹、侄女、兄嫂、弟媳，都难逃他的魔爪。甚至连他的外孙女，也遭其强奸。

----

湖北黄冈的林氏家族，出了3位在中国现代史上不同寻常的人物——林育南、林育英（又名张浩）和林彪（原名林育容）。
