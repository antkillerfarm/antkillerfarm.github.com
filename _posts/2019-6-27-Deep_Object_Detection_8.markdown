---
layout: post
title:  深度目标检测（八）——其它目标检测网络, 花式IOU, 3D目标检测, 旋转框检测, 小目标检测, 花式NMS
category: Deep Object Detection 
---

* toc
{:toc}

# Anchor-Free（续）

## RepPoints

https://mp.weixin.qq.com/s/aWv7_yiX5BFKpL21SRV1MQ

RepPoints：可形变卷积的进阶

https://mp.weixin.qq.com/s/nI_3kilFCsDHhtjFhRKytA

RepPoints:替代边界框，基于点集的物体表示新方法

https://mp.weixin.qq.com/s/VTb6CUOWnPpyU6WnYdYJ-g

RepPoints V2：将角点检测和前景热图引入纯回归目标检测算法

https://mp.weixin.qq.com/s/gnTZ-q2-lm8QPH6JEPylnw

RepPointv2：使用点集合表示来做目标检测

## 参考

https://mp.weixin.qq.com/s/T7DDWvtvCULfjcDmljvx5Q

Anchor-free的对象检测网络汇总

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

https://zhuanlan.zhihu.com/p/66156431

从Densebox到Dubox：更快、性能更优、更易部署的anchor-free目标检测

https://zhuanlan.zhihu.com/p/63273342

聊聊Anchor的"前世今生"（上）

https://zhuanlan.zhihu.com/p/68291859

聊聊Anchor的"前世今生"（下）

https://zhuanlan.zhihu.com/p/62372897

物体检测的轮回：anchor-based与anchor-free

https://mp.weixin.qq.com/s/m_PvEbq2QbTXNmj_gObKmQ

Anchor-free目标检测之ExtremeNet

https://mp.weixin.qq.com/s/LGeNgnXfYaVVAc_37j6-2A

Anchor Free及Transformer时代

https://mp.weixin.qq.com/s/52YBmlHioRkUgetZWHMZOw

Anchor-free目标检测：工业应用更友好的新网络

https://zhuanlan.zhihu.com/p/84398108

目标检测中Anchor的本质分析

https://mp.weixin.qq.com/s/LQOzrlaEOsrsMHj-V8l3hQ

FreeAnchor：抛弃单一的IoU匹配，更自由的anchor匹配方法

https://zhuanlan.zhihu.com/p/163266388

Anchor-free应用一览：目标检测、实例分割、多目标跟踪

https://mp.weixin.qq.com/s/4UEmRcSo0ZGoiLh6iKf_oQ

ATSS：自动选择样本，消除Anchor based和Anchor free物体检测方法之间的差别

https://mp.weixin.qq.com/s/UhHh_DFoxKW5K3OCe3Bjqg

目标检测：Anchor-Free时代

https://mp.weixin.qq.com/s/PqDkdxqvUvSKvTklVojOyA

CPNDet：简单地给CenterNet加入two-stage，更快更强

https://mp.weixin.qq.com/s/7mHhltqDcnYZdHWoRS_EBg

YOLO之外的另一选择，手机端97FPS的Anchor-Free目标检测模型NanoDet现已开源

https://zhuanlan.zhihu.com/p/336016003

OneNet: End-to-End One-Stage Object Detection

https://mp.weixin.qq.com/s/0FPpc2PhLPiE9mg6eRh11Q

OneNet：一阶段的端到端物体检测器，无需NMS

https://mp.weixin.qq.com/s/ov4xLhicTqsce0bG2pw95A

anchor-base和anchor-free差异分析

https://mp.weixin.qq.com/s/yft97xTTX0FUXpyHtI_XMQ

anchor-free存在什么缺点？

# 其它目标检测网络

## A-Fast-RCNN

A-Fast-RCNN首次将对抗学习引入到了目标检测领域，idea在当时是非常创新的。

http://blog.csdn.net/jesse_mx/article/details/72955981

A-Fast-RCNN论文笔记

## G-CNN

G-CNN是MaryLand大学的工作，论文主要的思路也是消除region proposal，和YOLO，SSD不同，G-CNN的工作借鉴了迭代的想法，把边框检测等价于找到初始边框到最终目标的一个路径。但是使用one-step regression不能处理这个非线性的过程，所以作者采用迭代的方法逐步接近最终的目标。

http://blog.csdn.net/zijin0802034/article/details/53535647

G-CNN: an Iterative Grid Based Object Detector

## D2D

![](/images/img4/D2D.png)

https://mp.weixin.qq.com/s/U15qB2PlQN3qpFBuQiY6RA

Describe-to-Detect(D2D)：一种新的特征检测方法

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

https://zhuanlan.zhihu.com/p/342991797

从L1 loss到EIoU loss，目标检测边框回归的损失函数一览

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

https://mp.weixin.qq.com/s/nGMMb6GHp-BajyfRTN-jDw

不用激光雷达，照样又快又准！3D目标检测之SMOKE

# 旋转框检测

