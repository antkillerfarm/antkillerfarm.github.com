---
layout: post
title:  深度学习（二十五）——多任务学习, 人脸检测/识别（2）
category: DL 
---

* toc
{:toc}

# 多任务学习

http://cs330.stanford.edu/

CS 330: Deep Multi-Task and Meta Learning

https://mp.weixin.qq.com/s/guAgXdhZSbEAkERSB1sLRA

多任务学习-Multitask Learning概述

https://mp.weixin.qq.com/s/A-CVKTz_moaFzTYywSt2gg

张宇 杨强：多任务学习概述

https://zhuanlan.zhihu.com/p/148132708

最新《深度多任务学习》综述论文

https://zhuanlan.zhihu.com/p/67524006

多任务学习综述-A Survey on Multi-Task Learning

https://mp.weixin.qq.com/s/QXOy2jo4RhCZrD5bSVzBOQ

共享相关任务表征，一文读懂深度神经网络多任务学习

https://zhuanlan.zhihu.com/p/59413549

Multi-task Learning(Review)多任务学习概述

https://mp.weixin.qq.com/s/X6FwTgr282hbqgOz3oBX-w

多任务学习概述论文：从定义和方法到应用和原理分析

https://blog.csdn.net/CoderPai/article/details/80080455

多任务学习与深度学习

https://blog.csdn.net/CoderPai/article/details/80087188

利用TensorFlow一步一步构建一个多任务学习模型

https://mp.weixin.qq.com/s/mm9bXXTEzd8DwyYlMgGMZg

NLP多任务学习：一种层次增长的神经网络结构

https://mp.weixin.qq.com/s/fcFb6WkJVP8TYpoxkQgiWQ

CMU提出“十字绣网络”，自动决定多任务学习的最佳共享层

https://mp.weixin.qq.com/s/i7WAFjQHK1NGVACR8x3v0A

自然语言十项全能：转化为问答的多任务学习

https://mp.weixin.qq.com/s/NpO1UP_mzyaeqW26xLY1Xg

CVPR 2018最佳论文作者亲笔解读：研究视觉任务关联性的Taskonomy

https://mp.weixin.qq.com/s/DUSa3SW1AgvJ0szL030NHQ

一个AI玩57个游戏，DeepMind离真正“万能”的AGI不远了！

https://mp.weixin.qq.com/s/P81I5vl99mV-4StNHmd_6A

作为多目标优化的多任务学习：寻找帕累托最优解

https://mp.weixin.qq.com/s/cyX_FzHF-6df9_AMUDu1aQ

One Model To Learn Them All

https://mp.weixin.qq.com/s/oY2yGrTmIpr3H2qvzYKInA

谷歌一个模型解决所有问题《One Model to Learn Them All》论文深度解读

https://mp.weixin.qq.com/s/VMHyEOn8uyRYt39QXVWUww

西湖大学张岳：自然语言处理中的多任务联合学习（384页PPT）

https://mp.weixin.qq.com/s/5zo-2WB9v2hOvKAC7ZhKuQ

基于学习的多任务框架L2MT，为多任务问题选择最优模型

https://mp.weixin.qq.com/s/MPhKUosKZbLtVjJ1XYGXYA

如何利用深度学习模型实现多任务学习？这里有三点经验

https://mp.weixin.qq.com/s/Oopgglg2G7TwnXeN2DtZhA

多任务+注意力机制的学习

https://mp.weixin.qq.com/s/vMgHCQ03Gt5v6GdgW-pY9A

一个神经网络实现4大图像任务，GitHub已开源

https://mp.weixin.qq.com/s/Zui8FFn1FDP_UoAGMH0L7Q

知其然，知其所以然：基于多任务学习的可解释推荐系统

https://mp.weixin.qq.com/s/8Ablt7Sa86DXTB1dE_RqaA

港中文开源基于PyTorch的多任务人脸识别框架

https://www.zhihu.com/question/345173757

多任务学习成功的原因是引入了别的数据库还是多任务框架本身呢？

https://zhuanlan.zhihu.com/p/52566508

深度神经网络中的多任务学习汇总

https://mp.weixin.qq.com/s/DUsXj46TCZ9w_Dxgw4ZH8g

用于多任务CNN的随机滤波分组，性能超现有基准方法

https://mp.weixin.qq.com/s/-SHLp26oGDDp9HG-23cetg

多任务学习在推荐算法中的应用

https://mp.weixin.qq.com/s/avWy3-mju0d0QjufX_bi4Q

Multi-task Learning的三个实用小知识

https://mp.weixin.qq.com/s/hLJB8yH8V0Ncug77jHU1Bw

