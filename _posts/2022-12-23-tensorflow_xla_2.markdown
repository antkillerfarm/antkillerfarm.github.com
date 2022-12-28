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

## IR & pass

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

---

HLO可以有pass：`HloModulePass`

```cpp
HloPassPipeline pipeline("pass");
pipeline.AddPass<XXXPass>();
```

和TVM相仿，MatchPattern仍然是最简单的一类Pass。HLO的实现叫做`HloMatcher`。

替换可以使用`ReplaceWithNewInstruction`。

ConstFolding、AlgebraicSimplifier、HloCSE、HloDCE、TransposeFolding

上层的XLA graph也有pass：GraphOptimizer

tensorflow/core/common_runtime/graph_optimizer.cc

https://wzzju.github.io/tensorflow/xla/2021/12/23/xla-pass/

XLA Pass功能分析

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
