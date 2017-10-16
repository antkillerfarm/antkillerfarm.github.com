---
layout: post
title:  深度学习（十六）——OpenPose, 目标跟踪
category: theory 
---

# 语义分割的展望

俗话说，“没有免费的午餐”（“No free lunch”）。基于深度学习的图像语义分割技术虽然可以取得相比传统方法突飞猛进的分割效果，但是其对数据标注的要求过高：不仅需要海量图像数据，同时这些图像还需提供精确到像素级别的标记信息（Semantic labels）。因此，越来越多的研究者开始将注意力转移到弱监督（Weakly-supervised）条件下的图像语义分割问题上。在这类问题中，图像仅需提供图像级别标注（如，有“人”，有“车”，无“电视”）而不需要昂贵的像素级别信息即可取得与现有方法可比的语义分割精度。

另外，示例级别（Instance level）的图像语义分割问题也同样热门。该类问题不仅需要对不同语义物体进行图像分割，同时还要求对同一语义的不同个体进行分割（例如需要对图中出现的九把椅子的像素用不同颜色分别标示出来）。

![](/images/article/Instance_level.jpg)

最后，基于视频的前景／物体分割（Video segmentation）也是今后计算机视觉语义分割领域的新热点之一，这一设定其实更加贴合自动驾驶系统的真实应用环境。

# 视频目标分割

视频目标分割任务和语义分割有两个基本区别：

1.视频目标分割任务分割的是一般的、非语义的目标；

2.视频目标分割添加了一个时序模块：它的任务是在视频的每一连续帧中寻找感兴趣目标的对应像素。

![](/images/article/Segmentation.png)

上图是Segmentation的细分，其中的每一个叶子都有一个示例数据集。

基于视频任务的特性，我们可以将问题分成两个子类：

无监督（亦称作视频显著性检测）：寻找并分割视频中的主要目标。这意味着算法需要自行决定哪个物体才是「主要的」。

半监督：在输入中（只）给出视频第一帧的正确分割掩膜，然后在之后的每一连续帧中分割标注的目标。



参考：

http://mp.weixin.qq.com/s/pGrzmq5aGoLb2uiJRYAXVw

一文概览视频目标分割

https://www.zhihu.com/question/52185576

视频中的目标检测与图像中的目标检测具体有什么区别？

# OpenPose

![](/images/article/openpose.png)

论文：

《Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields》

《Hand Keypoint Detection in Single Images using Multiview Bootstrapping》

《Convolutional pose machines》

代码：

https://github.com/CMU-Perceptual-Computing-Lab/openpose

# Mask R-CNN

Mask R-CNN虽然挂着R-CNN的名头，但却是一个对象实例分割（不仅要分出对象的类别，连同一类对象的不同实例也要分出来）的NN。它是何恺明2017年的新作。

论文：

《Mask R-CNN》

只有非官方的代码：

Caffe版本：

https://github.com/jasjeetIM/Mask-RCNN

TensorFlow版本：

https://github.com/hillox/TFMaskRCNN

参考：

https://zhuanlan.zhihu.com/p/25954683

Mask R-CNN个人理解

# 目标跟踪

https://www.zhihu.com/question/26493945

计算机视觉中，目前有哪些经典的目标跟踪算法？

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


