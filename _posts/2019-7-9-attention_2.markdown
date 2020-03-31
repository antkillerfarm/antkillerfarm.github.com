---
layout: post
title:  Attention（二）——花式Attention, Transformer
category: Attention 
---

# 花式Attention

## Self Attention（续）

其中，n表示序列长度，d表示词向量的维度，k表示卷积核的大小，r表示restricted self-attention中的neighborhood的数量。

可以看出，在n小于d的情况下，Self Attention是有计算量的优势的。

所谓的restricted self-attention是指：假设当前词只与前后r个词发生联系，因此注意力也只发生在这2r+1个词之间。

可以看到self-attention和convolution有点儿神似：

- 它摒弃了CNN的局部假设，改为寻找长距离的关联依赖。

- 它没有了递归的限制，就像CNN一样可以在每一层内实现并行。

- self-attention借鉴CNN中multi-kernel的思想，进一步进化成为Multi-Head attention。每一个不同的head使用不同的线性变换，学习不同的relationship。

## Position Embedding

然而，只要稍微思考一下就会发现，这样的模型并不能捕捉序列的顺序！换句话说，如果将K,V按行打乱顺序（相当于句子中的词序打乱），那么Attention的结果还是一样的。这就表明了，到目前为止，Attention模型顶多是一个非常精妙的“词袋模型”而已。

这问题就比较严重了，大家知道，对于时间序列来说，尤其是对于NLP中的任务来说，顺序是很重要的信息，它代表着局部甚至是全局的结构，学习不到顺序信息，那么效果将会大打折扣（比如机器翻译中，有可能只把每个词都翻译出来了，但是不能组织成合理的句子）。

于是Google再祭出了一招——Position Embedding，也就是“位置向量”，将每个位置编号，然后每个编号对应一个向量，通过结合位置向量和词向量，就给每个词都引入了一定的位置信息，**这样Attention就可以分辨出不同位置的词了**。

Position Embedding的公式为：

