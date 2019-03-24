---
layout: post
title:  深度学习（十七）——深度目标跟踪, 无监督/半监督/自监督深度学习, 细粒度分类, Spatial Transformer Networks
category: DL 
---

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

https://mp.weixin.qq.com/s/dB5u2No8eakLnrjto0kvyQ

DaSiamRPN的升级版，视觉目标跟踪之SiamRPN++

https://mp.weixin.qq.com/s/7hScDZQfv42nBKdYu4RA0Q

惊艳的SiamMask：开源快速同时进行目标跟踪与分割算法

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

https://mp.weixin.qq.com/s/5Kcbnme7151Ln6KiTyVaBw

多目标追踪资源列表

https://zhuanlan.zhihu.com/p/36462982

多目标追踪算法：条件随机场算法

https://mp.weixin.qq.com/s/Lmvx_KurxG3nfmvhXAD_tQ

一种轻量级在线多目标车辆跟踪方法

https://mp.weixin.qq.com/s/zlo59TVpSq5w6zTwJLjbXg

视觉多目标跟踪算法综述（上）

https://mp.weixin.qq.com/s/XwMXrsmSnImgD1vNSVErLg

深度多目标跟踪算法综述

https://mp.weixin.qq.com/s/KLTSUqprwfFBVeVIP7HJRw

视频中的多目标跟踪

https://zhuanlan.zhihu.com/p/55738110

商汤等提出：统一多目标跟踪框架

https://mp.weixin.qq.com/s/PGs3M5-vdQf4deE6hJuqng

多目标跟踪：SORT和Deep SORT

# 无监督/半监督/自监督深度学习

自监督学习是一种特殊目的的无监督学习。不同于传统的AutoEncoder等方法，仅仅以重构输入为目的，而是希望通过surrogate task学习到和高层语义信息相关联的特征。

## 参考

https://mp.weixin.qq.com/s/L4GQF0eE7MjLPrb8UygCww

无监督深度学习全景教程（193页PDF）

https://mp.weixin.qq.com/s/kbqTHIOzAj1aERl4tm-kVA

2017上半年无监督特征学习研究成果汇总

https://mp.weixin.qq.com/s/J50L6hESBROfT8IIAnofQQ

Yan LeCun109页最新报告：图嵌入, 内容理解，自监督学习

https://mp.weixin.qq.com/s/s440gdbUhLP41rLPjfgsmQ

Yann Lecun自监督学习指南（附114页Slides全文下载）

https://mp.weixin.qq.com/s/foP1xSa5G8oNtAv_pI6AqQ

深度神经网络自监督视觉特征学习综述

https://mp.weixin.qq.com/s/aCWAU2RXk9fTzfFqOyjqUw

能自主学习的人工突触，为无监督学习开辟新的路径

https://mp.weixin.qq.com/s/9kMz-eNRwC51Fi0-7BfKzA

Active Learning: 一个降低深度学习时间，空间，经济成本的解决方案

https://mp.weixin.qq.com/s/ZvTm9omnIRqPXcLFbZtoeg

深度学习的关键：无监督深度学习简介

https://mp.weixin.qq.com/s/GHjmiB6F2W3Zo8gVllTyyQ

重现“世界模型”实验，无监督方式快速训练

https://mp.weixin.qq.com/s/3_VtdZNKBwNtMEMf2xc7qw

CVPR智慧城市挑战赛：无监督交通异常检测，冠军团队技术分享

https://mp.weixin.qq.com/s/3aAaM1DWsnCWEEbP7dOZEg

伯克利等提出无监督特征学习新方法，代码已开源

https://mp.weixin.qq.com/s/ZDPPWH570Vc6e1irwP1b1Q

精细识别现实世界图像：李飞飞团队提出半监督适应性模型

https://mp.weixin.qq.com/s/X1Alcl7rVfTtZGZ40iXjXw

Spotlight 论文：非参数化方法实现的极端无监督特征学习

https://mp.weixin.qq.com/s/kxEfoSjCF8n2jxlDfMaNDA

半监督学习在图像分类上的基本工作方式

https://mp.weixin.qq.com/s/uUMPUdG2TI10W5RumPaXkA

DeepMind无监督表示学习重大突破：语音、图像、文本、强化学习全能冠军！

https://mp.weixin.qq.com/s/_VC6PGdCjlhcsndpunIteg

何恺明等人提出新型半监督实例分割方法：学习分割Every Thing

https://mp.weixin.qq.com/s/qaxzSSDuuscwL5tt0QCQ0Q

破解人类识别文字之谜：对图像中的字母进行无监督学习

https://mp.weixin.qq.com/s/IsLlzDWnUXe8LVp4Y1Jb_A

35亿张图像！Facebook基于弱监督学习刷新ImageNet基准测试记录

https://mp.weixin.qq.com/s/TEk_i4kEjUqmAqF8LgTVjg

FAIR提出用聚类方法结合卷积网络，实现无监督端到端图像分类

https://mp.weixin.qq.com/s/dSncg1pDHpIFOT4mXrFntA

Yan Lecun自监督学习：机器能像人一样学习吗？ 110页PPT

