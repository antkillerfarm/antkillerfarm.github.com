---
layout: post
title:  深度学习（八）——ResNet, Bi-directional RNN
category: DL 
---

* toc
{:toc}

# 神经元激活函数进阶（续）

## GLU

Gated Linear Unit是由facebook提出的：

$$(\boldsymbol{W}_1\boldsymbol{x}+\boldsymbol{b}_1)\otimes \sigma(\boldsymbol{W}_2\boldsymbol{x}+\boldsymbol{b}_2)$$

![](/images/img2/GLU.png)

上图右侧是一个Linear Unit，左侧的$$\sigma$$相当于一个Gate，故名。

论文：

《Language Modeling with Gated Convolutional Networks》

GLU一般用在NLP领域，它和CNN结合，也就是所谓的GCNN了。

## RRelu

论文：

《Empirical Evaluation of Rectified Activations in Convolution Network》

![](/images/img3/RRelu.png)

## GELU

论文：

《Gaussian Error Linear Units (GELUs)》

$$\text{GELU}(x)=x \Phi(x)$$

其中$$\Phi(x)$$是标准正态分布的累积概率函数，即：

$$\Phi(x)=\int_{-\infty}^x \frac{e^{-t^2/2}}{\sqrt{2\pi}}dt=\frac{1}{2}\left[1 + \text{erf}\left(\frac{x}{\sqrt{2}}\right)\right]$$

上式中$$\text{erf}$$函数，也称为正态分布的误差函数。

$$erf(x)=\frac{2}{\sqrt{ \pi}}\int_0^{x}e^{-t^2}dt$$

这个积分无法用初等函数表示，所以通常使用以下的近似泰勒展开式：

$$x\Phi(x)\approx x\sigma(1.702 x)$$

$$x\Phi(x)\approx \frac{1}{2} x \left[1 + \tanh\left(\sqrt{\frac{2}{\pi}}\left(x + 0.044715 x^3\right)\right)\right]$$

参考：

https://mp.weixin.qq.com/s/pA9JW75p9J5e5KHe3ifcBQ

从ReLU到GELU，一文概览神经网络的激活函数

https://mp.weixin.qq.com/s/LEPalstOc15CX6fuqMRJ8Q

超越ReLU却鲜为人知，3年后被挖掘：BERT、GPT-2等都在用的激活函数（GELU）

https://kexue.fm/archives/7309

GELU的两个初等函数近似是怎么来的

https://zhuanlan.zhihu.com/p/349492378

BERT中的激活函数GELU：高斯误差线性单元

https://www.cnblogs.com/htj10/p/8621771.html

正态分布（高斯分布）、Q函数、误差函数、互补误差函数

## Swish

Swish是Google大脑团队提出的一个新的激活函数：

$$\text{swish}(x)=x\cdot\sigma(x)=\frac{x}{1+e^{-x}}$$

它的图像如下图中的橙色曲线所示：

![](/images/article/swish.png)

Swish可以看作是GLU的特例（Swish的两组参数相同）。

Swish在原点附近不是饱和的，只有负半轴远离原点区域才是饱和的，而ReLu在原点附近也有一半的空间是饱和的。

而我们在训练模型时，一般采用的初始化参数是均匀初始化或者正态分布初始化，不管是哪种初始化，其均值一般都是0，也就是说，初始化的参数有一半处于ReLu的饱和区域，这使得刚开始时就有一半的参数没有利用上。

特别是由于诸如BN之类的策略，输出都自动近似满足均值为0的正态分布，因此这些情况都有一半的参数位于ReLu的饱和区。

相比之下，Swish好一点，因为它在负半轴也有一定的不饱和区，所以参数的利用率更大。

苏剑林据此提出了自己的激活函数：

$$\max(x, x\cdot e^{-\mid x\mid })$$

该函数的图像如上图的蓝色曲线所示。

参考：

https://mp.weixin.qq.com/s/JticD0itOWH7Aq7ye1yzvg

谷歌大脑提出新型激活函数Swish惹争议：可直接替换并优于ReLU？

http://kexue.fm/archives/4647/

浅谈神经网络中激活函数的设计

## 其他激活函数

### hard tanh

$$\text{HardTanh}(x)=\begin{cases}
-1, & x<-1 \\
x, & -1\le x \le 1 \\
1, & x>1 \\
\end{cases}$$

![](/images/article/hard_tanh.png)

hard tanh也叫做Relu1。

