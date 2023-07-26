---
layout: post
title:  深度目标检测（八）——其它目标检测网络, 花式IOU, 3D目标检测, 旋转框检测, 小目标检测, 花式NMS, YOLOv4
category: Deep Object Detection 
---

* toc
{:toc}

# Anchor-Free（续）

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

![](/images/img5/IOU.jpg)

![](/images/img5/IOU_2.jpg)

常规IOU存在两个问题：

问题1：即状态1的情况，当预测框和目标框不相交时，IOU=0，无法反应两个框距离的远近，此时损失函数不可导，IOU无法优化两个框不相交的情况。

问题2：即状态2和状态3的情况，当两个预测框大小相同，两个IOU也相同，IOU无法区分两者相交情况的不同。

## GIOU

![](/images/img5/IOU.jpg)

![](/images/img5/IOU_2.jpg)

问题：状态1、2、3都是预测框在目标框内部且预测框大小一致的情况，这时预测框和目标框的差集都是相同的，因此这三种状态的GIOU值也都是相同的，这时GIOU退化成了IOU，无法区分相对位置关系。

## DIOU

好的目标框回归函数应该考虑三个重要几何因素：重叠面积、中心点距离，长宽比。

![](/images/img5/IOU_3.jpg)

DIOU_Loss考虑了重叠面积和中心点距离，当目标框包裹预测框的时候，直接度量2个框的距离。

![](/images/img5/IOU_4.jpg)

但它没有考虑到长宽比。

## CIOU

CIOU loss= IOUloss+中心点损失+长宽比例损失

$$CIOU\_ Loss=1-CIOU=1-(IOU-\frac{Distance\_ 2^2}{Distance\_ C^2}-\frac{v^2}{(1-IOU)+v})$$

$$v=\frac{4}{\pi^2}(\arctan\frac{w^{gt}}{h^{gt}}-\arctan\frac{w^{p}}{h^{p}})^2$$

由于NMS也和IOU有关，所以对应的也有DIOU_nms等。

## EIoU & SIoU

EIOU loss =IOUloss+中心点损失+宽损失+长损失

SIoU =Distance cost（angle + distance）+Shape cost+IOU loss

https://zhuanlan.zhihu.com/p/537428926

一文搞懂EIoU与SIoU

## 参考

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

## WBF

加权框融合（WBF）是一种提高目标检测系统性能的强大技术。它是一种将多个边界框或感兴趣区域 (ROI) 的结果组合成一个更准确、更稳定的结果的方法。当使用多个模型或算法来检测图像或视频中的目标并且需要组合结果以提高整体性能时，该技术特别有用。

https://mp.weixin.qq.com/s/fsByXBrTWaWBDR-FjhHwnA

目标检测的后处理：NMS vs WBF

# YOLOv4

YOLO系列(v1-v3)作者Joe Redmon宣布不再继续CV方向的研究，引起学术圈一篇哗然。

YOLOv4（2020.4）的一作是Alexey Bochkovskiy。YOLO官方的github正式加入YOLOv4的论文和代码链接，也意味着YOLOv4得到了Joe Redmon的认可，也代表着YOLO的停更与交棒。

论文：

《YOLOv4: Optimal Speed and Accuracy of Object Detection》

代码：

https://github.com/AlexeyAB/darknet

![](/images/img5/YOLOv4.jpg)

Yolov4的五个基本组件：

- CBM：Yolov4网络结构中的最小组件，由Conv+Bn+Mish激活函数三者组成。
- CBL：由Conv+Bn+Leaky_relu激活函数三者组成。
- Res unit：借鉴Resnet网络中的残差结构，让网络可以构建的更深。
- CSPX：借鉴CSPNet网络结构，由卷积层和X个Res unint模块Concate组成。
- SPP：采用1×1，5×5，9×9，13×13的最大池化的方式，进行多尺度融合。
