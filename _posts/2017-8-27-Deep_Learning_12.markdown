---
layout: post
title:  深度学习（十二）——姿态/行为检测
category: DL 
---

# 姿态/行为检测

基于CNN的2D多人姿态估计方法，通常有2个思路（Bottom-Up Approaches和Top-Down Approaches）：

- Top-Down framework，就是先进行行人检测，得到边界框，然后在每一个边界框中检测人体关键点，连接成每个人的姿态，缺点是受人体检测框影响较大，代表算法有RMPE。

- Bottom-Up framework，就是先对整个图片进行每个人体关键点部件的检测，再将检测到的人体部位拼接成每个人的姿态，代表方法就是openpose。

## OpenPose

OpenPose是一个实时多人关键点检测的库，基于OpenCV和Caffe编写。它是CMU的Yaser Sheikh小组的作品。

>Yaser Ajmal Sheikh，巴基斯坦信德省易司哈克工程科学与技术学院本科（2001年）+中佛罗里达大学博士（2006年）。现为CMU副教授。

![](/images/article/openpose.png)

OpenPose的使用效果如上图所示。

论文：

《Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields》

《Hand Keypoint Detection in Single Images using Multiview Bootstrapping》

《Convolutional pose machines》

官方代码（caffe）：

https://github.com/CMU-Perceptual-Computing-Lab/openpose

Tensorflow版本：

https://github.com/ildoonet/tf-pose-estimation

参考：

https://zhuanlan.zhihu.com/p/37526892

OpenPose：实时多人2D姿态估计

## DensePose

与OpenPose类似的还有Facebook提出的DensePose。

论文：

《DensePose: Dense Human Pose Estimation In The Wild》

数据集：

http://densepose.org/

这里包含了一个名为DensePose-COCO的姿态数据集。

参考：

https://mp.weixin.qq.com/s/sFd9hrMrKDl5UJwlY6N7mw

Facebook提出DensePose数据集和网络架构：可实现实时的人体姿态估计

https://mp.weixin.qq.com/s/t29ITfRPD3yCmRD5wJyq7g

ICCV2017 PoseTrack challenge优异方法：基于检测和跟踪的视频中人体姿态估计

https://mp.weixin.qq.com/s/mGcKpu3BXlAGO-t2FUCxAg

基于深度模型的人脸对齐和姿态标准化

https://mp.weixin.qq.com/s/gwRD3SzTof349V8W0_lRfg

实时评估世界杯球员的正确姿势：FAIR开源DensePose

https://zhuanlan.zhihu.com/p/39219404

Dense Pose

https://blog.csdn.net/sinat_26917383/article/details/79704097

关键点定位：四款人体姿势关键点估计论文笔记

https://mp.weixin.qq.com/s/-A87-z5inWBsF1-5UYagTA

Facebook实时人体姿态估计：Dense Pose及其应用展望

## Hourglass networks

Hourglass networks是University of Michigan的Alejandro Newell的作品。（2016年3月）

论文：

《Stacked hourglass networks for human pose estimation》

![](/images/img3/Hourglass_Networks.png)

上图是Stacked Hourglass networks的网络结构图，其中的每个沙漏形状的结构，都是一个hourglass module，其结构如下图所示：

![](/images/img3/Hourglass_Networks_2.png)

hourglass module基本可以看作是把concat换成add之后的U-NET，或者也可以看作是resnet版的U-NET。上图中一个module包含了4次add，因此也被叫做4阶hourglass module。

参考：

https://blog.csdn.net/shenxiaolu1984/article/details/51428392

Stacked Hourglass算法详解

## 步态识别

https://mp.weixin.qq.com/s/g6032xTGEtvbsfwXboMJ4A

大阪大学副校长Yasushi Yagi：步态分析

http://mp.weixin.qq.com/s/Y-PvMz_Vz8nBGRZo9dwUCA

中科院步态识别技术：不看脸50米内在人群中认出你！

https://mp.weixin.qq.com/s/3Pe5wJ0VomzwKMF84OqcMg

步态识别的深度学习综述

https://mp.weixin.qq.com/s/afX8Y84nTS20q4Y36uOWqQ

复旦提出GaitSet算法，步态识别的重大突破！