模型独立学习：多任务学习与迁移学习

https://zhuanlan.zhihu.com/p/138597214

Multi-task Learning and Beyond: 过去，现在与未来

https://mp.weixin.qq.com/s/gIEJoE9B8mmNx6u6bqLspQ

最新《深度多任务学习》综述论文，22页pdf

https://mp.weixin.qq.com/s/ifTNRW0W7-P_LyfNldtavQ

多任务学习方法在推荐中的演变

# 人脸检测/识别

## OHEM

论文：

《Training Region-based Object Detectors with Online Hard Example Mining》

代码：

https://github.com/abhi2610/ohem

参考：

https://blog.csdn.net/zimenglan_sysu/article/details/51318058

论文笔记

https://blog.csdn.net/Wayne2019/article/details/78945099

OHEM，Batch Hard（识别乱入），Focal Loss

https://blog.csdn.net/Z5337209/article/details/72838049

Faster R-CNN 深入理解 && 改进方法汇总

https://zhuanlan.zhihu.com/p/28202204

困难样本挖掘

https://zhuanlan.zhihu.com/p/34179420

目标检测入门（二）：模型的评测与训练技巧

## FaceNet

论文：

《FaceNet: A Unified Embedding for Face Recognition and Clustering》

https://blog.csdn.net/stdcoutzyx/article/details/46687471

FaceNet--Google的人脸识别

## OpenFace

OpenFace是一款开源的人脸识别软件。它的原理基于CVPR 2015年的论文：FaceNet。由于采用了深度学习技术，OpenFace对人脸识别的准确率，大大超过了OpenCV。

OpenFace是用Python和Torch编写的。

官网：

https://cmusatyalab.github.io/openface/

参考：

http://www.cnblogs.com/pandaroll/p/6590339.html

开源人脸识别openface

https://mp.weixin.qq.com/s/RSCrkeIToeNKrFvMITxzDg

通过OpenFace来理解人脸识别

## SeetaFace

SeetaFace人脸识别引擎由中科院计算所山世光研究员带领的人脸识别研究组研发。代码基于C++实现，且不依赖于任何第三方的库函数，开源协议为BSD-2，可供学术界和工业界免费使用。

代码：

https://github.com/seetaface

第一代

https://github.com/seetafaceengine/SeetaFace2

第二代

论文：

《Coarse-to-Fine Auto-Encoder Networks (CFAN) for Real-Time Face Alignment》

参考：

https://zhuanlan.zhihu.com/p/22451474

SeetaFace开源人脸识别引擎介绍

http://www.cnblogs.com/nenya33/p/6801045.html

CFAN

https://mp.weixin.qq.com/s/iESGCZjdvwUzqW7pu0GwrQ

中科视拓开源SeetaFace2人脸识别算法

## S3FD

https://mp.weixin.qq.com/s/MyA8_yt4YCkFl67AyhpZow

尺度不变人脸检测器（S3FD-Single Shot Scale-invariant Face Detector）

https://github.com/sfzhang15/SFD

S3FD: Single Shot Scale-invariant Face Detector

## 人脸属性

人脸属性包括：人脸年龄、表情、颜值等。

相关应用可分为两类：

Get类：从图片中获得人脸属性，比如年龄估计、表情识别、颜值打分等。

Set类：编辑人脸属性。比如给一张年轻时的照片，来预估年老时的样子。

参考：

https://mp.weixin.qq.com/s/M952HsJnmfx3EGmiDcySGQ

初学深度学习人脸属性分析必读的文章

https://zhuanlan.zhihu.com/p/53229759

年龄估计技术综述

https://www.openu.ac.il/home/hassner/projects/cnn_agegender/

Age and Gender Classification using Convolutional Neural Networks

https://mp.weixin.qq.com/s/0hVPateb108B1KpVpYXK0A

人工智能：长相越“娘”颜值越高

https://mp.weixin.qq.com/s/YlrWHDPIPzN4dQO2vo4DjA

人脸颜值研究综述

https://www.oukohou.wang/2019/01/30/face-aging_using_GAN/

论文阅读-人脸老化：Generative Adversarial Style Transfer Networks for Face Aging

https://mp.weixin.qq.com/s/s2SFQgUOZLV-f6auqymVOg

基于Caffe的年龄&性别识别

https://mp.weixin.qq.com/s/I3MS1gB1yjSnadex4LfOYA

旷视提出极轻量级年龄估计模型C3AE

https://mp.weixin.qq.com/s/76DJk4mZ5tBEjQENWmFAEg

STGAN: 人脸高精度属性编辑模型

