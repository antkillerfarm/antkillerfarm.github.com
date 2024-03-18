---
layout: post
title:  Attention（四）——BERT, ELMo, GPT, ERNIE
category: Attention 
---

* toc
{:toc}

# 预训练语言模型进化史（续）

https://mp.weixin.qq.com/s/RKA_RxTQkIeJX3_VIKJiRQ

周明：预训练模型在多语言、多模态任务的进展

https://mp.weixin.qq.com/s/3fmZs1sFNW4IGmtql4KsVQ

邱锡鹏：自然语言处理中的预训练模型，90页ppt

https://mp.weixin.qq.com/s/irb_-T1T9sthW888hP6L4w

清华、复旦、人大联合推出43页预训练模型综述

## 预训练语言模型的前世今生

https://mp.weixin.qq.com/s/1ixYjJN-bJPGrr7v-4d7Rw

萌芽时代

https://mp.weixin.qq.com/s/g4jEVU3BkRem-DYXCn5eFQ

风起云涌

https://mp.weixin.qq.com/s/U8f0cXoPrN32PM3944Oqkg

十分钟了解文本分类通用训练技巧

https://mp.weixin.qq.com/s/uAJ_05g0Zo33mgygTnow1Q

银色独角兽GPT家族

https://mp.weixin.qq.com/s/2_MXIEk5-JP5KwsV6al9XQ

BERT，开启NLP新时代的王者

https://mp.weixin.qq.com/s/sMocYFvESXoBGtX_NWmQkQ

百度出品ERNIE合集，问国产预训练语言模型哪家强

https://mp.weixin.qq.com/s/uOGNoePkwfeixTtI4q4t8Q

MT-DNN(KD) : 预训练、多任务、知识蒸馏的结合

https://mp.weixin.qq.com/s/MXZ3ygSqwyXqOH1PrWEGqg

Transformer-XL超长上下文注意力模型

https://mp.weixin.qq.com/s/6XX2tkp2dbIEKqumnIQWbg

跨语种语言模型

https://mp.weixin.qq.com/s/U1O3j4FBRdiwlRjjlzbWJQ

XLNet：公平一战！多项任务效果超越BERT

https://mp.weixin.qq.com/s/f6RwSHz3Nc68oipHdYDVTw

RoBERTa: 捍卫BERT的尊严

https://mp.weixin.qq.com/s/fneyUitoQL6ZqX3xU9WpDg

跨模态语言模型

https://mp.weixin.qq.com/s/vgEKI9HjWDpkeSZQT2d8Qg

ENRIE(Tsinghua)：知识图谱与BERT相结合，为语言模型赋能助力

https://mp.weixin.qq.com/s/hLt2SnVovrLeNpuMUH1OSg

预训练模型的技术演进：乘风破浪的PTM

# BERT

## 概述

论文：

《BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding》

代码：

https://github.com/google-research/bert

BERT算的上是Google暴力美学的新作了（2018.10）。如果用家用显卡GTX 1080Ti的话，大概需要几个月的训练时间。幸好Google已经提供了预训练的模型：

https://github.com/google-research/bert/blob/master/multilingual.md

这里有一个使用预训练模型的参考代码：

https://github.com/macanv/BERT-BiLSMT-CRF-NER

这里有一个可视化工具：

https://github.com/jessevig/bertviz

Tool for visualizing attention in the Transformer model(BERT, GPT-2, XLNet, and RoBERTa)

https://github.com/thunlp/PLMpapers

预训练语言模型关系图+必读论文列表，清华荣誉出品

老规矩，最佳教程还是推荐Jay Alammar的：

http://jalammar.github.io/illustrated-bert/

图解BERT

https://jalammar.github.io/a-visual-guide-to-using-bert-for-the-first-time/

A Visual Guide to Using BERT for the First Time

## 基本结构

![](/images/img3/BERT.png)

上图是BERT的网络结构图。

BERT是Bidirectional Encoder Representations from Transformers的缩写。从这个名字可以看出它将Transformer中的encoder作为一个基本单元，然后采用了类似双向RNN的方式，做了一个双向的Transformer的结构。

![](/images/img5/self-attention-and-masked-self-attention.png)

无论双向，还是单向，其实结构都是一样的，差别仅仅在于有无Mask遮住后面的内容而已。

