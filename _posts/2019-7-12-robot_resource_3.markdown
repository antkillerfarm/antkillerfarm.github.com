---
layout: post
title:  机器人/无人驾驶（三）——组合导航, 软件仿真, Apollo, Autoware
category: robot 
---

* toc
{:toc}

# 机器人/无人驾驶

## 车道线检测

https://blog.csdn.net/Young_Gy/article/details/75194914

无人驾驶之车道线检测简易版

https://mp.weixin.qq.com/s/M-J_WaV6AstRwZISPYQ-NA

再识图像之高级车道线检测

https://mp.weixin.qq.com/s/i793SOBdI4GyrfG_5kb3lg

鲁汶大学提出可端到端学习的车道线检测算法

https://zhuanlan.zhihu.com/p/58980251

基于摄像头的车道线检测方法一览

https://mp.weixin.qq.com/s/SlQy59AHyh1BEzDpKOu5GA

初识图像之初级车道线检测

https://mp.weixin.qq.com/s/ANKdDuCxMZAWHEfQbN54MA

再识图像之高级车道线检测

https://mp.weixin.qq.com/s/K5MJTYmuJ4Vv0sKT-VpDvg

一种基于ACP理论的平行车道线检测方法，能有效解决目前车道线检测的困境

https://mp.weixin.qq.com/s/heh6B71IQ5NjmaTo6BaXdA

无人驾驶图像识别-车道线检测

https://mp.weixin.qq.com/s/aaqd0crHdk2WyKO4J9hmDg

自动驾驶中的车道线跟踪技术

https://mp.weixin.qq.com/s/NvVh3GO9iOGsftYgOlRb3Q

300+FPS！浙大提出一种超快速车道线检测方法

https://mp.weixin.qq.com/s/ekN9AvS4V0dcDdBlGCs3Bg

车道线检测：SCNN

https://mp.weixin.qq.com/s/xXdbWpegLdxFMuHldMGKVA

车道线Dark SCNN算法简述及车道线后处理代码细节简述

https://www.cnblogs.com/guoyaohua/p/8940871.html

SCNN车道线检测--(SCNN)Spatial As Deep: Spatial CNN for Traffic Scene Understanding（论文解读）

https://mp.weixin.qq.com/s/pQUKFZRgG1LNPmE6oHY3GA

CurcveLane-NAS：华为&中大提出一种结合NAS的曲线车道检测算法

https://mp.weixin.qq.com/s/HSh029kg2-rOtD688vLq2w

详解车道线检测算法之传统图像处理

https://mp.weixin.qq.com/s/4YUVlSaahIxxWs5gwVLbRw

基于图像处理的车道线检测

https://mp.weixin.qq.com/s/hjHXWewRYh_6j5cd1J2n_w

Transformer又立功了！又快(420 fps)又好的车道线检测算法

https://zhuanlan.zhihu.com/p/157530787

超快的车道线检测

## 组合导航

https://mp.weixin.qq.com/s/F8aLNue_qDlMyTAznBLC7g

GPS/IMU/DMI组合导航方法研究

https://mp.weixin.qq.com/s/tin5FtrJLnuKZCGDr8s3GA

组合导航系列文章（一）：开篇

https://mp.weixin.qq.com/s/BLEP6nmGSfPY3iMh_N3JKw

组合导航系列文章（二）：惯性器件综述

https://zhuanlan.zhihu.com/p/515485538

高精度组合导航里的松、紧、深耦合

https://mp.weixin.qq.com/s/YypYIwbXavrnodGENNfZnw

RTK，自动驾驶之锚

## 泊车

**半自动泊车**系统为驾驶员操控车速，计算平台根据车速及周边环境来确定并执行转向，对应于SAE自动驾驶级别中的L1；

**全自动泊车**为计算平台根据周边环境来确定并执行转向和加减速等全部操作，驾驶员可在车内或车外监控，对应于SAE L2级。

