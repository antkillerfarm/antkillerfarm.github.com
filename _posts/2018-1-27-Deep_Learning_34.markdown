---
layout: post
title:  深度学习（三十四）——词向量进阶, 深度贝叶斯学习
category: DL 
---

* toc
{:toc}

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

https://mp.weixin.qq.com/s/TaLsCHNIYPRAog38gu5N8g

word2vec, node2vec, graph2vec, X2vec：构建向量嵌入表示理论，120页ppt

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

https://mp.weixin.qq.com/s/YGBvWIENE9TASvb_t_Pebw

爱奇艺深度语义表示学习的探索与实践

https://mp.weixin.qq.com/s/SN6qnaorfYpIYMNcZQTR8Q

推荐系统embedding技术实践总结

https://mp.weixin.qq.com/s/ziKG-fvXyVc59eyNjoXseA

不要再对类别变量进行独热编码了

https://mp.weixin.qq.com/s/AjmaX0B7xaCwEWi8sDOJyA

论推荐算法中的Embedding思想

## 参考

http://kexue.fm/archives/4667/

更别致的词向量模型(一)：simpler glove

http://kexue.fm/archives/4669/

更别致的词向量模型(二)：对语言进行建模

http://kexue.fm/archives/4671/

更别致的词向量模型(三)：描述相关的模型

http://kexue.fm/archives/4675/

更别致的词向量模型(四)：模型的求解

http://kexue.fm/archives/4677/

更别致的词向量模型(五)：有趣的结果

http://kexue.fm/archives/4681/

更别致的词向量模型(六)：代码、分享与结语

https://mp.weixin.qq.com/s/GYTxN5X7MnSQ4k5bD2l-PQ

Salesforce的爱因斯坦AI最新NLP研究，通过情境化词向量从翻译中学习!

https://mp.weixin.qq.com/s/GUUkXrB1iyg4rQbBtICq6A

通过NMT训练的通用语境词向量：NLP中的预训练模型？

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

https://mp.weixin.qq.com/s/B0HAl9T8yKGpH_PZbbw0Yg

用于知识图中链接预测的嵌入方法SimplE

https://mp.weixin.qq.com/s/wekqgCtlMan_PJHYadSpcg

从词向量到句向量的技术详解

https://www.zhihu.com/question/33952003

如何通过词向量技术来计算2个文档的相似度？

https://mp.weixin.qq.com/s?__biz=MzI1NjM1ODEyMg==&mid=2247484632&idx=1&sn=e3ed259243c4ee26403edf4573165f76

嵌入方法在推荐系统中的应用

https://zhuanlan.zhihu.com/p/85739175

关于句子表征的学习笔记

https://mp.weixin.qq.com/s/M_Fo1BSv3yZ52XPxXCf77w

多模态预训练表示UNITER：通用图像-文本语言表示学习

https://mp.weixin.qq.com/s/RyKy8Iw_ZLTCrbPS3rFtOQ

Airbnb基于Embedding技术的实时个性化推荐

https://mp.weixin.qq.com/s/3ZZvTULbdeSKzF51Wa74WQ

分布式词向量表示，附239页PPT下载

https://zhuanlan.zhihu.com/p/109935332

万物皆可embedding

https://mp.weixin.qq.com/s/iMU2LPDadmVMgzUifw3-XA

浅谈电商搜索推荐中ID类特征的统一建模：Hema Embedding解读

https://lumingdong.cn/application-practice-of-embedding-in-recommendation-system.html

推荐系统的中embedding的应用实践

https://mp.weixin.qq.com/s/76BaQ6b1FYz05pZUbplGhQ

一文总结词向量的计算、评估与优化

https://mp.weixin.qq.com/s/BwqVwN-9R8ROjgfnTUcMQg

万字长文聊一聊Embedding技术

# 深度贝叶斯学习

https://mp.weixin.qq.com/s/4sDNUZiOiS6VH_oRSnW6HQ

牛津大学YARIN GAL《贝叶斯深度学习》入门教程，336页ppt

https://mp.weixin.qq.com/s/w_phnVwm13P8dU0Ks-b-YA

《贝叶斯深度学习: DL与Bayesian原理 》NeurIPS2019硬核教程

https://mp.weixin.qq.com/s/J_sbJb8i-O8CwhHvVyPv1w

《深度贝叶斯数据挖掘》，附257页PPT下载

https://mp.weixin.qq.com/s/cDqxmRVQCIqdM5oiUh82YQ

Yee Whye Teh：《贝叶斯深度学习与深度贝叶斯学习》

https://mp.weixin.qq.com/s/JZVl0kygVawdW8qflPps6g

《神经贝叶斯信息处理》教程，220页ppt，国立交通大学

https://mp.weixin.qq.com/s/Zk2YG-IJNhJxTBU8THSM-g

让DL可解释？这一份66页贝叶斯深度学习教程告诉你

https://mp.weixin.qq.com/s/-izo9VUdxN33pwVFGV_tjw

299页PPT带你回顾深度贝叶斯学习最新发展脉络

https://mp.weixin.qq.com/s/UiLyQKhIe2rDYiwPcqyqaw

可跟踪概率模型，209页最新教程

https://mp.weixin.qq.com/s/pHAbxeYBI2q6pUHNrAt1og

贝叶斯学习与未来人工智能

https://mp.weixin.qq.com/s/Zd4rFU7Lebr4zmzxThNyVw

详解珠算：清华大学开源的贝叶斯深度学习库

https://mp.weixin.qq.com/s/RpaOrngeXTKycLb3iCygZw

利用贝叶斯神经网络进行随机动力系统中的学习与策略搜索

https://github.com/bayesgroup/deepbayes-2018

Seminars DeepBayes Summer School 2018

https://mp.weixin.qq.com/s/WCRYppBLdl_M4etUChnfgw

PyMC3和Theano代码构建贝叶斯深度网络

https://mp.weixin.qq.com/s/7mwJpQFWWXJ3dvTAwDFI7Q

贝叶斯卷积神经网络：架起深度学习与统计学的桥梁

https://mp.weixin.qq.com/s/2LkpuchuHs82Sxs5rD8bWA

《深度贝叶斯与序列学习》，279页PPT带你知晓深度贝叶斯序列模型在NLP最新进展

https://zhuanlan.zhihu.com/p/74573041

针对推荐系统的深度贝叶斯多目标学习

https://mp.weixin.qq.com/s/b041h_hbHQYiXCiDHGaD5w

深度贝叶斯自然语言处理，304页ppt带你了解最新研究进展

https://zhuanlan.zhihu.com/p/77140176

构建贝叶斯深度学习分类器

https://mp.weixin.qq.com/s/0e4GHNRCF9xKFELAZ4zRFA

A simple tutorial for Bayesian neural network

https://mp.weixin.qq.com/s/NkRemHPRnEcEwbs5b-Mz9w

贝叶斯编程，378页pdf，Bayesian Programming

https://mp.weixin.qq.com/s/DDg4HTp-APwEIul1ZaFFPQ

最新《贝叶斯推断》教程，125页ppt与视频，DeepMind Shakir Mohamed博士

https://zhuanlan.zhihu.com/p/283633149

Bayesian Deep Learning最新研究总结
