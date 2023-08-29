---
layout: post
title:  Attention（五）——XLNet, 轻量化BERT, Attention in CV & RS
category: Attention 
---

* toc
{:toc}

# ERNIE（续）

https://mp.weixin.qq.com/s/FoX2bXCJlFYjb9U6JcZCqg

超详细中文预训练模型ERNIE使用指南

https://mp.weixin.qq.com/s/EYQXM-1WSommj9mKJZVVzw

百度正式发布ERNIE 2.0，16项中英文任务超越BERT、XLNet，刷新SOTA

https://mp.weixin.qq.com/s/PwiVCgN8dDWXTGZsiqM-2g

最新NLP架构的直观解释：多任务学习–ERNIE 2.0

https://mp.weixin.qq.com/s/yZvKMaBZyodr8SLvcAn7Mg

深度剖析知识增强语义表示模型——ERNIE

https://mp.weixin.qq.com/s/mPPnPwAvPzvJtv0B8jjDbw

详解ERNIE-Baidu进化史及应用场景

https://mp.weixin.qq.com/s/r5Vk_hc5-jsZ69y0dfqIfg

登顶GLUE的百度ERNIE再突破：语言生成预训练模型ERNIE-GEN刷新SOTA

# XLNet

https://mp.weixin.qq.com/s/29y2bg4KE-HNwsimD3aauw

20项任务全面碾压BERT，CMU全新XLNet预训练模型屠榜

https://mp.weixin.qq.com/s/wlV8UbOUYSmc-AdaGfQQMQ

XLNet详解

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

https://zhuanlan.zhihu.com/p/107350079

什么是XLNet，它为什么比BERT效果好？

https://mp.weixin.qq.com/s/EozsQNQ2YrczRg18hTZBhA

什么是XLNet中的双流自注意力

https://mp.weixin.qq.com/s/VfytCWa-h8CmUZW1RWAdnQ

从XLNet的多流机制看最新预训练模型的研究进展

https://mp.weixin.qq.com/s/LurjTAjq4bVxDxIefSxcwg

XLNET：换一个思路做预训练，效果杠杠滴

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

https://mp.weixin.qq.com/s/u44jtXwNCzsZcrr9WrytLQ

EdgeBERT：极限压缩，比ALBERT再轻13倍！树莓派上跑BERT的日子要来了？

https://mp.weixin.qq.com/s/sNv9UirZJ6xT3zf8XhJaRg

FastFormers：实现Transformers在CPU上223倍的推理加速

https://mp.weixin.qq.com/s/XQSEg2_8_1lFqWdHVG6TVA

Switch Transformer: 高效稀疏的万亿参数Transformer

https://zhuanlan.zhihu.com/p/362525526

深入解读首个万亿级语言模型Switch Transformer

https://mp.weixin.qq.com/s/6RUvMR-fjzB5PkZBQ4YFNQ

BERT模型压缩：量化、剪枝和蒸馏

https://zhuanlan.zhihu.com/p/576495529

Fast and Effective！一文速览轻量化Transformer各领域研究进展

# 快速BERT

轻量化BERT是从计算量/时间/空间的角度出发，对于传统BERT的优化。而快速BERT主要着眼于软件工程角度，如何更好的利用各种硬件加速BERT的计算。典型的有NVIDIA的FasterTransformer和腾讯的TurboTransformer。

![](/images/img5/QKV_cache.png)

例如上图就是生成token的过程中，对于KV进行cache的一种优化方法。

https://mp.weixin.qq.com/s/1R_plHqxTLE-Fw3TjYnlJQ

GPU BERT上线性能不合格，看看微信AI的PPoPP论文

https://mp.weixin.qq.com/s/OgTQ3O_6lvOG07U-tjpTDA

如何让Transformer在GPU上跑得更快？快手：需要GPU底层优化

https://www.zhihu.com/question/611236756

FlashAttention的速度优化原理是怎样的？

https://zhuanlan.zhihu.com/p/638468472

从FlashAttention到PagedAttention, 如何进一步优化Attention性能

# Linformer

https://mp.weixin.qq.com/s/IOc-gxOa6a415Hf1VBmiQw

“Linformer”拍了拍“被吊打Transformers的后浪们”

https://mp.weixin.qq.com/s/cDQW5992hTaeGoA7zL7Vzg

Linformer: 线性复杂度的Attention

Self-Attention 加速方法一览：ISSA、CCNet、CGNL、Linformer

# Attention in CV & RS

《Attention Mechanisms in Computer Vision: A Survey》

https://github.com/MenghaoGuo/Awesome-Vision-Attentions

![](/images/img4/Attention.png)

和Normalization一样，Attention应用于CV领域也有不同的花式。

![](/images/img4/Attention_2.png)

![](/images/img4/Attention_3.png)

![](/images/img4/Attention_4.png)

## ViT

上面这些主要还局限于Layer级别的替换，在这里Attention无非是Conv的平替而已。

而下面的ViT则是从体系层面对CNN的一种颠覆了。

![](/images/img5/ViT.png)

ViT借鉴了NLP的一些做法，将图片分成若干小块，每一块就是一个词向量。这样就把一个CV问题变成了NLP问题。

## Masked Autoencoders

![](/images/img4/MAE.png)

何恺明的MAE也是类似的套路。

https://mp.weixin.qq.com/s/x-ruExbM9T8EIv2gZW0Nnw

视觉预训练新范式MAE

https://www.zhihu.com/question/498364155

如何看待何恺明最新一作论文Masked Autoencoders？

https://mp.weixin.qq.com/s/CxEvEZ9AiEfB1TFFjev0aA

NLP和CV的双子星，注入Mask的预训练模型BERT和MAE

## Swin Transformer

![](/images/img5/Swin_Transformer.png)

Swin Transformer的改进：

1.考虑了不同的分辨率。

2.除了分类之外，还可以接入目标检测等后续任务。

3.对MSA（multi-head self attention）进行了改进。提出了W-MSA和SW-MSA，也就是Window版本的MSA和滑动Window版的MSA。

https://mp.weixin.qq.com/s/t_J0MODtWzfnJse0aNGyWg

Swin Transformer对CNN的降维打击

https://mp.weixin.qq.com/s/8x1pgRLWaMkFSjT7zjhTgQ

图解swin transformer

https://zhuanlan.zhihu.com/p/361366090

CV+Transformer之Swin Transformer

https://mp.weixin.qq.com/s/z_ILLmNx3cbJI1B-M028vQ

Swin Transformer重磅升级！Swin V2：向更大容量、更高分辨率的更大模型迈进

## DETR

https://mp.weixin.qq.com/s/b5Ont9vHPeCPnAjuDGv5Bg

Facebook开源新思路！DETR：用Transformers来进行端到端的目标检测

https://mp.weixin.qq.com/s/eHZGiyeZG36Dg6JV1boEeA

语言模型“不务正业”做起目标检测，性能还比DETR、Faster R-CNN更好

## BEVFormer

BEV（Bird's-eye-view）

Deformable Attention

GridSample

https://zhuanlan.zhihu.com/p/543335939

万字长文理解纯视觉感知算法——BEVFormer

https://zhuanlan.zhihu.com/p/538490215

一文读懂BEVFormer论文

https://zhuanlan.zhihu.com/p/629792598

一文看懂BEVFormer技术及其背后价值

https://www.zhihu.com/question/521842610

自动驾驶BEV感知有哪些让人眼前一亮的新方法？

https://zhuanlan.zhihu.com/p/509207308

特斯拉AI DAY感知详解
