---
layout: post
title:  深度学习（二十四）——深度目标跟踪, L2 Normalization
category: DL 
---

# LSTM进阶（续）

## 《On the compression of recurrent neural networks with an application to lvcsr acoustic modeling for embedded speech recognition》

![](/images/img2/RNN_SVD.png)

这是另一篇RNN模型压缩的论文。上图中的a是原始RNN，而b是对$$W_h^l$$使用了SVD之后的RNN。

## 《Video Summarization with Long Short-term Memory》

这是一篇用于提取视频关键帧（也叫静态视频摘要）的论文，是南加州大学沙飞小组的作品。

![](/images/img2/dppLSTM.png)

上图是该文提出的DPP LSTM的网络结构图。它的主体是一个BiLSTM，算是中规中矩吧。

该文的创新点在于提出了DPP loss的概念。上图中的$$y_t$$表示帧的分值（越大表示越重要），$$\phi_t$$表示帧之间的相似度。该文的实验表明，将两个特征分开抽取，有助于提升模型的准确度。

这篇论文主要用到了3个数据集：

TVSum dataset: 

https://github.com/yalesong/tvsum

这个需要Yahoo账号和一个高校的邮件地址才行。

SumMe dataset: 

https://people.ee.ethz.ch/~gyglim/vsum/#benchmark

OVP and YouTube datasets: 

https://sites.google.com/site/vsummsite/

需要翻墙。

## LSTM运算加速技巧

LSTM的主要运算量集中在$$W[h_{t-1},x_t]$$上，这里实际上可以用$$W[(h_{t-2}+\Delta h_{t-1}),x_t]$$代替。

由于时间序列通常具有惯性，因此$$\Delta h_{t-1}$$一般包含了大量的0，这对于某些具有跳0功能的硬件来说，是非常有利的。

## Convolutional LSTM Network

论文：

《Convolutional LSTM Network: A Machine Learning Approach for Precipitation Nowcasting》

## 参考

https://mp.weixin.qq.com/s/4IHzOAvNhHG9c8GP0zXVkQ

Simple Recurrent Unit For Sentence Classification

https://mp.weixin.qq.com/s/fCzHbOi7aJ8-W9GzctUFNg

LSTM文本分类实战

http://mp.weixin.qq.com/s/3nwgft9c27ih172ANwHzvg

从零开始：如何使用LSTM预测汇率变化趋势

https://mp.weixin.qq.com/s/M18c3sgvjV2b2ksCsyOxbQ

Nested LSTM：一种能处理更长期信息的新型LSTM扩展

https://mp.weixin.qq.com/s/XAbzaMXP3QOret_vxqVF9A

用深度学习LSTM炒股：对冲基金案例分析

https://mp.weixin.qq.com/s/eeA5RZh35BvlFt45ywVvFg

可视化LSTM网络：探索“记忆”的形成

https://mp.weixin.qq.com/s/h-MYTNTLy7ToPPEZ2JVHpw

阿里巴巴论文提出Advanced LSTM：关于更优时间依赖性刻画在情感识别方面的应用

https://mp.weixin.qq.com/s/pv3gQfCayGmsmGKLbMIFpA

神奇！只有遗忘门的LSTM性能优于标准LSTM

https://mp.weixin.qq.com/s/8BPZ_M8EGk3KxkSleYWSNw

训练可解释、可压缩、高准确率的LSTM

https://hanxiao.github.io/2018/06/24/4-Encoding-Blocks-You-Need-to-Know-Besides-LSTM-RNN-in-Tensorflow/

4 Sequence Encoding Blocks You Must Know Besides RNN/LSTM in Tensorflow

https://mp.weixin.qq.com/s/zMCBQ2D21HoDcDgDolmGMA

上海交大：基于近似随机Dropout的LSTM训练加速

https://mp.weixin.qq.com/s/wPYd2jLUPzlPwIZkb_wSbA

深度递归LSTM-LRP非线性时变多因子模型

https://mp.weixin.qq.com/s/3djAWJs6ecDdPSpQMxqmrg

清华、李飞飞团队等提出强记忆力E3D-LSTM网络

https://mp.weixin.qq.com/s/__qC6Wzy4jaGNGzWr3eOIg

引入额外门控运算，LSTM稍做修改，性能便堪比Transformer-XL

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
