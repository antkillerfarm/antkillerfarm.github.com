---
layout: post
title:  Attention（二）——花式Attention
category: Attention 
---

* toc
{:toc}

# 花式Attention（续）

## Multi-Head Attention

![](/images/img2/Attention_6.png)

这个是Google提出的新概念，是Attention机制的完善。不过从形式上看，它其实就再简单不过了，就是把Q,K,V通过参数矩阵映射一下，然后再做Attention，把这个过程重复做h次，结果拼接起来就行了，可谓“大道至简”了。具体来说：

$$head_i = Attention(\boldsymbol{Q}\boldsymbol{W}_i^Q,\boldsymbol{K}\boldsymbol{W}_i^K,\boldsymbol{V}\boldsymbol{W}_i^V)$$

所谓“多头”（Multi-Head），就是指多做几次同样的事情（参数不共享），然后把结果拼接。

https://www.zhihu.com/question/446385446

BERT中，multi-head 768 * 64 * 12与直接使用768 * 768矩阵统一计算，有什么区别？

## Self Attention

到目前为止，对Attention层的描述都是一般化的，我们可以落实一些应用。比如，如果做阅读理解的话，Q可以是篇章的词向量序列，取K=V为问题的词向量序列，那么输出就是所谓的Aligned Question Embedding。

而在Google的论文中，大部分的Attention都是Self Attention，即“自注意力”，或者叫内部注意力。

所谓Self Attention，其实就是Attention(X,X,X)，X就是前面说的输入序列。也就是说，在序列内部做Attention，寻找序列内部的联系。

下表展示了Self Attention相对于其他运算的计算量分析：

| Layer Type | Complexity per Layer | Sequential Operations | Maximum Path Length |
|:--:|:--:|:--:|:--:|
| Self-Attention | $$O(n^2 \cdot d)$$ | $$O(1)$$ | $$O(1)$$ |
| Recurrent | $$O(n \cdot d^2)$$ | $$O(n)$$ | $$O(n)$$ |
| Convolutional | $$O(k \cdot n \cdot d^2)$$ | $$O(1)$$ | $$O(\log_k(n))$$ |
| Self-Attention (restricted) | $$O(r \cdot n \cdot d)$$ | $$O(1)$$ | $$O(n/r)$$ |

其中，n表示序列长度，d表示词向量的维度，k表示卷积核的大小，r表示restricted self-attention中的neighborhood的数量。

可以看出，在n小于d的情况下，Self Attention是有计算量的优势的。

所谓的restricted self-attention是指：假设当前词只与前后r个词发生联系，因此注意力也只发生在这2r+1个词之间。

可以看到self-attention和convolution有点儿神似：

- 它摒弃了CNN的局部假设，改为寻找长距离的关联依赖。

- 它没有了递归的限制，就像CNN一样可以在每一层内实现并行。

- self-attention借鉴CNN中multi-kernel的思想，进一步进化成为Multi-Head attention。每一个不同的head使用不同的线性变换，学习不同的relationship。

---

![](/images/img3/self_attention.jpg)

- 一维卷积的感受野是有限的，注意力机制的感受野是无限的（全局的）。

- 一维卷积的连接强度（权重）是与输入无关的，注意力机制的连接强度是与输入相关的。

参考：

https://www.zhihu.com/question/288081659

attention跟一维卷积的区别是啥？

## Position Embedding

然而，只要稍微思考一下就会发现，这样的模型并不能捕捉序列的顺序！换句话说，如果将K,V按行打乱顺序（相当于句子中的词序打乱），那么Attention的结果还是一样的。这就表明了，到目前为止，Attention模型顶多是一个非常精妙的“词袋模型”而已。

这问题就比较严重了，大家知道，对于时间序列来说，尤其是对于NLP中的任务来说，顺序是很重要的信息，它代表着局部甚至是全局的结构，学习不到顺序信息，那么效果将会大打折扣（比如机器翻译中，有可能只把每个词都翻译出来了，但是不能组织成合理的句子）。

于是Google再祭出了一招——Position Embedding，也就是“位置向量”，将每个位置编号，然后每个编号对应一个向量，通过结合位置向量和词向量，就给每个词都引入了一定的位置信息，**这样Attention就可以分辨出不同位置的词了**。

Position Embedding的公式为：

$$\left\{\begin{aligned}&PE_{2i}(p)=\sin\Big(p/10000^{2i/{d_{pos}}}\Big)\\ 
&PE_{2i+1}(p)=\cos\Big(p/10000^{2i/{d_{pos}}}\Big) 
\end{aligned}\right.$$

其中，pos表示位置，i表示位置向量的下标。

由于我们有$$\sin(\alpha+\beta)=\sin\alpha\cos\beta+\cos\alpha\sin\beta$$以及$$\cos(\alpha+\beta)=\cos\alpha\cos\beta-\sin\alpha\sin\beta$$，这表明位置p+k的向量可以表示成位置p的向量的线性变换，这提供了表达相对位置信息的可能性。

在实际的实现中，由于幂函数的指数比较小，数值稳定性较差，常采用指数+对数的方式进行计算。

参考：

https://zhuanlan.zhihu.com/p/92017824

浅谈Transformer-based模型中的位置表示

https://mp.weixin.qq.com/s/ENpXBYQ4hfdTLSXBIoF00Q

如何优雅地编码文本中的位置信息？三种positioanl encoding方法简述

## Hard Attention

论文：

《Show, Attend and Tell: Neural Image Caption Generation with Visual Attention》

我们之前所描述的传统的Attention Mechanism是Soft Attention。Soft Attention是参数化的（Parameterization），因此可导，可以被嵌入到模型中去，直接训练。梯度可以经过Attention Mechanism模块，反向传播到模型其他部分。

相反，Hard Attention是一个随机的过程。Hard Attention不会选择整个encoder的输出做为其输入，Hard Attention会依概率$$S_i$$来采样输入端的隐状态一部分来进行计算，而不是整个encoder的隐状态。为了实现梯度的反向传播，需要采用蒙特卡洛采样的方法来估计模块的梯度。

两种Attention Mechanism都有各自的优势，但目前更多的研究和应用还是更倾向于使用Soft Attention，因为其可以直接求导，进行梯度反向传播。

## Local Attention

论文：

《Effective Approaches to Attention-based Neural Machine Translation》

>Thang Luong，越南人，Stanford博士（2016），现为Google研究员。导师是Christopher Manning。
>个人主页：   
>https://nlp.stanford.edu/~lmthang/

>Christopher Manning，澳大利亚人，Stanford博士（1994），现为Stanford教授。从事NLP近三十年，率先将统计方法引入NLP。

![](/images/img2/Global_attention.png)

上图中蓝色表示输入的词向量，红色表示输出的词向量。

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

https://mp.weixin.qq.com/s/PF02OwP0CHDf6l4BHHDqow

一文读懂Attention机制
