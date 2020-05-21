---
layout: post
title:  推荐系统（一）——推荐系统进阶, 常用排序算法
category: Recommender System 
---

# 推荐系统进阶

除了《机器学习（十六～十七）》提及的ALS和PCA之外，相关的算法还包括：

## FM：Factorization Machines

Factorization Machines是Steffen Rendle于2010年提出的算法。

>注：Steffen Rendle，弗赖堡大学博士，现为Google研究员。libFM的作者，被誉为推荐系统的新星。

FM算法实际上是一大类与矩阵分解有关的算法的广义模型。

参考文献1是Rendle本人的论文，其中有章节证明了SVD++、PITF、FPMC等算法，都是FM算法的特例。《机器学习（十四）》中提到的ALS算法，也是FM的特例。

参考文献2是国人写的中文说明，相对浅显一些。

参考：

https://www.ismll.uni-hildesheim.de/pub/pdfs/Rendle2010FM.pdf

http://blog.csdn.net/itplus/article/details/40534885

Factorization Machines 学习笔记（一）预测任务

https://tech.meituan.com/deep-understanding-of-ffm-principles-and-practices.html

深入FFM原理与实践

https://github.com/aksnzhy/xlearn

这是一个集成了FM和FFM等算法的库

https://mp.weixin.qq.com/s/MEjhJycDwssvzXm7DaSp7Q

推荐系统召回模型之全能的FM模型

https://mp.weixin.qq.com/s/zIvzGpHNnm7WRgDmtdFd-w

Factorization Machines因子分解机详解

https://mp.weixin.qq.com/s/SlflfdqSIZGR59ch50DIAA

因子分解机

https://mp.weixin.qq.com/s/qynApDntA9xiGRBQpoIsjQ

FFM：Field-aware Factorization Machines

https://mp.weixin.qq.com/s/Fg9GKw-QLlOskEqX7pl1dA

推荐系统之FM算法原理及实现（附代码）

## PITF

配对互动张量分解（Pairwise Interaction Tensor Factorization）算法，也是最早由Rendle引入推荐系统领域的。

论文：

http://www.wsdm-conference.org/2010/proceedings/docs/p81.pdf

## 其他

https://mp.weixin.qq.com/s/7yjA3_oCI5nSH4tv04BIhQ

HFT

https://mp.weixin.qq.com/s/gHKOArFzUM9Zn8hEsA-1wQ

FISM

https://mp.weixin.qq.com/s/VymwTuKq86JP2PL4v8LyyQ

POI by Friends

https://mp.weixin.qq.com/s/LnV-Oq3pCCeMk9RRhha-Aw

GLSLIM

https://mp.weixin.qq.com/s/xnJq-aBAZW22tP7RQylKLw

iCD

https://mp.weixin.qq.com/s/-IPwfrBz1dtYDupuGv4IjQ

Ensemble

https://mp.weixin.qq.com/s/SC8kNYvexetmDuxfQvwSDw

CKE

https://mp.weixin.qq.com/s/bu9rSno_WmHHisE3lzYnqg

ConvMF

https://mp.weixin.qq.com/s/opJtn5mPVjnfRwr5UZ4aJg

FTRL原理与工程实践

http://www.cnblogs.com/EE-NovRain/p/3810737.html

各大公司广泛使用的在线学习算法FTRL详解

https://mp.weixin.qq.com/s/ghDTO9cauM87mMvc-8ojYQ

文章推荐系统之基于FTRL模型的在线排序

## 稀疏性

稀疏性问题的本质数据的不完备。我们研究稀疏性问题的意义就是使得整个推荐系统中的内容不再沉淀在冗余的空间里，而不被利用。目前推荐系统稀疏性问题的方法主要是使用多级关联规则挖掘方法、ICHM 算法、或者是矩阵聚类、信任度传播策略。其中 ICHM 算法是针对项目采用了传递关联来解决稀疏性，与之类似的还有 UCHM 算法，这个是对用户概貌进行聚类。

## 冷启动

冷启动问题是由于新进用户或者内容尚未建立起内容关联而导致推荐效果差的情况。

目前有两种解决方向：

- 一个是不考虑内容的解决方案，比如说利用随机推荐，当一个新进用户开始使用系统时，给他随机地推荐一些内容，这样子不断地积累此用户概貌，然后慢慢养成，慢慢调教，直到推荐出用户喜欢的内容。除了随机推荐法之外，还有平均值法、众数法、信息熵法等相关解决办法。

- 另一种方案是考虑内容的解决方案，比如说结合机器学习的相关方法或者是统计学上的一些策略模型进行解决。

参考：

https://mp.weixin.qq.com/s/ll2Nx7_1Sg7XfuiH-OQnWA

推荐系统如何冷启动？

