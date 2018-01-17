---
layout: post
title:  机器学习（三十）——垃圾筐, NLP机器翻译常用评价度量
category: ML 
---

# 垃圾筐

高斯背景

laurent series

Base32/Base36/Base64/Base85

Reinforcement Learning and Control

Linear Discriminant Analysis

https://mp.weixin.qq.com/s/7Vy7l1YyBT7rMYW2i1AsuA

线性判别分析(LDA)原理详解

Partial Least Squares Discriminant Analysis

http://blog.codinglabs.org/articles/algorithms-for-cardinality-estimation-part-i.html

解读Cardinality Estimation算法

http://blog.csdn.net/icvpr/article/details/12342159

局部敏感哈希(Locality-Sensitive Hashing, LSH)方法介绍

https://mp.weixin.qq.com/s/zQJve_w5OoM6u-WcSWArdQ

神速Hash

http://www.cnblogs.com/wentingtu/archive/2012/03/13/2393993.html

Learning to Rank入门小结

FPN (Feature Pyramid Network)

http://www.cnblogs.com/pinard/p/6221564.html

谱聚类（spectral clustering）原理总结

https://mp.weixin.qq.com/s/NlJ4-b5SjIjPGgvLUuSxFw

孩子，有时候并不是生活欺骗了你，而是你可能还不懂概率统计……

http://blog.csdn.net/luo123n/article/details/48574123

PMI（Pointwise Mutual Information）

http://www.cnblogs.com/6DAN_HUST/archive/2010/11/11/1874681.html

运筹学——线性规划及单纯形法求解

https://mp.weixin.qq.com/s/E_EzwLr4JOzR1pk3MOeU_w

复动力系统简介

https://mp.weixin.qq.com/s/QmDCFY0SP3cl71HvwvthJQ

如果高斯没有故意“坑”黎曼，估计这门神奇的学科就不会出现了……

https://mp.weixin.qq.com/s/QJQnRCrBu9j94-VhNq_4tA

群论的创立：两个少年天才的接力

## 三门问题

https://www.zhihu.com/question/26709273/

蒙提霍尔问题（又称三门问题、山羊汽车问题）的正解是什么？

https://zhuanlan.zhihu.com/p/21461266

数学杂谈——“三门问题”：Monty Hall Problem

https://zhuanlan.zhihu.com/p/23338174

蒙提霍尔问题/三门问题（Monty Hall problem）

# ACBM算法

ACBM算法是在AC（Aho-Corasick）自动机（UNIX上的fgrep命令使用的就是AC算法）的基础之上，引入了BM（Boyer-Moore）算法的多模扩展，实现的高效的多模匹配。和AC自动机不同的是，ACBM算法不需要扫描目标文本串中的每一个字符，可以利用本次匹配不成功的信息，跳过尽可能多的字符，实现高效匹配。

>注： Alfred Vaino Aho，1941年生，加拿大计算机科学家。普林斯顿大学博士，长期供职于贝尔实验室，后为哥伦比亚大学教授。egrep和fgrep的发明人，AWK语言的联合发明人。著有《Principles of Compiler Design Compilers: Principles, Techniques, and Tools》。该书由于封面上有龙图案，又被称为“龙书”，是编译原理方面的权威书籍。2003年获IEEE John von Neumann Medal。

>Margaret John Corasick，贝尔实验室研究员。

>Robert Stephen Boyer，德克萨斯大学教授。

>J Strother Moore，德克萨斯大学教授。Boyer的好朋友，两人的绝大多数成就都是合作完成的。

参见：

http://blog.csdn.net/sealyao/article/details/6817944

ACBM算法

https://mp.weixin.qq.com/s/yVOgAuD9hEYAMdLyvae2VA

最长公共子序列与最长公共子串

https://mp.weixin.qq.com/s/8rP3fF9iVnplhkFmxuj6fw

一文读懂KMP算法

https://mp.weixin.qq.com/s/if7P0yq59DbBEjW15vfaQA

推荐一个高效算法wumanber：每秒680万匹配！

# 高斯过程回归

从大的分类来说，机器学习的算法可分为两类：

1.定义一个模型，用训练数据训练模型的参数，然后用训练好的模型进行预测。这种方法的缺点在于，预测效果和模型与样本的匹配程度有关。比如对非线性样本采用线性模型，其预测效果通常不会太好。但是增加模型的复杂度，又会导致过拟合。

2.定义一个函数分布，赋予每一种可能的函数一个先验概率，可能性越大的函数，其先验概率越大。但是可能的函数往往为一个不可数集，即有无限个可能的函数，随之引入一个新的问题：如何在有限的时间内对这些无限的函数进行选择？一种有效解决方法就是高斯过程回归(Gaussian process regression，GPR)。

>注：Radford M. Neal，1956年生，加拿大科学家。多伦多大学博士（1995）和教授。贝叶斯神经网络的发明人。导师为Geoffrey Hinton。   
>个人主页：http://www.cs.toronto.edu/~radford/

>Danie G. Krige，1919～2013，南非矿业工程师和统计学家，威特沃特斯兰德大学教授。地理统计学早期的代表人物之一。

http://www.cnblogs.com/hxsyl/p/5229746.html

浅谈高斯过程回归

https://mqshen.gitbooks.io/prml/content/Chapter6/gaussian/gaussian_processes_regression.html

Gaussian Processes Regression

http://www.gaussianprocess.org/gpml/chapters/RW.pdf

Gaussian Processes for Machine Learning

http://people.cs.umass.edu/~wallach/talks/gp_intro.pdf

Introduction to Gaussian Process Regression

http://wenku.baidu.com/view/72f80113915f804d2b16c173.html

高斯过程回归方法综述

https://mp.weixin.qq.com/s/ZE_Chzlgy_7wl9E9q1ckOA

AlphaGo Zero用它来调参？“高斯过程”到底有何过人之处？

# 莫比乌斯反演

http://blog.csdn.net/acdreamers/article/details/8542292

莫比乌斯反演

http://blog.csdn.net/litble/article/details/72804050

初涉莫比乌斯反演

http://blog.csdn.net/u013632138/article/details/52250567

莫比乌斯反演讲解

http://blog.csdn.net/Ripped/article/details/70263965

莫比乌斯反演详解

http://blog.csdn.net/hzj1054689699/article/details/51659994

莫比乌斯反演入门

http://blog.csdn.net/outer_form/article/details/50588307

莫比乌斯反演定理证明

# NLP机器翻译常用评价度量

机器翻译的评价指标主要有：BLEU、NIST、Rouge、METEOR等。

参考：

http://blog.csdn.net/joshuaxx316/article/details/58696552

BLEU，ROUGE，METEOR，ROUGE-浅述自然语言处理机器翻译常用评价度量

http://blog.csdn.net/guolindonggld/article/details/56966200

机器翻译评价指标之BLEU

http://blog.csdn.net/han_xiaoyang/article/details/10118517

机器翻译评估标准介绍和计算方法

http://blog.csdn.net/lcj369387335/article/details/69845385

自动文档摘要评价方法---Edmundson和ROUGE

https://mp.weixin.qq.com/s/XiZ6Uc5cHZjczn-qoupQnA

对话系统评价方法综述


