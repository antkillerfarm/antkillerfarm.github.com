---
layout: post
title:  TF XLA（二）
category: DL Framework 
---

* toc
{:toc}

# XLA

## op register & filter（续）

上述OpFilter开关一旦打开，则所有计算无论计算量的大小，都会被派发到NPU上，然而有些计算图实在过于微小，在CPU上的计算时间，甚至小于调度到NPU上的时间。

针对这个问题，在graphcore的tf实现中，有LiteralEvaluateForScalarElementwiseGraph函数，用于回退到CPU上。其关键点在于：

```cpp
  HloEvaluator hlo_evaluator(1);
  TF_ASSIGN_OR_RETURN(Literal literal_evaluate,
                      hlo_evaluator.Evaluate(*comp, arg_literals));
```

HloEvaluator中已经有了一套默认的CPU实现。

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

## Pytorch XLA

Pytorch官方提供了如下项目支持XLA：

https://github.com/pytorch/xla

这个项目实际上是Google来维护的。

粗看了一下，都是些上层的代码，底层直接调用TF的实现。所以如果目标硬件已经接入TF XLA接口的话，理论上不需要修改就可以跑pytorch。

![](/images/img5/pytorch_xla.png)

文档：

https://pytorch.org/xla/master/

PyTorch on XLA Devices

示例：

https://github.com/pytorch/xla/blob/master/test/test_train_mp_mnist.py

编译：

```bash
git clone --recursive https://github.com/pytorch/xla.git
cd xla

# modify bazel/dependencies.bzl to config pytorch:
# PYTORCH_LOCAL_DIR = "../pytorch"

# modify WORKSPACE to config openxla:
# by commenting out the http_archive above and uncommenting the following:
# local_repository(
#    name = "xla",
#    path = "/path/to/openxla",
# )

export XLA_CUDA=0
export BUNDLE_LIBTPU=0
python setup.py install
```

---

Backend的参考实现：

Intel XPU:

https://github.com/intel/intel-extension-for-openxla

IREE:

https://github.com/openxla/openxla-pjrt-plugin

---

Mesh API：

![](/images/img5/Mesh_API.png)

https://pytorch.org/blog/pytorch-xla-spmd

PyTorch/XLA SPMD: Scale Up Model Training and Serving with Automatic Parallelization

https://www.openteams.com/large-scale-training-of-hugging-face-transformers-on-tpus-with-pytorch-xla-fsdp

Large Scale Training of Hugging Face Transformers on TPUs With PyTorch/XLA FSDP

---

有时候conda环境里使用的gcc版本，和本地的gcc版本有差异，会导致编译产生的py库，有一些问题：

https://blog.csdn.net/weixin_39379635/article/details/129159713

如何解决version `GLIBCXX_3.4.29‘ not found的问题

类似的，还有库找不到的问题，一般用LD_LIBRARY_PATH解决之。

---

https://blog.csdn.net/qq_40329272/article/details/111801695

undefined symbol:_ZN5torchXXXX

---

打印网络信息：

torch_xla/torch_xla/core/dynamo_bridge.py:

`def extract_compiled_graph(xla_model: torch.fx.GraphModule, xla_args):`

To print a tabular representation of the graph, use:

`xla_model.graph.print_tabular()`

To get a SVG visualization of the graph, use:

```python
from torch.fx.passes.graph_drawer import FxGraphDrawer
drawer = FxGraphDrawer(xla_model, "model_name")
with open(f"svg/save/dir/{drawer._name}.svg", mode="wb") as f:
    f.write(drawer.get_dot_graph().create_svg())
```

---

参考：

https://pytorch.org/blog/pytorch-2.0-xla/

PyTorch 2.0 & XLA—The Latest Cutting Edge Features

https://huggingface.co/blog/pytorch-xla

Hugging Face on PyTorch / XLA TPUs: Faster and cheaper training

https://pytorch.org/tutorials/recipes/intel_extension_for_pytorch.html

INTEL EXTENSION FOR PYTORCH

## OpenXLA

2022.10 Google又创建了一个新的OpenXLA项目，旨在将XLA从TF中解耦。

官网：

https://github.com/openxla/xla

![](/images/img5/TPU_XLA.png)

## XRT & PJRT

PJRT：Pretty much Just another RunTime

TF的代码中有如下两个文件夹：

tensorflow/compiler/xla/xrt

tensorflow/compiler/xla/pjrt

XRT & PJRT的作用是：为其他框架如Pytorch/JAX提供生成XLA IR，并执行的能力。

PJRT是XRT的升级版。

参考：

https://github.com/openxla/xla/blob/main/xla/pjrt/c/docs/pjrt_integration_guide.md

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