$$\left\{\begin{aligned}&PE_{2i}(p)=\sin\Big(p/10000^{2i/{d_{pos}}}\Big)\\ 
&PE_{2i+1}(p)=\cos\Big(p/10000^{2i/{d_{pos}}}\Big) 
\end{aligned}\right.$$

由于我们有$$\sin(\alpha+\beta)=\sin\alpha\cos\beta+\cos\alpha\sin\beta$$以及$$\cos(\alpha+\beta)=\cos\alpha\cos\beta-\sin\alpha\sin\beta$$，这表明位置p+k的向量可以表示成位置p的向量的线性变换，这提供了表达相对位置信息的可能性。

参考：

https://zhuanlan.zhihu.com/p/92017824

浅谈Transformer-based模型中的位置表示

https://mp.weixin.qq.com/s/ENpXBYQ4hfdTLSXBIoF00Q

如何优雅地编码文本中的位置信息？三种positioanl encoding方法简述

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

https://blog.csdn.net/mijiaoxiaosan/article/details/73251443

对Attention is all you need的理解

http://geek.csdn.net/news/detail/106118

Attention and Augmented Recurrent Neural Networks译文

http://blog.csdn.net/rtygbwwwerr/article/details/50548311

Neural Turing Machines

http://www.robots.ox.ac.uk/~tvg/publications/talks/NeuralTuringMachines.pdf

Neural Turing Machines

http://blog.csdn.net/malefactor/article/details/50550211

自然语言处理中的Attention Model

https://mp.weixin.qq.com/s/ZBaBtnQR7e39jZsY_JOqfw

自然语言处理中注意力机制综述

https://yq.aliyun.com/articles/65356

图文结合详解深度学习Memory & Attention

http://www.cosmosshadow.com/ml/%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C/2016/03/08/Attention.html

Attention

https://mp.weixin.qq.com/s/b5d_jNNDB_a_VFdT3J3Vog

通俗易懂理解Attention机制

https://mp.weixin.qq.com/s/eY2XJipUIKHIlrYVOhiMRQ

什么是自注意力机制？

https://mp.weixin.qq.com/s/azce34Q3N4hnhIlE3NVTVw

注意力机制(Attention)最新综述论文及相关源码

http://geek.csdn.net/news/detail/50558

深度学习和自然语言处理中的attention和memory机制

https://mp.weixin.qq.com/s/rKvh9fqfVf_EYBxa5QRDOw

不得不了解的五种Attention模型方法及其应用

https://mp.weixin.qq.com/s/XrlveG0kwij2qNL45TZdBg

Attention的另类用法

https://zhuanlan.zhihu.com/p/31547842

深度学习中Attention Mechanism详细介绍：原理、分类及应用

https://zhuanlan.zhihu.com/p/32089282

Attention学习笔记

https://mp.weixin.qq.com/s/LQ7uv0-AakkHE5b17yemqw

Awni Hannun：序列模型Attention Model中的问题与挑战

https://mp.weixin.qq.com/s/xr_1ZYbvADMMwgxLEAflCw

如何在语言翻译中理解Attention Mechanism？

https://mp.weixin.qq.com/s/Nyq_36aFmQYRWdpgbgxpuA

将注意力机制引入RNN，解决5大应用领域的序列预测问题

https://mp.weixin.qq.com/s/g2PcmsDW9ixUCh_yP8W-Vg

各类Seq2Seq模型对比及《Attention Is All You Need》中技术详解

https://mp.weixin.qq.com/s/FtI94xY6a8TEvFCHfjMnmA

小组讨论谷歌机器翻译Attention is All You Need

https://mp.weixin.qq.com/s/SqIMkiP1IZMGWzwZWGOI7w

谈谈神经网络的注意机制和使用方法

https://mp.weixin.qq.com/s/EMCZHuvk5dOV_Rz00GkJMA

近年火爆的Attention模型，它的套路这里都有！

https://zhuanlan.zhihu.com/p/27464080

从《Convolutional Sequence to Sequence Learning》到《Attention Is All You Need》

http://www.cnblogs.com/robert-dlut/p/8638283.html

自然语言处理中的自注意力机制！

https://mp.weixin.qq.com/s/sAYOXEjAdA91x3nliHNX8w

Attention模型方法综述

https://mp.weixin.qq.com/s/MZ8qSQzXqZQPQa97BKitHA

深入理解注意力机制

https://mp.weixin.qq.com/s/s8sKoTzqyf-_-N0TSLnPow

不用看数学公式！图解谷歌神经机器翻译核心部分：注意力机制

https://mp.weixin.qq.com/s/TM5poGwSGi5C9szO13GYxg

一文解读NLP中的注意力机制

https://zhuanlan.zhihu.com/p/59698165

NLP中的Attention机制

https://mp.weixin.qq.com/s/JVkhX_v2fCaICawk-P-fzw

通俗易懂：8大步骤图解注意力机制

https://juejin.im/post/5e57d69b6fb9a07c8a5a1aa2

啥是Attention?

# Transformer

之前的文章已经介绍了Attention和《Attention is All You Need》。但实际上，《Attention is All You Need》不仅提出了两种Attention模块，而且还提出了如下图所示的Transformer模型。该模型主要用于NMT领域，由于Attention不依赖上一刻的数据，同时精度也不弱于LSTM，因此有很好并行计算特性，在工业界得到了广泛应用。阿里巴巴和搜狗目前的NMT方案都是基于Transformer模型的。

代码：

https://github.com/Kyubyong/transformer

![](/images/img2/Transformer.png)

上图中的Feed Forward的公式为：

$$FFN(x) = \max(0,xW_1 + b_1)W_2 + b_2$$

Transformer的讲解首推：

http://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/

Visualizing A Neural Machine Translation Model (Mechanics of Seq2seq Models With Attention)

http://jalammar.github.io/illustrated-transformer/

The Illustrated Transformer

这里仅对要点做一个总结：

1.Transformer是一个标准的Seq2seq结构，有encoder和decoder两部分。

2.encoder可以并行执行，一次性算完。而decoder的输入不仅包含encoder的输出，还包含了decoder上次的输出，因此还是一个循环结构，并不能完全并行。

3.为了解决循环结构的次序问题，论文提出了上图所示的Masked Multi-Head Attention。

## Reformer

https://mp.weixin.qq.com/s/9P-hF9VzDMgoPZ_CGREn8w

Reformer：一个高效的Transformer

https://mp.weixin.qq.com/s/-45YL1mzPmSOESfWlxUclA

图解Reformer：一种高效的Transformer

https://mp.weixin.qq.com/s/wJbbki3S8l10rSpFTWFpgg

对Reformer的深入解读

## 参考

https://zhuanlan.zhihu.com/p/39034683

Attention is all you need模型笔记

https://zhuanlan.zhihu.com/p/40920384

真正的完全图解Seq2Seq Attention模型

https://mp.weixin.qq.com/s/RLxWevVWHXgX-UcoxDS70w

细讲《Attention Is All You Need》

https://mp.weixin.qq.com/s/1wReNLTtpKySPwi5u1iXMA

All Attention You Need

https://mp.weixin.qq.com/s/lUqpCae3TPkZlgT7gUatpg

Self-Attention与Transformer
