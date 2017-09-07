---
layout: post
title:  深度学习（十一）——SSD, 花式卷积
category: theory 
---

# SSD

SSD是Wei Liu于2016年提出的算法。

论文：

《SSD: Single Shot MultiBox Detector》

代码：

https://github.com/weiliu89/caffe

>Wei Liu，南京大学本科（2009）+北卡罗莱娜大学博士（在读）。   
>个人主页：   
>http://www.cs.unc.edu/~wliu/

参考：

http://www.jianshu.com/p/ebebfcd274e6

Caffe-SSD训练自己的数据集教程

https://zhuanlan.zhihu.com/p/24954433

SSD

http://blog.csdn.net/zy1034092330/article/details/72862030

SSD详解

http://blog.csdn.net/jesse_mx/article/details/74011886

SSD: Single Shot MultiBox Detector模型fine-tune和网络架构

## ENet

https://github.com/TimoSaemann/ENet

《ENet: A Deep Neural Network Architecture for Real-Time Semantic Segmentation》

http://blog.csdn.net/zijinxuxu/article/details/67638290

# 花式卷积

在DL中，卷积实际上是一大类计算的总称。除了原始的卷积、反卷积之外，还有各种各样的花式卷积。

参考：

https://github.com/vdumoulin/conv_arithmetic

Convolution arithmetic

http://deeplearning.net/software/theano_versions/dev/tutorial/conv_arithmetic.html

Convolution arithmetic

https://mp.weixin.qq.com/s/dR2nhGqpz7OdmxKPYSaaxw

如何理解空洞卷积（dilated convolution）？

https://mp.weixin.qq.com/s/CLFbhWMcat4rN8YS_7q25g

这12张图生动的告诉你，深度学习中的卷积网络是怎么一回事？

http://mp.weixin.qq.com/s/dvuX3Ih_DZrv0kgqFn8-lg

卷积神经网络结构变化——可变形卷积网络deformable convolutional networks

http://cs.nyu.edu/~fergus/drafts/utexas2.pdf

Deconvolutional Networks

https://zhuanlan.zhihu.com/p/22245268

CNN-反卷积

http://buptldy.github.io/2016/10/29/2016-10-29-deconv/

Transposed Convolution, Fractionally Strided Convolution or Deconvolution（中文blog）

https://buptldy.github.io/2016/10/01/2016-10-01-im2col/

Implementing convolution as a matrix multiplication（中文blog）

# 花式池化

![](/images/article/max_pooling.png)

参考：

http://www.cnblogs.com/tornadomeet/p/3432093.html

Stochastic Pooling简单理解

