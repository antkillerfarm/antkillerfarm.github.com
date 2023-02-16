---
layout: post
title:  Attention（六）——ChatGPT
category: Attention 
---

* toc
{:toc}

# Attention in CV & RS（续）

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

https://mp.weixin.qq.com/s/hn4EMcVJuBSjfGxJ_qM3Tw

搞懂Vision Transformer原理和代码，看这篇技术综述就够了

https://mp.weixin.qq.com/s/ozUHHGMqIC0-FRWoNGhVYQ

搞懂Vision Transformer原理和代码，看这篇技术综述就够了（二）

https://mp.weixin.qq.com/s/dysKMpOXAjSRgb5xGDO3FA

搞懂Vision Transformer原理和代码，看这篇技术综述就够了(三)

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

https://mp.weixin.qq.com/s/XHnt5MRa52IeJKK6nfDb8Q

Vision Transformer学习笔记1

https://mp.weixin.qq.com/s/RhBK0szHORt7XHyoMEVnSA

Vision Transformer学习笔记2: Swin Transformer

https://mp.weixin.qq.com/s/faYB3JCoUfTw_zSVxrKJzA

最新“视频Transformer”2022综述

https://github.com/NVIDIA-Merlin/Transformers4Rec

NVIDIA推出的RS库

# ChatGPT

![](/images/img4/ChatGPT.jpg)

![](/images/img4/ChatGPT_2.jpg)

TAMER（Training an Agent Manually via Evaluative Reinforcement，评估式强化人工训练代理）框架将人类标记者引入到Agents的学习循环中，可以通过人类向Agents提供奖励反馈（即指导Agents进行训练），从而快速达到训练任务目标。

![](/images/img4/TAMER.jpg)

这算得上是一种有监督学习+RL了。

利用强化学习在大模型中注入人类的经验，所谓的Reinforcement Learning from Human Feedback(RLHF)，Policy Network输出的多样性及Reward的学习是ChatGPT成功的关键。

---

国内某牛的山寨版本：

https://zhuanlan.zhihu.com/p/605425639

RWKV 14B对比GLM 130B和NeoX 20B，展示RWKV的性能

代码：

https://github.com/BlinkDL/ChatRWKV

RWKV没有使用attention，而是号称100% RNN。

RNN-based没有attention之类机制的模型是怎么获得long memory的能力的啊？

这个形式就是Transformers are RNNs的形式，只不过把Q换成了positional invariant的time weighting。 最近很多work都显示Attention里的Q其实没啥用，换成一个跟着相对位置exponential decay的term就行了。

---

参考：

https://zhuanlan.zhihu.com/p/590655677

ChatGPT特点、原理、技术架构和产业未来

https://zhuanlan.zhihu.com/p/592671478

ChatGPT背后的算法——RLHF

https://mp.weixin.qq.com/s/L8E-dd9988Prbxau5awFtw

ChatGPT怎么突然变得这么强？华人博士万字长文深度拆解GPT-3.5能力起源

https://mp.weixin.qq.com/s/FPws8Gk18pW-TRcorlTczg

ChatGPT研究框架

https://www.zhihu.com/question/575481512

为什么chatgpt的上下文连续对话能力得到了大幅度提升？

## 微软小冰

微软内部之前有个类似ChatGPT 的项目，叫微软小冰，几个负责人都是那种技术栈和技术思路非常老旧的老人，在微软内部吸血吸了很多年，微软后来体面的裁掉了这个团队，转头去投了OpenAI 10亿美金。

这个被裁团队出来之后，一阵包装，说是微软为了他们更好的发展，所以让他们独立出来，然后去vc那融了好多钱。就在去年12月份投资人纷纷对比了ChatGPT和小冰的智能化程度后（对比效果简直辣眼睛，小冰那也叫智能？）

小冰的技术原理，走的是传统nlp原理那一套，已经过时了，没有使用深度学习，基于知识图谱回答，学习的知识非常有限。

