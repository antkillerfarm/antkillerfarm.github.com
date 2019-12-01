---
layout: post
title:  Attention（三）——BERT
category: Attention 
---

# Transformer（续）

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

《Incorporating BERT into Neural Machine Translation》

## 基本结构

![](/images/img3/BERT.png)

上图是BERT的网络结构图。

BERT是Bidirectional Encoder Representations from Transformers的缩写。从这个名字可以看出它将Transformer中的encoder作为一个基本单元，然后采用了类似双向RNN的方式，做了一个双向的Transformer的结构。

## pre-training

BERT的强大，主要不在网络结构上。上面介绍的GPT虽然输给了BERT，但更深、维度更大的GPT 2却赢了BERT，可见单向或者双向的Transformer，并不是问题的关键。让这些模型强大的原因主要在于pre-training。

![](/images/img3/BERT_2.png)


![](/images/img3/BERT_3.png)

![](/images/img3/BERT_4.png)

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

## GPT 2.0

论文：

《Improving Language Understandingby Generative Pre-Training》

![](/images/img3/GPT.png)

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

## ERNIE

https://mp.weixin.qq.com/s/xoQhz6ljbsbzKRBJlTQQuQ

百度提出ERNIE，多项中文NLP任务表现出色

https://mp.weixin.qq.com/s/rQ8ISipvV3Irrjd3MI-Idw

百度ERNIE，中文任务全面超越BERT

https://mp.weixin.qq.com/s/_ZBvq7gXvbiP2IQve9tcKg

清华等提出ERNIE：知识图谱结合BERT才是“有文化”的语言模型

https://mp.weixin.qq.com/s/QVEYQfEQV0CsklI9S4vOiA

ERNIE真有官方说的那么好？亲测告诉你答案！

https://mp.weixin.qq.com/s/FoX2bXCJlFYjb9U6JcZCqg

超详细中文预训练模型ERNIE使用指南

https://mp.weixin.qq.com/s/EYQXM-1WSommj9mKJZVVzw

百度正式发布ERNIE 2.0，16项中英文任务超越BERT、XLNet，刷新SOTA

https://mp.weixin.qq.com/s/PwiVCgN8dDWXTGZsiqM-2g

最新NLP架构的直观解释：多任务学习–ERNIE 2.0

## XLNet

https://mp.weixin.qq.com/s/29y2bg4KE-HNwsimD3aauw

20项任务全面碾压BERT，CMU全新XLNet预训练模型屠榜

https://mp.weixin.qq.com/s/itNtDuQS4KF_sLnfiwdyNg

拆解XLNet模型设计，回顾语言表征学习的思想演进

https://mp.weixin.qq.com/s/2zuR0x-Cb1NTeRHYeTjrHQ

一文详解Google最新NLP模型XLNet

https://mp.weixin.qq.com/s/t8XDCPOYna8mZ1Iqk_g7Zw

最新语言表示方法XLNet

https://zhuanlan.zhihu.com/p/70257427

XLNet:运行机制及和Bert的异同比较

https://mp.weixin.qq.com/s/SAiIIa9_-16dqRMKASsuhw

追溯XLNet的前世今生：从Transformer到XLNet

https://mp.weixin.qq.com/s/qzAN6VlKcfqmpX9kQCJ7Gg

XLnet：GPT和BERT的合体，博采众长，所以更强

https://zhuanlan.zhihu.com/p/80216580

XLnet：集合了GPT和BERT的预训练模型

https://mp.weixin.qq.com/s/7ZTDJmsOxOwJ7fYUxK6eTw

XLNet详解

## 轻量化BERT

https://www.zhihu.com/question/347898375

如何看待瘦身成功版BERT——ALBERT？

https://mp.weixin.qq.com/s/a0d0b1jSm5HxHso9Lz8MSQ

小版BERT也能出奇迹：最火的预训练语言库探索小巧之路

https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650771134&idx=2&sn=012082a897dbf125000e38b73520c51d

TinyBERT：模型小7倍，速度快8倍，华中科大、华为出品

https://mp.weixin.qq.com/s/i82wGiSOlA4l4ozimrE2gg

加速BERT模型有多少种方法？从架构优化、模型压缩到模型蒸馏最新进展详解！

https://mp.weixin.qq.com/s/R2MW_5kskvXyuSOh7kfJaA

ALBERT：更轻更快的NLP预训练模型

https://mp.weixin.qq.com/s/dWzpqP_U8Y5DyfWHVTl5Vg

BERT瘦身之路：Distillation，Quantization，Pruning

https://mp.weixin.qq.com/s/DAsY9-Dl5T6peo_71ICOtw

基于ALBERT的文本相似度计算

https://mp.weixin.qq.com/s/PY6U3cbz_LnvZTkhqMLFLQ

15篇论文全面概览BERT压缩方法

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

https://mp.weixin.qq.com/s/CofeiL4fImq98UeuJ4hWTg

预训练BERT，官方代码发布前他们是这样用TensorFlow解决的

https://mp.weixin.qq.com/s/vFdm-UHns7Nhbmdoiu6jWg

谷歌终于开源BERT代码：3亿参数量，机器之心全面解读

https://mp.weixin.qq.com/s/dV4RkxZOC9o2BxNi0GljKQ

谷歌最强NLP模型BERT官方中文版来了！多语言模型支持100种语言

https://mp.weixin.qq.com/s/fz-bQMAi5bs2_bvRhf3ERg

从Word Embedding到Bert模型—自然语言处理中的预训练技术发展史

https://mp.weixin.qq.com/s/k_33UK1RkMyHn6TSudU6Kg

详解谷歌最强NLP模型BERT

https://mp.weixin.qq.com/s/d2MZQbamdo0EC_MVtf-HZA

BERT详解：开创性自然语言处理框架的全面指南

https://mp.weixin.qq.com/s/pD4it8vQ-aE474uSMQG0YQ

两行代码玩转Google BERT句向量词向量

https://mp.weixin.qq.com/s/osmUZxAAX3x-oTHYJbzemA

谷歌BERT模型fine-tune终极实践教程

https://mp.weixin.qq.com/s/XmeDjHSFI0UsQmKeOgwnyA

小数据福音！BERT在极小数据下带来显著提升的开源实现
