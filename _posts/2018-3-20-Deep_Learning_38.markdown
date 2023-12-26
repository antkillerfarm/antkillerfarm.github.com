---
layout: post
title:  深度学习（三十八）——RNN进阶, 显著性检测
category: DL 
---

* toc
{:toc}

# RNN进阶

## IndRNN

https://mp.weixin.qq.com/s/cAqpclkkeVrTiifz07HC1g

新型循环神经网络IndRNN：可构建更长更深的RNN

https://mp.weixin.qq.com/s/7-K-nZTijoYCaprRNYXxFg

新型RNN：将层内神经元相互独立以提高长程记忆

## ODE

https://mp.weixin.qq.com/s/4CrPGKnR7RLN-2ROG5X4uw

ODE网络：一场颠覆RNN的革命即将到来

https://mp.weixin.qq.com/s/0vju0Q_DcIWwdEo9EEE3iQ

NeurIPS18最佳论文NeuralODE，现在有了TensorFlow实现

https://mp.weixin.qq.com/s/i6VEYjbac4QP3s51meN1VA

Hinton向量学院推出神经ODE：超越ResNet 4大性能优势

https://mp.weixin.qq.com/s/ZEIsyV-0aTvYn6K8GyANPA

硬核NeruIPS 2018最佳论文，一个神经了的常微分方程

https://mp.weixin.qq.com/s/uAiRTeYkZKy9q3d9v3dR5A

神经微分方程--钢琴和小提琴

## 参考

https://mp.weixin.qq.com/s/0TLaC8ACXAFEK5aMNK9O-Q

简单循环单元SRU：像CNN一样快速训练RNN

https://zhuanlan.zhihu.com/p/27104240

CW-RNN收益率时间序列回归

https://mp.weixin.qq.com/s/SeR_zNZTu4t7kqB6ltNrmQ

从循环到卷积，探索序列建模的奥秘

https://mp.weixin.qq.com/s/_q69BV1r46S9X5wnLuFPSw

关于序列建模，是时候抛弃RNN和LSTM了

https://mp.weixin.qq.com/s/m5GRNp6qDfVfC0mkQ4m4Yw

神经语言模型如何利用上下文信息：长距离上下文的词序并不重要

https://mp.weixin.qq.com/s/kuoUnt2Vhz9NhfnNqMFAhQ

DeepMind提出关系RNN：构建关系推理模块，强化学习利器

https://mp.weixin.qq.com/s/wfOzCxe3L2t11VguYLGC9Q

上海交大搞出SRNN，比普通RNN也就快135倍

https://mp.weixin.qq.com/s/f0sv7c-H5o5L_wy2sUonUQ

CNN取代RNN？当序列建模不再需要循环网络

https://mp.weixin.qq.com/s/h3fF6Zvr1rSzSMpqdu8B0A

电子科大提出BT-RNN：替代全连接操作而大幅度提升LSTM效率

https://mp.weixin.qq.com/s/OgN4rVDKH5WABIaRY7CHog

如何让RNN神经元拥有基础通用的注意力能力

https://mp.weixin.qq.com/s/KBLCrupGIuPa5nVrxcS5WQ

新研究将GRU简化成单门架构，或更适用于语音识别

https://mp.weixin.qq.com/s/kQozftKd_n_kYIF7KKCc8g

短视频那么多，快手如何利用GRU实现各种炫酷的语音应用

https://mp.weixin.qq.com/s/xwuM2Vj8G7UyuEyzTyO13A

将CNN与RNN组合使用，天才还是错乱？

https://mp.weixin.qq.com/s/c7XkzjLH1n5EtqdQik618g

Dropout在RNN中的应用综述

https://mp.weixin.qq.com/s/K6LK47_GCTeZJPAW0-Xp4Q

多伦多大学提出可逆RNN：内存大降，性能不减！

https://mp.weixin.qq.com/s/lvaWx7J4HFTvYxy7-B9vYg

周志华等提出RNN可解释性方法，看看RNN内部都干了些什么

https://mp.weixin.qq.com/s/YbdiEHb8ld1pp1ehgBzTOQ

将未来信息作为正则项，Twin Networks加强RNN对长期依赖的建模能力

https://mp.weixin.qq.com/s/ty8RyPREo_EA7O8vA2pQuQ

AI编曲震撼人心，RNN生成流行音乐

https://mp.weixin.qq.com/s/vIL-bKHZK-6eXZYWxrc9vw

这种有序神经元，像你熟知的循环神经网络吗？