## pre-training

BERT的强大，主要不在网络结构上。上面提到的GPT 1.0虽然输给了BERT，但网络更深、向量维度也更大的GPT 2.0却赢了BERT，可见单向或者双向的Transformer，并不是问题的关键。让这些模型真正强大的原因主要在于pre-training。

![](/images/img3/BERT_2.png)

上图是BERT的pre-training和fine-tuning的结构图。

所谓的pre-training其实就是海量文本的无监督学习。

如何进行无监督学习呢？

BERT主要用了两个任务：

- Masked Language Model，MLM。随机盖住一句话的某个词，让NN去预测这个被盖住的词。

- Next Sentence Prediction，NSP。预测下一段话。

这两个任务，算是NLP的老任务了。但在传统的NLP pipeline中，属于非常下游的任务。BERT利用它们的特点，进行无监督学习，算是一个很大的突破了。

## 海量文本

BERT以及后来的GPT 2.0取得重大突破的关键，还在于海量的训练文本。

BERT拥有3.3亿个参数，训练数据包括：BooksCorpus（800M words）和English Wikipedia（2500M words）

GPT 2.0拥有15亿个参数，训练数据除了上述之外，还包括了800M个网页的文本。

如此海量的参数和数据，注定了这些模型的训练是一个超费算力的过程。NLP的游戏规则将变成：

- 土豪大科技公司靠暴力上数据规模，上GPU或者TPU集群，训练好预训练模型发布出来，不断刷出大新闻。通过暴力美学横扫一切，这是土豪端的玩法。

- 而对于大多数人来说，你能做的是在别人放出来的预训练模型上，做小修正，或者刷应用，或者刷各种榜单，逐步走向了应用人员的方向，这是大多数NLP从业者，未来几年要面对的dilemma。

## fine-tuning

fine-tuning是针对实际业务数据进行的微调。下图展示了在若干任务中进行fine-tuning的网络设计。

![](/images/img3/BERT_3.png)

>其他NLP任务的网络设计，可以参看后面《GPT》一节的配图。

值得一提的是，GPT 2.0的实践表明：海量的无监督训练已经能达到很好的效果，fine-tuning只是锦上添花而已。

事实上，在CV领域也可以看到，通过海量图片训练出的预训练网络，比随机初始化有效率多了。

## input embeddings

BERT对于input embeddings也做了改进。（如下图所示）

![](/images/img3/BERT_4.png)

其中，Segment Embeddings用于区分输入中的不同句子。这一方案的使用，使得输入文本不再局限于一句话之内，从而大大增加了输入文本的长度，对于获得文本的全局信息，很有好处。

## NMT

BERT的论文并未提到执行NMT任务时的网络结构，但从下面的论文：

《Incorporating BERT into Neural Machine Translation》

可以看出NMT的网络结构仍然是和Transformer类似的seq2seq结构：

![](/images/img3/BERT_5.png)

也就是说，仍然有decoder部分，仍然不能完全并行。

# ELMo

https://mp.weixin.qq.com/s/I315hYPrxV0YYryqsUysXw

NLP的游戏规则从此改写？从word2vec, ELMo到BERT

https://mp.weixin.qq.com/s/VL09dIQE6kAUzj5-CRFoXA

ELMo的朋友圈：预训练语言模型真的一枝独秀吗？

https://mp.weixin.qq.com/s/1nZV6wXzeIxIAAovFOageA

手把手教你用ELMo模型提取文本特征

https://mp.weixin.qq.com/s/S3dISq03PDXPvFsL4xjzNQ

通俗理解ELMo

https://zhuanlan.zhihu.com/p/37684922

ELMo

https://mp.weixin.qq.com/s/i7EJSNzDsNNbK2YA_YNu8g

词向量与ELMo模型

https://mp.weixin.qq.com/s/qbXZGiKYEuTI-2l4iYZlbQ

图文并茂带你细致了解ELMo的各种细节

# GPT

GPT-1论文：

《Improving Language Understandingby Generative Pre-Training》

GPT-2论文：

《Language Models are Unsupervised Multitask Learners》

![](/images/img3/GPT.png)

Jay Alammar的教程：

https://jalammar.github.io/illustrated-gpt2/



参考：

