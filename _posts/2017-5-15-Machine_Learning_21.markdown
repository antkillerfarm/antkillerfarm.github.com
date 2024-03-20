---
layout: post
title:  机器学习（二十一）——NLP历史, NLP常用评价度量, Tokenization
category: ML 
---

* toc
{:toc}

# Loss function详解

## Triplet Loss（续）

针对每个样本$$x_i$$，训练一个参数共享或者不共享的网络，得到三个元素的特征表达，分别记为：$$f(x_i^a), f(x_i^p), f(x_i^n)$$。

**triplet loss的目的就是通过学习（即上图中的Learning），让$$x^a$$和$$x^p$$特征表达之间的距离尽可能小，而$$x^a$$和$$x^n$$的特征表达之间的距离尽可能大。**

它的公式化的表示就是：

$$\|f(x_i^a)-f(x_i^p)\|_2^2 + \alpha < \|f(x_i^a)-f(x_i^n)\|_2^2$$

其中，$$\alpha$$表示两个距离之间的间隔。因此，对应的目标函数也就很清楚了：

$$\sum_i^N\left[\|f(x_i^a)-f(x_i^p)\|_2^2 - \|f(x_i^a)-f(x_i^n)\|_2^2 + \alpha \right]_+$$

这里距离用欧式距离度量，+表示[]内的值大于零的时候，取该值为损失，小于零的时候，损失为零。

>需要注意的是Triplet Loss以及后面介绍的各种改进版softmax，其收敛速度不如softmax，因此，先用softmax训练几轮，再改用这些loss，也是常用的调参技巧。

参考：

https://blog.csdn.net/u010167269/article/details/52027378

Triplet Loss、Coupled Cluster Loss探究

https://blog.csdn.net/tangwei2014/article/details/46788025

triplet loss原理以及梯度推导

https://www.zhihu.com/question/62486208

triplet loss在深度学习中主要应用在什么地方？有什么明显的优势？

https://mp.weixin.qq.com/s/XB9VsW3NRwHua6AdRL3n8w

Lossless Triplet Loss:一种高效的Siamese网络损失函数

## 参考

https://mp.weixin.qq.com/s/CPfhGxig9BMAgimBSOLy3g

用于图像检索的等距离等分布三元组损失函数

https://www.zhihu.com/question/375794498

深度学习的多个loss如何平衡？

https://mp.weixin.qq.com/s/tzY_lG0F9dP5Q-LmwuHLmQ

常见损失函数和评价指标总结

https://mp.weixin.qq.com/s/lw9frtqocqsS-q2KGfzO1Q

深入理解计算机视觉中的损失函数

https://mp.weixin.qq.com/s/_HQ5an_krRCYMVnwEgGJow

深度学习的多个loss如何平衡 & 有哪些“魔改”损失函数，曾经拯救了你的深度学习模型？

https://blog.csdn.net/shanglianlm/article/details/85019768

十九种损失函数

https://mp.weixin.qq.com/s/8oKiVRjtPQIH1D2HltsREQ

图像分割损失函数最全面、最详细总结

https://zhuanlan.zhihu.com/p/158853633

一文理解Ranking Loss/Margin Loss/Triplet Loss

https://zhuanlan.zhihu.com/p/235533342

目标检测：Loss整理

https://zhuanlan.zhihu.com/p/191355122

NLP样本不均衡之常用损失函数对比

https://mp.weixin.qq.com/s/KL_D8pWtcXCJHz7dd70jyw

Face Recognition Loss on Mnist with Pytorch

https://zhuanlan.zhihu.com/p/77686118

机器学习常用损失函数小结

https://zhuanlan.zhihu.com/p/38855840

SphereReID：从人脸到行人，Softmax变种效果显著

https://mp.weixin.qq.com/s/ZoLO6OilivPgle03KdNzCQ

人脸识别中Softmax-based Loss的演化史

https://mp.weixin.qq.com/s/DwtA6GivVCDvL4MXNDBFWg

阿里巴巴提出DR Loss：解决目标检测的样本不平衡问题

https://zhuanlan.zhihu.com/p/145927429

DR Loss

https://mp.weixin.qq.com/s/x0aBo-w669_2FyCGKWJ4iQ

