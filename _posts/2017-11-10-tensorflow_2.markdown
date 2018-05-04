---
layout: post
title:  TensorFlow（二）
category: AI 
---

# TensorFlow

## 模型文件（续）

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

## 可视化神经网络结构

`writer = tf.summary.FileWriter("logs/", sess.graph)`

然后

`tensorboard --logdir='logs/'`

参考：

http://blog.csdn.net/u013082989/article/details/53510625

TensorFlow学习_01_安装_基本操作_可视化结构、过程_Mnist

## 权值保持不变

迁移学习的时候，有的时候需要保持某几层的权值，在后续训练中不被改变。这时，可以在创建Variable时，令trainable=false。

## 我的TensorFlow实践

### MNIST+Softmax

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_mnist.py

### MNIST+CNN

代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/hello_cnn.py

第一个例子中，我对CPU的计算能力还没有切肤之痛，但在这里使用CPU差不多要花半个小时时间。。。

### 框架怀古（2017.9）

http://deeplearning.net/

这个网站是Theano的主站，也是我最早接触DL时浏览的网站。其时，我虽然对DL有浓厚的兴趣，但尚未以此作为工作内容。

从该网站提供的招聘信息来看，Caffe、Theano、Torch是当时主流的三大框架库。

岂料时隔一年半载之后，这三大框架都渐趋式微。

Caffe被Caffe 2替代，但使用的广泛度仍超过后者。

Theano被同样基于计算图的TensorFlow淘汰。2017年9月停止更新。

Torch相对变动最小，它被PyTorch替代。这更可以看作是python对于lua的胜利。

## TFLite

Tensorflow源代码中自带的toco工具，可用于生成一个可供TensorFlow Lite框架使用的tflite文件。

代码：

https://github.com/tensorflow/tensorflow/tree/master/tensorflow/contrib/lite/toco

参考：

https://www.jianshu.com/p/fa204a54a956

生成TFLite模型文件

## blog

http://www.jianshu.com/u/eaec1fc422e9

一个TF的blog

http://blog.csdn.net/u012436149

一个TensorFlow+PyTorch的blog

## 参考

https://mp.weixin.qq.com/s/IzijD8Sh3G2WsCz7aaxyhg

TensorFlow深度学习概述

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

https://mp.weixin.qq.com/s/7er3wNV_IhxhFDOIwNMpww

深度强化学习入门：用TensorFlow构建你的第一个游戏AI

https://mp.weixin.qq.com/s/TZMOO_LFCxk297lKNQfvGQ

TensorFlow从基础到实战：一步步教你创建交通标志分类神经网络

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

https://mp.weixin.qq.com/s/gW_KX6eF9XEsSUO1UzJ3WQ

基于LSTM的情感分析

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

https://mp.weixin.qq.com/s/3URLEdhB8hs0XXekKbvsnw

使用TensorFlow在卷积神经网络上实现L2约束的softmax损失函数

https://mp.weixin.qq.com/s/dgLJrn3omUKMqmqTIEcoyg

Tensorflow实现DDPG

https://mp.weixin.qq.com/s/FdPrRQr0LukcWh7B703MlQ

利用tf.gradients在TensorFlow中实现梯度下降

https://mp.weixin.qq.com/s/a68brFJthczgwiFoUBh30A

TensorFlow数据集和估算器介绍

https://github.com/indiejoseph/cnn-text-classification-tf-chinese

CNN for Chinese Text Classification in Tensorflow

https://mp.weixin.qq.com/s/yc1ssCzaPzI4UUsl4jl5Yw

用TensorFlow和TensorBoard从零开始构建ConvNet

https://mp.weixin.qq.com/s/hquOoKeeHQXqWcHM6Bkvbw

如何训练一个简单的音频识别网络

https://mp.weixin.qq.com/s/KohwsQQetwjfTj-PXvLjwA

使用MNIST数据集，在TensorFlow上实现基础LSTM网络

http://mp.weixin.qq.com/s/ioaS7RQ6bsJs4_X0G4ZHyQ

