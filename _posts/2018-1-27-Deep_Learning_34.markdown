---
layout: post
title:  深度学习（三十四）——词向量进阶, 深度强化学习（1）
category: DL 
---

# 词向量进阶

## All is Embedding

向量化是机器学习处理非数值数据的必经之路。因此除了词向量之外，还有其他的Embedding。比如Network Embedding。

参考：

https://mp.weixin.qq.com/s/wcFlZPbB5dl6C87kdfjmKw

NE(Network Embedding)论文小览

https://mp.weixin.qq.com/s/rWn6Bi5T3JfucD4C0jYSBA

Network Embedding指南

https://mp.weixin.qq.com/s/luSTZVzaK6fPw4veIDuMwg

网络表示学习综述：一文理解Network Embedding

https://mp.weixin.qq.com/s/HjzZJIA77gUlkG4doeT3uQ

网络表示学习介绍

https://zhuanlan.zhihu.com/p/36788547

网络表示学习论文引介

https://mp.weixin.qq.com/s/zTNX_LeVMeHhJG7kPewn2g

除了自然语言处理，你还可以用Word2Vec做什么？

https://mp.weixin.qq.com/s/7dsrvHp6KIvlE-VXiUH1Rw

HIN2Vec：异质信息网络中的表示学习

https://mp.weixin.qq.com/s/CqJ7o1-ptCBBocB3PfEuXg

万物向量化：用协作学习的方法生成更广泛的实体向量

https://mp.weixin.qq.com/s/UtfidoBCJ0Wjpnl_C1a7iw

浅谈向量化与Hash-Trick

https://mp.weixin.qq.com/s/0JmB0sMUVsiuwrVObN_10g

浙江大学提出设计网络嵌入算法的度惩罚原则，可有效保留无标度特性

## 参考

https://mp.weixin.qq.com/s/4ZlVisY08XvisjbK5zSheg

NLP中“词袋”模型和词嵌入模型的比较

https://mp.weixin.qq.com/s/L6_BV-cWR4wge2ritmyqzA

让你上瘾的网易云音乐推荐算法，用Word2vec就可以实现

https://mp.weixin.qq.com/s/ktzqTUbqMeTLvc-pLJbUvA

蚂蚁金服公开最新基于笔画的中文词向量算法

https://mp.weixin.qq.com/s/GGXI-ZzPc9LLjVQpf5ia1A

词嵌入2017年进展全面梳理：趋势和未来方向

https://mp.weixin.qq.com/s/gI82_PHc94OAsZQBk6DwQw

一次搞定多种语言：Facebook展示全新多语言嵌入系统

https://mp.weixin.qq.com/s/diIzbc0tpCW4xhbIQu8mCw

阿里凑单算法首次公开！基于Graph Embedding的打包购商品挖掘系统解析

https://mp.weixin.qq.com/s/8uyIuO_A0NwusyJV-bKRug

个性化序列推荐：卷积序列嵌入方法

https://mp.weixin.qq.com/s/chiHw5gKnJyTJTQeF6gViw

在向量空间中启用网络分析和推理，清华大学崔鹏博士最新分享

https://mp.weixin.qq.com/s/oKwxWbCkH-xqYSJIBdb92A

2018超网络节点表示学习

https://mp.weixin.qq.com/s/kQlxLDHLI6xxFzwJVjFj7w

GraRep: 基于全局结构信息的图结点表示学习

https://mp.weixin.qq.com/s/vrdprH_8J80J3VHQqA-bKg

通过动态融合方式学习多模态词表示，中科院自动化所宗成庆老师团队最新工作

https://mp.weixin.qq.com/s/7Ujxc5_CwhYxXTLAjac0-g

基于异构网络节点表示的推荐系统

https://mp.weixin.qq.com/s/98k1fNHDIwITFyLnNutt4A

Entity Embeddings: 利用深度学习训练结构化数据的实体嵌入

https://blog.csdn.net/malefactor/article/details/50767711

利用Word Embedding自动生成语义相近句子

