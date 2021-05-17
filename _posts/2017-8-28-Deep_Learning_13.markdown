---
layout: post
title:  深度学习（十三）——花式池化, Regularization, seq2seq
category: DL 
---

* toc
{:toc}

# 姿态/行为检测

## DensePose（续）

https://mp.weixin.qq.com/s/gwRD3SzTof349V8W0_lRfg

实时评估世界杯球员的正确姿势：FAIR开源DensePose

https://zhuanlan.zhihu.com/p/39219404

Dense Pose

https://blog.csdn.net/sinat_26917383/article/details/79704097

关键点定位：四款人体姿势关键点估计论文笔记

https://mp.weixin.qq.com/s/-A87-z5inWBsF1-5UYagTA

Facebook实时人体姿态估计：Dense Pose及其应用展望

## Hourglass networks

Hourglass networks是University of Michigan的Alejandro Newell的作品。（2016年3月）

论文：

《Stacked hourglass networks for human pose estimation》

![](/images/img3/Hourglass_Networks.png)

上图是Stacked Hourglass networks的网络结构图，其中的每个沙漏形状的结构，都是一个hourglass module，其结构如下图所示：

![](/images/img3/Hourglass_Networks_2.png)

hourglass module基本可以看作是把concat换成add之后的U-NET，或者也可以看作是resnet版的U-NET。上图中一个module包含了4次add，因此也被叫做4阶hourglass module。

参考：

https://blog.csdn.net/shenxiaolu1984/article/details/51428392

Stacked Hourglass算法详解

https://mp.weixin.qq.com/s/nfPBRBLG1ThsY3DvONHYrA

CenterNet骨干网络之hourglass

https://mp.weixin.qq.com/s/lzxd9J97nkOBLXgEcbdoKA

使用Hourglass网络来理解人体姿态

## 评价度量

Object Keypoint Similarity(OKS)：

$$\mathbf{OKS} = \frac{\sum_i exp(-\frac{d_i^2}{2s^2k_i^2}) \delta (v_i >0)}{\sum_i \delta (v_i >0)}$$

其中，$$d_i$$是检测的关键点与groundtruth关键点之间的欧氏距离；$$v_i$$是groundtruth关键点的可见性标志；s是目标的尺度；$$k_i$$是控制衰减(falloff)的per-keypoint常数。

## 步态识别

https://mp.weixin.qq.com/s/g6032xTGEtvbsfwXboMJ4A

大阪大学副校长Yasushi Yagi：步态分析

http://mp.weixin.qq.com/s/Y-PvMz_Vz8nBGRZo9dwUCA

中科院步态识别技术：不看脸50米内在人群中认出你！

https://mp.weixin.qq.com/s/3Pe5wJ0VomzwKMF84OqcMg

步态识别的深度学习综述

https://mp.weixin.qq.com/s/afX8Y84nTS20q4Y36uOWqQ

复旦提出GaitSet算法，步态识别的重大突破！

# 花式池化

池化和卷积一样，都是信号采样的一种方式。

## 普通池化

池化的一般步骤是：选择区域P，令$$Y=f(P)$$。这里的f为池化函数。

![](/images/article/max_pooling.png)

上图是Max Pool的示意图，也就是选择池子里最大的那个值。

除了max之外，常用的池化函数还有：

Min Pool:

$$Y=\min(P)$$

Average Pool:

$$Y=\text{mean}(P)$$

L2 Pool:

$$Y=\sqrt{\frac{\sum p^2}{n}}$$

ICLR2013上，Zeiler提出了另一种pooling手段stochastic pooling。只需对Pooling区域中的元素按照其概率值大小随机选择，即元素值大的被选中的概率也大。而不像max-pooling那样，永远只取那个最大值元素。

根据相关理论，特征提取的误差主要来自两个方面：

（1）邻域大小受限造成的估计值方差增大；

（2）卷积层参数误差造成估计均值的偏移。

一般来说，mean-pooling能减小第一种误差，更多的保留图像的背景信息，max-pooling能减小第二种误差，更多的保留纹理信息。

Stochastic-pooling则介于两者之间，通过对像素点按照数值大小赋予概率，再按照概率进行亚采样，在平均意义上，与mean-pooling近似，在局部意义上，则服从max-pooling的准则。