如何优雅地用TensorFlow预测时间序列：TFTS库详细教程

http://mp.weixin.qq.com/s/hpv6bzr-5VZet-UCHOCQLQ

谷歌TFX：基于TensorFlow可大规模扩展的机器学习平台

https://mp.weixin.qq.com/s/GaK_iSTBl7B4LTdaOtiR_Q

香港科技大学TensorFlow课件分享

https://mp.weixin.qq.com/s/cPWXAI2TBv3_ssnWDFoQ4w

TensorFlow sucks，有人吐槽TensorFlow晦涩难用

https://mp.weixin.qq.com/s/_kr28kN0_1QFP8BR_wGo5w

TensorFlow RNN入门

https://mp.weixin.qq.com/s/WqE-FRl-Thys7tHUvFNlWQ

盯住梅西：TensorFlow目标检测实战

https://mp.weixin.qq.com/s/WfzlHtz0FFJMsPFwPoMqJg

如何利用VGG-16等模型在CPU上测评各深度学习框架

http://www.jianshu.com/p/4e16ae0aad25

利用TensorFlow入门Word2Vec

https://mp.weixin.qq.com/s/YJmMfBhQ3cLNUp_HHsXhGA

手把手教你使用TensorFlow生成对抗样本

https://mp.weixin.qq.com/s/pIESRzjsmqoO46P4x5Iqhw

Tensorflow卷积神经网络

https://mp.weixin.qq.com/s/Cge_GY19aZ1AcMkhW93C1A

TensorFlow中的那些高级API

https://mp.weixin.qq.com/s/kJxXIN6D5TEEFSFhGJNIyw

开源神经网络图片上色技术解析

https://mp.weixin.qq.com/s/qXMRHxDDRa-_rJZMhXWB4w

详解TensorFlow的新seq2seq模块及其用法

https://mp.weixin.qq.com/s/YdcIDXadEnDsyfc6Iu1gGw

手把手教你用TensorFlow训练模型

https://mp.weixin.qq.com/s/Off0pgaRNyik2nvjHaQQkw

在TensorFlow中对比两大生成模型：VAE与GAN

https://mp.weixin.qq.com/s/rMYjsIgFNvv47F4YZjY8SA

如何在K8S上玩转TensorFlow？

https://mp.weixin.qq.com/s/xJaoqHLVFmouaspjaLjoSg

TFX：基于TensorFlow可大规模扩展的机器学习平台

https://mp.weixin.qq.com/s/Yp2zE85VCx8q67YXvuw5qw

TensorFlow引入了动态图机制Eager Execution

https://zhuanlan.zhihu.com/p/30751039

TensorFlow全新的数据读取方式：Dataset API入门教程

https://github.com/ZhuanZhiCode/TensorFlow-Eager-Execution-Examples

Eager Execution的代码示例

https://mp.weixin.qq.com/s/8Hrq_z8s_5ms6Q_6OOaU-g

如何使用TensorFlow和自编码器模型生成手写数字

https://mp.weixin.qq.com/s/vbBNYzKq6AnsDTy8lFsKAw

TensorFlow RNN深度学习BiLSTM+CRF实现sequence labeling序列标注

https://mp.weixin.qq.com/s/knw7yuUxHe-qeCLfj20onw

Bayesian GAN的TensorFlow实现

https://mp.weixin.qq.com/s/Sxui9CvdGocIxVG2FM4JtQ

基于tensorflow使用CNN-RNN进行中文文本分类！

https://mp.weixin.qq.com/s/X7ISLRMy5cddpmO5mev4MA

自创数据集，使用TensorFlow预测股票入门

https://mp.weixin.qq.com/s/65HiEwCyzeA_d9flPBcpLQ

谷歌正式发布TensorFlowLite，半监督跨平台快速训练ML模型

http://www.jianshu.com/p/1da012a83b74

利用TensorFlow实现排序和搜索算法

https://mp.weixin.qq.com/s/oEqMjOTj8xpd3sg60ZUhqA

TensorFlow的c++实践及各种坑
