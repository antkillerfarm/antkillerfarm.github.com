---
layout: post
title:  深度学习（三十七）——人脸识别, 迁移学习
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

# 迁移学习

>龙明盛，清华本科（2008）+博士（2016），迁移学习的最早提出者之一。清华大学副教授，博导。   
>个人主页：   
>http://ise.thss.tsinghua.edu.cn/~mlong/

https://mp.weixin.qq.com/s/HmkTkv7QT08lGtJsHD7EvQ

迁移学习（Transfer Learning）技术概述

https://zhuanlan.zhihu.com/wjdml

《小王爱迁移》系列blog

https://mp.weixin.qq.com/s/BWBrso7O1O3Rfxa4QWZH4g

分分钟学会基于深度学习的图像真实风格迁移！

https://mp.weixin.qq.com/s/5DtTgc9bIrdXQkmuqRm8CA

谷歌大脑迁移学习：减少调参，直接在数据集中学习最佳图像架构

https://mp.weixin.qq.com/s/fEKc6yFZwTPAHjXJlcHA-w

香港科技大学提出L2T框架：学习如何迁移学习

https://mp.weixin.qq.com/s/pbyByPoZ9SVoP9B7pJMxXg

深度卷积网络迁移学习的脸部表情识别

https://www.zhihu.com/question/50996014

什么是One/zero-shot learning？

https://mp.weixin.qq.com/s/SZlFgnUBL0T6yNa-i_WLvg

领域适应性Domain Adaptation、One-shot/zero-shot Learning概述

https://mp.weixin.qq.com/s/sAf2fLLnKHOs433pV_6bSQ

One-shot Learning：孪生网络少样本精准分类

https://mp.weixin.qq.com/s/J8ZmIVKd-4X3hMGGIJWoDQ

一文看懂迁移学习：从基础概念到技术研究！

https://mp.weixin.qq.com/s/qYoTgqwjaUlEycuk9LlonA

迁移学习：6张图像vs13000张图像，超越2013 Kaggle猫狗识别竞赛领先水平

http://mp.weixin.qq.com/s/6Urv6TfUfc-BWV1YqTM1PQ

迁移学习+BPE，改进低资源语言的神经翻译结果

https://zhuanlan.zhihu.com/p/30242073

人脸识别中的迁移学习简介（Transfer Learning）

https://mp.weixin.qq.com/s/rVYWV-LsbmA4QhC6207SWA

14篇论文为你呈现“迁移学习”研究全貌

https://mp.weixin.qq.com/s/l-l1xbUaPNKc-w5XndjCbQ

通过网络结构迁移学习提高图像识别任务的拓展性

https://mp.weixin.qq.com/s/-KssC3yXsG3ZuV8-I6D_nQ

学习迁移架构用于Scalable图像的识别

https://mp.weixin.qq.com/s/NQED6DdCJNpNyzURUOZPnA

迁移学习：机器学习的下一个前沿阵地！

https://mp.weixin.qq.com/s/Hok9D8dAzYrBz7XoFmGE2A

AliExpress：在检索式问答系统中应用迁移学习

https://mp.weixin.qq.com/s/f_vB2AXCytnvoZaqfMeIpw

应用TF-Slim快速实现迁移学习

https://mp.weixin.qq.com/s/R1bKmhADfhQAZmhXL9ObiQ

多重预训练视觉模型的迁移学习

https://mp.weixin.qq.com/s/pDK4qBWArtETARE1fjbbmA

迁移学习在深度学习中的应用

https://mp.weixin.qq.com/s/mB1AEFVdM_s1rk0irST4Ww

迁移学习在图像分类中的简单应用策略

https://mp.weixin.qq.com/s/PDyp_GO0ovWV0KoGTwp_gQ

简述迁移学习在深度学习中的应用

https://mp.weixin.qq.com/s/FHmijTVqQ26osp6PzZsbvQ

付彦伟：零样本、小样本以及开集条件下的社交媒体分析

https://mp.weixin.qq.com/s/109hJaWsL4mcr9dc9vdDMg

结合主动学习与迁移学习：让医学图像标注工作量减少一半

https://mp.weixin.qq.com/s/A7PAu6-B1JRUfGmb2Fm_vA

中国科学院大学Oral论文：使用鉴别性特征实现零样本识别

https://mp.weixin.qq.com/s/P4kGtXZRW_fMbKbkGIJEfA

迁移学习比赛：OpenAI喊你重温“音速小子索尼克”

https://mp.weixin.qq.com/s/e4iv6FJAMZACEqWFmxeSFg

让AI掌握星际争霸微操：中科院提出强化学习+课程迁移学习方法

http://chenrudan.github.io/blog/2017/12/15/domainadaptation1.html

迁移学习之Domain Adaptation

https://mp.weixin.qq.com/s/ovSltv5Ct8AsPcxCFsYwYw

用于部分迁移学习的加权对抗网络

https://mp.weixin.qq.com/s/3H1u2JJ0JwfjwDKs5imcxQ

零资源机器翻译的最新进展

https://mp.weixin.qq.com/s/5DkqsOkTo9l9QtNp16MGOg

北京大学计算机研究所提出深度跨媒体知识迁移方法

https://mp.weixin.qq.com/s/ZvBzU_PSTCTS4hWrkJ2ZFg

zero-shot视觉模仿系统GSP，仅观察演示就学会执行任务

https://mp.weixin.qq.com/s/kTiQdoCI-mk8c-m0uL5BkQ

如何实现少样本学习？先让神经网络get视觉比较能力

