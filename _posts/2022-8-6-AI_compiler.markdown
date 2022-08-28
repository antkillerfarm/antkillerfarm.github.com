---
layout: post
title:  NN中间语言, AI Compiler
category: toolchain 
---

* toc
{:toc}

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

# AI Compiler

## 概述

我们在《NN中间语言》一文中已经展示了各种中间语言/接口。

从目前来看，DL方面的中间语言/接口/编译器架构实在太多了。下图是Google（2019.4）推出的MLIR对自家各种优化技术的总结，这里还不包括其他家的相关技术。

![](/images/img2/MLIR.png)

从趋势来看，仅仅纠结于各种模型的导入/导出已经不再是最佳的做法，AI compiler才是王道。

---

某网友的评价：

Tensorflow Model / ONNX / Caffe Model / ... ---> DL IR (nGraph IR / *.IR) ---> LLVM IR ---> CPU JIT / GPU / ...

如果把前面的Model看成一种语言或者DSL，就是DSL ---> DL IR ---> LLVM IR ---> Target，然后你就在中间层疯狂的做优化，编译器优化开发也是这样做的。

在LLVM IR出现以前，很多编译器都有几层的IR表示，比如 C++ ---> 1st IR ---> OPT ---> 2nd IR ---> .... -> Target，只是LLVM出来以后，LLVM IR做了统一，编译器变为了 C++ ---> LLVM IR ---> OPT ---> LLVM IR ---> Target

![](/images/img3/IR.jpg)

---

Google（2020.9）又推出了IREE项目，定位和TVM类似。

官网：

https://google.github.io/iree/

同期，还有一个叫做TFRT的项目：

https://github.com/tensorflow/runtime

---

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

Tensor Operator Set Architecture (TOSA) Dialect

![](/images/img4/codegen-dialect-hierarchy.svg)



文档：

https://mlir.llvm.org/docs/Dialects/

---

ONNX MLIR:

http://onnx.ai/onnx-mlir

---

Torch-MLIR

![](/images/img4/torch_mlir.jpg)

https://blog.csdn.net/HaoBBNuanMM/article/details/124385542

Torch-MLIR技术详解

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

https://zhuanlan.zhihu.com/p/442140282

MLIR: A Brief Survey

https://zhuanlan.zhihu.com/p/545672504

MLIR原理与应用技术杂谈

# 并行 & 框架 & 优化+

https://mp.weixin.qq.com/s/UbZtUL6Iveb4S3nTU0liGw

深度神经网络的分布式训练概述：常用方法和技巧全面总结

https://mp.weixin.qq.com/s/kLXJsHbBnRIFC3NLChPhzA

如何高效进行大规模分类？港中文联合商汤提出新方法

https://mp.weixin.qq.com/s/F10UaaoxGPOE4pc59LBCRw

数据并行化对神经网络训练有何影响？谷歌大脑进行了实证研究

https://mp.weixin.qq.com/s/UF7DDenUQJ3bL83IHxOkIw

分布式优化算法及其在多智能体系统与机器学习中的应用

https://mp.weixin.qq.com/s/6h9MeBs89hTtWsYSZ4pZ5g

蚂蚁金服核心技术：百亿特征实时推荐算法揭秘

https://mp.weixin.qq.com/s/xV5cLbCPb7Nh6i4i7DxJIQ

没人告诉你的大规模部署AI高效流程！

https://mp.weixin.qq.com/s/8R7YhcZ_Dt0oFIF3bQovxw

为了提升DL模型性能，阿里工程师打造了流式编程框架

https://mp.weixin.qq.com/s/z6gXp-EeDID1ed8_DsUbOg

90秒训练AlexNet！商汤刷新纪录

https://mp.weixin.qq.com/s/HY2yPZ--Zm5_m3B70baWjQ

谷歌开源效率怪兽GPipe，速度提升25倍，CIFAR-10精度达到99%

https://mp.weixin.qq.com/s/HQW2bPyDY_3ecZWP6NYr-w

大规模机器学习在LinkedIn预测模型中的应用实践

https://mp.weixin.qq.com/s/i1PLA1xr3CefKx1EcVUVIg

谷歌破世界纪录！圆周率计算到小数点后31.4万亿位

https://mp.weixin.qq.com/s/rX8L63-jDGJT6lCAj04I3Q

独家解读！阿里重磅发布机器学习平台PAI 3.0

https://mp.weixin.qq.com/s/Ye2GVTFIrX3SbU1-4cDLoQ

