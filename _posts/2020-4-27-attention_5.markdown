---
layout: post
title:  Attention（五）——Attention in CV, BERT进阶
category: Attention 
---

* toc
{:toc}

# 轻量化BERT

| Paper | Prune | Factor | Distill | W. Sharing | Quant. | Pre-train | Downstream |
|:--:|:--:|:--:|:--:|:--:|:--:|:--:|:--:|
| Compressing BERT: Studying the Effects of Weight Pruning on Transfer Learning | Y |  |  |  |  | Y | Y |
| Are Sixteen Heads Really Better than One? | Y |  |  |  |  |  | Y |
| Pruning a BERT-based Question Answering Model | Y |  |  |  |  |  | Y |
| Reducing Transformer Depth on Demand with Structured Dropout | Y |  |  |  |  | Y |  |
| Reweighted Proximal Pruning for Large-Scale Language Representation | Y |  |  |  |  | Y |  |
| Structured Pruning of Large Language Models |  | Y |  |  |  |  | Y |
| ALBERT: A Lite BERT for Self-supervised Learning of Language Representations |  | Y |  | Y |  | Y |  |
| Extreme Language Model Compression with Optimal Subwords and Shared Projections |  |  | Y |  |  | Y |  |
| DistilBERT, a distilled version of BERT: smaller, faster, cheaper and lighter |  |  | Y |  |  | Y |  |
| Distilling Task-Specific Knowledge from BERT into Simple Neural Networks |  |  | Y |  |  |  | Y |
| Distilling Transformers into Simple Neural Networks with Unlabeled Transfer Data |  |  | Y |  |  |  | Y |
| Attentive Student Meets Multi-Task Teacher: Improved Knowledge Distillation for Pretrained Models |  |  | Y |  |  | Multi-task  | Y |
| Patient Knowledge Distillation for BERT Model Compression |  |  | Y |  |  |  | Y |
| TinyBERT: Distilling BERT for Natural Language Understanding |  |  | Y |  |  | Y | Y |
| MobileBERT: Task-Agnostic Compression of BERT by Progressive Knowledge Transfer |  |  | Y |  |  | Y |  |
| Q8BERT: Quantized 8Bit BERT |  |  |  |  | Y |  | Y |
| Q-BERT: Hessian Based Ultra Low Precision Quantization of BERT |  |  |  |  | Y |  | Y |

![](/images/img4/BERT.png)

https://www.zhihu.com/question/347898375

如何看待瘦身成功版BERT——ALBERT？

https://zhuanlan.zhihu.com/p/316865623

2020年9月谷歌研究给出的综述“Efficient Transformers: A Survey”

https://mp.weixin.qq.com/s/a0d0b1jSm5HxHso9Lz8MSQ

小版BERT也能出奇迹：最火的预训练语言库探索小巧之路

https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650771134&idx=2&sn=012082a897dbf125000e38b73520c51d

TinyBERT：模型小7倍，速度快8倍，华中科大、华为出品

https://mp.weixin.qq.com/s/rBiafIT8JUuSe_zib9yssw

TinyBert: 模型蒸馏的全方位应用

https://mp.weixin.qq.com/s/i82wGiSOlA4l4ozimrE2gg

加速BERT模型有多少种方法？从架构优化、模型压缩到模型蒸馏最新进展详解！

https://mp.weixin.qq.com/s/R2MW_5kskvXyuSOh7kfJaA

ALBERT：更轻更快的NLP预训练模型

https://mp.weixin.qq.com/s/dWzpqP_U8Y5DyfWHVTl5Vg

BERT瘦身之路：Distillation，Quantization，Pruning

https://mp.weixin.qq.com/s/DAsY9-Dl5T6peo_71ICOtw

基于ALBERT的文本相似度计算

http://mitchgordon.me/machine/learning/2019/11/18/all-the-ways-to-compress-BERT.html

15篇论文全面概览BERT压缩方法

https://mp.weixin.qq.com/s/5tYuP09dtkmYYGX2R-mCPQ

从transformer到albert

https://zhuanlan.zhihu.com/p/110934513

关于BERT的模型压缩简介

https://mp.weixin.qq.com/s/CkAHKXWi24tDBz4HiWkhBw

模型小快好！微软预训练语言模型通用压缩方法MiniLM助你“事半功倍”

https://mp.weixin.qq.com/s/iLO1FOE-4z1p07RCfCJIaA

MiniLM：通用模型压缩方法

https://mp.weixin.qq.com/s/LF8TiVccYcm4B6krCOGVTQ

ALBERT论文图解介绍

https://mp.weixin.qq.com/s/QdrwlaFZi3VRGptw4cYJSQ

别再蒸馏3层BERT了！变矮又能变瘦的DynaBERT了解一下

https://mp.weixin.qq.com/s/1ZqLWCeyUeb8rsuwTJeZQw

如何修剪BERT达到加速目的？理论与实现

