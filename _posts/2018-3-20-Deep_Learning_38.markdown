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

# 并行 & 框架 & 优化+

https://mp.weixin.qq.com/s/r951Iasr4dke6MPHsUO0TA

开源DAWN，Stanford的又一力作

https://mp.weixin.qq.com/s/2jrMDeMcb47zpPfFLEcnIA

深度学习平台技术演进

https://mp.weixin.qq.com/s/L4CMKS53pNyvhhqvQhja0g

5种商业AI产品的技术架构设计

https://mp.weixin.qq.com/s/IqjKdAlGYREqCR9XQB5N1A

伯克利AI分布式框架Ray，兼容TensorFlow、PyTorch与MXNet

https://mp.weixin.qq.com/s/aNX_8UDYI_0u-MwMTYeqdQ

开发易、通用难，深度学习框架何时才能飞入寻常百姓家？

https://mp.weixin.qq.com/s/UbAHB-uEIvqYZCB7xIAJTg

机器学习新框架Propel：使用JavaScript做可微分编程

https://mp.weixin.qq.com/s/Ctl65r4iZNEOBxiiX2I2eQ

Momenta王晋玮：让深度学习更高效运行的两个视角

https://zhuanlan.zhihu.com/p/371499074

OneFlow——让每一位算法工程师都有能力训练GPT

https://mp.weixin.qq.com/s/LjdHBEyQhJq3ptMj8XVT-w

TensorFlow在推荐系统中的分布式训练优化实践

https://mp.weixin.qq.com/s/rEHhf32L09KXGJ9bbB2LEA

TensorFlow在美团外卖推荐场景的GPU训练优化实践

https://zhuanlan.zhihu.com/p/522759214

手把手推导分布式矩阵乘的最优并行策略

https://mp.weixin.qq.com/s/_o7fzCOeuZE6qFc5gHb26g

美团视觉GPU推理服务部署架构优化实践

https://zhuanlan.zhihu.com/p/611325149

大型语言模型(LLM)训练指南

https://zhuanlan.zhihu.com/p/629589593

LLM应用开发全栈指南

https://mp.weixin.qq.com/s/UxN9ZRmKLN30s7uPqMpHPQ

Jeff Dean等提出动态控制流编程模型，大规模机器学习性能提升21%

https://mp.weixin.qq.com/s/fx0Pfu0MOPjSkzi5mL6U_A

清华&斯坦福提出深度梯度压缩DGC，大幅降低分布式训练网络带宽需求

https://mp.weixin.qq.com/s/wIdTDHEPffWqHA3_XWBLyw

没错，纯SQL查询语句可以实现神经网络。

>SQL跑神经网络固然没有太大意义，然而分布式数据库已经有数十年的历史，对于设计分布式深度学习框架亦有重大的启发意义。

https://mp.weixin.qq.com/s/F10UaaoxGPOE4pc59LBCRw

数据并行化对神经网络训练有何影响？谷歌大脑进行了实证研究

https://mp.weixin.qq.com/s/UF7DDenUQJ3bL83IHxOkIw

分布式优化算法及其在多智能体系统与机器学习中的应用

https://mp.weixin.qq.com/s/6h9MeBs89hTtWsYSZ4pZ5g

蚂蚁金服核心技术：百亿特征实时推荐算法揭秘

https://mp.weixin.qq.com/s/xV5cLbCPb7Nh6i4i7DxJIQ

没人告诉你的大规模部署AI高效流程！

https://mp.weixin.qq.com/s/8R7YhcZ_Dt0oFIF3bQovxw

为了提升DL模型性能，阿里工程师打造了流式编程框架

https://mp.weixin.qq.com/s/z6gXp-EeDID1ed8_DsUbOg

90秒训练AlexNet！商汤刷新纪录

https://mp.weixin.qq.com/s/HQW2bPyDY_3ecZWP6NYr-w

大规模机器学习在LinkedIn预测模型中的应用实践

https://mp.weixin.qq.com/s/i1PLA1xr3CefKx1EcVUVIg

谷歌破世界纪录！圆周率计算到小数点后31.4万亿位

https://mp.weixin.qq.com/s/rX8L63-jDGJT6lCAj04I3Q

独家解读！阿里重磅发布机器学习平台PAI 3.0

https://mp.weixin.qq.com/s/Ye2GVTFIrX3SbU1-4cDLoQ

你天天叫的外卖，你知道这里面深度学习的水有多深吗

https://mp.weixin.qq.com/s/FIWfbCLgckVzeNvfThIl4Q

阿里线下智能方案进化史

https://mp.weixin.qq.com/s/pqxiF6yEZzrw8qXu2hEsaA

单机训练速度提升640倍！独家解读快手商业广告模型GPU训练平台Persia

https://mp.weixin.qq.com/s/Jcz4XWDjMmbhmAiI_zBQXQ

流式计算优化：时效性

https://zhuanlan.zhihu.com/p/33351291

基于忆阻器（ReRAM），Computing-in-Memory的DLA

https://mp.weixin.qq.com/s/UbZtUL6Iveb4S3nTU0liGw

深度神经网络的分布式训练概述：常用方法和技巧全面总结

https://mp.weixin.qq.com/s/kLXJsHbBnRIFC3NLChPhzA

如何高效进行大规模分类？港中文联合商汤提出新方法

https://www.zhihu.com/question/454589636

为什么模型和数据都在gpu上，却打不满GPU的使用率？
