---
layout: post
title:  深度学习（三十七）——人脸识别, Graph NN
category: DL 
---

# 人脸识别

## Cascade CNN

《A Convolutional Neural Network Cascade for Face Detection》

http://blog.csdn.net/shuzfan/article/details/50358809

人脸检测——CascadeCNN

## MTCNN

http://blog.csdn.net/qq_14845119/article/details/52680940

MTCNN（Multi-task convolutional neural networks）人脸对齐

http://blog.csdn.net/shuzfan/article/details/52668935

人脸检测——MTCNN

https://mp.weixin.qq.com/s/IrZEQ69RNUdcs0Fl8fHmmQ

如何应用MTCNN和FaceNet模型实现人脸检测及识别

## 人脸年龄识别

https://www.openu.ac.il/home/hassner/projects/cnn_agegender/

Age and Gender Classification using Convolutional Neural Networks

## DeepFace

《DeepFace: Closing the Gap to Human-Level Performance in Face Verification》

DeepFace先进行了两次全卷积＋一次池化，提取了低层次的边缘／纹理等特征。

后接了3个Local-Conv层，这里是用Local-Conv的原因是，人脸在不同的区域存在不同的特征（眼睛／鼻子／嘴的分布位置相对固定），当不存在全局的局部特征分布时，Local-Conv更适合特征的提取。

## 参考

https://mp.weixin.qq.com/s/eZ78biXN-mVw3s9Ky_LBZg

如何走近深度学习人脸识别？你需要这篇超长综述

http://www.cnblogs.com/pandaroll/p/6590339.html

开源人脸识别openface

http://mp.weixin.qq.com/s/KQxGQdLa3XzKVIFYqlrV7g

人脸检测与识别的趋势和分析

https://zhuanlan.zhihu.com/p/25335957

人脸检测与深度学习

http://www.leiphone.com/news/201608/MPXlWtGaJLPYL7NB.html

人脸检测发展：从VJ到深度学习

https://zhuanlan.zhihu.com/p/22591740

从事人脸识别研究必读的N篇文章

https://mp.weixin.qq.com/s/ZZmLbFzi843g0k3gTncQmA

拿下人脸识别“世界杯”冠军！松下-NUS和美国东北大学实战分享

https://mp.weixin.qq.com/s/DYCXef_09yFFNR0uHL2Q0Q

基于Python的开源人脸识别库：离线识别率高达99.38%

https://mp.weixin.qq.com/s/eOxd5XbeyVcRYyAAmPr20Q

利用空间融合卷积神经网络通过面部关键点进行伪装人脸识别

https://mp.weixin.qq.com/s/vAjWHpn5HP_lwSv49g71SA

新型半参数变分自动编码器DeepCoder：可分层级编码人脸动作

https://mp.weixin.qq.com/s/IS5iAPZeUrvvyWs29O8Ukg

通过提取神经元知识实现人脸模型压缩

https://mp.weixin.qq.com/s/DEJ0z2CahZIrhTE3VXNVvg

基于注意力机制学习的人脸幻构

https://mp.weixin.qq.com/s/-G94Mj-8972i2HtEcIZDpA

人脸识别世界杯榜单出炉，微软百万名人识别竞赛冠军分享

https://mp.weixin.qq.com/s/bqWle_188lhYO4hpCfafkQ

用浏览器做人脸检测，竟然这么简单？

https://mp.weixin.qq.com/s/kn9JS55wIW2cfpUv7Jm0eQ

深度学习教你如何“以貌取人”！

https://mp.weixin.qq.com/s/3xEDtMoe0iRQSZiN5A1FGw

IPHONE X“刷脸”技术奥秘大揭底

https://mp.weixin.qq.com/s/s5HL6y2P9_KqpSAQg08URw

世界最大人脸对齐数据集ICCV 2017：距离解决人脸对齐已不远

https://mp.weixin.qq.com/s/Z06oUe7oExgkKZ8g2_PnPw

科普人脸表情识别技术

https://mp.weixin.qq.com/s/tS_9gS2ADoEdSKCLB-cxpA

刘小明教授为你讲解人脸识别

https://mp.weixin.qq.com/s/CLgyaAslE4hYHi62UhzeGw

韩琥：深度学习让机器给人脸“贴标签”