https://mp.weixin.qq.com/s/GGK9T0DeyIdD5ahHy5uvfg

LightRNN：存储和计算高效的RNN

https://mp.weixin.qq.com/s/JGZpKSF5HPCMCD061jwq9A

Bengio等人提出新型循环架构，大幅提升模型泛化性能

https://mp.weixin.qq.com/s/GN0m5nWuV6VDYsTk0XLoDA

pytorch中如何处理RNN输入变长序列padding

https://mp.weixin.qq.com/s/bts9mdIrGIjO8UCUxSV-xg

Transformer的潜在竞争对手QRNN论文解读，训练更快的RNN

# 显著性检测

视觉显著性检测(Visual Saliency Detection)指通过智能算法模拟人的视觉特点，提取图像中的显著区域(即人类感兴趣的区域)。

https://blog.csdn.net/dawnlooo

一个显著性检测的专栏

https://mp.weixin.qq.com/s/Mi62oqtXUT5If_Dj4KmVYA

计算机视觉如何知道你想看什么？个人显著性检测

https://mp.weixin.qq.com/s/47TcGoasB9E_Et2zwl3OCw

全局对比度的图像显著性检测算法

https://zhuanlan.zhihu.com/p/65307842

快、好、实现简单并且开源的显著性检测方法

https://mp.weixin.qq.com/s/tmp1HXU7cLerLr0DY9NluQ

杂乱环境下的显著性物体： 将显著性物体检测推向新高度

https://mp.weixin.qq.com/s/urgkUcu2ZWQMGPZdArWzYg

PoolNet：基于池化技术的显著性目标检测

https://zhuanlan.zhihu.com/p/71538356

BASNet，一种能关注边缘的显著性检测算法

https://mp.weixin.qq.com/s/ntSH2aS4YHqrLaTAfWFLsQ

可选择性与不变性：关注边界的显著性目标检测

https://mp.weixin.qq.com/s/0T1QhiT_20BrerNcTjKreQ

南开提出边缘引导的显著目标检测算法EGNet，刷新主流数据集所有评价指标

https://mp.weixin.qq.com/s/p4lHnte3FYu6XtD3PnSeKw

光场显著性检测研究综述

https://mp.weixin.qq.com/s/8QrNvb-1zmrTWo5zThpyvg

U²-Net：使用显著性物体检测来生成真实的铅笔肖像画

# Attention进阶+

https://mp.weixin.qq.com/s/CftkSOmAx0UTtCixdxj6_A

深度学习中的注意力机制（完结篇）

https://mp.weixin.qq.com/s/Qs6tm50YvzHaJv2rh60WMw

撩一发深度文本分类之RNN via Attention

https://mp.weixin.qq.com/s/MMIZGHTKM5FrvNE6ucQRYQ

33页最新《自然语言处理中神经注意力机制综述》论文

https://mp.weixin.qq.com/s/Q0Ft5bWTuiZUIQSTk7X6ZQ

图解神经机器翻译中的注意力机制

https://mp.weixin.qq.com/s/D7GQ8DRzss9ppP6pyAs1qA

从0到1再读注意力机制

https://mp.weixin.qq.com/s/K_VRt0B9-Xw7YJndmb4WZg

Attention！注意力机制模型最新综述

https://mp.weixin.qq.com/s/hzwp5oGspdtDyNBmq8sMsw

HAN：基于双层注意力机制的异质图深度神经网络

https://mp.weixin.qq.com/s/SBrLPZjx2RdBwZpPQQ5DXQ

HAN：异构图注意力网络

https://mp.weixin.qq.com/s/0EDN-ILeL_diZ1G11ikwjw

T5模型：NLP Text-to-Text预训练模型超大规模探索

https://zhuanlan.zhihu.com/p/89719631

T5: Text-to-Text Transfer Transformer阅读笔记

https://mp.weixin.qq.com/s/X1mLXPzJU7k_ANMzvVPxjA

BERT、RoBERTa、DistilBERT与XLNet，我们到底该如何选择？

https://mp.weixin.qq.com/s/GGRORF5EfJ5xzMLwAsJt5w

从词袋到Transfomer，NLP十年突破史

https://zhuanlan.zhihu.com/p/125145283

Rethink深度学习中的Attention机制

https://mp.weixin.qq.com/s/fxEg8UOa3MeJ6qx5SjEHog

NLP领域中各式各样Attention知识系统性的梳理和总结

