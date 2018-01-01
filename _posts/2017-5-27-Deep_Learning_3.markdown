---
layout: post
title:  深度学习（三）——CNN, Autoencoder
category: DL 
---

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

LeNet-5的caffe模板：

https://github.com/BVLC/caffe/blob/master/examples/mnist/lenet.prototxt

### 卷积

在《数学狂想曲（五）》中我们讨论了卷积的数学含义，结合《 图像处理理论（一）》和《 图像处理理论（二）》，不难看出卷积或者模板（算子），在前DL时代，几乎是图像处理算法的基础和灵魂。为了实现各种目的，人们手工定义或发现了一系列算子。

到了DL时代，卷积仍然起着非常重要的作用。但这个时候，不再需要人工指定算子，算子本身也将由学习获得。我们需要做的只不过是指定算子的个数而已。

比如，LeNet-5的C1:6@28*28，其中的6就是算子的个数。显然算子的个数越多，计算越慢。但太少的话，又会导致提取的特征数太少，神经网络学不到东西。

需要注意的是，传统的CV算法中，通常只有单一的卷积运算。而CNN中的卷积层，实际上包括了**卷积+激活**两种运算，即：

$$L_2=\sigma(Conv(L_1,W)+b)$$

因此，相比全连接层而言，卷积层每次只有部分元素参与到最终的激活运算。从宏观角度看，这些元素实际上对应了图片的局部空间二维信息，它和后面的Pooling操作一道，起到了空间降维的作用。

实际上，传统的MLP（MultiLayer Perceptron）网络，就是由于1D全连接的神经元控制了太多参数，而不利于学习到稀疏特征。

CNN网络中，2D全连接的神经元则控制了局部感受野，有利于解离出稀疏特征。

至于激活函数，则是为了保证变换的非线性。这也是CNN被归类为NN的根本原因。

### 池化

Pooling操作（也称Subsampling）使输入表示（特征维度）变得更小，并且网络中的参数和计算的数量更加可控的减小，因此，可以控制过拟合。

它还可使网络对于输入图像中更小的变化、冗余和变换变得不变性。

### Gaussian Connections

LeNet-5最后一步的Gaussian Connections是一个当年的历史遗迹，目前已经被Softmax所取代。它的含义在上面提到的Yann LeCun的原始论文中有描述。

>注意：现代版的LeNet-5最后一步的Softmax层，实际上包含了$$Wx+b$$和Softmax两种计算。相当于用Softmax函数替换Sigmoid/ReLU函数。

### 其他

![](/images/article/CNN_1.jpg)

上图展示了不同分类的图片特征在特征空间中的分布，可以看出在CNN的低层中，这些特征是混杂在一起的；而到了CNN的高层，这些特征就被区分开来了。

![](/images/article/CNN_2.jpg)

上图是若干ML、DL算法按照不同维度划分的情况。

## 多通道卷积

MNIST的例子中，由于图像是单通道（灰度图）的，因此，多数教程都只是展示了单通道卷积的计算步骤：

$$g(i,j)=\sum_{k,l}f(i+k,j+l)h(k,l)$$

而多通道（例如彩色图像的RGB三通道）卷积实际上就是将各通道卷积之后的结果再加在一起：

$$g(i,j)=\sum_{c,k,l}f(i+k,j+l)h(k,l)$$

![](/images/article/conv_1.png)

上图展示了一个4通道图像经卷积之后，得到2通道图像的过程。

从中可以得出以下结论：

1.RGB通道信息在卷积之后，就不复存在了。无论输入图像有多少个通道，输出图像的通道数只和feature的个数相关。

2.即使是LeNet-5的MNIST示例中，实际上也是有多通道卷积的，只不过不在第一个卷积层而已。

3.多通道卷积除了二维空间信息的卷积之外，还包括了**通道间信息**的卷积。这也是CNN中1*1卷积的意义之一。

多通道卷积操作最终可以转化为矩阵运算，如下图所示：

![](/images/article/conv.jpg)

参见：

