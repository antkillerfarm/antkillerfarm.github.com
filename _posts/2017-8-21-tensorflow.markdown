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

TensorFlow的官方教程：

http://tensorflowtutorial.net/tensorflow-tutorial

教程中文版：

http://wiki.jikexueyuan.com/project/tensorflow-zh/

TensorFlow中文社区：

http://www.tensorfly.cn/

安装：

`sudo pip install tensorflow`

由于我的PC显卡不合要求，因此直接安装的是CPU版本，这也是最通用的版本。

TensorFlow技术栈：

![](/images/article/tensorflow_layer.png)

## CS 20SI

斯坦福最近专门为Tensorflow开设了一门课程：CS 20SI: Tensorflow for Deep Learning Research。

网址：

https://web.stanford.edu/class/cs20si/syllabus.html

课程的主讲Chip Huyen，一个越南妹子，目前在斯坦福读本科（大三）。应该说本科生上讲台的确是一件稀罕事，在这里为斯坦福的学术氛围点赞。

个人主页：

https://huyenchip.com

不用为课程的质量担心，Chip Huyen的师兄们客串了很多节课，Andrej Karpathy为课程设计了网页。

Chip Huyen可谓是一战成名。但也带来了烦恼，再去请教别人的时候，总有助教或同学会反问：“这些你不该早就知道了么？你不就是教这些的么？”

参见：

http://www.sohu.com/a/164277987_473283

一名在斯坦福教授TensorFlow教师的“忏悔”：我觉得自己像个骗子

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
bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg #生成wheel文件
{% endhighlight %}

**Step 3**：安装TensorFlow。

`sudo pip uninstall tensorflow`

`sudo pip install /tmp/tensorflow_pkg/tensorflow-1.3.0-cp27-cp27mu-linux_x86_64.whl`

参考：

http://www.jianshu.com/p/b1faa10c9238

TensorFlow CPU环境SSE/AVX/FMA指令集编译

http://www.hankcs.com/ml/compile-and-install-tensorflow-from-source.html

从源码编译安装TensorFlow

http://blog.csdn.net/sinat_28731575/article/details/74633476

Mac下使用源码编译安装TensorFlow CPU版本

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

参见：

http://geek.csdn.net/news/detail/126133

如何用TensorFlow和TF-Slim实现图像分类与分割

http://www.infoq.com/cn/articles/introduction-of-tensorflow-part06

深入浅出TensorFlow（六）TensorFlow高层封装

实战心得：

tf-slim-mnist例子中mnist数据不是原始格式的，而是经过了`datasets/download_and_convert_mnist.py`的转换。

该示例执行时也没有控制台的输出信息，一度让我觉得很不方便。后来才发现，原来可以用TensorBoard查看log文件夹。

TensorBoard是一个http服务，用以监控TensorFlow的执行。

`tensorboard --logdir=log`

启动之后，用浏览器打开`http://localhost:6006`即可。

## 参考

https://mp.weixin.qq.com/s/IzijD8Sh3G2WsCz7aaxyhg

TensorFlow 深度学习概述

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

http://mp.weixin.qq.com/s/JZ1ceGQDmQUaNW5wl6biLA

TensorFlow实现流行机器学习算法教程汇集

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

https://mp.weixin.qq.com/s/zZCEOdNQsPovn_i-C57Z9g

如何使用最流行框架Tensorflow进行时间序列分析？

https://mp.weixin.qq.com/s/CqOo7Fu6t5-yJiYhzo03oQ

利用TensorFlow和神经网络来处理文本分类问题

https://mp.weixin.qq.com/s/VlvQmrS7Qi2qq6fTBXKTYw

从零开始用TensorFlow搭建卷积神经网络

https://mp.weixin.qq.com/s/hETnA81WlkMG3rftAHg9bw

PyTorch和TensorFlow哪家强：九项对比读懂各自长项短板

http://blog.csdn.net/wiinter_fdd/article/details/72821923

Tensorflow中的模型持久化

https://mp.weixin.qq.com/s/7R-Gvegnta9XBwIaSPBL_Q

基于Tensorflow的验证码识别

https://mp.weixin.qq.com/s/Es_5KUnkDzMwf_8WD8aW3g

GitHub万星：适用于初学者的TensorFlow代码资源集

https://mp.weixin.qq.com/s/3QgtemxxsQmuNQVEdpiMwA

如何做准确率达98%的交通标志识别系统？

https://mp.weixin.qq.com/s/pSE2V8wD3_KHMI71kLTXng

如何基于TensorFlow使用LSTM和CNN实现时序分类任务

https://mp.weixin.qq.com/s/dHkmDvFVUGmt4Ch-gv3s1g

一步一步带你用TensorFlow玩转LSTM

https://mp.weixin.qq.com/s/Bx5Djj-RE0jPJ7LjyQ7GPg

基于gym和tensorflow的强化学习算法实现

## 我的TensorFlow实践

### MNIST+Softmax

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_mnist.py

### MNIST+CNN

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_cnn.py

第一个例子中，我对CPU的计算能力还没有切肤之痛，但在这里使用CPU差不多要花半个小时时间。。。


