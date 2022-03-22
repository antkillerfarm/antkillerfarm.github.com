---
layout: post
title:  TensorFlow（五）
category: AI 
---

* toc
{:toc}

# TensorFlow.js（续）

https://mp.weixin.qq.com/s/NO_XY-JmTpIkoC-fpkZ-qg

在浏览器上也能训练神经网络？TensorFlow.js带你玩游戏~

https://mp.weixin.qq.com/s/vjpMr3TsF3Lui8Q0IstQxw

浏览器上跑：TensorFlow发布实时人物分割模型，秒速25帧，24个部位

https://mp.weixin.qq.com/s/-BblgnvPLuqpYM8PZ7PQCQ

三行代码实时追踪你的手，只要有浏览器就够了

https://mp.weixin.qq.com/s/C7QdVathJ8YTXF-zXPC-Ow

有人分析了7个基于JS语言的DL框架，发现还有很长的路要走

# Estimator

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

https://mp.weixin.qq.com/s/zpEVU1E5DfElAnFqHCqHOw

训练效率低？GPU利用率上不去？快来看看别人家的tricks吧～

# Log

TF里有两套Log系统：`LOG`和`VLOG`。

`LOG`由`TF_CPP_MIN_LOG_LEVEL`控制，值越小，信息越多。

`VLOG`都是INFO级别的Log，因此，`TF_CPP_MIN_LOG_LEVEL`必须为0。此外，`VLOG`本身亦有不同等级，可使用`TF_CPP_MIN_VLOG_LEVEL`控制，值越大，信息越多。

# loss & accuracy

loss：训练集损失值

accuracy:训练集准确率

val_loss:测试集损失值

val_accruacy:测试集准确率

以下5种情况可供参考：

train loss 不断下降，test loss不断下降，说明网络仍在学习;（最好的）

train loss 不断下降，test loss趋于不变，说明网络过拟合;（max pool或者正则化）

train loss 趋于不变，test loss不断下降，说明数据集100%有问题;（检查dataset）

train loss 趋于不变，test loss趋于不变，说明学习遇到瓶颈，需要减小学习率或批量数目;（减少学习率）

train loss 不断上升，test loss不断上升，说明网络结构设计不当，训练超参数设置不当，数据集经过清洗等问题。（最不好的情况）

参考：

https://www.cnblogs.com/Timeouting-Study/p/12591448.html

TensorFlow中loss与val_loss、accuracy和val_accuracy分别是什么含义

# Eager Execution

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

https://github.com/snowkylin/tensorflow-handbook

简单粗暴TensorFlow 2.0

https://mp.weixin.qq.com/s/zz8XCykJ6jxbE5J4YwAkEA

一招教你使用tf.keras和eager execution解决复杂问题

# tf.data

tf.data提供了一套构建灵活高效的输入流水线的API。

![](/images/img2/datasets_without_pipelining.png)

![](/images/img2/datasets_with_pipelining.png)

上面两幅图中，第一幅图是没有使用流水线的情况，而第二幅图则是使用流水线的情况。

参考：

https://mp.weixin.qq.com/s/dfXTV4PFgC1Wbti42Zf4wQ

tf.data API，让你轻松处理数据

https://mp.weixin.qq.com/s/mjUnrPBPBuY6XKXkUymX-w

实例介绍TensorFlow的输入流水线

https://mp.weixin.qq.com/s/1ZlyVDJK6RWZ_1Ox7399IA

用一行tf.data实现数据Shuffle、Batch划分、异步预加载等

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

# cross-compile

TF的交叉编译不是不好用，而是非常不好用。。。这一点其实Google内部也心知肚明。但凡能不用bazel的地方，其实Google也不想用，比如TF Lite就提供了CMake的编译选项。

但TF由于是个跨语言的项目（至少包含了Python和C++），所以Bazel还是有一定的优势的。

网上关于TF交叉编译的文章不多，写的比较好的主要有：

https://www.morethantechnical.com/2018/03/08/cross-compile-latest-tensorflow-1-5-for-the-nvidia-jetson-tk1/

Cross-compile latest Tensorflow (1.5+) for the Nvidia Jetson TK1

然而这个已经有点年头了，并不适合新版本的bazel。

其实目前官方代码库中，已经有一些交叉编译的例子了。比如raspberry pi的：

`./tensorflow/tools/ci_build/pi/build_raspberry_pi.sh AARCH64`

TF也有一个repo用于放置工具链相关的内容：

https://github.com/tensorflow/toolchains.git

我这里是参考`toolchains/cpus/arm/cc_config.bzl.tpl`来编写适合自己的脚本。

目前可行的编译选项如下：

```bash
bazel build --crosstool_top=//cross_compiler:toolchain --cpu=aarch64 --host_crosstool_top=@bazel_tools//tools/cpp:toolchain --distinct_host_configuration=true --config=opt //tensorflow/tools/pip_package:build_pip_package
```

相关选项的含义如下：

`--crosstool_top`：指定交叉编译的工具链。

`--host_crosstool_top`：有些项目实际上需要编译Host版本，比如flatbuffers。这些项目的bazel文件中，往往能找到类似`cfg = "host"`的选项。因此，这里还需要指定Host的工具链。当然Host的工具链一般都是系统自带的，也许并不需要特殊指定，这时可以使用`@bazel_tools//tools/cpp:toolchain`这样的默认设置。

`--distinct_host_configuration=true`：即使配置好Host的工具链，也不代表项目会被按照Host编译。这时就需要打开这个开关了。

最后是打包wheel的环节：

虽然打包出来的wheel文件名字叫做`tensorflow-2.7.0-cp39-cp39-linux_x86_64.whl`，但是不要紧，相关的cross-compile的内容已经在里面了，只需要将之改名字为`tensorflow-2.7.0-cp39-none-linux_aarch64.whl`即可。

---

以下是一些趟坑的细节：

一般来说，标准库的头文件是不需要加入项目的依赖的，如果bazel报这方面的问题，设置一下`cxx_builtin_include_directories`即可。

`__float128`只存在于X86体系下。如果报错，多半是工具链没有设置到`aarch64`的头文件路径下。

编译python包的话，还需要相应平台提供python-dev的环境，不然这些也要自己搞定。

---

参考：

https://bazel.build/tutorials/cc-toolchain-config

Bazel Tutorial: Configure C++ Toolchains

https://github.com/bazelbuild/bazel/issues/1353

Using bazel to cross-compile tensorflow for other targets.

https://www.cnblogs.com/jojodru/p/7744630.html

在Ubuntu 16.04上使用bazel交叉编译tensorflow

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

https://mp.weixin.qq.com/s/MYBTWL3X_OhLZL6C4rISzw

TensorFlow训练线性回归

https://mp.weixin.qq.com/s/5QYlh6gV9IqdQfraK4DC8w

10种深度学习算法的TensorFlow实现

https://mp.weixin.qq.com/s/W1KP213Ngj-BNEyx-_nVyw

利用TensorFlow实现卷积自编码器

https://mp.weixin.qq.com/s/pIESRzjsmqoO46P4x5Iqhw

Tensorflow卷积神经网络

https://mp.weixin.qq.com/s/Cge_GY19aZ1AcMkhW93C1A

TensorFlow中的那些高级API

https://mp.weixin.qq.com/s/kYOwUWlTP4T0IYKDWDbCsg

tensorflow object detection API训练公开数据集Oxford-IIIT Pets Dataset