在真实的场景中，许多时候，我们不仅仅需要找一个“方方正正”的框把物体框起来(英文中，这种框称之为Axis-Aligned)，而可能更需要的是能够找一个，“有一些旋转角度的，能够把物体完全的包络起来的框”。

![](/images/img3/rotated_OD.jpg)

上右图使用水平的检测框从图中将物体扣出，效果会很差，因为：

1、每个检测框中还会同时包含很多其他物体的“一部分”。

2、水平检测框相互之间的IoU值较高，在NMS过程中很容易被抑制掉。

![](/images/img4/AlphaRotate.png)

《AlphaRotate: A Rotation Detection Benchmark using TensorFlow》

参考：

https://zhuanlan.zhihu.com/p/105841613

旋转框检测方法综述-FasterRCNN的问题

https://zhuanlan.zhihu.com/p/105881332

旋转框检测方法综述-RotateAnchor系列

https://mp.weixin.qq.com/s/nOBfPFfJMBkkkVEPV0TG0Q

PIoU Loss：倾斜目标检测专用损失函数，公开超难倾斜目标数据集Retail50K

https://mp.weixin.qq.com/s/U8RXvWP7K1xM_tzhdttPfA

BBAVectors：一种Anchor Free的旋转物体检测方法

https://zhuanlan.zhihu.com/p/98703562

旋转目标(遥感/文字)检测方法整理（2017-19年）

https://zhuanlan.zhihu.com/p/163696749

用CenterNet对旋转目标进行检测

https://mp.weixin.qq.com/s/pDjszZk43vuVO6kY3bRczw

ODTK：来自NVIDIA的旋转框物体检测工具箱

https://zhuanlan.zhihu.com/p/337272217

Dynamic Anchor Learning for Object Detection

https://mp.weixin.qq.com/s/Zf6_L9MfKd0AhhgsGSi6EA

旋转目标检测中anchor匹配机制的问题和一些思考

# 小目标检测

https://mp.weixin.qq.com/s/8k0Mhver2mLnKmV8rVqJHQ

小目标检测相关技巧总结

https://mp.weixin.qq.com/s/svqygu4nFkW4ci7dYMnKsw

小目标检测的数据增广秘籍

https://blog.csdn.net/wq604887956/article/details/83053927

2018小目标检测文章总结

https://mp.weixin.qq.com/s/UQLvHDf62iV8KeZ5LoQdsA

在小目标检测上另辟蹊径的SNIP

https://mp.weixin.qq.com/s/iaeHnfepyeLuOioHqMO9bQ

一种小目标检测中有效的数据增强方法

https://mp.weixin.qq.com/s/UbCLqfyEOKUGnvySweUzMw

使用关键点进行小目标检测

https://mp.weixin.qq.com/s/1v84QyvH-k0lzRPOEyaBgw

我们是如何改进YOLOv3进行红外小目标检测的？

https://mp.weixin.qq.com/s/5TRm1vYx8dYVQHU7FdHdJg

在目标检测中如何解决小目标的问题？

https://mp.weixin.qq.com/s/V9CJVaRYlS5Snyveblb3uw

小目标检测的一些问题，思路和方案

https://mp.weixin.qq.com/s/zvKxi0b-D0fXzx928Wp51A

小目标检测研究进展

# 花式NMS

https://mp.weixin.qq.com/s/ro0lG3uMUPYNZA9rM3I_YQ

目标检测算法中检测框合并策略技术综述

https://mp.weixin.qq.com/s/GdNcQqDeVQ1vtIJrAIYpWw

目标检测之非极大值抑制(NMS)各种变体

https://zhuanlan.zhihu.com/p/151914931

一文打尽目标检测NMS——精度提升篇

https://zhuanlan.zhihu.com/p/157900024

一文打尽目标检测NMS——效率提升篇

https://zhuanlan.zhihu.com/p/151398233

一文了解目标检测边界框概率分布

https://mp.weixin.qq.com/s/OnJQm8xCmxa4szB-lJC9Uw

或许你的NMS该换了，Confluence更准、更稳的目标检测结果

https://mp.weixin.qq.com/s/W6m2eaysYiK6-3Niz4KeOA

Confluence：物体检测中不依赖IoU的NMS替代算法论文解析

# 目标检测进阶

https://mp.weixin.qq.com/s/1nlOJ7X9ogBHTl1j2adqyg

83页《目标分类和目标检测综述（2D和3D数据）》论文

https://mp.weixin.qq.com/s/HmUhlw90b2aTsoEwBdYbdQ

目标检测二十年技术综述

https://mp.weixin.qq.com/s/cWCwcTA01oBy0BM3qRHb4Q

综述：目标检测二十年（2001-2021）

https://mp.weixin.qq.com/s/S1IrgEqS1Q4xqGl5adNrlg

目标检测近年综述

https://mp.weixin.qq.com/s/7QT7n9MpbXjo5-r-aY2Yvg

深度学习目标检测方法综述

https://mp.weixin.qq.com/s/0B08Mzn8ngL6GoNilrjsGA

基于深度学习目标检测方法一览

https://zhuanlan.zhihu.com/p/181169225

12篇论文看尽深度学习目标检测史
