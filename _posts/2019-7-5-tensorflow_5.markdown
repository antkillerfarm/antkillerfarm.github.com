---
layout: post
title:  TensorFlow（五）
category: AI 
---

* toc
{:toc}

# Estimator

![](/images/img2/tensorflow_programming_environment.png)

Estimator是一个非常高级的API，其抽象等级甚至在Keras之上。

Estimator主要包括以下部分：

1.初始化。定义网络结构。

2.train。

3.evaluate。

4.predict。

TensorFlow已经包含了一些预置的Estimator。例如：BoostedTreesClassifier、DNNClassifier、LinearClassifier等。具体可参见：

https://tensorflow.google.cn/api_docs/python/tf/estimator

参考：

https://mp.weixin.qq.com/s/a68brFJthczgwiFoUBh30A

TensorFlow数据集和估算器介绍

https://mp.weixin.qq.com/s/zpEVU1E5DfElAnFqHCqHOw

训练效率低？GPU利用率上不去？快来看看别人家的tricks吧～

# 参考

https://mp.weixin.qq.com/s/t1QFIOq-VBNOrSm0zW-PlQ

深度学习TensorFlow实现集合

https://mp.weixin.qq.com/s/IzijD8Sh3G2WsCz7aaxyhg

TensorFlow深度学习概述

https://silencezjl.coding.me/2017/05/01/%E5%81%B7%E4%B8%80%E6%B3%A2%E8%B5%84%E6%BA%90/

各种TensorFlow资源

https://mp.weixin.qq.com/s/haj9lS59yWtk-C75EtGIcw

深度学习工程模板：简化加载数据、构建网络、训练模型和预测样本的流程

https://github.com/zsdev2015/machine_learning

某国内小牛写的中文入门demo，注释非常详细

https://morvanzhou.github.io/tutorials/

一个以python语言教学的ML、DL教程，比较通俗易懂。

https://mp.weixin.qq.com/s/gJBDXf_5ViPR9dNm3eH2Hg

TensorFlow初学者必须了解的55个经典案例

http://mp.weixin.qq.com/s/JZ1ceGQDmQUaNW5wl6biLA

TensorFlow实现流行机器学习算法教程汇集

https://github.com/taki0112/Tensorflow-Cookbook

1500+星标，简单易用TensorFlow代码集

https://mp.weixin.qq.com/s/bjxJyOitynRtCoW0FX1gXw

一文带你入门Tensorflow

https://mp.weixin.qq.com/s/zmTqWNXlYcDyZb_dmEo_5Q

TensorFlow/PyTorch/Sklearn实现的五十种机器学习模型

https://mp.weixin.qq.com/s/O5vvGKHWkJQWzeiL7A_S_g

TensorFlow简单介绍

https://mp.weixin.qq.com/s/JSZwQkyxSSwfBWKJ578j3A

TensorFlow最好的入门文章

https://mp.weixin.qq.com/s/68vaQRqUo8u09iheKzFVEw

玩转TensorFlow深度学习

https://mp.weixin.qq.com/s/Es_5KUnkDzMwf_8WD8aW3g

GitHub万星：适用于初学者的TensorFlow代码资源集

https://mp.weixin.qq.com/s/GaK_iSTBl7B4LTdaOtiR_Q

香港科技大学TensorFlow课件分享

https://mp.weixin.qq.com/s/RR3EEI8vm05EZSd7dGU__A

史上最全的Tensorflow学习资源汇总，速藏！

https://mp.weixin.qq.com/s/MYBTWL3X_OhLZL6C4rISzw

TensorFlow训练线性回归

https://mp.weixin.qq.com/s/5QYlh6gV9IqdQfraK4DC8w

10种深度学习算法的TensorFlow实现

https://mp.weixin.qq.com/s/W1KP213Ngj-BNEyx-_nVyw

利用TensorFlow实现卷积自编码器

https://mp.weixin.qq.com/s/pIESRzjsmqoO46P4x5Iqhw

Tensorflow卷积神经网络

https://mp.weixin.qq.com/s/Cge_GY19aZ1AcMkhW93C1A

TensorFlow中的那些高级API

https://mp.weixin.qq.com/s/kYOwUWlTP4T0IYKDWDbCsg

tensorflow object detection API训练公开数据集Oxford-IIIT Pets Dataset

https://mp.weixin.qq.com/s/8uDsaZjsiKXGea6M-w-RvA

tensorflow object detection API使用之GPU训练实现宠物识别

https://mp.weixin.qq.com/s/knw7yuUxHe-qeCLfj20onw

Bayesian GAN的TensorFlow实现

https://mp.weixin.qq.com/s/Sxui9CvdGocIxVG2FM4JtQ

基于tensorflow使用CNN-RNN进行中文文本分类！

https://mp.weixin.qq.com/s/kJxXIN6D5TEEFSFhGJNIyw

开源神经网络图片上色技术解析

https://mp.weixin.qq.com/s/qXMRHxDDRa-_rJZMhXWB4w

详解TensorFlow的新seq2seq模块及其用法