https://mp.weixin.qq.com/s/pUcVO8Fj-n9-pMwAzBvWuQ

山世光：人脸识别领域的“激荡20年”

https://zhuanlan.zhihu.com/p/23340343

Center Loss及其在人脸识别中的应用

https://mp.weixin.qq.com/s/7AnF0uMgepchiUeqfqVbCg

清华大学王生进：新智能安防：人脸识别技术与应用系统

https://mp.weixin.qq.com/s/-T5k2ViPjvEoXccKt-_J3Q

中科院自动化研究所提出FaceBoxes：实时、高准确率的CPU面部检测器

https://www.leiphone.com/news/201707/mFuwXGvZBhoVQD5S.html

一秒分辨出杨臣刚、王大治和孙楠，这个黑产居然用AI来"打码"

https://mp.weixin.qq.com/s/PF7_kSnwngnJ1jeh7ebyww

手把手教你用1行代码实现人脸识别

https://mp.weixin.qq.com/s/Gqlo4wU0wtIcJDvQs2kTAw

为了让你分清人脸识别与人脸检测，苹果要亲自给你科普

http://blog.csdn.net/gitchat/article/details/78546894

TensorFlow人脸识别网络与对抗网络搭建

https://mp.weixin.qq.com/s/ZJmgC8xTruaRLfCocodjqA

人脸检测与识别总结

https://mp.weixin.qq.com/s/lGIkLcglQGvDzdBQmZW0Qw

判别特征学习方法用于人脸识别

https://mp.weixin.qq.com/s/t-vFSkQ7SYlTGGxEEY1mSg

近期人脸对齐的实证性研究

https://mp.weixin.qq.com/s/zhIyZ9MEFXAuIVUxAGkW-w

人脸对齐之GBDT(ERT)算法解读

https://mp.weixin.qq.com/s/1g9PXc_3nhKMf1_-E_cVAA

图像识别的原理、过程、应用前景，精华篇！

https://mp.weixin.qq.com/s/CvdeV5xgUF0kStJQdRst0w

从传统方法到深度学习，人脸关键点检测方法综述

https://mp.weixin.qq.com/s/YRsVi09u3W0aQMdsR5KY4Q

腾讯AI Lab提出Face R-FCN与Face CNN，刷新人脸检测与识别两大测评记录

https://mp.weixin.qq.com/s/A1pbiU5PA9Owe69lGX9afw

活体识别告诉你为什么照片无法破解人脸系统

https://mp.weixin.qq.com/s/zyMIRGig-m732rvraPKxwA

单样本学习：使用孪生神经网络进行人脸识别

https://mp.weixin.qq.com/s/QJm7YoCYmiF0dX8uac5w4Q

旷视研究院：被遮挡人脸区域检测的技术细节

https://mp.weixin.qq.com/s/Fmi9RJz-bMOYBoZWt1nWag

人脸注意机制网络

https://mp.weixin.qq.com/s/s9H_OXX-CCakrTAQUFDm8g

申省梅颜水成团队获国际非受限人脸识别竞赛IJB-A冠军，主要负责人熊霖技术分享

https://mp.weixin.qq.com/s/ZFFSTFDVxFUe2KOFy8XDxw

人脸识别——新的一个境界（无约束）

https://mp.weixin.qq.com/s/GlS2VJdX7Y_nfBOEnUt2NQ

使用Siamese神经网络进行人脸识别

https://mp.weixin.qq.com/s/RSCrkeIToeNKrFvMITxzDg

通过OpenFace来理解人脸识别

https://zhuanlan.zhihu.com/p/34404607

人脸识别的LOSS（上）

https://mp.weixin.qq.com/s/ZrnAqDJCLtMy_qTQ2RZT0A

级联MobileNet-V2实现人脸关键点检测

https://mp.weixin.qq.com/s/xDEga2tITO8rVkvXCZ62sg

中国团以98%精度夺得MegaFace人脸识别冠军

https://mp.weixin.qq.com/s/HVooLtr_k6fwh2N3GjMb1A

新研究提出深度残差等价映射：由正脸加强侧脸识别效果

https://mp.weixin.qq.com/s/9noWOZJSRAi424ZDTD1IRQ