https://mp.weixin.qq.com/s/_5YaZdYa8bTFiAzHyrMFBg

理解卷积神经网络中的自注意力机制

https://mp.weixin.qq.com/s/y_hIhdJ1EN7D3p2PVaoZwA

阿里北大提出新attention建模框架，一个模型预测多种行为

https://mp.weixin.qq.com/s/Yq3S4WrsQRQC06GvRgGjTQ

打入神经网络思维内部

https://mp.weixin.qq.com/s/MJ1578NdTKbjU-j3Uuo9Ww

基于文档级问答任务的新注意力模型

https://mp.weixin.qq.com/s/_3pA8FZwzegSpyz_cK63BQ

Self-Attention GAN中的self-attention机制

https://mp.weixin.qq.com/s/l4HN0_VzaiO-DwtNp9cLVA

循环注意力区域实现图像多标签分类

https://mp.weixin.qq.com/s/zhZLK4pgJzQXN49YkYnSjA

自适应注意力机制在Image Caption中的应用

https://mp.weixin.qq.com/s/uvr-G5-_lKpyfyn5g7ES0w

基于注意力机制，机器之心带你理解与训练神经机器翻译系统

https://mp.weixin.qq.com/s/ANpBFnsLXTIiW6WHzGrv2g

自注意力机制学习句子embedding

https://mp.weixin.qq.com/s/49fQX8yiOIwDyof3PD01rA

CMU&谷歌大脑提出新型问答模型QANet：仅使用卷积和自注意力，性能大大优于RNN

https://mp.weixin.qq.com/s/c64XucML13OwI26_UE9xDQ

滴滴披露语音识别新进展：基于Attention显著提升中文识别率

https://mp.weixin.qq.com/s/7OYY3L7gL4wVv_EjoosOHA

如何增强Attention Model的推理能力

https://mp.weixin.qq.com/s/9Kt6_DfeYRnhsb10aCSFGw

FAGAN：完全注意力机制（Full Attention）GAN，Self-attention+GAN

https://mp.weixin.qq.com/s/lZOIK5BRXZrmL_Z9crl6sA

机器翻译新突破！“普适注意力”模型：概念简单参数少，性能大增

https://mp.weixin.qq.com/s/jRfOzKO6OlQLokIzipbqUQ

为什么使用自注意力机制？

https://zhuanlan.zhihu.com/p/339123850

关于attention机制的一些细节的思考

https://mp.weixin.qq.com/s/n4mzHSweOT-vDWBGs0XFbw

卷积神经网络中的自我注意

https://mp.weixin.qq.com/s/h7sLwVXb_UI8jvJU-oe3Cg

Google AI提出“透明注意力”机制，实现更深层NMT模型

https://mp.weixin.qq.com/s/1LYz5SH5rVnPPJ0tZvRQAA

从各种注意力机制窥探深度学习在NLP中的神威

https://zhuanlan.zhihu.com/p/33078323

数字串识别：基于位置的硬性注意力机制

https://mp.weixin.qq.com/s/-gAISWjSiG6ccPuOPAEg3A

五张动图，看清神经机器翻译里的Attention！

https://mp.weixin.qq.com/s/aixpv9t1PLPRWUP6PvZ0EQ

用自注意力增强卷积：这是新老两代神经网络的对话

https://mp.weixin.qq.com/s/i3Xd_IB7R0-QPztn-pgpng

遍地开花的Attention，你真的懂吗？

https://zhuanlan.zhihu.com/p/151640509

注意力机制在推荐系统中的应用

https://mp.weixin.qq.com/s/-SU5cNbklI31WLmTawZJIQ

自注意模型学不好？这个方法帮你解决！

https://mp.weixin.qq.com/s/K5EbO0djcXHN4K5LQiMh5g

Triplet Attention机制让Channel和Spatial交互更加丰富

https://mp.weixin.qq.com/s/C4f0N_bVWU9YPY34t-HAEA

UNC&Adobe提出模块化注意力模型MAttNet，解决指示表达的理解问题

https://mp.weixin.qq.com/s/V3brXuey7Gear0f_KAdq2A

基于注意力机制的交易上下文感知推荐，悉尼科技大学和电子科技大学最新工作

https://mp.weixin.qq.com/s/2gxp7A38epQWoy7wK8Nl6A

谷歌翻译最新突破，“关注机制”让机器读懂词与词的联系

https://zhuanlan.zhihu.com/p/25928551

用深度学习（CNN RNN Attention）解决大规模文本分类问题-综述和实践
