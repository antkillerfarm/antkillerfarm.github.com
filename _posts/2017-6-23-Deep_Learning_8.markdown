---
layout: post
title:  深度学习（八）——ResNet
category: DL 
---

* toc
{:toc}

# 神经元激活函数进阶

## ReLU的缺点（续）

ReLU还经常被“诟病”的另一个问题是输出具有**偏移现象**，即输出均值恒大于零。偏移现象和神经元死亡会共同影响网络的收敛性。实验表明，如果不采用Batch Normalization，即使用MSRA初始化30层以上的ReLU网络，最终也难以收敛。

为了解决上述问题，人们提出了Leaky ReLU、PReLU、RReLU、ELU、Maxout等ReLU的变种。

Leaky ReLU:

$$f(x)  = \begin{cases}
    x & \mbox{if } x > 0 \\
    a x & \mbox{otherwise}
\end{cases}$$

这里的a是个常数，如果是个vector的话，那么就是PReLU了。

ELU：

$$f(x) = \begin{cases} 
x & \mbox{if } x \geq 0 \\ 
a(e^x-1) & \mbox{otherwise}
\end{cases}$$

## Maxout

Maxout Networks是Ian J. Goodfellow于2013年提出的一大类激活函数。

![](/images/article/maxout.png)

上图是Maxout Networks的结构图。传统的激活函数一般是这样的形式：$$\sigma(Wx+b)$$

Maxout Networks将$$Wx+b$$这部分运算，分成k个组。每组的w和b都不相同。然后对每组计算结果$$z_{ij}$$取最大值。

从这个意义来说，ReLU可以看做是Maxout的特殊情况，即：

$$y=\max(W_1x+b_1,W_2x+b_2)=\max(0,Wx+b)$$

更多的情况参见下图：

![](/images/article/maxout_2.png)

从Maxout Networks的角度来看，ReLU和DropOut实际上是非常类似的。

参考：

http://blog.csdn.net/hjimce/article/details/50414467

Maxout网络学习

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

![](/images/img4/GELU.png)

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

$$soft\_ relu(x) = \ln(1 + \exp(\max(\min(x, threshold), -threshold)))$$

### SELU

![](/images/img4/SELU.png)

$$SELU(x) = scale * (\max(0,x) + \min(0, \alpha * (\exp(x) - 1)))$$

$$\alpha = 1.6732632423543772848170429916717$$

$$scale = 1.0507009873554804934193349852946$$

### CELU

![](/images/img4/CELU.png)

$$CELU(x) = \max(0,x) + \min(0, \alpha * (\exp(x/\alpha) - 1))$$

### CRELU

$$CReLU(x)= Concat[ ReLU(x), ReLU(−x) ]$$

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

https://blog.csdn.net/shuzfan/article/details/77807550

CReLU激活函数

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
