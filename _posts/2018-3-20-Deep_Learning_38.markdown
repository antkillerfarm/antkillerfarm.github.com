---
layout: post
title:  深度学习（三十八）——RNN进阶, 显著性检测
category: DL 
---

* toc
{:toc}

# RNN进阶

## IndRNN

https://mp.weixin.qq.com/s/cAqpclkkeVrTiifz07HC1g

新型循环神经网络IndRNN：可构建更长更深的RNN

https://mp.weixin.qq.com/s/7-K-nZTijoYCaprRNYXxFg

新型RNN：将层内神经元相互独立以提高长程记忆

## ODE

https://mp.weixin.qq.com/s/4CrPGKnR7RLN-2ROG5X4uw

ODE网络：一场颠覆RNN的革命即将到来

https://mp.weixin.qq.com/s/0vju0Q_DcIWwdEo9EEE3iQ

NeurIPS18最佳论文NeuralODE，现在有了TensorFlow实现

https://mp.weixin.qq.com/s/i6VEYjbac4QP3s51meN1VA

Hinton向量学院推出神经ODE：超越ResNet 4大性能优势

https://mp.weixin.qq.com/s/ZEIsyV-0aTvYn6K8GyANPA

硬核NeruIPS 2018最佳论文，一个神经了的常微分方程

https://mp.weixin.qq.com/s/uAiRTeYkZKy9q3d9v3dR5A

神经微分方程--钢琴和小提琴

## 参考

https://mp.weixin.qq.com/s/0TLaC8ACXAFEK5aMNK9O-Q

简单循环单元SRU：像CNN一样快速训练RNN

https://zhuanlan.zhihu.com/p/27104240

CW-RNN收益率时间序列回归

https://mp.weixin.qq.com/s/SeR_zNZTu4t7kqB6ltNrmQ

从循环到卷积，探索序列建模的奥秘

https://mp.weixin.qq.com/s/_q69BV1r46S9X5wnLuFPSw

关于序列建模，是时候抛弃RNN和LSTM了

https://mp.weixin.qq.com/s/m5GRNp6qDfVfC0mkQ4m4Yw

神经语言模型如何利用上下文信息：长距离上下文的词序并不重要

https://mp.weixin.qq.com/s/kuoUnt2Vhz9NhfnNqMFAhQ

DeepMind提出关系RNN：构建关系推理模块，强化学习利器

https://mp.weixin.qq.com/s/wfOzCxe3L2t11VguYLGC9Q

上海交大搞出SRNN，比普通RNN也就快135倍

https://mp.weixin.qq.com/s/f0sv7c-H5o5L_wy2sUonUQ

CNN取代RNN？当序列建模不再需要循环网络

https://mp.weixin.qq.com/s/h3fF6Zvr1rSzSMpqdu8B0A

电子科大提出BT-RNN：替代全连接操作而大幅度提升LSTM效率

https://mp.weixin.qq.com/s/OgN4rVDKH5WABIaRY7CHog

如何让RNN神经元拥有基础通用的注意力能力

https://mp.weixin.qq.com/s/KBLCrupGIuPa5nVrxcS5WQ

新研究将GRU简化成单门架构，或更适用于语音识别

https://mp.weixin.qq.com/s/kQozftKd_n_kYIF7KKCc8g

短视频那么多，快手如何利用GRU实现各种炫酷的语音应用

https://mp.weixin.qq.com/s/xwuM2Vj8G7UyuEyzTyO13A

将CNN与RNN组合使用，天才还是错乱？

https://mp.weixin.qq.com/s/c7XkzjLH1n5EtqdQik618g

Dropout在RNN中的应用综述

https://mp.weixin.qq.com/s/K6LK47_GCTeZJPAW0-Xp4Q

多伦多大学提出可逆RNN：内存大降，性能不减！

https://mp.weixin.qq.com/s/lvaWx7J4HFTvYxy7-B9vYg

周志华等提出RNN可解释性方法，看看RNN内部都干了些什么

https://mp.weixin.qq.com/s/YbdiEHb8ld1pp1ehgBzTOQ

将未来信息作为正则项，Twin Networks加强RNN对长期依赖的建模能力

https://mp.weixin.qq.com/s/ty8RyPREo_EA7O8vA2pQuQ

AI编曲震撼人心，RNN生成流行音乐

https://mp.weixin.qq.com/s/vIL-bKHZK-6eXZYWxrc9vw

这种有序神经元，像你熟知的循环神经网络吗？

https://mp.weixin.qq.com/s/GGK9T0DeyIdD5ahHy5uvfg

LightRNN：存储和计算高效的RNN

https://mp.weixin.qq.com/s/JGZpKSF5HPCMCD061jwq9A

Bengio等人提出新型循环架构，大幅提升模型泛化性能

https://mp.weixin.qq.com/s/GN0m5nWuV6VDYsTk0XLoDA

