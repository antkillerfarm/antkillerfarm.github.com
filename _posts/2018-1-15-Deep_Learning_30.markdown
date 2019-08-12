---
layout: post
title:  深度学习（三十）——自动求导, 元学习, 深度哈希
category: DL 
---

# Graph NN（续）

https://mp.weixin.qq.com/s/7DyPJ9LnqZ9XyAop33SxSw

ST-GCN动作识别算法详解

https://mp.weixin.qq.com/s/fxVsN2dDmayxJfxBRIXHhQ

解读PingSage：图卷积神经网络在数十亿数据网络级别推荐系统的应用

https://mp.weixin.qq.com/s/SWcJut6QqOvbziirxTd2Kg

斯坦福教授ICLR演讲：图网络最新进展GraphRNN和GCPN

https://mp.weixin.qq.com/s/Lakq83_ngUJf1ES3N7J9_g

图卷积在基于骨架的动作识别中的应用

https://mp.weixin.qq.com/s/5wSgC4pXBfRLoCX-73DLnw

什么是图卷积网络？行为识别领域新星

https://mp.weixin.qq.com/s/1-Dmckby2NcXsaoK08zk8w

视频理解中的图表示学习

https://mp.weixin.qq.com/s/lyy3AhqLDBT88B2LSSIbZQ

图表示解决长文本关系匹配问题：腾讯提出概念交互图算法

https://mp.weixin.qq.com/s/bvp3NIrrarJc_MesKy1x_A

崔泽宇：套装搭配推荐在图神经网络上的应用

https://mp.weixin.qq.com/s/_8K0s9WceJ-xlRViHhz2Zw

Google图挖掘团队最新博客《图表示学习中的创新》

https://mp.weixin.qq.com/s/c3SBGlxzJOYhQBrJ2h3j0g

呼奋宇：深度层次化图卷积神经网络

https://mp.weixin.qq.com/s/YPV2BR6eayKUlPazUeZVnQ

何时能懂你的心——图卷积神经网络（GCN）

https://mp.weixin.qq.com/s/sRKW8DLXZXWLUUVTb12F4Q

“AI新贵”图神经网络算法及平台在阿里的大规模实践

https://mp.weixin.qq.com/s/tAfTmGWqG6IR8SOP0uKW6g

什么限制了GNN的能力？首篇探究GNN普适性与局限性的论文出炉！

https://mp.weixin.qq.com/s/zOdy-1vCJD_dPFSoe0ELFA

图论与图学习（一）：图的基本概念

https://mp.weixin.qq.com/s/0ZdS1WOSDZiXnxP8fybBAw

图论与图学习（二）：图算法

https://mp.weixin.qq.com/s/Orv47r4EchVIR7VcleoJ5Q

谷歌图表征学习创新：学习单个节点多个嵌入&自动学习最优超参数

https://mp.weixin.qq.com/s/sJB4N_ObUqKM8H65yU_1sg

Graph基础知识介绍

https://mp.weixin.qq.com/s/jBQOgP-I4FQT1EU8y72ICA

图神经网络的“开山之作”CGN模型

https://mp.weixin.qq.com/s/DJAimuhrXIXjAqm2dciTXg

何时能懂你的心——图卷积神经网络（GCN）

https://mp.weixin.qq.com/s/DNePTCpyjrlZEixw5L7w5A

GraphSAGE：我寻思GCN也没我牛逼

https://mp.weixin.qq.com/s/1DHvLLysMU24dBeLzbSpUA

GraphSAGE

https://mp.weixin.qq.com/s/C-Pa1jznQntyhocdxS-4Hg

节点嵌入训练加快300倍！解读开源高性能图嵌入系统GraphVite

https://mp.weixin.qq.com/s/edrh-HXqW01Yx7c8tQ8UxA

从数据结构到算法：图网络方法初探

# 自动求导

DL发展到现在，其基本运算单元早就不止CNN、RNN之类的简单模块了。针对新运算层出不穷的现状，各大DL框架基本都实现了自动求导的功能。

论文：

《Automatic Differentiation in Machine Learning: a Survey》

## Numerical differentiation

数值微分最大的特点就是很直观，好计算，它直接利用了导数定义：

$$f'(x)=\lim_{h\to 0}{f(x+h)-f(x)\over h}$$

不过这里有一个很大的问题：h怎么选择？选大了，误差会很大；选小了，不小心就陷进了浮点数的精度极限里，造成舍入误差。

第二个问题是对于参数比较多时，对深度学习模型来说，上面的计算是不够高效的，因为每计算一个参数的导数，你都需要重新计算$$f(x+h)$$。

因此，这种方法并不常用，而主要用于做梯度检查（Gradient check），你可以用这种不高效但简单的方法去检查其他方法得到的梯度是否正确。

## Symbolic differentiation

