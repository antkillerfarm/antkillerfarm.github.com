---
layout: post
title:  Attention（三）——BERT（1）
category: Attention 
---

# Transformer

## 参考（续）

https://mp.weixin.qq.com/s/O8_oOCtCCcU-kVklgxRoBg

Transformers Assemble（PART I）

https://mp.weixin.qq.com/s/zmChXVkF0pUiKrahx00EzQ

Transformers Assemble（PART II）

https://mp.weixin.qq.com/s/uPBLzJ-WidH4WBDiPt5NqA

Transformers Assemble（PART III）

https://mp.weixin.qq.com/s/8pNZVe66LmQ-G3gqs6tOxw

Transformers Assemble（PART IV）

https://mp.weixin.qq.com/s/i7oCZKb84w2jzuINAWWtiA

Transformers Assemble（PART V）

https://mp.weixin.qq.com/s/S_xhaDrOaPe38ZvDLWl4dg

从技术到产品，搜狗为我们解读了神经机器翻译的现状

https://mp.weixin.qq.com/s/vzjKU_0qhapWKOYZ4Rnj-Q

谷歌的机器翻译模型Transformer，现在可以用来做任何事了

https://mp.weixin.qq.com/s/lgGDTCF3qg84njv2IeHC9A

大规模集成Transformer模型，阿里达摩院如何打造WMT 2018机器翻译获胜系统

https://mp.weixin.qq.com/s/_UC2jlOfb34tfB_tsEXjMg

谷歌全新神经网络架构Transformer：基于自注意力机制，擅长自然语言理解

https://mp.weixin.qq.com/s/w3IKoygTLDsAxk1MB5JrGg

详细讲解Transformer新型神经网络在机器翻译中的应用

https://mp.weixin.qq.com/s/HzzDG8PpDlyilQjr2PH6PA

Transformer注解及PyTorch实现（上）

https://mp.weixin.qq.com/s/YDaSv5oHLEtyJrp4Y5e64A

Transformer注解及PyTorch实现（下）

https://mp.weixin.qq.com/s/j0KRAOf8Sd0_tTlRadnw9Q

利用篇章信息提升机器翻译质量

https://mp.weixin.qq.com/s/s_s-MtrEwRNyllV_9qpAQA

放弃幻想，全面拥抱Transformer：NLP三大特征抽取器（CNN/RNN/TF）比较

https://mp.weixin.qq.com/s/NtWMcwUGg591meuGubWY1g

CMU和谷歌联手放出XL号Transformer！提速1800倍

https://mp.weixin.qq.com/s/_MI-OQUHVbyZ3Utd52rWMw

Facebook推出最新跨语言预训练模型，刷新多项跨语言任务记录

https://mp.weixin.qq.com/s/C0p1U0-x6aRipvYJItn8-g

Transformer在进化！谷歌大脑用架构搜索方法找到Evolved Transformer

https://mp.weixin.qq.com/s/E7wygpWbSHoq6R7wlalFkA

放弃幻想，全面拥抱Transformer！NLP三大特征抽取器（CNN/RNN/TF）比较

https://mp.weixin.qq.com/s/cs4IjkmdPmu2-3Mu36f8UQ

OpenAI提出Sparse Transformer，文本、图像、声音都能预测，序列长度提高30倍

https://mp.weixin.qq.com/s/cpbIHt-rBu48uGifg_lPfg

Gaussian Transformer：一种自然语言推理的轻量方法

https://mp.weixin.qq.com/s/1y8jTqCcI7HkMA3qXtqdIg

阿里首次将Transformer用于淘宝电商推荐！效果超越深度兴趣网络DIN和谷歌WDL

https://mp.weixin.qq.com/s/E6E6tc2uDJofuNDJArB5RQ

邵晨泽：非自回归机器翻译

https://mp.weixin.qq.com/s/ZfvShgevhjhe39PmF-ONnA

周龙：同步双向文本生成

https://mp.weixin.qq.com/s/O7B-pQveMefaCuMswT7H-A

跨8千个字学习知识，上下文要多长还是交给Transformer自己决定吧

https://mp.weixin.qq.com/s/VG5WHwuO6DKRzcm0pddeMQ

参数少一半，效果还更好，天津大学和微软提出Transformer压缩模型

https://mp.weixin.qq.com/s/9aW1p03f6mEnvSRf56rcgw

Transformer-XL模型浅析

https://mp.weixin.qq.com/s/Xxk6n6r0lSuybjKlwzLAnw

TransformerXL：因为XL，所以更牛

https://mp.weixin.qq.com/s/o__YU5vlfKi4HGytlug3og

从头开始了解Transformer

https://mp.weixin.qq.com/s/DuRpnJRqvZ8Jfc7jutpIXw

Transformer研究指南

https://mp.weixin.qq.com/s/ehhsN0Xj1aUVyAxSxnFJmQ