理解计算机视觉中的损失函数

https://mp.weixin.qq.com/s/LOewKsxtWm7dFJS6ioryuw

Siamese网络，Triplet Loss以及Circle Loss的解释

https://zhuanlan.zhihu.com/p/58883095

常见的损失函数(loss function)总结

https://mp.weixin.qq.com/s/UjBCjwNDIxDoAoyQAf8V6A

旷视研究院提出Circle Loss，革新深度特征学习范式

https://mp.weixin.qq.com/s/5RpbXzuHp_tR6C_nBdiXGA

Circle Loss：从统一的相似性对的优化角度进行深度特征学习

https://zhuanlan.zhihu.com/p/304462034

根据标签分布来选择损失函数

https://mp.weixin.qq.com/s/Ywzbn2_QqYd1W8dv8cxupw

Seesaw Loss：一种面向长尾目标检测的平衡损失函数

https://mp.weixin.qq.com/s/qJIlbLuM7--wj3fDLUecYw

Pytorch中的四种经典Loss源码解析

https://mp.weixin.qq.com/s/7Jg-YvS3nvcPJ-zYhK96EA

分享神经网络中设计loss function的一些技巧

https://mp.weixin.qq.com/s/cYcztl8N9JF-XXp9xLJIxg

一文道尽softmax loss及其变种

https://mp.weixin.qq.com/s/MTeuRYutMiCmthEAObyAIg

从最优化的角度看待Softmax损失函数

https://zhuanlan.zhihu.com/p/23340343

Center Loss及其在人脸识别中的应用

https://zhuanlan.zhihu.com/p/34404607

人脸识别的LOSS（上）

https://zhuanlan.zhihu.com/p/34436551

人脸识别的LOSS（下）

https://mp.weixin.qq.com/s/kI22wSoyNT3QXXI8pVwbjA

腾讯AI Lab提出新型损失函数LMCL：可显著增强人脸识别模型的判别能力

https://mp.weixin.qq.com/s/8KM7wUg_lnFBd0fIoczTHQ

用收缩损失(Shrinkage Loss)进行深度回归跟踪

https://mp.weixin.qq.com/s/piYyhPbA6kAXuSE5yHfQ1g

人脸识别损失函数综述

https://zhuanlan.zhihu.com/p/60747096

人脸识别损失函数简介与Pytorch实现：ArcFace、SphereFace、CosFace

https://mp.weixin.qq.com/s/_cVNNZBBJljdWBPU9d38CA

常见的损失函数超全总结

https://mp.weixin.qq.com/s/P6xLYrNP4pNKtHcxAWAOKg

数据竞赛中的各种loss function

https://mp.weixin.qq.com/s/Q4ryiTOSJQaJ2e5clmXjtg

一文看尽深度学习中的15种损失函数

# NLP历史

![](/images/img2/text.jpg)

![](/images/img2/NLP_history.png)

第一代技术（1950s）：符号主义，用计算机的符号操作来模拟人的认知过程。

第二代技术（1970s）：语法规则，依赖于专家人工制定的语法规则和本体设计（ontological design）。

第三代技术（1990s）：统计学习，即让计算机阅读大量文章。

第四代技术（2010s）：深度学习，用一个复杂的模型像人脑神经网络一样运作。

![](/images/img2/Han.jpg)

---

http://elinguistics.net/

这个网站可用于比较两种语言的亲缘/差异程度。

https://www.zhihu.com/question/526103427

维吾尔语能否和土耳其语互通？如果能的话，能互通多少？

---

参考：

https://mp.weixin.qq.com/s/1cdg635KcPTV6mFdwPo2OQ

达观数据：文字的起源与文本挖掘的前世今生

https://mp.weixin.qq.com/s/krW1eUFu7iz9YyYFGbFVeQ

香侬科技提出中文字型的深度学习模型Glyce，横扫13项中文NLP记录

https://mp.weixin.qq.com/s/VtEM6paUPH28GFrwVNmz4w

自然语言处理起源：马尔科夫和香农的语言建模实验

https://mp.weixin.qq.com/s/qlKGgWq_FTYonMXEkhRwpw

中国境内语言概览

https://www.zhihu.com/question/30873035

Unicode字符集中有哪些神奇的字符？

