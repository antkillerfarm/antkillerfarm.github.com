---
layout: post
title:  深度学习（四十七）——3D处理, NetVLAD, Spatial Transformer Networks
category: DL 
---

* toc
{:toc}

# 3D处理

https://mp.weixin.qq.com/s/19-ggA8nPEmXgMMivI7lLQ

最新“三维深度学习(3D DL):过去与未来”教程，156页ppt

https://github.com/timzhang642/3D-Machine-Learning

3D机器学习资源汇总

https://mp.weixin.qq.com/s/pMhNgAdnfN8wnPKV-aw3hg

基于图像的三维建模参考资料

https://mp.weixin.qq.com/s/CGF-6P4RDtzQnc2XClu5zw

3D计算机视觉简介

https://mp.weixin.qq.com/s/tTdKqRZqBFxFFiZaDD9AJw

三维深度学习中的目标分类与语义分割

https://mp.weixin.qq.com/s/SBWGvu84oPKZkY5d8gz-oQ

3D实例分割

https://mp.weixin.qq.com/s/BVP7vJ1RxwyRoU0purj0Xw

3D数据分类深度学习方法综述，25页论文带你全面了解最新进展

https://zhuanlan.zhihu.com/p/59865273

三维语义分割概述及总结

https://mp.weixin.qq.com/s/wHIT2pm_5O4MOR_QLpHVLQ

PRNet：人脸3D重建与密集对齐

https://mp.weixin.qq.com/s/rxP9PlTJJdw5VmGbwRPMog

解决3D重建难题，伯克利大学根据单张平面彩图重建高精度3D结构

https://mp.weixin.qq.com/s/xIx3TJGoXsjPPDQVQMBdqA

悉尼大学发布首篇《基于图像的自动驾驶三维目标检测》研究进展，阐述3D检测数据、方法与挑战

https://mp.weixin.qq.com/s/RwofVKhBFqo3dYJzy9mlJw

国科大本科生连续在CVPR，AAAI发文，系统提出三维模型库变形分析方法

https://mp.weixin.qq.com/s/aCAFcCWkyAcz-wyqhOldNg

基于图像的场景三维建模

https://mp.weixin.qq.com/s/MldsH2WSI53ljbcbBwyPzA

国防科大、普林斯顿提出共面性检测网络：助力三维场景重建

https://mp.weixin.qq.com/s/Yoledi-OYgJmafxKBlHVPg

国科大本科生以第一作者身份发表AAAI论文，用神经网络分析三维模型

https://mp.weixin.qq.com/s/YQpuYuzk0jv5OngH5u8bEg

阿里菜鸟物流：使用深度强化学习方法求解一类新型三维装箱问题

https://mp.weixin.qq.com/s/gtIRUM58DtLgZtJYBOixvA

单幅RGB图像整体三维场景解析与重建

https://mp.weixin.qq.com/s/zDKjyxqSbqYVlx3TveL8fg

单摄像头数秒构建3D人体模型

https://mp.weixin.qq.com/s/oNXmYpp--GutT66c5p6Q8A

AI说，它可以把你变成个游戏：3D人体模型

https://mp.weixin.qq.com/s/mfdF5I2vsYJeShnOh_cp8Q

MIT新算法骗过神经网络3D物体分类，成功率超90%

https://mp.weixin.qq.com/s/1R9ttaKnXso_quIGTY1nfA

海康、UCLA、北理联合提出3D DescriptorNet：可按条件生成3D形状，克服模式崩溃

https://mp.weixin.qq.com/s/wIdcZh3EkUIm6-91pT2oSQ

DeepMind生成查询网络GQN，无监督学习展现3D场景

https://mp.weixin.qq.com/s/dwtul6XjwRYOtRqmZl2SMg

来看一场AI重建的3D全息世界杯比赛！

https://mp.weixin.qq.com/s/6agNNraba4wlOzWnDfedBg

UIUC & Zillow提出LayoutNet：从单个RGB图像中重建3D房间布局

https://mp.weixin.qq.com/s/af10gqXNfohPEISSnpJ0tQ

UCB吴翼&FAIR田渊栋等人提出强化学习环境Hourse3D

https://mp.weixin.qq.com/s/x0r-2J_YdYgIQlRDqvGofg

CVPR 2017论文解读：用于单目图像车辆3D检测的多任务网络

https://mp.weixin.qq.com/s/OfDaoRb2l_gei--bLSzTRw

宅男福音DeepFake进阶版！基于位置映射图网络进行3D人脸重建

https://mp.weixin.qq.com/s/Fgptle5p9jFtP6JPvjPHNw

腾讯优图提出几何对抗损失函数在单视图3D物体重建中的应用

https://mp.weixin.qq.com/s/0BZxFKjcwxKj7R66NRIWqQ