https://mp.weixin.qq.com/s/YdcIDXadEnDsyfc6Iu1gGw

手把手教你用TensorFlow训练模型

https://mp.weixin.qq.com/s/Off0pgaRNyik2nvjHaQQkw

在TensorFlow中对比两大生成模型：VAE与GAN

https://mp.weixin.qq.com/s/rMYjsIgFNvv47F4YZjY8SA

如何在K8S上玩转TensorFlow？

https://zhuanlan.zhihu.com/p/30751039

TensorFlow全新的数据读取方式：Dataset API入门教程

https://mp.weixin.qq.com/s/SDVQSn1aVDXk_RPGuVcQgQ

TensorFlow深度自动编码器入门实践

https://mp.weixin.qq.com/s/mZ79KAuSIJLtBEXvKwUi-w

如何利用TensorFlow.js部署简单的AI版“你画我猜”图像识别应用

https://mp.weixin.qq.com/s/yi-PNmMNMbwSi56aXo6ZSQ

tensorflow对象检测框架训练VOC数据集常见的两个问题

https://mp.weixin.qq.com/s/bcLCCvWrJLbMxwDl9GutjQ

TensorFlow动态图5行代码实现迁移学习-识别转变风格的MNIST

https://mp.weixin.qq.com/s/NMRwXqwr4VFbMUPgI8Uccg

使用Google Cloud上的tf.Transform对TensorFlow管道模式进行预处理

https://mp.weixin.qq.com/s/UKt1cFLcRYZQTJiZRiajwQ

TensorFlow Servering C/S通信约束

https://mp.weixin.qq.com/s/DpqI4AfjiygCh8dqq_Kgmw

基于TensorFlow Serving的深度学习在线预估

https://mp.weixin.qq.com/s/LRxyvVazRAOR_B0as7ujvg

腾讯互娱基于CPU环境的分布式YOLOv3实现

https://mp.weixin.qq.com/s/XcqVtFBY5rIn0FgPEx0eTg

工业领域中的AI：BHGE通过使用TensorFlow概率编程工具包开发的基于物理的概率深度学习

http://brucedone.com/archives/1005

Tensorflow破解验证码

https://mp.weixin.qq.com/s/CjlEY_m6tp-NJ3B2MiAZRg

基于TensorFlow的深度模型训练GPU显存优化

http://gitbook.cn/books/593d71ba4686067a2200aec6/index.html

用TensorFlow实现智能机器人的原理及如何实现一个对话机器人

https://mp.weixin.qq.com/s/lLaSXG1VF9Rys2GNzFP7pw

轻松使用多种预训练卷积网络抽取图像特征

https://mp.weixin.qq.com/s/tVqp1Tht1P-0EQazJizQaA

利用人口普查收入数据集预测收入

https://mp.weixin.qq.com/s/3WleFm9S_wMIPTz_WfjKQw

TensorFlow支持Unicode，中文NLP终于省心了

https://mp.weixin.qq.com/s/jFEOokxfJ1Kw-P3wvw3EAg

带你了解，不规则张量！

https://mp.weixin.qq.com/s/tMtx4PZbpo5IrnhzLz8Lzw

AutoGraph：图的简易控制流程

https://mp.weixin.qq.com/s/zY7rGh-kA-36VEo9DiaKbg

TensorFlow进行简单的图像处理

https://mp.weixin.qq.com/s/DAV3TDI4JYr0sXqTGU6t2A

分布式TensorFlow入坑指南：从实例到代码带你玩转多机器深度学习

https://mp.weixin.qq.com/s/QU5NjksCEswjHnkY7WXWXQ

分布式TensorFlow入门教程

https://mp.weixin.qq.com/s/pBR4wMITrigbSVAvn0d6vQ

利用TensorFlow实现上下文的Chat-bots

https://mp.weixin.qq.com/s/8I5Nvw4t2jT1NR9vIYT5XA

深入理解TensorFlow中的tf.metrics算子

https://mp.weixin.qq.com/s/aMarI-nyIvFqhtpJWQrNhQ

谷歌推强化学习新框架“多巴胺“，基于TensorFlow，已开源

https://mp.weixin.qq.com/s/ntHkMIef1o2-FF-AJf_bZQ

三分钟训练眼球追踪术，AI就知道你在盯着哪个妹子——TensorFlow.js代码

https://mp.weixin.qq.com/s/7rTmEBfh613SrNnTQvfSjw

懒人福利：不写代码调优深度模型，谷歌开源的“What-If”了解一下

https://mp.weixin.qq.com/s/eX3LWYiSH-KObH_7F_3QCA

TensorFlow 1.11.0发布，一键多GPU

https://mp.weixin.qq.com/s/316VVXLQfeIsKNk4ld-VRw

TensorFlow语义分割套件开源了ECCV18旷视科技BiSeNet实时分割算法

https://mp.weixin.qq.com/s/XI1J4ardEWKP4UQ4IXZGTQ

TensorFlow Hub,给您带来全新的Web体验

http://www.jianshu.com/p/1da012a83b74

利用TensorFlow实现排序和搜索算法