pytorch中如何处理RNN输入变长序列padding

https://mp.weixin.qq.com/s/bts9mdIrGIjO8UCUxSV-xg

Transformer的潜在竞争对手QRNN论文解读，训练更快的RNN

# 显著性检测

视觉显著性检测(Visual Saliency Detection)指通过智能算法模拟人的视觉特点，提取图像中的显著区域(即人类感兴趣的区域)。

https://blog.csdn.net/dawnlooo

一个显著性检测的专栏

https://mp.weixin.qq.com/s/Mi62oqtXUT5If_Dj4KmVYA

计算机视觉如何知道你想看什么？个人显著性检测

https://mp.weixin.qq.com/s/47TcGoasB9E_Et2zwl3OCw

全局对比度的图像显著性检测算法

https://zhuanlan.zhihu.com/p/65307842

快、好、实现简单并且开源的显著性检测方法

https://mp.weixin.qq.com/s/tmp1HXU7cLerLr0DY9NluQ

杂乱环境下的显著性物体： 将显著性物体检测推向新高度

https://mp.weixin.qq.com/s/urgkUcu2ZWQMGPZdArWzYg

PoolNet：基于池化技术的显著性目标检测

https://zhuanlan.zhihu.com/p/71538356

BASNet，一种能关注边缘的显著性检测算法

https://mp.weixin.qq.com/s/ntSH2aS4YHqrLaTAfWFLsQ

可选择性与不变性：关注边界的显著性目标检测

https://mp.weixin.qq.com/s/0T1QhiT_20BrerNcTjKreQ

南开提出边缘引导的显著目标检测算法EGNet，刷新主流数据集所有评价指标

https://mp.weixin.qq.com/s/p4lHnte3FYu6XtD3PnSeKw

光场显著性检测研究综述

https://mp.weixin.qq.com/s/8QrNvb-1zmrTWo5zThpyvg

U²-Net：使用显著性物体检测来生成真实的铅笔肖像画

# 人脸检测/识别+

https://mp.weixin.qq.com/s/p0Y3Svrk1WWTed6R4AFWLQ

异质人脸识别研究综述

https://mp.weixin.qq.com/s/yoCVFWlqReUHx1Wmo0-rtw

强大的姿势感知模型用于姿势不变的人脸识别

https://mp.weixin.qq.com/s/MEB-qLGiOKcvdee-vlsjYw

改进的阴影抑制用于光照鲁棒的人脸识别

https://mp.weixin.qq.com/s/BE0bBFNVOoKjXLUH5vr5Kg

在警察领域高级人脸识别技术的一致性

https://mp.weixin.qq.com/s/fyx_P_M_CyuAshdc9lCAcg

鲁棒异构判别分析的单样本人脸识别

https://mp.weixin.qq.com/s/lhut5ndqZWTwlKZjeY484w

漫画人脸检测：全局和局部信息融合的深度神经网络

https://mp.weixin.qq.com/s/7CV0a-UshPtadgfcsFScJw

用孪生网络、对比损失和三重损失进行人脸识别的单样本学习

https://mp.weixin.qq.com/s/UUDmWMaFEkW9qbMaJkMhhA

低光照人脸检测竞赛冠军代码与PPT分享

https://mp.weixin.qq.com/s/4J_pPkNoE9eSDQK4NVs0sw

看一眼就知道你的BMI：基于Keras与迁移学习的人脸预测系统

https://mp.weixin.qq.com/s/CwMGZUWaopT3HAIXmlowsA

目前最强开源人脸检测算法RetinaFace

https://mp.weixin.qq.com/s/ViX3kAT96aaLZ0r9SNtSWA

强判别能力的深度人脸识别

https://zhuanlan.zhihu.com/p/55479744

人脸检测与对齐python实现

https://mp.weixin.qq.com/s/QpOl26RzsKELGctKDskS4w

2K图像90FPS，中科院开源轻量级通用人脸检测器

https://mp.weixin.qq.com/s/0JX-zEQwkC_0Iq5ZBU6xsg

级联卷积神经网络用于人脸检测

https://mp.weixin.qq.com/s/09iDezt4pJ-7oO77juB2GQ

快速准确的人脸检测、识别和验证新框架（DPSSD）

https://mp.weixin.qq.com/s/45R7yxjPbm0wO4EW7RzXnQ

LFFD：轻量级人脸检测器，不止是快

https://mp.weixin.qq.com/s/pT2XgyvFeV2P3k5coN6ztA

局部人脸识别的动态特征匹配

https://mp.weixin.qq.com/s/GNYrR0NOaN42SgLXsm2KIg

三维"ZAO"脸，单张图片估计人脸几何，效果堪比真实皮肤

https://mp.weixin.qq.com/s/-1W4SqEX_xxxdBFMKJJ6Xg

腾讯（优图）新技术的人脸检测

