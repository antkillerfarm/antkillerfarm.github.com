---
layout: post
title:  深度目标检测（八）——花式IOU, 3D目标检测
category: Deep Object Detection 
---

* toc
{:toc}

# 花式IOU

https://zhuanlan.zhihu.com/p/57992040

使用GIoU作为检测任务的Loss

https://mp.weixin.qq.com/s/ZbryNlV3EnODofKs2d01RA

目标检测回归损失函数简介：Smooth L1/IoU/GIoU/DIoU/CIoU Loss

https://mp.weixin.qq.com/s/F07Wp-cXIOE4-qdL55WtJQ

基于DIou改进的YOLOv3目标检测

https://mp.weixin.qq.com/s/YeyxuN6RCgF2TcxctXiJQg

使用GIoU作为目标检测新loss

https://mp.weixin.qq.com/s/m8D5uutwlvkQNucFX_TnGw

DIoU和CIoU：IoU在目标检测中的正确打开方式

https://mp.weixin.qq.com/s/VtBfIVj74dhg9HjpxN7rhw

DIoU损失函数详解

https://zhuanlan.zhihu.com/p/109677830

Distance-IoU Loss

https://zhuanlan.zhihu.com/p/94799295

IoU、GIoU、DIoU、CIoU损失函数的那点事儿

# 3D目标检测

https://mp.weixin.qq.com/s/bAV74fxvwI73Qt7qm_jPJA

一文读懂深度学习在摄像头和激光雷达融合的3-D目标检测中的应用

https://mp.weixin.qq.com/s/8an3eBrOZ5d6_PdNp6QkyA

一文教你读懂3D目标检测

https://zhuanlan.zhihu.com/p/112836340

谷歌最新论文：从图像中进行3-D目标检测

https://mp.weixin.qq.com/s/DxDkYfzW5BqhidPlqT772Q

从单幅图像到双目立体视觉的3D目标检测算法

https://blog.csdn.net/savant_ning/article/details/69950588

多视图3D目标检测学习笔记

https://mp.weixin.qq.com/s/3JzwA2HAzoWtE_j2UhZCSw

Stereo R-CNN 3D目标检测

https://zhuanlan.zhihu.com/p/58734240

3D Object Detection Overview - 2019

https://mp.weixin.qq.com/s/n3ZUsq2I0CaJ5pIl2nZUFQ

3D目标检测：MonoDIS

https://mp.weixin.qq.com/s/WBiDyIBV9OaKbxUhBml2AA

GS3D(monocular 3D detection)

https://mp.weixin.qq.com/s/G7YNTR8FAsCtkCxLQv1-dQ

Facebook开源3D目标检测框架VoteNet，曾刷新两大数据集最高精度

https://mp.weixin.qq.com/s/s_cAZ--KHvNZq3thrIGx3w

MVX-Net：多模型三位像素网络用于3D目标检测

https://mp.weixin.qq.com/s/ouBxEXcY4s894Sec4ifBtQ

基于YOLO的3D目标检测：YOLO-6D

https://mp.weixin.qq.com/s/yCu5Xx6peDKyU06yBlixpA

首个实时单目3D目标检测算法：RTM3D

https://zhuanlan.zhihu.com/p/101346137

Det3D-首个通用3D目标检测框架

https://zhuanlan.zhihu.com/p/85686290

MLOD：基于鲁棒特征融合方法的多视点三维目标检测

https://mp.weixin.qq.com/s/LN2l67jhivb0hyy6JSLMVA

3D目标检测深度学习方法数据预处理综述

# 旋转框检测

在真实的场景中，许多时候，我们不仅仅需要找一个“方方正正”的框把物体框起来(英文中，这种框称之为Axis-Aligned)，而可能更需要的是能够找一个，“有一些旋转角度的，能够把物体完全的包络起来的框”。

![](/images/img3/rotated_OD.jpg)

上右图使用水平的检测框从图中将物体扣出，效果会很差，因为：

1、每个检测框中还会同时包含很多其他物体的“一部分”。

2、水平检测框相互之间的IoU值较高，在NMS过程中很容易被抑制掉。

参考：

https://zhuanlan.zhihu.com/p/105841613

旋转框检测方法综述-FasterRCNN的问题

https://zhuanlan.zhihu.com/p/105881332

旋转框检测方法综述-RotateAnchor系列

# 目标检测进阶

https://mp.weixin.qq.com/s/1nlOJ7X9ogBHTl1j2adqyg

83页《目标分类和目标检测综述（2D和3D数据）》论文

https://mp.weixin.qq.com/s/HmUhlw90b2aTsoEwBdYbdQ

目标检测二十年技术综述

https://mp.weixin.qq.com/s/S1IrgEqS1Q4xqGl5adNrlg

目标检测近年综述

https://mp.weixin.qq.com/s/7QT7n9MpbXjo5-r-aY2Yvg

深度学习目标检测方法综述

https://mp.weixin.qq.com/s/0B08Mzn8ngL6GoNilrjsGA

基于深度学习目标检测方法一览

https://mp.weixin.qq.com/s/5I9uzGCNFD93L1mzakTl0Q

目标检测网络学习总结（RCNN-->YOLO V3）

https://mp.weixin.qq.com/s/8Vac8MRpmviVDKRrAeFR0A

后R-CNN时代，Faster R-CNN、SSD、YOLO各类变体统治下的目标检测综述：Faster R-CNN系列胜了吗？

https://mp.weixin.qq.com/s/zeruKQOye_QNWgluVIN0BA

从R-CNN到RFBNet，目标检测架构5年演进全盘点

