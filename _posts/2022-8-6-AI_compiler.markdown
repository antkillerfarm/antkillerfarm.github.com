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

https://zhuanlan.zhihu.com/p/513872467

面向ASIC设备的编译器框架：TVM or MLIR？

# 编译原理+

## PPCG

Polyhedral Parallel Code Generation

代码：

https://github.com/Meinersbur/ppcg

## PLUTO

An automatic parallelizer and locality optimizer for affine loop nests

官网：

http://pluto-compiler.sourceforge.net/

参考：

https://www.zhihu.com/question/329294933

如何评价PLUTO编译器？

## ANTLR

ANTLR—Another Tool for Language Recognition，其前身是PCCTS，它为包括Java，C++，C#,python在内的语言提供了一个通过语法描述来自动构造自定义语言的识别器（recognizer），编译器（parser）和解释器（translator）的框架。

官网：

http://www.antlr.org/

参考：

http://yuzhouwan.com/posts/55501/

Antlr

https://www.ibm.com/developerworks/cn/java/j-lo-antlr/index.html

使用Antlr开发领域语言

## MPS

MPS是jetbrains推出的用于构建DSL的工具。

官网：

https://www.jetbrains.com/mps/

## 参考

x86一开始并没有使用太多的通用寄存器，原因之一（注意，只是之一）是当时的编译器无力进行寄存器分配，让编译器自动决定程序中众多变量哪些应该装入寄存器哪些应该换出、哪些变量应该映射到同一个寄存器上，并不是一件易事，JVM采用堆栈结构的原因之一就是不信任编译器的寄存器分配能力，转而使用堆栈结构，躲开寄存器分配的难题。

到80年代早期，IBM的G. J. Chaitin公开了他们的图染色寄存器分配算法之后，编译器的分配能力获得长足进步，形成了现在这样的编译器主导的寄存器分配格局。

https://www.zhihu.com/question/24551779

为什么ARM和MIPS那么多寄存器，x86那么少？

---

https://mp.weixin.qq.com/s/MqfteZBSWbnBpHbFYw8Eqw

如何编写一个简单的Python编译器

https://zhuanlan.zhihu.com/p/28637279

使用LLVM+PLY实现一个C语言子集的玩具编译器

https://mp.weixin.qq.com/s/7wmBsJgPnOtPXcYaoQd1qA

基于LLVM的源码级依赖分析方案的设计与实现

https://mp.weixin.qq.com/s/vOJPxzH_1SUyXzNeE85zHQ

编译器入门：没有siri的那些年，我们如何实现人机对话？

https://zhuanlan.zhihu.com/p/66793637

A Tour to LLVM IR（上）

https://zhuanlan.zhihu.com/p/66909226

A Tour to LLVM IR（下）

https://mp.weixin.qq.com/s/4FJzxPyCmakjIU-9xlQmJQ

阁下可知文言编程之精妙？CMU本科生开源文言文编程语言，数天2K星

https://mp.weixin.qq.com/s/7PH8o1tbjLsM4-nOnjbwLw

Java即时编译器原理解析及实践

https://mp.weixin.qq.com/s/s2W_VVlS-UC8PaaVYJlNgw

浅谈编译过程

https://mp.weixin.qq.com/s/j8_8QwFnyOr66m9aekor1g

用JS解释JS！详解AST及其应用

https://zhuanlan.zhihu.com/p/471250907

从零开始，写个编译器！

https://www.zhihu.com/question/34619258

现代c++编译器对临时对象做了怎样的优化？

https://zhuanlan.zhihu.com/p/474324656

我对深度学习编译器和框架的认识

# Build tools+

## OkBuck

OkBuck是Uber推出的构建工具。

官网：

https://github.com/uber/okbuck

## WAF

WAF是一个python写的构建工具。

官网：

https://waf.io

## vcpkg

这是MS提供的一个C/C++包管理工具，一般配合CMake使用。支持平台包括Windows/Linux/MacOS。

官网：

https://github.com/Microsoft/vcpkg