你天天叫的外卖，你知道这里面深度学习的水有多深吗

https://mp.weixin.qq.com/s/FIWfbCLgckVzeNvfThIl4Q

阿里线下智能方案进化史

https://mp.weixin.qq.com/s/pqxiF6yEZzrw8qXu2hEsaA

单机训练速度提升640倍！独家解读快手商业广告模型GPU训练平台Persia

https://mp.weixin.qq.com/s/Jcz4XWDjMmbhmAiI_zBQXQ

流式计算优化：时效性

https://mp.weixin.qq.com/s/iAHvfgn54zIwfM9K8KFJnw

DLM：微信大规模分布式n-gram语言模型系统

https://mp.weixin.qq.com/s/s7sHzzLANOp8-1LxgXQskA

谷歌开发者大会上，蚂蚁金服开源ElasticDL分布式深度学习系统

https://mp.weixin.qq.com/s/IQMXg6nIJO-9-IG3mJpvRg

ElasticDL：同时提升集群利用率和研发效率的分布式深度学习框架

https://mp.weixin.qq.com/s/sn8fMAbJbeT6JUbCpBpN6A

Jeff Dean与David Patterson：不思考体系结构的深度学习研究者不是好工程师

https://mp.weixin.qq.com/s/6zLrWJ4nE0bHFlVe5dMxHw

分布式深度学习新进展：让“分布式”和“深度学习”真正深度融合

https://mp.weixin.qq.com/s/hjC-WTMIpbWWpmXoLBfD2g

腾讯大规模分布式机器学习系统无量是如何进行技术选型的？

https://mp.weixin.qq.com/s/mg-d1W5i9rzaLMNrvq0tSQ

32分钟训练神经机器翻译，速度提升45倍

https://mp.weixin.qq.com/s/iW0k80TUPuWDE9xwHvX91g

为什么你需要Raven：全球首个真正分布式深度学习训练协议

https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650750181&idx=1&sn=156dac3c5646143fc2577972f1506836

GPU捉襟见肘还想训练大批量模型？谁说不可以

https://mp.weixin.qq.com/s/-CTVyKWtdTK0RIfzzPVyNQ

分布式与高效深度学习，140页ppt详述深度学习压缩与联邦学习训练技术进展

https://mp.weixin.qq.com/s/nTBuYuW7h9wZYuo3w1xGmQ

分布式训练的方案和效率对比

https://mp.weixin.qq.com/s/LOTQfD9KKtAq0zz4rObCGA

EB级系统空中换引擎：阿里调度执行框架如何全面升级？（DAG 2.0）

https://zhuanlan.zhihu.com/p/276122469

分布式训练常用技术简介

https://mp.weixin.qq.com/s/uQzwqcGwC9ZveuW64Lzkmg

分布式训练怎么还减速了呢？

https://zhuanlan.zhihu.com/p/294698838

DLPerf—分布式深度学习最佳入门(踩坑)指南

https://zhuanlan.zhihu.com/p/76638962

Pytorch分布式训练

https://zhuanlan.zhihu.com/p/360405558

PyTorch分布式训练

https://mp.weixin.qq.com/s/0aSBHvscloEnPMRLyNjQsg

PyTorch分布式训练简明教程

https://blog.csdn.net/orangerfun/article/details/123887725

torch分布式训练

https://mp.weixin.qq.com/s/r7kt1k7D1wurWs_uxdLCtg

PyTorch源码解读之分布式训练

https://mp.weixin.qq.com/s/_85oWK2plv2QOX5Qfg_-ZA

大规模机器学习优化，195页ppt与视频

https://mp.weixin.qq.com/s/soruo90Dbtzi6d1kA63Akg

阿里提出智能算力引擎DCAF，节省20%GPU算力

https://mp.weixin.qq.com/s/oDak7peTT5ynNYrH7LSWTg

分布式层次GPU参数服务器架构

https://zhuanlan.zhihu.com/p/28226956

浮点峰值那些事儿

https://zhuanlan.zhihu.com/p/285994980

针对深度学习的GPU共享

https://mp.weixin.qq.com/s/Np4w7RC2JFlB7ZGIduu71w

爱奇艺机器学习平台的建设实践

https://mp.weixin.qq.com/s/9k6PDusoDHjmz58HAZxZcw

GPipe: 小批量流水线带来的大模型训练

https://mp.weixin.qq.com/s/DwjvEn04lGzKU8mDu-5q4g

大幅提升训练性能，字节跳动与清华提出新型分布式DNN训练架构
