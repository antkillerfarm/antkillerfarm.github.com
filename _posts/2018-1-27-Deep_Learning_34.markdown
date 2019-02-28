---
layout: post
title:  深度学习（三十四）——词向量进阶, 深度时间序列, 图像超分辨率进阶, CNN进阶
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

https://mp.weixin.qq.com/s/8uyIuO_A0NwusyJV-bKRug

个性化序列推荐：卷积序列嵌入方法

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

https://mp.weixin.qq.com/s/x_4k1ICo0GUFJzo4jRSoIQ

中文词向量论文综述（一）

https://mp.weixin.qq.com/s/79kTL79JIpB-YxbjSoQ-Xw

复旦大学黄萱菁教授带你了解自然语言理解中的表示学习

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

https://mp.weixin.qq.com/s/oJUzrZP_cxhiarq0w9Yqpg

从KDD 2018最佳论文看Airbnb实时搜索排序中的Embedding技巧

https://mp.weixin.qq.com/s/sY9ILRTUCmupmJyJ3RX6-w

57页清华大学孙茂松组《知识表示学习》综述论文

https://mp.weixin.qq.com/s/KjDwumX8FViKC4FNWdml_w

如何给词嵌入模型选择最优维度

https://mp.weixin.qq.com/s/nAWiNAgA_Ste8rzcqaH8YA

情感分析词嵌入预处理细粒度实验综述

https://mp.weixin.qq.com/s/caG7kwZfo2qpvLDbrvfpng

AAAI 2019教程—361页PPT带你回顾最新词句Embedding技术和应用

# 深度时间序列

https://mp.weixin.qq.com/s/cChJO6YrE_XAgeInS7J1Vw

130页序列推荐系统教程重磅发布

https://mp.weixin.qq.com/s/zv264-dqDQYRkYmjX_QZpQ

郑宇解读地理传感器时间序列预测问题

https://mp.weixin.qq.com/s/JwRXBNmXBaQM2GK6BDRqMw

Kaggle网站流量预测任务第一名解决方案：从模型到代码详解时序预测

https://mp.weixin.qq.com/s/caXnseARUwLIXdsZ7BXOUw

用于金融时序预测的神经网络：可改善移动平均线经典策略

http://mp.weixin.qq.com/s/9fJT0dMLvYQdfMVvSEiG4A

深度学习的时间序列模型评价

https://mp.weixin.qq.com/s/1Gdx-U3DRZtSJoFyCLnD0w

深度学习之Sequence Learning

https://mp.weixin.qq.com/s/SWKWI7BADnX43e-sCBxRPg

神经网络在算法交易上的应用系列——简单时序预测

https://mp.weixin.qq.com/s/Ux7XjLHzlaGFtM1uSMu2gQ

神经网络在算法交易上的应用系列——时序预测+回测

https://mp.weixin.qq.com/s/fUqQo0m7nMLZO85YG-duRw

神经网络在算法交易上的应用系列——多元时间序列

https://mp.weixin.qq.com/s/PS1SqSWckuHuyZP6d5ZUFw

“深度学习”信号处理和时序分析的最后选择？

https://zhuanlan.zhihu.com/p/54471673

基于前馈神经网络的时间序列异常检测算法

https://mp.weixin.qq.com/s/g9upS70qFOCFBMm-T5nI1A

利用深度学习最新前沿预测股价走势

https://mp.weixin.qq.com/s/otwmDtiDfVVID65RQgT4Uw

教你如何鉴别那些用深度学习预测股价的花哨模型？

# 图像超分辨率进阶

https://mp.weixin.qq.com/s/xpvGz1HVo9eLNDMv9v7vqg

NTIRE2017夺冠论文：用于单一图像超分辨率的增强型深度残差网络

https://www.zhihu.com/question/25401250

如何通过多帧影像进行超分辨率重构？

https://www.zhihu.com/question/38637977

超分辨率重建还有什么可以研究的吗？

https://zhuanlan.zhihu.com/p/25912465

胎儿MRI高分辨率重建技术：现状与趋势

https://mp.weixin.qq.com/s/i-im1sy6MNWP1Fmi5oWMZg

华为推出新型HiSR：移动端的超分辨率算法

https://mp.weixin.qq.com/s/h4Xzt-aS1_-5zjTB0ypTLg

普通视频转高清：10个基于深度学习的超分辨率神经网络

https://mp.weixin.qq.com/s/WmqagSGRy98USgnz21W3Pg

WDSR

https://mp.weixin.qq.com/s/oNavFLOPskHNxWIyBbFzHw

WDSR (NTIRE2018 超分辨率冠军)

https://mp.weixin.qq.com/s/AxHTaT-G5_Y6Iw_3aIIxCg

超分辨率技术如何发展？这6篇ECCV 18论文带你一次尽览

