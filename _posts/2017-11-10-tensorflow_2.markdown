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

HLO(High Level Optimizer)

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

参考：

https://mp.weixin.qq.com/s/RO3FrPxhK2GEoDCGE9DXrw

利用XLA将GPU性能推向极限

https://mp.weixin.qq.com/s/MPI9KERDS-Al4DTBDRV04w

TensorFlow XLA工作原理简介

https://sketch2sky.com/

一个XLA方面的blog

https://tensorflow.juejin.im/performance/xla/jit.html

使用即时编译

# Eigen

Eigen是一个线性代数方面的C++模板库。tensorflow和caffe2都使用了这个库。

官网：

http://eigen.tuxfamily.org/

使用Eigen也比较简单，无须link，只要引用相关头文件即可。

参见：

https://zhuanlan.zhihu.com/p/26512099

tensorflow和caffe2

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

# TFRecord

TFRecord是TensorFlow官方定义的存放样本数据文件。

参考：

http://www.cnblogs.com/antflow/p/7299029.html

TFRecord的使用

https://zhuanlan.zhihu.com/p/27481108

TensorFlow直接读取图片和读写TFRecords速度对比

# 多核(multicore)，多线程(multi-thread)

在Tensorflow程序中，我们会经常看到”with tf.device("/cpu:0"): “ 这个语句。单独使用这个语句，而不做其他限制，实际上默认tensorflow程序占用所有可以使用的内存资源和CPU核。

参考：

http://deepnlp.org/blog/tensorflow-parallelism/

Tensorflow并行：多核(multicore)，多线程(multi-thread)

# 控制流

## tf.cond

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

## tf.case

```python
decode_png = lambda :tf.image.decode_png(image_tensor, channels)
decode_jpg = lambda :tf.image.decode_jpeg(image_tensor, channels)
decoder = { tf.equal(image_ext, '.png'):  decode_png,
            tf.equal(image_ext, '.jpg'):  decode_jpg}
image_tensor = tf.case(decoder, default = decode_png, exclusive = True)
```

# 内存布局

Tensorflow和Caffe的内存布局存在较大差异，这是两者模型转换时，最常遇到的问题。一般认为，Caffe的内存布局对卷积硬件加速更友好一些。

|  | Tensorflow | Caffe |
|:--:|:--:|:--:|
| Tensor | NHWC | NCHW |
| Weight | HWIO | OIHW |

# TFLite

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

tflite模型使用flatbuffers进行序列化，因此也可以使用flatbuffers解析相关模型。

需要注意的是flatbuffers生成的代码，有两种版本：

- 精简版。默认设置。网上的解析代码用的都是这个版本。缺点：无法修改相应的模型。

- 专业版。`--gen-object-api`。新增`UnPack/UnpackTo/Pack`方法，进行对象结构体与table结构体间的转换。

专业版不是所有语言都有，至少ubuntu自带的flatc就没有提供对python的专业版支持。但是tensorflow自带的flatc是可以的。

`bazel build //tensorflow/lite/tools:visualize`

这个命令会生成一个schema_py_generated.py文件，也就是所谓的专业版本了。

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
