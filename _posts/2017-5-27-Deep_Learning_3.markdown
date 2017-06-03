---
layout: post
title:  深度学习（三）——词向量, 神经元激活函数进阶
category: theory 
---

# 词向量（续）

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

这类方案的典型是Latent Semantic Analysis(LSA)。参见《机器学习（二十一）》。

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

## doc2vec

doc2vec是Mikolov在word2vec的基础上提出的一种生成句子向量的方法。

论文：

《Distributed Representations of Sentences and Documents》

http://cs.stanford.edu/~quocle/paragraph_vector.pdf

![](/images/article/doc2vec.png)

上图是doc2vec的框架图，可以看出

https://www.zhihu.com/question/33952003

如何通过词向量技术来计算2个文档的相似度?

## FastText

Word2Vec作者Mikolov加盟Facebook之后，提出了文本分类新作FastText。

FastText模型架构和Word2Vec中的CBOW模型很类似。不同之处在于，FastText预测标签，而CBOW模型预测中间词。

http://www.algorithmdog.com/fast-fasttext

Github：

https://github.com/facebookresearch/fastText

## Item2Vec

本质上，word2vec模型是在word-context的co-occurrence矩阵基础上建立起来的。因此，任何基于co-occurrence矩阵的算法模型，都可以套用word2vec算法的思路加以改进。

比如，推荐系统领域的协同过滤算法。

协同过滤算法是建立在一个user-item的co-occurrence矩阵的基础上，通过行向量或列向量的相似性进行推荐。如果我们将同一个user购买的item视为一个context，就可以建立一个item-context的矩阵。进一步的，可以在这个矩阵上借鉴CBoW模型或Skip-gram模型计算出item的向量表达，在更高阶上计算item间的相似度。

论文：

《Item2Vec: Neural Item Embedding for Collaborative Filtering》

# RNN

RNN是Recurrent Neural Network和Recursive Neural Network的简称。前者主要用于处理和时序相关的输入，而后者目前已经没落。本文只讨论前者。

![](/images/article/RNN.png)

上图是RNN的结构图。

参考：

http://blog.csdn.net/aws3217150/article/details/50768453

递归神经网络(RNN)简介

http://blog.csdn.net/heyongluoyao8/article/details/48636251

循环神经网络(RNN, Recurrent Neural Networks)介绍

http://mp.weixin.qq.com/s?__biz=MzIzODExMDE5MA==&mid=2694182661&idx=1&sn=ddfb3f301f5021571992824b21ddcafe

循环神经网络

http://www.wildml.com/2015/10/recurrent-neural-networks-tutorial-part-3-backpropagation-through-time-and-vanishing-gradients/

Backpropagation Through Time算法

# LSTM

http://www.jianshu.com/p/9dc9f41f0b29

理解LSTM网络

http://www.csdn.net/article/2015-06-05/2824880

深入浅出LSTM神经网络

https://zhuanlan.zhihu.com/p/25821063

循环神经网络——scan实现LSTM

http://blog.csdn.net/a635661820/article/details/45390671

LSTM简介以及数学推导(FULL BPTT)

# 深度强化学习

![](/images/article/reinforcement_learning.png)

https://www.nervanasys.com/demystifying-deep-reinforcement-learning/

深度强化学习揭秘

# 神经元激活函数进阶

https://zhuanlan.zhihu.com/p/22142013

深度学习中的激活函数导引

http://blog.csdn.net/u012328159/article/details/69898137

几种常见的激活函数

https://mp.weixin.qq.com/s/Hic01RxwWT_YwnErsJaipQ

什么是激活函数？

# GAN

http://www.jianshu.com/p/e2d2d7cbbe49

50行代码实现GAN

http://mp.weixin.qq.com/s/bzwG0QxnP2drqS4RwcZlBg

微软详解：到底什么是生成式对抗网络GAN？

https://mp.weixin.qq.com/s/oCDlhzjOYTIhsr5JuoRCJQ

IRGAN：大一统信息检索模型的博弈竞争

https://mp.weixin.qq.com/s/QacQCrjh3KmrQSMp-G_rEg

贝叶斯生成对抗网络

https://github.com/hindupuravinash/the-gan-zoo

GAN的各种变种。

