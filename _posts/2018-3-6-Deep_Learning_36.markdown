---
layout: post
title:  深度学习（三十六）——深度推荐系统（2）, 人脸检测/识别（2）, AI可解释性
category: DL 
---

# 深度推荐系统

https://zhuanlan.zhihu.com/p/57234648

深入解读Airbnb推荐算法

https://mp.weixin.qq.com/s/oOQfV35x9IJgE0y_ecjntA

解读Airbnb的个性化搜索排序算法

https://mp.weixin.qq.com/s/2Th2PO4rC-r4LOU46uYDjg

重读Youtube深度学习推荐系统论文，字字珠玑，惊为神文

https://mp.weixin.qq.com/s/mcf3Aoub6VbNwtA-Izsdbg

YouTube深度学习推荐系统的十大工程问题

https://mp.weixin.qq.com/s/A7Qg1gd66aksA6npJl3dNQ

深度学习在搜索业务中的探索与实践

https://mp.weixin.qq.com/s/wjgoH6-eJQDL1KUQD3aQUQ

大众点评搜索基于知识图谱的深度学习排序实践

https://mp.weixin.qq.com/s/Dmkc7X-GgRrdNVQXz6Mwgg

镶嵌在互联网技术上的明珠：漫谈深度学习时代点击率预估技术进展

https://mp.weixin.qq.com/s/-EDxUwAKdXnmUKeLJTiAUw

解构阿里深度兴趣网络（DIN）：如何将注意力机制引入推荐系统？

https://mp.weixin.qq.com/s/lnCe_3ssP6IEOHEX-N4vyg

达观数据于敬：个性化推荐系统实践应用

https://mp.weixin.qq.com/s/CYW-WMl94FT2-fqsxShNDw

读者喜欢看什么文章？腾讯微信融合时间过程与内容特征寻找答案

https://mp.weixin.qq.com/s/4k106lCBns-iEg3gy5mcbg

WSDM 2019教程—李航、何向南等，深度学习匹配在搜索和推荐中的应用

https://mp.weixin.qq.com/s/SYFShtmBIgEW8_cEYec0kw

清华张敏教授：个性化推荐研究进展（可解释性、鲁棒性和公平性）

https://zhuanlan.zhihu.com/p/57056588

一图胜千言: 解读阿里的Deep Image CTR Model

https://mp.weixin.qq.com/s/98X2agJlZcCH6IdZqf9m0A

深度学习在美团配送ETA预估中的探索与实践

https://mp.weixin.qq.com/s/J0j9NwSNhxab6bXqBBzaUw

进击的下一代推荐系统：多目标学习如何让知乎用户互动率提升100%？

https://mp.weixin.qq.com/s/BInWMRt-fVcRbY8aCiPnpA

淘宝总知道你要什么？万字讲述智能内容生成实践

https://mp.weixin.qq.com/s/E6EH6aJjzTwN2UZf_4nwoA

从FFM到DeepFFM，推荐排序模型到底哪家强？

https://www.zhihu.com/question/20830906/

deep learning可以用来做推荐系统吗？

https://mp.weixin.qq.com/s/WZ5CYdinRvRrCuniLHok-Q

Embedding技术在深度学习CTR模型中的应用

https://mp.weixin.qq.com/s/MBVac45WpZy4c208zF3Ang

当你打开天猫的那一刻，推荐系统做了哪些工作？

https://mp.weixin.qq.com/s/V7QODQ2F9q8Q7lfuny8JPw

强化学习用于推荐系统，蚂蚁金服提出生成对抗用户模型

https://mp.weixin.qq.com/s/FevsysNqRhq6Njh0ILCveQ

推荐系统遇上深度学习，9篇阿里推荐论文汇总！

# 人脸检测/识别

## 人脸识别

人脸检测是从一张图片中，识别出人脸，这和通常的目标检测没有太大的差别。**而人脸识别，则是精确到具体的人。**

人脸识别通常的做法是：

1.使用人脸检测，得到人脸区域的图像。

2.提取人脸特征。一般采用CNN+FC+loss的结构。其中，CNN+FC用于提取特征，而loss仅用于训练阶段。在推理阶段，我们使用CNN+FC得到人脸的特征向量即可。

3.特征的对比。比较两个特征向量的相似度（可以使用LMS或者cos相似度）。超过阈值，即认为是同一张脸。

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

论文：

《Coarse-to-Fine Auto-Encoder Networks (CFAN) for Real-Time Face Alignment》

参考：

https://zhuanlan.zhihu.com/p/22451474

SeetaFace开源人脸识别引擎介绍

http://www.cnblogs.com/nenya33/p/6801045.html

CFAN

## Siamese network

https://blog.csdn.net/shenziheng1/article/details/81213668

Siamese Network（原理篇）

https://www.jianshu.com/p/92d7f6eaacf5

Siamese network孪生神经网络--一个简单神奇的结构

https://blog.csdn.net/sxf1061926959/article/details/54836696

Siamese Network理解

