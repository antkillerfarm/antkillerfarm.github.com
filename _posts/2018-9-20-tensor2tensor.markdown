---
layout: post
title:  Tensor2Tensor, MXNet, Horovod
category: DL Framework 
---

* toc
{:toc}

# Tensor2Tensor

T2T库利用TensorFlow工具来开发，定义了一个深度学习系统中需要的多个部分：数据集、模型架构、优化工具、学习速率衰减计划，以及超参数等等。

最重要的是，T2T在所有这些部分之间实现了标准接口，并配置了当前机器学习的最佳行为方式。

因此，你可以选择任意数据集、模型、优化工具，以及一套超参数，随后运行训练，看看效果如何。

论文：

《Tensor2Tensor for Neural Machine Translation》

代码：

https://github.com/tensorflow/tensor2tensor

参考：

https://cloud.tencent.com/developer/article/1153079

“变形金刚”为何强大：从模型到代码全面解析Google Tensor2Tensor系统

这篇blog不仅对Tensor2Tensor的基本结构做了介绍，也对transformer模型进行了介绍。transformer模型的详解参见《Attention（二）》。

## mesh tensorflow

T2T不仅支持单机，还支持网格（Mesh）计算，推出了所谓的mesh tensorflow，简称MTF。

## 参考

https://blog.csdn.net/hpulfc/article/details/81172498

如何使用tensor2tensor自定义数据并训练模型

https://blog.csdn.net/hpulfc/article/details/82625217

tensor2tensor自定义问题，训练模型(bpe篇)

https://mp.weixin.qq.com/s/YRDZ8inqaxmdD5g_SR1MLg

四种常见NLP框架使用总结

# Tensor2Tensor transformer实战

## 准备数据

tensor2tensor/data_generators/translate_enzh.py

这个脚本包含了很多数据集的下载地址。

我们这里使用的是官方提供英汉翻译数据集：

http://data.statmt.org/wmt18/translation-task/training-parallel-nc-v13.tgz

这个数据集中，中英文是分开的：

training-parallel-nc-v13/news-commentary-v13.zh-en.en

training-parallel-nc-v13/news-commentary-v13.zh-en.zh

上面是训练集，测试集也是类似的。

## 数据预处理

这个过程比较漫长，大约1小时左右，期间CPU全满，而GPU全空，一度让我以为我的GPU相关配置不对。

数据预处理主要完成如下数据转换：

token（文字）->id（词典中的index）

它主要由T2T中的Problem模块进行配置。

例如，英汉翻译的设置一般为：`--problem=translate_enzh_wmt32k`。

Problem可以是一条继承链，比如上例中的继承链为：

TranslateEnzhWmt32k->TranslateProblem->Text2TextProblem->Problem

Problem类是所有Problem的基类。

由于中文符号表中，不仅有字还有词，让我一度以为使用了什么分词工具，后来才发现只是简单的词频统计相关的处理而已。相关代码在：

tensor2tensor/data_generators/text_encoder.py：SubwordTextEncoder.build_from_token_counts

T2T默认使用谷歌内部的subword分词法。这个分词法还有个独立的入口在：

tensor2tensor/data_generators/text_encoder_build_subword.py

除此之外，网上也有人使用Byte Pair Encoding(BPE)分词法进行分词。

tensor2tensor/layers/common_layers.py：embedding

导入词向量：

transformer模型的input/output都是单词在词典中的index，需要通过查询词向量表，将其替换为词向量。

tensor2tensor/layers/modalities.py：SymbolModality

encoder导入：input_emb

decoder导入：target_emb

## 模型

transformer模型定义文件如下：

tensor2tensor/models/transformer.py

这里我采用的是transformer_base_single_gpu的超参，loss可降至0.4左右。如果采用transformer_base的话，就只能降到2.0左右。

num_encoder_layers/num_decoder_layers控制transformer的层数，如果为0，就使用num_hidden_layers的值。

位置信息的函数：

get_timing_signal_1d


# MXNet

https://mp.weixin.qq.com/s/50uGupiwU2UVuVeMm_bTag

李沐介绍MXNet新接口Gluon：高效支持命令式与符号式编程

https://mp.weixin.qq.com/s/VRmhAluhKWxyPbStmMoWtg

一张速查表实现Apache MXNet深度学习框架五大特征的开发利用