https://mp.weixin.qq.com/s/sCGNUI-mUSYxD69uBDQNoQ

基于深度学习的目标检测算法综述：算法改进

https://mp.weixin.qq.com/s/yswy7VwEapQJ9M5n_Uo93w

目标检测最新进展总结与展望

https://mp.weixin.qq.com/s/s1qmCA8djEEanwCxeLSV2Q

63页《深度CNN-目标检测》综述

https://mp.weixin.qq.com/s/j-arl6qiD6mei4crfQPrgw

《深度学习显著目标检测综述》

https://mp.weixin.qq.com/s/2PLp2xNfhkHB3fPQr5Ts6g

密歇根大学40页《20年目标检测综述》最新论文，带你全面了解目标检测方法

https://mp.weixin.qq.com/s/Pl8HABuVN27CZv-lvGROTw

基于深度学习的目标检测算法近5年发展历史

https://mp.weixin.qq.com/s/S6sz5dPgGNcJvrIAZ3ZjGg

基于深度学习的通用物体检测算法对比探索

https://zhuanlan.zhihu.com/p/59915784

目标检测中的Consistent Optimization

https://mp.weixin.qq.com/s/ts4WFnuN4cHLfUh8--96Kw

Libra R-CNN：全面平衡的目标检测器

https://mp.weixin.qq.com/s/groq55Cbts272k1mfhJwaQ

超越bounding box的代表性点集：视觉物体表示的新方法

https://mp.weixin.qq.com/s/BXwL33qOf3f7BtJvHsi23Q

目标检测：Segmentation is All You Need？

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

https://zhuanlan.zhihu.com/p/54334986

TridentNet：处理目标检测中尺度变化新思路

https://mp.weixin.qq.com/s/oF3MAkl1UikRkOhrj3equw

深度学习的目标检测算法是如何解决尺度问题的？

https://mp.weixin.qq.com/s/oxStDMh90jB7_EY4vqja2w

目标检测论文阅读：DetNet

https://zhuanlan.zhihu.com/p/55972055

SimpleDet:一套简单通用的目标检测与物体识别框架

https://zhuanlan.zhihu.com/p/55854246

Guided Anchoring: 物体检测器也能自己学Anchor

https://mp.weixin.qq.com/s/-G47vOGx2iNQCarYRAiNPg

基于区域分解集成的目标检测

https://mp.weixin.qq.com/s/rlmgN0LbUfd2n9MI8OMT2w

性能大幅度提升（速度&遮挡）:基于区域分解&集成的目标检测

https://zhuanlan.zhihu.com/p/59398728

CVPR2019目标检测方法进展综述

https://mp.weixin.qq.com/s/apLEAMshqd3O8nU8Q0Wycg

李祥泰：Context modeling in semantic segmentation

https://mp.weixin.qq.com/s/svqygu4nFkW4ci7dYMnKsw

小目标检测的数据增广秘籍

https://mp.weixin.qq.com/s/bzgMWR2kzAI9NeXEY92GmA

目标检测任务的优化策略tricks

https://mp.weixin.qq.com/s/0_ap6CsBlz4pvx21c57-ag

旷视研究院提出新型损失函数：改善边界框模糊问题

https://zhuanlan.zhihu.com/p/67714508

“取长补短”的RefineDet物体检测算法

https://mp.weixin.qq.com/s/pB3_ho7JLANKRtQK4gsR5Q

Kaggle实战目标检测奇淫技巧合集

https://mp.weixin.qq.com/s/PpT-NmTVjRi0_SEq0lISXw

旷视科技Oral论文解读：IoU-Net让目标检测用上定位置信度

https://mp.weixin.qq.com/s/OqlZ2TRGbHURYW00440lgQ

微软亚洲研究院与北京大学共同提出用于物体检测的可学习区域特征提取模块

https://www.zhihu.com/question/270143544

目标检测中，不同物体之间的距离非常接近如何解决？

https://mp.weixin.qq.com/s/b4s8Te29DyS71xwQU789pQ

实体零售场景下密集商品的精确探测

https://mp.weixin.qq.com/s/iW-k12CIO0gSx8Y6etTzgA

三分支网络——目前目标检测性能最佳网络框架

https://mp.weixin.qq.com/s/JN1N-IqIL4tAh4rIkZcxpg

Grid R-CNN解读：商汤最新目标检测算法

https://mp.weixin.qq.com/s/_2DwSY6olj3wKy2xKukEGg

商汤开源Grid R-CNN Plus：相比Grid RCNN，速度更快，精度更高

https://mp.weixin.qq.com/s/baPfFVi7deEsCAFu3ColoQ

CVPR2018目标检测算法总览

https://mp.weixin.qq.com/s/t5p1xGKVnwd7wbiOzucFqQ

基于深度学习的目标检测算法剖析与实现

https://mp.weixin.qq.com/s/-zQZjHVs7bYyGkGuMUf3qg

目标检测领域还有什么可做的？19个方向给你建议

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

https://blog.csdn.net/wq604887956/article/details/83053927

2018小目标检测文章总结

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

https://mp.weixin.qq.com/s/UQLvHDf62iV8KeZ5LoQdsA

在小目标检测上另辟蹊径的SNIP

https://mp.weixin.qq.com/s/GpZHGksl0elxMcaQYosK-A

SNIP的升级版SNIPER（效果比Mosaic更佳）

https://mp.weixin.qq.com/s/XdH54ImSfgadCoISmVyyVg

基于单目摄像头的物体检测

https://mp.weixin.qq.com/s/h_ENriEXr7WI_XR_DtxpMQ

这样可以更精确的目标检测——超网络