https://mp.weixin.qq.com/s/E7FLbXYvE9irSpJ9Cdx5tg

GLUE排行榜上全面超越BERT的模型近日公布了

https://mp.weixin.qq.com/s/ZitIqX-9MNk6L1mAC_AwBQ

OpenAI发布参数量高达15亿的通用语言模型GPT-2

https://mp.weixin.qq.com/s/7u_W4LTYqQBmz3geux5QNQ

对标Bert？刷屏的GPT 2.0意味着什么

https://mp.weixin.qq.com/s/1GIQGBwciP22CZvFxhmLzA

如何构建OpenAI的GPT 2：“太危险而无法释放的人工智能”

https://mp.weixin.qq.com/s/eJn379q9raDHY9FdWaXeKQ

AI界最危险武器GPT-2使用指南：从Finetune到部署

https://mp.weixin.qq.com/s/WDFwKqNynwPtXhM8rZnOsA

自动生成马斯克的推特几乎无破绽！MIT用GPT-2模型做了个名人发言模仿器

https://mp.weixin.qq.com/s/67Z_dslvwTyRl3OMrArhCg

完全图解GPT-2（一）

https://mp.weixin.qq.com/s/xk5fWrSBKErH8tvl-3pgtg

完全图解GPT-2（二）

https://zhuanlan.zhihu.com/p/80215294

GPT：第一个引入Transformer的预训练模型

https://mp.weixin.qq.com/s/dibf3bU4hQ7nXTPGMFsKbg

GPT-3王者来袭！1750亿参数少样本无需微调，网友：“调参侠”都没的当了

https://mp.weixin.qq.com/s/aPA0PEqVn509u3xbgmhIwQ

一天star量破千，300行代码，特斯拉AI总监Karpathy写了个GPT的Pytorch训练库

https://mp.weixin.qq.com/s/FOCR-9X5LVtjxVMWoAtw4g

GPT的野望

# ERNIE

ERNIE是百度2019年提出的。

论文：

《ERNIE: Enhanced Representation through Knowledge Integration》

《ERNIE 2.0: A continual pre-training framework for language understanding》

代码：

https://github.com/PaddlePaddle/ERNIE/

除此之外，清华也有一篇叫ERNIE的论文：

《ERNIE: Enhanced Language Representation with Informative Entities》

这几篇论文主要讨论了，如何将语义信息融入BERT。篇幅原因，这里只关注百度的两篇论文的做法。

![](/images/img3/ERNIE.png)

相较于BERT学习原始语言信号，ERNIE模型通过对词、实体等语义单元的掩码，使得模型学习完整概念的语义表示。上例中，BERT通过“哈”与“滨”的局部共现，即可判断出“尔”字，但它没有学习到与“哈尔滨”相关的知识。而ERNIE通过学习词与实体的表达，使模型能够建模出“哈尔滨”与“黑龙江”的关系，学到“哈尔滨”是“黑龙江”的省会以及“哈尔滨”是个冰雪城市。

![](/images/img3/ERNIE_2.png)

为了学习相关语义，ERNIE提出了如上图所示的不同级别的mask方法。

![](/images/img3/ERNIE_3.png)

还有如上图所示的语义嵌入。

BERT已经证明了预训练模型对于多种NLP任务的有效性，因此使用多任务学习就成为一个很自然的想法。

![](/images/img3/ERNIE_4.png)

上图是ERNIE 2.0的多任务训练的结构图。可以认为ERNIE 2.0就是多任务版的ERNIE。

![](/images/img3/ERNIE_5.png)

为了适应多任务版本的要求，ERNIE 2.0还提出了Task Embedding。

参考：

https://mp.weixin.qq.com/s/xoQhz6ljbsbzKRBJlTQQuQ

百度提出ERNIE，多项中文NLP任务表现出色

https://mp.weixin.qq.com/s/rQ8ISipvV3Irrjd3MI-Idw

百度ERNIE，中文任务全面超越BERT

https://mp.weixin.qq.com/s/_ZBvq7gXvbiP2IQve9tcKg

清华等提出ERNIE：知识图谱结合BERT才是“有文化”的语言模型

https://mp.weixin.qq.com/s/QVEYQfEQV0CsklI9S4vOiA

ERNIE真有官方说的那么好？亲测告诉你答案！