http://blog.csdn.net/u014114990/article/details/51125776

多通道(比如RGB三通道)卷积过程

https://www.zhihu.com/question/28385679

在Caffe中如何计算卷积？

## CNN的反向传播算法

由于卷积和池化两层，不是一般的神经网络结构。因此CNN的反向传播算法实际上也是很有技巧的。

相关推导见参考文献，这里只给出卷积层的结论：

正向：$$Y=X*K$$

反向：$$\Delta X = \Delta Y * rot_{180}(K)$$

参见：

http://www.cnblogs.com/pinard/p/6494810.html

卷积神经网络(CNN)反向传播算法

http://blog.csdn.net/zy3381/article/details/44409535

CNN误差反传时旋转卷积核的简明分析

卷积的反向传播，有时也被称为反卷积。

![](/images/article/dcign.png)

上图是Deep convolutional inverse graphics networks的结构图。DCIGN实际上是一个正向CNN连上一个反向CNN，以实现图片合成的目的。其原理可参考《深度学习（三）》中的Autoencoder。

## 参考

http://lib.csdn.net/article/deeplearning/58185

BP神经网络与卷积神经网络

http://blog.csdn.net/Fate_fjh/article/details/52882134

卷积神经网络系列blog

http://mp.weixin.qq.com/s/YRwGwelyA3VOYZ4XGAjUBw

CNN 感受野首次可视化：深入解读及计算指南

https://mp.weixin.qq.com/s/EJyG3Y4EHTGMm_Q1mY4RvA

CNN入门手册（上）

https://mp.weixin.qq.com/s/T3tHFdjnQh4asE0V25vTog

CNN入门手册（中）

https://mp.weixin.qq.com/s/chsDjS39qcoHICUNbSdQHQ

长文揭秘图像处理和卷积神经网络架构

https://mp.weixin.qq.com/s/nIbfiDXkqkpdLzQo2Gmc2Q

利用卷积神经网络处理CIFAR图像分类

https://mp.weixin.qq.com/s/5BMU7SRQeuDg68XDcOUBZw

训练集样本不平衡问题对CNN的影响

https://mp.weixin.qq.com/s/p-wZ_6ZQW-zXzDqmRenNow

深度学习入门：几幅手稿讲解CNN

https://mp.weixin.qq.com/s/xXf7hTfH-vx4YbzlZVQucA

CNN入门再介绍

https://mp.weixin.qq.com/s/Q4snAlAi8tPQAyGm0qUy4w

CNN的全面解析

https://mp.weixin.qq.com/s/Do6erhin3W4dK_-RTAyD6A

卷积神经网络(CNN)概念解释

http://www.qingruanit.net/blog/23930/note5837.html

卷积神经网络（CNN）学习算法之----基于LeNet网络的中文验证码识别

https://mp.weixin.qq.com/s/XiaAPd20YxbM0wDiSTAYMg

深度学习之卷积神经网络(CNN)的模型结构

https://mp.weixin.qq.com/s/x-H6h4sRqTrZlOXKStnhPw

卷积神经网络背后的数学原理

https://mp.weixin.qq.com/s/qIdjHqurqvdahEd0dXYIqA

徒手实现CNN：综述论文详解卷积网络的数学本质

https://mp.weixin.qq.com/s/D6ok6dQqyx6cCJKc2M8YpA

从AlexNet剖析-卷积网络CNN的一般结构

https://mp.weixin.qq.com/s/XZeZX8zTtNom0az_ralp4A

ImageNet冠军带你入门计算机视觉：卷积神经网络

https://mp.weixin.qq.com/s/t8jg_bpEcQiJmgIqKarefQ

卷积神经网络CNN学习笔记

# Autoencoder

Bengio在2003年的《A neural probabilistic language model》中指出，维度过高，会导致每次学习，都会强制改变大部分参数。

由此发生蝴蝶效应，本来很好的参数，可能就因为一个小小传播误差，就改的乱七八糟。