https://vra.github.io/2016/12/13/siamese-caffe/

Caffe中的Siamese网络

https://mp.weixin.qq.com/s/rPC542OcO8B4bjxn7JRFrw

深度学习网络只能有一个输入吗

https://mp.weixin.qq.com/s/GlS2VJdX7Y_nfBOEnUt2NQ

使用Siamese神经网络进行人脸识别

https://mp.weixin.qq.com/s/lDlijjIUGmzNzcP89IzJnw

张志鹏:基于siamese网络的单目标跟踪

## S3FD

https://mp.weixin.qq.com/s/MyA8_yt4YCkFl67AyhpZow

尺度不变人脸检测器（S3FD-Single Shot Scale-invariant Face Detector）

https://github.com/sfzhang15/SFD

S3FD: Single Shot Scale-invariant Face Detector

## 人脸年龄识别

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

## 活体检测

https://mp.weixin.qq.com/s/zOnmKSnQctnyx7pKXX-tpQ

活体检测新文解读：利用多帧人脸来预测更精确的深度

https://mp.weixin.qq.com/s?__biz=MzU4MjQ3MDkwNA==&mid=2247486721&idx=1&sn=f0e5b2b0165e391c0d5adc4ce253f2f6

人脸识别中的活体检测算法综述

https://mp.weixin.qq.com/s/A1pbiU5PA9Owe69lGX9afw

活体识别告诉你为什么照片无法破解人脸系统

https://mp.weixin.qq.com/s/sPnoZyCkAhcCs_GtA79DrA

单目可见光静默活体检测Binary or Auxiliary Supervision论文解读

https://mp.weixin.qq.com/s/Vi2ypwO3uCD2lQZOwDFqTA

基于rPPG的人脸活体检测综述

## DeepFace

《DeepFace: Closing the Gap to Human-Level Performance in Face Verification》

DeepFace先进行了两次全卷积＋一次池化，提取了低层次的边缘／纹理等特征。

后接了3个Local-Conv层，这里是用Local-Conv的原因是，人脸在不同的区域存在不同的特征（眼睛／鼻子／嘴的分布位置相对固定），当不存在全局的局部特征分布时，Local-Conv更适合特征的提取。

## 人脸关键点

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

## 参考

https://github.com/ChanChiChoi/awesome-Face_Recognition

不止面部识别，一切关于人脸AI的资源都能在这里下载

https://github.com/ShownX/FacePaperCollection

人脸相关文献代码集锦：人脸检测、人脸识别、人脸生成等

https://mp.weixin.qq.com/s/eZ78biXN-mVw3s9Ky_LBZg

如何走近深度学习人脸识别？你需要这篇超长综述

https://mp.weixin.qq.com/s/tum24mzS2cUiKW87a1weFA

人脸识别全面总结：从传统方法到深度学习

https://mp.weixin.qq.com/s/bFnuGu4xLJiRfANi0Rkduw

人脸识别的前世今生：从人工特征的百花齐放到深度学习的一统江湖

https://zhuanlan.zhihu.com/p/32702868

人脸检测背景介绍和发展现状

# AI可解释性

XAI(Explainable Artificial Intelligence)

https://github.com/pbiecek/xai_resources

AI可解释性资源汇总

https://mp.weixin.qq.com/s/XVl6voP5cwdC7DcvTMQvVQ

机器学习可解释性工具箱XAI

https://github.com/jphall663/awesome-machine-learning-interpretability

最全的机器学习可解释性资料

https://mp.weixin.qq.com/s/OV4vXu7TAuyV7qU9BAMF6g

机器学习模型的“可解释性”到底有多重要？

https://mp.weixin.qq.com/s/33VQNVvb7JGlk10Jc3mmeg

从可视化到新模型：纵览深度学习的视觉可解释性

https://github.com/ModelOriented/DrWhy

可解释AI(XAI)工具集—DrWhy

# GAN参考资源

https://mp.weixin.qq.com/s/fmqaw0xm4Lm7h3rPsLD7yA

关于GAN的灵魂七问

https://mp.weixin.qq.com/s/CwB7IDaCZBzcQV8IBsYQDw

最新《生成式对抗网络GAN进展》论文

https://mp.weixin.qq.com/s/-SVtuZGOn6Wk9kwe50JJqw

《GAN实战：生成对抗网络深度学习》牛津大学Jakub著作

https://mp.weixin.qq.com/s/QqQ0SoF-kFTQRLxo4Tzm2g

反思基于能量的生成式模型：中山大学研究者从粒子演化角度改进经典的FRAME

https://mp.weixin.qq.com/s/ExsDftIvi8VUdvxfka-1ow

南大和中大“合体”拯救手残党：基于GAN的PI-REC重构网络，“老婆”画作有救了

https://mp.weixin.qq.com/s/RhYVCO0jNlY97ptIUvRRQQ

带你读论文：生成对抗网络GAN论文TOP 10

https://mp.weixin.qq.com/s/jByvPPrlN7zIOPXON4fZJw

生成对抗网络也需要注意力机制
