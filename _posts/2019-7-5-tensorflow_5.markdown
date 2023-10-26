---
layout: post
title:  TensorFlow（五）
category: DL Framework 
---

* toc
{:toc}

# op Backprop

## compute_gradients & apply_gradients

由源代码可以知道`optimizer.minimize`实际上包含了两个步骤，即`compute_gradients`和`apply_gradients`，前者用于计算梯度，后者用于使用计算得到的梯度来更新对应的variable。

如果想要部分更新某个Variable的话，可用如下步骤：

1.生成需要更新的元素的mask tensor。1代表要更新，0代表不更新。

2.`compute_gradients`得到grad tensor。

3.`grad = grad * mask`

4.`apply_gradients`。

通常来说，如果一个计算图中没有optimizer，则一般只包含forward运算，而没有backward运算。

## Add

```cpp
//forward
REGISTER3(BinaryOp, GPU, "AddV2", functor::add, float, Eigen::half, double);
tensorflow/core/kernels/cwise_ops_common.h: BinaryOp

//backward
tensorflow/python/ops/math_grad.py:
@ops.RegisterGradient("AddV2")
def _AddGrad(op, grad):
tensorflow/core/ops/math_grad.cc:
REGISTER_OP_GRADIENT("AddV2", AddGrad);

//RegisterGradient
tensorflow/python/framework/ops.py:
class RegisterGradient(object):
```

Gradient有两种处理方式：（tensorflow/python/ops/gradients_util.py: _GradientsHelper）

- 有RegisterGradient的op，直接调用注册的函数。

- 没有的，调用SymbolicGradient。

参考：

https://www.zhihu.com/question/56443480

TensorFlow的自动求导具体是在哪部分代码里实现的？

## Conv

```cpp
tensorflow/cc/gradients/nn_grad.cc:
REGISTER_GRADIENT_OP("Conv2D", Conv2DGrad);

tensorflow/python/ops/nn_grad.py:
@ops.RegisterGradient("Conv2DBackpropInput")
def _Conv2DBackpropInputGrad(op, grad):

@ops.RegisterGradient("Conv2DBackpropFilter")
def _Conv2DBackpropFilterGrad(op, grad):
```

Conv2D的Backprop操作可分为两部分：

- Conv2DBackpropInput负责计算上一层的梯度，也就是所谓的in_grad。

- Conv2DBackpropFilter负责计算Kernel的梯度。（似乎没有计算bias梯度）

```cpp
// BP input
// tensorflow source code:
tensorflow/core/kernels/conv_grad_input_ops.cc: LaunchConv2DBackpropInputOp
tensorflow/core/kernels/conv_grad_input_ops.h: LaunchConv2DBackpropInputOpImpl
tensorflow/core/kernels/eigen_backward_spatial_convolutions.h: Eigen::SpatialConvolutionBackwardInput
// eigen source code:
unsupported/Eigen/CXX11/src/Tensor/TensorBase.h: TensorBase::contract()
unsupported/Eigen/CXX11/src/Tensor/TensorContraction.h: evalGemmPartial
unsupported/Eigen/CXX11/src/Tensor/TensorContraction.h: TensorContractionKernel
Eigen/src/Core/products/GeneralBlockPanelKernel.h: gebp_kernel::operator()

// BP filter
// tensorflow source code:
tensorflow/core/kernels/conv_grad_filter_ops.cc: LaunchConv2DBackpropFilterOp
tensorflow/core/kernels/eigen_backward_spatial_convolutions.h: Eigen::SpatialConvolutionBackwardKernel
// eigen source code:
unsupported/Eigen/CXX11/src/Tensor/TensorBase.h: TensorBase::contract()
```

以上是CPU计算BP的调用路径，要点如下：

- 无论是计算BP input，还是BP filter，最终都会转换成GEMM运算。

- GEMM运算会调用TensorContractionKernel。

Tensor contraction是一种Tensor运算，参见《线性代数（一）》中的“张量分析”一节。

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

# TFRecord

TFRecord是TensorFlow官方定义的存放样本数据文件。

参考：

http://www.cnblogs.com/antflow/p/7299029.html

TFRecord的使用

https://zhuanlan.zhihu.com/p/27481108

TensorFlow直接读取图片和读写TFRecords速度对比

# TensorSensor

https://mp.weixin.qq.com/s/ZxmoBcWJa7luGOHQ32ru1A

