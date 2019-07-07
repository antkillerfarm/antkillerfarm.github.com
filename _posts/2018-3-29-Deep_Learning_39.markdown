---
layout: post
title:  深度学习（三十九）——视频处理, Mask R-CNN
category: DL 
---

# 视频处理

## 视频目标分割

视频目标分割任务和语义分割有两个基本区别：

1.视频目标分割任务分割的是一般的、非语义的目标；

2.视频目标分割添加了一个时序模块：它的任务是在视频的每一连续帧中寻找感兴趣目标的对应像素。

![](/images/article/Segmentation.png)

上图是Segmentation的细分，其中的每一个叶子都有一个示例数据集。

基于视频任务的特性，我们可以将问题分成两个子类：

无监督（亦称作视频显著性检测）：寻找并分割视频中的主要目标。这意味着算法需要自行决定哪个物体才是“主要的”。

半监督：在输入中（只）给出视频第一帧的正确分割掩膜，然后在之后的每一连续帧中分割标注的目标。

## 参考

http://mp.weixin.qq.com/s/pGrzmq5aGoLb2uiJRYAXVw

一文概览视频目标分割

https://www.zhihu.com/question/52185576

视频中的目标检测与图像中的目标检测具体有什么区别？

https://mp.weixin.qq.com/s/noljXreGfoMfiZb_n90R3w

模仿人类的印象机制，商汤提出精确实时的视频目标检测方法

http://mp.weixin.qq.com/s/-Av3-ZNi6UGlKNv_jduAeQ

微软新论文：如何利用深度特征流提高视频识别准确率？

https://mp.weixin.qq.com/s/z1APyCxlOEPHn48OeJAHkQ

基于深度学习的视频内容识别

https://mp.weixin.qq.com/s/WMakTEN68KPi7X9kMQetiw

OpenAI:3段视频演示无人驾驶目标检测强大的对抗性样本！

https://mp.weixin.qq.com/s/j5YPHYEPioLiEIDc6lK3kA

在线视频衣物精确检索技术，开启刷剧败明星同款时代

https://mp.weixin.qq.com/s/CXKuSMi0Vd43BGDf5BgoqA

弱监督视频物体识别新方法：香港科技大学联合CMU提出TD-Graph LSTM

https://mp.weixin.qq.com/s/nZVIVJ9z0AWA8VFouNpafg

深度学习之视频摘要简述

https://mp.weixin.qq.com/s/7ccEaDRngVo42OSU6FBlVg

从视频到语句，优必选获TRECVID 2017子任务冠军

https://mp.weixin.qq.com/s/XorPkuIdhRNI1zGLwg-55A

斯坦福新深度学习系统 NoScope：视频对象检测快1000倍

https://mp.weixin.qq.com/s/7w5nYWfZO-YOJ4cA47kQXg

无监督视频物体分割新思路：实例嵌入迁移

https://mp.weixin.qq.com/s/PhMPa-e4sbzqWKmFzRZE4Q

实时替换视频背景：谷歌展示全新移动端分割技术

https://mp.weixin.qq.com/s/ovjoHCcR1xYb9N6kyFJUTg

视频广告段落检测——从一个偏门说计算机视觉的发展历史

https://mp.weixin.qq.com/s/0JgwBizaCwvPP9TfLKTang

密歇根大学&谷歌提出TAL-Net：将Faster R-CNN泛化至视频动作定位中

http://mp.weixin.qq.com/s/LAgDobWyK0SOH08GCLXG7A

减少30%流量，增加清晰度：MIT提出人工智能视频缓存新算法

https://mp.weixin.qq.com/s/_ZmbwM-lmS0o2DjAAc_TWQ

美图云+中科院AAAI2018：视频语义理解的类脑智能NOASSOM

https://mp.weixin.qq.com/s/iqLHjbmLOmvfEeEUB_SqSA

计算机视觉视频理解领域的经典方法和最新成果

