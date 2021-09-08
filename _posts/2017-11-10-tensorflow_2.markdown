---
layout: post
title:  TensorFlow（二）
category: AI 
---

* toc
{:toc}

# XLA

XLA(Accelerated Linear Algebra)是TensorFlow计算图的编译器。

官网：

https://www.tensorflow.org/xla?hl=zh-cn

基本架构：

https://www.tensorflow.org/xla/architecture

![](/images/img4/XLA.png)

CSE(Common subexpression elimination)

DCE(Dead code elimination)

TFE(Tensorflow Eager)

## HLO

XLA用HLO(High Level Optimizer)这种中间表示形式，表示正在被优化的计算图。

三个概念，hlo module, computation, instruction。

- hlo module用源码注释的解释，就是一个编译单元，相当于是一个完整可运行的程序。既然是一个程序，就有入口函数，也就是entry_computation，每个module都有且仅有一个entry_computation，相当于main函数，有输入和输出，输入可以是多个参数，但输出只有一个（root instruction的值），如果要返回多个值，需要把多个值构造成一个元组（tuple）返回。
- 一个module可以包含多个computation，除了entry_computation，其他的都是"nested"，也就是被调用。
- HLO instructions就是op了，对应了官网上列出的operation semantics，看注释已经解释的非常清楚了，op融合和向llvm ir转换都是在这个层面进行的。

op的官方定义：

https://tensorflow.google.cn/xla/operation_semantics

参考：

https://zhuanlan.zhihu.com/p/71980945

tensorflow xla hlo基本概念和pass pipeline

## 应用层

TF目前（v2.4.1）的默认编译选项中已经包含了XLA，但是默认不会启动。

启动方法：

- 手动启动

设置环境变量：

`TF_XLA_FLAGS="--tf_xla_enable_xla_devices"`

手动指定需要XLA计算的op：

```bash
with tf.device("/device:XLA_CPU:0"):
  ...
```

这种方法的缺点是：代码需要修改，且XLA不支持有些复杂的op。

- 自动启动

`TF_XLA_FLAGS="--tf_xla_auto_jit=2 --tf_xla_cpu_global_jit"`

代码无需修改。

unit test：

tensorflow/compiler/xla/tests

```bash
bazel build //tensorflow/compiler/xla/tests:convolution_test
./bazel-bin/tensorflow/compiler/xla/tests/convolution_test_cpu --gtest_filter="XXXX"
```

## 底层实现

XLA支持两种接入模式：

- JIT。compiler/jit/

- AOT。compiler/aot/

将整个计算图用_XlaCompile & _XlaRun替换。

compiler/tf2xla/ -> compiler/xla/client/ -> compiler/xla/service/

最终的计算由service负责实现。

## backward

tensorflow/compiler/tf2xla/g3doc/gpu_supported_ops.md

```cpp
CanonicalizeBackwardFilterConvolution
Conv2DBackpropFilter
GetKnownXLAWhitelistOp
XlaOpRegistry::GetAllRegisteredOps
REGISTER_XLA_OP
HloInstruction::Visit
class ConvBackpropFilterOp : public XlaOpKernel
MakeXlaBackpropFilterConvOp
ConvGeneralDilated
```

https://discuss.tvm.apache.org/t/rfc-mlir-frontend/6473

## backend

官方backend：

tensorflow/compiler/xla/service

第三方的XLA backend接入：

tensorflow/compiler/plugin

第三方的XLA backend中，比较出名的是graphcore。

它的TF实现：

https://github.com/graphcore/tensorflow/tensorflow/compiler/plugin/poplar

XLA的主要目的是方便硬件厂商更好的适配tensorflow。因此，作为XLA基础的HLO，其op数非常少，仅有不到100个。用户只要实现了这些op，就可以接入tf了——其他不支持的tf op，都被分解为简单的HLO op。

```cpp
HloTransposeInstruction
HandleTranspose
```

HLO op的弊端是颗粒度太细，导致执行效率不高。因此，XLA还提供了高级op的注册功能，主要是用`xla::CustomCall`来实现。

```cpp
MaxPool2DGradOp
REGISTER_XLA_OP(Name("MaxPoolGrad").Device(DEVICE_IPU_XLA_JIT),
                MaxPool2DGradOp);
xla::CustomCall(&b, PoplarOp_Name(PoplarOp::MaxPoolGrad), args,
                          input_shape, attribute_map_.Serialise());
```

model test:

tensorflow/compiler/plugin/poplar/docs/example_tf2_model_fit.py

## 混合backend

XLA支持混合多backend的运行，可用`tf.debugging.set_log_device_placement(True)`查看相关的设备指派信息。

设备指派主要由Placer模块负责：

https://www.cnblogs.com/deep-learning-stacks/p/9823486.html

TensorFlow中的Placement启发式算法模块——Placer

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

# TensorFlow Addons

TensorFlow SIG Addons是包含社区贡献的代码库，也就是1.x时代的contrib文件夹内的内容。一般用`tfa`作为包前缀。

# TensorFlow高层封装

目前对TensorFlow的封装如下所示：

1.TensorFlow-Slim。主要提供了层一级的封装。粒度和OpenVX类似。

2.tf.contrib.learn（之前也被称为skflow）。提供了类似sklearn的接口。

前2个是TensorFlow自带的封装

3.第三个是TFLearn。在tf.contrib.learn上的封装。需单独安装：

`sudo pip install tflearn`

http://tflearn.org/

4.Keras。