**自主泊车**又称为代客泊车或一键泊车：指驾驶员可以在指定地点处召唤停车位上的车辆，或让当前驾驶的车辆停入指定或随机的停车位。整个过程正常状态下无需人员操作和监管，对应于SAE L3级别。

参考：

https://zhuanlan.zhihu.com/p/56236181

自动/自主泊车技术简介

https://mp.weixin.qq.com/s/GN4u91Dc5uYLP8N8rptczA

全自动泊车辅助F-APA简介

https://mp.weixin.qq.com/s/o74udojH7jGPPoxJSSBdMg

停车不再难，L2到L4的泊车辅助系统技术剖析

https://mp.weixin.qq.com/s/M5VphyTAFXiK2mJhIsvyvA

从自动泊车到自主泊车

## 软件仿真

https://mp.weixin.qq.com/s/yNr4_tEyYg_VBmNjIrjYug

自动驾驶车辆仿真模拟软件盘点

https://zhuanlan.zhihu.com/p/66961439

交通模型仿真工具SUMO介绍

https://mp.weixin.qq.com/s/eQJ8dgNNd3iE2fXXUcXG2A

一贴集齐86种交通建模与仿真软件综述

https://mp.weixin.qq.com/s/Ja6DxqadbFBIWUk44DjGqw

密集场景中自动驾驶车辆的仿真与导航

https://zhuanlan.zhihu.com/p/57169482

自动驾驶研发模拟仿真系统的工作介绍

https://zhuanlan.zhihu.com/p/66963787

自动驾驶模拟仿真系统中的传感器模型

https://mp.weixin.qq.com/s/E-Vo3mqANB21Gv-X3b6o1A

用虚拟仿真平台实现ADAS目标融合、检测和跟踪

https://mp.weixin.qq.com/s/seAKqsJDcPHAa_BZDfav_w

谈谈自动驾驶仿真

https://mp.weixin.qq.com/s/pOX84vc2JlYidkTZt73rcQ

英特尔&丰田联合开源城市驾驶模拟器CARLA

https://mp.weixin.qq.com/s/bs6DQveNwJh6auIAExj4VA

驾驶模拟器之CARLA篇：An Open Urban Driving Simulator

https://mp.weixin.qq.com/s/yHP9bKUtQRR1YPcI3nTjiQ

机器人系统仿真

## 电气&机械

电气&机械虽然不是控制系统算法的一部分，但却是控制系统的物理基础。

https://mp.weixin.qq.com/s/tSrb5wljrCvy3TgfbsyjOQ

汽车电源为何抛弃12V选择48V？

https://mp.weixin.qq.com/s/XJVyoQTDljO8F6GVthsxKw

汽车安全气囊结构原理

## Boston Dynamics

Boston Dynamics算是人形机器人的标杆企业了。它采用了液压驱动的路线。

相比于电机驱动，液压驱动功率更大，环境适应性更好，缺点是非线性系统，控制比较复杂。

参考：

https://mp.weixin.qq.com/s/BNanIQKY9SSNpms0qSTOBA

硬核干货！揭秘波士顿动力背后的专利技术

https://www.zhihu.com/question/437149222

波士顿动力12月29日发布了机器人跳舞的视频，在这个领域我们现在差距有多大？

https://www.zhihu.com/question/347701202

波士顿动力旋转跳跃360°视频火了，那么它的背后是通过什么算法实现的呢？

## 大疆

https://zhuanlan.zhihu.com/p/59329666

如何使用大疆无人机开展国土三调

https://www.zhihu.com/question/292721850

大疆为什么不做油动无人机？

https://www.zhihu.com/answer/3038289774

大疆的国产化率

## Apollo

Apollo是百度开源的无人驾驶平台，也是目前已开源的平台中最专业的。

官网：

http://apollo.auto/

代码：

https://github.com/ApolloAuto/apollo

参考：

https://github.com/TeamStuQ/skill-map/blob/master/data/map-Apollo.md

Apollo自动驾驶工程师技能图谱