https://mp.weixin.qq.com/s/4sqMV3tS146VNrTyyncYnQ

中英文词向量评测理论与实践

https://mp.weixin.qq.com/s/p7sCt5Xj1U5vIZftRXBvzw

cw2vec理论及其实现

https://mp.weixin.qq.com/s/df-k5kJcJXhSWbFMu2mtCA

当前最好的词句嵌入技术概览：从无监督学习转向监督、多任务学习

https://mp.weixin.qq.com/s/nmuLM38Q_0MVN_6EtQ_7fQ

艾伦人工智能研究所提出新型深度语境化词表征

https://mp.weixin.qq.com/s/rYvbV4ssmXgblOg20KQVyQ

文本嵌入的经典模型与最新进展

https://mp.weixin.qq.com/s/lpQWcGg7uRBAorj_5QyunA

使用三重损失网络学习位置嵌入：让位置数据也能进行算术运算

https://mp.weixin.qq.com/s/BHA-tFCQjvhf1Tj53SBEaw

基线系统需要受到更多关注：基于词向量的简单模型

https://mp.weixin.qq.com/s/WQlSghxG89JCroNZSmop8w

朱军：关于图的表达学习

https://mp.weixin.qq.com/s/x_4k1ICo0GUFJzo4jRSoIQ

中文词向量论文综述（一）

https://mp.weixin.qq.com/s/79kTL79JIpB-YxbjSoQ-Xw

复旦大学黄萱菁教授带你了解自然语言理解中的表示学习

https://mp.weixin.qq.com/s/WMpcamrHjUDnYwqyISdooA

斯坦福Jure Leskovec图表示学习：无监督和有监督方法

https://mp.weixin.qq.com/s/0QyLIvH1McooGLc19hIAHA

预测你的下一步-动态网络节点表示学习

https://mp.weixin.qq.com/s/QW7t7iaIN1yaHYzOtwgYAQ

Representation Learning on Network网络表示学习

https://mp.weixin.qq.com/s/F2jF1vuzK4u8ZPrDK_CyLw

KDD2018 网络表示学习最新教程：DeepWalk作者Perozzi等人带你探索最前沿

https://mp.weixin.qq.com/s/KK-_G7Sc9HyYlxH1nLxVfA

斯坦福大学提出全新网络嵌入方法—GraphWave

https://zhuanlan.zhihu.com/p/44755895

"Social Rank":节点重要度在网络表示学习中的影响

https://mp.weixin.qq.com/s/2GPPC1LfpKLj80EJq7rcDA

如何帮助大家找工作？领英利用深度表征学习提升人才搜索和推荐系统

https://mp.weixin.qq.com/s/I2Gh3f_8cmfeOt0uEGj8hA

FAIR动态元嵌入:动态选择词嵌入模型

https://mp.weixin.qq.com/s/5ko0-W5giZ7wBCLUmkQeKA

神经网络词嵌入：如何将《战争与和平》表示成一个向量？

https://mp.weixin.qq.com/s/4gqpdTxt7Lip-hfuuUweAw

词嵌入获得的信息远比我们想象中的要多得多

https://mp.weixin.qq.com/s/-hh9iRIZtxh8Q-ZU_oCekA

网络嵌入之STNE model

https://mp.weixin.qq.com/s/iLaGGJXLwgI7qzcgkySlSQ

工程实践也能拿KDD最佳论文？解读Embeddings at Airbnb

https://mp.weixin.qq.com/s/sY9ILRTUCmupmJyJ3RX6-w

57页清华大学孙茂松组《知识表示学习》综述论文

# 深度强化学习

https://mp.weixin.qq.com/s/7BsXPQ8wC6_fHulU63ZQiQ

当强化学习遇见泛函分析

https://mp.weixin.qq.com/s/6n5HawyR4AgH8Dq0gJMw2g

强化学习的基本概念与代码实现

https://zhuanlan.zhihu.com/p/27699682

荐译一篇通俗易懂的策略梯度（Policy Gradient）方法讲解

