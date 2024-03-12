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

# XLA+

## AutoClustering（续）

https://sketch2sky.com/2019/09/24/tensorflow-jit-%E6%8A%80%E6%9C%AF%E8%AF%A6%E8%A7%A3/

Tensorflow JIT技术详解

https://blog.csdn.net/gaofeipaopaotang/article/details/80679100

模型优化之XLA（上）

https://blog.csdn.net/gaofeipaopaotang/article/details/80703367

模型优化之XLA（下）

https://blog.csdn.net/weixin_41644391/article/details/120948964

MarkForCompilationPass

https://blog.csdn.net/weixin_41644391/article/details/120949032

EncapsulateSubgraphsPass

## Grappler

Grappler是TensorFlow运行时中的默认计算图优化系统。

Grappler生成的计算图，会做为XLA的输入。

https://www.tensorflow.org/guide/graph_optimization

使用Grappler优化TensorFlow计算图

https://blog.csdn.net/gaofeipaopaotang/article/details/80621902

模型优化之Grappler

## 代码生成

如果是CPU/GPU的话，一般会用LLVM生成代码。

xla.cpu.IrEmitter，将xla.HloModule中的每个xla.HloComputation转化为llvm IR表示，并创建对应的llvm.Module。

如果是DSA的话，一般采用直接代理HLO IR的模式。

xla.DfsHloVisitorBase会遍历整个Cluster。

## FSDP for torch_xla

FSDP的基本概念，参见《并行 & 框架 & 优化（四）》。

分布式，包括FSDP的测试用例主要在：

- test/test_train_mp_mnist.py
- test/test_train_mp_mnist_zero1.py
- test/test_train_mp_mnist_fsdp_with_ckpt.py

XlaFullyShardedDataParallel

参考：

https://pytorch.org/blog/scaling-pytorch-models-on-cloud-tpus-with-fsdp/

Scaling PyTorch models on Cloud TPUs with FSDP

---

FSDP的计算图会随时根据需要，创建/删除weight，如果不加特殊处理的话，后续优化环节的subexpression elimination (CSE) ，会将这些操作优化掉，所以XLA引入了OptimizationBarrier算子来阻止这个优化。

## Other

XLA在内的主流深度学习框架，都是基于Static Shape语义的编译器框架。即，just-in-time运行的编译器，会在运行时捕捉待编译子图的实际输入shape组合，并且为每一个输入shape组合生成一份编译结果。

## JAX

一款由谷歌团队打造（非官方发布），用于从纯Python和Numpy机器学习程序中生成高性能加速器（accelerator）代码，且特定于域的跟踪JIT编译器。

代码：

https://github.com/google/jax

文档：

https://jax.readthedocs.io/en/latest/

JAX的底层也是基于XLA的。

JAX并不是TF的替代品，它缺失了一些数据准备和调度的功能。这些功能一般可用haiku/flax提供。

RLax：这是一个基于Jax的强化学习库。

参考：

https://mp.weixin.qq.com/s/IMMdbF33ZHEz7N_XwgIhHA

试试谷歌这个新工具：说不定比TensorFlow还好用！

https://mp.weixin.qq.com/s/tZ3yWQ9--l9e81UqoUoWIQ

要替代TensorFlow？谷歌开源机器学习库JAX

https://mp.weixin.qq.com/s/eaYwiV2LZNRwzPEeOA1XFg

新星JAX ：双挑TensorFlow和PyTorch！有望担纲Google主要科学计算库和神经网络库

https://mp.weixin.qq.com/s/NhMbr_niHjSaqh2azuSaog

只知道TF和PyTorch还不够，快来看看怎么从PyTorch转向自动微分神器JAX

https://wzzju.github.io/jax/xla/2022/02/17/jax-cpp/

JAX程序转HLO执行

https://zhuanlan.zhihu.com/p/532504225

面向PyTorch用户的JAX简易教程(1): JAX介绍

https://zhuanlan.zhihu.com/p/544216783

面向PyTorch用户的JAX简易教程(2): 如何训练一个神经网络

## 参考

https://mp.weixin.qq.com/s/RO3FrPxhK2GEoDCGE9DXrw

利用XLA将GPU性能推向极限

https://mp.weixin.qq.com/s/MPI9KERDS-Al4DTBDRV04w

TensorFlow XLA工作原理简介

https://sketch2sky.com/

一个XLA方面的blog

https://tensorflow.juejin.im/performance/xla/jit.html

使用即时编译

https://blog.slinuxer.com/2019/06/tensorflow-xla

TensorFlow XLA初步接触

https://github.com/horance-liu/tensorflow-internals

电子书《TensorFlow Internals》

https://wzzju.github.io/tensorflow/xla/2021/06/12/xla-overview/

XLA编译执行原理分析

https://haosdent.gitbooks.io/tensorflow-document/content/resources/xla_prerelease.html

XLA: The TensorFlow compiler framework

http://zhengsz.tech/2019/10/23/Tensorflow-XLA-%E6%8E%A2%E7%A9%B6/

Tensorflow/XLA探究

https://wzzju.github.io/tensorflow/xla/2021/06/12/xla-overview/

XLA编译执行原理分析