https://mp.weixin.qq.com/s/I_MbkbpyQWKCA8QQu5355A

MobileBERT：一个在资源有限设备上使用的BERT模型

https://mp.weixin.qq.com/s/jBJvrR71OIov2aOucFfd6Q

BERT模型压缩技术概览

https://mp.weixin.qq.com/s/LXp6otaW34r0v8yc4TJNog

LSRA: 轻量级Transformer，注意力长短搭配

https://zhuanlan.zhihu.com/p/343229835

Poor Man's BERT: 更小更快的Transformer模型

# 快速BERT

轻量化BERT是从计算量的角度出发，对于传统BERT的优化。而快速BERT主要着眼于软件工程角度，如何更好的利用各种硬件加速BERT的计算。典型的有NVIDIA的FasterTransformer和腾讯的TurboTransformer。

https://mp.weixin.qq.com/s/1R_plHqxTLE-Fw3TjYnlJQ

GPU BERT上线性能不合格，看看微信AI的PPoPP论文

# Linformer

https://mp.weixin.qq.com/s/IOc-gxOa6a415Hf1VBmiQw

“Linformer”拍了拍“被吊打Transformers的后浪们”

https://mp.weixin.qq.com/s/cDQW5992hTaeGoA7zL7Vzg

Linformer: 线性复杂度的Attention

Self-Attention 加速方法一览：ISSA、CCNet、CGNL、Linformer

# Attention in CV

https://mp.weixin.qq.com/s/PD2YnFb6yleDEMhz3ahFSQ

计算机视觉"新"范式: Transformer

https://mp.weixin.qq.com/s/wAy3VsOIHxR948eOuXghmA

使用Transformers创建计算机视觉模型

https://zhuanlan.zhihu.com/p/288758894

CV注意力机制论文阅读笔记

https://mp.weixin.qq.com/s/M3VRlz8-McbTbp9VcctU0w

如何让BERT拥有视觉感知能力？两种方式将视频信息注入BERT

https://mp.weixin.qq.com/s/-eBL9gFbAGFtmqkLMAoUTw

文本+视觉，多篇Visual/Video BERT论文介绍

http://mp.weixin.qq.com/s/Bt6EMD4opHCnRoHKYitsUA

结合人类视觉注意力进行图像分类

https://mp.weixin.qq.com/s/POYTh4Jf7HttxoLhrHZQhw

基于双向注意力机制视觉问答pyTorch实现

http://blog.csdn.net/leo_xu06/article/details/53491400

视觉注意力的循环神经网络模型

https://mp.weixin.qq.com/s/JoTzaInn_uAA9oZgMcfskw

计算机视觉技术self-attention最新进展

https://zhuanlan.zhihu.com/p/32928645

计算机视觉中的注意力机制

https://zhuanlan.zhihu.com/p/56501461

计算机视觉中的注意力机制

https://zhuanlan.zhihu.com/p/32971586

图像描述：基于项的注意力机制

https://zhuanlan.zhihu.com/p/33158614

图像识别：基于位置的柔性注意力机制

https://mp.weixin.qq.com/s/tVKEJ9rqlMaZ9bx6ngIelw

自注意力机制在计算机视觉中的应用

https://mp.weixin.qq.com/s/Di-TbseiezMBc-MUYoEFHg

CV领域的注意力机制

https://mp.weixin.qq.com/s/7ETHeN2xV_hEwkDxrhJyNg

用Attention玩转CV，一文总览自注意力语义分割进展

https://mp.weixin.qq.com/s/G4mFW8cn-ho3KGmbw5sSTw

计算机视觉中注意力机制原理及其模型发展和应用

https://mp.weixin.qq.com/s/gar7zcl68W4oKnFPLFekoQ

Attention增强的卷积网络

https://zhuanlan.zhihu.com/p/308301901

3W字长文带你轻松入门视觉transformer

https://mp.weixin.qq.com/s/MZo3LFyzXp-qpi5jEOQS5Q

FPT：又是借鉴Transformer，这次多方向融合特征金字塔

https://mp.weixin.qq.com/s/N2PAgp-epq4i9CLll1nzJA

华为联合北大、悉尼大学对Visual Transformer的最新综述

https://mp.weixin.qq.com/s/cLPMJm4u67QDsJg0IkmYFQ

解析Vision Transformer

https://www.zhihu.com/question/437495132

如何看待Transformer在CV上的应用前景，未来有可能替代CNN吗？

https://mp.weixin.qq.com/s/EwzjBAeZQfeBt-EkaeXPAg

搞懂Vision Transformer原理和代码，看这篇技术综述就够了

# BERT进阶

## AR vs AE

自回归模型，是统计上一种处理时间序列的方法，用同一变数例如x的之前各期，亦即$$x_1$$至$$x_{t-1}$$来预测本期$$x_t$$的表现，并假设它们为一线性关系。因为这是从回归分析中的线性回归发展而来，只是不用x预测y，而是用x预测x自己，所以叫做自回归。

----

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