https://mp.weixin.qq.com/s/W4zwKqkVQN4v-IKzGrkudg

通过传递不变性实现自监督视觉表征学习

https://zhuanlan.zhihu.com/p/30265894

自监督学习近期进展

https://mp.weixin.qq.com/s/cTlXMxcpzc7_5NVsTm1jcA

学习一帧，为整段黑白视频上色：谷歌提出自监督视觉追踪模型

https://mp.weixin.qq.com/s/Amr34SdrPZho1GQpFS7WBA

见微知著：语义分割中的弱监督学习

https://mp.weixin.qq.com/s/zOWA1oKbopZJuYIAYYlKTA

港中文-商汤联合论文：自监督语义分割的混合与匹配调节

https://mp.weixin.qq.com/s/5xlSoC5sgzsAwMYMSFCjnw

TextTopicNet:CMU开源无标注高精度自监督模型

https://mp.weixin.qq.com/s/343DfjOvkaozuxNK89V3zQ

前景目标检测的无监督学习

https://mp.weixin.qq.com/s/DwY0oGu-G30Szs-ArI5WaQ

程明明：面向弱监督的图像理解

https://mp.weixin.qq.com/s/LFOljv-Hr6JqyI6TQ2X4sw

半监督学习也能自动化？南大和第四范式提出Auto-SSL

https://mp.weixin.qq.com/s/83xAXrc_H_OExW3vii08hA

谷歌提出新方法：基于单目视频的无监督深度学习结构化

https://mp.weixin.qq.com/s/gr0_p4WFToTrDfy47h-p0A

基于自监督学习的视听觉信息同一性判断

https://mp.weixin.qq.com/s/Dqz97_U5pw_4d9KFblJfLg

基于自编码器的表征学习：如何攻克半监督和无监督学习？

https://mp.weixin.qq.com/s/LaIvAuBHYGNMug3NZ1pLhQ

半监督深度学习小结：类协同训练和一致性正则化

https://mp.weixin.qq.com/s/aBDgV7u93MAv2MogZKBmvw

Google提出Grasp2Vec模型：利用自监督方法学习物体表示

https://mp.weixin.qq.com/s/YfDZMEkOnxp0_ei2Oam-YQ

基于弱监督的视频时序动作检测的介绍

https://mp.weixin.qq.com/s/RiL-s50oOI--PZyIOd2E0g

弱监督语义分割最新方法资源列表

https://mp.weixin.qq.com/s/USOWECXk_az4b6eTssfOBw

基于弱监督深度学习的图像分割方法综述

https://mp.weixin.qq.com/s/8oEdQOmSRrkIaTVQdhk2Dw

无监督领域特定单图像去模糊

https://mp.weixin.qq.com/s/FpIaa8XoJ9GsHxL-W1Cl5Q

斯坦福AI实验室机器学习编程新范式：弱监督

# 细粒度分类

https://mp.weixin.qq.com/s/sdQ0rWbDDMN_P0B_RiYZmw

分段映射：帮助利用少量样本习得新类别细粒度分类器

https://mp.weixin.qq.com/s/zeN7rjmAnvh_7BbTmScrZw

细粒度分类你懂吗？——fine-gained image classification

https://mp.weixin.qq.com/s/LtWMGRBk2sbPDjeC9PmJ7g

弱监督学习下的商品识别：CVPR 2018细粒度识别挑战赛获胜方案简介

https://mp.weixin.qq.com/s/hcoAL1AHm_HtderWU8fSBw

大连理工大学在CVPR18大规模精细粒度物种识别竞赛中获得冠军

https://mp.weixin.qq.com/s/31r9FjuJn9yxrZMnfozkMQ

全卷积注意网络的细粒度识别

https://zhuanlan.zhihu.com/p/24738319

“见微知著”——细粒度图像分析进展综述

https://zhuanlan.zhihu.com/p/42067661

CVPR Look Closer to See Better

https://mp.weixin.qq.com/s/52hm3Cq3TFRnTMfDppivSQ

中山大学等提出HSE：基于层次语义嵌入模型的精细化物体分类

https://zhuanlan.zhihu.com/p/48192930

Object-Part Attention Model for FGVC

https://mp.weixin.qq.com/s/slmod5rW4qRhxGnbNN2J8g

双线性汇合(bilinear pooling)在细粒度图像分析及其他领域的进展综述

https://mp.weixin.qq.com/s/JGQdHS_yqkOMrN_Z3jEb7A

基于深度学习的细粒度图像分类综述

# Spatial Transformer Networks

论文：

《Spatial Transformer Networks》

参考：

http://www.cnblogs.com/neopenx/p/4851806.html

Spatial Transformer Networks(空间变换神经网络)

http://blog.csdn.net/shaoxiaohu1/article/details/51809605

论文笔记：Spatial Transformer Networks

http://blog.csdn.net/shaoxiaohu1/article/details/51809605

Spatial Transformer Networks

https://mp.weixin.qq.com/s/ciqQMezcB-oM24X8eQqTNg

花式玩耍Spatial Transformation Networks

https://mp.weixin.qq.com/s/4VE2lZeFf05AyLp_s3nTFQ

理解Spatial Transformer Networks
