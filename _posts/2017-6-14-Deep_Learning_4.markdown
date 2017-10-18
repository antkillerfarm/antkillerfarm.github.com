---
layout: post
title:  深度学习（四）——RNN
category: theory 
---

# 词向量（续）

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

## word2vec/doc2vec的缺点

1.word2vec/doc2vec基于BOW（Bag Of Word，词袋）模型。该模型的特点是忽略词序，因此对于那些交换词序会改变含义的句子，无法准确评估它们的区别。

2.虽然我们一般使用word2vec/doc2vec来比较文本相似度，但是从原理来说，word2vec/doc2vec提供的是关联性（relatedness），而不是相似性（similarity）。这会带来以下问题：不但近义词的词向量相似，反义词的词向量也相似。因为它们和其他词的关系（也就是语境）是类似的。

3.由于一个词只有一个向量来表示，因此，无法处理一词多义的情况。

然而关联性并非都是坏事，有的时候也会起到意想不到的效果。比如在客服对话的案例中，客户可能会提供自己的收货地址，显然每个客户的地址都是不同的，但是有意思的是，这些地址的词向量是非常相似的。

总之，**只利用无标注数据训练得到的Word Embedding在匹配度计算的实用效果上和主题模型技术相差不大，它们本质上都是基于共现信息的训练。**

参考：

https://www.zhihu.com/question/22266868

Word2Vec如何解决多义词的问题？

## All is Embedding

向量化是机器学习处理非数值数据的必经之路。因此除了词向量之外，还有其他的Embedding。比如Network Embedding。

https://mp.weixin.qq.com/s/wcFlZPbB5dl6C87kdfjmKw

NE(Network Embedding)论文小览

https://mp.weixin.qq.com/s/zTNX_LeVMeHhJG7kPewn2g

除了自然语言处理，你还可以用Word2Vec做什么？

## 参考

http://www.cnblogs.com/iloveai/p/word2vec.html

word2vec前世今生

http://www.cnblogs.com/maybe2030/p/5427148.html

文本深度表示模型——word2vec&doc2vec词向量模型

https://www.zhihu.com/question/29978268

如何用word2vec计算两个句子之间的相似度？

https://mp.weixin.qq.com/s/kGi-Hf7CX6OKcCMe7IC7zA

NLP之Wrod2Vec三部曲

https://mp.weixin.qq.com/s/VMK_UpOUI0y7apTGR02D1Q

图解Word2Vec

https://mp.weixin.qq.com/s/XMGZXAt_LUX5iTet7_SX4Q

深度学习和自然语言处理：诠释词向量的魅力

https://mp.weixin.qq.com/s/Rn_aJYozQ0f53Mjq4MKSwA

哪种词向量模型更胜一筹？Word2Vec，WordRank or FastText?

https://mp.weixin.qq.com/s/H7m7lqWpK27pJp9obXxlIQ

见微知著，从细节处提升词向量的表示能力

http://licstar.net/archives/328

词向量和语言模型

https://mp.weixin.qq.com/s/eq1I92rjIAWEpYw-1fEHeQ

从Facebook AI Research开源fastText谈起文本分类：词向量模性、深度表征和全连接

https://mp.weixin.qq.com/s/GYTxN5X7MnSQ4k5bD2l-PQ

Salesforce的爱因斯坦AI最新NLP研究，通过情境化词向量从翻译中学习!

https://mp.weixin.qq.com/s/GUUkXrB1iyg4rQbBtICq6A

通过NMT训练的通用语境词向量：NLP中的预训练模型？

http://geek.csdn.net/news/detail/135736

漫谈词向量之基于Softmax与Sampling的方法

# RNN

## RNN的基本结构

RNN是Recurrent Neural Network和Recursive Neural Network的简称。前者主要用于处理和时序相关的输入，而后者目前已经没落。本文只讨论前者。

![](/images/article/RNN.png)

上图是RNN的结构图。其中，展开箭头左边是RNN的静态结构图。不同于之前的神经网络表示，这里的圆形不是单个神经元，而是一层神经元。权值也不是单个权值，而是权值向量。

从静态结构图可以看出RNN实际上和3层MLP的结构，是基本类似的。差别在于RNN的隐藏层多了一个指向自己的环状结构。

上图的展开箭头右边是RNN的时序展开结构图。从纵向来看，它只是一个3层的浅层神经网络，然而从横向来看，它却是一个深层的神经网络。可见神经网络深浅与否，不仅和模型本身的层数有关，也与神经元之间的连接方式密切相关。

虽然理论上，我们可以给每一时刻赋予不同的$$U,V,W$$，然而出于简化计算和稀疏度的考量，RNN所有时刻的$$U,V,W$$都是相同的。

RNN的误差反向传播算法，被称作**Backpropagation Through Time**。其主要公式如下：

$$\nabla U=\frac{\partial E}{\partial U}=\sum_t\frac{\partial e_t}{\partial U} \\\nabla V=\frac{\partial E}{\partial V}=\sum_t\frac{\partial e_t}{\partial V} \\\nabla W=\frac{\partial E}{\partial W}=\sum_t\frac{\partial e_t}{\partial W}$$

从上式可以看出，三个误差梯度实际上都是**时域的积分**。