https://mp.weixin.qq.com/s/qtoHF4Mnj_aeGBJbkjJUMA

Apollo小度车载系统

https://mp.weixin.qq.com/s/rWhnazEC35U7nsVpsxhFEg

Apollo 2.0软硬件框架初探（一）

https://mp.weixin.qq.com/s/GccST3xJ1QRIVM7cFgsn3A

Apollo 2.0软硬件框架初探（二）

https://mp.weixin.qq.com/s/UHQXF2Ju8erw9thm0Jjt2A

Apollo 2.0框架及源码分析(三)

https://mp.weixin.qq.com/s/3i8mJn4OPjt-KV1fp8bZQQ

Apollo Planning模块源代码分析（上）

https://mp.weixin.qq.com/s/DfdMDOQMncTau-9zGq9CYw

Apollo Planning模块源代码分析（下）

https://mp.weixin.qq.com/s/bH6khM8gO3JE4YT9i8hSlQ

Apollo 2.0自动驾驶平台技术解析与应用

https://mp.weixin.qq.com/s/EgsdlDhd8lIXU3bnTYJh0w

Apollo高精地图解析

https://mp.weixin.qq.com/s/027QogNXtW2NPIDVcGQGzw

Apollo“云+端”研发迭代新模式实战

https://mp.weixin.qq.com/s/Pjfhs-03HxFeyeKo1BmAPw

Apollo小度车载系统打造更舒心的出行

https://mp.weixin.qq.com/s/j4faPastB3nFmgH_nhVW6g

Apollo仿真平台如何Hold住99.9999%的复杂场景？

https://mp.weixin.qq.com/s/PFCCRBGWmaTQkeyOlG1qng

Apollo 2.0多传感器融合定位模块

https://mp.weixin.qq.com/s/KEdXmwUGHysy_LZAfdkmlw

Apollo的分布式可扩展计算平台探索

https://mp.weixin.qq.com/s/MfTsH9mowuWFA54z7vs5fA

Apollo Control模块源码解析

https://mp.weixin.qq.com/s/bIjnm74cJcHP8Zab4kt5zA

Apollo 2.5实时相对地图的分享

https://mp.weixin.qq.com/s/hU8__GPgxaCStSgv4FyLIw

无人驾驶行业及Apollo的Overview

https://mp.weixin.qq.com/s/48DcWP1kAoze0Lv8jHY3Ow

Apollo 2.5预测系统介绍

https://mp.weixin.qq.com/s/7ftd941pycD7h_R10cDecg

Apollo 2.5自动驾驶规划控制

https://mp.weixin.qq.com/s/y_mOKpgLEvDjoO-UdMBcyw

Apollo AdaperManager和Routing模块源代码分析

https://mp.weixin.qq.com/s/k5GNHhxEE6mIOnxXlCpKXg

Apollo 3.5软件架构

https://mp.weixin.qq.com/s/Xhy1w87l_b47_hS3zzCgaA

3D障碍物感知

https://mp.weixin.qq.com/s?__biz=MzI1NjkxOTMyNQ==&mid=2247488270&idx=1&sn=25b6e9b4fbe02ef6c84f3171c4bd2e2a

Apollo决策技术分享

https://mp.weixin.qq.com/s/E65x4kNNZ4ctc12e5v095w

自动驾驶专用计算框架探索和实践

## Autoware

Autoware是另一个开源的无人驾驶平台。不像Apollo，没有百度这样的强势公司的介入，社区氛围更浓一些，相对的，功能也要弱一些。

官网：

https://www.autoware.org/

主要由一下组件构成：

- autoware.ai

https://www.autoware.ai/

这个组件基于ROS 1.0，是目前的方案。

- autoware.auto

https://www.autoware.auto/

这个组件基于ROS 2.0，是面向未来的方案。

- autoware.io

https://www.autoware.io/

autoware提供的模拟器。

代码仓库：

https://gitlab.com/autowarefoundation/autoware.ai