https://mp.weixin.qq.com/s/zWoQCKbZNz2td3cZxEsqKQ

腾讯优图提出SRN-DeblurNet：高效高质量去除复杂图像模糊

https://mp.weixin.qq.com/s/eJkkbGBYxWlngkT5gjjW7g

效果惊人：上古卷轴III等经典游戏也能使用超分辨率GAN重制了

https://mp.weixin.qq.com/s/M8gCrQDtjT1lszsxV2QQKg

FSRNet：端到端深度可训练人脸超分辨网络

https://mp.weixin.qq.com/s/fkHfzkHRFpEGc-L4uGlqcg

从网络设计到实际应用，深度学习图像超分辨率综述

https://mp.weixin.qq.com/s/C9Ku4MvU2QuZ_GJcs8FelA

基于深度学习的图像超分辨率最新进展与趋势

# CNN进阶

http://mp.weixin.qq.com/s/ddTNr-967IahTZ2X1LNSEQ

盘点影响计算机视觉Top100论文：从ResNet到AlexNet

https://mp.weixin.qq.com/s/gwH9s1ggMTj2dJkad9wUuw

从VGG到NASNet，一文概览图像分类网络

https://mp.weixin.qq.com/s/hIAIbpqItS09KDOSFxaeqg

从Inception v1到Inception-ResNet，一文概览Inception家族的“奋斗史”

https://mp.weixin.qq.com/s/r143qYj8bziu_N-27RWRRw

机器学习5年大跃进，可能是个错觉

https://mp.weixin.qq.com/s/pollD4LN_GHJckzJAjwPqg

侧抑制”卷积神经网络，了解一下？

https://mp.weixin.qq.com/s/gil2K-JKzfRqdzc-abnh6A

王井东：深度融合——一种神经网络结构设计模式

https://mp.weixin.qq.com/s/7l0J5LIAawlkCIrUEGG9aA

卷积神经网络“失陷”，CoordConv来填坑

https://mp.weixin.qq.com/s/4GPLeJiQoDfBZJzF4QmuAg

Excel再现人脸识别：CNN用于计算机视觉任务不再神秘

https://mp.weixin.qq.com/s/30_AwwHS0eqfmYwTvRQ85Q

pooling去哪儿了？

https://mp.weixin.qq.com/s/yTkAhje3A0dRSyTobjsm2A

Pervasive Attention：用于序列到序列预测的2D卷积神经网络

https://zhuanlan.zhihu.com/p/31595192

Deep Image Prior：深度卷积网络先天就理解自然图像

https://mp.weixin.qq.com/s/TelGG-uVQyxwQjiDGE1pqA

特征金字塔网络FPN

https://mp.weixin.qq.com/s/6KC1vhKcyqQ0eZEXJyPqVA

密集连接卷积网络

https://mp.weixin.qq.com/s/vUK2NneOs8OR0vcJEZNbrA

田渊栋等人论文：何时卷积滤波器容易学习？

https://mp.weixin.qq.com/s/PiQB2AvhtDceMJxYN8O8jA

通用卷积神经网络交错组卷积

https://mp.weixin.qq.com/s/2w_Bwqx9C9KwenK480sVMQ

如何可视化卷积网络分类图像时关注的焦点

https://mp.weixin.qq.com/s/yQG0983RjQHIkt9oBlkDqQ

瞎谈CNN：通过优化求解输入图像

https://zhuanlan.zhihu.com/p/33445638

传统不死：在CNN中学习和构建空间传播模块

https://mp.weixin.qq.com/s/mtocyqpybSEpwekS20xgmQ

使用CNN生成图像先验，实现更广泛场景的盲图像去模糊

https://mp.weixin.qq.com/s/Uc0VFMmYoOFvH0c7IExKIg

何恺明团队计算机视觉最新进展：从特征金字塔网络、Mask R-CNN到学习分割一切

https://mp.weixin.qq.com/s/W2DqCjmF3oyPLKeETAO5XA

CNN实现“读脑术”，成功解码人脑视觉活动，准确率超50%

https://mp.weixin.qq.com/s/t9ZHW8fkWtnvpM7ynJOTDw

从语义上理解卷积核行为，UCLA朱松纯等人使用决策树量化解释CNN

https://mp.weixin.qq.com/s/Fhge-Idk_adBjUuzaAtzyQ

7大类深度CNN架构创新综述

https://mp.weixin.qq.com/s/f1uOimoA3a37e1BK1l1rvw

从群等变卷积网络到球面卷积网络

https://mp.weixin.qq.com/s/DG4ofF78fIgiIMiJaxuIVA

AAAI 2019 论文解读：卷积神经网络继续进步

https://mp.weixin.qq.com/s/1TK4h7UoeVas1S1NfNgRag

为什么深度学习图像分类的输入多是224*224
