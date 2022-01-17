---
layout: post
title:  深度学习（十八）——无监督/半监督/自监督深度学习（2）, Regularization
category: DL 
---

* toc
{:toc}

# 无监督/半监督/自监督深度学习（续）

https://zhuanlan.zhihu.com/p/30265894

自监督学习近期进展

https://mp.weixin.qq.com/s/qyQjKsgktWv9SihotaQX3w

从顶会看自监督学习最新研究进展

https://mp.weixin.qq.com/s/cTlXMxcpzc7_5NVsTm1jcA

学习一帧，为整段黑白视频上色：谷歌提出自监督视觉追踪模型

https://mp.weixin.qq.com/s/Amr34SdrPZho1GQpFS7WBA

见微知著：语义分割中的弱监督学习

https://mp.weixin.qq.com/s/zOWA1oKbopZJuYIAYYlKTA

港中文-商汤联合论文：自监督语义分割的混合与匹配调节

https://mp.weixin.qq.com/s/5xlSoC5sgzsAwMYMSFCjnw

TextTopicNet:CMU开源无标注高精度自监督模型

https://mp.weixin.qq.com/s/343DfjOvkaozuxNK89V3zQ

前景目标检测的无监督学习

https://mp.weixin.qq.com/s/DwY0oGu-G30Szs-ArI5WaQ

程明明：面向弱监督的图像理解

https://mp.weixin.qq.com/s/LFOljv-Hr6JqyI6TQ2X4sw

半监督学习也能自动化？南大和第四范式提出Auto-SSL

https://mp.weixin.qq.com/s/83xAXrc_H_OExW3vii08hA

谷歌提出新方法：基于单目视频的无监督深度学习结构化

https://mp.weixin.qq.com/s/gr0_p4WFToTrDfy47h-p0A

基于自监督学习的视听觉信息同一性判断

https://mp.weixin.qq.com/s/Dqz97_U5pw_4d9KFblJfLg

基于自编码器的表征学习：如何攻克半监督和无监督学习？

https://mp.weixin.qq.com/s/LaIvAuBHYGNMug3NZ1pLhQ

半监督深度学习小结：类协同训练和一致性正则化

https://mp.weixin.qq.com/s/aBDgV7u93MAv2MogZKBmvw

Google提出Grasp2Vec模型：利用自监督方法学习物体表示

https://mp.weixin.qq.com/s/YfDZMEkOnxp0_ei2Oam-YQ

基于弱监督的视频时序动作检测的介绍

https://mp.weixin.qq.com/s/RiL-s50oOI--PZyIOd2E0g

弱监督语义分割最新方法资源列表

https://mp.weixin.qq.com/s/USOWECXk_az4b6eTssfOBw

基于弱监督深度学习的图像分割方法综述

https://mp.weixin.qq.com/s/8oEdQOmSRrkIaTVQdhk2Dw

无监督领域特定单图像去模糊

https://mp.weixin.qq.com/s/FpIaa8XoJ9GsHxL-W1Cl5Q

斯坦福AI实验室机器学习编程新范式：弱监督

https://mp.weixin.qq.com/s/ys9iiiBL3iL2SJL247AMlA

多伦多大学&NVIDIA最新成果：图像标注速度提升10倍！

https://mp.weixin.qq.com/s/V6xiG931OUJyVx15QFb_mQ

弱监督视觉理解笔记

https://mp.weixin.qq.com/s/HopNSLS75TgE28LfY02qog

不同视角构造cycle-consistency，降低视频标注成本

https://mp.weixin.qq.com/s/XiLBHkraT8lJcOu2faqK5g

关于弱监督学习，这可能是目前最详尽的一篇科普文

https://mp.weixin.qq.com/s/VnOfYuHQQf_q92VHVE3mrQ

谷歌新发布的半监督学习算法降低4倍错误率

https://mp.weixin.qq.com/s/rOj_J1zNYf-Vj9tqLG5KOQ

超强半监督学习MixMatch

https://zhuanlan.zhihu.com/p/66389797

虚拟对抗训练（VAT）：一种新颖的半监督学习正则化方法

https://mp.weixin.qq.com/s/DAtHXSfCpqCAZ0iVsfWkDA

半监督学习理论及其研究进展概述

https://mp.weixin.qq.com/s/eHzNIO-RSY-uf-K-OwtWfw

集多种半监督学习范式为一体，谷歌新研究提出新型半监督方法MixMatch

https://mp.weixin.qq.com/s/3el7bPAeJrTQGfWW29ewuA

新技术“红”不过十年，半监督学习为什么是个例外？

https://mp.weixin.qq.com/s/alnji5kgTxc34O7k78uGiA

无监督学习中的目标检测

