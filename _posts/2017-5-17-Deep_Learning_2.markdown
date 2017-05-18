---
layout: post
title:  深度学习（二）——深度学习常用术语解释, CNN
category: theory 
---

# 深度学习常用术语解释（续）

## Vanilla

Vanilla是神经网络领域的常见词汇，比如Vanilla Neural Networks、Vanilla CNN等。Vanilla本意是香草，在这里基本等同于raw。比如Vanilla Neural Networks实际上就是BP神经网络，而Vanilla CNN实际上就是最原始的CNN。

## weight decay

weight decay（权值衰减）的使用既不是为了提高你所说的收敛精确度也不是为了提高收敛速度，其最终目的是防止过拟合。在损失函数中，weight decay是放在正则项（regularization）前面的一个系数，正则项一般指示模型的复杂度，所以weight decay的作用是调节模型复杂度对损失函数的影响，若weight decay很大，则复杂的模型损失函数的值也就大。

https://mp.weixin.qq.com/s/W4d2fkiJig--PuDPM11ozA

## momentum

momentum是梯度下降法中一种常用的加速技术。如果上一次的momentum与这一次的负梯度方向是相同的，那这次下降的幅度就会加大，所以这样做能够达到加速收敛的过程。

## Batch Normalization

1.提高梯度在网络中的流动。Normalization能够使特征全部缩放到[0,1]，这样在反向传播时候的梯度都是在1左右，避免了梯度消失现象。

2.提升学习速率。归一化后的数据能够快速的达到收敛。

3.减少模型训练对初始化的依赖。

http://www.cnblogs.com/neopenx/p/5211969.html

从Bayesian角度浅析Batch Normalization

https://www.zhihu.com/question/38102762

深度学习中 Batch Normalization为什么效果好？

# CNN

## 概述

卷积神经网络（Convolutional Neural Networks，ConvNets或CNNs）属于神经网络的范畴，已经在诸如图像识别和分类的领域证明了其高效的能力。

CNN的开山之作是Yann LeCun的论文：

《Gradient-Based Learning Applied to Document Recognition》

>注：科学界的许多重要成果的开山之作，其名称往往和成果的最终名称有一定的差距。比如LeCun的这篇文章的名称中，就没有CNN。类似的还有Vapnik的SVM，最早被称为Support Vector Network。

英文不好的，推荐以下文章：

http://www.hackcv.com/index.php/archives/104/

CNN的直观解释

## 关键点

这里以最经典的LeNet-5为例，提点一下CNN的要点。

![](/images/article/LeNet_5.jpg)

### 卷积



实际上，传统MLP（MultiLayer Perceptron）网络就是犯了这个错误，1D全连接的神经元控制了太多参数，不利于学习到稀疏特征。

CNN网络，2D全连接的神经元则控制了局部感受野，有利于解离出稀疏特征。

## 参考

http://lib.csdn.net/article/deeplearning/58185

BP神经网络与卷积神经网络

http://blog.csdn.net/Fate_fjh/article/details/52882134

卷积神经网络系列blog

http://mp.weixin.qq.com/s/ZKMi4gRfDRcTxzKlTQb-Mw

计算机视觉识别简史：从AlexNet、ResNet到Mask RCNN

http://mp.weixin.qq.com/s/YRwGwelyA3VOYZ4XGAjUBw

CNN 感受野首次可视化：深入解读及计算指南

http://mp.weixin.qq.com/s/dvuX3Ih_DZrv0kgqFn8-lg

卷积神经网络结构变化——可变形卷积网络deformable convolutional networks

http://mp.weixin.qq.com/s/kbHzA3h-CfTRcnkViY37MQ

详解CNN五大经典模型:Lenet，Alexnet，Googlenet，VGG，DRL

https://zhuanlan.zhihu.com/p/22094600

Deep Learning回顾之LeNet、AlexNet、GoogLeNet、VGG、ResNet

http://www.leiphone.com/news/201609/303vE8MIwFC7E3DB.html

Google最新开源Inception-ResNet-v2，借助残差网络进一步提升图像分类水准

http://mp.weixin.qq.com/s/2TUw_2d36uFAiJTkvaaqpA

解读Keras在ImageNet中的应用：详解5种主要的图像识别模型

![](/images/article/reinforcement_learning.png)

![](/images/article/CNN_1.jpg)

![](/images/article/CNN_2.jpg)

# Autoencoders

Bengio在2003年的A neural probabilistic language model中指出，维度过高，导致每次学习，都会强制改变大部分参数。

由此发生蝴蝶效应，本来很好的参数，可能就因为一个小小传播误差，就改的乱七八糟。

实际上，传统MLP网络就是犯了这个错误，1D全连接的神经元控制了太多参数，不利于学习到稀疏特征。

CNN网络，2D全连接的神经元则控制了局部感受野，有利于解离出稀疏特征。

http://ufldl.stanford.edu/tutorial/unsupervised/Autoencoders/

![](/images/article/Autoencoder.png)

# Neural Network Zoo

![](/images/article/Neural_Networks.png)

上图的原地址为：

http://www.asimovinstitute.org/neural-network-zoo/

