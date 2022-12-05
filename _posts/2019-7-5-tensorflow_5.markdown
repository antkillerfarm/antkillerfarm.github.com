---
layout: post
title:  TensorFlow（五）
category: DL Framework 
---

* toc
{:toc}

# TensorFlow.js

https://mp.weixin.qq.com/s/dqMS4NjmNYs7IFHm8uFM8w

TensorFlow发布面向JavaScript开发者的机器学习框架TensorFlow.js

https://zhuanlan.zhihu.com/p/35181413

TensorFlow.js人脸识别—玩转吃豆豆小游戏

https://mp.weixin.qq.com/s/ebLHZAG8H78TsZUKSzAtIw

TF官方博客：基于TensorFlow.js框架的浏览器实时姿态估计

https://mp.weixin.qq.com/s/z6p4A4DfCuK8IBGVGwrtLQ

如何利用TensorFlow.js部署简单的AI版“你画我猜”图像识别应用

https://mp.weixin.qq.com/s/NO_XY-JmTpIkoC-fpkZ-qg

在浏览器上也能训练神经网络？TensorFlow.js带你玩游戏~

https://mp.weixin.qq.com/s/vjpMr3TsF3Lui8Q0IstQxw

浏览器上跑：TensorFlow发布实时人物分割模型，秒速25帧，24个部位

https://mp.weixin.qq.com/s/-BblgnvPLuqpYM8PZ7PQCQ

三行代码实时追踪你的手，只要有浏览器就够了

https://mp.weixin.qq.com/s/C7QdVathJ8YTXF-zXPC-Ow

有人分析了7个基于JS语言的DL框架，发现还有很长的路要走

# 混合精度训练

https://tensorflow.google.cn/guide/mixed_precision

# 细节

执行`session.run(out)`，会在终端打印out的值，但执行`res = session.run(out)`则不会。

此外，`session.run`可以接受list作为参数。返回值也是一个list，分别对应输入list的每个元素的计算结果。

---

tensorflow的程序中,在main函数下,都是使用tf.app.run()来启动。查看源码可知,该函数是用来处理flag解析，然后执行main函数。

https://blog.csdn.net/lujiandong1/article/details/53262612

tensorflow中的tf.app.run()

---

TF提供了一套专门的IO函数：tf.gfile。主要优点在于：对于写文件来说，open操作直到真的需要写的时候才执行。

---

迁移学习的时候，有的时候需要保持某几层的权值，在后续训练中不被改变。这时，可以在创建Variable时，令trainable=false。

---

sparse_softmax_cross_entropy_with_logits和softmax_cross_entropy_with_logits的区别在于：后者的label是一个one hot的tensor，而前者label直接用对应分类的index表示就行了。

---

CNN中的padding：

"SAME" = with zero padding。

"VALID" = without padding。

---

op的自定义实现可使用`tf.py_func`。

---

tf.dtypes.cast: 类型转换

---

Keras对大部分权重矩阵都采用了标准的Glorot uniform初始化，对GRU的recurrent weight采用了正交初始化，对所有偏置都采用了零初始化；而PyTorch对所有参数都一律采用了uniform初始化，但范围与Glorot不同。

https://www.zhihu.com/question/268494717

同一个模型用theano，tf，pytorch实现，performance可能差距较大吗？

---

`CUDA_VISIBLE_DEVICES`用于指定使用的显卡，因此`CUDA_VISIBLE_DEVICES=0`表示使用0号显卡。如果打算使用CPU的话，需要`CUDA_VISIBLE_DEVICES=`。

# TensorFlow参考

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

https://mp.weixin.qq.com/s/3QgtemxxsQmuNQVEdpiMwA

如何做准确率达98%的交通标志识别系统？