https://mp.weixin.qq.com/s/LzKsD_vFlA1n-TYOGJkDZg

商汤科技开源DAVIS2017视频目标分割冠军代码

https://mp.weixin.qq.com/s/FiAju9F_MWexstP7FrIquw

凭一张照片找到视频中你所有的镜头，包括背影

https://mp.weixin.qq.com/s/3H0ZJjnPsh1BzALmG0W7og

DAVIS2017视频目标分割冠军代码开源了

https://mp.weixin.qq.com/s/ZqnfSL6U5E9NzE15QMdxtg

腾讯AI Lab提出视频再定位任务，准确定位相关视频内容

https://mp.weixin.qq.com/s/6MXLtUDi_idMYqbHARkbcg

港中文林达华团队提出计算机视觉新方向：电影情节分析

https://mp.weixin.qq.com/s/Np4xyvPrncd7MJ9q1WShBA

Python视频深度学习：计算任意影片中所有演员出镜时间

https://mp.weixin.qq.com/s/Nt4QLX_lbHhszb8fFlmOLA

DeepVS：基于深度学习的视频显著性方法

https://mp.weixin.qq.com/s/NgsSQS6opjOsusTIr9Vx-w

腾讯AI Lab、MIT等机构提出TVNet：可端到端学习视频的运动表征

https://mp.weixin.qq.com/s/26OZ5sLK3floF8I1SNIKuA

时空建模新文解读：用于高效视频理解的TSM 

https://mp.weixin.qq.com/s/TzNqZNEPBewR7neU7Or9nQ

更侧重工业的应用：PRCV2018美图短视频实时分类挑战赛冠军技术方案

https://mp.weixin.qq.com/s/lBu1q5Pyw9dZIxSYXUp2pw

视频语义分割介绍

https://mp.weixin.qq.com/s/qtRV9Sb54o8TnDEhLlB69Q

基于视频的目标检测的发展

https://mp.weixin.qq.com/s/MzVPesFK0vJ1UuQPPSSN2w

百度、MIT等提出StNet：局部+全局的视频时空联合建模

https://mp.weixin.qq.com/s/UeQc3orm2ooZ5zlvrSLzOw

视频内容理解在Hulu的应用与实践

https://mp.weixin.qq.com/s/syZObdxjPv6jq3B_mgP9Sw

拒绝“不可描述”！爱奇艺短视频软色情识别技术解析

https://mp.weixin.qq.com/s/8YpyfdhDypSZOP3dQegQdQ

谷歌大脑提出基于流的视频预测模型，可产生高质量随机预测结果

https://mp.weixin.qq.com/s/-FF3tuEB2V8RlQCjQhu5Bg

人大ML研究组提出新的视频测谎算法

https://mp.weixin.qq.com/s/T-Rg9xLfdYmV8bJESK0h8g

快速端到端嵌入学习用于视频中的目标分割

https://mp.weixin.qq.com/s/pKSrokV_j8Repa-JMloUHg

Google又发大招：高效实时实现视频目标检测

https://mp.weixin.qq.com/s/ySAfdII8291hvTxUBtE5qA

详解爱奇艺ZoomAI视频增强技术的应用

https://mp.weixin.qq.com/s/l6WMJnrGNNK4A1cTD2drcg

视频跟踪新思路，完全无需手工标注。这是一篇Visual Tracking和Optical Flow Estimation相互结合的论文

https://zhuanlan.zhihu.com/p/59915784

Video Action Recognition的近期进展

https://mp.weixin.qq.com/s/NQSJvlcjOAoIZjI2cgjhkw

谷歌AI：根据视频生成深度图，效果堪比激光雷达

https://mp.weixin.qq.com/s/k4Ilj11wbuj5oIXLHo-5ew

《视频目标分割与跟踪》最新39页综述论文

https://mp.weixin.qq.com/s/fxKHMVRYCR9CycifjEnArQ

视频显著性目标检测