https://mp.weixin.qq.com/s/RLEuaiRdq6AbTSUcYQ5O3A

深度强化学习在NLP怎么用？看清华黄民烈老师这一份120页《自然语言处理和搜索中的深度强化学习应用》讲义

https://mp.weixin.qq.com/s/I8IwPCY6-zocJKFXMr6rUg

深度强化学习的18个关键问题

https://mp.weixin.qq.com/s/_dskX5U8gHAEl6aToBvQvg

从Q学习到DDPG，一文简述多种强化学习算法

https://zhuanlan.zhihu.com/p/29019246

基于策略的增强学习

https://mp.weixin.qq.com/s/OY56lJ_NFf5vVAgKfKyx2A

利用强化学习自动搜索最优化方法

https://mp.weixin.qq.com/s/uDFsWebfLmka-zZX3Y_8kg

深度强化学习在面向任务的对话管理中的应用

https://mp.weixin.qq.com/s/nYOOwVoijl1p4V0A7yaI3w

机遇与挑战：用强化学习自动搜索优化算法

http://mp.weixin.qq.com/s/TBVVdX3erOpXNjXmhLmxOw

学“深度强化学习”，看懂DeepMind这篇文章就够了!

https://mp.weixin.qq.com/s/_Di73PkEWJV1-OLLHfz7yQ

组合在线学习：实时反馈玩转组合优化

https://mp.weixin.qq.com/s/lR6BSa_pJzcinkSaSWsM2A

伯克利提出强化学习新方法，可让智能体同时学习多个解决方案

https://mp.weixin.qq.com/s/P-iSI80IVmb5s-Q15Re2HQ

All In!我学会了用强化学习打德州扑克

https://zhuanlan.zhihu.com/p/36322095

最前沿：从虚拟到现实，DRL让小狗机器人跑起来了！

https://mp.weixin.qq.com/s/xr-2cNoSYpCftLI3dV6zEw

如何使用深度强化学习帮助自动驾驶汽车通过交叉路口？

https://mp.weixin.qq.com/s/R_pfTXDMaLHmiCaSV2t_YA

英特尔Nervana发布强化学习库Coach：支持多种价值与策略优化算法

https://mp.weixin.qq.com/s/AyW7oOC7yxVtmswaMT1DGQ

腾讯AI Lab获得计算机视觉权威赛事MSCOCO Captions冠军

https://mp.weixin.qq.com/s/4aENmxUMEEPVPnexLKrg7Q

新型强化学习算法ACKTR

https://mp.weixin.qq.com/s/5PzTiPoXPC1gH3xszzT2dQ

邓力等人提出BBQ网络：将深度强化学习用于对话系统

https://mp.weixin.qq.com/s/pM8oykHmtu5O5jYJBZjO_w

伯克利研究人员使用内在激励，教AI学会好奇

https://mp.weixin.qq.com/s/3WI3QgfHXcrCPbvmHWOEkg

强化学习在生成对抗网络文本生成中的作用

https://mp.weixin.qq.com/s/IvR0O6dpz2GJCG7UQb5kUQ

清华大学冯珺：基于强化学习的关系抽取和文本分类

https://zhuanlan.zhihu.com/p/31579144

让我们从零开始做一个机械手臂(强化学习)

https://mp.weixin.qq.com/s/FiR_GRYqJYpJRO-2p44-Cg

伯克利强化学习新研究：机器人只用几分钟随机数据就能学会轨迹跟踪

https://mp.weixin.qq.com/s/u49cuDV21ITs1aV9tJR85g

Pieter Abbeel：《深度学习在机器人中的应用》

https://mp.weixin.qq.com/s/_dHjZQ_7_7H34PHhV_lC3w

全新强化学习算法详解，看贝叶斯神经网络如何进行策略搜索

https://mp.weixin.qq.com/s/YCPXkFzYdC1gMfnprZsVXg

如何让机器人多技能？通过最大熵强化学习

https://mp.weixin.qq.com/s/dbtdNsT3nvLt4UKsrNsn_Q

量化深度强化学习算法的泛化能力
