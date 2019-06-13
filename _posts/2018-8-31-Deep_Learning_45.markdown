---
layout: post
title:  深度学习（四十五）——AutoDL, 图像超分辨率进阶
category: DL 
---

# AutoDL

DL领域目前存在的主要问题之一是：如何设计网络结构和调整超参数。目前的做法，通常依赖于作者的直觉，属于典型的拍脑袋想点子。

既然AI已经能够做很多事了，那么有没有可能，使用AI自动生成网络结构呢？

Google的这两篇论文在这里做了一些尝试：

《Neural Architecture Search With Reinforcement Learning》

《Learning Transferable Architectures for Scalable Image Recognition》

这里主要采用强化学习的方法，在一个广阔的搜索空间中，寻找最合适的网络结构。但对于计算能力提出了很高的要求。论文中提到，他们使用了500块GPU。**有钱真的是可以为所欲为的。**

这里学到的模型，一般被称为NASNet。

## 工具

https://mp.weixin.qq.com/s/9OZiFgziY8fMn-lkmdtiqQ

终结谷歌AutoML的真正杀手！Saleforce开源TransmogrifAI

https://mp.weixin.qq.com/s/GFjYzxf6IdKPvkij8-i_Dg

调参工要凉？微软重磅开源AutoML工具包NNI

https://mp.weixin.qq.com/s/L1nkhc4I6VX2s6S5Tiv0Zw

小白也能搭建深度模型，百度EasyDL的背后你知多少

![](/images/img2/AdaNet.gif)

https://mp.weixin.qq.com/s/HiD-OqAz67cwwchjSyIjWA

AutoML又一利器来了，谷歌宣布开源AdaNet

https://mp.weixin.qq.com/s/o3WqVIxlsukt3_JcY-10LA

AdaNet简介：快速灵活的AutoML，提供学习保证

https://mp.weixin.qq.com/s/2ehni5O__XnPaLjBRmAiIQ

利用AdaNet将多个TensorFlow Hub模块组合成一个集成网络

https://mp.weixin.qq.com/s/rllQLPPYxxLCK9-dgOjLDg

另一种可微架构搜索：商汤提出在反传中学习架构参数的SNAS

https://mp.weixin.qq.com/s/AV8fD5Vf6uzhLeW9nfMASg

一文详解随机神经网络结构搜索(SNAS)

## 书籍

https://mp.weixin.qq.com/s/wTwjbripELzqdCjs64Dzzw

告别调参，AutoML新书发布

## ENAS

https://mp.weixin.qq.com/s/SwJs0OUzBlhiIOyFHFBK_g

一文看懂Jeff Dean等提出的ENAS到底好在哪？

https://mp.weixin.qq.com/s/Jw2kzCf2uFibhIpREnjO_A

图解高效神经网络结构搜索（ENAS）

## DARTS

论文：

《DARTS: Differentiable Architecture Search》

代码：

https://github.com/quark0/darts

参考：

https://mp.weixin.qq.com/s/bjVTpdaKFx4fFtT8BjMNew

指数级加速架构搜索：CMU提出基于梯度下降的可微架构搜索方法（DARTS）

## 参考

https://github.com/hibayesian/awesome-automl-papers

自动机器学习（auto machine learning）相关资源列表

https://github.com/guan-yuan/awesome-AutoML-and-Lightweight-Models

一份高质量AutoML工作和轻量级模型的列表，包括神经结构搜索，轻量级结构，模型压缩和加速，超参数优化，自动特征工程。

https://mp.weixin.qq.com/s/Ia-8qFLAyY65Nai5PGAw0w

人人都能用的深度学习：当前三大自动化深度学习平台简介

https://mp.weixin.qq.com/s/tFzbJdW-L342tMNXDiacCg

AutoML研究综述：让AI学习设计AI

https://mp.weixin.qq.com/s/62cqtwslax_cvA57252zYA

IBM自动机器学习网络架构搜索最新综述

http://blog.csdn.net/u014380165/article/details/78525687

自学网络结构（二）：Learning Transferable Architectures for Scalable Image Recognition

http://blog.csdn.net/u014380165/article/details/78525500

自学网络结构（一）：Neural Architecture Search With Reinforcement Learning

https://www.zhihu.com/question/67477086

如何评价Google最新的论文NASNet？

https://mp.weixin.qq.com/s/Yh8Zj7Jhliy7a9h6mW8FxQ

解读谷歌NASNet：一个大规模图像识别架构!

https://mp.weixin.qq.com/s/MkXBtGq4xt5YOh1-uhMBbg

循环神经网络自动生成程序：谷歌大脑提出“优先级队列训练”

https://mp.weixin.qq.com/s/D0HngY-U7_fP4vqDIjvaew

自动选模型+调参：谷歌AutoML背后的技术解析

https://mp.weixin.qq.com/s/vctbsYk4LRwrQ7_Hs7fqkg

谷歌大脑发布神经架构搜索新方法：提速1000倍

https://mp.weixin.qq.com/s/tWO1Qv1aoemC8nHvJXZyeA

清华大学张长水教授：神经网络模型的结构优化

https://mp.weixin.qq.com/s/9qpZUVoEzWaY8zILc3Pl1A

进化算法+AutoML，谷歌提出新型神经网络架构搜索方法

https://mp.weixin.qq.com/s/HJ5caV1bQi7qVDeNXZ21qg

手把手教你用Cloud AutoML做毒蜘蛛分类器

https://zhuanlan.zhihu.com/p/35050923

跬步至千里：揭秘谷歌AutoML背后的渐进式搜索技术