https://mp.weixin.qq.com/s/oSoCIAEF78iKIxLxj2H1mA

基于光流的视频目标检测系列文章解读

https://mp.weixin.qq.com/s/1tcoGGbJnnWARu-2wefWdQ

不同视角构造cycle-consistency，降低视频标注成本

https://mp.weixin.qq.com/s/pDMBnX3CeQbv8hr-79Mvvg

商汤EDVR算法获NTIRE 2019视频恢复比赛全部四项冠军

https://mp.weixin.qq.com/s/GCCqIm4Q7UfUhhiqFbBS3g

Pytorch视频分类教程

https://mp.weixin.qq.com/s/ua8V2g2uZAditKui-IcoKw

物体检测算法在视频中的应用

https://mp.weixin.qq.com/s/BtIYc7SSi0E6mT3muV6NhQ

视频编辑利器，不喜欢就框除！开源视频物体移除软件video object removal

# Mask R-CNN

Mask R-CNN虽然挂着R-CNN的名头，但却是一个对象实例分割（不仅要分出对象的类别，连同一类对象的不同实例也要分出来）的网络。它是何恺明2017年的新作。

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

上图是Mask R-CNN的网络结构图。它实际上就是在Faster R-CNN的基础上添加了一个FCN。

![](/images/img3/Mask_R-CNN.png)

上图也是Mask R-CNN的网络结构图，但它对Faster R-CNN中与本主题无关的部分做了省略。

Mask R-CNN的要点主要有：

- **RoI Align**

RoI Align避免对RoI的边界或者块（bins）做任何量化，例如直接使用x/16代替[x/16]。

然而这就引来一个问题：如果x/16不是整数该怎么采样呢？

答案：对临近的整数采样点，使用双线性插值（bilinear interpolation）拟合，得到非整数采样点的值。

- **独立的类别预测**

把loss由`tf.nn.softmax_cross_entropy_with_logits`换成`tf.nn.sigmoid_cross_entropy_with_logits`。参见《深度目标检测（五）》的YOLOv3一节。没错，YOLOv3借鉴了Mask R-CNN的这一设计思路。

- **对象实例分割**

Mask R-CNN只对RoI Align后的区域进行分割，而不像U-NET等会对全景进行分割。因此，更适合抠图之类的应用。

参考：

https://zhuanlan.zhihu.com/p/25954683

Mask R-CNN个人理解

https://mp.weixin.qq.com/s/E0P2B798pukbtRarWooUkg

Mask R-CNN的Keras/TensorFlow/Pytorch代码实现

https://zhuanlan.zhihu.com/p/30967656

从R-CNN到Mask R-CNN

https://www.zhihu.com/question/57403701

如何评价Kaiming He最新的Mask R-CNN?

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

R-CNN、Fast/Faster/Mask R-CNN、FCN、RFCN、SSD原理简析

https://mp.weixin.qq.com/s/CsEHuGz_fAq8eWpHRq7d6g

性能超越何恺明Mask R-CNN！华科硕士生开源图像分割新方法

https://zhuanlan.zhihu.com/p/57629509

实例分割的进阶三级跳：从Mask R-CNN到Hybrid Task Cascade

https://mp.weixin.qq.com/s/7Z8unW7Gsu0cf1hAwvjAxw

何恺明等人提TensorMask框架：比肩Mask R-CNN，4D张量预测新突破

https://mp.weixin.qq.com/s/SUZcgq6wOqct_CrWB0j1gA

CVPR2019-实例分割Mask Scoring R-CNN

https://mp.weixin.qq.com/s/Uc0VFMmYoOFvH0c7IExKIg

何恺明团队计算机视觉最新进展：从特征金字塔网络、Mask R-CNN到学习分割一切

https://mp.weixin.qq.com/s/sRU9_M9LsP-j46kNdcI0QQ

Cascade R-CNN升级！目标检测制霸COCO，实例分割超越Mask R-CNN
