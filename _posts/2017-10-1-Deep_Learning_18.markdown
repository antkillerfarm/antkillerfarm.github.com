---
layout: post
title:  深度学习（十八）——目标跟踪
category: DL 
---

# 目标跟踪

## 概述

目标跟踪（object tracking）就是在连续的视频序列中，建立所要跟踪物体的位置关系，得到物体完整的运动轨迹。

目标跟踪分为单目标跟踪和多目标跟踪。本文如无特别指出，均指单目标跟踪。

通常的做法是：

1.在第1帧给一个bbox框住需要跟踪的物体。

2.在不借助重检测（re-detection）的情况下，尽可能长时间的跟住物体。

3.不能使用依赖外部特征的姿态估计（pose estimation）。

当然这是针对目标跟踪算法的要求，至于实际产品中，对象的重检测以及依赖外部特征的姿态估计都是必不可少的。

比如，自动驾驶领域的车辆跟踪，一般都会针对车辆的运动特点建立模型，以辅助目标跟踪。

## TB50 & TB100

这个领域最著名的数据集是吴毅提出的TB50 & TB100，50和100分别代表视频数量。有的论文也把它们称作OTB-2013和OTB-2015。

>吴毅，中国科学院自动化研究所博士（2009年）。南京审计大学副教授。

官网：

http://cvlab.hanyang.ac.kr/tracker_benchmark/

论文：

《Online object tracking: A benchmark》

《Object tracking benchmark》

上面的论文在TB数据集上比较了包括2012年及之前的29个顶尖的tracker，基本解决了统一标准的问题，因此也成为了目标跟踪领域的权威数据集。

《Object tracking: A survey》

这篇论文总结了2006年以前的目标跟踪算法。

## VOT

VOT竞赛数据集是另一个常用数据集。官网：

http://votchallenge.net/challenges.html

OTB包括25%的灰度序列，但VOT都是彩色序列，这也是造成很多颜色特征算法性能差异的原因。

## 目标跟踪的难点

![](/images/article/tracker_hard.png)

![](/images/article/tracker_hard_2.png)

## 参考

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

