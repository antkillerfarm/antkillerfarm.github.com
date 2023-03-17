---
layout: post
title:  TF XLA（二）
category: DL Framework 
---

# XLA

## 混合backend

XLA支持混合多backend的运行，可用`tf.debugging.set_log_device_placement(True)`查看相关的设备指派信息。

设备指派主要由Placer模块负责：

https://www.cnblogs.com/deep-learning-stacks/p/9823486.html

TensorFlow中的Placement启发式算法模块——Placer

tensorflow/core/common_runtime/placer.cc

`tf.config.set_soft_device_placement(True)`能让tensorflow遇到无法用GPU跑的数据时，自动切换成CPU进行。

设备指派之后，就是根据设备指派的信息，切割计算图了。

https://www.cnblogs.com/deep-learning-stacks/p/10054529.html

TensorFlow的图切割模块——Graph Partitioner

tensorflow/core/graph/graph_partition.cc

## StreamExecutor

StreamExecutor是Google内部为并行编程模型开发的库。TensorFlow中的StreamExecutor是StreamExecutor的开源简版。

https://www.cnblogs.com/deep-learning-stacks/p/9386188.html

TensorFlow中的并行执行引擎——StreamExecutor框架

## backend优先级

`REGISTER_LOCAL_DEVICE_FACTORY(DEVICE_XLA_XXX_NPU, XlaXXXNpuDeviceFactory, 500);`

CPU的优先级是50，添加的backend的优先级只要大于50，就可以得到调度权。

## unit test

写好的backend需要测试，同时Unit Test也是编写一个backend的入门级入口。

```bash
bazel build -c dbg tensorflow/compiler/xla/tests:convolution_variants_test
./bazel-bin/tensorflow/compiler/xla/tests/convolution_variants_test_cpu --gtest_filter=*BackwardInputLowPaddingLessThanHighPadding*
```

## python/C++ wrapper

python: `pywrap_tfe.TFE_Py_Execute`

C++: `TFE_Py_Execute`

## Pytorch XLA

Pytorch官方提供了如下项目支持XLA：

https://github.com/pytorch/xla

粗看了一下，都是些上层的代码，底层直接调用TF的实现。所以如果目标硬件已经接入TF XLA接口的话，理论上不需要修改就可以跑pytorch。

![](/images/img5/pytorch_xla.png)

## IR

XLA IR一般如下所示：

```text
n121 = Identity[T=float, device=NPU:0](n98) @ n119
```

`[]`内是属性，`()`内是操作数，`@`表示要等到后面的操作数都做完了，才能开始执行。

HLO IR一般如下所示：

```text
%convolution = f32[16,24,24,6]{3,2,1,0} convolution(f32[16,1,28,28]{3,2,1,0} %transpose, f32[6,1,5,5]{3,2,1,0} %transpose.1), window={size=5x5}, dim_labels=bf01_oi01->b01f, metadata={op_type="Conv2D" op_name="sequential/conv2d/Conv2D"}
```

`[]`内是shape，`()`内是layout。

## HLO pass

HLO可以有pass：`HloModulePass`

```cpp
HloPassPipeline pipeline("pass");
pipeline.AddPass<XXXPass>();
```

和TVM相仿，MatchPattern仍然是最简单的一类Pass。HLO的实现叫做`HloMatcher`。

替换可以使用`ReplaceWithNewInstruction`。

ConstFolding、AlgebraicSimplifier、HloCSE、HloDCE、TransposeFolding

参考：

https://wzzju.github.io/tensorflow/xla/2021/12/23/xla-pass/

XLA Pass功能分析

## XLA pass

在分析细节之前，我们首先给一下debug这些pass的手段：

```bash
export TF_XLA_FLAGS="--tf_xla_clustering_debug"
export TF_DUMP_GRAPH_PREFIX=./dump_graph
```

---

上层的XLA graph有pass：GraphOptimizer

tensorflow/core/common_runtime/graph_optimizer.cc