ChatGPT的出现打了两种人的脸：一种是对强人工智能保持悲观态度，认为强人工智能很长时间内都不可能出现的；一种是对超大模型持怀疑态度，认为通过超大模型来实现人工智能是错误道路的。

在2022年11月30日之前，市面上有大大小小的互联网或IT企业需要进行文本处理，相应地，也就需要雇佣大量的NLP工程师们来解决相关的问题。

绝大多数的NLP工程师们所做的工程项目，主要是针对某些特定任务提出一个具体的模型，进行有针对性的数据标注，然后再制作模型。简而言之，就是以NLP子任务独立进行研究开发。比如分词、实体识别、文本分类、相似度判别、机器翻译、文摘系统、事件抽取，等等，不一而足。

也就是说，NLP产业界实际上处于一种手工业模式，你干你的，我干我的，针对不同的企业、不同的需求，需要不断地定制模型、定制数据来完成工作。

NLP中，还有一部分内容：知识图谱。知识图谱这个概念专门用来记录现实世界中的客观存在的事务的关联关系，对于 NLP任务也极为重要。更准确地讲，应当叫做领域知识图谱，几乎没有哪个机构可以做出一个通泛的图谱来供应用。

但知识谱图属于有多少人工，就有多少智能的最典型代表。知识图谱做一万年做不到GPT3的水平，就像蒸汽机做的再好也驱动不了登月火箭。

ChatGPT 已经完全抹去了传统NLP业态中，需要分不同子任务、分不同领域数据场景的手工业模式，而是直接采用大模型，以对话形式，直接形成了大一统，进入了机器时代。类似于传统的手工纺织女工，完全由机器替代了。

评价NLP模型的效果，应当从两方面入手：

一方面，是评价模型对自然语言本身的拟合，比如，前后语句连贯、通顺、符合正常人类的叙述习惯，能够理解反问、反讽、情绪、基本世界观的构建和逻辑推断。客观讲，ChatGPT已经做到了极致，它的语言表达水平，比很多人的作文能力都强非常多。

另一方面，是评价模型对知识、事实类信息的拟合，这块能力ChatGPT还无法很好胜任，比如模型会告诉我3+13=23，汪小菲的姐姐是大S，谷歌的CEO是库克，等等，大家也都明白，大量的博客、文章、段子，讨论过很多了，不再赘述。

目前，这些事实、知识，是由知识图谱来解决的。但以笨拙的实体、关系、属性等为基本概念所作的构建，显然是没有前途的。

参考：

https://zhuanlan.zhihu.com/p/158009816

开域聊天机器人-微软小冰的技术介绍（现实篇）

https://mp.weixin.qq.com/s/wBsh9dmMPks04X2pDB8Ang

沈向洋等重磅论文：公开微软小冰系统设计，迄今最详细！

https://www.zhihu.com/question/583134530

微软解散元宇宙团队投资近900亿搞ChatGPT，如何从商业角度解读此举？

https://zhuanlan.zhihu.com/p/605673596

ChatGPT这么强，会影响NLPer的就业环境吗

# BERT进阶

## AR vs AE

自回归模型，是统计上一种处理时间序列的方法，用同一变数例如x的之前各期，亦即$$x_1$$至$$x_{t-1}$$来预测本期$$x_t$$的表现，并假设它们为一线性关系。因为这是从回归分析中的线性回归发展而来，只是不用x预测y，而是用x预测x自己，所以叫做自回归。

---

**AR**: Autoregressive Lanuage Modeling，又叫自回归语言模型。它指的是，依据前面(或后面)出现的tokens来预测当前时刻的token，代表模型有ELMO、GTP等。

$$\text{forward:}p(x)=\prod_{t=1}^Tp(x_t|x_{<t})$$

$$\text{backward:}p(x)=\prod_{t=T}^1p(x_t|x_{>t})$$

- 缺点：它只能利用单向语义而不能同时利用上下文信息。ELMO通过双向都做AR模型，然后进行拼接，但从结果来看，效果并不是太好。

- 优点：对自然语言生成任务(NLG)友好，天然符合生成式任务的生成过程。这也是为什么GPT能够编故事的原因。
