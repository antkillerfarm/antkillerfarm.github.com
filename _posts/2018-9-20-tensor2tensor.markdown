---
layout: post
title:  Tensor2Tensor, NN中间语言, OpenCV（二）, Dlib, MXNet
category: AI 
---

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

这篇blog不仅对Tensor2Tensor的基本结构做了介绍，也对transformer模型进行了介绍。transformer模型的详解参见《深度学习（二十五）》。

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

## mesh tensorflow

T2T不仅支持单机，还支持网格（Mesh）计算，推出了所谓的mesh tensorflow，简称MTF。

## 参考

https://blog.csdn.net/hpulfc/article/details/81172498

如何使用tensor2tensor自定义数据并训练模型

https://blog.csdn.net/hpulfc/article/details/82625217

tensor2tensor自定义问题，训练模型(bpe篇)

# GNU Octave

GNU Octave是Matlab的一个开源实现。它拥有和后者兼容的语法，类似的IDE，并实现了大部分的基础库。

官网：

https://gnu.org/software/octave/

安装方法:

`sudo apt-get install octave`

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

https://github.com/onnx

参考：

https://mp.weixin.qq.com/s/etSrI8Z3-NWbrqNWIbfzjw

微软Facebook联手发布AI生态系统

https://mp.weixin.qq.com/s/D5rQ6r3s54PR_esAAu5rhQ

开源一年多的模型交换格式ONNX，已经一统框架江湖了？

## NNVM

陈天奇等推出的NNVM也是一个类似的中间表示。官网：

https://github.com/dmlc/nnvm

参考：

https://mp.weixin.qq.com/s/qkvX0rmEe0yQ-BhCmWAXSQ

李沐：AWS开源端到端AI框架编译器NNVM

## Android NN

https://developer.android.com/ndk/guides/neuralnetworks/index.html

这是Android NDK中的NN相关的接口文档

https://developer.arm.com/products/software/mali-drivers/android-nnapi

这是ARM对于Android NN的一个实现。

## MMdnn

MMdnn是微软推出的工具集，也是目前功能最强的工具集。

官网：

https://github.com/Microsoft/MMdnn

## 参考

https://zhuanlan.zhihu.com/p/32711259

从NNVM和ONNX看AI芯片的基础运算算子

https://mp.weixin.qq.com/s/jjT0x99ht8xtfWmzL-0R1A

深度学习的IR“之争”

# OpenCV

https://www.jianshu.com/p/53982e81d781

OpenCV十字激光中心与端点识别

https://mp.weixin.qq.com/s/N4mRrQ4180qM2tuq5smK8w

如何识别出轮廓准确的长和宽

https://mp.weixin.qq.com/s/uDg0ho3jHsomfSUDTXTe7Q

OpenCV4.0 Mask RCNN 实例分割示例 C++/Python实现

https://mp.weixin.qq.com/s/tzfC3DpkPvEEEtbiGVoCSA

OpenCV多目标跟踪与视频分析

https://mp.weixin.qq.com/s/Ds3tYNFHcITgOAAgdANgJA

OpenCV深度学习人脸识别示例——看大佬如何秀恩爱

https://mp.weixin.qq.com/s/GMt66ZIO5AiF1B2B8nDVKw

开源、高质量QR码多语言生成库

https://mp.weixin.qq.com/s/Q52nxDKWQieygmUt2tG-BA

OpenCV图像处理之基于积分图实现NCC快速相似度匹配

https://blog.csdn.net/Young_Gy/article/details/75194914

无人驾驶之车道线检测简易版

https://mp.weixin.qq.com/s/pdaMAzlZioWsRoC_VdVNYA

OpenCV手部关键点检测（手势识别）

https://mp.weixin.qq.com/s/rImpez9hv-yzFR0EWVFiig

OpenCV寻找复杂背景下物体的轮廓

https://mp.weixin.qq.com/s/L7fRMpaCqgdY9bIAnJy4kg

Adrian小哥教程：如何使用Tesseract和OpenCV执行OCR和文本识别

https://mp.weixin.qq.com/s/0jeM3r4pvX2hAtNq8O1i-A

OpenCV轮廓层次分析实现欧拉数计算

https://mp.weixin.qq.com/s/SkAR1RwtMMpQPBldRTr3Ag

OpenCV金字塔图像分辨率重建与融合

https://mp.weixin.qq.com/s/RU2jskHewLvMigwCHIWyGw

多目标追踪器：用OpenCV实现多目标追踪

https://mp.weixin.qq.com/s/hNrhpghzR0negxFggu0PCQ

OpenCV中KLT光流跟踪原理详解与代码演示

https://mp.weixin.qq.com/s/14p7nDxAknWLVl95Qs2Bjw

从人脸检测到语义分割，OpenCV预训练模型库

https://mp.weixin.qq.com/s/MRRljFBH1T1STvTm0CT4Qw

OpenCV深度神经网络实现人体姿态评估

https://mp.weixin.qq.com/s/g1m3I9U9Walj9FxbYJng9A

万圣节教你用OpenCV Remix一张n合1脸

https://mp.weixin.qq.com/s/LkDcohFvQab4LY7B1Xg0Mw

简析缝隙检测算法新思路

https://mp.weixin.qq.com/s/GnZRurbPNdouTKu3JG7drQ

深度学习美颜系列---AI美颜磨皮算法

https://mp.weixin.qq.com/s/-Vde_zdmLaF-2F4IRJQuBw

一文道尽传统图像降噪方法

# Dlib

Dlib是一个C++写的机器学习库。

官网：

http://dlib.net/

代码：

https://github.com/davisking/dlib

参考：

https://mp.weixin.qq.com/s/QkjMY-qEqn2cGfYbFEmv5w

计算机视觉开源工具中的瑞士军刀—Dlib最新高级特性教程

https://mp.weixin.qq.com/s/TWzUTHXOzlLre4egBIkM_w

OpenCV vs Dlib人脸检测比较分析

https://mp.weixin.qq.com/s/bY5UIKPPvDE1XBC1LyoqPg

教你快速使用OpenCV/Python/dlib进行眨眼检测识别！

https://mp.weixin.qq.com/s/cgxTJ6X5ya6PDIfeJ9_z6Q

基于OpenCV与Dlib的行人计数开源实现

https://mp.weixin.qq.com/s/JOd9_80pZKzz77Ay5kvUeQ

PyImageSearch新出教程：Dlib多目标跟踪

https://mp.weixin.qq.com/s/hK6BJGAPyg_RqqilCcf2NA

全局自动优化：C++机器学习库dlib引入自动调参算法

https://zhuanlan.zhihu.com/p/45827914

Github开源人脸识别项目face_recognition

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
