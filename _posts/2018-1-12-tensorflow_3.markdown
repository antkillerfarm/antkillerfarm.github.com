---
layout: post
title:  TensorFlow（三）
category: AI 
---

* toc
{:toc}

# Hama

TensorFlow实际上是Google开发的第二代DL框架。在它之前，Google内部还有一个叫做DistBelief的框架。这个框架没有开源，但是有论文发表。因此，就有了一个叫做Apache Hama的项目，作为它的开源实现。

官网：

https://hama.apache.org/

这个项目采用了一种叫做Bulk Synchronous Parallel的并行计算模型。

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

## 参考

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

tflite模型中间结果的导出，不是太方便，原因是相关内存被复用。

解决办法有两个：

- 把想要dump的tensor设置为网络的output，然后转成tflite。

- 修改tflite.invoke的代码，以导出中间结果。

参考：

https://stackoverflow.com/questions/57139676/savedmodel-tflite-signaturedef-tensorinfo-get-intermediate-layer-outputs

这里还有一个非常Hack的方法：

https://github.com/raymond-li/tflite_tensor_outputter/blob/master/tflite_tensor_outputter.py

这个脚本跑起来有些问题，需要配合专业版的schema_py_generated.py才能使用。

https://blog.csdn.net/abc20002929/article/details/112529203

tflite模型调试-中间层output输出

---

https://gitee.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/ml/tensorflow/tflite/tflite_multi_output_quant.py

这个例子包含了以下内容：

- 如何直接用tf算子搭建网络，并导出为tflite文件。注意`@tf.function`的用法。
- 如何搭建多输出的网络。
- 如何生成量化模型。注意`representative_dataset_gen`，它展示了如何用fake data替换真实数据。

---

参考：

https://www.cnblogs.com/zhouyang209117/p/8087258.html

使用flatbuffers

http://harmonyhu.com/2019/02/03/flatbuffers-reflection/

FlatBuffers反射

https://blog.csdn.net/u011279649/article/details/83186550

TFLite:模型文件的结构和解析器

https://jackwish.net/2020/introducing-tflite-parser-package.html

Introducing TFLite Parser Python Package

https://jackwish.net/tflite/

Easily Parse TFLite Models with Python

## 参考

https://www.jianshu.com/p/fa204a54a956

生成TFLite模型文件

https://mp.weixin.qq.com/s/eSczqqyzh4PZomJL4saxug

出门问问：使用TensorFlow Lite在嵌入式端部署热词检测模型

https://mp.weixin.qq.com/s/U_Pew90j9swIqti3oKEIQg

玩转TensorFlow Lite：有道云笔记实操案例分享

https://mp.weixin.qq.com/s/lNP9WdzSWE4FjB_-Sjc2aA

TensorFlow Lite for Android初探

https://mp.weixin.qq.com/s/IuD1oxeiFBq8kqh_zRLb0Q

一步实现从TF到TF Lite，谷歌提出定制on-device模型框架

https://mp.weixin.qq.com/s/65HiEwCyzeA_d9flPBcpLQ

谷歌正式发布TensorFlowLite，半监督跨平台快速训练ML模型

https://mp.weixin.qq.com/s/6_yZPlKLYiWBRQFk5Y1OKA

TensorFlow Lite微控制器

# Android NN

TFLite是Google的Tensorflow团队开发的移动DL框架，它可以在任意系统（非android，甚至非linux）上执行。而Android NN则是Google的Android团队针对Android平台开发的DL框架。

团队的不同，决定了这两款产品并非完全兼容。一般来说，TFLite由于紧跟Tensorflow，其对新op的支持要比后者更及时一些。而Android NN由于有Facebook等外部客户的需求推动，在个别情况下，也有相反的情况发生。

Android NN支持的算子的代码在frameworks/ml/nn/common/operations下。

>最新的代码里改到了packages/modules/NeuralNetworks/common/operations下。

参考：

https://developer.android.google.cn/ndk/reference/group/neural-networks

这是Android NDK中的NN相关的接口文档

https://developer.android.google.cn/ndk/guides/neuralnetworks

Android NN的指南

https://developer.arm.com/products/software/mali-drivers/android-nnapi

这是ARM对于Android NN的一个实现。

# TensorFlow Serving

TensorFlow Serving是一个用于机器学习模型serving的高性能开源库。它可以将训练好的机器学习模型部署到线上，使用gRPC作为接口接受外部调用。更加让人眼前一亮的是，它支持模型热更新与自动模型版本管理。

代码：

https://github.com/tensorflow/serving

TensorFlow Serving实际上是TensorFlow Extended (TFX)的一部分：

https://tensorflow.google.cn/tfx

TFX还包括了Data Validation、Transform和Model Analysis等方面的功能。

参考：

https://zhuanlan.zhihu.com/p/23361413

TensorFlow Serving尝尝鲜

http://www.cnblogs.com/xuchenCN/p/5888638.html

tensorflow serving

https://mp.weixin.qq.com/s/iqvpX6QuBEmF_UK9RMu9eQ

TensorFlow Serving入门

https://mp.weixin.qq.com/s/b569est_LpcxsoTNWXcfog

TensorFlow Extended帮你快速落地项目

https://mp.weixin.qq.com/s/qOy9fR8Zd3SufvsMmLpoGg

使用TensorFlow Serving优化TensorFlow模型

https://mp.weixin.qq.com/s/IPwOZKvDsONegyIuwkG6bQ

将深度学习模型部署为web应用有多难？答案自己找

https://mp.weixin.qq.com/s/7nugWFKtD-C6cpwm2TyvdQ

手把手教你如何部署深度学习模型

https://zhuanlan.zhihu.com/p/77664408

如何解决推荐系统工程难题——深度学习推荐模型线上serving？

https://mp.weixin.qq.com/s/vqFRbsM9DGu8ikJ3VNp_-g

TensorFlow Extended(TFX)：面向生产环境的机器学习

http://mp.weixin.qq.com/s/hpv6bzr-5VZet-UCHOCQLQ

谷歌TFX：基于TensorFlow可大规模扩展的机器学习平台

https://mp.weixin.qq.com/s/ANoY3MZEvz7SvKXDE-24NQ

迈向ML工程：TensorFlow Extended(TFX)简史

https://mp.weixin.qq.com/s/DkCGusznH8F8p39oRLuNBQ

TensorFlow Serving模型更新毛刺的完全优化实践

https://mp.weixin.qq.com/s/GoaePWnq3OtU4hNatbIJ5A

丰富TF Serving生态，爱奇艺开源灵活高性能的推理系统XGBoost Serving

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
