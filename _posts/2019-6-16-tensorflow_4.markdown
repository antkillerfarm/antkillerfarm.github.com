---
layout: post
title:  TensorFlow（四）
category: AI 
---

* toc
{:toc}

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

# Hama

TensorFlow实际上是Google开发的第二代DL框架。在它之前，Google内部还有一个叫做DistBelief的框架。这个框架没有开源，但是有论文发表。因此，就有了一个叫做Apache Hama的项目，作为它的开源实现。

官网：

https://hama.apache.org/

这个项目采用了一种叫做Bulk Synchronous Parallel的并行计算模型。

# Tensorflow 2.x

![](/images/img3/TF.png)

![](/images/img3/TF_2.png)

```python
import tensorflow
main_version = tensorflow.__version__.split('.')[0]
if int(main_version) == 2:
    import tensorflow.compat.v1 as tf
    tf.compat.v1.disable_v2_behavior()
    import tensorflow.compat.v1.lite as tflite
else:
    import tensorflow as tf
    import tensorflow.contrib.lite as tflite
```

https://mp.weixin.qq.com/s/BD-nJSZJLjBBq1n7HEHpKw

将您的代码升级至TensorFlow 2.0

https://mp.weixin.qq.com/s/xgsUF97aI1YfGSdh0FJ6Cw

都在关心TensorFlow 2.0，那我手里基于1.x构建的程序怎么办？

https://mp.weixin.qq.com/s/s8hAYadCw9-_BpWSCh38gg

TensorFlow 2.0：数据读取与使用方式

https://mp.weixin.qq.com/s/rVSC1AXj9YECjUrl5PkSGw

详解深度强化学习展现TensorFlow 2.0新特性

https://mp.weixin.qq.com/s/8D8kxFSfruwWhU2jmYL3sg

Google大佬Josh Gordon发布Tensorflow 2.0入门教程

https://cloud.tencent.com/developer/article/1498043

有了TensorFlow2.0，我手里的1.x程序怎么办？

https://mp.weixin.qq.com/s/ddHKc5AffznRaEY_qhHN_g

升级到tensorflow2.0，我整个人都不好了

https://mp.weixin.qq.com/s/RcolwQnCqrAsGaKEK0oo_A

TensorFlow 2.0中的tf.keras和Keras有何区别？为什么以后一定要用tf.keras？

# 细节

执行`session.run(out)`，会在终端打印out的值，但执行`res = session.run(out)`则不会。

此外，`session.run`可以接受list作为参数。返回值也是一个list，分别对应输入list的每个元素的计算结果。

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

----

CNN中的padding：

"SAME" = with zero padding。

"VALID" = without padding。

----

op的自定义实现可使用`tf.py_func`。

----

tf.dtypes.cast: 类型转换

# TensorSensor

https://mp.weixin.qq.com/s/ZxmoBcWJa7luGOHQ32ru1A

推荐一个快速定位深度学习代码bug的炼丹神器

# TensorNetwork

TensorFlow的计算图模型不仅可以用于DL领域，亦可应用于其他科学计算领域。TensorNetwork就是一个基于TensorFlow的张量运算库。现成的矩阵运算库已经很多了，这次升级为张量运算库了。

https://github.com/google/TensorNetwork

参考：

https://mp.weixin.qq.com/s/jdjX0jirTHOUqsGagJmGLQ

谷歌AI开源张量计算库TensorNetwork，计算速度暴涨100倍

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

# Debug/Profiling

## 设置python

Setting中搜索`python path`，设置路径类似于：`/anaconda3/envs/mlbook/bin/python`

打开python文件，在状态栏有python版本的提示，点击该提示，可以切换不同的python版本。

## gdb调试

Tensorflow App，一般是从python开始的，因此需要掌握python+C的混合调试方法。

在所有模块import之后（否则后面的gdb加载不到相关的符号），添加如下语句：

`input("pid: " + str(os.getpid()) +", press enter after attached")`

启动gdb，使用attach命令，attach到相关的进程。设置断点，然后continue即可。

参考：

https://sketch2sky.com/2019/08/25/tensorflow-debugtrick/

Tensorflow XLA Debug/Profiling Methods

https://www.cnblogs.com/djzny/p/4956752.html

gdb命令中attach使用

## vscode调试

vscode调试同样需要两段式的方法：

https://nadiah.org/2020/03/01/example-debug-mixed-python-c-in-visual-studio-code/

Example debugging mixed Python C++ in VS Code

相关配置文件参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/vscode/launch.json