符号微分的主要步骤如下：

1.需要预置基本运算单元的求导公式。

2.遍历计算图，得到运算表达式。

3.根据导数的代入法则和四则运算法则，求出复杂运算的求导公式。

这种方法没有误差，是目前的主流，但遍历比较费时间。

## Automatic differentiation

除此之外，常用的自动求导技术，还有Automatic differentiation。（请注意这里的AD是一个很狭义的概念。）

类比复数的概念：

$$x = a + bi \quad (i^2 = -1)$$

我们定义Dual number：

$$x \mapsto x = x + \dot{x} d \quad (d^2=0)$$

定义Dual number的运算法则：

$$(x + \dot{x}d) + ( y + \dot{y}d) = x + y + (\dot{x} + \dot{y})d$$

$$(x + \dot{x}d) ( y + \dot{y}d) = xy + \dot{x}yd + x\dot{y}d  +  \dot{x}\dot{y}d^2 = xy + (\dot{x}y+ x\dot{y})d$$

$$-(x + \dot{x}d) = - x - \dot{x}d$$

$$\frac{1}{x + \dot{x}d} = \frac{1}{x} - \frac{\dot{x}}{x^2}d$$

dual number有很多非常不错的性质。以下面的指数运算多项式为例：

$$f(x) = p_0 + p_1x + p_2x^2 + ... + p_nx^n$$

用$$x + \dot{x}d$$替换x，则有：

$$f(x + \dot{x}d) =   p_0 + p_1(x + \dot{x}d) + ... +  p_n(x + \dot{x}d)^n \\ 
= p_0 + p_1x + p_2x^2 + ... + p_nx^n + \\ 
p_1\dot{x}d + 2p_2x\dot{x}d + ... + np_{n-1}x\dot{x}d\\ 
= f(x) + f'(x)\dot{x}d$$

可以看出d的系数就是$$f'(x)$$。

## 不可导函数的求导

不可导函数的求导，一般采用泰勒展开的方式。典型的算法有PGD（Proximal Gradient Descent）。

参考：

https://blog.csdn.net/bingecuilab/article/details/50628634

Proximal Gradient Descent for L1 Regularization

## 参考

https://mp.weixin.qq.com/s/7Z2tDhSle-9MOslYEUpq6g

从概念到实践，我们该如何构建自动微分库

https://mp.weixin.qq.com/s/bigKoR3IX_Jvo-re9UjqUA

机器学习之——自动求导

https://www.jianshu.com/p/4c2032c685dc

自动求导框架综述

https://mp.weixin.qq.com/s/xXwbV46-kTobAMRwfKyk_w

自动求导--Deep Learning框架必备技术二三事

https://mp.weixin.qq.com/s/f0xFfA1inOVOdJnSZR4k6Q

自动微分技术

# 元学习

人工智能的历史显示了明确的进展方向：

第一代：**良好的老式人工智能**

>手工预测   
>什么都不学

第二代：**浅学习**

>手工功能   
>学习预测

第三代：**深度学习**

>手工算法（优化器，目标，架构......）   
>端到端地学习功能和预测

第四代：**元学习**

>无手工   
>端到端学习算法和功能以及预测

## 参考

https://github.com/gopala-kr/meta-learning

元学习（meta-learning）相关文献资源大列表

https://github.com/sudharsan13296/Awesome-Meta-Learning

元学习相关资源汇总

https://mp.weixin.qq.com/s/KKK3VEpwL90g6Aro8qtXxQ

学习如何学习的算法：简述元学习研究方向现状

https://mp.weixin.qq.com/s/qoKQwEvOnP384i5Z-_jO1A

CVPR2019最新元学习教程：基于元学习的计算机视觉应用

https://mp.weixin.qq.com/s/KtO3OTZ-bZ6m0ZSI6jTyjw

OpenAI提出Reptile：可扩展的元学习算法

https://mp.weixin.qq.com/s/T4GiL9vW7ALOzWloE_QQBA

OpenAI开发可拓展元学习算法Reptile，能快速学习

https://mp.weixin.qq.com/s/MWcoGsQJg1GBbSqzyPD9uQ

基于梯度的元学习算法，可高效适应非平稳环境

https://zhuanlan.zhihu.com/p/35695477

基于Meta Learning在动态竞争环境中实现策略自适应

https://mp.weixin.qq.com/s/AhadWUjtgsFmb8uTylTvqg

OpenAI提出新型元学习方法EPG，调整损失函数实现新任务上的快速训练

https://mp.weixin.qq.com/s/dmRdp2oMn0vGukclJSVZDg

Uber AI论文：利用反向传播训练可塑神经网络，生物启发的元学习范式

https://mp.weixin.qq.com/s/Cc4EHc6ei-PtZWhewM10xw

学习如何学习的算法：简述元学习研究方向现状

