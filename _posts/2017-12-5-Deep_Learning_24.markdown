---
layout: post
title:  深度学习（二十四）——L2 Normalization, 数据增强, Attention（1）
category: DL 
---

# LSTM进阶（续）

## 《Video Summarization with Long Short-term Memory》

这是一篇用于提取视频关键帧（也叫静态视频摘要）的论文，是南加州大学沙飞小组的作品。

![](/images/img2/dppLSTM.png)

上图是该文提出的DPP LSTM的网络结构图。它的主体是一个BiLSTM，算是中规中矩吧。

该文的创新点在于提出了DPP loss的概念。上图中的$$y_t$$表示帧的分值（越大表示越重要），$$\phi_t$$表示帧之间的相似度。该文的实验表明，将两个特征分开抽取，有助于提升模型的准确度。

这篇论文主要用到了3个数据集：

TVSum dataset: 

https://github.com/yalesong/tvsum

这个需要Yahoo账号和一个高校的邮件地址才行。

SumMe dataset: 

https://people.ee.ethz.ch/~gyglim/vsum/#benchmark

OVP and YouTube datasets: 

https://sites.google.com/site/vsummsite/

需要翻墙。

## LSTM加速技巧

LSTM的主要运算量集中在$$W[h_{t-1},x_t]$$上，这里实际上可以用$$W[(h_{t-2}+\Delta h_{t-1}),x_t]$$代替。

由于时间序列通常具有惯性，因此$$\Delta h_{t-1}$$一般包含了大量的0，这对于某些具有跳0功能的硬件来说，是非常有利的。

## Convolutional LSTM Network

论文：

《Convolutional LSTM Network: A Machine Learning Approach for Precipitation Nowcasting》

## 参考

https://mp.weixin.qq.com/s/4IHzOAvNhHG9c8GP0zXVkQ

Simple Recurrent Unit For Sentence Classification

https://mp.weixin.qq.com/s/fCzHbOi7aJ8-W9GzctUFNg

LSTM文本分类实战

http://mp.weixin.qq.com/s/3nwgft9c27ih172ANwHzvg

从零开始：如何使用LSTM预测汇率变化趋势

https://mp.weixin.qq.com/s/M18c3sgvjV2b2ksCsyOxbQ

Nested LSTM：一种能处理更长期信息的新型LSTM扩展

https://www.zhihu.com/question/62399257

如何理解LSTM后接CRF？

https://mp.weixin.qq.com/s/XAbzaMXP3QOret_vxqVF9A

用深度学习LSTM炒股：对冲基金案例分析

https://mp.weixin.qq.com/s/eeA5RZh35BvlFt45ywVvFg

可视化LSTM网络：探索“记忆”的形成

https://mp.weixin.qq.com/s/h-MYTNTLy7ToPPEZ2JVHpw

阿里巴巴论文提出Advanced LSTM：关于更优时间依赖性刻画在情感识别方面的应用

https://mp.weixin.qq.com/s/pv3gQfCayGmsmGKLbMIFpA

神奇！只有遗忘门的LSTM性能优于标准LSTM

https://mp.weixin.qq.com/s/8BPZ_M8EGk3KxkSleYWSNw

训练可解释、可压缩、高准确率的LSTM

# L2 Normalization

L2 Normalization本身并不复杂，然而多数资料都只提到1维的L2 Normalization的计算公式：

$$x=[x_1,x_2,\dots,x_d]\\
y=[y_1,y_2,\dots,y_d]\\
y=\frac{x}{\sqrt{\sum_{i=1}^dx_i^2}}=\frac{x}{\sqrt{x^Tx}}
$$

对于多维L2 Normalization几乎未曾提及，这里以3维tensor：A[width, height, channel]为例介绍一下多维L2 Normalization的计算方法。

多维L2 Normalization有一个叫axis(有时也叫dim)的参数，如果axis=0的话，实际上就是将整个tensor flatten之后，再L2 Normalization。这个是比较简单的。