UC Berkeley新研究：多视角图像3D模型重建技术

https://mp.weixin.qq.com/s/ZTjklje_3fcAn6ZQ23yd4A

ScanComplete：可实现3D扫描的大规模“场景完成”和“语义分割”

https://mp.weixin.qq.com/s/joRcvCQwLYgRd29mfPd7XA

中科大&微软提出立体神经风格迁移模型，可用于3D视频风格化

https://mp.weixin.qq.com/s/a4A5WCFalH1en0IEM3mu2Q

双流束网络：北理工提出深度立体匹配新方法

https://mp.weixin.qq.com/s/XqD1iDdqQomYRMXhie_Xqw

2017以来的2D to 3D

https://mp.weixin.qq.com/s/ZU6NN-jdKzYeSnJweP-E0g

Pixel2Mesh：从单帧RGB图像生成三维网格模型

https://mp.weixin.qq.com/s/pK8zt6UFXtSIS9GAQNgICw

超越像素平面：聚焦3D深度学习的现在和未来

https://mp.weixin.qq.com/s/7P6iMcZUNNVAefrUz-dAsQ

MIT谷歌伯克利三强联手，AI创造超现实主义3D运动雕塑

https://mp.weixin.qq.com/s/bmtZ_-NaRtFitoJumukQSw

通过端到端几何推理发现潜在3D关键点

https://mp.weixin.qq.com/s/ZdzPDb8NWmWkgAufeylYLg

Google AI提出物体识别新方法：端到端发现同类物体最优3D关键点

https://mp.weixin.qq.com/s/ZTUXQVnXfcRVZOoMziTLJQ

AI做不了“真”3D图像？试试Google的新生成模型

https://mp.weixin.qq.com/s/z7m1bj-zip5lqeeC4MHbxg

SkeletonNet：完整的人体三维位姿重建方法

https://mp.weixin.qq.com/s/adSbAAV7POblqE_jaOGZhg

深度学习新应用：在PyTorch中用单个2D图像创建3D模型

https://mp.weixin.qq.com/s/l7RNfRB-KvxRHRKNYx1oeQ

云从3D人体重建登顶三项榜单，一张照片就能生成3D形象

https://mp.weixin.qq.com/s/SnvXvijf5gBMNsnVW1-KMA

一张照片获得3D人体信息，云从科技提出新型DenseBody框架

https://mp.weixin.qq.com/s/UthnL7fqEIEGvzcaXmFHKQ

基于骨架表达的单张图片三维物体重建方法

https://mp.weixin.qq.com/s/h8tk28IEuqB7g1e-vsyN5g

2DASL：目前最好的开源人脸3D重建与密集对齐算法

https://zhuanlan.zhihu.com/p/62964002

AM3D:高精度单目三维检测器

https://mp.weixin.qq.com/s/6hXNcm4ICNqa2Ezao-oE-g

AR版“神笔马良”：从单张2D图片建立3D人物运动模型，华盛顿大学与Facebook 3D重建

https://mp.weixin.qq.com/s/pPgjBmct11POKqeVv8ymIA

3D重建：硬派几何求解vs深度学习打天下？

https://mp.weixin.qq.com/s/uKCQVpkN6Va8jZisAyAfNA

3D人脸技术漫游指南

https://mp.weixin.qq.com/s/S6cf_d6CxPblRS-hnM-jSg

从多视角RGB图像生成三维网格模型Pixel2Mesh++

https://mp.weixin.qq.com/s/srHmkY_t3ChFAzhvXG6RPA

图像转换3D模型只需5行代码，英伟达推出3D深度学习工具Kaolin

https://mp.weixin.qq.com/s/CpzqhkmZOn-2Qavk90RTvQ

初学深度学习单张图像三维人脸重建需要读的文章

https://mp.weixin.qq.com/s/HQLgiRfJ0KzstPfGatKHOA

基于Siamese网络的多视角三维人脸重建

https://mp.weixin.qq.com/s/CA7_70Y5EZkULOYQ2g9O-A

Hinton最新研究NASA：一种更好地学习三维模型动作的方法

https://mp.weixin.qq.com/s/_59iRdLATm2n99M89JBqQA

KITTI立体匹配霸榜论文算法详解

https://mp.weixin.qq.com/s/npKNDrlXZ6oIlE0NG80BpQ

Geoffrey Hinton再出神作：通过神经网络估计有关节可变形的三维模型

https://zhuanlan.zhihu.com/p/112103579

深度学习在三维环境重建中的应用

https://mp.weixin.qq.com/s/cBbDCE4kQrahT8vLo3uMOw

你们还在做2D的物体检测吗？谷歌已经开始玩转3D了

https://mp.weixin.qq.com/s/SMDzPAJCpgjELsKHeQvW-A

序列化的三维形状生成网络PQ-NET