https://mp.weixin.qq.com/s/ecAA43az0-6UIEPR_maNAw

推荐系统之冷启动问题

https://mp.weixin.qq.com/s/ycxigg0HbiOTRUBPAWk21Q

推荐系统中的冷启动问题和探索利用问题

https://mp.weixin.qq.com/s/82REVmDV5cfeQP766qOZmw

推荐系统冷启动

https://mp.weixin.qq.com/s/x9uDExR-Un8fAI43TOQ7CA

个性化推荐系统入门指南

https://mp.weixin.qq.com/s/KSW47hbNLaHTw9Ib0wMO8g

图推荐算法在E&E问题上的应用

## 负采样策略 & 非采样策略

推荐系统中经常用到的数据是隐式反馈数据（Implicit Feedback Data），如浏览历史，点击日志等。这些数据往往只有正反馈而没有负反馈，并且数据是高度稀疏的（相比于用户点击过的产品，没有被点击过的产品的数量非常多）。在推荐系统中使用隐式反馈数据，有两种常见的学习方法：

- **负采样策略**：从所有用户未标记的样例中抽取一部分作为负例。

- **非采样策略**：将所有用户未标记的样例作为负例。

参考：

https://mp.weixin.qq.com/s/OGLJx-1tGYYuLWFricfRKg

浅谈个性化推荐系统中的非采样学习

## 参考

https://zhuanlan.zhihu.com/p/95350982

推荐算法三视角:矩阵，图，时间线

https://mp.weixin.qq.com/s/q9FU19Hpw2eWLLhsY5lYJQ

parameter-free contextual bandits

https://mp.weixin.qq.com/s/T-yCjebTzc_t6D4o5gyQLQ

Collaborative Metric Learning

https://mp.weixin.qq.com/s/9xxLU51eqhc6C81jzHQijQ

简述推荐系统中的矩阵分解

https://mp.weixin.qq.com/s/qmkMJJkMqumbtynM4cLatw

计算广告CTR预估系列

https://zhuanlan.zhihu.com/p/38117606

早期购买行为的分析和预测建模

https://mp.weixin.qq.com/s/nQAqEZW_TJSsbVp-kVcKGA

简谈马尔可夫模型在个性化推荐中的应用

https://mp.weixin.qq.com/s/Zalby_gZsxzrmBq6ZePsiQ

短视频如何做到千人千面？FM+GBM排序模型深度解析

https://mp.weixin.qq.com/s/hkXyqe6tDdgNuLgk3e90JQ

如何发现品牌潜客？目标人群优选算法模型及实践解析

https://mp.weixin.qq.com/s/jCXfu6AHFWnQL118r38Zpw

全球顶级算法赛事Top5选手，跟你聊聊推荐系统领域的“战斗机”

https://mp.weixin.qq.com/s/rzCqz2HasT5zxBQjHy13Tw

前深度学习时代CTR预估模型的演化之路：从LR到FFM

https://mp.weixin.qq.com/s/Sc6VR--Qzk1BpS638SpS9Q

盘点前深度学习时代阿里、谷歌、Facebook的CTR预估模型

https://mp.weixin.qq.com/s/iEMGS1tbNPYXYIM7pKkq4A

达观数据：计算广告系统算法与架构综述

https://mp.weixin.qq.com/s/pWcFuOecG-dZHZ365clDjg

阿里妈妈新突破！深度树匹配如何扛住千万级推荐系统压力

https://mp.weixin.qq.com/s/R6-y-W0CGhlEUPXDkYdtJw

Item-Based协同过滤算法

https://mp.weixin.qq.com/s/C5cokipqnSsgg53chbi3oA

我是怎么走上推荐系统这条（不归）路的……

https://mp.weixin.qq.com/s/RA6Elm6C4gnlg0wIOnvlug

一步步构建推荐系统

https://mp.weixin.qq.com/s/WV0igXcxFuTNKU5MvZrl0Q

再谈亚马逊Item-based推荐系统

https://mp.weixin.qq.com/s/mXE3f2ZO6wQRSURZGmyBpQ

产品聚类

https://mp.weixin.qq.com/s/lvbXtLfa6Z4h_5lCHKn_6A

CTR点击率预估之经典模型回顾

https://mp.weixin.qq.com/s/GowWRaE5mZoPVAPK6_AkrA

如何构建可解释的推荐系统？

https://mp.weixin.qq.com/s/7QrSSEiLjmGjNhFfU6ue0A

推荐系统产品与算法概述

https://mp.weixin.qq.com/s/49o_AobVNs_9Lgm4qbQcPA

一文全面了解基于内容的推荐算法

https://mp.weixin.qq.com/s/uxi0OTxjmn047fE8nbYplw

推荐系统：石器与青铜时代