### hard sigmoid

$$\text{HardSigmoid}(x)=\begin{cases}
0, & x<-2.5 \\
0.2x, & -2.5\le x \le 2.5 \\
1, & x>2.5 \\
\end{cases}$$

![](/images/img3/hard_sigmoid2.png)

### soft sign

$$\text{softsign}(x)=\frac{x}{1+\mid x\mid }$$

### Hardswish

$$Hardswish(x) = x\frac{RELU6(x+3)}{6} = 
\left\{
\begin{aligned}
&0, & \text{if } x \leq -3 \\
&x, & \text{if } x \geq 3 \\
&\frac{x(x+3)}{6}, & \text{otherwise}
\end{aligned}
\right.
$$

### Mish

$$Mish(x)=x\tanh (\ln(1+e^x))$$

参考：

https://mp.weixin.qq.com/s/i8aShQvJhSgP7KY5Qgm36A

ReLU的继任者Mish：一个新的state of the art的激活函数

https://mp.weixin.qq.com/s/a_roXfjNX2szJMUrww0Fmg

YOLOv4中的Mish激活函数

### soft relu

$$soft_relu(x) = \ln(1 + \exp(\max(\min(x, threshold), -threshold)))$$

## 参考

https://zhuanlan.zhihu.com/p/22142013

深度学习中的激活函数导引

http://blog.csdn.net/u012328159/article/details/69898137

几种常见的激活函数

https://mp.weixin.qq.com/s/Hic01RxwWT_YwnErsJaipQ

什么是激活函数？

https://mp.weixin.qq.com/s/4gElB_8AveWuDVjtLw5JUA

深度学习激活函数大全

https://mp.weixin.qq.com/s/7DgiXCNBS5vb07WIKTFYRQ

从ReLU到Sinc，26种神经网络激活函数可视化

http://www.cnblogs.com/ymjyqsx/p/6294021.html

PReLU与ReLU

http://www.cnblogs.com/pinard/p/6437495.html

深度神经网络（DNN）损失函数和激活函数的选择

https://mp.weixin.qq.com/s/VSRtjIH1tvAVhGAByEH0bg

21种NLP任务激活函数大比拼：你一定猜不到谁赢了

https://www.cnblogs.com/makefile/p/activation-function.html

激活函数(ReLU, Swish, Maxout)

https://mp.weixin.qq.com/s/YVi9ke3VSidBvzfLPjMkZg

激活函数-从人工设计到自动搜索

https://mp.weixin.qq.com/s/XttlCNKGvGZrD7OQZOQGnw

如何发现“将死”的ReLu？

https://mp.weixin.qq.com/s/_qeqicHWnJ50BfiQpLWVVA

Dynamic ReLU：微软推出提点神器，可能是最好的ReLU改进

https://mp.weixin.qq.com/s/jMLarSpq3Wg0OIrRIW26dA

从Binary到Swish——激活函数深度详解

https://mp.weixin.qq.com/s/IuZsTKGesgTw1ATXx4OBEA

深度学习最常用的10个激活函数

# ResNet

无论采用何种方法，可训练的神经网络的层数都不可能无限深。有的时候，即使没有梯度消失，也存在训练退化（即深层网络的效果还不如浅层网络）的问题。

最终2015年，微软亚洲研究院的何恺明等人，使用残差网络ResNet参加了当年的ILSVRC，在图像分类、目标检测等任务中的表现大幅超越前一年的比赛的性能水准，并最终取得冠军。

论文：

《Deep Residual Learning for Image Recognition》

代码：

https://github.com/KaimingHe/deep-residual-networks

残差网络的明显特征是有着相当深的深度，从32层到152层，其深度远远超过了之前提出的深度网络结构，而后又针对小数据设计了1001层的网络结构。

其简化版的结构图如下所示：

![](/images/article/drn.png)

简单的说，就是把前面的层跨几层直接接到后面去，以使误差梯度能够传的更远一些。

DRN的基本思想倒不是什么新东西了，在2003年Bengio提出的词向量模型中，就已经采用了这样的思路。

DRN的实现依赖于下图所示的res block：

![](/images/article/res_block.png)

从中可以看出，所谓残差跨层传递，其实就是将本层ternsor $$\mathcal{F}(x)$$和跨层tensor x加在一起而已。

如果在网络中每个层只有少量的隐藏单元对不同的输入改变它们的激活值，而大部分隐藏单元对不同的输入都是相同的反应，此时整个权重矩阵的秩不高。并且随着网络层数的增加，连乘后使得整个秩变的更低，这就是我们常说的网络退化问题。

虽然权重矩阵是一个很高维的矩阵，但是大部分维度却没有信息，使得网络的表达能力没有看起来那么强大。这样的情况一定程度上来自于网络的对称性，而残差连接打破了网络的对称性。

![](/images/img3/ResNet.png)

随着ResNet的应用越来越广泛，其设计也有一定的微调。上图左边是原始的ResNet结构，而右边是新的结构，据说新结构更有利于梯度的更新。

参考：

https://zhuanlan.zhihu.com/p/22447440

深度残差网络

https://www.leiphone.com/news/201608/vhqwt5eWmUsLBcnv.html

何恺明的深度残差网络PPT

https://mp.weixin.qq.com/s/kcTQVesjUIPNcz2YTxVUBQ

ResNet 6大变体：何恺明,孙剑,颜水成引领计算机视觉这两年

https://mp.weixin.qq.com/s/5M3QiUVoA8QDIZsHjX5hRw

一文弄懂ResNet有多大威力？最近又有了哪些变体？

http://www.jianshu.com/p/b724411571ab

ResNet到底深不深？

https://mp.weixin.qq.com/s/Kgwwq5XOt88WW6KL8gADmQ

你必须要知道CNN模型：ResNet

https://mp.weixin.qq.com/s/7fWh2dovmfbsF8afaX9UOg

一文简述ResNet及其多种变体

https://mp.weixin.qq.com/s/fxq-H2_ZyXVd_kJx6rwEcQ

何恺明CVPR2018关于视觉深度表示学习教程

https://mp.weixin.qq.com/s/xTJr-jWMjk73TCZ8gBT4Ww

一个神经元统治一切：ResNet强大的理论证明

https://mp.weixin.qq.com/s/-SmmtqHWJjq2A4pu5KqYfQ

resnet中的残差连接，你确定真的看懂了？

https://mp.weixin.qq.com/s/AyJ_ZtNFTjkWVH3_Kw7wJg

ResNet架构可逆！多大等提出性能优越的可逆残差网络

https://mp.weixin.qq.com/s/2JwgiCuBoluBNYesYp4zAA

ResNet及其变种的结构梳理、有效性分析与代码解读

https://mp.weixin.qq.com/s/CFKRzF9WuDrNVSivMf3YNw

目标检测新突破！了解Res2Net深度多尺度目标检测架构

https://mp.weixin.qq.com/s/1R7XWPqiDBNcUjIE-sF08Q

ResNeXt深入解读与模型实现

https://zhuanlan.zhihu.com/p/100122970

基于Keras框架的深度残差收缩网络代码

https://mp.weixin.qq.com/s/scFnuqx0zOtBvFh0JYA0UA

来聊聊ResNet及其变种

https://mp.weixin.qq.com/s/W4IqXMRZJbQ-7fGEF43-sA

真正的最强ResNet改进，高性能“即插即用”金字塔卷积

# Bi-directional RNN

众所周知，RNN在处理长距离依赖关系时会出现问题。LSTM虽然改进了一些，但也只能缓解问题，而不能解决该问题。

研究人员发现将原文倒序（将其倒序输入编码器）产生了显著改善的结果，因为从解码器到编码器对应部分的路径被缩短了。同样，两次输入同一个序列似乎也有助于网络更好地记忆。

基于这样的实验结果，1997年Mike Schuster提出了Bi-directional RNN模型。

>注：Mike Schuster，杜伊斯堡大学硕士（1993）+奈良科技大学博士。语音识别专家，尤其是日语、韩语方面。Google研究员。

论文：

《Bidirectional Recurrent Neural Networks》

下图是Bi-directional RNN的结构示意图：

![](/images/article/Bi_directional_RNN.png)

从图中可以看出，Bi-directional RNN有两个隐层，分别处理前向和后向的时序信息。

除了原始的Bi-directional RNN之外，后来还出现了Deep Bi-directional RNN。

![](/images/article/Deep_Bi_RNN.png)

上图是包含3个隐层的Deep Bi-directional RNN。

参见：

https://mp.weixin.qq.com/s/_CENjzEK1kjsFpvX0H5gpQ

结合堆叠与深度转换的新型神经翻译架构：爱丁堡大学提出BiDeep RNN