jit compilation这样的构图阶段也有pass：

tensorflow/compiler/jit/jit_compilation_pass_registration.cc

一个典型的pass的注册如下所示：

```cpp
REGISTER_OPTIMIZATION(OptimizationPassRegistry::POST_REWRITE_FOR_EXEC, 10,
                      MarkForCompilationPass);
```

第一个参数表示是什么阶段的pass。目前有以下几个阶段：

```cpp
    PRE_PLACEMENT,          // after cost model assignment, before placement.
    POST_PLACEMENT,         // after placement.
    POST_REWRITE_FOR_EXEC,  // after re-write using feed/fetch endpoints.
    POST_PARTITIONING,      // after partitioning
```

同一个阶段的pass，使用优先级（上面的第二个参数）确定执行顺序。所有pass都必须走一遍，不能条件执行。

在`PRE_PLACEMENT`和`POST_PLACEMENT`之间会插入`placer.Run()`。在`POST_REWRITE_FOR_EXEC`和`POST_PARTITIONING`之间会插入`PartitionFunctionGraph`。

必须指出的是，这些阶段的划分都是人为的。比如`POST_PLACEMENT`和`POST_REWRITE_FOR_EXEC`之间并无任何其他操作，pass放在哪里，对于结果都没有任何影响。

## AutoClustering

1.CloneConstantsForBetterClusteringPass, ClusterScopingPass都是一些准备工作，方便MarkForCompilationPass的处理。

2.MarkForCompilationPass负责寻找合适的Cluster, 并为找到的同一个Cluster内所有节点设置同样的_xla_compile_id属性。

3.ForceXlaConstantsOnHostPass, IncreaseDynamismForAutoJitPass, PartiallyDeclusterPass则是对MarkForCompilationPass的结果进行微调。

4.EncapsulateSubgraphsPass将每个Cluster内的多个节点替换为单个节点，但在单个节点内记录了Cluster内子图的信息。

5.将EncapsulateSubgraphsPass融合的节点替换为XlaCompileOp + XlaRunOp两个算子。

https://zhuanlan.zhihu.com/p/427444916

Tensorflow编译加速器XLA源码深入解读

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

# Polyhedral Model+++

https://mp.weixin.qq.com/s/QEooKxP1sm5O90AUiqKQEQ

Polyhedral Model—AI芯片软硬件优化利器（一）

https://mp.weixin.qq.com/s/NRtud1UImE5ArZ2zQWFRyg

Polyhedral Model—AI芯片软硬件优化利器（二）

https://mp.weixin.qq.com/s/bLBIrJb82IsnyoXSEr2xtw

Polyhedral Model—AI芯片软硬件优化利器（三）

https://www.cnblogs.com/XianghuanHe/articles/15432300.html

编译原理（龙书）第十一章 并行和局部优化学习笔记

https://mp.weixin.qq.com/s/wjk2Mxhd2NDpH72l3rdqgQ

多面体编译技术在软硬协同设计中的应用

https://mp.weixin.qq.com/s/mBheJ9NG8khcLRshI40b2w

AI编译关键技术 • 高层循环编译优化 - 不仅仅是分块和合并

https://zhuanlan.zhihu.com/p/199683290

Polyhedral编译调度算法(1)——Pluto算法

https://zhuanlan.zhihu.com/p/232070003

Polyhedral编译调度算法(2)——Feautrier算法

https://zhuanlan.zhihu.com/p/259311866

Polyhedral编译调度算法(3)——isl中的调度算法

# TensorFlow参考+

https://mp.weixin.qq.com/s/kEowgNPVS1nAGBPbzkatlQ

如何构建高可读性和高可重用的TensorFlow模型

https://mp.weixin.qq.com/s/O_IN39FBVPeD5fRYBsPuZQ

用TensorFlow开发问答系统

https://mp.weixin.qq.com/s/8Hrq_z8s_5ms6Q_6OOaU-g

如何使用TensorFlow和自编码器模型生成手写数字

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