https://zhuanlan.zhihu.com/p/74978160

揭开Top-N经典算法SLIM和FISM之谜

https://mp.weixin.qq.com/s/S9-GgQTlx2c14mN_3kNQiQ

基于标签的实时短视频推荐系统

https://mp.weixin.qq.com/s/sw16_sUsyYuzpqqy39RsdQ

阿里妈妈深度树检索技术（TDM）及应用框架的探索实践

https://mp.weixin.qq.com/s/_zSe_Ia4DPrFKqsqW3iQ8w

电商推荐那点事

https://mp.weixin.qq.com/s/frEjB8SznDzJxOL3bkHnYw

矩阵分解推荐算法

http://www.infoq.com/cn/articles/we-are-bringing-learning-to-rank-to-elasticsearch

在Elasticsearch中应用机器学习排序LTR

https://mp.weixin.qq.com/s/IrpIMNxQ4frgBKl5pbsfdg

达观数据：用好学习排序(LTR),资讯信息流推荐效果翻倍

https://mp.weixin.qq.com/s/8vdl8QOJNTP6Wekm4LR3mA

LTR那点事—AUC及其与线上点击率的关联详解

https://mp.weixin.qq.com/s/MtnHYmPVoDAid9SNHnlzUw

阿里妈妈首次公开自研CTR预估核心算法MLR

https://mp.weixin.qq.com/s/G5a0YK39RZzgce_szbwoTA

你点一次广告，会创造多少价值？

https://mp.weixin.qq.com/s/zXABOzbQRV7sfgAhr_WqZw

跨领域推荐系统文献综述（上）

https://mp.weixin.qq.com/s/OxUCIhoIED_Z1Cf91Va36g

跨领域推荐系统文献综述（下）

https://mp.weixin.qq.com/s/PwIDOeC-wPshZkGet1pE2w

基于朴素ML思想的协同过滤推荐算法

https://mp.weixin.qq.com/s/lScxlqHARGYbs1eD3ZuEAQ

推荐系统-大规模信息网络Embedding表征学习

https://blog.csdn.net/yz930618/article/details/84862751

《基于行列式点过程的推荐多样性提升算法》原理详解

https://mp.weixin.qq.com/s/NJIEqlW4oKfEon3YXc1U6g

混合推荐系统介绍

https://mp.weixin.qq.com/s/TMxvs4TXyNo8FojbgO3K3w

推荐多样性重排算法之MMR

https://zhuanlan.zhihu.com/p/67287992

推荐系统研究中常用的评价指标

https://mp.weixin.qq.com/s/wIMVTZPOc4iGVRrnimbZhg

推荐系统从入门到接着入门

https://mp.weixin.qq.com/s/Ppvz4KkzWkMy6jXfbv6VqA

推荐系统之矩阵分解家族

https://mp.weixin.qq.com/s/eIldlMXri3WbyTzc3HeXsA

个性化搜索的介绍，推荐和搜索的强强结合

https://mp.weixin.qq.com/s/urGCBjJegKwIqkcCBelptA

深入理解推荐系统：召回

https://mp.weixin.qq.com/s/sY_EFq7XF1ab2_0nv19XTg

KDD Cup 2020推荐系统赛道—数据分析

https://mp.weixin.qq.com/s/ZieyCARzUGqa_7ME57WGtA

用机器学习来提升你的用户增长：第一步，了解你的目标

https://mp.weixin.qq.com/s/B7BV0mp-kfmyFZIf77mg2g

Netflix推荐算法，让每个人看到不一样的电影海报

https://mp.weixin.qq.com/s/S5_7wQynrBR0yLtNRKC-kw

多模型融合推荐算法在达观数据的运用

https://mp.weixin.qq.com/s/2Fu3s1Hy6ZW8Ex8yLPDnyQ

提速100倍，秒杀传统A/B测试！Netflix推出交错测试个性化推荐算法

https://mp.weixin.qq.com/s/6OMMboBdbLVA-HsEjA3bSA

Ctr预估之Calibration

# 推荐算法中的常用排序算法

![](/images/img3/ranking.png)

## Pointwise方法

Pranking (NIPS 2002), OAP-BPM (EMCL 2003), Ranking with Large Margin Principles (NIPS 2002), Constraint Ordinal Regression (ICML 2005)。

## Pairwise方法

Learning to Retrieve Information (SCC 1995), Learning to Order Things (NIPS 1998), Ranking SVM (ICANN 1999), RankBoost (JMLR 2003), LDM (SIGIR 2005), RankNet (ICML 2005), Frank (SIGIR 2007), MHR(SIGIR 2007), Round Robin Ranking (ECML 2003), GBRank (SIGIR 2007), QBRank (NIPS 2007), MPRank (ICML 2007), IRSVM (SIGIR 2006)。
