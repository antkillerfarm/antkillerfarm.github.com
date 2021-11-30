---
layout: post
title:  Attention（六）——BERT进阶
category: Attention 
---

* toc
{:toc}


# Attention in CV & RS（续）

https://mp.weixin.qq.com/s/EXtTUh4_w07Kc7hfBBMBiw

搞懂Vision Transformer原理和代码，看这篇技术综述就够了（四）

https://mp.weixin.qq.com/s/MyRJl_QsO2X1yF4akPGktg

搞懂Vision Transformer原理和代码，看这篇技术综述就够了（五）

https://mp.weixin.qq.com/s/FIilwbLzYk4av8w11VgJeQ

计算机视觉中的Transformer

https://mp.weixin.qq.com/s/2BECepucUdzLYlyU1aM7bA

网络架构设计：CNN based和Transformer based

https://mp.weixin.qq.com/s/k-pe1qTelVmvcwY6hmSi4A

Transformer是巧合还是必然？搜索推荐领域的新潮流

https://mp.weixin.qq.com/s/rATLyYBgo2nWY4rKXmgV5w

来自Transformer的降维打击：ReID各项任务全面领先，阿里&浙大提出TransReID

https://mp.weixin.qq.com/s/aWzHpeNS3OUrjrbEvnI87g

用Pytorch轻松实现28个视觉Transformer，开源库timm了解一下

https://mp.weixin.qq.com/s/J7Fw-T1tYSqi9_vx8VSqYA

TimeSformer：视频理解所需的只是时空注意力吗？

https://mp.weixin.qq.com/s/dHWc0MFwuyLMsCoBGN353Q

PVT：可用于密集任务backbone的金字塔视觉transformer

https://mp.weixin.qq.com/s/DKWSeRu_ThMf_vf9j1GCbQ

PoseFormer：首个纯基于Transformer的3D人体姿态估计网络，性能达到SOTA

https://mp.weixin.qq.com/s/O-xcsIHufrPQKPQGNcKjkg

视觉子领域中的Transformer

https://mp.weixin.qq.com/s/IeQdvz8DrNAULy2k7oFgWw

Transformers在计算机视觉概述

https://mp.weixin.qq.com/s/H2GZgnR8jN5ztUACiowpZQ

Vision Transformer新秀：VOLO

https://mp.weixin.qq.com/s/_ETbYLu6qklaxJ2dv-xeSA

计算机视觉中的Transformer，98页ppt

# BERT进阶

## AR vs AE

自回归模型，是统计上一种处理时间序列的方法，用同一变数例如x的之前各期，亦即$$x_1$$至$$x_{t-1}$$来预测本期$$x_t$$的表现，并假设它们为一线性关系。因为这是从回归分析中的线性回归发展而来，只是不用x预测y，而是用x预测x自己，所以叫做自回归。

---

**AR**: Autoregressive Lanuage Modeling，又叫自回归语言模型。它指的是，依据前面(或后面)出现的tokens来预测当前时刻的token，代表模型有ELMO、GTP等。

$$\text{forward:}p(x)=\prod_{t=1}^Tp(x_t|x_{<t})$$

$$\text{backward:}p(x)=\prod_{t=T}^1p(x_t|x_{>t})$$

- 缺点：它只能利用单向语义而不能同时利用上下文信息。ELMO通过双向都做AR模型，然后进行拼接，但从结果来看，效果并不是太好。

- 优点：对自然语言生成任务(NLG)友好，天然符合生成式任务的生成过程。这也是为什么GPT能够编故事的原因。

**AE**:Autoencoding Language Modeling，又叫自编码语言。通过上下文信息来预测当前被mask的token，代表有BERT，Word2Vec(CBOW)。

$$p(x)=\prod_{x\in Mask}p(x_t|context)$$

- 缺点：由于训练中采用了MASK标记，导致预训练与微调阶段不一致的问题。此外对于生成式问题，AE模型也显得捉襟见肘，这也是目前BERT为数不多没有实现大的突破的领域。

- 优点：能够很好的编码上下文语义信息，在自然语言理解(NLU)相关的下游任务上表现突出。

参考：

https://mp.weixin.qq.com/s/n6F6MTjrUCmvEoaLiVZpxA

更深的编码器+更浅的解码器=更快的自回归模型

https://mp.weixin.qq.com/s/pe2E69Gpw0nT9sSHvtBGSg

自回归与非自回归模型不可兼得？预训练模型BANG全都要！

## UniLM

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

https://mp.weixin.qq.com/s/vFdm-UHns7Nhbmdoiu6jWg

谷歌终于开源BERT代码：3亿参数量，机器之心全面解读

https://zhuanlan.zhihu.com/p/58425003

从Word2Vec到Bert，聊聊词向量的前世今生（一）

https://mp.weixin.qq.com/s/SfMIKfF_B4agFCHN_U_mzQ

BAM！利用知识蒸馏和多任务学习构建的通用语言模型

https://mp.weixin.qq.com/s/6G5Mu7-1omGtQ_9Gt9lUBw