https://zhuanlan.zhihu.com/p/81353261

CVPR2019_Group Sampling

https://mp.weixin.qq.com/s/QZvGiri0x5UetBp0ZoDHDQ

目前最强判别能力的深度人脸识别

https://mp.weixin.qq.com/s/zSawuk6vn2D6oM5d1pJyWw

选择性细化网络用于高性能人脸检测

https://mp.weixin.qq.com/s/RY26yEIfX4NwSlL65_6RTQ

人脸检测小江湖

https://mp.weixin.qq.com/s/bAf5x1OOf8xqcQyMdTlWyg

强判别能力的深度人脸识别

https://mp.weixin.qq.com/s/zkdD0BDkB5W_j5N0emW53Q

基于生成对抗网络（GAN）的人脸变形

https://mp.weixin.qq.com/s/tj-ZdB83itO1nAKBGKlb_Q

爱奇艺提出半监督损失函数，利用无标签数据优化人脸识别模型

https://mp.weixin.qq.com/s/L1H7PXwuItuF6Vp2Uyfowg

人脸识别剩下的难题：从遮挡，年龄，姿态，妆造到亲属关系，人脸攻击

https://mp.weixin.qq.com/s/pmnb_WnncL12T1IZoso5DA

DeepID

https://mp.weixin.qq.com/s/kH3-WUX4rc2SaLJcGzVLcQ

如何检测极小人脸？试试超分辨率

https://github.com/ShiqiYu/libfacedetection

libfacedetection算法开源

https://mp.weixin.qq.com/s/DkvQ86tgU4DarsmY_qTLdg

最快人脸检测遇敌手！ZQCNN vs libfacedetection

https://mp.weixin.qq.com/s/PsToV8Z17tRzXW_AbaMNmg

Blazeface人脸检测器

https://mp.weixin.qq.com/s/5ANMZNaR2ayYMUQEJ6aM3w

人脸识别基于通用表示学习

https://mp.weixin.qq.com/s/jrynQZgoljFi6zdVKWo5nw

在浏览器中实现AR试妆

https://mp.weixin.qq.com/s/lr9OVNiIN0x7k4cmNZoCUw

用于人脸检测的SSH算法

https://mp.weixin.qq.com/s/A5-Z6APwS1iFCUgYxtYNyA

重磅！GroupFace人脸识别，刷新9个数据集SOTA

https://mp.weixin.qq.com/s/-VknGwKKY14cT-lZ7sC43A

人脸识别--基于深度学习以人类为中心的图像理解

https://zhuanlan.zhihu.com/p/69542283

人脸防伪检测挑战赛-俄初创公司夺冠,中美企业位列二三

https://mp.weixin.qq.com/s/ROJtk8MSTlHJe-42-f1OYw

部分遮挡下的人脸识别技术

https://mp.weixin.qq.com/s/VOclUEBXnAtmAZmZrzQQoA

面向大规模无标注视频的人脸对齐方法

https://zhuanlan.zhihu.com/p/112140909

基于深度学习的人脸识别方法介绍

https://mp.weixin.qq.com/s/CR4Nf_RklcXA45mswPv8Zw

腾讯优图开源人脸检测算法DSFD，刷新两项数据集纪录

https://mp.weixin.qq.com/s/B8r2YEd9UbKMTT8Tup2Y_w

腾讯（优图）新技术的人脸检测（DSFD）

https://mp.weixin.qq.com/s/iI-q0s8aSojeDFbasiFVKw

人脸检测器：对DSFD的理解

https://zhuanlan.zhihu.com/p/62954487

旷视研究院新出8000点人脸关键点，堪比电影级表情捕捉

https://mp.weixin.qq.com/s/bAlaCWg4OEprpoZqQFQg1w

百度提出PyramidBox人脸检测算法

https://mp.weixin.qq.com/s/CYZvFb7kryE-8HV3teGRzA

有效遮挡检测的鲁棒人脸识别

https://mp.weixin.qq.com/s/XTIl505glmfCItuiwuPLwg

人脸聚类——Linkage Based Face Clustering via GCN

https://mp.weixin.qq.com/s/fpnq3rkcmckip30a4SlysQ

三维人脸识别研究进展综述

https://mp.weixin.qq.com/s/QEUiq5CbASd4eAM6eDPa6A

换脸算法和人脸驱动都有哪些核心技术，如何对其长期深入学习

https://mp.weixin.qq.com/s/ROTKi-Ycm0ypIi0FhCW4Xw

视频人脸识别进展综述

https://mp.weixin.qq.com/s/9d6EhZEdn6XY-wFC22LysQ

最新“深度学习人脸检测”综述论文，17页pdf概述50种人脸检测SOTA方法

https://mp.weixin.qq.com/s/kEZYpPnqJiiLxqH-CIK-Zw

一文读懂3D人脸识别十年发展及未来趋势
