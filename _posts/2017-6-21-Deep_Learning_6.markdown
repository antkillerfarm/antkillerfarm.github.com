---
layout: post
title:  深度学习（六）——RNN
category: DL 
---

* toc
{:toc}

# 词向量

## FastText

Word2Vec作者Mikolov加盟Facebook之后，提出了文本分类新作FastText。

FastText模型架构和Word2Vec中的CBOW模型很类似。不同之处在于，FastText预测标签，而CBOW模型预测中间词。

Github：

https://github.com/facebookresearch/fastText

FastText的论文其实有两篇：

《Bag of Tricks for Efficient Text Classification》

这篇就是上述文本分类的论文。

《Enriching Word Vectors with Subword Information》

这篇是根据词根改进词向量的论文。因此，有的blog说，使用FastText生成词向量，实际上就是指的这篇文章。

参考：

http://www.algorithmdog.com/fast-fasttext

超快的 fastText

https://mp.weixin.qq.com/s/tXgF7rQdZm3IFAluMU5ywg

fastText之源码分析

https://mp.weixin.qq.com/s/S4RGicXwZqis6vQFpB31qQ

Bag of Tricks for Efficient Text Classification

https://mp.weixin.qq.com/s/eq1I92rjIAWEpYw-1fEHeQ

从Facebook AI Research开源fastText谈起文本分类：词向量模性、深度表征和全连接

https://mp.weixin.qq.com/s/aq_kWkwgwtz5qFo0lNEEqg

Tomas Mikolov论文简评：从Word2Vec到FastText

https://mp.weixin.qq.com/s/v1-mLhmbp5MoRR824tdPDw

玩转词向量：用fastText预训练向量做个智能小程序

https://mp.weixin.qq.com/s/LLrq1F2uEC2xEWZrd9uijA

一行代码自动调参，支持模型压缩指定大小，Facebook升级FastText

https://mp.weixin.qq.com/s/VxODwO8qA33Cr1n62YXYBQ

fastText：极快的文本分类工具

https://mp.weixin.qq.com/s/TRrL6_nI4GimH_OJ1CswiQ

NLP重铸篇之Fasttext

## RNNLM

RNNLM是Mikolov早期提出的文本分类的工具。（其实就是他的博士毕业论文）

官网：

http://rnnlm.org/

yandex后来又提出了一个加速版本的RNNLM：

https://github.com/yandex/faster-rnnlm

## Item2Vec

本质上，word2vec模型是在word-context的co-occurrence矩阵基础上建立起来的。因此，任何基于co-occurrence矩阵的算法模型，都可以套用word2vec算法的思路加以改进。

比如，推荐系统领域的协同过滤算法。

协同过滤算法是建立在一个user-item的co-occurrence矩阵的基础上，通过行向量或列向量的相似性进行推荐。如果我们将同一个user购买的item视为一个context，就可以建立一个item-context的矩阵。进一步的，可以在这个矩阵上借鉴CBoW模型或Skip-gram模型计算出item的向量表达，在更高阶上计算item间的相似度。

论文：

《Item2Vec: Neural Item Embedding for Collaborative Filtering》

在实际的新闻信息流推荐中，Word2Vec的点击效果比ALS要好30%+，主要有两个原因：

1. 用户的兴趣和行为是多样的，局部的行为往往更偏相关，往往整体的样本差异是很大的；

2. 在负样本采样中，ALS 是全局的负样本采样，Word2Vec 更倾向高频，倾向高频的采样更不容易让学习出的结果都与高频（头部）的结果相似。

参考：

https://mp.weixin.qq.com/s/vpxCP1Uw23y9XNTRUhY79w

达观数据推荐算法实现：协同过滤之item embedding

https://www.sohu.com/a/215535516_99992181

有这好事？神经网络模型Word2vec竟能根据个人喜好推荐音乐

https://mp.weixin.qq.com/s/Ta2Im4WCWq5eQ8SF-mNpuQ

万物皆Embedding，从经典的word2vec到深度学习基本操作item2vec

https://mp.weixin.qq.com/s/6XJuZBTmfRWWFwS9J3HOsQ

推荐技术随谈

## word2vec/doc2vec的缺点

1.word2vec/doc2vec基于BOW（Bag Of Word，词袋）模型。该模型的特点是忽略词序，因此对于那些交换词序会改变含义的句子，无法准确评估它们的区别。

2.虽然我们一般使用word2vec/doc2vec来比较文本相似度，但是从原理来说，word2vec/doc2vec提供的是关联性（relatedness），而不是相似性（similarity）。这会带来以下问题：不但近义词的词向量相似，反义词的词向量也相似。因为它们和其他词的关系（也就是语境）是类似的。

3.由于一个词只有一个向量来表示，因此，无法处理一词多义的情况。

然而关联性并非都是坏事，有的时候也会起到意想不到的效果。比如在客服对话的案例中，客户可能会提供自己的收货地址，显然每个客户的地址都是不同的，但是有意思的是，这些地址的词向量是非常相似的。

总之，**只利用无标注数据训练得到的Word Embedding在匹配度计算的实用效果上和主题模型技术相差不大，它们本质上都是基于共现信息的训练。**

4.除了余弦相似度之外，词向量的模长，也是一个重要的特征，词向量模长越大，重要性越低（或者越高，取决于生成词向量的算法）。

参考：

https://www.zhihu.com/question/22266868