# NLP常用评价度量

机器翻译的评价指标主要有：BLEU、NIST、Rouge、METEOR等。

参考：

http://blog.csdn.net/joshuaxx316/article/details/58696552

BLEU，ROUGE，METEOR，ROUGE-浅述自然语言处理机器翻译常用评价度量

http://blog.csdn.net/guolindonggld/article/details/56966200

机器翻译评价指标之BLEU

https://mp.weixin.qq.com/s/niVOM-lnzI2-Tgxn8Qterw

NLP中评价文本输出都有哪些方法？为什么要小心使用BLEU？

http://blog.csdn.net/han_xiaoyang/article/details/10118517

机器翻译评估标准介绍和计算方法

http://blog.csdn.net/lcj369387335/article/details/69845385

自动文档摘要评价方法---Edmundson和ROUGE

https://mp.weixin.qq.com/s/XiZ6Uc5cHZjczn-qoupQnA

对话系统评价方法综述

https://zhuanlan.zhihu.com/p/33088748

数据集和评价指标介绍

https://mp.weixin.qq.com/s/9hoM_yF96XxSbQHEP6oasw

怎样生成语言才能更自然，斯坦福提出超越Perplexity的评估新方法

https://mp.weixin.qq.com/s/TnAZUjFSn7ATtRmBexJlXw

文本生成评价方法BLEU ROUGE CIDEr SPICE Perplexity METEOR

https://mp.weixin.qq.com/s/n4OnRGkrn5DYuZUmmrmzpg

NLG任务评价指标BLEU与ROUGE

https://zhuanlan.zhihu.com/p/445450292

为什么bleu不是一个完美的文本生成评测指标

## LLM评估

https://zhuanlan.zhihu.com/p/677583745

评估大模型快速入门：用MMLU评估 GPT-3

https://zhuanlan.zhihu.com/p/656260901

TruthfulQA: Measuring How Models Mimic Human Falsehoods - 事实性评估

# Tokenization

## Subword

对于英文来说，文字的粒度从细到粗依次是character, subword, word。character和word都很好理解，分别是字母和单词。而subword相当于英文中的词根、前缀、后缀等。

之前的Neural Machine Translation基本上都是基于word单词作为基本单位的，但是其缺点是不能很好的解决out-of-vocabulary即单词不在词汇库里的情况，且对于单词的一些词法上的修饰(morphology)处理的也不是很好。一个自然的想法就是能够利用比word更基本的组成来建立模型，以更好的解决这些问题。

参考：

https://plmsmile.github.io/2017/10/19/subword-units/

subword-units

https://zhuanlan.zhihu.com/p/69414965

Subword模型

https://zhuanlan.zhihu.com/p/86965595

深入理解NLP Subword算法：BPE、WordPiece、ULM

https://mp.weixin.qq.com/s/la3nZNFDviRcSFNVso29oQ

NLP三大Subword模型详解：BPE、WordPiece、ULM

https://mp.weixin.qq.com/s/TPRqDyGpkVuJcgomTu774A

子词技巧：The Tricks of Subword

https://mp.weixin.qq.com/s/fe7-wimFCAtp3ohB3TVywg

通俗讲解Subword Models

https://mp.weixin.qq.com/s/5z3CMmIR0U9-p2BwJnsYKg

神经机器翻译的Subword技术

## BPE

Byte Pair Encoding(BPE)本来是一种数据压缩算法，后来被用于分词。它从命名实体、同根词、外来语、组合词（罕见词有相当大比例是上述几种）的翻译策略中得到启发，认为把这些罕见词拆分为“子词单元”(subword units)的组合，可以有效的缓解NMT的OOV和罕见词翻译的问题。BPE对英文、德文、俄文等表音文字效果较好，但不太适用于中文。

论文：

《Neural Machine Translation of Rare Words with Subword Units》

代码：

https://github.com/rsennrich/subword-nmt

![](/images/img5/Tokenizer.webp)

参考：

https://www.cnblogs.com/huangyc/p/10223075.html

一文读懂BERT中的WordPiece

https://mp.weixin.qq.com/s/OGBk_ZptFzbjKdnv2RVZFA

机器如何认识文本？NLP中的Tokenization方法总结