因此，数据降维是数据预处理中，非常重要的一环。常用的降维算法，除了线性的PCA算法之外，还有非线性的Autoencoder。

![](/images/article/Autoencoder.png)

Autoencoder的结构如上图所示。它的特殊之处在于：

1.输入样本就是输出样本。

2.隐藏层的神经元数量小于样本的维度。

粗看起来，这类恒等变换没有太大意义。然而这类恒等变换之所以能够成立，最根本的地方在于，隐藏层的神经元具有表达输出样本的能力，也就是用低维表达高维的能力。反过来，我们就可以利用这一点，实现数据的降维操作。

但是，不是所有的数据都能够降维，而这种情况通常会导致Autoencoder的训练失败。

和Autoencoder类似的神经网络还有：Denoising Autoencoder（DAE）、Variational Autoencoder（VAE）、Sparse Autoencoder（SAE）。

参考：

http://ufldl.stanford.edu/tutorial/unsupervised/Autoencoders/

http://blog.csdn.net/changyuanchn/article/details/15681853

深度学习之autoencoder

http://www.cnblogs.com/neopenx/p/4370350.html

降噪自动编码器（Denoising Autoencoder)

https://zhuanlan.zhihu.com/p/27549418

花式解释AutoEncoder与VAE

https://mp.weixin.qq.com/s/lODy8ucB3Bw9Y1sy1NxTJg

无监督学习中的两个非概率模型：稀疏编码与自编码器

https://mp.weixin.qq.com/s/LQFuXgI7uZK2UKRfZvlVbA

Variational AutoEncoder

https://mp.weixin.qq.com/s/lnSMdOk8fYfdU4aGeI5j7Q

未标注的数据如何处理？一文读懂变分自编码器VAE

https://mp.weixin.qq.com/s/QuDa__mi1NX1wOxo5Ki94A

深度学习：自动编码器基础和类型

http://blog.csdn.net/losteng/article/details/51067216

CAE(Convolutional Auto-Encode) 卷积自编码

https://mp.weixin.qq.com/s/q-WExyS-zylMA-L8ojOgRg

简单易懂的自动编码器

https://mp.weixin.qq.com/s/ZlLuhu08m_RnD-h86df8sA

清华大学提出SA-VAE框架，通过单样本/少样本学习生成任意风格的汉字

https://mp.weixin.qq.com/s/t4YYIl4o_TAPG7737ZfiaA

面向无监督任务：DeepMind提出神经离散表示学习生成模型VQ-VAE

# 词向量

## One-hot Representation

NLP是ML和DL的重要研究领域。但是多数的ML或DL算法都是针对数值进行计算的，因此如何将自然语言中的文本表示为数值，就成为了一个重要的基础问题。

词向量顾名思义就是单词的向量化表示。最简单的词向量表示法当属**One-hot Representation**：

假设语料库的单词表中有N个单词，则词向量可表示为N维向量$$[0,\dots,0,1,0,\dots,0]$$

这种表示法由于N维向量中只有一个非零元素，故名。该非零元素的序号，就是所表示的单词在单词表中的序号。

One-hot Representation的缺点在于：

1.该表示法中，由于任意两个单词的词向量都是正交的，因此无法反映单词之间的语义相似度。

2.一个词库的大小是$$10^5$$以上的量级。维度过高，会妨碍神经网络学习到稀疏特征。

参考：

https://mp.weixin.qq.com/s?__biz=MzI4MzM2NTU0Mg==&mid=2247483698&idx=1&sn=cf185232e43b4523ab9b0bc0ce425ed4

One-Hot编码与哑变量

## Word Embedding

针对One-hot Representation的不足，Bengio提出了Distributed Representation，也称为Word Embedding。

![](/images/article/word_vector.png)

Word Embedding的思路如上图所示，即想办法**将高维的One-hot词向量映射到低维的语义空间中**。

Bengio自己提出了一种基于神经网络的Word Embedding的方案，然而由于计算量过大，目前已经被淘汰了。

参考：

http://www.cnblogs.com/neopenx/p/4570648.html

词向量概况