这里说说axis=3的情况。axis=3意味着对channel进行Normalization，也就是：

$$B_{xy}=\sum_{z=0}^Z \sqrt{A_{xyz}^2}\\
C_{xyz}=\frac{A_{xyz}}{B_{xy}}\\
D_{xyz}=C_{xyz} \cdot S_{z}
$$

一般来说，求出C被称作L2 Normalization，而求出D被称作L2 Scale Normalization，S被称为Scale。

# 数据增强

https://mp.weixin.qq.com/s/GqPfvWwH1T0XFwiZ86cW8A

SamplePairing：针对图像处理领域的高效数据增强方式

https://mp.weixin.qq.com/s/cQtXvOjSXFc4YKn7ANBc_w

谷歌大脑提出自动数据增强方法AutoAugment：可迁移至不同数据集

https://mp.weixin.qq.com/s/ojFo7-gUh73iK3uImFS2-Q

一文道尽主流开源框架中的数据增强

https://mp.weixin.qq.com/s/xJhWu-1FyhIWbFBC5oHMkw

一文道尽深度学习中的数据增强方法（上）

https://mp.weixin.qq.com/s/OctAGrcBB0a6TOGWMmVKUw

深度学习中的数据增强（下）

https://mp.weixin.qq.com/s/lMU6_ywQqneyunqEV6uDiA

如何改善你的训练数据集？

https://mp.weixin.qq.com/s/ooX9Hj5ejO6po6Ghb4zOug

一文解读合成数据在机器学习技术下的表现

https://zhuanlan.zhihu.com/p/33485388

mixup与paring samples ，ICLR2018投稿论文的数据增广两种方式

https://mp.weixin.qq.com/s/_7xFBLPGT0VRTJ22toHJ3g

深度学习中常用的图像数据增强方法

https://mp.weixin.qq.com/s/sXV9epWguGbJEZYo4yNp5Q

如何正确使用样本扩充改进目标检测性能

https://zhuanlan.zhihu.com/p/46833956

图像数据增强之弹性形变（Elastic Distortions）

# Attention

## 概述

众所周知，RNN在处理长距离依赖关系时会出现问题。理论上，LSTM这类结构能够处理这个问题，但在实践中，长距离依赖关系仍旧是个问题。例如，研究人员发现将原文倒序（将其倒序输入编码器）产生了显著改善的结果，因为从解码器到编码器对应部分的路径被缩短了。同样，两次输入同一个序列似乎也有助于网络更好地记忆。

倒序句子这种方法属于“hack”手段。它属于被实践证明有效的方法，而不是有理论依据的解决方法。

大多数翻译的基准都是用法语、德语等语种，它们和英语非常相似（即使汉语的词序与英语也极其相似）。但是有些语种（像日语）句子的最后一个词语在英语译文中对第一个词语有高度预言性。那么，倒序输入将使得结果更糟糕。

还有其它办法吗？那就是Attention机制。

![](/images/article/attention.png)

上图是Attention机制的结构图。y是编码器生成的译文词语，x是原文的词语。上图使用了双向递归网络，但这并不是重点，你先忽略反向的路径吧。重点在于现在每个解码器输出的词语$$y_t$$取决于所有输入状态的一个权重组合，而不只是最后一个状态。a是决定每个输入状态对输出状态的权重贡献。因此，如果$$a_{3,2}$$的值很大，这意味着解码器在生成译文的第三个词语时，会更关注于原文句子的第二个状态。a求和的结果通常归一化到1（因此它是输入状态的一个分布）。

Attention机制的一个主要优势是它让我们能够解释并可视化整个模型。举个例子，通过对attention权重矩阵a的可视化，我们能够理解模型翻译的过程。

![](/images/article/attention_2.png)

我们注意到当从法语译为英语时，网络模型顺序地关注每个输入状态，但有时输出一个词语时会关注两个原文的词语，比如将“la Syrie”翻译为“Syria”。