https://mp.weixin.qq.com/s/8FtDhpgc-1j3TSL771N-Ng

无标注数据是鸡肋还是宝藏？阿里工程师这样用它

https://mp.weixin.qq.com/s/LdfLd2cZCdpvNYLKHUNwuA

简述无监督图像分类发展现状

https://mp.weixin.qq.com/s/qaLQK3uzaeyp68AbL0aOOQ

怎么在视频标注上省钱？这里有一个面向视频推荐的多视图主动学习

https://mp.weixin.qq.com/s/-cXOUw9zJteVUkbpRMIWtQ

何恺明一作，刷新7项检测分割任务，无监督预训练完胜有监督

https://mp.weixin.qq.com/s/wtHrHFoT2E_HLHukPdJUig

OpenAI科学家一文详解自监督学习

https://mp.weixin.qq.com/s/fy1gUElWVWcOVvzv6fGmdg

谷歌大脑推出视觉领域任务自适应基准：VTAB

https://zhuanlan.zhihu.com/p/80815225

Image-Level弱监督图像语义分割汇总简析

https://mp.weixin.qq.com/s/5czWf0xpqva5pmuvJDn5AQ

Google研究院提出FixMatch，简单粗暴却极其有效的半监督学习方法，附14页PDF下载

https://zhuanlan.zhihu.com/p/108088719

SSL:Self-Supervised Learning(自监督学习)

https://zhuanlan.zhihu.com/p/108625273

Self-Supervised Learning入门介绍

https://zhuanlan.zhihu.com/p/108906502

Self-supervised Learning再次入门

https://mp.weixin.qq.com/s/VvUj0S2OTf8BowGRjDuVag

图解自监督学习，人工智能蛋糕中最大的一块

https://mp.weixin.qq.com/s/df51T24mBVycBeI_M7QqOQ

无标记数据学习, 83ppt

https://mp.weixin.qq.com/s/2FxD6ga6b_WOdAni16wd2Q

自监督学习在计算机视觉应用最新概述，108页ppt Self-supervised learning

https://mp.weixin.qq.com/s/3kwLoojFjJoPz4pUuEVA8g

神奇的自监督场景去遮挡

https://mp.weixin.qq.com/s/eROWWPQkUs91bcv4VsQqSA

NLP中的自监督表示学习，全是动图，很过瘾的

https://mp.weixin.qq.com/s/gXqB7JJyIEJa74McbYcrzg

只有正样本和无标记数据的半监督学习（PU Learning）

https://mp.weixin.qq.com/s/kGProJmrf43-2O48PMPM5g

正样本和无标签学习（PU Learning）：使用机器学习恢复数据的标签

https://mp.weixin.qq.com/s/vm1p3YceIC0nd191xsktfg

自监督学习的视觉语言建模，115页ppt讲述多模态预训练进展

https://mp.weixin.qq.com/s/PCXcvzwv8DF693_KzXK5bg

计算机视觉研究新方向：自监督表示学习总结

https://mp.weixin.qq.com/s/TOwOa3noN_UYrd5g0Nrrrg

半监督学习技术在金融文本分类上的实践

https://mp.weixin.qq.com/s/uh25WRHVsFpoKwFyTSZtIw

计算机视觉中的半监督学习

https://mp.weixin.qq.com/s/lweM2STVbldYEGwPcK1YEg

图像自标记的可视化指南

https://mp.weixin.qq.com/s/hLFPWiHmDIzeUlQjInbgGw

ActBERT: 自监督多模态视频文字学习

https://mp.weixin.qq.com/s/1hK3k6Mf3uTEXrqMFr1evA

Kaggle知识点：伪标签Pseudo Label

https://zhuanlan.zhihu.com/p/157325083

伪标签（Pseudo-Labelling）——锋利的匕首

https://mp.weixin.qq.com/s/qVGveKfCfNKqJoqwMbUVKg

长文总结半监督学习

https://mp.weixin.qq.com/s/LAnP5OMuJFDhsfJWRoVMFw

无监督领域迁移及文本表示学习的相关进展

https://mp.weixin.qq.com/s/Tau5jzNbBd0NketdgytvAg

计算机视觉中的自监督表示学习近期进展

https://mp.weixin.qq.com/s/uYmHxScroi4jB2okmqwHcA

半监督学习入门基础（一）

https://zhuanlan.zhihu.com/p/212873650

Contrastive Self-Supervised Learning

https://mp.weixin.qq.com/s/XwGvH0mTEf-jF5XQKk2lBw

电子科大最新《深度半监督学习》综述论文，24页pdf

https://zhuanlan.zhihu.com/p/355523266

从SimCLR到BarLow Twins，一文了解自监督学习不断打脸的认知发展史

