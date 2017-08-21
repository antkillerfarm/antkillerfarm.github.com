---
layout: post
title:  TensorFlow
category: technology 
---

# TensorFlow

## 概述

TensorFlow是Google主导的开源深度学习库。官网：

https://www.tensorflow.org/

代码：

https://github.com/tensorflow/tensorflow

TensorFlow提供了一个可视化的神经网络展示：

http://playground.tensorflow.org/

还有若干已经实现好的经典神经网络模型（比如Autoencoder、ResNet等）：

https://github.com/tensorflow/models/

TensorFlow的教程：

http://tensorflowtutorial.net/tensorflow-tutorial

教程中文版：

http://wiki.jikexueyuan.com/project/tensorflow-zh/

安装：

`pip install tensorflow`

由于我的PC显卡不合要求，因此直接安装的是CPU版本，这也是最通用的版本。

TensorFlow技术栈：

![](/images/article/tensorflow_layer.png)

## 源代码编译

**Step 1**：安装Bazel。

{% highlight bash %}
echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -
sudo apt-get update && sudo apt-get install bazel
{% endhighlight %}

**Step 2**：编译TensorFlow。

{% highlight bash %}
./configure
# configure的时候要选择一些东西是否支持，这里建议都选N，不然后面会包错，如果支持显卡，就在cuda的时候选择y
bazel build --config=opt //tensorflow/tools/pip_package:build_pip_package # CPU only
bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package # GPU
{% endhighlight %}

参考：

http://www.jianshu.com/p/b1faa10c9238

TensorFlow CPU环境SSE/AVX/FMA指令集编译

## 基本概念

**Variables**：训练模型的时候，那些在样本之间表示结点状态的变量。一般来说，就是神经网络的参数。

**Placeholders**：对于每个样本都不相同的变量。比如神经网络的输入变量x和输出变量y。

## 图计算

图计算是各个深度学习框架的中心概念，这里单独提出来讨论一下。

![](/images/article/fused_graph.png)

上图是softmax运算的计算图示例。计算图是一个有向无环图，它的结点代表原始数据和计算中间结果，边代表数据的流向。计算操作被称为**Operation**。

因此，**计算图实际上是工作流（work flow）思想在数值计算领域的拓展**。这种流水线一般被称为**Graph**。

流水线有了，还要有被加工的数据。数据在流水线上被加工的执行过程，被称为**Session**。

Graph和Session的关系，类似于类和对象的关系。Session是Graph的动态实例。

图计算的大致步骤如下：（这里以OpenVX函数为例，因为它更接近底层和硬件。）

1.vxCreateGraph。创建计算图。

2.vxProcessGraph。运行计算图。

在大多数Tensorflow示例中，你看不到Graph的身影。但它并不是不存在，而是默认所有新加入的Operation都添加到默认的Graph。

以下是使用多个Graph的示例：

{% highlight python %}
import tensorflow as tf
g1 = tf.Graph()
with g1.as_default():
    c1 = tf.constant([1.0])
with tf.Graph().as_default() as g2:
    c2 = tf.constant([2.0])

with tf.Session(graph=g1) as sess1:
    print sess1.run(c1)
with tf.Session(graph=g2) as sess2:
    print sess2.run(c2)
{% endhighlight %}

Tensorflow对计算图的简化，不仅在于使用默认的Graph。还在于可以只计算部分的Graph。以上面的softmax运算为例，如果`sess.run(add)`的话，后面的ReLU和softmax运算都不会被执行。

参考：

http://www.algorithmdog.com/dynamic-tensorflow

动态图计算：Tensorflow 第一次清晰地在设计理念上领先

https://zhuanlan.zhihu.com/p/23932714

YJango的TensorFlow整体把握

http://www.cnblogs.com/lienhua34/p/5998853.html

Tensorflow学习笔记2：About Session, Graph, Operation and Tensor

## Fused Graph

Fused Graph是TensorFlow新推出的概念。这里以softmax运算为例，讲一下它的基本思想。