Word2Vec如何解决多义词的问题？

## 参考

http://www.cnblogs.com/iloveai/p/word2vec.html

word2vec前世今生

https://mp.weixin.qq.com/s/NMngfR7EWk-pa6c4_FY9Yw

图解Word2vec，读这一篇就够了

http://www.cnblogs.com/maybe2030/p/5427148.html

文本深度表示模型——word2vec&doc2vec词向量模型

https://www.zhihu.com/question/29978268

如何用word2vec计算两个句子之间的相似度？

https://mp.weixin.qq.com/s/kGi-Hf7CX6OKcCMe7IC7zA

NLP之Wrod2Vec三部曲

https://mp.weixin.qq.com/s/VMK_UpOUI0y7apTGR02D1Q

图解Word2Vec

https://mp.weixin.qq.com/s/CMcNkEFW9UUXAW0832dT6g

对学习/理解Word2Vec有帮助的材料

https://mp.weixin.qq.com/s/XMGZXAt_LUX5iTet7_SX4Q

深度学习和自然语言处理：诠释词向量的魅力

https://mp.weixin.qq.com/s/Rn_aJYozQ0f53Mjq4MKSwA

哪种词向量模型更胜一筹？Word2Vec，WordRank or FastText?

https://mp.weixin.qq.com/s/H7m7lqWpK27pJp9obXxlIQ

见微知著，从细节处提升词向量的表示能力

http://licstar.net/archives/328

词向量和语言模型

http://geek.csdn.net/news/detail/135736

漫谈词向量之基于Softmax与Sampling的方法

https://mp.weixin.qq.com/s/nLFRJO2QEG_kAmeRYUdT3g

十分钟带你看遍词向量模型

https://zhuanlan.zhihu.com/p/30868040

文本表示的应用与评价

https://mp.weixin.qq.com/s/GOPIIlDBd3vXpgq-a5s2fQ

文本分类特征提取之Word2Vec

https://www.zhihu.com/question/339184168

为什么很多NLP的工作在使用预训练词向量时选择GloVe而不是Word2Vec或其他?

https://mp.weixin.qq.com/s/pOShNO2iOntcGSRMbR9uxg

Word2Vec与GloVe技术浅析与对比

https://mp.weixin.qq.com/s/dUadWioBqIEnG85hJFfBJQ

word2vec在工业界的应用场景

https://mp.weixin.qq.com/s/md3SL076cw0TgZDRlwWG5A

用数据玩点花样！如何构建skim-gram模型来训练和可视化词向量

https://mp.weixin.qq.com/s/HjNjTk_Hs82K87pP3QrNqw

不懂word2vec，还敢说自己是做NLP？

https://mp.weixin.qq.com/s/nHEyJLU18AE-SatW9HKeOw

Word2Vec——深度学习的一小步，自然语言处理的一大步

https://www.zhihu.com/answer/543419468

CNN文本分类中是否可以使用字向量代替词向量？

https://mp.weixin.qq.com/s/zDneR1BU6xvt8cndEF4_Xw

深入浅出Word2Vec原理解析

https://mp.weixin.qq.com/s/CQ9FdFcWuW0Ku3UtbGmmgg

Word Vectors

# RNN

## RNN的基本结构

RNN是Recurrent Neural Network和Recursive Neural Network的简称。前者主要用于处理和时序相关的输入，而后者目前已经没落。本文只讨论前者。

![](/images/article/RNN.png)

上图是RNN的结构图。其中，展开箭头左边是RNN的静态结构图。不同于之前的神经网络表示，这里的圆形不是单个神经元，而是一层神经元。权值也不是单个权值，而是权值向量。

从静态结构图可以看出RNN实际上和3层MLP的结构，是基本类似的。差别在于RNN的隐藏层多了一个指向自己的环状结构。

上图的展开箭头右边是RNN的时序展开结构图。从纵向来看，它只是一个3层的浅层神经网络，然而从横向来看，它却是一个深层的神经网络。可见神经网络深浅与否，不仅和模型本身的层数有关，也与神经元之间的连接方式密切相关。

虽然理论上，我们可以给每一时刻赋予不同的$$U,V,W$$，然而出于简化计算和稀疏度的考量，RNN所有时刻的$$U,V,W$$都是相同的。

RNN的误差反向传播算法，被称作**Backpropagation Through Time（BPTT）**。其主要公式如下：

$$
\begin{array}\\
\nabla U=\frac{\partial E}{\partial U}=\sum_t\frac{\partial e_t}{\partial U} \\
\nabla V=\frac{\partial E}{\partial V}=\sum_t\frac{\partial e_t}{\partial V} \\
\nabla W=\frac{\partial E}{\partial W}=\sum_t\frac{\partial e_t}{\partial W}
\end{array}
$$

从上式可以看出，三个误差梯度实际上都是**时域的积分**。

正因为RNN的状态和过去、现在都有关系，因此，RNN也被看作是一种拥有“记忆性”的神经网络。

## RNN的训练困难

理论上，RNN可以支持无限长的时间序列，然而实际情况却没这么简单。

Yoshua Bengio在论文《On the difficulty of training recurrent neural networks》（http://proceedings.mlr.press/v28/pascanu13.pdf）中，给出了如下公式：

$$\left\|\prod_{k<i\le t} \frac{\partial h_{i}}{\partial h_{i-1}}\right\| \le \eta^{t-k}$$

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
