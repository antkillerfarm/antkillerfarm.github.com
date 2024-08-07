---
layout: post
title:  TF XLA（二）
category: DL Framework 
---

* toc
{:toc}

# XLA

## system op

除了运算和控制流的op之外，tf中还存在相当数量的用于执行框架功能的system op。

例如：

`_Arg`: 计算图的输入。

`_Retval`: 计算图的输出。

`AssignAddVariableOp`: 一般用于给`_Arg`搬运数据。

`IteratorGetNext`: 启动下一个Iterator。主要是给graph准备训练数据和标签。

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

```bazel
cc_library(
    name = "xla_ops",
    srcs = ["xla_ops.cc"],
    deps = ["//tensorflow/core:framework"],
    alwayslink = 1,
)

tf_gen_op_wrapper_py(
    name = "xla_ops_wrapper_py",
    out = "xla_ops.py",
    deps = ["//tensorflow/compiler/jit/ops:xla_ops"],
)
```

有两个xla_ops.cc，一个在ops下，另一个在kernels下，前者是声明，后者是实现。

tf_gen_op_wrapper_py之后，会生成xla_ops.py。

`gen_xla_ops.xla_dot_v2()`

这样就可以通过python调用C++。

## IR

XLA IR一般如下所示：

```text
n121 = Identity[T=float, device=NPU:0](n98) @ n119
```

`[]`内是属性，`()`内是操作数，`@`表示要等到后面的操作数都做完了，才能开始执行。

---

HLO IR一般如下所示：

```text
%convolution = f32[16,24,24,6]{3,2,1,0} convolution(f32[16,1,28,28]{3,2,1,0} %transpose, f32[6,1,5,5]{3,2,1,0} %transpose.1), window={size=5x5}, dim_labels=bf01_oi01->b01f, metadata={op_type="Conv2D" op_name="sequential/conv2d/Conv2D"}
```

`[]`内是shape，`()`内是layout。

---

打印HLO subgraph的输入，在XLA graph中的名字：

```
  for (int i = 1; i < ctx->num_inputs(); ++i) {
    LOG(INFO) << "XlaRunOp::Compute input : " << requested_input(i);
  }
```

## HLO pass

HLO可以有pass：`HloModulePass`

```cpp
HloPassPipeline pipeline("pass");
pipeline.AddPass<XXXPass>();
```

和TVM相仿，MatchPattern仍然是最简单的一类Pass。HLO的实现叫做`HloMatcher`。

替换可以使用`ReplaceWithNewInstruction`。

ConstFolding、AlgebraicSimplifier、HloCSE、HloDCE、TransposeFolding

---

`export XLA_FLAGS="--xla_dump_to=/some/path --xla_dump_hlo_pass_re=.* --xla_dump_hlo_as_html"`

- xla_dump_to: 希望生成的中间表示存放在哪里。
- xla_dump_hlo_pass_re: 默认xla是不会导出hlo内部pass的，但是使用这个选项后可以导出对应的pass，.*表示所有pass

---

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

---

http://zhengsz.tech/2019/10/28/XLA%E6%8E%A2%E7%A9%B6-%E7%9F%A9%E9%98%B5%E4%B9%98%E6%B3%95/

XLA探究：矩阵乘法

上文表示：XLA的图优化很保守，至少难以发掘图上运算模式规约的可能性。其实也就是没有识别出写的graph实际上是一个矩阵乘法。

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

## Binary Cache

NV的Binary被保存为.ptx文件，其调用的stack大致如下：

```cpp
Service::BuildExecutable
GpuCompiler::RunBackend
GpuCompiler::CompileToBackendResult
GpuCompiler::CompileModuleToLlvmIr
GpuCompiler::CompileToTargetBinary
GpuCompiler::CompileSingleModule
NVPTXCompiler::CompileTargetBinary
```

加载：

```cpp
MaybeLoadPtxFromFile
```

保存文件的名字，使用xla::FilenameFor函数获得，其实就是hlo module的ID。

## Other

XLA在内的主流深度学习框架，都是基于Static Shape语义的编译器框架。即，just-in-time运行的编译器，会在运行时捕捉待编译子图的实际输入shape组合，并且为每一个输入shape组合生成一份编译结果。

---

Reduce算子的定义实际上是一个递归定义，对于输入`[10, 11, 12, 13]`，它的计算公式为：

`f(10, f(11, f(12, f(init_value, 13)))`

所以你会发现`f`的定义一般是这样的：

`(x: pred[], y: pred[]) -> pred[]`

参数和返回值都是同一类型，而且都是单个元素，而非整个tensor。

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