5.TensorLayer。这个的封装粒度介于TensorFlow-Slim和TFLearn之间。

https://tensorlayer.readthedocs.io/en/stable/user/tutorials.html

这个Tutorials的内容比较多，除了常见的CNN、RNN之外，还有RL和DAE的内容。

6.Pretty Tensor。来自google的TensorFlow封装。

https://github.com/google/prettytensor

7.Sonnet。来自Deepmind的TensorFlow封装。

https://github.com/deepmind/sonnet

参见：

http://www.infoq.com/cn/articles/introduction-of-tensorflow-part06

深入浅出TensorFlow（六）TensorFlow高层封装

# Slim

代码：

tensorflow/contrib/slim

示例：

https://github.com/mnuke/tf-slim-mnist

参见：

http://geek.csdn.net/news/detail/126133

如何用TensorFlow和TF-Slim实现图像分类与分割

实战心得：

tf-slim-mnist例子中mnist数据不是原始格式的，而是经过了`datasets/download_and_convert_mnist.py`的转换。

该示例执行时也没有控制台的输出信息，一度让我觉得很不方便。后来才发现，原来可以用TensorBoard查看log文件夹。

# TensorBoard

TensorBoard是一个http服务，用以监控TensorFlow的执行。

`writer = tf.summary.FileWriter("logs/", sess.graph)`

然后

`tensorboard --logdir='logs/'`

启动之后，用浏览器打开`http://localhost:6006`即可。

TensorBoard会将同类结点Group，但Group之后，有时反而不易观察具体的结构。这个时候最好Ungroup一下。

参考：

http://blog.csdn.net/u013082989/article/details/53510625

TensorFlow学习_01_安装_基本操作_可视化结构、过程_Mnist

https://blog.csdn.net/sinat_33761963/article/details/62433234

Tensorflow的可视化工具Tensorboard的初步使用

https://mp.weixin.qq.com/s/Zaz9hmTuUbd-hCx-zHhBgg

TensorBoard：可视化学习

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

# 模型文件

tensorflow model包含2个文件：

a）Meta graph:

使用protocol buffer来保存整个tensorflow graph.例如所有的variables, operations, collections等等。这个文件使用.meta后缀。

b) Checkpoint file:

有2个文件：

mymodel.data-00000-of-00001

mymodel.index

.data文件包含所有的weights,biases,gradients和其他variables的值。

tensorflow还有一个叫checkpoint的文件，用来简单保存最近一次的checkpoint记录。

## 保存模型

```python
w1 = tf.Variable(tf.random_normal(shape=[2]), name='w1')
w2 = tf.Variable(tf.random_normal(shape=[5]), name='w2')
saver = tf.train.Saver()
sess = tf.Session()
sess.run(tf.global_variables_initializer())
saver.save(sess, 'my_test_model')
```

## 加载模型

```python
new_saver = tf.train.import_meta_graph('my_test_model-1000.meta')
new_saver.restore(sess, tf.train.latest_checkpoint('./‘))
```

参考：

http://www.cnblogs.com/azheng333/archive/2017/06/09/6972619.html

Tensorflow模型保存和加载

http://blog.csdn.net/wiinter_fdd/article/details/72821923

Tensorflow中的模型持久化

https://mp.weixin.qq.com/s/3GfxnwzIeeQj1LVSYKnZjQ

如何保存和恢复TensorFlow训练的模型？

# .pb文件

TensorFlow常用的模型保存格式还有.pb格式。这种格式下，模型和权重被整合为一个.pb文件，便于模型的发布和部署。相对应的，这种格式对于train就不太友好了。

以下的脚本可用于将.pb文件导入到tensorboard中：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/ml/tensorflow/graph/pb_visualize.py

参考：

https://www.jianshu.com/p/243d4f0b656c

TensorFlow自定义模型导出：将.ckpt格式转化为.pb格式

https://www.jianshu.com/p/c9fd5c01715e

TensorFlow模型保存与恢复

# 模型文件的图操作

基本操作一般基于tf.Graph：

https://tensorflow.google.cn/api_docs/python/tf/Graph

复杂一点的进阶操作可参见：

https://tensorflow.google.cn/api_guides/python/contrib.graph_editor

示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/ml/tensorflow/graph/hello_graph.py

除了运算类op之外，TF还有辅助类的op，例如tf.shape和tf.Print。下面的示例展示了如何在Graph中插入tf.shape和tf.Print结点，从而导出中间的计算结果：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/ml/tensorflow/graph/insert_print_node.py

# TFLite

官网：

https://tensorflow.google.cn/lite/

Tensorflow源代码中自带的toco（Tensorflow Optimizing COnverter）工具，可用于生成一个可供TensorFlow Lite框架使用的tflite文件。

代码：

https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/lite/toco

## 模型文件解析

tflite模型使用flatbuffers进行序列化，因此也可以使用flatbuffers解析相关模型。

需要注意的是flatbuffers生成的代码，有两种版本：

- 精简版。默认设置。网上的解析代码用的都是这个版本。缺点：无法修改相应的模型。

- 专业版。`--gen-object-api`。新增`UnPack/UnpackTo/Pack`方法，进行对象结构体与table结构体间的转换。

专业版不是所有语言都有，至少ubuntu自带的flatc就没有提供对python的专业版支持。但是tensorflow自带的flatc是可以的。

`bazel build //tensorflow/lite/tools:visualize`

这个命令会生成一个schema_py_generated.py文件，也就是所谓的专业版本了。