正因为RNN的状态和过去、现在都有关系，因此，RNN也被看作是一种拥有“记忆性”的神经网络。

## RNN的训练困难

理论上，RNN可以支持无限长的时间序列，然而实际情况却没这么简单。

Yoshua Bengio在论文《On the difficulty of training recurrent neural networks》（http://proceedings.mlr.press/v28/pascanu13.pdf）中，给出了如下公式：

$$||\prod_{k<i\le t} \frac{\partial h_{i}}{\partial h_{i-1}}|| \le \eta^{t-k}$$

并指出当$$\eta < 1$$时，RNN会Gradient Vanish，而当$$\eta > 1$$时，RNN会Gradient Explode。

这里显然不考虑$$\eta > 1$$的情况，因为Gradient Explode，直接会导致训练无法收敛，从而没有实用价值。

因此有实用价值的，只剩下$$\eta < 1$$了，但是Gradient Vanish又注定了RNN所谓的“记忆性”维持不了多久，一般也就5～7层左右。

上述内容只是一般性的讨论，实际训练还是有很多trick的。

比如，针对$$\eta > 1$$的情况，可以采用Gradient Clipping技术，通过设置梯度的上限，来避免Gradient Explode。

还可使用正交初始化技术，在训练之初就将$$\eta$$调整到1附近。

## RNN的历史

上面研究的RNN结构，又被称为Elman RNN。最早是Jeffrey Elman于1990年发明的。

$$\begin{align}
h_t &= \sigma_h(W_{h} x_t + U_{h} h_{t-1} + b_h) \\
y_t &= \sigma_y(W_{y} h_t + b_y)
\end{align}$$

论文：

《Finding Structure in Time》

>Jeffrey Locke Elman，1948年生，Harvard College本科（1969年）+University of Texas博士（1977年）。University of California, San Diego教授，American Academy of Arts and Sciences院士（2015年）。 美国心理学会会员。  
>个人主页：   
>https://tatar.ucsd.edu/jeffelman/

>Harvard College是Harvard University最古老的本部，目前一般提供本科教育。它和其他许多研究生院以及相关部门，共同组成了Harvard University。类似的还有Yale College和Yale University。

>American Academy of Arts and Sciences建于1780年。当时，美国正在法国等国的协助下与英国作战，所以美国的创立者选择比照包括作家、人文学者、科学家、军事家、政治家在内的法兰西学术院，建立新大陆的学术院。   
>后来，林肯总统比照英国皇家学会，于1863年创建了主要涵盖自然科学的National Academy of Sciences，United States。   
>这两个学院是美国学术界最权威的组织。

>美国的创立者，一般被翻译为Founding Fathers of the United States。此外还有一个更响亮的称号76ers。没错，就是NBA那支球队的名字。

除了Elman RNN之外，还有Jordan RNN。（没错，吴恩达的导师的作品）

$$\begin{align}
h_t &= \sigma_h(W_{h} x_t + U_{h} y_{t-1} + b_h) \\
y_t &= \sigma_y(W_{y} h_t + b_y)
\end{align}$$

Elman RNN的记忆来自于隐层单元，而Jordan RNN的记忆来自于输出层单元。

## 参考

http://blog.csdn.net/aws3217150/article/details/50768453

递归神经网络(RNN)简介

http://blog.csdn.net/heyongluoyao8/article/details/48636251

循环神经网络(RNN, Recurrent Neural Networks)介绍

http://mp.weixin.qq.com/s?__biz=MzIzODExMDE5MA==&mid=2694182661&idx=1&sn=ddfb3f301f5021571992824b21ddcafe

循环神经网络

http://www.wildml.com/2015/10/recurrent-neural-networks-tutorial-part-3-backpropagation-through-time-and-vanishing-gradients/

Backpropagation Through Time算法

https://baijia.baidu.com/s?old_id=560025

Tomas Mikolov详解RNN与机器智能的实现

https://sanwen8.cn/p/3f8sRTh.html

为什么RNN需要做正交初始化？

http://blog.csdn.net/shenxiaolu1984/article/details/71508892

RNN的梯度消失/爆炸与正交初始化

https://mp.weixin.qq.com/s/vHQ1WbADHAISXCGxOqnP2A

看大牛如何复盘递归神经网络！

https://mp.weixin.qq.com/s/0V9DeG39is_BxAYX0Yomww

为何循环神经网络在众多机器学习方法中脱颖而出？

https://mp.weixin.qq.com/s/-Am9Z4_SsOc-fZA_54Qg3A

深度理解RNN：时间序列数据的首选神经网络！

https://mp.weixin.qq.com/s/ztIrt4_xIPrmCwS1fCn_dA

“魔性”的循环神经网络

https://mp.weixin.qq.com/s/tIXJNkT9gIjGYZz7dekiNw

手把手教你写一个RNN

https://mp.weixin.qq.com/s/BqVicouktsZu8xLVR-XnFg

完全图解RNN、RNN变体、Seq2Seq、Attention机制

https://mp.weixin.qq.com/s/gGGXKT2fTn2xPPvo7PE8IA

像训练CNN一样快速训练RNN：全新RNN实现，比优化后的LSTM快10倍

https://mp.weixin.qq.com/s/0TLaC8ACXAFEK5aMNK9O-Q

简单循环单元SRU：像CNN一样快速训练RNN

