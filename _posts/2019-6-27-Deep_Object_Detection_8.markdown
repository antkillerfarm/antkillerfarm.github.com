---
layout: post
title:  深度目标检测（八）——其它目标检测网络, 花式IOU, 3D目标检测, 旋转框检测, 小目标检测, 花式NMS, YOLOv4
category: Deep Object Detection 
---

* toc
{:toc}

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

各部分的改进如下：

- 输入端：这里指的创新主要是训练时对输入端的改进，主要包括Mosaic数据增强、cmBN、SAT自对抗训练。
- BackBone主干网络：将各种新的方式结合起来，包括：CSPDarknet53、Mish激活函数、Dropblock。
- Neck：目标检测网络在BackBone和最后的输出层之间往往会插入一些层，比如Yolov4中的SPP模块、FPN+PAN结构。
- Prediction：输出层的锚框机制和Yolov3相同，主要改进的是训练时的损失函数CIOU_Loss，以及预测框筛选的nms变为DIOU_nms。

参考：

https://zhuanlan.zhihu.com/p/135909702

大神接棒，YOLOv4来了！

https://mp.weixin.qq.com/s/Ia1ZhAeTgt8anXVd4qxE3A

一张图梳理YOLOv4论文

https://mp.weixin.qq.com/s/ugx6CwMTqGR8CT5xpye6vw

对象检测YOLOv4版本来了！

https://mp.weixin.qq.com/s/XEPhK81Ms-wdDnoz5oPZgA

YOLO v4它来了：接棒者出现，速度效果双提升

https://mp.weixin.qq.com/s/Ny_4lK1E3bqz-LL-hHiFlg

YOLO项目复活！大神接过衣钵，YOLO之父隐退2月后，v4版正式发布，性能大幅提升

https://mp.weixin.qq.com/s/9SR5CUDIBmdJeYEWABASWA

YOLOv4的各种新实现、配置、测试、训练资源汇总

https://mp.weixin.qq.com/s/iGhYxBLdGHPydVi2FgkNtg

YOLO系列：V1,V2,V3,V4简介

https://mp.weixin.qq.com/s/E5TS0NuSWCWmxrJnN8AUKA

想读懂YOLOV4，你需要先了解下列技术(一)

https://mp.weixin.qq.com/s/5usz-wraHArK6_HcE4RuZw

想读懂YOLOV4，你需要先了解下列技术(二)

https://mp.weixin.qq.com/s/v2x3u3_FELz2lHqBJKR-dg

Yolov3和Yolov4核心内容、代码梳理

https://zhuanlan.zhihu.com/p/143747206

深入浅出Yolo系列之Yolov3&Yolov4&Yolov5&Yolox核心基础知识完整讲解

https://zhuanlan.zhihu.com/p/150127712

YOLO V4—网络结构解析

https://zhuanlan.zhihu.com/p/159209199

YOLO V4—损失函数解析

https://mp.weixin.qq.com/s/KRJ5e50NuACk2ZXi1Rxkxw

YOLOv4中的数据增强

# YOLOv5

YOLOv5由Darknet的另一贡献者Ultralytics创建并维护（2010.5）。这是一家总部位于美国的粒子物理和人工智能初创公司。

代码：

https://github.com/ultralytics/yolov5
