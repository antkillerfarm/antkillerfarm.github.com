---
layout: post
title:  深度学习（二十七）——VAE（1）
category: DL 
---

# Attention（续）

## Position Embedding

然而，只要稍微思考一下就会发现，这样的模型并不能捕捉序列的顺序！换句话说，如果将K,V

按行打乱顺序（相当于句子中的词序打乱），那么Attention的结果还是一样的。这就表明了，到目前为止，Attention模型顶多是一个非常精妙的“词袋模型”而已。

这问题就比较严重了，大家知道，对于时间序列来说，尤其是对于NLP中的任务来说，顺序是很重要的信息，它代表着局部甚至是全局的结构，学习不到顺序信息，那么效果将会大打折扣（比如机器翻译中，有可能只把每个词都翻译出来了，但是不能组织成合理的句子）。

于是Google再祭出了一招——Position Embedding，也就是“位置向量”，将每个位置编号，然后每个编号对应一个向量，通过结合位置向量和词向量，就给每个词都引入了一定的位置信息，这样Attention就可以分辨出不同位置的词了。

## Hard Attention

论文：

《Show, Attend and Tell: Neural Image Caption Generation with Visual Attention》

我们之前所描述的传统的Attention Mechanism是Soft Attention。Soft Attention是参数化的（Parameterization），因此可导，可以被嵌入到模型中去，直接训练。梯度可以经过Attention Mechanism模块，反向传播到模型其他部分。

相反，Hard Attention是一个随机的过程。Hard Attention不会选择整个encoder的输出做为其输入，Hard Attention会依概率Si来采样输入端的隐状态一部分来进行计算，而不是整个encoder的隐状态。为了实现梯度的反向传播，需要采用蒙特卡洛采样的方法来估计模块的梯度。

两种Attention Mechanism都有各自的优势，但目前更多的研究和应用还是更倾向于使用Soft Attention，因为其可以直接求导，进行梯度反向传播。

## Local Attention

论文：

《Effective Approaches to Attention-based Neural Machine Translation》

>Thang Luong，越南人，Stanford博士（2016），现为Google研究员。导师是Christopher Manning。
>个人主页：   
>https://nlp.stanford.edu/~lmthang/

>Christopher Manning，澳大利亚人，Stanford博士（1994），现为Stanford教授。从事NLP近三十年，率先将统计方法引入NLP。

![](/images/img2/Global_attention.png)

传统的Attention model中，所有的hidden state都被用于计算Context vector的权重，因此也叫做Global Attention。

Local Attention：Global Attention有一个明显的缺点就是，每一次，encoder端的所有hidden state都要参与计算，这样做计算开销会比较大，特别是当encoder的句子偏长，比如，一段话或者一篇文章，效率偏低。因此，为了提高效率，Local Attention应运而生。

Local Attention是一种介于Kelvin Xu所提出的Soft Attention和Hard Attention之间的一种Attention方式，即把两种方式结合起来。其结构如下图所示。

![](/images/img2/Local_attention.png)

## Attention over Attention

《Attention-over-Attention Neural Networks for Reading Comprehension》

![](/images/img2/Attention-over-Attention.png)

## 总结

从最初的原始Attention，到后面的各种示例，不难看出**Attention实际上是一个大箩筐，凡是不好用CNN、RNN、FC概括的累计乘加，基本都可冠以XX Attention的名义**。

虽然，权重的确代表了Attention的程度，然而直接叫累计乘加，似乎更接近操作本身一些。

考虑到神经网络的各种操作基本都是累计乘加的变种，因此，Attention is All You Need实际上是很自然的结论，你总可以对Attention进行修改，让它实现CNN、RNN、FC的效果。

这点在AI芯片领域尤为突出，**无论IC架构差异如何巨大，硬件底层基本就是乘累加器。**

## 参考

http://geek.csdn.net/news/detail/106118

Attention and Augmented Recurrent Neural Networks译文

http://blog.csdn.net/rtygbwwwerr/article/details/50548311

Neural Turing Machines

http://www.robots.ox.ac.uk/~tvg/publications/talks/NeuralTuringMachines.pdf

Neural Turing Machines

http://blog.csdn.net/malefactor/article/details/50550211

自然语言处理中的Attention Model

https://yq.aliyun.com/articles/65356

图文结合详解深度学习Memory & Attention

http://www.cosmosshadow.com/ml/%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C/2016/03/08/Attention.html

Attention

http://geek.csdn.net/news/detail/50558

深度学习和自然语言处理中的attention和memory机制

https://zhuanlan.zhihu.com/p/25928551

用深度学习（CNN RNN Attention）解决大规模文本分类问题-综述和实践

http://blog.csdn.net/leo_xu06/article/details/53491400

视觉注意力的循环神经网络模型

https://mp.weixin.qq.com/s/XrlveG0kwij2qNL45TZdBg

Attention的另类用法

https://zhuanlan.zhihu.com/p/31547842

深度学习中Attention Mechanism详细介绍：原理、分类及应用

https://zhuanlan.zhihu.com/p/32089282

Attention学习笔记

https://mp.weixin.qq.com/s/0yb-YRGe-q4-vpKpuE4D_w

多种注意力机制互补完成VQA（视觉问答）

https://mp.weixin.qq.com/s/LQ7uv0-AakkHE5b17yemqw

Awni Hannun：序列模型Attention Model中的问题与挑战

https://mp.weixin.qq.com/s/xr_1ZYbvADMMwgxLEAflCw

