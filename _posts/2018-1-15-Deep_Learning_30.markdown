---
layout: post
title:  深度学习（三十）——元学习, 深度哈希
category: DL 
---

* toc
{:toc}

# 元学习

人工智能的历史显示了明确的进展方向：

第一代：**良好的老式人工智能**

- 手工预测
- 什么都不学

第二代：**浅学习**

- 手工功能
- 学习预测

第三代：**深度学习**

- 手工算法（优化器，目标，架构......）
- 端到端地学习功能和预测

第四代：**元学习（Meta-Learning）**

- 无手工
- 端到端学习算法和功能以及预测

![](/images/img4/Meta_Learning.png)

## 教程

http://web.stanford.edu/class/cs330/

CS 330: Deep Multi-Task and Meta Learning

http://metalearning.ml

这是一个Meta-Learning方面的专题讨论会，有不少好东西。

https://mp.weixin.qq.com/s/sQmDZsVGIADwO97yEFATkw

ICML2019《元学习》教程与必读论文列表

李宏毅的教程中也有一章介绍Meta-Learning。

## 参考

https://github.com/gopala-kr/meta-learning

元学习（meta-learning）相关文献资源大列表

https://github.com/sudharsan13296/Awesome-Meta-Learning

元学习相关资源汇总

https://github.com/floodsung/Meta-Learning-Papers

另一个元学习相关资源汇总

https://coladrill.github.io/2018/10/24/元学习总览/

元学习总览

https://zhuanlan.zhihu.com/p/261170127

元学习综述《Meta-Learning in Neural Networks: A Survey》

https://mp.weixin.qq.com/s/KKK3VEpwL90g6Aro8qtXxQ

学习如何学习的算法：简述元学习研究方向现状

https://mp.weixin.qq.com/s/jlD5p5GXFmrWlxg9xvehxg

元学习—Meta Learning的兴起

https://mp.weixin.qq.com/s/WZtmy_RK4lsWqp9qIMCnUA

Meta Learning 1: 基于度量的方法

https://mp.weixin.qq.com/s/A4amtD9jPIRus8Ojnhd3PA

Meta Learning 2: 更多基于度量的方法

https://mp.weixin.qq.com/s/j72Xlh8vUAltvUx0DFGoCA

Meta Learning 3: 少样本文本分类 InductionNet

https://zhuanlan.zhihu.com/p/133159617

Meta-Learning in Neural Networks: A survey

https://mp.weixin.qq.com/s/qoKQwEvOnP384i5Z-_jO1A

CVPR2019最新元学习教程：基于元学习的计算机视觉应用

https://zhuanlan.zhihu.com/p/136975128

一文入门元学习（Meta-Learning）

https://mp.weixin.qq.com/s/z7t2dSnjZqZ3w6q7PUtTVg

最新《元学习》教程，牛津大学Yee Whye Teh教授，165页ppt

https://mp.weixin.qq.com/s/hMTm38gCccxt-Jnz28Xx1A

元强化学习综述及前沿进展

https://mp.weixin.qq.com/s/8koAVoPHczRSfiZkU7kiVQ

元学习最新AAAI2021-Tutorial，附视频与240页ppt

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

最前沿：Meta RL论文解读

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

https://zhuanlan.zhihu.com/p/70782949

最前沿：General Meta Learning

https://zhuanlan.zhihu.com/p/72920138

Meta Learning入门：MAML和Reptile

https://mp.weixin.qq.com/s/MsIAkJAcYHWkkMjzd7qXKA

元学习与强化学习的概率视角，47页ppt，DeepMind牛津Yee Whye Teh

https://mp.weixin.qq.com/s/IdUhvWJYviKtPs9jCbtybA

元知识图谱推理

https://www.zhihu.com/question/291656490

求问meta-learning和few-shot learning的关系是什么？

https://mp.weixin.qq.com/s/LZbprcnben6vPqsoC1DgDA

DeepMind提出元梯度强化学习算法，显著提高大规模深度强化学习应用的性能

https://mp.weixin.qq.com/s/AH35EGTH1YDSx4WzUwY15g

三四行代码打造元学习核心，PyTorch元学习库L2L现已开源