http://zh.gluon.ai/index.html

李沐：《动手学深度学习》（使用Gluon）

https://zhuanlan.zhihu.com/p/30966663

用Gluon炼丹体验

https://mp.weixin.qq.com/s/B8TYVG5WjDJ4NX-wirS2Fg

Alex Smola：深度学习触手可及，架构Gluon高中生就能用

https://mp.weixin.qq.com/s/wdDYUOLpi0l2mRirQhHjPQ

Apache MXNet助力数字营销，检测社交网络照片中的商标品牌

http://mp.weixin.qq.com/s/Z5X7Vamuw1BwtienhOrSvQ

解构TensorFlow, 学习MXNet--新一代深度学习系统的核心思想

https://mp.weixin.qq.com/s/_PAzff6V9UgwLCos05B4rg

十分钟从PyTorch转MXNet

https://mp.weixin.qq.com/s/2ulMPGEHYkOqWhT2XY449w

TensorFlow、MXNet、PaddlePaddle三个开源库对比

https://mp.weixin.qq.com/s/-BlBm3ZTZ32ijORJux7rTA

DMLC团队发布GluonCV和GluonNLP：两种简单易用的DL工具箱

https://mp.weixin.qq.com/s/Vb-7hGw7UOx4jP3lwW3Leg

从VGG到ResNet，你想要的MXNet预训练模型轻松学

https://mp.weixin.qq.com/s/CgxrvNfyu35SMvWBAt-5kg

MXNet开放支持Keras，高效实现CNN与RNN的分布式训练

https://mp.weixin.qq.com/s/qLax1Jb0OOfFOU68wn_2PA

从三大神经网络，测试对比TensorFlow、MXNet、CNTK、Theano四个框架

https://mp.weixin.qq.com/s/hRH7hVsaQBqf0vhD_BqBgg

五大深度学习框架三类神经网络全面测评

https://zhuanlan.zhihu.com/p/42345854

如何基于gluon训练一个强有力的Reid Baseline

# Horovod

Horovod是Uber开源的一套分布式深度学习框架。Horovod支持TensorFlow、Keras、PyTorch和Apache MXNet等后端框架。

官网：

https://eng.uber.com/horovod/

代码：

https://github.com/horovod/horovod

参考：

https://www.cnblogs.com/rossiXYZ/p/14856464.html

深度学习分布式训练框架Horovod

https://zhuanlan.zhihu.com/p/40578792

Horovod-基于TensorFlow分布式深度学习框架

https://zhuanlan.zhihu.com/p/158375055

PyTorch单机多卡操作(分布式DataParallel，混合精度，Horovod)

https://mp.weixin.qq.com/s/iQIRj7ifsOEnupYZuQsVwQ

是时候放弃TensorFlow集群，拥抱Horovod了

https://mp.weixin.qq.com/s/qOjGrR59Mf0Mzgh4bpDhrA

详解Horovod：Uber开源的TensorFlow分布式深度学习框架

https://mp.weixin.qq.com/s/yNxjJHpGns6utpBpI-0XDA

分布式训练框架Horovod初步学习

https://zhuanlan.zhihu.com/p/374575049

一文看懂Horovod源码

# DMLC

Distributed (Deep) Machine Learning Community是陈天奇发起的一个社区。

它的核心库，被称为dmlc-core。目前已被TVM、MXNet等项目所采用。

代码：

https://github.com/dmlc/dmlc-core

# 机器人/无人驾驶参考资源+

https://mp.weixin.qq.com/s/cLhnFCQdftj6Qi5j-PWDzw

控制技术详解——控制理论

https://mp.weixin.qq.com/s/N39CF1M7nV2cwUUMGRnG7g

控制技术详解——基于模型的控制方法

https://mp.weixin.qq.com/s/DrWeZOIhJV1dbpg7Ns6gJQ

控制技术详解——控制器的类型

https://mp.weixin.qq.com/s/Q4me1G9B8uwwRIr6dfETEA

行为轨迹预测技术

https://mp.weixin.qq.com/s/3ApXC3u13_O8s4QCYxldlQ

基于模块化轻量级网络的道路目标检测

https://mp.weixin.qq.com/s/kAP8MMjTjVyeBPcpY5E1FQ

障碍物行为预测技术

https://mp.weixin.qq.com/s/VwLNq84Nmg5--yTHlRdSJA