Transformer落地：使用话语重写器改进多轮人机对话

https://zhuanlan.zhihu.com/p/85825460

Transformer在推荐模型中的应用总结

https://mp.weixin.qq.com/s/io87CCfThDZDwpIHngunqQ

Step-by-step to Transformer：深入解析工作原理（以Pytorch机器翻译为例）

https://mp.weixin.qq.com/s/9Sgln-bfP2DdB_o1RAgiGg

Transformer在深度推荐系统中的应用

https://mp.weixin.qq.com/s/61ZeO46s19sCW-3xQ_FUJg

如何在NLP中有效利用Deep Transformer？

https://www.zhihu.com/question/302392659

有了Transformer框架后是不是RNN完全可以废弃了？

https://mp.weixin.qq.com/s/_gTNZKCfhoazYR09hmu5lQ

时间自适应卷积：比自注意力更快的特征提取器

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

## pre-training

BERT的强大，主要不在网络结构上。上面提到的GPT 1.0虽然输给了BERT，但网络更深、向量维度也更大的GPT 2.0却赢了BERT，可见单向或者双向的Transformer，并不是问题的关键。让这些模型真正强大的原因主要在于pre-training。

![](/images/img3/BERT_2.png)

上图是BERT的pre-training和fine-tuning的结构图。

所谓的pre-training其实就是海量文本的无监督学习。

如何进行无监督学习呢？

BERT主要用了两个任务：

- Masked Language Model。随机盖住一句话的某个词，让NN去预测这个被盖住的词。

- Next Sentence Prediction。预测下一段话。

这两个任务，算是NLP的老任务了。但在传统的NLP pipeline中，属于非常下游的任务。BERT利用它们的特点，进行无监督学习，算是一个很大的突破了。

## 海量文本

BERT以及后来的GPT 2.0取得重大突破的关键，还在于海量的训练文本。

BERT拥有3.3亿个参数，训练数据包括：BooksCorpus（800M words）和English Wikipedia（2500M words）

GPT 2.0拥有15亿个参数，训练数据除了上述之外，还包括了800M个网页的文本。

如此海量的参数和数据，注定了这些模型的训练是一个超费算力的过程。NLP的游戏规则将变成：

- 土豪大科技公司靠暴力上数据规模，上GPU或者TPU集群，训练好预训练模型发布出来，不断刷出大新闻。通过暴力美学横扫一切，这是土豪端的玩法。

- 而对于大多数人来说，你能做的是在别人放出来的预训练模型上做小修正或者刷应用或者刷各种榜单，逐步走向了应用人员的方向，这是大多数NLP从业者未来几年要面对的dilemma。

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

## 预训练语言模型进化史

![](/images/img3/BERT.jpg)

![](/images/img3/PTM.jpg)

https://mp.weixin.qq.com/s/kwKZfNSYTzc-PGKxTxm8-w

复旦大学：最新《预训练语言模型》2020综述论文

https://zhuanlan.zhihu.com/p/115014536

全面总结！PTMs：NLP预训练模型

https://mp.weixin.qq.com/s/1ixYjJN-bJPGrr7v-4d7Rw

预训练语言模型的前世今生：萌芽时代

https://mp.weixin.qq.com/s/g4jEVU3BkRem-DYXCn5eFQ

预训练语言模型的前世今生：风起云涌

https://mp.weixin.qq.com/s/U8f0cXoPrN32PM3944Oqkg

预训练语言模型的前世今生：十分钟了解文本分类通用训练技巧

https://mp.weixin.qq.com/s/uAJ_05g0Zo33mgygTnow1Q

预训练语言模型的前世今生：银色独角兽GPT家族

https://mp.weixin.qq.com/s/2_MXIEk5-JP5KwsV6al9XQ

预训练语言模型的前世今生：BERT，开启NLP新时代的王者

https://mp.weixin.qq.com/s/yRn1AELGK3jgfOaaxIo67Q

预训练语言模型的前世今生：Huggingface简介及BERT代码浅析

https://mp.weixin.qq.com/s/sMocYFvESXoBGtX_NWmQkQ

预训练语言模型的前世今生：百度出品ERNIE合集，问国产预训练语言模型哪家强

https://mp.weixin.qq.com/s/WDXGCC_MPK_sBCj4Bx6EDw

从静态到动态，词表征近几十年发展回顾

https://mp.weixin.qq.com/s/vW8jglstKsR7OSbyjexKrQ

自然语言处理嵌入：语义向量表示理论与进展，从Word2Vec到BERT，163页pdf

https://mp.weixin.qq.com/s/tokxh7Conb-hajj8pnr2fA

Google BERT作者Jacob斯坦福亲授《上下文词向量与预训练语言模型: BERT到T5》43页ppt

https://mp.weixin.qq.com/s/FRfjOSbnquQeFSBDI1FWwg

6个你应该用用看的用于文本分类的最新开源预训练模型

## ELMo

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