https://github.com/tristandeleu/pytorch-meta

PyTorch上方便好用的元学习工具包

https://mp.weixin.qq.com/s/Fte0SQ7J57AVGyTiIwWKAw

元学习与深度强化学习的机器人应用，84页ppt

https://mp.weixin.qq.com/s/xu5ieaPP2de0GML7b-1BsA

谈谈元学习的技术实现框架

https://mp.weixin.qq.com/s/spRlzjFTh4KeyFfd8pmZgw

新框架ES-MAML：基于进化策略、简易的元学习方法

https://mp.weixin.qq.com/s/uAdFWT5rP40IMsLfFyr7XQ

一种深度网络快速适应的模型无关元学习方法(元学习经典论文)

https://mp.weixin.qq.com/s/joUb4cBxzUVyichYfN9l8g

元强化学习迎来一盆冷水：不比元Q学习好多少

https://mp.weixin.qq.com/s/MPNQPNfFjUqCKUH7OdTDzA

元迁移学习的小样本学习，Meta-transfer Learning for Few-shot Learning，33页ppt

https://mp.weixin.qq.com/s/eUQQEB_0ts0K9h1mw7GQcA

元人脸识别，Learning Meta Face Recognition

https://mp.weixin.qq.com/s/wPKSUpQcB-WrpgsU1Q4iZA

使用MAML元学习的少样本图分类

https://mp.weixin.qq.com/s/CBAdmV4sItmAjxPIA8Fa2w

元学习: 深度阐述元学习的理论模型

https://zhuanlan.zhihu.com/p/113701629

深度阐述元学习的理论模型

https://mp.weixin.qq.com/s/L8eqDp62rMR6Lyco0uGQUQ

南加州大学等开源元学习研究库learn2learn

https://mp.weixin.qq.com/s/BZEqg8pBJ8mcJZhfVAVn8g

最新《元学习神经架构、初始权值、超参数和算法组件》报告，附视频与PPT

https://mp.weixin.qq.com/s/dK2qwYcAZ8Gdb7U2tKIcAA

达摩院基于元学习的对话系统

https://mp.weixin.qq.com/s/46nvCl5o4QYvRemzOrnaoQ

进入Meta Learning的世界(一)

https://mp.weixin.qq.com/s/BhB1n70aAvlwXK7YtNvF0g

《元学习》研究进展

# 深度哈希

https://mp.weixin.qq.com/s/7HALWPQN4PD8JMdeu7zCig

深度哈希方法综述

https://mp.weixin.qq.com/s/EsGCPljYNysFkyMHp1IeUQ

深度哈希图像检索综述论文，14页pdf

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

https://mp.weixin.qq.com/s/QklCVuukfElVDBFNxLXNKQ

哈希革新Transformer：这篇ICLR高分论文让一块GPU处理64K长度序列

https://zhuanlan.zhihu.com/p/161058660

⾼维特征的哈希技巧

# 行人重识别+

https://mp.weixin.qq.com/s/Vi_1Sg8OKG-EG4aC4QTCWA

半监督学习的新助力：无监督数据扩增法

https://mp.weixin.qq.com/s/omUtD3GFOpP1dvfWZgLDww

计算机视觉模型效果不佳，你可能是被相机的Exif信息坑了

https://mp.weixin.qq.com/s/pV7C2sSJwP3rBO6OYeF-nw

基于马尔可夫链的数据增强

https://mp.weixin.qq.com/s/6yfHwsk-fTEtQhrciMEBug

重识别（re-ID）特征适合直接用于跟踪（tracking）问题么？

https://mp.weixin.qq.com/s/Q34wjziJBBOrb1VPhQJK8g

行人重识别简介

https://mp.weixin.qq.com/s/OALGxuvUdQMbsK1k4g2V7Q

遮挡也能识别？地平线提出用时序信息提升行人检测准确度

https://mp.weixin.qq.com/s/2v-Y_-_si6_dxq3cICy-lg

疫情蔓延让这项CV技术突然火了，盘点开源代码

https://mp.weixin.qq.com/s/Z6l9R5uzWsAn_DTgTsGstg

京东发布FastReID：目前最强悍的目标重识别开源库！