世界权威评测冠军：百度人脸检测算法PyramidBox

https://www.zhihu.com/question/37060782

人脸识别哪家强？不如问哪家公司吹牛逼强

https://zhuanlan.zhihu.com/p/22451474

SeetaFace开源人脸识别引擎介绍

https://mp.weixin.qq.com/s/MyA8_yt4YCkFl67AyhpZow

尺度不变人脸检测器（S3FD-Single Shot Scale-invariant Face Detector）

https://mp.weixin.qq.com/s/b-v_hDHJjUxMithBDr3TcQ

实时旋转鲁棒人脸检测算法

https://blog.csdn.net/xiamentingtao/article/details/50908190

Facial Landmark Detection(人脸特征点检测)

https://mp.weixin.qq.com/s/i4HdS-lCrsv9YR39Hja8ow

深度人脸表情识别技术综述，没有比这更全的了

https://mp.weixin.qq.com/s/S_T0tYhZ1pjoIMysP0aVWA

美军AI黑科技：黑暗中也能准确识别人脸，谁该为此感到紧张？

https://mp.weixin.qq.com/s/GLKvzC_o6MR1ixThAVc9lQ

CosFace: 面向深度人脸识别的增强边缘余弦损失函数设计

https://mp.weixin.qq.com/s/AMDkkbdTQbL-2jyweVid-A

摆好Pose却没管理好面部表情？腾讯优图Facelet-Bank人脸处理技术了解一下

https://mp.weixin.qq.com/s/OjId_YfxkhEh4tJ1Sw-Hbw

多伦多大学反人脸识别，身份欺骗成功率达99.5%

https://mp.weixin.qq.com/s/tUSNk5R_zbEFz-yIx0LXYQ

基于DNN的人脸识别中的反欺骗机制

https://mp.weixin.qq.com/s/mjdW7xY77H03RIKuKuFmQg

人脸画像合成研究的综述与对比分析

https://mp.weixin.qq.com/s/Ieha-lJ_KuEnpbJha_nxJw

利用人脸图片准确识别年龄：上海大学研究者提出“深度回归森林”

## Graph NN

https://mp.weixin.qq.com/s/_aydey5ZVwrObmoFXXIYcw

Bengio等人提出图注意网络架构GAT，可处理复杂结构图

https://zhuanlan.zhihu.com/p/34232818

《Graph Attention Networks》阅读笔记

https://zhuanlan.zhihu.com/p/28170197

《Gated Graph Sequence Neural Networks》阅读笔记

https://mp.weixin.qq.com/s/Pm1HiEQOBnbo_GQ_v6Y_zw

腾讯提出自适应图卷积神经网络，接受不同图结构和规模的数据

https://mp.weixin.qq.com/s/bMpugd2Lp35VPr8fQAPzsg

一文概览图卷积网络基本结构和最新进展

https://zhuanlan.zhihu.com/p/31067515

《Semi-Supervised Classification with Graph Convolutional Networks》阅读笔记

https://mp.weixin.qq.com/s/6viSk0Ts_7eTfYrWYi_HDQ

基于图结构的实体和关系联合抽取模型简介

https://mp.weixin.qq.com/s/w5ldyp00CqkX8Kp-8Aw0nQ

图深度学习(GraphDL)，下一个人工智能算法热点？一文了解最新GDL相关文章

https://mp.weixin.qq.com/s/Jt6CjMqNFEXWoL5pkLeVyw

洛桑理工：Graph上的深度学习报告

https://zhuanlan.zhihu.com/p/36117802

《Learn to Represent Programs with Graphs》阅读笔记。这篇论文讲述了DL在程序代码纠错方面的应用。

https://zhuanlan.zhihu.com/p/37278426

Graph2Seq: Graph to Sequence Learning with Attention-based Neural Networks

https://mp.weixin.qq.com/s/iQYVyo2PHuGbEsYgdIf_oQ

DeepMind等机构提出“图网络”：面向关系推理

https://mp.weixin.qq.com/s/TAccHagxXQ82lfE91Y6xWg

CNN已老，GNN来了：重磅论文讲述深度学习的因果推理

https://mp.weixin.qq.com/s/UONtTJJgDawRPWtatAVKkg

如何利用高效的搜索算法来搜索网络的拓扑结构