https://mp.weixin.qq.com/s/qZOCUuLt98fTpstGpC1ilg

Variational DropPath：提高3D CNN时空融合分析效率的秘诀

https://mp.weixin.qq.com/s/BNYUkocbKzqTjTNbGuNBzg

基于3DMM的三维人脸重建技术总结

https://mp.weixin.qq.com/s/-9e86JF7VyHbqTyn2whOzg

如何入门多视角人脸正面化生成？不得不看的超详细最新综述！

## 三维重建

三维重建是计算机视觉和深度学习的重要任务。目前三维重建按照其表达方式主要分为两大类：一类是显式表达，以点云为代表（也有生成网格和体素），由神经网络直接回归生成三维空间中的几何元素；另一类是隐式表达（如NeRF），神经网络只是建模三维物体的空间占用，需要后续的渲染或表面提取获得显式三维形状。

https://www.cnblogs.com/wuyida/p/6301262.html

三维重建技术概述

https://blog.csdn.net/wishchin/article/category/5723249

一个三维重建/SLAM的blog

https://zhuanlan.zhihu.com/p/79628068

基于深度学习的视觉三维重建研究总结

https://mp.weixin.qq.com/s/C2h0xvcHCrnEYcDgHse5xA

从NeRF -> GRAF -> GIRAFFE，2021 CVPR Best Paper诞生记

https://mp.weixin.qq.com/s/ihrExTygb-Pnnh4o4tAYnQ

基于单目视觉的三维重建算法综述

https://mp.weixin.qq.com/s/Uj2gYOS2TZGVSCka57M0zA

MVSNet: 非结构化多视点三维重建网络

https://mp.weixin.qq.com/s/gRFq44pnhRScO9p3M15Gfw

基于三维重建的全新相机姿态估计方法

https://mp.weixin.qq.com/s/GyE86DG7VqW0n4p0IP-lmQ

R-MVSNet:一个高精度高效率的三维重建网络

https://www.zhihu.com/column/c_1451693094165516288

人体三维重建与虚拟试衣

https://mp.weixin.qq.com/s/LeiaqqqZ4uI0cmUU0rxYhA

一篇文章教你学会使用三维重建知名开源系统

https://mp.weixin.qq.com/s/BmlICavHIKV72rt1aBU3pg

一文了解3DGS在CV应用领域的SOTA进展

## NetVLAD

NetVLAD算的上是CNN+传统算子的一个范例。

论文：

《NetVLAD: CNN architecture for weakly supervised place recognition》

《GhostVLAD for set-based face recognition》

数据集：

http://places.csail.mit.edu/

## VLAD

Vector of Locally Aggregated Descriptors

https://www.cnblogs.com/minemine/p/7364950.html

场景分类(scene classification)摘录

http://www.cnblogs.com/mafuqiang/p/6909556.html

图像检索——VLAD

## 参考

https://www.oukohou.wang/2018/11/27/NetVLAD/

论文阅读-NetVLAD

https://www.oukohou.wang/2018/12/26/GhostVLAD/

论文阅读-GhostVLAD

https://mp.weixin.qq.com/s/cfUl0Eym0mu7rSJJL7Zt1A

基于深度学习的视觉实例搜索研究进展

https://zhuanlan.zhihu.com/p/25013378

深度纹理编码网络 (Deep TEN: Texture Encoding Network)

https://blog.csdn.net/LiGuang923/article/details/85416407

图像检索与降维（一）：VLAD

https://blog.csdn.net/LiGuang923/article/details/85470289

图像检索与降维（二）：NetVLAD

https://zhuanlan.zhihu.com/p/96718053

从VLAD到NetVLAD，再到NeXtVlad

https://mp.weixin.qq.com/s/HkaG9KeJ5w6FqZmo15n9JA

短视频潜力预测及其在微视推荐冷启动中的应用

https://mp.weixin.qq.com/s/WUP4C4XBHfVKI_xgHXUXiA

动作识别时序汇合（Temporal Pooling）方法介绍

# Spatial Transformer Networks

论文：

《Spatial Transformer Networks》

参考：

http://www.cnblogs.com/neopenx/p/4851806.html

Spatial Transformer Networks(空间变换神经网络)

http://blog.csdn.net/shaoxiaohu1/article/details/51809605

论文笔记：Spatial Transformer Networks

http://blog.csdn.net/shaoxiaohu1/article/details/51809605

Spatial Transformer Networks

https://mp.weixin.qq.com/s/ciqQMezcB-oM24X8eQqTNg

花式玩耍Spatial Transformation Networks

https://mp.weixin.qq.com/s/4VE2lZeFf05AyLp_s3nTFQ

理解Spatial Transformer Networks

https://mp.weixin.qq.com/s/AI_QC8IgnjMSOAZJNbQjfQ

空间变换网络简单介绍
