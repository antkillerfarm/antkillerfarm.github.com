---
layout: post
title:  深度学习（五十）——深度推理, DNC, Transformer
category: DL 
---

# 深度推理

https://mp.weixin.qq.com/s/rE96BprEO1ZatVOQeO-4YA

因果推理学习算法资源大列表

https://mp.weixin.qq.com/s/RS39wqZDf6gEhOY6gcOVSA

74页教程融合逻辑推理和深度学习

https://mp.weixin.qq.com/s/NcUHJ89DPVdB7cJttsh-xw

基于神经网络的知识推理

https://mp.weixin.qq.com/s/ouHPvm4vQKga5sZfG_CHBw

DeepMind用深度学习模仿大脑推理，预测编码智能推进一大步！

https://zhuanlan.zhihu.com/p/28654835

视觉推理（Visual Reasoning），神经网络也可以有逻辑

https://mp.weixin.qq.com/s/HP8KLbo26W5UscCGavKDRw

IBM Watson提出人机推理网络HuMaINs，结合人机两者优势

https://mp.weixin.qq.com/s/210l9K94KMqtJtqbrZ-mgg

打开黑箱重要一步，MIT提出TbD-net，弥合视觉推理模型的性能与可解释性鸿沟

https://mp.weixin.qq.com/s/7NpJJn4k5oGPOB04S9Jyqw

DeepMind新论文，关联推理为什么是智能最重要的特征

http://mp.weixin.qq.com/s/6I0Z_yY7UT_7qOKOWcd7Mw

李飞飞发表研究新成果：视觉推理的推断和执行程序！

http://mp.weixin.qq.com/s/9mtgdNynv92FC2dA8-5KJA

VAE和Adam发明人博士论文：变分推理和深度学习

https://mp.weixin.qq.com/s/sbV5SL6fAGad5KlBoqUKFQ

斯坦福大学教授Christopher Manning提出全可微神经网络架构MAC：可用于机器推理

https://mp.weixin.qq.com/s/5vDSrWeUvlBuZgmX0R9pBQ

斯坦福“黑盒学习”研究：使用神经变分推理的无向图模型，可替代“采样”

https://mp.weixin.qq.com/s/XESTrLMERQPR9eXcFA56gg

神经符号系统：让机器善解人意

https://mp.weixin.qq.com/s/GYe1psxy1KMCbV7f3K8f4Q

神经规则引擎：让符号规则学会变通

https://mp.weixin.qq.com/s/-qc2MENppQmdClaIKV6Lww

深度推理学习中的图网络与关系表征

https://mp.weixin.qq.com/s/_FHXhfZ7TEpBzVf5kB0BsA

为思想“层次”建模，递归推理让AI更聪明

# LSM

liquid state machine (LSM)

http://www.docin.com/p-390935406.html

基于液体状态机的脑运动神经系统的建模研究

# DNC

https://zhuanlan.zhihu.com/p/27773709

浅析至强RNN可微分神经计算机(DNC)

https://zhuanlan.zhihu.com/p/27964341

浅析至强RNN可微分神经计算机(DNC)-2

https://zhuanlan.zhihu.com/p/28209628

DNC-3滚动分类的模式识别

https://zhuanlan.zhihu.com/p/28433712

DNC4广义线性回归

# Transformer

之前的文章已经介绍了Attention和《Attention is All You Need》。但实际上，《Attention is All You Need》不仅提出了两种Attention模块，而且还提出了如下图所示的Transformer模型。该模型主要用于NMT领域，由于Attention不依赖上一刻的数据，同时精度也不弱于LSTM，因此有很好并行计算特性，在工业界得到了广泛应用。阿里巴巴和搜狗目前的NMT方案都是基于Transformer模型的。

![](/images/img2/Transformer.png)

$$FFN(x) = \max(0,xW_1 + b_1)W_2 + b_2$$

代码：

https://github.com/Kyubyong/transformer

参考：

http://jalammar.github.io/illustrated-transformer/

The Illustrated Transformer

http://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/

Visualizing A Neural Machine Translation Model (Mechanics of Seq2seq Models With Attention)

https://zhuanlan.zhihu.com/p/39034683

Attention is all you need模型笔记

https://zhuanlan.zhihu.com/p/40920384

真正的完全图解Seq2Seq Attention模型

https://mp.weixin.qq.com/s/RLxWevVWHXgX-UcoxDS70w

细讲《Attention Is All You Need》

https://mp.weixin.qq.com/s/HquT_mKm7x_rbDGz4Voqpw

阿里巴巴最新实践：TVM+TensorFlow提高神经机器翻译性能

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
