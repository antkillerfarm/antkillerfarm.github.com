---
layout: post
title:  机器学习（四十）——Adaboost, 特征工程
category: ML 
---

* toc
{:toc}

# Adaboost

Adaboost是Yoav Freund和Robert Schapire于1997年提出的算法。两人后来因为该算法被授予Gödel Prize（2003）。

>Yoav Freund，UCSC博士，UCSD教授。

>Robert Elias Schapire，MIT博士。先后供职于Princeton University、AT&T Labs和Microsoft Research。

>Gödel Prize，由欧洲计算机学会（EATCS）与美国计算机学会基础理论专业组织（ACM SIGACT）于1993年共同设立，颁给理论计算机领域最杰出的学术论文。其名称取自Kurt Gödel。

>Kurt Friedrich Gödel，1906～1978，奥地利逻辑学家，数学家，哲学家，后加入美国藉。维也纳大学博士（1930）。在逻辑学方面，他是继Aristotle、Gottlob Frege之后最伟大的逻辑学家。在数学方面，他以哥德尔不完备定理著称，和Bertrand Russell、 David Hilbert、Georg Cantor齐名。

Adaboost既可用于分类问题，也可用于回归问题。这里仅针对二分类问题进行讨论。

假设我们有数据集$$\{(x_1, y_1), \ldots, (x_N, y_N)\}$$，其中$$y_i \in \{-1, 1\}$$，还有一系列弱分类器$$\{k_1, \ldots, k_L\}$$。

由于Boost算法是个串行算法，每次迭代就会加入一个弱分类器。这样m-1次迭代之后的分类器如下所示：

$$C_{(m-1)}(x_i) = \alpha_1k_1(x_i) + \cdots + \alpha_{m-1}k_{m-1}(x_i)$$

而m次迭代之后的分类器则为：

$$C_{m}(x_i) = C_{(m-1)}(x_i) + \alpha_m k_m(x_i)$$

如何选择新加入的弱分类器$$k_m$$和对应的权重$$\alpha_m$$呢？我们可以定义误差E如下所示：

$$E = \sum_{i=1}^N e^{-y_i C_m(x_i)}$$

令$$w_i^{(1)} = 1,w_i^{(m)} = e^{-y_i C_{m-1}(x_i)}$$，则：

$$E = \sum_{i=1}^N w_i^{(m)}e^{-y_i\alpha_m k_m(x_i)}$$

因为$$k_m$$分类正确时，$$y_i k_m(x_i) = 1$$，分类错误时，$$y_i k_m(x_i) = -1$$。所以：

$$E = \sum_{y_i = k_m(x_i)} w_i^{(m)}e^{-\alpha_m} + \sum_{y_i \neq k_m(x_i)} w_i^{(m)}e^{\alpha_m}\\= \sum_{i=1}^N w_i^{(m)}e^{-\alpha_m} + \sum_{y_i \neq k_m(x_i)} w_i^{(m)}(e^{\alpha_m}-e^{-\alpha_m})$$

可以看出和$$k_m$$相关的实际上只有上式的右半部分。显然，使得$$\sum_{y_i \neq k_m(x_i)} w_i^{(m)}$$最小的$$k_m$$，也会令E最小，这也就是我们选择加入的$$k_m$$。

对E求导，得：

$$\frac{d E}{d \alpha_m} = \frac{d (\sum_{y_i = k_m(x_i)} w_i^{(m)}e^{-\alpha_m} + \sum_{y_i \neq k_m(x_i)} w_i^{(m)}e^{\alpha_m}) }{d \alpha_m}$$

令导数为0，可得：

$$\alpha_m = \frac{1}{2}\ln\left(\frac{\sum_{y_i = k_m(x_i)} w_i^{(m)}}{\sum_{y_i \neq k_m(x_i)} w_i^{(m)}}\right)$$

令$$\epsilon_m = \sum_{y_i \neq k_m(x_i)} w_i^{(m)} / \sum_{i=1}^N w_i^{(m)}$$，则：

$$\alpha_m = \frac{1}{2}\ln\left( \frac{1 - \epsilon_m}{\epsilon_m}\right)$$

参考：

https://mp.weixin.qq.com/s/G06VDc6iTwmNGsH4IfSeJQ

Adaboost从原理到实现

https://mp.weixin.qq.com/s/PZ-1fkNvdJmv_8zLbvoW1g

Adaboost算法原理小结

https://mp.weixin.qq.com/s/KoOUgwXLOfJfOjWhbFX52Q

如果Boosting你懂，那Adaboost你懂么？

# 模型驱动 vs 数据驱动

最近阅读了这篇文章，深有感慨：

https://mp.weixin.qq.com/s/N7DE0kvf8THhJQwroHj4vA

成不了AI高手？因为你根本不懂数据！听听这位老教授多年心血练就的最实用统计学

>注：吴喜之教授是我国著名的统计学家，退休前在中国人民大学统计学院任统计学教授。吴教授上世纪六十年代就读于北京大学数学力学系，八十年代出国深造，在美国北卡罗来纳大学获得统计学博士学位，是改革开放之后第一批留美并获得统计学博士学位的中国学者。多年来吴教授在国内外数十所高校讲授统计学课程，在国内统计学界享有盛誉。其知名的学生有李舰和刘思喆。

>李舰，从2003年开始，一直把R当作随身武器奋战在统计学和数据分析的第一线，是Rweibo、Rwordseg、tmcn等高质量R包的作者，在业界积累了大量的经验，目前供职于Mango Solutions（中国），任数据总监。

>刘思喆，2012至2016年就职于京东商城，推荐系统平台部高级经理，主要负责和推荐系统离线、在线相关的用户行为、商品特征的建模，以及数据监控平台。因工作业绩，在《京东技术解密》一书中获“数据达人”称号。