如何在语言翻译中理解Attention Mechanism？

https://mp.weixin.qq.com/s/Nyq_36aFmQYRWdpgbgxpuA

将注意力机制引入RNN，解决5大应用领域的序列预测问题

https://mp.weixin.qq.com/s/2gxp7A38epQWoy7wK8Nl6A

谷歌翻译最新突破，“关注机制”让机器读懂词与词的联系

https://mp.weixin.qq.com/s/g2PcmsDW9ixUCh_yP8W-Vg

各类Seq2Seq模型对比及《Attention Is All You Need》中技术详解

https://mp.weixin.qq.com/s/FtI94xY6a8TEvFCHfjMnmA

小组讨论谷歌机器翻译Attention is All You Need

https://mp.weixin.qq.com/s/SqIMkiP1IZMGWzwZWGOI7w

谈谈神经网络的注意机制和使用方法

https://mp.weixin.qq.com/s/POYTh4Jf7HttxoLhrHZQhw

基于双向注意力机制视觉问答pyTorch实现

https://mp.weixin.qq.com/s/EMCZHuvk5dOV_Rz00GkJMA

近年火爆的Attention模型，它的套路这里都有！

https://mp.weixin.qq.com/s/y_hIhdJ1EN7D3p2PVaoZwA

阿里北大提出新attention建模框架，一个模型预测多种行为

https://mp.weixin.qq.com/s/Yq3S4WrsQRQC06GvRgGjTQ

打入神经网络思维内部

https://mp.weixin.qq.com/s/MJ1578NdTKbjU-j3Uuo9Ww

基于文档级问答任务的新注意力模型

https://mp.weixin.qq.com/s/C4f0N_bVWU9YPY34t-HAEA

UNC&Adobe提出模块化注意力模型MAttNet，解决指示表达的理解问题

https://mp.weixin.qq.com/s/V3brXuey7Gear0f_KAdq2A

基于注意力机制的交易上下文感知推荐，悉尼科技大学和电子科技大学最新工作

# VAE

变分自编码器（Variational Auto-Encoder，VAE）是Autoencoder的一种扩展。

论文：

《Auto-Encoding Variational Bayes》

以下部分主要摘自：

https://kexue.fm/archives/5253

变分自编码器：原来是这么一回事

## 分布变换

通常我们会拿VAE跟GAN比较，的确，它们两个的目标基本是一致的——希望构建一个从隐变量Z生成目标数据X的模型，但是实现上有所不同。更准确地讲，它们是假设了Z服从某些常见的分布（比如正态分布或均匀分布），然后希望训练一个模型$$X=g(Z)$$，这个模型能够将原来的概率分布映射到训练集的概率分布，也就是说，它们的目的都是进行分布之间的映射。

现在假设Z服从标准的正态分布，那么我就可以从中采样得到若干个$$Z_1, Z_2, \dots, Z_n$$，然后对它做变换得到$$\hat{X}_1 = g(Z_1),\hat{X}_2 = g(Z_2),\dots,\hat{X}_n = g(Z_n)$$，我们怎么判断这个通过f构造出来的数据集，它的分布跟我们目标数据集的分布是不是一样的呢？

![](/images/img2/VAE.png)

**生成模型的难题就是判断生成分布与真实分布的相似度，因为我们只知道两者的采样结果，不知道它们的分布表达式。**

有读者说不是有KL散度吗？当然不行，因为KL散度是根据两个概率分布的表达式来算它们的相似度的，然而目前我们并不知道它们的概率分布的表达式，我们只有一批从构造的分布采样而来的数据$$\{\hat{X}_1,\hat{X}_2,\dots,\hat{X}_n\}$$，还有一批从真实的分布采样而来的数据$$\{X_1,X_2,\dots,X_n\}$$（也就是我们希望生成的训练集）。我们只有样本本身，没有分布表达式，当然也就没有方法算KL散度。

虽然遇到困难，但还是要想办法解决的。GAN的思路很直接粗犷：既然没有合适的度量，那我干脆把这个度量也用神经网络训练出来吧。而VAE则使用了一个精致迂回的技巧。

## VAE的传统理解

首先我们有一批数据样本$$\{X_1,\dots,X_n\}$$，其整体用X来描述，我们本想根据$$\{X_1,\dots,X_n\}$$得到X的分布p(X)，如果能得到的话，那我直接根据p(X)来采样，就可以得到所有可能的X了（包括$$\{X_1,\dots,X_n\}$$以外的），这是一个终极理想的生成模型了。当然，这个理想很难实现，于是我们将分布改一改：

$$p(X)=\sum_Z p(X|Z)p(Z)$$

此时$$p(X\mid Z)$$就描述了一个由Z来生成X的模型，而我们假设Z服从标准正态分布，也就是$$p(Z)=\mathcal{N}(0,I)$$。如果这个理想能实现，那么我们就可以先从标准正态分布中采样一个Z，然后根据Z来算一个X，也是一个很棒的生成模型。接下来就是结合自编码器来实现重构，保证有效信息没有丢失，再加上一系列的推导，最后把模型实现。框架的示意图如下：

![](/images/img2/VAE_2.png)

看出了什么问题了吗？如果像这个图的话，我们其实完全不清楚：究竟经过重新采样出来的$$Z_k$$，是不是还对应着原来的$$X_k$$，所以我们如果直接最小化$$\mathcal{D}(\hat{X}_k,X_k)^2$$（这里D代表某种距离函数）是很不科学的，而事实上你看代码也会发现根本不是这样实现的。