深度学习在道路封闭挖掘方案的探索与实践

https://mp.weixin.qq.com/s/sxm2cD7DAhJ6eIhUDvYRKA

运动轨迹规划技术

https://mp.weixin.qq.com/s/hs6uMkRctASmH1YrNOKFdw

脱手检测在自动驾驶中的原理及应用

https://zhuanlan.zhihu.com/p/83129242

自动驾驶近期行为预测和规划的一些文章介绍

https://zhuanlan.zhihu.com/p/83605553

移动机器人同时估计自身位姿和物体位姿

https://mp.weixin.qq.com/s/M-ihYsQUsT3deekhniixrQ

聊聊无人驾驶技术中的路由寻径

https://mp.weixin.qq.com/s/eVHAmNn7sHcghLkl-ksnAw

谈谈“域控制器”

https://mp.weixin.qq.com/s/aaN7W1cnlQyl4kPNGvxn_Q

斯坦福大学李飞飞高徒朱玉可博士毕业论文——闭合感知-动作循环: 通用自主机器人探索

https://mp.weixin.qq.com/s/w8gjhL_NiQSY_OKZ_RXb2g

3D机器人视觉在仓储物流和工业自动化领域的应用

https://mp.weixin.qq.com/s/rA9AAVx7AlNuS1l4IUuG0w

深度学习技术在自动驾驶中的应用

https://mp.weixin.qq.com/s/4_jtb9gv20F6h1Ljw4JwEw

车载以太网通信的“套娃游戏”

https://zhuanlan.zhihu.com/p/86184886

行人的行为意图建模和预测(上)

https://zhuanlan.zhihu.com/p/86185203

行人的行为意图建模和预测(下)

https://zhuanlan.zhihu.com/p/90773462

多传感器数据深度图的融合：最近基于深度学习的方法

https://mp.weixin.qq.com/s/4tOYmCRiFN0xsG6vbedrrg

ADAS系统中的动态目标感知策略（一）

https://mp.weixin.qq.com/s/JT4p03m77ohOufL3JFecvA

从硬件角度剖析自动驾驶，为什么说它是复杂的系统工程？

https://mp.weixin.qq.com/s/UGdZCC80gQRTgHz9GV6USA

MEMS IMU/陀螺仪对准基础

https://mp.weixin.qq.com/s/v3gsmCWSI9pEwRolN8qWNA

基于深度卷积网络的自动驾驶多模态轨迹预测

https://mp.weixin.qq.com/s/R-17JgcGHG67KZ6yRYNFlA

基于Frenet优化轨迹的无人车动作规划方法

https://mp.weixin.qq.com/s/qZwqp5x6yEXbMBHGzevm0g

ACC自适应巡航控制系统介绍

https://zhuanlan.zhihu.com/p/57077589

自动驾驶中路上行人的行为和意图理解及预测

https://zhuanlan.zhihu.com/p/109900137

传感器融合-任务篇

https://zhuanlan.zhihu.com/p/109895639

传感器融合-数据篇

https://mp.weixin.qq.com/s/1sbL2vmugiIlSn_ehIOuig

车载多传感器融合定位方案：GPS +IMU+MM

https://mp.weixin.qq.com/s/5kJfhp3vi9uuSFaeONKfrA

打破传统方法，MIT新芯片帮自动驾驶汽车穿越浓雾

https://zhuanlan.zhihu.com/p/57781001

自动驾驶系统的硬件平台讨论

https://mp.weixin.qq.com/s/ZUxkZXLC0tPkzptFKtudhw

让机器人也能“问路”的视觉语言导航新方法

https://mp.weixin.qq.com/s/6OkLjK1bMbw6a3bvYn_DCQ

深度学习在自动驾驶感知领域的应用

https://zhuanlan.zhihu.com/p/57029694

自动驾驶中单目摄像头检测输出3-D边界框的方法一览

https://mp.weixin.qq.com/s/SZlwjnZrxCyaqRBg_sjQaA

浅谈自动驾驶中的行为风险识别（一）

https://mp.weixin.qq.com/s/QBnvLrD93b8cDEeNeZ5kAw

车联网正步入歧途，命悬一线的开始

https://zhuanlan.zhihu.com/p/553588646

最新的“视觉为中心的BEV感知”综述论文