上面的softmax运算计算图中，总共有4个operation。Fused Graph则将这4个op整合为1个op，发给运算单元。

这样不同的硬件厂商就可以自行对这个整合的op进行解释。功能强的硬件，可能直接就支持softmax运算。功能弱的硬件也不怕，反正总归可以将softmax分解为基本运算的。

Qualcomm Hexagon平台的Fused Graph实现可参见：

tensorflow/core/kernels/hexagon

参考：

https://developers.googleblog.com/2017/03/xla-tensorflow-compiled.html

XLA - TensorFlow, compiled

## Eigen

Eigen是一个线性代数方面的C++模板库。tensorflow和caffe2都使用了这个库。

官网：

http://eigen.tuxfamily.org/

参见：

https://zhuanlan.zhihu.com/p/26512099

tensorflow和caffe2

## Slim

原始的TensorFlow API由于过于简单，在实际使用中多有不便。因此TensorFlow又提供了一套Slim API，用以简化编程。和Keras这样的前端不同，Slim API可以和TensorFlow API混编。或者说两者本来就是一体的。

代码：

tensorflow/contrib/slim

示例：

https://github.com/mnuke/tf-slim-mnist

## 参考

https://github.com/yahoo/TensorFlowOnSpark

TensorFlow On Spark

https://silencezjl.coding.me/2017/05/01/%E5%81%B7%E4%B8%80%E6%B3%A2%E8%B5%84%E6%BA%90/

各种TensorFlow资源

https://github.com/zsdev2015/machine_learning

某国内小牛写的中文入门demo，注释非常详细

https://morvanzhou.github.io/tutorials/

一个以python语言教学的ML、DL教程，比较通俗易懂。

https://mp.weixin.qq.com/s/gJBDXf_5ViPR9dNm3eH2Hg

TensorFlow初学者必须了解的55个经典案例

https://mp.weixin.qq.com/s/bjxJyOitynRtCoW0FX1gXw

一文带你入门Tensorflow

https://mp.weixin.qq.com/s/zmTqWNXlYcDyZb_dmEo_5Q

TensorFlow/PyTorch/Sklearn实现的五十种机器学习模型

https://mp.weixin.qq.com/s/O5vvGKHWkJQWzeiL7A_S_g

TensorFlow简单介绍

https://mp.weixin.qq.com/s/OmVAnkHV2aI4D4pMKyVjCQ

基于TensorFlow理解三大降维技术：PCA、t-SNE和自编码器

https://mp.weixin.qq.com/s/5wy6yqaW_9pMBhgw8qDdOQ

基于TensorFlow打造强化学习API

https://mp.weixin.qq.com/s/68vaQRqUo8u09iheKzFVEw

玩转TensorFlow深度学习

https://mp.weixin.qq.com/s/TZMOO_LFCxk297lKNQfvGQ

TensorFlow从基础到实战：一步步教你创建交通标志分类神经网络

http://blog.csdn.net/u012436149

一个TensorFlow+PyTorch的blog

https://mp.weixin.qq.com/s/HUUwtyjRllg-5olqYHK4XA

基于TensorFlow的开源项目FaceRank

https://mp.weixin.qq.com/s/N2OP1uX7JjfIJQ_B4NHKpw

横向对比三大分布式机器学习平台：Spark、PMLS、TensorFlow

https://github.com/jinfagang/rl_atari_pytorch

ReinforcementLearning Learn Play Atari Using DDPG and LSTM.

https://mp.weixin.qq.com/s/JSZwQkyxSSwfBWKJ578j3A

TensorFlow最好的入门文章

https://mp.weixin.qq.com/s/jMPVl3CWvL7MSzq5F12YxQ

维度、广播操作与可视化：如何高效使用TensorFlow

https://mp.weixin.qq.com/s/EytvywrsgydXAJQhuUqKvg

简易浣熊识别器是如何实现的