## profiling

`pip install -U tensorboard-plugin-profile`

```python
from tensorflow.profiler.experimental import Profile

with Profile('/logdir_path'):
    # do sth
```

参考：

https://zhuanlan.zhihu.com/p/140343833

tensorflow profiling工具简介——tensorflow原生工具

https://www.tensorflow.org/tensorboard/tensorboard_profiling_keras

TensorBoard性能分析:在Keras中对基本训练指标进行性能分析

https://github.com/tensorflow/benchmarks

TensorFlow benchmarks

https://blog.csdn.net/zkbaba/article/details/106178542

TensorFlow性能分析工具—TensorFlow Profiler

# 参考

https://mp.weixin.qq.com/s/t1QFIOq-VBNOrSm0zW-PlQ

深度学习TensorFlow实现集合

https://mp.weixin.qq.com/s/IzijD8Sh3G2WsCz7aaxyhg

TensorFlow深度学习概述

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

https://github.com/taki0112/Tensorflow-Cookbook

1500+星标，简单易用TensorFlow代码集

https://mp.weixin.qq.com/s/bjxJyOitynRtCoW0FX1gXw

一文带你入门Tensorflow

https://mp.weixin.qq.com/s/zmTqWNXlYcDyZb_dmEo_5Q

TensorFlow/PyTorch/Sklearn实现的五十种机器学习模型

https://mp.weixin.qq.com/s/O5vvGKHWkJQWzeiL7A_S_g

TensorFlow简单介绍

https://mp.weixin.qq.com/s/JSZwQkyxSSwfBWKJ578j3A

TensorFlow最好的入门文章

https://mp.weixin.qq.com/s/68vaQRqUo8u09iheKzFVEw

玩转TensorFlow深度学习

https://mp.weixin.qq.com/s/Es_5KUnkDzMwf_8WD8aW3g

GitHub万星：适用于初学者的TensorFlow代码资源集

https://mp.weixin.qq.com/s/GaK_iSTBl7B4LTdaOtiR_Q

香港科技大学TensorFlow课件分享

https://mp.weixin.qq.com/s/RR3EEI8vm05EZSd7dGU__A

史上最全的Tensorflow学习资源汇总，速藏！

https://mp.weixin.qq.com/s/w4uo9Uodmud4FhqxRNanUw

TensorFlow指南：GPU的使用

https://mp.weixin.qq.com/s/gnDTOLWuPZiCVzspTk_zCQ

TensorFlow轻度入门

https://mp.weixin.qq.com/s/sbJdU7qMMyrSRuybTh7bKg

开源中文书《TensorFlow内核剖析》，335页pdf

https://mp.weixin.qq.com/s/5wy6yqaW_9pMBhgw8qDdOQ

基于TensorFlow打造强化学习API

https://mp.weixin.qq.com/s/MYBTWL3X_OhLZL6C4rISzw

TensorFlow训练线性回归

https://mp.weixin.qq.com/s/5QYlh6gV9IqdQfraK4DC8w

10种深度学习算法的TensorFlow实现

https://mp.weixin.qq.com/s/W1KP213Ngj-BNEyx-_nVyw

利用TensorFlow实现卷积自编码器

https://mp.weixin.qq.com/s/2COA8aRQBpxaihKnlLkXZQ

快速上手图像识别：用TensorFlow API实现图像分类实例

https://mp.weixin.qq.com/s/TZMOO_LFCxk297lKNQfvGQ

TensorFlow从基础到实战：一步步教你创建交通标志分类神经网络

https://mp.weixin.qq.com/s/HUUwtyjRllg-5olqYHK4XA

基于TensorFlow的开源项目FaceRank

https://mp.weixin.qq.com/s/N2OP1uX7JjfIJQ_B4NHKpw

横向对比三大分布式机器学习平台：Spark、PMLS、TensorFlow

https://mp.weixin.qq.com/s/bJigyEv6iNDp799KlHsclg

TensorFlow 不仅用于机器学习，还能模拟偏微分方程（水滴特效）

https://mp.weixin.qq.com/s/dmNEzrWNX3YWmocjpORpig

谷歌开源TF-Ranking可扩展库，支持多种排序学习

https://mp.weixin.qq.com/s/n_zU7Rg7v6PwjZWEF88fNA

如何使用TensorFlow实现音频分类任务

https://mp.weixin.qq.com/s/UbBJYOmWtUXPFliRMyzDrg

最新TensorFlow专业深度学习实战书籍和代码《Pro Deep Learning with TensorFlow》