推荐一个快速定位深度学习代码bug的炼丹神器

# TensorNetwork

TensorFlow的计算图模型不仅可以用于DL领域，亦可应用于其他科学计算领域。TensorNetwork就是一个基于TensorFlow的张量运算库。现成的矩阵运算库已经很多了，这次升级为张量运算库了。

https://github.com/google/TensorNetwork

参考：

https://mp.weixin.qq.com/s/jdjX0jirTHOUqsGagJmGLQ

谷歌AI开源张量计算库TensorNetwork，计算速度暴涨100倍

# 混合精度训练

https://tensorflow.google.cn/guide/mixed_precision

tensorflow/python/keras/mixed_precision/loss_scale_optimizer.py

# 细节

执行`session.run(out)`，会在终端打印out的值，但执行`res = session.run(out)`则不会。

此外，`session.run`可以接受list作为参数。返回值也是一个list，分别对应输入list的每个元素的计算结果。

---

tensorflow的程序中,在main函数下,都是使用tf.app.run()来启动。查看源码可知,该函数是用来处理flag解析，然后执行main函数。

https://blog.csdn.net/lujiandong1/article/details/53262612

tensorflow中的tf.app.run()

---

TF提供了一套专门的IO函数：tf.gfile。主要优点在于：对于写文件来说，open操作直到真的需要写的时候才执行。

---

迁移学习的时候，有的时候需要保持某几层的权值，在后续训练中不被改变。这时，可以在创建Variable时，令trainable=false。

---

sparse_softmax_cross_entropy_with_logits和softmax_cross_entropy_with_logits的区别在于：后者的label是一个one hot的tensor，而前者label直接用对应分类的index表示就行了。

---

CNN中的padding：

"SAME" = with zero padding。

"VALID" = without padding。

---

op的自定义实现可使用`tf.py_func`。

---

tf.dtypes.cast: 类型转换

---

Keras对大部分权重矩阵都采用了标准的Glorot uniform初始化，对GRU的recurrent weight采用了正交初始化，对所有偏置都采用了零初始化；而PyTorch对所有参数都一律采用了uniform初始化，但范围与Glorot不同。

https://www.zhihu.com/question/268494717

同一个模型用theano，tf，pytorch实现，performance可能差距较大吗？

---

`CUDA_VISIBLE_DEVICES`用于指定使用的显卡，因此`CUDA_VISIBLE_DEVICES=0`表示使用0号显卡。如果打算使用CPU的话，需要`CUDA_VISIBLE_DEVICES=`。

# TensorFlow参考

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

https://mp.weixin.qq.com/s/tVqp1Tht1P-0EQazJizQaA

利用人口普查收入数据集预测收入

https://mp.weixin.qq.com/s/3WleFm9S_wMIPTz_WfjKQw

TensorFlow支持Unicode，中文NLP终于省心了

https://mp.weixin.qq.com/s/jFEOokxfJ1Kw-P3wvw3EAg

带你了解，不规则张量！

https://mp.weixin.qq.com/s/tMtx4PZbpo5IrnhzLz8Lzw

AutoGraph：图的简易控制流程

https://mp.weixin.qq.com/s/zY7rGh-kA-36VEo9DiaKbg

TensorFlow进行简单的图像处理

https://mp.weixin.qq.com/s/DAV3TDI4JYr0sXqTGU6t2A

分布式TensorFlow入坑指南：从实例到代码带你玩转多机器深度学习

https://mp.weixin.qq.com/s/QU5NjksCEswjHnkY7WXWXQ

分布式TensorFlow入门教程

https://mp.weixin.qq.com/s/pBR4wMITrigbSVAvn0d6vQ

利用TensorFlow实现上下文的Chat-bots

https://mp.weixin.qq.com/s/8I5Nvw4t2jT1NR9vIYT5XA

深入理解TensorFlow中的tf.metrics算子

https://mp.weixin.qq.com/s/aMarI-nyIvFqhtpJWQrNhQ

谷歌推强化学习新框架“多巴胺“，基于TensorFlow，已开源

https://mp.weixin.qq.com/s/ntHkMIef1o2-FF-AJf_bZQ

三分钟训练眼球追踪术，AI就知道你在盯着哪个妹子——TensorFlow.js代码

https://mp.weixin.qq.com/s/7rTmEBfh613SrNnTQvfSjw

懒人福利：不写代码调优深度模型，谷歌开源的“What-If”了解一下