https://mp.weixin.qq.com/s/wGt3Xk_ARqHei-ddU9iXNg

算力节省240倍！上交大、MIT新方法低成本达到谷歌AutoML性能

https://mp.weixin.qq.com/s/pXskwKkzCtgIuNtgFVF_wg

ImageNet分类精度再创新高！李飞飞组ECCV Oral提出全新渐进式神经结构搜索

https://mp.weixin.qq.com/s/GLiOZjqC9DSvGEX3xqbHJg

鸡生蛋与蛋生鸡，纵览神经架构搜索方法

https://mp.weixin.qq.com/s/DLpMVOmkvpWqlHIAZojwog

一文看懂深度学习新王者“AutoML”：是什么、怎么用、未来如何发展？

https://mp.weixin.qq.com/s/X6m4ZHDKY4gC-v0YN5g4YA

谷歌云提出渐进式神经架构搜索：高效搜索高质量CNN结构

https://mp.weixin.qq.com/s/0te0JUKYZlSLs3kzsFV-NA

神经网络架构搜索（NAS）综述

https://mp.weixin.qq.com/s/dSAjdx7jsKgI5SQpMY213w

微软&中科大提出新型自动神经架构设计方法NAO

https://mp.weixin.qq.com/s/KvkkmDIY7Y0NBgYF86E2Pw

神经网络突变自动选择AI优化算法，速度提升50000倍！

https://zhuanlan.zhihu.com/p/44576620

让算法解放算法工程师----NAS综述

https://mp.weixin.qq.com/s/3njCAKQsQ8UhL3gqDDHZGQ

近期大热的AutoML领域，都有哪些值得读的论文？

https://mp.weixin.qq.com/s/l6uzHwGSTY5xRSkPD_B2rw

深度学习模型超参数搜索实用指南

https://mp.weixin.qq.com/s/7dNB6V4gHdkN-N2td43NrA

一种简单有效的网络结构搜索

https://mp.weixin.qq.com/s/5H3wk93QoxHteR_HD2hWiA

DeepMind提出架构搜索新方法：使用分层表示，时间短精度高

https://zhuanlan.zhihu.com/p/46350372

语义分割领域开山之作：Google提出用神经网络搜索实现语义分割

https://mp.weixin.qq.com/s/VZVaWmCit5Ey7ZboeMbK7g

AutoML、AutoKeras......这四个“Auto”的自动机器学习方法你分得清吗？

https://mp.weixin.qq.com/s/DQcRqWALLuPpNabCv83Vcg

自动机器学习，神经网络自主编程

https://mp.weixin.qq.com/s?__biz=MzUyMjE2MTE0Mw==&mid=2247486970&idx=2&sn=6878205053d13467f66055a3314eeba1

如何利用高效的搜索算法来搜索网络的拓扑结构

https://mp.weixin.qq.com/s/Y8i25dZmj41dr8nz2WyNbQ

搜索一次就够了：中科院&图森提出通过稀疏优化进行一次神经架构搜索

https://mp.weixin.qq.com/s/45QCGUXJ2VM96PNj-LEFxg

麻省理工HAN Lab提出ProxylessNAS自动为目标任务和硬件定制高效CNN结构

https://mp.weixin.qq.com/s/xbkFUfJbaw_h_bCZj3pdAQ

李飞飞等人提出Auto-DeepLab：自动搜索图像语义分割架构

https://mp.weixin.qq.com/s/B0Qld2Xi8kaoe3hf8TN7rA

华中科大提出EAT-NAS方法：显著提升大规模神经模型搜索速度

https://mp.weixin.qq.com/s/RDMb8YD3K_E06RxAgwwOnw

雷军强推：小米造最强超分辨率算法，现已开源

https://mp.weixin.qq.com/s/IHYuRjSZTE0wqJJwf3wGmw

机器学习论文笔记—如何利用高效的搜索算法来搜索网络的拓扑结构

https://mp.weixin.qq.com/s/4hHoRDKqPT25ltU7nyaRAQ

旷视提出One-Shot模型搜索框架的新变体

https://mp.weixin.qq.com/s/5kynyTPod0AUIYhL5kIDbA

何恺明等人新作：效果超ResNet，利用NAS方法设计随机连接网络

https://zhuanlan.zhihu.com/p/62523100

NAS之Randomly Wired Neural Networks

https://mp.weixin.qq.com/s/20jDhSv1wazb9UD_bYRXFQ

用神经架构搜索实现更好的目标检测

https://mp.weixin.qq.com/s/rgbHJ_95tDf-kYMTO1YYcw

谷歌大脑提出NAS-FPN：这是一种学会自动架构搜索的特征金字塔网络

https://mp.weixin.qq.com/s/j-EA1ha1lcLtqJQJO8y0ZA

Google Brain新作：网络结构搜索NAS在物体检测金字塔FPN框架大放异彩

https://mp.weixin.qq.com/s/Awn_fNRpGVa1xhy-jsCOFQ

AutoML for Mobile Compression and Acceleration on Mobile Devices

https://mp.weixin.qq.com/s/rMpV2na0GJxiO1v0HRJjPA

何凯明团队《随机生成网络 RandWire》

https://mp.weixin.qq.com/s/ZTeIuBZzC44NhLNZkxJNQw

商汤使用AutoML设计Loss函数，全面超越人工设计

https://mp.weixin.qq.com/s/mQ01MkRCWL0gZVtVWZmbJw

谷歌发布颠覆性研究：不训练不调参，AI自动构建超强网络，告别炼丹一大步（WANN）

# 图像超分辨率进阶

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

基于多级神经纹理迁移的图像超分辨方法 (Adobe Research)