https://mp.weixin.qq.com/s/4f6-gXovdrYk7240TrUwJg

谷歌大脑：基于元学习的无监督学习更新规则

https://mp.weixin.qq.com/s/cAbMB-DB9vu2ua8t5J28ww

从零开始，了解元学习

https://mp.weixin.qq.com/s/Q36vpS1HF2IfeCsFLh656A

基于元强化学习的神经科学新理论

https://mp.weixin.qq.com/s/XtzvHOk7CdXRBy02kUmgsg

近期爆火的Meta Learning，遗传算法与深度学习的火花，再不了解你就out了

https://mp.weixin.qq.com/s/KvgYyuyICueNQPo_S27fEA

BAIR展示新型模仿学习，学会像人那样执行任务

https://zhuanlan.zhihu.com/p/41223529

最前沿:Meta RL论文解读

https://zhuanlan.zhihu.com/p/40600485

最前沿：Meta Learning前沿进展扫描

https://zhuanlan.zhihu.com/p/28639662

百家争鸣的Meta Learning/Learning to learn

https://zhuanlan.zhihu.com/p/45845001

最前沿：用模仿学习来学习增强学习

https://zhuanlan.zhihu.com/p/46059552

Meta Learning单排小教学

https://zhuanlan.zhihu.com/p/46131981

最前沿：Meta Learning在少样本文本翻译上的应用

https://zhuanlan.zhihu.com/p/46339823

谈谈无监督Meta Learning的研究

https://zhuanlan.zhihu.com/p/46340382

ICLR19最新论文解读之Meta Domain Adaptation

https://mp.weixin.qq.com/s/RBMGI20AI92ZcWSlYczqAA

伯克利、OpenAI等提出基于模型的元策略优化强化学习

https://mp.weixin.qq.com/s/p0dcov84pZqsU7XP30bexQ

Meta-Learning元学习：学会快速学习

https://mp.weixin.qq.com/s/wl8j7dLu3OxPV7MNaO2-7Q

《基于梯度的元学习》199页伯克利博士论文带你回顾元学习最新发展脉络

https://mp.weixin.qq.com/s/ftiGPBhAx5iqlW_Ltg1yhg

《元监督视觉学习》132页伯克利博士论文带你回顾元监督视觉应用最新发展脉络

https://mp.weixin.qq.com/s/K7sLM-LMcF6-gQrV1ddrDw

让智能体主动交互，DeepMind提出用元强化学习实现因果推理

https://mp.weixin.qq.com/s/8sBXlnXiZNsPRwFsgJVRQQ

谷歌提出元奖励学习，两大基准测试刷新最优结果

https://mp.weixin.qq.com/s/x7uk7jBNvnM7Tgk9lFKy3Q

元学习(Meta-Learning)综述及五篇顶会论文推荐

https://mp.weixin.qq.com/s/GF_NLkSw64_6msmFep81fw

Google Brain ICLR Talk：元学习的前沿与挑战

https://mp.weixin.qq.com/s/sQmDZsVGIADwO97yEFATkw

ICML2019《元学习》教程与必读论文列表

https://zhuanlan.zhihu.com/p/70782949

最前沿：General Meta Learning

https://mp.weixin.qq.com/s/rZdd-vWlicynthaSasX3kQ

Meta Learning入门：MAML 和 Reptile

https://mp.weixin.qq.com/s/MsIAkJAcYHWkkMjzd7qXKA

元学习与强化学习的概率视角，47页ppt，DeepMind牛津Yee Whye Teh

# 深度哈希

https://mp.weixin.qq.com/s/iVKnLyNJGVRsR5fWc92Rwg

深度离散哈希算法，可用于图像检索！

https://mp.weixin.qq.com/s/XUYJub0559wwQ9H1wA_SAg

机器学习时代的哈希算法，将如何更高效地索引数据

https://mp.weixin.qq.com/s/vFBlFAQLvDZP7IvwKoaPhA

无问西东，只问哈希

https://mp.weixin.qq.com/s/XAxuLg2i3q5_uKDo1wU_rA

从哈希到卷积神经网络：高精度&低功耗

https://mp.weixin.qq.com/s/i8iQtCC7ahXLY1a1wOacsA

Science：最新发现哈希可能是大脑的通用计算原理

https://mp.weixin.qq.com/s/ZOVWXNym5yHoo-MmpxXo0A

自监督对抗哈希SSAH：当前最佳的跨模态检索框架

https://mp.weixin.qq.com/s/VldzlYg5AfDRho8bsROL_g

HashGAN:基于注意力机制的深度对抗哈希模型提升跨模态检索效果

https://mp.weixin.qq.com/s/3Z2Zc8zTq2uiPyw7ZuuZfw

解密美图大规模多媒体数据检索技术DeepHash
