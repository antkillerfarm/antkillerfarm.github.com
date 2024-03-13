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

## 教程

https://dlsyscourse.org/lectures/

10-414/714: Deep Learning Systems

https://mlc.ai/

Machine Learning Compilation

这两个课程都是陈天奇主讲的。

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

https://github.com/openxla/iree

![](/images/img5/iree_architecture.svg)

同期，还有一个叫做TFRT的项目：

https://github.com/tensorflow/runtime

---

ML编译器核心问题主要有三部分：auto-tensorize、auto-tiling和auto-schedule。

---

High-level IR（图层IR），如XLA的HLO，TVM的Relay IR以及MindSpore的MindIR等，重点关注非循环相关的优化。除了传统编译器中常见的常量折叠、代数化简、公共子表达式等优化外，还会完成Layout转换，算子融合等优化，通过分析和优化现有网络计算图逻辑，对原有计算逻辑进行拆分、重组、融合等操作，以减少算子执行间隙的开销并且提升设备计算资源利用率，从而实现网络整体执行时间的优化。

Low-level IR，如TVM的TIR，HalideIR，以及isl schedule tree等。针对Low-level IR主要有循环变换、循环切分等调度相关的优化，与硬件intrinsic映射、内存分配等后端pass优化。其中，当前的自动调度优化主要包含了基于搜索的自动调度优化（如ansor）和基于polyhedral编译技术的自动调度优化（如TC和MindAKG）。

---

![](/images/img5/Dynamic_Shape_Compiler.jpg)

https://zhuanlan.zhihu.com/p/305546437

AI编译优化--Dynamic Shape Compiler

---

图算融合：大算子怎样打开、小算子如何重新融合。

https://zhuanlan.zhihu.com/p/629048218

AI编译器和推理引擎的区别

---

https://mp.weixin.qq.com/s/jjT0x99ht8xtfWmzL-0R1A

深度学习的IR“之争”

https://mp.weixin.qq.com/s/G36IllLOTXXbc4LagbNH9Q

编译器与IR的思考: LLVM IR，SPIR-V到MLIR

https://www.zhihu.com/question/391811802

如何评价TensorFlow开源的新运行时TFRT？

https://zhuanlan.zhihu.com/p/347599203

TFRT的开源代码分析

https://zhuanlan.zhihu.com/p/600622073

AI编译器之后端优化

## OpenAI Triton

一个类似于TVMscript的可以通过python语法去写高性能GPU程序的库。注意不要和NVIDIA Triton搞混了。后者是一个AI推理框架。

NVIDIA Triton Inference Server（此前称为TensorRT Inference Server）能够帮助开发人员和IT/DevOps轻松地在云端、本地数据中心或边缘部署高性能推理服务器。

官网：

https://github.com/openai/triton

参考：

https://zhuanlan.zhihu.com/p/394377526

OpenAI开源GPU编程语言Triton，将同时支持N卡和A卡

https://zhuanlan.zhihu.com/p/613244988

谈谈对OpenAI Triton的一些理解

## NVFuser

NVFuser是NV专门为Pytorch设计的自动化的GPU代码生成器。

## AKG

AKG是面向华为昇腾AI处理器的张量编译器。

官网：

https://gitee.com/mindspore/akg/

![](/images/img5/akg-design.png)
