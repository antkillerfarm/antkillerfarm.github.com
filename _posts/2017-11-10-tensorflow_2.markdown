---
layout: post
title:  TensorFlow（二）
category: AI 
---

# TensorFlow

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

{% highlight python %}
w1 = tf.Variable(tf.random_normal(shape=[2]), name='w1')
w2 = tf.Variable(tf.random_normal(shape=[5]), name='w2')
saver = tf.train.Saver()
sess = tf.Session()
sess.run(tf.global_variables_initializer())
saver.save(sess, 'my_test_model')
{% endhighlight %}

### 加载模型

{% highlight python %}
new_saver = tf.train.import_meta_graph('my_test_model-1000.meta')
new_saver.restore(sess, tf.train.latest_checkpoint('./‘))
{% endhighlight %}

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

{% highlight python %}
a=tf.constant(2)
b=tf.constant(3)
x=tf.constant(4)
y=tf.constant(5)
z = tf.multiply(a, b)
result = tf.cond(x < y, lambda: tf.add(x, z), lambda: tf.square(y))
with tf.Session() as session:
    print(result.eval())
{% endhighlight %}

### tf.case

{% highlight python %}
decode_png = lambda :tf.image.decode_png(image_tensor, channels)
decode_jpg = lambda :tf.image.decode_jpeg(image_tensor, channels)
decoder = { tf.equal(image_ext, '.png'):  decode_png,
            tf.equal(image_ext, '.jpg'):  decode_jpg}
image_tensor = tf.case(decoder, default = decode_png, exclusive = True)
{% endhighlight %}

## 内存布局

Tensorflow和Caffe的内存布局存在较大差异，这是两者模型转换时，最常遇到的问题。一般认为，Caffe的内存布局对硬件加速更友好一些（局部数据在内存中摆放在一起）。

|  | Tensorflow | Caffe |
|:--:|:--:|:--:|
| Tensor | NHWC | NCHW |
| Weight | HWIO | OIHW |

## TFLite

Tensorflow源代码中自带的toco工具，可用于生成一个可供TensorFlow Lite框架使用的tflite文件。

代码：

https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/lite/toco

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

## Android NN

TFLite是Google的Tensorflow团队开发的移动DL框架，它可以在任意系统（非android，甚至非linux）执行。而Android NN则是Google的Android团队针对Android平台开发的DL框架。

团队的不同，决定了这两款产品并非完全兼容。一般来说，TFLite由于紧跟Tensorflow，其对新op的支持要更及时一些。但Android NN由于有Facebook等外部客户的需求推动，在个别情况下，也有相反的情况发生。

参考：

https://developer.android.google.cn/ndk/reference/group/neural-networks

这是Android NDK中的NN相关的接口文档

https://developer.arm.com/products/software/mali-drivers/android-nnapi

这是ARM对于Android NN的一个实现。

https://mp.weixin.qq.com/s/fal6vz9gaZMbR41QMGE3AQ

MLIR发布：全新的中介码与编译器框架

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

## op的C++实现

有的时候为了将Tensorflow的op移植到其他平台，需要找到相应op的cpu实现。比如space_to_batch这个op，它的实现在：

core/kernels/spacetobatch_op.cc

简单的op一般找到这里就可以了，但space_to_batch还要更深一层：

core/kernels/spacetobatch_functor.cc

一般XXX_impl.cc或者XXX_functor.cc才是op实现真正所在的位置。

此外，TFlite的实现往往更加简单：

tensorflow/contrib/lite/kernels/internal/reference/reference_ops.h

## TensorFlow.js

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

## Eager Execution

TensorFlow的Eager Execution可立即评估操作，无需构建图：操作会返回具体的值，而不是构建以后再运行的计算图。这也就是所谓的动态图计算的概念。

参考：

https://mp.weixin.qq.com/s/Yp2zE85VCx8q67YXvuw5qw

TensorFlow引入了动态图机制Eager Execution

https://github.com/ZhuanZhiCode/TensorFlow-Eager-Execution-Examples

Eager Execution的代码示例

https://github.com/madalinabuzau/tensorflow-eager-tutorials

TensorFlow的动态图工具Eager怎么用？这是一篇极简教程

https://mp.weixin.qq.com/s/Lvd4NfLg0Lzivb4BingV7w

Tensorflow Eager Execution入门指南

https://github.com/snowkylin/TensorFlow-cn

简单粗暴TensorFlow Eager教程

https://mp.weixin.qq.com/s/zz8XCykJ6jxbE5J4YwAkEA

一招教你使用tf.keras和eager execution解决复杂问题
