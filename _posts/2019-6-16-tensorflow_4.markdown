---
layout: post
title:  TensorFlow（四）
category: DL Framework 
---

* toc
{:toc}

# Debug

## VS Code + gdb

- 设置python

Setting中搜索`python path`，设置路径类似于：`/anaconda3/envs/mlbook/bin/python`

打开python文件，在状态栏有python版本的提示，点击该提示，可以切换不同的python版本。

- gdb调试

Tensorflow App，一般是从python开始的，因此需要掌握python+C的混合调试方法。

在所有模块import之后（否则后面的gdb加载不到相关的符号），添加如下语句：

`input("pid: " + str(os.getpid()) +", press enter after attached")`

启动gdb，使用attach命令，attach到相关的进程。设置断点，然后continue即可。

参考：

https://sketch2sky.com/2019/08/25/tensorflow-debugtrick/

Tensorflow XLA Debug/Profiling Methods

https://www.cnblogs.com/djzny/p/4956752.html

gdb命令中attach使用

https://wzzju.github.io/tensorflow/gdb/2020/03/17/tf-compile/

使用gdb调试并阅读TensorFlow源码

- vscode调试

vscode调试同样需要两段式的方法：

https://nadiah.org/2020/03/01/example-debug-mixed-python-c-in-visual-studio-code/

Example debugging mixed Python C++ in VS Code

相关配置文件参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/vscode/launch.json

## 打印stack trace

C++：
```cpp
#include "tensorflow/core/platform/stacktrace.h"
tensorflow::CurrentStackTrace()
```

Python：`tf.debugging.disable_traceback_filtering()`

## Log

TF里有两套Log系统：`LOG`和`VLOG`。

`LOG`由`TF_CPP_MIN_LOG_LEVEL`控制，值越小，信息越多。

`VLOG`都是INFO级别的Log，因此，`TF_CPP_MIN_LOG_LEVEL`必须为0。此外，`VLOG`本身亦有不同等级，可使用`TF_CPP_MIN_VLOG_LEVEL`控制，值越大，信息越多。

# TensorBoard

TensorBoard是一个http服务，用以监控TensorFlow的执行。

`writer = tf.summary.FileWriter("logs/", sess.graph)`

然后

`tensorboard --logdir='logs/'`

启动之后，用浏览器打开`http://localhost:6006`即可。

如果想在局域网中用另一台PC访问TensorBoard服务，则可：

`tensorboard --logdir='logs/' --host=0.0.0.0 --port=1234`

TensorBoard会将同类结点Group，但Group之后，有时反而不易观察具体的结构。这个时候最好Ungroup一下。

参考：

https://neptune.ai/blog/tensorboard-tutorial

Deep Dive Into TensorBoard: Tutorial With Examples

http://blog.csdn.net/u013082989/article/details/53510625

TensorFlow学习_01_安装_基本操作_可视化结构、过程_Mnist

https://blog.csdn.net/sinat_33761963/article/details/62433234

Tensorflow的可视化工具Tensorboard的初步使用

https://mp.weixin.qq.com/s/Zaz9hmTuUbd-hCx-zHhBgg

TensorBoard：可视化学习

https://www.cnblogs.com/deepllz/p/9207981.html

Tensorboard数据(tfevents文件)格式解析及ofstream使用问题

https://mp.weixin.qq.com/s/Kc-DqiuG2kn0NlVxkcNa4w

TensorBoard直方图信息中心

https://mp.weixin.qq.com/s?__biz=MzU2OTA0NzE2NA==&mid=2247515390&idx=2&sn=ebf548bac3c7db9b0174265666c67d0c

tensorboard学习笔记

https://mp.weixin.qq.com/s/JRa0tgXtGdzaj0UnYcmZ3Q

tensorboard指南

https://mp.weixin.qq.com/s/BAR-UM3rTveYrKa4kiJvcQ

使用TensorBoard进行超参数优化

https://mp.weixin.qq.com/s/5zfKiP9Fxpl7suqBQILL-g

还在用Tensorboard？机器学习实验管理平台大盘点