https://mp.weixin.qq.com/s/WqUb9MY_3hVPRdxSl9BE1Q

S4L: 半监督+自监督学习

https://mp.weixin.qq.com/s/1f1Ma2ZQVTuPo38_uCE0fQ

大规模推荐系统的自监督学习

https://mp.weixin.qq.com/s/qgP39JKD3fbVNK8e4Hw4PQ

重邮高新波等最新《少样本目标检测算法》综述论文

## 对比学习

![](/images/img4/SimCLR.jpg)

https://mp.weixin.qq.com/s/r1uXn2jGsHZcZ8Nk7GnGFA

语义表征的无监督对比学习：一个新理论框架

https://zhuanlan.zhihu.com/p/346686467

对比学习（Contrastive Learning）综述

https://mp.weixin.qq.com/s/SOaA9XNnymLgGgJ5JNSdBg

对比学习（Contrastive Learning）相关进展梳理

https://mp.weixin.qq.com/s/U0pTQkW55evm94iQORwGeA

图解SimCLR框架，用对比学习得到一个好的视觉预训练模型

https://mp.weixin.qq.com/s/1RJ4bbfDC5LiN2PNIxdzew

SimCLR框架的理解和代码实现以及代码讲解

https://mp.weixin.qq.com/s/-Vtl_8nND7WCPLdL5bNlMw

探索孪生神经网络：请停止你的梯度传递

https://zhuanlan.zhihu.com/p/321642265

《探索简单孪生网络表示学习》阅读笔记

https://mp.weixin.qq.com/s/6qqFAQBaOFuXtaeRSmQgsQ

一文梳理2020年大热的对比学习模型

https://mp.weixin.qq.com/s/SeAZERYdfqDbtqTLnuWfGg

盘点近期大热对比学习模型：MoCo/SimCLR/BYOL/SimSiam

https://mp.weixin.qq.com/s/jHVg-BMRRVNjAf6ZFEoPxQ

自监督学习的SimCLRv2框架

https://mp.weixin.qq.com/s/7iBC_n6EARW3V8bNuKUqQA

Hinton团队力作：SimCLR系列

https://mp.weixin.qq.com/s/sH-G4g0EyQLu2l91Xvdefw

Neighbor2Neighbor：无需干净图像的自监督图像降噪

https://mp.weixin.qq.com/s/xYlCAUIue_z14Or4oyaCCg

对比学习研究进展精要

https://mp.weixin.qq.com/s/VlSoMmAGDblQ2UYhLD96gA

什么是contrastive learning？

https://mp.weixin.qq.com/s/qnG0YLf0yjs4aT9URRMDyw

有监督对比学习的一个简单的例子

https://mp.weixin.qq.com/s/h8loG3enT5U-5F2a2UflJg

对比学习小综述

https://mp.weixin.qq.com/s/v5p9QA3vDl-WTF3-7shp4g

对比学习简述

https://mp.weixin.qq.com/s/CeqoXqHjfa6UTWa8mmo_Ww

Paper和陈丹琦撞车是一种怎样的体验（ConSERT vs. SimCSE）

https://mp.weixin.qq.com/s/C4KaIXO9Lp8tlqhS3b0VCw

美团提出基于对比学习的文本表示模型（ConSERT）

# Regularization

DL中的Regularization除了常见的$$l_1$$-norm、$$l_2$$-norm和squared $$l_2$$-norm之外，还有Group Regularization。它的定义如下：

$$loss(W;x;y) = loss_D(W;x;y) + \lambda_R R(W) + \lambda_g \sum_{l=1}^{L} R_g(W_l^{(G)})$$

$$R_g(w^{(g)}) = \sum_{g=1}^{G} \lVert w^{(g)} \rVert_g = \sum_{g=1}^{G} \sum_{i=1}^{|w^{(g)}|} {(w_i^{(g)})}^2$$

Group Regularization也叫做Block Regularization或Structured Regularization。

# 何恺明

## MoCo

https://mp.weixin.qq.com/s/9zaTjwwGPHHzSv1ZmHf8_g

不妨试试MoCo，来替换ImageNet上pretrain模型！

https://mp.weixin.qq.com/s/GC6PGlweneYtYo7_SUr0Zw

MoCo V3：我并不是你想的那样！

https://mp.weixin.qq.com/s/sAYh3l2eab2r2KpbdxN30A

MoCo三部曲

## Masked Autoencoders

![](/images/img4/MAE.png)

https://mp.weixin.qq.com/s/x-ruExbM9T8EIv2gZW0Nnw

视觉预训练新范式MAE

https://www.zhihu.com/question/498364155

如何看待何恺明最新一作论文Masked Autoencoders？