## 池化的反向传播

池化的反向传播比较简单。以上图的Max Pooling为例，由于取的是最大值7,因此，误差只要传递给7所在的神经元即可。

这里再次强调一下，池化只是对信号的下采样。对于图像来说，这种下采样保留了图像的某些特征，因而是有意义的。但对于另外的任务则未必如此。

比如，AlphaGo采用CNN识别棋局，但对棋局来说，下采样显然是没有什么物理意义的，因此，**AlphaGo的CNN是没有Pooling的**。

## 全局平均池化

Global Average Pool是另一类池化操作，一般用于替换FullConnection层。

![](/images/article/global_average_pooling.png)

上图是FC和GAP在CNN中的使用方法图。从中可以看出Conv转换成FC，实际上进行了如下操作：

1.对每个通道的feature map进行flatten操作得到一维的tensor。

2.将不同通道的tensor连接成一个大的一维tensor。

![](/images/article/FC.png)

上图展示了FC与Conv、Softmax等层联动时的运算操作。

![](/images/article/GAP.png)

上图是GAP与Conv、Softmax等层联动时的运算操作。可以看出，GAP的实际操作如下：

1.计算每个通道的feature map的均值。

2.将不同通道的均值连接成一个一维tensor。

GAP实际上就是kernel size等于WxH的AP。类似的，还有Global Max Pool。

## UnPooling

UnPooling是一种常见的上采样操作。其过程如下图所示：

![](/images/article/unpool.png)

1.在Pooling（一般是Max Pooling）时，保存最大值的位置（Max Location）。

2.中间经历若干网络层的运算。

3.上采样阶段，利用第1步保存的Max Location，重建下一层的feature map。

从上面的描述可以看出，UnPooling不完全是Pooling的逆运算：

1.Pooling之后的feature map，要经过若干运算，才会进行UnPooling操作。

2.对于非Max Location的地方以零填充。然而这样并不能完全还原信息。

参考：

http://blog.csdn.net/u012938704/article/details/52831532

caffe反卷积

## K-max Pooling

![](/images/article/kmax_pooling.png)

## 参考

http://www.cnblogs.com/tornadomeet/p/3432093.html

Stochastic Pooling简单理解

http://mp.weixin.qq.com/s/XzOri12hwyOCdI1TgGQV3w

新型池化层sort_pool2d实现更快更好的收敛：表现优于最大池化层

http://blog.csdn.net/liuchonge/article/details/67638232

CNN与句子分类之动态池化方法DCNN--模型介绍篇

https://mp.weixin.qq.com/s/K1RBux3AfxVFT8_uezYHFA

被Hinton，DeepMind和斯坦福嫌弃的池化，到底是什么？

https://mp.weixin.qq.com/s/J4opJ6NvbTxbHWAWNHEltw

自然语言处理中CNN模型几种常见的Max Pooling操作

https://mp.weixin.qq.com/s/KGFsMl3X52_T50h7Bhk65w

CNN一定需要池化层吗？

https://zhuanlan.zhihu.com/p/341820742

深度神经网络中的池化方法：全面调研（1989-2020）

https://mp.weixin.qq.com/s/1Np5KFDR1Wnbwl5Akod13g

SoftPool：基于Softmax加权的池化操作

# Regularization

DL中的Regularization除了常见的$$l_1$$-norm、$$l_2$$-norm和squared $$l_2$$-norm之外，还有Group Regularization。它的定义如下：

$$loss(W;x;y) = loss_D(W;x;y) + \lambda_R R(W) + \lambda_g \sum_{l=1}^{L} R_g(W_l^{(G)})$$

$$R_g(w^{(g)}) = \sum_{g=1}^{G} \lVert w^{(g)} \rVert_g = \sum_{g=1}^{G} \sum_{i=1}^{|w^{(g)}|} {(w_i^{(g)})}^2$$

Group Regularization也叫做Block Regularization或Structured Regularization。

# seq2seq

seq2seq最早用于Neural Machine Translation领域（与之相对应的有Statistical Machine Translation）。训练后的seq2seq模型，可以根据输入语句，自动生成翻译后的输出语句。

![](/images/article/seq2seq.png)