https://mp.weixin.qq.com/s/8scMr0jcW87y6k_wFgOBEg

使用Tensorboard投影进行高维向量的可视化

# Profiling

## 查看

文档：

https://tensorflow.google.cn/guide/profiler

Optimize TensorFlow performance using the Profiler

https://tensorflow.google.cn/tensorboard/tensorboard_profiling_keras

TensorFlow Profiler: Profile model performance

安装：

`pip install -U tensorboard-plugin-profile`

代码：

```python
from tensorflow.profiler.experimental import Profile

with Profile('/logdir_path'):
    # do sth
```

```python
# Create a TensorBoard callback
logs = "logs/" + datetime.now().strftime("%Y%m%d-%H%M%S")

tboard_callback = tf.keras.callbacks.TensorBoard(log_dir = logs,
                                                 histogram_freq = 1,
                                                 profile_batch = '500,520')

model.fit(ds_train,
          epochs=2,
          validation_data=ds_test,
          callbacks = [tboard_callback])
```

查看步骤：

1.打开TensorBoard之后，右上角下拉中选择`PROFILE`。

2.左侧的`Tools`下拉中，有好多工具。其中`tensorflow_stats`和`trace_viewer`比较重要。

trace可以保存在本地，也可以用grpc输出到远程，并通过`trace_viewer`中的`Capture Profile`按钮来接收。

---

Trace Viewer是Google的Chromium项目开发的一个强大的可视化展示和分析工具。TensorBoard中也使用了它。

Trace Viewer有一套自己的Trace Event Format，只要文件遵循这个格式，就可以被展示。

查看方法：

chrome://tracing/

或者

https://ui.perfetto.dev/

后者是Chromium项目新开发的用以替代前者的工具。

https://blog.csdn.net/u011331731/article/details/108354605

强大的可视化利器Chrome Trace Viewer使用详解

---

https://blog.csdn.net/zkbaba/article/details/106178542

TensorFlow性能分析工具—TensorFlow Profiler

https://zhuanlan.zhihu.com/p/140343833

tensorflow profiling工具简介——tensorflow原生工具

https://www.tensorflow.org/tensorboard/tensorboard_profiling_keras

TensorBoard性能分析:在Keras中对基本训练指标进行性能分析

https://github.com/tensorflow/benchmarks

TensorFlow benchmarks

## 代码实现

GPU Profiling需要硬件厂商的支持。

比如Nvidia的CUDA Profiling Tools Interface (CUPTI)、Graphcore的PopVision trace instrumentation library (libpvti)等。

```cpp
class GpuTracer : public profiler::ProfilerInterface {
 public:
  GpuTracer(RocmTracer* rocm_tracer)
```

CuptiTracerEvent/RocmTracerEvent

HloExecutionProfile/ExecutionProfile

`export XLA_FLAGS="--xla_hlo_profile"`

参考：

https://zhuanlan.zhihu.com/p/357191706

使用Graphcore PopVision分析工具优化AI性能

# op Backprop

## compute_gradients & apply_gradients

由源代码可以知道`optimizer.minimize`实际上包含了两个步骤，即`compute_gradients`和`apply_gradients`，前者用于计算梯度，后者用于使用计算得到的梯度来更新对应的variable。

如果想要部分更新某个Variable的话，可用如下步骤：

1.生成需要更新的元素的mask tensor。1代表要更新，0代表不更新。

2.`compute_gradients`得到grad tensor。

3.`grad = grad * mask`

4.`apply_gradients`。

通常来说，如果一个计算图中没有optimizer，则一般只包含forward运算，而没有backward运算。

## Add

```cpp
//forward
REGISTER3(BinaryOp, GPU, "AddV2", functor::add, float, Eigen::half, double);
tensorflow/core/kernels/cwise_ops_common.h: BinaryOp

//backward
tensorflow/python/ops/math_grad.py:
@ops.RegisterGradient("AddV2")
def _AddGrad(op, grad):
tensorflow/core/ops/math_grad.cc:
REGISTER_OP_GRADIENT("AddV2", AddGrad);

//RegisterGradient
tensorflow/python/framework/ops.py:
class RegisterGradient(object):
```