如果再仔细观察attention的等式，我们会发现attention机制有一定的成本。我们需要为每个输入输出组合分别计算attention值。50个单词的输入序列和50个单词的输出序列需要计算2500个attention值。这还不算太糟糕，但如果你做字符级别的计算，而且字符序列长达几百个字符，那么attention机制将会变得代价昂贵。

attention机制解决的根本问题是允许网络返回到输入序列，而不是把所有信息编码成固定长度的向量。正如我在上面提到，我认为使用attention有点儿用词不当。换句话说，attention机制只是简单地让网络模型访问它的内部存储器，也就是编码器的隐藏状态。在这种解释中，网络选择从记忆中检索东西，而不是选择“注意”什么。不同于典型的内存，这里的内存访问机制是弹性的，也就是说模型检索到的是所有内存位置的加权组合，而不是某个独立离散位置的值。弹性的内存访问机制好处在于我们可以很容易地用反向传播算法端到端地训练网络模型（虽然有non-fuzzy的方法，其中的梯度使用抽样方法计算，而不是反向传播）。

论文：

《Learning to combine foveal glimpses with a third-order Boltzmann machine》

《Learning where to Attend with Deep Architectures for Image Tracking》

《Neural Machine Translation by Jointly Learning to Align and Translate》

## Neural Turing Machines

以下章节主要翻译自下文：

https://distill.pub/2016/augmented-rnns/

Attention and Augmented Recurrent Neural Networks

>distill.pub虽然blog数量不多，但篇篇都是经典。背后站台的更有Yoshua Bengio、Ian Goodfellow、Andrej Karpathy等大牛。

该文主要讲述了Attention在RNN领域的应用。

----

NTM是一种使用Neural Network为基础来实现传统图灵机的理论计算模型。利用该模型，可以通过训练的方式让系统“学会”具有时序关联的任务流。

图灵机的详细定义可参见：

https://baike.baidu.com/item/图灵机

![](/images/img2/NTM.png)

和传统图灵机相比，这里的memory中保存的是向量，我们的目标是根据输入序列，确定读写操作。具体的步骤如下图所示：

![](/images/img2/NTM_2.png)

1.计算query vector和memory中每个vector的相似度。

2.将相似度softmax为一个分布。

3.用之前训练好的attention模型调整分布值。

4.图灵机的Shift操作也可以引入attention模型。

5.sharpen分布值，选择最终的读写操作。sharpen操作，实际上就是选择较大的概率值，而忽略较小的概率值。

## Attentional Interfaces

以下是另外的一些Attention的用例。

![](/images/img2/Attention.png)

上图中，一个RNN模型可以输入另一个RNN模型的输出。它在每一步都会关注另一个RNN模型的不同位置。

![](/images/img2/Attention_2.png)

这是一个和NTM非常类似的模型。RNN模型生成一个搜索词描述其希望关注的位置。然后计算每条内容与搜索词的点乘得分，表示其与搜索词的匹配程度。这些分数经过softmax函数的运算产生聚焦的分布。

![](/images/img2/Attention_3.png)

上图是语言翻译方面的模型。若用传统的序列到序列模型做翻译，需要把整个输入词汇串缩简为单个向量，然后再展开恢复为序列目标语言的词汇串。Attention机制则可以避免上述操作，RNN模型逐个处理输入词语的信息，随即生成相对应的词语。

![](/images/img2/Attention_4.png)

RNN模型之间的这类聚焦还有许多其它的应用。它可以用于语音识别，其中一个RNN模型处理语音信号，另一个RNN模型则滑动处理其输出，然后关注相关的区域生成文本内容。

## Adaptive Computation Time

标准的RNN模型每一步所消耗的计算时间都相同。这似乎与我们的直觉不符。我们在思考难题的时候难道不需要更多的时间吗？

适应性计算时间（Adaptive Computation Time）是解决RNN每一步消耗不同计算量的方法。笼统地说：就是让RNN模型可以在每个时间片段内进行不同次数的计算步骤。

![](/images/img2/ACT.png)

上图是ACT的网络结构图。下面来分步讲解一下。

