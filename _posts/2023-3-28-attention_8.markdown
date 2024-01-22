---
layout: post
title:  Attention（八）——Beyond Transformer, BERT进阶
category: Attention 
---

* toc
{:toc}

# Large Language Model（续）

![](/images/img5/LLM_pipeline.jpg)

---

参考：

https://www.zhihu.com/question/584132646

中国的大语言模型“悟道2.0”参数是GPT-3十倍，是否中国在大语言模型训练技术上已经远远超过美国？

https://zhuanlan.zhihu.com/p/463352552

稀疏性在机器学习中的发展趋势——Sparsity，稀疏激活，高效计算，MoE，稀疏注意力机制

https://zhuanlan.zhihu.com/p/254821426

乘风破浪的PTM：两年来预训练模型的技术进展（2020年之前的主流技术）

https://zhuanlan.zhihu.com/p/597586623

通向AGI之路：大型语言模型（LLM）技术精要

https://mp.weixin.qq.com/s/eV_9Mi2879w_gfoyiSm8Ug

LLM全景图（The Landscape of LLM）

https://www.zhihu.com/question/604592470

前两个月国产类ChatGPT大模型如雨后春笋，为何最近都没声音了?

https://mp.weixin.qq.com/s/oqhi58NcEVH1oVrW2p4xlQ

大模型参数高效微调技术原理综述（一）-背景、参数高效微调简介

https://mp.weixin.qq.com/s/fUAUr9X3XLndfjga2QIHbA

大模型参数高效微调技术原理综述（二）-BitFit、Prefix Tuning、Prompt Tuning

https://mp.weixin.qq.com/s/f4l04f78F507JRrCawnV8w

大模型参数高效微调技术原理综述（三）-P-Tuning、P-Tuning v2

https://mp.weixin.qq.com/s/nUAcCz6mcgGuUeuTfgqmOQ

大模型参数高效微调技术原理综述（四）-Adapter Tuning及其变体

https://mp.weixin.qq.com/s/N_N6RqKB9pjZ1tozfM5f5A

大模型参数高效微调技术原理综述（五）-LoRA、AdaLoRA、QLoRA

https://mp.weixin.qq.com/s/M2nds_FJBXooi08qDU-4yA

大模型参数高效微调技术原理综述（六）-MAM Adapter、UniPELT

https://mp.weixin.qq.com/s/P_AmTa4s8dOyc_0fZBgNPA

大模型参数高效微调技术原理综述（七）-最佳实践、总结

https://zhuanlan.zhihu.com/p/618695885

LLaMA, Alpaca, ColossalChat系列模型研究

https://zhuanlan.zhihu.com/p/642412124

LLM的推理优化技术纵览

https://zhuanlan.zhihu.com/p/638809556

大模型高效微调综述上：Adapter Tuning、AdaMix、PET、Prefix-Tuning、Prompt Tuning、P-tuning、P-tuning v2

https://zhuanlan.zhihu.com/p/651564985

主流大语言模型从预训练到微调的技术原理

# Beyond Transformer

https://zhuanlan.zhihu.com/p/605425639

RWKV 14B对比GLM 130B和NeoX 20B，展示RWKV的性能

代码：

https://github.com/BlinkDL/ChatRWKV

RWKV没有使用attention，而是号称100% RNN。

RNN-based没有attention之类机制的模型是怎么获得long memory的能力的啊？

这个形式就是Transformers are RNNs的形式，只不过把Q换成了positional invariant的time weighting。最近很多work都显示Attention里的Q其实没啥用，换成一个跟着相对位置exponential decay的term就行了。

---

https://blog.csdn.net/v_JULY_v/article/details/134923301

一文通透想颠覆Transformer的Mamba：从SSM、S4到mamba、线性transformer(含RWKV解析)

# BERT进阶

## AR vs AE

自回归模型，是统计上一种处理时间序列的方法，用同一变数例如x的之前各期，亦即$$x_1$$至$$x_{t-1}$$来预测本期$$x_t$$的表现，并假设它们为一线性关系。因为这是从回归分析中的线性回归发展而来，只是不用x预测y，而是用x预测x自己，所以叫做自回归。

---

**AR**: Autoregressive Lanuage Modeling，又叫自回归语言模型。它指的是，依据前面(或后面)出现的tokens来预测当前时刻的token，代表模型有ELMO、GTP等。