https://mp.weixin.qq.com/s/oEqMjOTj8xpd3sg60ZUhqA

TensorFlow的c++实践及各种坑

https://mp.weixin.qq.com/s/-5RCRl9ztQ2dQmX00QvfvQ

在Python和TensorFlow上构建Word2Vec词嵌入模型

https://mp.weixin.qq.com/s/Nyjp0mZxcn04vLKjJXLSaw

如何用TensorFlow在安卓设备上实现深度学习推断

https://mp.weixin.qq.com/s/OVWbxBNc4i0_5jgy06xS1A

基于Tensorflow Estimators的文本分类

https://mp.weixin.qq.com/s/h4Ve_UUajsdlk7PNS6J4QA

TensorFlow Estimator模型从训练到部署

https://mp.weixin.qq.com/s/c_2_9gvOynHaVW6pi4qQjQ

用TensorFlow让机器人唱首歌给你听

https://mp.weixin.qq.com/s/hn-LqyREkusxP2TOWfTJ6g

使用TensorFlow官方Java API调用TensorFlow模型

https://mp.weixin.qq.com/s/kS92vYyeHLc38RGc_4CZbg

如何应用TFGAN快速实践生成对抗网络？

https://mp.weixin.qq.com/s/hquOoKeeHQXqWcHM6Bkvbw

如何训练一个简单的音频识别网络

https://mp.weixin.qq.com/s/aWez5FYXXnRnDbb0zcXFXQ

如何在TensorFlow中训练提升树模型

https://mp.weixin.qq.com/s/kEowgNPVS1nAGBPbzkatlQ

如何构建高可读性和高可重用的TensorFlow模型

https://mp.weixin.qq.com/s/O_IN39FBVPeD5fRYBsPuZQ

用TensorFlow开发问答系统

https://mp.weixin.qq.com/s/8Hrq_z8s_5ms6Q_6OOaU-g

如何使用TensorFlow和自编码器模型生成手写数字

https://mp.weixin.qq.com/s/rQ9eZosHOoDOXg9tAg4t6A

tensorflow Object Detection API使用预训练模型mask r-cnn实现对象检测

https://mp.weixin.qq.com/s/cPWXAI2TBv3_ssnWDFoQ4w

TensorFlow sucks，有人吐槽TensorFlow晦涩难用

https://mp.weixin.qq.com/s/_kr28kN0_1QFP8BR_wGo5w

TensorFlow RNN入门

https://mp.weixin.qq.com/s/WqE-FRl-Thys7tHUvFNlWQ

盯住梅西：TensorFlow目标检测实战

https://mp.weixin.qq.com/s/WfzlHtz0FFJMsPFwPoMqJg

如何利用VGG-16等模型在CPU上测评各深度学习框架

http://www.jianshu.com/p/4e16ae0aad25

利用TensorFlow入门Word2Vec

https://mp.weixin.qq.com/s/YJmMfBhQ3cLNUp_HHsXhGA

手把手教你使用TensorFlow生成对抗样本

https://mp.weixin.qq.com/s/nnjyR4XGVZQ1zXCIPzTNlg

基于TensorFlow的变分自编码器实现

https://mp.weixin.qq.com/s/iMgesGmdb7Jq4muCxb-nFA

Tensorflow实战：Discuz验证码识别

https://mp.weixin.qq.com/s/4aJUGBpPG_6Oc5EqOmM0Iw

作为TensorFlow的底层语言，你会用C++构建深度神经网络吗？

https://github.com/yahoo/TensorFlowOnSpark

TensorFlow On Spark

https://mp.weixin.qq.com/s/7er3wNV_IhxhFDOIwNMpww

深度强化学习入门：用TensorFlow构建你的第一个游戏AI

https://mp.weixin.qq.com/s/jMPVl3CWvL7MSzq5F12YxQ

维度、广播操作与可视化：如何高效使用TensorFlow

https://mp.weixin.qq.com/s/OmVAnkHV2aI4D4pMKyVjCQ

基于TensorFlow理解三大降维技术：PCA、t-SNE和自编码器

https://mp.weixin.qq.com/s/YOyOR8fdaEKcydAywcc-HA

如何使用TensorFlow API构建视频物体识别系统

https://zhuanlan.zhihu.com/p/28475975

如何优雅地用TensorFlow预测时间序列：TFTS库详细教程

https://mp.weixin.qq.com/s/zZCEOdNQsPovn_i-C57Z9g

如何使用最流行框架Tensorflow进行时间序列分析？

https://mp.weixin.qq.com/s/CqOo7Fu6t5-yJiYhzo03oQ

利用TensorFlow和神经网络来处理文本分类问题

https://mp.weixin.qq.com/s/VlvQmrS7Qi2qq6fTBXKTYw

从零开始用TensorFlow搭建卷积神经网络

https://mp.weixin.qq.com/s/hETnA81WlkMG3rftAHg9bw

PyTorch和TensorFlow哪家强：九项对比读懂各自长项短板

https://mp.weixin.qq.com/s/7R-Gvegnta9XBwIaSPBL_Q

基于Tensorflow的验证码识别
