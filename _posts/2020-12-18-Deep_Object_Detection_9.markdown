---
layout: post
title:  深度目标检测（九）——花式NMS, YOLOv4, YOLOv5, YOLOX, YOLOv6, YOLOv7, YOLOv8
category: Deep Object Detection 
---

* toc
{:toc}

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

YOLOv5由Darknet的另一贡献者Ultralytics创建并维护（2020.5）。这是一家总部位于美国的粒子物理和人工智能初创公司。

代码：

https://github.com/ultralytics/yolov5

![](/images/img5/YOLOv5.jpg)

- 输入端：Mosaic数据增强、自适应锚框计算、自适应图片缩放
- Backbone：Focus结构，CSP结构
- Neck：FPN+PAN结构
- Prediction：GIOU_Loss

参考：

https://mp.weixin.qq.com/s/0tNVoD4H6fRGenUJXuJ8yg

YOLOv5来了！基于PyTorch，体积比YOLOv4小巧90%，速度却超2倍

https://zhuanlan.zhihu.com/p/161083602

一文读懂YOLO V5与YOLO V4

https://zhuanlan.zhihu.com/p/183261974

YOLO系列(从v1到v5)模型解读(上)

https://zhuanlan.zhihu.com/p/183781646

YOLO系列(从v1到v5)模型解读(中)

https://zhuanlan.zhihu.com/p/186014243

YOLO系列(从v1到v5)模型解读(下)

https://zhuanlan.zhihu.com/p/143747206

深入浅出Yolo系列之Yolov3&Yolov4&Yolov5核心基础知识完整讲解

https://zhuanlan.zhihu.com/p/136382095

YOLO系列：YOLOv1,YOLOv2,YOLOv3,YOLOv4,YOLOv5简介

https://mp.weixin.qq.com/s/rzR-rAiJejR8LjDDgiauZA

C++实现yolov5的OpenVINO部署

https://mp.weixin.qq.com/s/OP5iLZtIABNcn_LFyBWOeA

YOLObile:面向移动设备的“实时目标检测”算法

https://mp.weixin.qq.com/s/JQFWf-lFT4bwWEfQBoIdwQ

目标检测算法YOLOF：You Only Look One-level Feature

https://mp.weixin.qq.com/s/QRPQaxUvQwNTXhuOQezdqg

Yolo发展史(v4/v5的创新点汇总！)

# YOLOX

YOLOX是旷视科技2021年的作品。

论文：

《YOLOX: Exceeding YOLO Series in 2021》

![](/images/img5/YOLOX.jpg)

![](/images/img5/Decoupled_Head.webp)

YOLOX改进点：

- Decoupled Head结构。简单说，分类和回归两个任务的head不再共享参数。
- Mosaic + MixUp的数据增强方法。不过需要注意：在训练的最后15个epoch，这两个数据增强会被关闭掉。
- Anchor Free。基于中心点，预测网格左上角的两个偏移量，以及预测框的高度和宽度。
- SimOTA样本匹配。

传统的YOLO系列都使用同一Head进行分类和回归。YOLOX则将分类和回归分支解耦。但由于出头的位置相当靠后，也没有RPN，所以还是One-stage模型。

参考：

https://www.zhihu.com/question/473350307

如何评价旷视开源的YOLOX，效果超过YOLOv5?

# YOLOv6

YOLOv6是美团2022年的作品，并得到了YOLO官方的认可。

这里首先推荐一下：

https://mmyolo.readthedocs.io/

这个网站包含了YOLOv5以后各YOLO系列的资料，包括网络结构图。

![](/images/img5/YOLOv6.png)

YOLOv6的改进：

- RepVGG style的Backbone。
- 更简洁高效的Decoupled Head。

## RepVGG

YOLOv6包括后面的YOLOv7、YOLOv8都采用了RepVGG style作为Backbone。因此这里我们先来讲一下什么是RepVGG。

论文：

《RepVGG: Making VGG-style ConvNets Great Again》

![](/images/img5/RepVGG.png)

相比于各种多分支架构（如ResNet，Inception，DenseNet，各种NAS架构），近年来VGG式模型鲜有关注，主要是因为效果差。但是单路架构毕竟也有计算速度快，省内存的优点。

因此，一个很自然的想法就是：如何同时利用多分支模型训练时的优势（性能高）和单路模型推理时的好处（速度快、省内存）。

RepVGG采用了一种很巧妙的构造方法做到了这一点。

![](/images/img5/RepVGG_2.png)

如上图所示3x3、1x1和bypass的分支，最终都被合并为一个计算出来的3x3 kernel。

当然，YOLOv6并没有直接使用RepVGG，而是使用了RepVGG style或者是RepVGG的思路。这种思路有时也叫做RepConv。

参考：

https://zhuanlan.zhihu.com/p/344324470

RepVGG：极简架构，SOTA性能，让VGG式模型再次伟大

## 参考

https://zhuanlan.zhihu.com/p/533127196

YOLOv6：又快又准的目标检测框架开源啦

https://zhuanlan.zhihu.com/p/566469003

YOLO内卷时期该如何选模型？

# YOLOv7

YOLOv7是Alexey Bochkovskiy团队（YOLOv4团队）2022年的作品。

![](/images/img5/YOLOv7.jpg)

论文：

《YOLOv7: Trainable bag-of-freebies sets new state-of-the-art for real-time object detectors》

YOLOv7的改进：

- 扩展了高效长程注意力网络，称为Extended-ELAN（E-ELAN）。
- auxiliary head。

参考：

https://www.zhihu.com/question/541985721

如何评价Alexey Bochkovskiy团队提出的YoloV7？

https://zhuanlan.zhihu.com/p/543743278

深入浅出Yolo系列之Yolov7基础网络结构详解

# YOLOv8

YOLOv8是Ultralytics（YOLOv5团队）2023年的作品。

![](/images/img5/YOLOv8.jpg)

YOLOv8的改进：

- 分类损失为VFL Loss，其回归损失为CIOU Loss+DFL（Distribution Focal Loss）。
- TOOD的TaskAlignedAssigner。

https://zhuanlan.zhihu.com/p/598566644

YOLOv8深度详解

https://blog.csdn.net/qq_40716944/article/details/128609569

详细解读YOLOv8的改进模块

# YOLOv9

YOLOv9是Ultralytics（YOLOv7团队）2024年的作品。

YOLOv9的改进：

- 可编程梯度信息(PGI)
- 广义高效层聚合网络(GELAN)

# YOLOv10

YOLOv10是Ultralytics（YOLOv8团队）2024年的作品。

YOLOv10的改进：

- PAN（路径聚合网络）层
- 无需NMS

https://zhuanlan.zhihu.com/p/1897599850524178071

YOLO版本迷信终结！11领域398万实例实测：告诉你的场景该用哪个版本？

# 目标检测进阶

https://mp.weixin.qq.com/s/1nlOJ7X9ogBHTl1j2adqyg

83页《目标分类和目标检测综述（2D和3D数据）》论文

https://mp.weixin.qq.com/s/HmUhlw90b2aTsoEwBdYbdQ

目标检测二十年技术综述

https://mp.weixin.qq.com/s/cWCwcTA01oBy0BM3qRHb4Q

综述：目标检测二十年（2001-2021）

https://mp.weixin.qq.com/s/S1IrgEqS1Q4xqGl5adNrlg

目标检测近年综述