基于预训练自然语言生成的文本摘要方法

https://mp.weixin.qq.com/s/yysnPauB22YgprpOi1ZWSQ

深入理解BERT Transformer，不仅仅是注意力机制

https://mp.weixin.qq.com/s/kFABJJ3fBC48-4DXK8PERQ

10大任务超越BERT，微软提出多任务深度神经网络MT-DNN

https://mp.weixin.qq.com/s/jlGfxkT_o9sgFlUuR_x5Tw

微软开源用于学习通用语言嵌入的MT-DNN模型

https://mp.weixin.qq.com/s/D68YzjYvpc2epGWFBP6rIQ

谷歌实习生新算法提速惊人！BERT训练从三天三夜，缩短到一个小时

https://mp.weixin.qq.com/s/iDGofh_ycWJzfqQriPEXGQ

如何用Python和BERT做中文文本二元分类？

https://zhuanlan.zhihu.com/p/91052495

当BERT遇上知识图谱

https://mp.weixin.qq.com/s/wQW-JT-sGMj60OtXwTssyQ

BERT模型推理加速总结

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

https://zhuanlan.zhihu.com/p/113326366

如何训练并使用Bert

https://mp.weixin.qq.com/s/dmHxEkmVFXcCGhv8eH91Tw

从Word2Vec到BERT:上下文嵌入(Contextual Embedding)最新综述论文

https://mp.weixin.qq.com/s/g6-NjoFMPpxjsh38X-wTFQ

BERT，GPT-2这些顶尖工具到底该怎么用到我的模型里?

https://mp.weixin.qq.com/s/N6xBFZ82dkSGCbj6vC5nLQ

上下文预训练模型最全整理：原理、应用、开源代码、数据分享

https://mp.weixin.qq.com/s/-6XpuO7_ve_EdSPCMeWE7g

Attention isn’t all you need！BERT的力量之源远不止注意力

https://mp.weixin.qq.com/s/Y2bs2QegRadSR7lbiFFnWg

BERT一作Jacob Devlin斯坦福演讲PPT：BERT介绍与答疑

https://zhuanlan.zhihu.com/p/62308732

浅谈Bert：语言理解中的预训练编码器

https://mp.weixin.qq.com/s/1Cz6js4kYdvc8g4oKjVPeA

BERT烹饪之法：fintune的艺术

https://mp.weixin.qq.com/s/nVM2Kxc_Mn7BAC6-Pig2Uw

BERT模型的标准调优和花式调优

https://mp.weixin.qq.com/s/FwmEIZ3ugeZBbLIGHmH-_g

BERT之后，GLUE基准升级为SuperGLUE：难度更大

https://mp.weixin.qq.com/s/SDVxn3Ra1dKmr-XgKNg5IA

罗玲：From Word Representation to BERT

https://mp.weixin.qq.com/s/-bh8BL4LxnevS8xnW5U9ZA

中科院自动化所提出BIFT模型：面向自然语言生成，同步双向推断

https://mp.weixin.qq.com/s/7yCnAHk6x0ICtEwBKxXpOw

序列到序列自然语言生成任务超越BERT、GPT！微软提出通用预训练模型MASS

https://mp.weixin.qq.com/s/7sIUaSON53hsXUJjq8uVUA

马聪：NLP中的生成式预训练模型

https://mp.weixin.qq.com/s/s1bQFdA6gtoHeeQMJKQ8UQ

Bert时代的创新：Bert应用模式比较及其它

https://mp.weixin.qq.com/s/zqlWx3e4LOJ3_Zy2DEbCjw

从语言模型看Bert的善变与GPT的坚守

https://mp.weixin.qq.com/s/LngE10Hnqe9bgFzpNfUwLQ

NLP中的词向量对比：word2vec/glove/fastText/elmo/GPT/bert

https://mp.weixin.qq.com/s/MgLLPEY3ynJGkuTgnIXndQ

站在BERT肩膀上的NLP新秀们（PART I）

https://mp.weixin.qq.com/s/yPq1cGnhcbaNLOjadj91pw

Bert时代的创新：Bert在NLP各领域的应用进展

https://mp.weixin.qq.com/s/l-de0vfx-L24g58IxK-NKQ

Jeff Dean强推：可视化Bert网络，发掘其中的语言、语法树与几何学

https://mp.weixin.qq.com/s/nlFXfgM5KKZXnPdwd97JYg

哈工大讯飞联合实验室发布基于全词覆盖的中文BERT预训练模型

https://zhuanlan.zhihu.com/p/70389596

一批高质量中文BERT预训练模型请查收（上）

https://mp.weixin.qq.com/s/h1VUSY7_UZF3PmjSN0DMSg

从One-hot, Word embedding到Transformer，一步步教你理解Bert

https://zhuanlan.zhihu.com/p/132554155

超细节的BERT/Transformer知识点

https://mp.weixin.qq.com/s/UJlmjFHWhnlXXJoRv4zkEQ

虽被BERT碾压，但还是有必要谈谈BERT时代与后时代的NLP
