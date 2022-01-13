---
layout: post
title:  TensorFlow（五）
category: AI 
---

* toc
{:toc}

# TensorFlow.js（续）

https://mp.weixin.qq.com/s/NO_XY-JmTpIkoC-fpkZ-qg

在浏览器上也能训练神经网络？TensorFlow.js带你玩游戏~

https://mp.weixin.qq.com/s/vjpMr3TsF3Lui8Q0IstQxw

浏览器上跑：TensorFlow发布实时人物分割模型，秒速25帧，24个部位

https://mp.weixin.qq.com/s/-BblgnvPLuqpYM8PZ7PQCQ

三行代码实时追踪你的手，只要有浏览器就够了

https://mp.weixin.qq.com/s/C7QdVathJ8YTXF-zXPC-Ow

有人分析了7个基于JS语言的DL框架，发现还有很长的路要走

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

# Log

TF里有两套Log系统：`LOG`和`VLOG`。

`LOG`由`TF_CPP_MIN_LOG_LEVEL`控制，值越小，信息越多。

`VLOG`都是INFO级别的Log，因此，`TF_CPP_MIN_LOG_LEVEL`必须为0。此外，`VLOG`本身亦有不同等级，可使用`TF_CPP_MIN_VLOG_LEVEL`控制，值越大，信息越多。

# loss & accuracy

loss：训练集损失值

accuracy:训练集准确率

val_loss:测试集损失值

val_accruacy:测试集准确率

以下5种情况可供参考：

train loss 不断下降，test loss不断下降，说明网络仍在学习;（最好的）

train loss 不断下降，test loss趋于不变，说明网络过拟合;（max pool或者正则化）

train loss 趋于不变，test loss不断下降，说明数据集100%有问题;（检查dataset）

train loss 趋于不变，test loss趋于不变，说明学习遇到瓶颈，需要减小学习率或批量数目;（减少学习率）

train loss 不断上升，test loss不断上升，说明网络结构设计不当，训练超参数设置不当，数据集经过清洗等问题。（最不好的情况）

参考：

https://www.cnblogs.com/Timeouting-Study/p/12591448.html

TensorFlow中loss与val_loss、accuracy和val_accuracy分别是什么含义

# Eager Execution

TensorFlow的Eager Execution可立即评估操作，无需构建图：操作会返回具体的值，而不是构建以后再运行的计算图。这也就是所谓的动态图计算的概念。

参考：

https://mp.weixin.qq.com/s/Yp2zE85VCx8q67YXvuw5qw

TensorFlow引入了动态图机制Eager Execution

https://github.com/ZhuanZhiCode/TensorFlow-Eager-Execution-Examples

Eager Execution的代码示例

https://github.com/madalinabuzau/tensorflow-eager-tutorials

TensorFlow的动态图工具Eager怎么用？这是一篇极简教程

https://mp.weixin.qq.com/s/Lvd4NfLg0Lzivb4BingV7w

Tensorflow Eager Execution入门指南

https://github.com/snowkylin/TensorFlow-cn

简单粗暴TensorFlow Eager教程

https://github.com/snowkylin/tensorflow-handbook

简单粗暴TensorFlow 2.0

https://mp.weixin.qq.com/s/zz8XCykJ6jxbE5J4YwAkEA

一招教你使用tf.keras和eager execution解决复杂问题

# tf.data

tf.data提供了一套构建灵活高效的输入流水线的API。

![](/images/img2/datasets_without_pipelining.png)

![](/images/img2/datasets_with_pipelining.png)

上面两幅图中，第一幅图是没有使用流水线的情况，而第二幅图则是使用流水线的情况。

参考：

https://mp.weixin.qq.com/s/dfXTV4PFgC1Wbti42Zf4wQ

tf.data API，让你轻松处理数据

https://mp.weixin.qq.com/s/mjUnrPBPBuY6XKXkUymX-w

实例介绍TensorFlow的输入流水线

https://mp.weixin.qq.com/s/1ZlyVDJK6RWZ_1Ox7399IA

用一行tf.data实现数据Shuffle、Batch划分、异步预加载等

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