Gradient有两种处理方式：（tensorflow/python/ops/gradients_util.py: _GradientsHelper）

- 有RegisterGradient的op，直接调用注册的函数。

- 没有的，调用SymbolicGradient。

参考：

https://www.zhihu.com/question/56443480

TensorFlow的自动求导具体是在哪部分代码里实现的？

## Conv

```cpp
tensorflow/cc/gradients/nn_grad.cc:
REGISTER_GRADIENT_OP("Conv2D", Conv2DGrad);

tensorflow/python/ops/nn_grad.py:
@ops.RegisterGradient("Conv2DBackpropInput")
def _Conv2DBackpropInputGrad(op, grad):

@ops.RegisterGradient("Conv2DBackpropFilter")
def _Conv2DBackpropFilterGrad(op, grad):
```

Conv2D的Backprop操作可分为两部分：

- Conv2DBackpropInput负责计算上一层的梯度，也就是所谓的in_grad。

- Conv2DBackpropFilter负责计算Kernel的梯度。（似乎没有计算bias梯度）

```cpp
// BP input
// tensorflow source code:
tensorflow/core/kernels/conv_grad_input_ops.cc: LaunchConv2DBackpropInputOp
tensorflow/core/kernels/conv_grad_input_ops.h: LaunchConv2DBackpropInputOpImpl
tensorflow/core/kernels/eigen_backward_spatial_convolutions.h: Eigen::SpatialConvolutionBackwardInput
// eigen source code:
unsupported/Eigen/CXX11/src/Tensor/TensorBase.h: TensorBase::contract()
unsupported/Eigen/CXX11/src/Tensor/TensorContraction.h: evalGemmPartial
unsupported/Eigen/CXX11/src/Tensor/TensorContraction.h: TensorContractionKernel
Eigen/src/Core/products/GeneralBlockPanelKernel.h: gebp_kernel::operator()

// BP filter
// tensorflow source code:
tensorflow/core/kernels/conv_grad_filter_ops.cc: LaunchConv2DBackpropFilterOp
tensorflow/core/kernels/eigen_backward_spatial_convolutions.h: Eigen::SpatialConvolutionBackwardKernel
// eigen source code:
unsupported/Eigen/CXX11/src/Tensor/TensorBase.h: TensorBase::contract()
```

以上是CPU计算BP的调用路径，要点如下：

- 无论是计算BP input，还是BP filter，最终都会转换成GEMM运算。

- GEMM运算会调用TensorContractionKernel。

Tensor contraction是一种Tensor运算，参见《线性代数（一）》中的“张量分析”一节。

# 我的TensorFlow实践

## MNIST+Softmax

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_mnist.py

## MNIST+CNN

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_cnn.py

第一个例子中，我对CPU的计算能力还没有切肤之痛，但在这里使用CPU差不多要花半个小时时间。。。

# Broadcast

Broadcast是一种填充元素以使操作数的形状相匹配的操作。例如，对一个[3,2]的张量和一个[3,1]的张量相加在TF中是合法的，TF会使用默认的规则将[3,1]的张量填充为[3,2]的张量，从而使操作能够执行下去。

参考：

https://www.cnblogs.com/yangmang/p/7125458.html

numpy数组广播

https://blog.csdn.net/LoseInVain/article/details/78763303

TensorFlow中的广播Broadcast机制

# TensorFlow Federated

TFF是一个开源框架，用于试验针对分散式数据的机器学习和其他计算。它采用的是一种名为联合学习(FL)的方法，许多参与的客户端能够训练共享的ML模型，同时将数据保存在本地。

这个项目感觉上和Leela Zero有些相似。

从原理上说，TFF主要使用了Federated Machine Learning技术。

参考：

https://mp.weixin.qq.com/s/K2-i3U-BCOctetMkvuvVxg

TensorFlow Federated发布

https://mp.weixin.qq.com/s/6QKyE3jIOwBK_2rcG-Vtiw

联邦机器学习-概念与应用