$$\text{forward:}p(x)=\prod_{t=1}^Tp(x_t|x_{<t})$$

$$\text{backward:}p(x)=\prod_{t=T}^1p(x_t|x_{>t})$$

- 缺点：它只能利用单向语义而不能同时利用上下文信息。ELMO通过双向都做AR模型，然后进行拼接，但从结果来看，效果并不是太好。

- 优点：对自然语言生成任务(NLG)友好，天然符合生成式任务的生成过程。这也是为什么GPT能够编故事的原因。

**AE**:Autoencoding Language Modeling，又叫自编码语言模型。通过上下文信息来预测当前被mask的token，代表有BERT，Word2Vec(CBOW)。

$$p(x)=\prod_{x\in Mask}p(x_t|context)$$

- 缺点：由于训练中采用了MASK标记，导致预训练与微调阶段不一致的问题。此外对于生成式问题，AE模型也显得捉襟见肘，这也是目前为止，BERT为数不多没有实现大的突破的领域。

- 优点：能够很好的编码上下文语义信息，在自然语言理解(NLU)相关的下游任务上表现突出。

---

2023.3

ChatGPT的出现，为自然语言生成任务找到了商业化的路径。有鉴于此，Google也不得不在BERT上对AR模型，做了一些有损逼格的妥协。。。囧

![](/images/img5/T5.png)

deep encoder+shallow decoder

---

参考：

https://mp.weixin.qq.com/s/n6F6MTjrUCmvEoaLiVZpxA

更深的编码器+更浅的解码器=更快的自回归模型

https://mp.weixin.qq.com/s/pe2E69Gpw0nT9sSHvtBGSg

自回归与非自回归模型不可兼得？预训练模型BANG全都要！

https://www.zhihu.com/question/588325646

为什么现在的LLM都是Decoder only的架构？

https://www.zhihu.com/question/592545459

大模型都是基于Transformer堆叠，采用Encoder或者Decoder堆叠，有什么区别？

## UniLM

![](/images/img5/UniLM.jpg)

|  | Encoder注意力 | Decoder注意力 | 是否共享参数 |
|:--:|:--:|:--:|:--:|
| GPT | 单向 | 单向 | 是 |
| UniLM | 双向 | 单向 | 是 |
| T5 | 双向 | 单向 | 否 |

https://mp.weixin.qq.com/s/m_FU4NmjUsvxusRidDb-Xg

UniLM:一种既能阅读又能自动生成的预训练模型

https://mp.weixin.qq.com/s/yyUPqxpfBwUSRbwM6SSAcQ

UniLM论文阅读笔记

https://mp.weixin.qq.com/s/RjeuHXa8O3MzSpTOuOHMkQ

站在BERT肩膀上的NLP新秀们：XLMs、MASS和UNILM

https://mp.weixin.qq.com/s/UEBKSKEkZTbpR49_Rh50Jg

微软统一预训练语言模型UniLM 2.0解读

## Electra

https://mp.weixin.qq.com/s/dFT7KKMH56unkOEA9H4Kuw

吊打BERT Large的小型预训练模型ELECTRA终于开源！真相却让人...

https://mp.weixin.qq.com/s/6i9eQISKsWU0jawKzWg8nQ

超越bert，最新预训练模型ELECTRA论文阅读笔记

https://mp.weixin.qq.com/s/lkB1xn6G2P5Nivj7DcYg5w

Electra: 判别还是生成，这是一个选择

## Embedding

预训练刚兴起时，在语言模型的输出端重用Embedding权重是很常见的操作，比如BERT、第一版的T5、早期的GPT，都使用了这个操作，这是因为当模型主干部分不大且词表很大时，Embedding层的参数量很可观，如果输出端再新增一个独立的同样大小的权重矩阵的话，会导致显存消耗的激增。不过随着模型参数规模的增大，Embedding层的占比相对变小了，加之《Rethinking embedding coupling in pre-trained language models》等研究表明共享Embedding可能会有些负面影响，所以现在共享Embedding的做法已经越来越少了。

https://kexue.fm/archives/9698

语言模型输出端共享Embedding的重新探索

## 外推性