https://mp.weixin.qq.com/s/BwqOABORt6B9qd9mEufa7w

人脸年龄编辑：无可奈何花落去，似曾相似春又来！

https://mp.weixin.qq.com/s/17Q21OazunLEicOaJUyMxg

人脸属性编辑都有哪些核心知识点，如何长期进行学习

https://mp.weixin.qq.com/s/hjeOBKuyR87zBWZHqb3gow

想提前目睹人到中年的发型？试试这款自制秃头生成器

https://mp.weixin.qq.com/s/egU6SkpKjCznConQfg_Zmw

人脸妆造迁移核心技术总结

https://mp.weixin.qq.com/s/FSRwIAwTBZUsT1GVPglZkA

深度学习换脸算法都有哪些？

https://mp.weixin.qq.com/s/m1TBxG_1wlfUUeDwP6Aleg

人脸算法新热点，人脸编辑都有哪些方向，如何学习

## 活体检测

https://mp.weixin.qq.com/s/zOnmKSnQctnyx7pKXX-tpQ

活体检测新文解读：利用多帧人脸来预测更精确的深度

https://mp.weixin.qq.com/s?__biz=MzU4MjQ3MDkwNA==&mid=2247486721&idx=1&sn=f0e5b2b0165e391c0d5adc4ce253f2f6

人脸识别中的活体检测算法综述

https://mp.weixin.qq.com/s/A1pbiU5PA9Owe69lGX9afw

活体识别告诉你为什么照片无法破解人脸系统

https://mp.weixin.qq.com/s/rroBWY0roDdRap28i-KYtQ

初学活体检测与伪造人脸检测必读的文章

https://mp.weixin.qq.com/s/sPnoZyCkAhcCs_GtA79DrA

单目可见光静默活体检测Binary or Auxiliary Supervision论文解读

https://mp.weixin.qq.com/s/Vi2ypwO3uCD2lQZOwDFqTA

基于rPPG的人脸活体检测综述

https://mp.weixin.qq.com/s/ViQDmUqNiXCQ1KzvlLI6Wg

OpenCV活体检测

https://mp.weixin.qq.com/s/2I8A_-utI4DqeDXJwwX4UQ

人脸反欺诈活体检测综述

https://mp.weixin.qq.com/s/8q2gk9ruVw4yxf72mgZkww

CVPR2020人脸防伪检测挑战赛冠军方案开源

https://mp.weixin.qq.com/s/wQcP_dqv9KI3jEQ0soYbMw

人脸静默活体检测最新综述

## DeepFace

《DeepFace: Closing the Gap to Human-Level Performance in Face Verification》

DeepFace先进行了两次全卷积＋一次池化，提取了低层次的边缘／纹理等特征。

后接了3个Local-Conv层，这里是用Local-Conv的原因是，人脸在不同的区域存在不同的特征（眼睛／鼻子／嘴的分布位置相对固定），当不存在全局的局部特征分布时，Local-Conv更适合特征的提取。

## 人脸关键点

https://mp.weixin.qq.com/s/qWMQV33k1bDAHWwwVod3_g

初学深度学习人脸关键点检测必读文章

https://zhuanlan.zhihu.com/p/42968117

人脸关键点检测综述

https://mp.weixin.qq.com/s/CvdeV5xgUF0kStJQdRst0w

从传统方法到深度学习，人脸关键点检测方法综述

https://mp.weixin.qq.com/s/ZrnAqDJCLtMy_qTQ2RZT0A

级联MobileNet-V2实现人脸关键点检测

https://mp.weixin.qq.com/s/ymeJPUPRAGb1FltskqBs-A

人脸关键点检测汇总（上）

https://mp.weixin.qq.com/s/N6y-RDx7VszgCVhSiwP8jA

人脸关键点检测汇总（下）

https://mp.weixin.qq.com/s/D435jGsGPkCH5j-p8Zoksg

遮挡、光照等因素的人脸关键点检测

https://mp.weixin.qq.com/s/BV3xv8mH6K7dV1nik0X5aw

PFLD：简单、快速、超高精度人脸特征点检测算法

https://mp.weixin.qq.com/s/kWRW81aAMl18GIDQWqX1Ow

PFLD：简单高效的实用人脸关键点检测算法

https://mp.weixin.qq.com/s/HpgcPkAZb9R5jF-zsGcfcw

美图影像实验室（MTlab）10000点人脸关键点技术全解读

https://mp.weixin.qq.com/s/YsuvIB56OxS9IBMQYSUdUg

深度学习AI美颜系列——人像静态/动态贴纸特效算法实现