上图是seq2seq的结构图。可以看出seq2seq实际上是一种Encoder-Decoder结构。

在Encoder阶段，RNN依次读入输入序列。但由于这时，没有输出序列与之对应，因此这仅仅相当于一个对隐层的编码过程，即将句子的语义编码为隐层的状态向量。

从中发现一个问题：状态向量的维数决定了存储的语义的内容上限（显然不能指望，一个200维的向量，能够表示一部百科全书。）因此，seq2seq通常只用于短文本的翻译。

在Decoder阶段，我们根据输出序列，反向修正RNN的参数，以达到训练神经网络的目的。

## Beam Search Decoder

https://guillaumegenthial.github.io/sequence-to-sequence.html

Seq2Seq with Attention and Beam Search

https://blog.csdn.net/mr_tyting/article/details/78604721

Seq2Seq Learning(Encoder-Decoder,Beam Search,Attention)

## 参考

https://github.com/ematvey/tensorflow-seq2seq-tutorials

一步步的seq2seq教程

http://blog.csdn.net/sunlylorn/article/details/50607376

seq2seq模型

http://datartisan.com/article/detail/120.html

Seq2Seq的DIY简介

https://mp.weixin.qq.com/s/U5yqXBHFD9LgIQJrqOlXFw

机器翻译不可不知的Seq2Seq模型

http://www.cnblogs.com/Determined22/p/6650373.html

DL4NLP——seq2seq+attention机制的应用：文档自动摘要（Automatic Text Summarization）

https://mp.weixin.qq.com/s/m-Z0UBgmFQ4CE0yLKYoHZw

seq2seq和attention如何应用到文档自动摘要

http://blog.csdn.net/young_gy/article/details/73412285

基于RNN的语言模型与机器翻译NMT

http://karpathy.github.io/2015/05/21/rnn-effectiveness/

The Unreasonable Effectiveness of Recurrent Neural Networks

https://mp.weixin.qq.com/s/8u3v9XzECkwcNn5Ay-kYQQ

基于Depthwise Separable Convolutions的Seq2Seq模型_SliceNet原理解析

https://mp.weixin.qq.com/s/H6eYxS7rXGDH_B8Znrxqsg

seq2seq中的beam search算法过程

https://mp.weixin.qq.com/s/U1yHIc5Zq0yKCezRm185VA

Attentive Sequence to Sequence Networks

https://mp.weixin.qq.com/s/cGXANj7BB2ktTdPAL4ZEWA

图解神经网络机器翻译原理：LSTM、seq2seq到Zero-Shot

https://mp.weixin.qq.com/s/jYUAKyTpm69J6Q34A06E-w

百度提出冷聚变方法：使用语言模型训练Seq2Seq模型

https://mp.weixin.qq.com/s/Fp6G1aI_utDd_kTbdHvEVQ

完全基于卷积神经网络的seq2seq

http://localhost:4500/theory/2017/06/21/Deep_Learning_6.html

从2017年顶会论文看Attention Model

https://mp.weixin.qq.com/s/Op_oYiNvaTXvsvAnl8Heew

基于Self-attention的文本向量表示方法，悉尼科技大学和华盛顿大学最新工作

https://mp.weixin.qq.com/s/fBrt4g_Kjmt1tGVZw5KgrQ

从LSTM到Seq2Seq

https://mp.weixin.qq.com/s/riIC6ybvqAJx9mzb-AQIOw

Facebook AI发布新版本FairSeq序列到序列(Seq2Seq)学习工具，可生成故事与快速推断

https://mp.weixin.qq.com/s/DIqjVxF_kACkivzez4_Hog

编码器-解码器网络：神经翻译模型详解

https://mp.weixin.qq.com/s/Alg4rOXNvb4GA8N4Joy-Jg

Seq2seq强化，Pointer Network简介

https://mp.weixin.qq.com/s/kdmmgVdWxz2nJPmjcprvqg

机器学习中的编码器-解码器结构哲学

https://mp.weixin.qq.com/s/OcrT2-sAWJg-ILdHwi4t5Q

seq2seq最新变体，稀疏序列模型

https://mp.weixin.qq.com/s/_1lr612F3x8ld9gvXj9L2A

推断速度达seq2seq模型的100倍，谷歌开源文本生成新方法LaserTagger