# 特征工程

https://mp.weixin.qq.com/s/ibiElLIgrT3wYx3tDYMMTw

理解特征工程

https://mp.weixin.qq.com/s/3Ce8uMf_Kyt-hEZUYfdh3g

特征工程之特征选择

https://mp.weixin.qq.com/s/tOcyfK68jW7Tr-PGCvdXMA

特征工程最后一个要点:特征预处理

https://mp.weixin.qq.com/s/GWMZ1jwbchE8O0r6EduYtQ

一文讲解特征工程！经典外文PPT及中文解析

https://mp.weixin.qq.com/s/c9iHdgtErVd_iitwny7_zw

Kaggle前1%参赛者经验：特征工程为何如此重要？

https://mp.weixin.qq.com/s/xbPJD0uoRB-T1x09AUYdzg

基于Python的自动特征工程——教你如何自动创建机器学习特征

https://mp.weixin.qq.com/s?__biz=MjM5MTQzNzU2NA==&mid=2651664000&idx=1&sn=ae6dda80df6d6278ae33b7bf7fbadcd2

深度特征合成：自动化特征工程的运作机制

https://mp.weixin.qq.com/s/R1MhoCfnd5drvg2CGLVsPw

哪种特征分析法适合你的任务？Ian Goodfellow提出显著性映射的可用性测试

https://mp.weixin.qq.com/s/XSovbUDVTKe59DDaC1Kl8Q

如何进行特征表达，你知道吗？

https://mp.weixin.qq.com/s/vhr5gXoa0S4-QqFcK7uz-w

模型吞噬特征工程

https://mp.weixin.qq.com/s/zgKbG3r_B8d1qQHnrD2NCg

特征工程宝典《Feature Engineering for Machine Learning》翻译及代码实现

https://mp.weixin.qq.com/s/3Clq9ECs6M52Sg-_xMxJGw

最核心的特征工程方法-分箱算法

https://mp.weixin.qq.com/s/ghfh1x_lsEcoA8PFPXE46w

练手扎实基本功必备：非结构文本特征提取方法

https://mp.weixin.qq.com/s?__biz=MzU2OTA0NzE2NA==&mid=2247515402&idx=1&sn=ee3cd5c64a707246216a532fa3af422b

面向机器学习和数据分析的特征工程

https://mp.weixin.qq.com/s/NKKk8nRd0qn5XhxXgYWknw

手把手带你入门和实践特征工程的万字笔记

https://mp.weixin.qq.com/s/QZeyEN2DDM_etEki7uodMg

一个神奇的特征选择轮子----MLFeatureSelection

https://mp.weixin.qq.com/s/8NI-NayCg_gZmJ6-1FZ_DA

一个Python特征选择工具，助力实现高效机器学习

https://mp.weixin.qq.com/s/LbXHpnC19euqriCtSHeg1Q

UC Berkeley提出特征选择新方法：条件协方差最小化

https://mp.weixin.qq.com/s/V3w5Iu804O6PmnBjmwCbgw

常用文本特征选择

https://mp.weixin.qq.com/s/Rj-ObD-eM5zEfs5fkWamGQ

三大特征选择策略，有效提升你的机器学习水准

https://mp.weixin.qq.com/s/rNipJC5wljzCT6Aq5gvvqw

一款功能强大的特征选择工具（FeatureSelector）

https://mp.weixin.qq.com/s/Bu34hPN0XAj6GmLXuQwVsQ

风控特征—关系网络特征工程入门实践

https://mp.weixin.qq.com/s/thd_dtd4erqSf7p6ZON72w

自动特征工程在推荐系统中的研究

https://zhuanlan.zhihu.com/p/96420594

特征工程架构性好文

https://mp.weixin.qq.com/s/demEVr5ZXKeSLbBIO1XgsQ

AutoFIS: 因数分解模型中用于预测点击率的自动特征交互选择

https://mp.weixin.qq.com/s/Z5cs6X1tFq9uKGfo3aHgmw

简介机器学习中的特征工程

https://mp.weixin.qq.com/s/BNiDjgBpdGQjCY-b96htlQ

机器学习中的特征工程总结

https://mp.weixin.qq.com/s/VBA02WHBJmU77RPLtIzprA

特征工程入门：应该保留和去掉那些特征

https://mp.weixin.qq.com/s/BfZ9BQXtOsEXCkAR3QYHhA

特征工程了解一下

https://mp.weixin.qq.com/s/dPnb7Mho-sQA6euvCdQV7w

类别特征目标编码

https://mp.weixin.qq.com/s/ZJjQY5g95p_s2Te9Rl2zIA

特征选择介绍及4种基于过滤器的方法来选择相关特征

https://mp.weixin.qq.com/s/q635XCJ3tVfesmTF4yvJ_w

categorical feature编码方法小结

https://mp.weixin.qq.com/s/vK5HP7e8d7ZXGFxaYN3G4g

我用特征工程+LR超过了xDeepFM

https://mp.weixin.qq.com/s/DdvXivS7OnAwC59nt5i3bg

天池项目总结，特征工程了解一下！

https://mp.weixin.qq.com/s/ktk8eUnu4-TyU3ob2y1fCA

特征交互新路线：阿里Co-action Network论文解读

https://mp.weixin.qq.com/s/FS7WJ1rG8Kt5Xp6H1InCAg

如何融合深度学习特征向量？

https://mp.weixin.qq.com/s/PAPHQ_Dq7ZqWvuSZQzuWag

样本组织篇

https://mp.weixin.qq.com/s/B0-VSkPhkDJkwpllHahJiQ

Null Importances

https://mp.weixin.qq.com/s/OESIXwjM8nFhz4NhNT1BzQ

使用神经网络的自动化特征工程
