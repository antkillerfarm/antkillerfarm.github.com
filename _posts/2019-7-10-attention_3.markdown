---
layout: post
title:  Attention（三）——预训练语言模型进化史, BERT
category: Attention 
---

* toc
{:toc}

# Transformer（续）

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

https://mp.weixin.qq.com/s/BIwUYRq0yP9nE80OOlMWaQ

关于Transformer那些的你不知道的事

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

https://mp.weixin.qq.com/s/Oixc46P9rQeiMDjI-0j0cw

Transformer在美团搜索排序中的实践

https://mp.weixin.qq.com/s/S_RELwKsqInvacTxdwJBPg

浅谈Transformer模型中的位置表示

https://mp.weixin.qq.com/s/_lEOcyAovKMJYxBYsQIWUQ

万字长文带你一览ICLR2020最新Transformers进展（上）

https://mp.weixin.qq.com/s/4KCcFHBEYJ3nWaHplqVRNA

万字长文带你一览ICLR2020最新Transformers进展（下）

https://mp.weixin.qq.com/s/8tVste5Wuwy4o4jh-jx5hA

Transformer温故知新

https://zhuanlan.zhihu.com/p/143852458

NLP中更好&更快的Transformer

https://mp.weixin.qq.com/s/b5Ont9vHPeCPnAjuDGv5Bg

Facebook开源新思路！DETR：用Transformers来进行端到端的目标检测

https://mp.weixin.qq.com/s/JpBds6NQIBZ0S8GsMo4LEA

这六大方法，如何让Transformer轻松应对高难度长文本序列？

https://mp.weixin.qq.com/s/R5jqk1ow_3cKLzM1XlDbUg

告别自注意力，谷歌为Transformer打造新内核Synthesizer

https://mp.weixin.qq.com/s/RrIUxwkRaGxzJo0hIx3SIA

TTSR：用Transformer来实现端到端的超分辨率任务

https://mp.weixin.qq.com/s/N1I4mGKzsHJiAluZL17sDQ

Transformers是一种图神经网络

# 预训练语言模型进化史

![](/images/img3/Sesame_Street.jpg)

Sesame Street（芝麻街）是是美国公共广播协会（PBS）制作播出的儿童教育电视节目，该节目于1969年11月10日在全国教育电视台（PBS的前身）上首次播出。它是迄今为止，获得艾美奖奖项最多的一个儿童节目。

从ELMO开始，一系列的预训练语言模型皆以该剧中的角色命名，甚至ERNIE还有清华和百度两个版本。。。

![](/images/img3/BERT.jpg)

![](/images/img3/PTM.jpg)

![](/images/img3/PTM.png)

https://mp.weixin.qq.com/s/kwKZfNSYTzc-PGKxTxm8-w

复旦大学：最新《预训练语言模型》2020综述论文

https://zhuanlan.zhihu.com/p/115014536

全面总结！PTMs：NLP预训练模型

https://mp.weixin.qq.com/s/WDXGCC_MPK_sBCj4Bx6EDw

从静态到动态，词表征近几十年发展回顾

https://mp.weixin.qq.com/s/vW8jglstKsR7OSbyjexKrQ

自然语言处理嵌入：语义向量表示理论与进展，从Word2Vec到BERT，163页pdf

https://mp.weixin.qq.com/s/tokxh7Conb-hajj8pnr2fA

Google BERT作者Jacob斯坦福亲授《上下文词向量与预训练语言模型: BERT到T5》43页ppt

https://mp.weixin.qq.com/s/FRfjOSbnquQeFSBDI1FWwg

6个你应该用用看的用于文本分类的最新开源预训练模型

https://mp.weixin.qq.com/s/UVeWDavdHxmziUWW39jrkA

原理篇一：从one-hot到Word2vec

https://mp.weixin.qq.com/s/JSWw5RBgQoW-PrfIhbMtjQ

原理篇二：从ELMo到ALBERT

https://zhuanlan.zhihu.com/p/69290203

Transformer结构及其应用详解--GPT、BERT、MT-DNN、GPT-2

https://mp.weixin.qq.com/s/Jf0uKIjwaBbzNCB7IiTjpA

预训练模型专辑（一）

https://mp.weixin.qq.com/s/pl27hVrphiFHYqZU85KGFw

预训练模型专辑（二）

https://zhuanlan.zhihu.com/p/145701656

预训练语言模型们(上)

https://zhuanlan.zhihu.com/p/146193549

预训练语言模型们(下)

https://mp.weixin.qq.com/s/BhcnTmSje983MYT_alEiiw

一文盘点预训练神经语言模型

https://mp.weixin.qq.com/s/RKA_RxTQkIeJX3_VIKJiRQ

周明：预训练模型在多语言、多模态任务的进展

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

https://mp.weixin.qq.com/s/yRn1AELGK3jgfOaaxIo67Q

Huggingface简介及BERT代码浅析

https://mp.weixin.qq.com/s/sMocYFvESXoBGtX_NWmQkQ

百度出品ERNIE合集，问国产预训练语言模型哪家强

https://mp.weixin.qq.com/s/uOGNoePkwfeixTtI4q4t8Q

MT-DNN(KD) : 预训练、多任务、知识蒸馏的结合

https://mp.weixin.qq.com/s/-fQ9VlaOjssDufcZuCe91w

结合HuggingFace代码浅析Transformer

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