对于Transformer模型来说，其长度的外推性是我们一直在追求的良好性质，它是指我们在短序列上训练的模型，能否不用微调地用到长序列上并依然保持不错的效果。

自从Transform被提出以来，一个基本问题还没有被解决，一个模型如何在推断时对训练期间没有见过的更长序列进行外推。众所周知，Bert支持的最长句子长度是512，那为什么Bert只能支持512的句子长度呢？

我们看一下BertEmbeddings的初始化，我们可以看到position_ids，被初始化成0-511，这个也就是BERT处理文本最大长度是512的原因，这里Bert使用的是绝对位置编码。

参考：

https://spaces.ac.cn/archives/9431

长度外推性与局部注意力

https://zhuanlan.zhihu.com/p/656684326

大模型位置编码-ALiBi位置编码

## RoPE

Rotary Position Embedding是苏剑林的作品，并被后来流行的LLAMA等大模型所采用。

参考：

https://spaces.ac.cn/archives/8265

博采众长的旋转式位置编码

## 参考

https://www.zhihu.com/question/298203515

如何评价BERT模型？

https://mp.weixin.qq.com/s/Fao3i99kZ1a6aa3UhAYKhA

全面超越人类！Google称霸SQuAD，BERT横扫11大NLP测试

https://mp.weixin.qq.com/s/INDOBcpg5p7vtPBChAIjAA

最强预训练模型BERT的Pytorch实现

https://mp.weixin.qq.com/s/SZMYj4rMneR3OWST007H-Q

解读谷歌最强NLP模型BERT：模型、数据和训练

https://mp.weixin.qq.com/s/8uZ2SJtzZhzQhoPY7XO9uw

详细解读谷歌新模型BERT为什么嗨翻AI圈

https://zhuanlan.zhihu.com/p/66053631

BERT

https://mp.weixin.qq.com/s/WEbJnO04DOrsxUbzpgL66g

BERT源码分析（PART I）

https://mp.weixin.qq.com/s/iXjE7KoyvFQ8uekLKRK4jw

BERT源码分析（PART II）

https://mp.weixin.qq.com/s/DxBC_x5ZWC6SECfnwDGnVg

BERT源码分析（PART III）

https://mp.weixin.qq.com/s/kI_k_plZbRzmdeXxt2_2WA

从Transformer到BERT模型

https://mp.weixin.qq.com/s/Bnk0nIjBdb58WVJEY8MqnA

NLP中各种各样的编码器

https://mp.weixin.qq.com/s/CofeiL4fImq98UeuJ4hWTg

预训练BERT，官方代码发布前他们是这样用TensorFlow解决的

https://mp.weixin.qq.com/s/HOD1Hb70NhTXXCXlopzfng

BERT推理加速实践

https://mp.weixin.qq.com/s/0luHJsw7WWJskJWGThR5qg

使用BERT做文本摘要

https://mp.weixin.qq.com/s/IY8J09LvDAr8owYffKi5Dw

五问BERT：深入理解NLP领域爆红的预训练模型

https://zhuanlan.zhihu.com/p/106901954

BERT, ELMo, & GPT-2: 这些上下文相关的表示到底有多上下文化？

https://mp.weixin.qq.com/s/mkDmn4zy_s87kiiDIkx0VQ

NLP的12种后BERT预训练方法

https://www.zhihu.com/question/327450789

Bert如何解决长文本问题？

https://mp.weixin.qq.com/s/QTELpbr480AJsBINm-FHKQ

代码也能预训练，微软&哈工大最新提出CodeBERT模型，支持自然-编程双语处理

https://mp.weixin.qq.com/s/ZEWCcxTEuEMvQ5__t3gkBg

BERT技术体系综述论文：40项分析探究BERT如何work

https://mp.weixin.qq.com/s/OsfeAA_tbzAddh1eunwx2w

关于BERT，面试官们都怎么问

https://mp.weixin.qq.com/s/e3n_16uB-qGeGSaGwzlBDw

这群工程师，业余将中文NLP推进了一大步（中文预训练模型）

https://mp.weixin.qq.com/s/V4pbjP5na1OYp-TorUik8g

详聊如何用BERT实现关系抽取

https://mp.weixin.qq.com/s/s5YIG6rBEy6fZkFLh-CzoA

后BERT时代生存指南之VL-BERT篇
