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

官网：

https://chat.openai.com/chat

ChatGPT本身并没有论文，大部分是基于InstructGPT这篇论文：

https://openai.com/blog/instruction-following/

Aligning Language Models to Follow Instructions

![](/images/img4/ChatGPT.jpg)

![](/images/img4/ChatGPT_2.jpg)

![](/images/img5/GPT.jpg)

TAMER（Training an Agent Manually via Evaluative Reinforcement，评估式强化人工训练代理）框架将人类标记者引入到Agents的学习循环中，可以通过人类向Agents提供奖励反馈（即指导Agents进行训练），从而快速达到训练任务目标。

![](/images/img4/TAMER.jpg)

这算得上是一种有监督学习+RL了。

![](/images/img5/RLHF.png)

利用强化学习在大模型中注入人类的经验，所谓的Reinforcement Learning from Human Feedback(RLHF)，Policy Network输出的多样性及Reward的学习是ChatGPT成功的关键。

---

基座预训练（Base pretrain）

SFT微调（Supervised Fine-Tuning）

奖励函数训练（Reward Modeling, RM），最常用的是基于排序的奖励函数建模（Ranking-Based Reward Modeling，RBRM）

基于人类反馈的强化学习（RLHF，基于RM/RBRM进行PPO强化学习训练）

与人类对齐（Align AI with human values）

https://zhuanlan.zhihu.com/p/614987279

ChatGPT的训练过程术语整理

---

模型数据集可分为六类，分别是：维基百科、书籍、期刊、Reddit链接、Common Crawl和其他数据集。

![](/images/img5/huge_data.jpg)

上图展示了各大模型所使用的数据来源。

数据语言: 主要是英语~46%, 俄, 德, 日与中文都是~5%左右。

除了文本之外，ChatGPT还利用了github的代码进行训练。有一些研究者认为ChatGPT展现出了初级推理能力，要归功于其使用代码作为语言数据训练，这些进化出的初级逻辑思维链，在中文上也有体现。

---

国内某牛的山寨版本：

https://zhuanlan.zhihu.com/p/605425639

RWKV 14B对比GLM 130B和NeoX 20B，展示RWKV的性能

代码：

https://github.com/BlinkDL/ChatRWKV

RWKV没有使用attention，而是号称100% RNN。

RNN-based没有attention之类机制的模型是怎么获得long memory的能力的啊？

这个形式就是Transformers are RNNs的形式，只不过把Q换成了positional invariant的time weighting。最近很多work都显示Attention里的Q其实没啥用，换成一个跟着相对位置exponential decay的term就行了。

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

https://www.zhihu.com/question/560134313

为什么强化学习里很少有预训练模型（Pretrained Model）？

https://www.zhihu.com/question/570431477

ChatGPT的训练集来自哪里？

https://www.zhihu.com/question/570189639

如何评价OpenAI的超级对话模型ChatGPT？

https://mp.weixin.qq.com/s/_ipboVExouPNtGhvKnJX_g

ChatGPT的技术体系总结

https://mp.weixin.qq.com/s/1fSxWdgfyAaFPBXMGhBV2Q

且看Chat GPT如何应对职场PUA

https://zhuanlan.zhihu.com/p/607847588

复现ChatGPT的难点与平替

https://zhuanlan.zhihu.com/p/601044938

nanoGPT源码阅读

https://zhuanlan.zhihu.com/p/608705255

GPT-3+RL全流程训练开源整理

## 微软小冰

微软内部之前有个类似ChatGPT的项目，叫微软小冰，几个负责人都是那种技术栈和技术思路非常老旧的老人，在微软内部吸血吸了很多年，微软后来体面的裁掉了这个团队，转头去投了OpenAI 10亿美金。

这个被裁团队出来之后，一阵包装，说是微软为了他们更好的发展，所以让他们独立出来，然后去vc那融了好多钱。就在去年12月份投资人纷纷对比了ChatGPT和小冰的智能化程度。对比效果简直辣眼睛，小冰那也叫智能？

小冰的技术原理，走的是传统NLP原理那一套，已经过时了，没有使用深度学习，基于知识图谱回答，学习的知识非常有限。

ChatGPT的出现打了两种人的脸：一种是对强人工智能保持悲观态度，认为强人工智能很长时间内都不可能出现的；一种是对超大模型持怀疑态度，认为通过超大模型来实现人工智能是错误道路的。

在2022年11月30日之前，市面上有大大小小的互联网或IT企业需要进行文本处理，相应地，也就需要雇佣大量的NLP工程师们来解决相关的问题。

绝大多数的NLP工程师们所做的工程项目，主要是针对某些特定任务提出一个具体的模型，进行有针对性的数据标注，然后再制作模型。简而言之，就是以NLP子任务独立进行研究开发。比如分词、实体识别、文本分类、相似度判别、机器翻译、文摘系统、事件抽取，等等，不一而足。

也就是说，NLP产业界实际上处于一种手工业模式，你干你的，我干我的，针对不同的企业、不同的需求，需要不断地定制模型、定制数据来完成工作。

NLP中，还有一部分内容：知识图谱。知识图谱这个概念专门用来记录现实世界中的客观存在的事务的关联关系，对于 NLP任务也极为重要。更准确地讲，应当叫做领域知识图谱，几乎没有哪个机构可以做出一个通泛的图谱来供应用。

但知识谱图属于有多少人工，就有多少智能的最典型代表。知识图谱做一万年做不到GPT3的水平，就像蒸汽机做的再好也驱动不了登月火箭。

ChatGPT已经完全抹去了传统NLP业态中，需要分不同子任务、分不同领域数据场景的手工业模式，而是直接采用大模型，以对话形式，直接形成了大一统，进入了机器时代。类似于传统的手工纺织女工，完全由机器替代了。很多NLP子领域不再具备独立研究价值。

评价NLP模型的效果，应当从两方面入手：

一方面，是评价模型对自然语言本身的拟合，比如，前后语句连贯、通顺、符合正常人类的叙述习惯，能够理解反问、反讽、情绪、基本世界观的构建和逻辑推断。客观讲，ChatGPT已经做到了极致，它的语言表达水平，比很多人的作文能力都强非常多。

另一方面，是评价模型对知识、事实类信息的拟合，这块能力ChatGPT还无法很好胜任，比如模型会告诉我3+13=23，汪小菲的姐姐是大S，谷歌的CEO是库克，等等，大家也都明白，大量的博客、文章、段子，讨论过很多了，不再赘述。

目前，这些事实、知识，是由知识图谱来解决的。但以笨拙的实体、关系、属性等为基本概念所作的构建，显然是没有前途的。
