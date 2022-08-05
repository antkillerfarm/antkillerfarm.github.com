---
layout: post
title:  Tensor2Tensor, NN中间语言, MXNet, Horovod
category: AI 
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

# NN中间语言

## 概述

随着目前DL框架越来越多，如何在框架之间对模型进行相互转换，就成为了工业界的一个大问题。

https://github.com/ysh329/deep-learning-model-convertor

这个网页包含了各种深度框架之间的模型相互转换的工具的列表。从中可以看出ONNX和MMdnn算是目前比较有用的转换工具了。

这类many-to-many工具从实现原理上，主要是将各种模型转换成中间语言（IR，intermediate representation），然后再变换成目标语言。

## NNEF

Neural Network Exchange Format是Khronos制定的用于交换NN模型数据的数据格式标准。

官网：

https://www.khronos.org/nnef

## ONNX

和NNEF竞争的标准还有微软和Facebook联合推出的Open Neural Network Exchange。

官网：

https://onnx.ai/

代码：

https://github.com/onnx

![](/images/img3/ONNX.jpg)

截止2020.5，ONNX已经获得压倒性的胜利，成为了事实上的标准。

中文指南：

https://zhuanlan.zhihu.com/p/41255090

ONNX--跨框架的模型中间表达框架

修改ONNX模型：

https://github.com/saurabh-shandilya/onnx-utils

参考：

https://mp.weixin.qq.com/s/etSrI8Z3-NWbrqNWIbfzjw

微软Facebook联手发布AI生态系统

https://mp.weixin.qq.com/s/D5rQ6r3s54PR_esAAu5rhQ

开源一年多的模型交换格式ONNX，已经一统框架江湖了？

https://blog.csdn.net/xxradon/article/details/104715524

onnx模型如何增加或者去除里面node，即修改图方法

https://mp.weixin.qq.com/s/H1tDcmrg0vTcSw9PgpgIIQ

ONNX初探

https://mp.weixin.qq.com/s/_iNhfZNR5-swXLhHKjYRkQ

ONNX再探

https://zhuanlan.zhihu.com/p/350702340

onnx simplifier和optimizer

https://mp.weixin.qq.com/s/naJwpmXm7Yl3pxEFYiGf-g

深度探索ONNX模型部署

https://zhuanlan.zhihu.com/p/32711259

从NNVM和ONNX看AI芯片的基础运算算子

## MMdnn

MMdnn是微软推出的工具集，也是目前功能最强的工具集。

官网：

https://github.com/Microsoft/MMdnn

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

# AI Compiler+

https://mp.weixin.qq.com/s/jjT0x99ht8xtfWmzL-0R1A

深度学习的IR“之争”

https://mp.weixin.qq.com/s/hEt4BSPMP1WWuHFMEbICqw

机器学习编译器：MLIR Dialect体系

https://mp.weixin.qq.com/s/G36IllLOTXXbc4LagbNH9Q

编译器与IR的思考: LLVM IR，SPIR-V到MLIR

https://www.zhihu.com/question/391811802

如何评价TensorFlow开源的新运行时TFRT？

https://zhuanlan.zhihu.com/p/347599203

TFRT的开源代码分析

## MLIR

Multi-Level IR

代码：

tensorflow/compiler/mlir

三种到XLA的IR dialect：

chlo：client HLO dialect，上层前端的IR。

mhlo：支持动态shape的IR。

lmhlo：内存分配之后的IR，也就是无动态shape的IR。

---

Affine Dialect：这种Dialect使用来自多面体编译的技术使依赖分析和循环转换高效可靠。

GPU Dialect：MLIR中的GPU Dialect模拟了类似于CUDA或OpenCL的通用GPU编程范式。它的目标是提供抽象来模拟GPU特定的操作和属性。它在很大程度上意味着与供应商无关。

文档：

https://mlir.llvm.org/docs/Dialects/

---

参考：

https://mp.weixin.qq.com/s/fal6vz9gaZMbR41QMGE3AQ

MLIR发布：全新的中介码与编译器框架

https://zhuanlan.zhihu.com/p/361448250

MLIR Toy Tutorials

https://zhuanlan.zhihu.com/p/141256429

MLIR文章视频汇总

https://zhuanlan.zhihu.com/p/379063169

MLIR: 编译器基础架构重定义

https://zhuanlan.zhihu.com/p/508345356

AI编译器的概览、挑战和实践

https://blog.csdn.net/just_sort/article/details/123624966

基于MLIR的矩阵乘法高性能GPU代码生成：一些早期结果
