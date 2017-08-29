---
layout: post
title:  深度学习（三）——Autoencoder, 词向量
category: theory 
---

# CNN（续）

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

3.多通道卷积除了二维空间信息的卷积之外，还包括了通道间信息的卷积。这也是CNN中1*1卷积的意义之一。

多通道卷积操作最终可以转化为矩阵运算，如下图所示：

![](/images/article/conv.jpg)

参见：

http://blog.csdn.net/u014114990/article/details/51125776

多通道(比如RGB三通道)卷积过程

https://www.zhihu.com/question/56024942

卷积神经网络中用1*1卷积有什么作用或者好处呢？

https://www.zhihu.com/question/28385679

在Caffe中如何计算卷积？

## CNN的反向传播算法

由于卷积和池化两层，不是一般的神经网络结构。因此CNN的反向传播算法实际上也是很有技巧的。

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

http://mp.weixin.qq.com/s/dvuX3Ih_DZrv0kgqFn8-lg

卷积神经网络结构变化——可变形卷积网络deformable convolutional networks

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

# 词向量

## One-hot Representation

NLP是ML和DL的重要研究领域。但是多数的ML或DL算法都是针对数值进行计算的，因此如何将自然语言中的文本表示为数值，就成为了一个重要的基础问题。

词向量顾名思义就是单词的向量化表示。最简单的词向量表示法当属**One-hot Representation**：

假设语料库的单词表中有N个单词，则词向量可表示为N维向量$$[0,\dots,0,1,0,\dots,0]$$

这种表示法由于N维向量中只有一个非零元素，故名。该非零元素的序号，就是所表示的单词在单词表中的序号。

One-hot Representation的缺点在于：

1.该表示法中，由于任意两个单词的词向量都是正交的，因此无法反映单词之间的语义相似度。

2.一个词库的大小是$$10^5$$以上的量级。维度过高，会妨碍神经网络学习到稀疏特征。

## Word Embedding

针对One-hot Representation的不足，Bengio提出了Distributed Representation，也称为Word Embedding。

![](/images/article/word_vector.png)

Word Embedding的思路如上图所示，即想办法**将高维的One-hot词向量映射到低维的语义空间中**。

Bengio自己提出了一种基于神经网络的Word Embedding的方案，然而由于计算量过大，目前已经被淘汰了。

参考：

http://www.cnblogs.com/neopenx/p/4570648.html

词向量概况

## word2vec

除了Bengio方案之外，早期人们还尝试过基于共生矩阵（Co-occurrence Matrix）SVD分解的Word Embedding方案。该方案对于少量语料有不错的效果，但一旦语料增大，计算量即呈指数级上升。

这类方案的典型是Latent Semantic Analysis(LSA)。参见《机器学习（二十）》。

Tomas Mikolov于2013年对Bengio方案进行了简化改进，提出了目前最为常用的word2vec方案。

介绍word2vec的数学原理比较好的有：

《Deep Learning实战之word2vec》，网易有道的邓澍军、陆光明、夏龙著。

《word2vec中的数学》，peghoty著。该书的网页版：

http://blog.csdn.net/itplus/article/details/37969519

老惯例这里只对最重要的内容进行摘要。

### CBOW & Skip-gram

![](/images/article/word2vec.png)

上图是word2vec中使用到的两种模型的示意图。

从图中可知，word2vec虽然使用了神经网络，但是从层数来说，只有3层而已，还谈不上是Deep Learning。但是考虑到DL，基本就是神经网络的同义词，因此这里仍然将word2vec归为DL的范畴。

>注：深度学习不全是神经网络，周志华教授提出的gcForest就是一个有益的另类尝试。

研究一个神经网络模型，最重要的除了神经元之间的连接关系之外，就是神经网络的输入输出了。

CBOW（Continuous Bag-of-Words Model）模型和Skip-gram（Continuous Skip-gram Model）模型脱胎于n-gram模型，即一个词出现的概率只与它前后的n个词有关。这里的n也被称为窗口大小.

上图中，窗口大小为5，即一个中心词$$\{w_t\}$$+前面的两个词$$\{w_{t-1},w_{t-2}\}$$+后面的两个词$$\{w_{t+1},w_{t+2}\}$$。

| 名称 | CBOW | Skip-gram |
|:--:|:--:|:--:|
| 输入 | $$\{w_{t-1},w_{t-2},w_{t+1},w_{t+2}\}$$ | $$\{w_t\}$$ |
| 输出 | $$\{w_t\}$$ | $$\{w_{t-1},w_{t-2},w_{t+1},w_{t+2}\}$$ |
| 目标 | 在输入确定的情况下，最大化输出值的概率。 | 在输入确定的情况下，最大化输出值的概率。 |

### Hierarchical Softmax

word2vec的输出层有两种模型：Hierarchical Softmax和Negative Sampling。

Softmax是DL中常用的输出层结构，它表征**多分类中的每一个分类所对应的概率**。

然而在这里，每个分类表示一个单词，即：分类的个数=词汇表的单词个数。如此众多的分类直接映射到隐层，显然并不容易训练出有效特征。

Hierarchical Softmax是Softmax的一个变种。这时的输出层不再是一个扁平的多分类层，而变成了一个层次化的二分类层。

Hierarchical Softmax一般基于Huffman编码构建。在本例中，我们首先统计词频，以获得每个词所对应的Huffman编码，然后输出层会利用Huffman编码所对应的层次二叉树的路径来计算每个词的概率，并逆传播到隐藏层。

由Huffman编码的特性可知，Hierarchical Softmax的计算量要小于一般的Softmax。

### Negative Sampling

在CBOW模型中，已知w的上下文Context(w)需要预测w，则w就是正样本，而其他词是负样本。

负样本那么多，该如何选取呢？Negative Sampling就是一种对负样本采样的方法。

![](/images/article/Negative_Sampling.png)

上图是Negative Sampling的原理图。L轴表示的是词频分布，很明显这是一个非等距剖分。而M轴是一个等距剖分。

每次生成一个M轴上的随机数，将之映射到L轴上的一个单词。映射方法如上图中的虚线所示。

除了word2vec之外，类似的Word Embedding方案还有SENNA、RNN-LM、Glove等。但影响力仍以word2vec最大。

Skip-Gram Negative Sampling，又被简称为SGNS。

## doc2vec

我们知道，word是sentence的基本组成单位。一个最简单也是最直接得到sentence embedding的方法是将组成sentence的所有word的embedding向量全部加起来。

显然，这种简单粗暴的方法会丢失很多信息。

doc2vec是Mikolov在word2vec的基础上提出的一种生成句子向量的方法。

论文：

《Distributed Representations of Sentences and Documents》

http://cs.stanford.edu/~quocle/paragraph_vector.pdf

![](/images/article/doc2vec.png)

上图是doc2vec的框架图，可以看出doc2vec的原理与word2vec基本一致，区别仅在于前者多出来一个Paragraph Vector参与CBOW或Skip-gram的训练。

Paragraph Vector可以和Word Vector一起生成，也可以单独生成，也就是训练时，采用预训练的Word Vector，并只改变Paragraph Vector的值。

https://www.zhihu.com/question/33952003

如何通过词向量技术来计算2个文档的相似度?

