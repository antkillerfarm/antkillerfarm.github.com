---
layout: post
title:  TensorFlow（二）
category: AI 
---

# TensorFlow

## 模型文件

tensorflow model包含2个文件：

a）Meta graph:

使用protocol buffer来保存整个tensorflow graph.例如所有的variables, operations, collections等等。这个文件使用.meta后缀。

b) Checkpoint file:

二进制文件包含所有的weights,biases,gradients和其他variables的值。这个文件使用.ckpt后缀，有2个文件：

mymodel.data-00000-of-00001

mymodel.index

.data文件用于保存训练好的variables，以供未来的推断之用。

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

https://www.jianshu.com/p/243d4f0b656c

TensorFlow自定义模型导出：将.ckpt格式转化为.pb格式

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

## Eager Execution

TensorFlow的Eager Execution可立即评估操作，无需构建图：操作会返回具体的值，而不是构建以后再运行的计算图。这也就是所谓的动态图计算的概念。

参考：

https://mp.weixin.qq.com/s/Yp2zE85VCx8q67YXvuw5qw

TensorFlow引入了动态图机制Eager Execution

https://github.com/ZhuanZhiCode/TensorFlow-Eager-Execution-Examples

Eager Execution的代码示例

https://mp.weixin.qq.com/s/By_GKPtY6xr8MwkWA6frzA

TensorFlow的动态图工具Eager怎么用？这是一篇极简教程

https://mp.weixin.qq.com/s/Lvd4NfLg0Lzivb4BingV7w

Tensorflow Eager Execution入门指南

https://mp.weixin.qq.com/s/q6bJfCV5kU8BzvWjOXkCDg

简单粗暴TensorFlow Eager教程

https://mp.weixin.qq.com/s/zz8XCykJ6jxbE5J4YwAkEA

一招教你使用tf.keras和eager execution解决复杂问题

## Estimator

![](/images/img2/tensorflow_programming_environment.png)

Estimator是一个非常高级的API，其抽象等级甚至在Keras之上。

Estimator主要包括以下部分：

1.初始化。定义网络结构。

2.train。

3.evaluate。

4.predict。

TensorFlow已经包含了一些预置的Estimator。例如：BoostedTreesClassifier、DNNClassifier、LinearClassifier等。具体可参见：

https://tensorflow.google.cn/api_docs/python/tf/estimator

参考：

https://mp.weixin.qq.com/s/a68brFJthczgwiFoUBh30A

TensorFlow数据集和估算器介绍

## 细节

执行`session.run(out)`，会在终端打印out的值，但执行`res = session.run(out)`则不会。

----

tensorflow的程序中,在main函数下,都是使用tf.app.run()来启动。查看源码可知,该函数是用来处理flag解析，然后执行main函数。

https://blog.csdn.net/lujiandong1/article/details/53262612

tensorflow中的tf.app.run()

----

TF提供了一套专门的IO函数：tf.gfile。主要优点在于：对于写文件来说，open操作直到真的需要写的时候才执行。

----

迁移学习的时候，有的时候需要保持某几层的权值，在后续训练中不被改变。这时，可以在创建Variable时，令trainable=false。

----

sparse_softmax_cross_entropy_with_logits和softmax_cross_entropy_with_logits的区别在于：后者的label是一个one hot的tensor，而前者label直接用对应分类的index表示就行了。

## blog

http://www.jianshu.com/u/eaec1fc422e9

一个TF的blog

http://blog.csdn.net/u012436149

一个TensorFlow+PyTorch的blog

# 我的TensorFlow实践

## MNIST+Softmax

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_mnist.py

## MNIST+CNN

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_cnn.py

第一个例子中，我对CPU的计算能力还没有切肤之痛，但在这里使用CPU差不多要花半个小时时间。。。

## 框架怀古（2017.9）

http://deeplearning.net/

这个网站是Theano的主站，也是我最早接触DL时浏览的网站。其时，我虽然对DL有浓厚的兴趣，但尚未以此作为工作内容。

从该网站提供的招聘信息来看，Caffe、Theano、Torch是当时主流的三大框架库。

岂料时隔一年半载之后，这三大框架都渐趋式微。

Caffe被Caffe 2替代，但使用的广泛度仍超过后者。

Theano被同样基于计算图的TensorFlow淘汰。2017年9月停止更新。

Torch相对变动最小，它被PyTorch替代。这更可以看作是python对于lua的胜利。

# TensorFlow参考

https://mp.weixin.qq.com/s/t1QFIOq-VBNOrSm0zW-PlQ

深度学习TensorFlow实现集合

https://mp.weixin.qq.com/s/IzijD8Sh3G2WsCz7aaxyhg

TensorFlow深度学习概述

https://github.com/yahoo/TensorFlowOnSpark

TensorFlow On Spark

https://silencezjl.coding.me/2017/05/01/%E5%81%B7%E4%B8%80%E6%B3%A2%E8%B5%84%E6%BA%90/

各种TensorFlow资源

https://mp.weixin.qq.com/s/haj9lS59yWtk-C75EtGIcw

深度学习工程模板：简化加载数据、构建网络、训练模型和预测样本的流程

https://github.com/zsdev2015/machine_learning

某国内小牛写的中文入门demo，注释非常详细

https://morvanzhou.github.io/tutorials/

一个以python语言教学的ML、DL教程，比较通俗易懂。

https://mp.weixin.qq.com/s/gJBDXf_5ViPR9dNm3eH2Hg

TensorFlow初学者必须了解的55个经典案例

http://mp.weixin.qq.com/s/JZ1ceGQDmQUaNW5wl6biLA

TensorFlow实现流行机器学习算法教程汇集