https://mp.weixin.qq.com/s/YOyOR8fdaEKcydAywcc-HA

如何使用TensorFlow API构建视频物体识别系统

https://mp.weixin.qq.com/s/gnDTOLWuPZiCVzspTk_zCQ

TensorFlow轻度入门

https://mp.weixin.qq.com/s/MYBTWL3X_OhLZL6C4rISzw

TensorFlow训练线性回归

http://www.jianshu.com/p/d443aab9bcb1

在TensorFlow上使用LSTM进行情感分析

https://mp.weixin.qq.com/s/5QYlh6gV9IqdQfraK4DC8w

10种深度学习算法的TensorFlow实现

https://zhuanlan.zhihu.com/p/28475975

如何优雅地用TensorFlow预测时间序列：TFTS库详细教程

## 我的TensorFlow实践

### MNIST+Softmax

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_mnist.py

### MNIST+CNN

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_cnn.py

第一个例子中，我对CPU的计算能力还没有切肤之痛，但在这里使用CPU差不多要花半个小时时间。。。

# Machine Learning之Python篇

## iPython

ipython是一个python的交互式 shell，比默认的python shell 好用得多，支持变量自动补全，自动缩进，支持 bash shell 命令，内置了许多很有用的功能和函数。

在较新的ipython版本中，添加了ipython notebook的功能，弥补了ipython shell下代码不易保存等缺点，并且在使用--pylab inline选项后，可以在代码执行后立即显示运行结果（包括图片，数据表格等），因此在数据分析中运用十分广泛。

`sudo apt-get install ipython ipython-notebook`

## Jupyter

Jupyter是iPython的后继项目，它不仅支持python语言，还支持其他50多种交互式语言。成为目前最流行的交互式shell和数据文本交换格式。

官网：

https://jupyter.org/

`pip install jupyter`

参见：

https://mp.weixin.qq.com/s/UXlPhX3Vb2yqocpUH_3W5w

Jupyter项目的前世今生

## 参考

https://github.com/neozhaoliang/pywonderland/tree/master/

如何用Python画各种著名数学图案

http://old.sebug.net/paper/books/scipydoc/index.html

用Python做科学计算

https://pan.baidu.com/s/1qYPUHNQ

小抄合集

https://mp.weixin.qq.com/s/HKHLiCqEAtoCAqf4CHv86w

python机器学习算法代码实现

https://mp.weixin.qq.com/s/1--i1OhxRNPvNnmP9bFo3g

全机器学习和Python的27个速查表

https://mp.weixin.qq.com/s/PtZ3YvFrUgbK5y3pcAeW9A

使用OpenCV和Python实现的机器学习

https://mp.weixin.qq.com/s/wbcAIrI0eNALfKhgkZQYTQ

用Python也能进军金融领域？这有一份股票交易策略开发指南

https://mp.weixin.qq.com/s/e--IeRTRZMqhs_DSJKpgyQ

特征工程之Scikit-learn

https://mp.weixin.qq.com/s/z3N5v4H_W2sxh2XPpFbAcA

除了Python，这些语言写的机器学习项目也很牛

https://mp.weixin.qq.com/s/U2cDVRxZPVsqoL-zHn__CA

Python做机器学习之路

https://mp.weixin.qq.com/s/LgVS5N80UlCeEfrPtyUF4Q

深度学习矩阵运算的概念和代码实现

https://mp.weixin.qq.com/s/0XteuIk71qSpxrZPGVnMbg

Python3实现K-近邻算法

https://mp.weixin.qq.com/s/bGudwxu8c0LIxv1h64qZtQ

Python3实现决策树算法

https://mp.weixin.qq.com/s?__biz=MzA3MTM3NTA5Ng==&mid=2651056396&idx=1&sn=dabbda2c433e54a2bad506fc13bfd743

<<战狼Ⅱ>>豆瓣十二万影评浅析

https://mp.weixin.qq.com/s/fXI5suCVna6fBxPnVyKevw

浅谈NumPy和Pandas库



