---
layout: post
title:  TensorFlow（二）
category: AI 
---

* toc
{:toc}

# TensorFlow

## Slim

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

## TensorBoard

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

## 模型文件

tensorflow model包含2个文件：

a）Meta graph:

使用protocol buffer来保存整个tensorflow graph.例如所有的variables, operations, collections等等。这个文件使用.meta后缀。

b) Checkpoint file:

有2个文件：

mymodel.data-00000-of-00001

mymodel.index

.data文件包含所有的weights,biases,gradients和其他variables的值。

tensorflow还有一个叫checkpoint的文件，用来简单保存最近一次的checkpoint记录。

### 保存模型

```python
w1 = tf.Variable(tf.random_normal(shape=[2]), name='w1')
w2 = tf.Variable(tf.random_normal(shape=[5]), name='w2')
saver = tf.train.Saver()
sess = tf.Session()
sess.run(tf.global_variables_initializer())
saver.save(sess, 'my_test_model')
```

### 加载模型

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

## .pb文件

TensorFlow常用的模型保存格式还有.pb格式。这种格式下，模型和权重被整合为一个.pb文件，便于模型的发布和部署。相对应的，这种格式对于train就不太友好了。

以下的脚本可用于将.pb文件导入到tensorboard中：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/ml/tensorflow/graph/pb_visualize.py

参考：

https://www.jianshu.com/p/243d4f0b656c

TensorFlow自定义模型导出：将.ckpt格式转化为.pb格式

https://www.jianshu.com/p/c9fd5c01715e

TensorFlow模型保存与恢复

## 模型文件的图操作

基本操作一般基于tf.Graph：

https://tensorflow.google.cn/api_docs/python/tf/Graph

复杂一点的进阶操作可参见：

https://tensorflow.google.cn/api_guides/python/contrib.graph_editor

示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/ml/tensorflow/graph/hello_graph.py

除了运算类op之外，TF还有辅助类的op，例如tf.shape和tf.Print。下面的示例展示了如何在Graph中插入tf.shape和tf.Print结点，从而导出中间的计算结果：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/ml/tensorflow/graph/insert_print_node.py

## TFRecord

TFRecord是TensorFlow官方定义的存放样本数据文件。

参考：

http://www.cnblogs.com/antflow/p/7299029.html

TFRecord的使用

https://zhuanlan.zhihu.com/p/27481108

TensorFlow直接读取图片和读写TFRecords速度对比

## 多核(multicore)，多线程(multi-thread)

在Tensorflow程序中，我们会经常看到”with tf.device("/cpu:0"): “ 这个语句。单独使用这个语句，而不做其他限制，实际上默认tensorflow程序占用所有可以使用的内存资源和CPU核。

参考：

http://deepnlp.org/blog/tensorflow-parallelism/

Tensorflow并行：多核(multicore)，多线程(multi-thread)

## 控制流

### tf.cond

```python
a=tf.constant(2)
b=tf.constant(3)
x=tf.constant(4)
y=tf.constant(5)
z = tf.multiply(a, b)
result = tf.cond(x < y, lambda: tf.add(x, z), lambda: tf.square(y))
with tf.Session() as session:
    print(result.eval())
```

### tf.case

```python
decode_png = lambda :tf.image.decode_png(image_tensor, channels)
decode_jpg = lambda :tf.image.decode_jpeg(image_tensor, channels)
decoder = { tf.equal(image_ext, '.png'):  decode_png,
            tf.equal(image_ext, '.jpg'):  decode_jpg}
image_tensor = tf.case(decoder, default = decode_png, exclusive = True)
```

## 内存布局

Tensorflow和Caffe的内存布局存在较大差异，这是两者模型转换时，最常遇到的问题。一般认为，Caffe的内存布局对卷积硬件加速更友好一些。

|  | Tensorflow | Caffe |
|:--:|:--:|:--:|
| Tensor | NHWC | NCHW |
| Weight | HWIO | OIHW |

## TFLite

官网：

https://tensorflow.google.cn/lite/

Tensorflow源代码中自带的toco（Tensorflow Optimizing COnverter）工具，可用于生成一个可供TensorFlow Lite框架使用的tflite文件。

代码：

https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/lite/toco

----

tflite模型中间结果的导出，不是太方便，原因是相关内存被复用。

解决办法有两个：

- 把想要dump的tensor设置为网络的output，然后转成tflite。

- 修改tflite.invoke的代码，以导出中间结果。

参考：

https://stackoverflow.com/questions/57139676/savedmodel-tflite-signaturedef-tensorinfo-get-intermediate-layer-outputs

这里还有一个非常Hack的方法，但是已经过时了：

https://github.com/raymond-li/tflite_tensor_outputter/blob/master/tflite_tensor_outputter.py

----

参考：

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

## Android NN

TFLite是Google的Tensorflow团队开发的移动DL框架，它可以在任意系统（非android，甚至非linux）上执行。而Android NN则是Google的Android团队针对Android平台开发的DL框架。

团队的不同，决定了这两款产品并非完全兼容。一般来说，TFLite由于紧跟Tensorflow，其对新op的支持要比后者更及时一些。而Android NN由于有Facebook等外部客户的需求推动，在个别情况下，也有相反的情况发生。

Android NN支持的算子的代码在aosp/frameworks/ml/nn/common/operations下。

参考：

https://developer.android.google.cn/ndk/reference/group/neural-networks

这是Android NDK中的NN相关的接口文档

https://developer.android.google.cn/ndk/guides/neuralnetworks

Android NN的指南

https://developer.arm.com/products/software/mali-drivers/android-nnapi

这是ARM对于Android NN的一个实现。

## Broadcast

Broadcast是一种填充元素以使操作数的形状相匹配的操作。例如，对一个[3,2]的张量和一个[3,1]的张量相加在TF中是合法的，TF会使用默认的规则将[3,1]的张量填充为[3,2]的张量，从而使操作能够执行下去。

参考：

https://www.cnblogs.com/yangmang/p/7125458.html

numpy数组广播

https://blog.csdn.net/LoseInVain/article/details/78763303

TensorFlow中的广播Broadcast机制

## TensorFlow Serving

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

https://mp.weixin.qq.com/s/TL87BY3DdP1bolc0Sxkahg

gRPC客户端创建和调用原理解析

https://zhuanlan.zhihu.com/p/30628048

远程通信协议：从CORBA到gRPC

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

## op的C++实现

有的时候为了将Tensorflow的op移植到其他平台，需要找到相应op的cpu实现。比如space_to_batch这个op，它的实现在：

core/kernels/spacetobatch_op.cc

简单的op一般找到这里就可以了，但space_to_batch还要更深一层：

core/kernels/spacetobatch_functor.cc

一般XXX_impl.cc或者XXX_functor.cc才是op实现真正所在的位置。

此外，TFlite的实现往往更加简单：

tensorflow/contrib/lite/kernels/internal/reference/reference_ops.h
