---
layout: post
title:  TensorFlow（四）
category: DL Framework 
---

* toc
{:toc}

# Debug

# op的C++实现

有的时候为了将Tensorflow的op移植到其他平台，需要找到相应op的cpu实现。比如space_to_batch这个op，它的实现在：

core/kernels/spacetobatch_op.cc

简单的op一般找到这里就可以了，但space_to_batch还要更深一层：

core/kernels/spacetobatch_functor.cc

一般XXX_impl.cc或者XXX_functor.cc才是op实现真正所在的位置。

kernel的注册，一般在：

tensorflow/core/ops

此外，TFlite的实现往往更加简单：

tensorflow/contrib/lite/kernels/internal/reference/reference_ops.h

注册一个tfop分为两部分:Op和OpKernel。其中，Op是tfop的声明部分，类似于函数的声明，主要描述Op静态属性。OpKernel是tfop的实现部分，同样类似于函数的实现，主要描述OpKernel的具体计算逻辑。

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

---

https://zhuanlan.zhihu.com/p/620708213

vscode python调试 (torchpack attach调试)

https://zhuanlan.zhihu.com/p/664239908

VSCode Python：解决Timeout waiting for launcher to connect

## 打印stack trace

C++：
```cpp
#include "tensorflow/core/platform/stacktrace.h"
tensorflow::CurrentStackTrace()
```

`CurrentStackTrace`的实现，在Win平台调用了`CaptureStackBackTrace`函数，在Linux平台调用了`backtrace`函数。

---

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
                                                 profile_batch = (2,6))

model.fit(ds_train,
          epochs=2,
          validation_data=ds_test,
          callbacks = [tboard_callback])
```

profile_batch参数用于设置在第几个batch进行profile。为0，表示disable。一般从第2个batch开始，以避免硬件冷启动对于profile的影响。

脚本运行之后，会在logs路径下生成plugins/profile文件夹。

查看步骤：

1.打开TensorBoard之后，右上角下拉中选择`PROFILE`。

2.左侧的`Tools`下拉中，有好多工具。其中`tensorflow_stats`和`trace_viewer`比较重要。

trace可以保存在本地，也可以用grpc输出到远程，并通过`trace_viewer`中的`Capture Profile`按钮来接收。

Profiler需要联网加载Google Chart库，才能完整显示。

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

LTTng是一个开源的trace框架。

官网：

https://lttng.org/

LTTng有相关的工具，可以导出CTF（Chrome Trace Format）格式的数据。

---

minitrace可以直接生成CTF格式的数据。

代码：

https://github.com/hrydgard/minitrace

---

https://blog.csdn.net/zkbaba/article/details/106178542

TensorFlow性能分析工具—TensorFlow Profiler

https://zhuanlan.zhihu.com/p/140343833

tensorflow profiling工具简介——tensorflow原生工具

https://www.tensorflow.org/tensorboard/tensorboard_profiling_keras

TensorBoard性能分析:在Keras中对基本训练指标进行性能分析

https://github.com/tensorflow/benchmarks

TensorFlow benchmarks

https://blog.csdn.net/kenneth_yu/article/details/77466776

使用profiler检测神经网络模型的运行性能

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

```cpp
tensorflow::profiler::XLineBuilder
XEventBuilder
XPlaneBuilder
HumanReadableProfileBuilder
```

```
{
  TraceMe trace("step");
  ... do some work ...
}

auto id = ActivityStart("step");
  ... do some work ...
ActivityEnd(id);

profiler::AnnotatedTraceMe
```

参考：

https://zhuanlan.zhihu.com/p/357191706

使用Graphcore PopVision分析工具优化AI性能

# 内存布局

Tensorflow和Caffe的内存布局存在较大差异，这是两者模型转换时，最常遇到的问题。一般认为，Caffe的内存布局对卷积硬件加速更友好一些。

|  | Tensorflow | Caffe |
|:--:|:--:|:--:|
| Tensor | NHWC | NCHW |
| Weight | HWIO | OIHW |

# 我的TensorFlow实践

## MNIST+Softmax

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_mnist.py

## MNIST+CNN

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_cnn.py

第一个例子中，我对CPU的计算能力还没有切肤之痛，但在这里使用CPU差不多要花半个小时时间。。。

# TensorFlow.js

https://mp.weixin.qq.com/s/dqMS4NjmNYs7IFHm8uFM8w

TensorFlow发布面向JavaScript开发者的机器学习框架TensorFlow.js

https://zhuanlan.zhihu.com/p/35181413

TensorFlow.js人脸识别—玩转吃豆豆小游戏

https://mp.weixin.qq.com/s/ebLHZAG8H78TsZUKSzAtIw

TF官方博客：基于TensorFlow.js框架的浏览器实时姿态估计

https://mp.weixin.qq.com/s/z6p4A4DfCuK8IBGVGwrtLQ

如何利用TensorFlow.js部署简单的AI版“你画我猜”图像识别应用

https://mp.weixin.qq.com/s/NO_XY-JmTpIkoC-fpkZ-qg

在浏览器上也能训练神经网络？TensorFlow.js带你玩游戏~

https://mp.weixin.qq.com/s/vjpMr3TsF3Lui8Q0IstQxw

浏览器上跑：TensorFlow发布实时人物分割模型，秒速25帧，24个部位

https://mp.weixin.qq.com/s/-BblgnvPLuqpYM8PZ7PQCQ

三行代码实时追踪你的手，只要有浏览器就够了

https://mp.weixin.qq.com/s/C7QdVathJ8YTXF-zXPC-Ow

有人分析了7个基于JS语言的DL框架，发现还有很长的路要走

# TensorFlow Probability

TensorFlow Probability是一个概率编程工具包。

官网：

https://tensorflow.google.cn/probability/

参考：

https://mp.weixin.qq.com/s/NPuYanaUnaX4mYbaNbNNSQ

概率编程工具：TensorFlow Probability官方简介

https://mp.weixin.qq.com/s/cV-5W4YWC9f9wsoNX5fIXA

使用TensorFlow Probability对金融模型中的误差进行介绍性分析

https://mp.weixin.qq.com/s/cxC3SarlBBPTwIxQZ4AG_g

快速上手TensorFlow Probability内置概率编程教材

https://mp.weixin.qq.com/s/T0TsS8YwyCbCjt4J-xonOw

使用TensorFlow Probability Layers的变分自编码器

https://mp.weixin.qq.com/s/6l-NS0NbYK44JS0jnRl82w

使用TensorFlow Probability的概率层执行回归

https://mp.weixin.qq.com/s/2cbd7LBPBRqGt-QO1A7SfQ

在TensorFlow Probability中对结构时间序列建模

https://mp.weixin.qq.com/s/7CjLP5SYpQ-hoC1jwxT1vQ

TensorFlow Probability中的联合分布变分推断
