---
layout: post
title:  深度学习（四十九）——模型压缩与加速（2）, Mask R-CNN
category: DL 
---

# 模型压缩与加速

https://mp.weixin.qq.com/s/_JlaxwEYqdTTuS4hNSQTTw

这么Deep且又轻量的Network，实时目标检测

https://mp.weixin.qq.com/s/mFuxCl0Mzv5hmDFewWZkrw

FAIR&MIT提出知识蒸馏新方法：数据集蒸馏

https://mp.weixin.qq.com/s/IxVMMu_7UL5zFsDCcYfzYA

AutoML自动模型压缩再升级，MIT韩松团队利用强化学习全面超越手工调参

https://mp.weixin.qq.com/s/fU-AeaPz-lHlg0CBgqnpZQ

轻量化神经网络综述

https://mp.weixin.qq.com/s/BMsvhXytSy2nWIsGOSOSBQ

自动生成高效DNN，适用于边缘设备的生成合成工具FermiNets

https://mp.weixin.qq.com/s/nEMvoiqImd0RxrskIH7c9A

仅17KB、一万个权重的微型风格迁移网络！

https://mp.weixin.qq.com/s/pc8fJx5StxnX9it2AVU5NA

基于手机系统的实时目标检测

https://mp.weixin.qq.com/s/6wzmyhIvUVeAN4Xjfhb1Yw

论文解读：Channel pruning for Accelerating Very Deep Neural Networks

https://mp.weixin.qq.com/s/-X7NYTzOzljzOaQL7_jOkw

惊呆了！速度高达15000fps的人脸检测算法！

https://mp.weixin.qq.com/s/6eyEMW9dVBR5cZrHxn8iqA

腾讯AI Lab详解3大热点：模型压缩、自动机器学习及最优化算法

https://xmfbit.github.io/2018/02/24/paper-ssl-dnn/

论文-Learning Structured Sparsity in Deep Neural Networks

https://mp.weixin.qq.com/s/d6HFVbbHwkxPGdnbyVuMyQ

密歇根州立大学提出NestDNN：动态分配多任务资源的移动端深度学习框架

https://mp.weixin.qq.com/s/lUTusig94Htf7_4Z3X1fTQ

清华&伯克利ICLR论文：重新思考6大剪枝方法

https://mp.weixin.qq.com/s/g3y9mRhkFtzSuSMAornnDQ

韩松博士论文：面向深度学习的高效方法与硬件

https://mp.weixin.qq.com/s/aH1zQ7we8OE59-O9n4IXhw

应对未来物联网大潮：如何在内存有限的情况下部署深度学习？

https://mp.weixin.qq.com/s/IfvXrsUq8-cBDC4_3O5v_w

Facebook新研究优化硬件浮点运算，强化AI模型运行速率

https://mp.weixin.qq.com/s/Jsxiha_BFtWVLvO4HMwJ3Q

工业界第一手实战经验：深度学习高效网络结构设计

https://mp.weixin.qq.com/s/uXbLb5ITHOU0dZRSWNobVg

算力限制场景下的目标检测实战浅谈

https://mp.weixin.qq.com/s/DoeoPGnS88HQmxagKJWLlg

小米开源FALSR算法：快速精确轻量级的超分辨率模型

https://mp.weixin.qq.com/s/wT39oUWfrQK-dg7hGXRynQ

实时单人姿态估计，在自己手机上就能实现

https://mp.weixin.qq.com/s/GJ7JMtWiKBku7dVJWOfLOA

CNN能同时兼顾速度与准确度吗？CMU提出AdaScale

https://mp.weixin.qq.com/s/pmel2k2J159zQi87ib3q8A

如何让CNN高效地在移动端运行

https://mp.weixin.qq.com/s/m-wQRm3VpfQkEOoUAxEdoA

论文解读: Quantized Convolutional Neural Networks for Mobile Devices

https://mp.weixin.qq.com/s/w7O2JxDH2ECqPn50sLfxpg

不用重新训练，直接将现有模型转换为MobileNet

https://mp.weixin.qq.com/s/EW6jvf98ifBucVz74SfSIA

文档扫描：深度神经网络在移动端的实践

https://mp.weixin.qq.com/s/FvR6loJ8KUxm7qwclestcQ

专门为卷积神经网络设计的训练方法：RePr

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

https://mp.weixin.qq.com/s/G_2tuZlaxX5w-2c1DO8FwQ

利用边缘监督信息加速Mask R-CNN实例分割训练

https://mp.weixin.qq.com/s/Ug4ZEQWVF5UjhqWw4Kwb8A

Mask R-CNN抢车位，快人一步！

https://zhuanlan.zhihu.com/p/47579399

R-CNN、Fast/Faster/Mask R-CNN、FCN、RFCN 、SSD原理简析

https://mp.weixin.qq.com/s/CsEHuGz_fAq8eWpHRq7d6g

性能超越何恺明Mask R-CNN！华科硕士生开源图像分割新方法

https://mp.weixin.qq.com/s/xug0xKfc9RgJEUci1a_xog8

实例分割的进阶三级跳：从 Mask R-CNN 到 Hybrid Task Cascade
