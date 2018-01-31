---
layout: post
title:  机器学习（三十四）——机器学习语录
category: ML 
---

# 机器学习语录

这里收录一些网上的只言片语式的心得，以区别于一般的教程。

>首先要考虑你的数据维度是线性相关的还是非线性相关的，数据是稀疏的还是稠密的，正例反例比例是多少，数据量是否充足。数据是否具有可分类性。是否需要降维。是否有噪音，是否有异常点等，然后去选择分类策略。通常包括数据采集，预处理，分类训练，预测，后处理等过程。

## 角度值的特征化

角度值是数据分析中常见的值，然而它不是线性的，比如0度和359度之间只相差1度，然而数值上却差了359度，因此无法将角度值直接代入线性回归等模型。因为后者的loss函数是用线性的欧氏距离定义的，角度显然不满足要求。

既然角度在一维上不是线性的，那么二维呢？没错，可以采用复数坐标(x,y)来表示角度，这样角度就是线性的了。

## 数值计算中的小技巧

1.两个数值趋近于无穷小且相近的数字相除的结果应该大约为1，但却因为分母接近为0而变成无穷。这时可以直接取对数执行运算，其结果在数值上会较为稳定。

## 相关关系不等同于因果关系

我们可以通过一句调侃的话来解释：“地球变暖、地震、龙卷风，以及其他自然灾害，都和18世纪以来全球海盗数量的减少有直接关系”。这两个变量的变化有相关性，但是并不能说存在因果关系，因为往往存在第三类（甚至第4、5类）未被观察到的变量在起作用。相关关系应该看作是潜在的因果关系的一定程度的体现，但需要进一步研究。

# MEMM

Maximum Entropy Markov Model

http://www.cnblogs.com/en-heng/p/6201893.html

中文分词：最大熵马尔可夫模型MEMM

http://www.cnblogs.com/549294286/archive/2013/06/06/3121761.html

统计模型之间的比较，HMM，最大熵模型，CRF条件随机场

http://blog.csdn.net/caohao2008/article/details/4242308

HMM,MEMM,CRF模型的比较

http://blog.csdn.net/zhoubl668/article/details/7787690

标注偏置问题(Label Bias Problem)和HMM、MEMM、CRF模型比较

# CRF

条件随机场(Conditional Random Field)由Lafferty等人于2001年提出，结合了最大熵模型和隐马尔可夫模型的特点，是一种无向图模型，近年来在分词、词性标注和命名实体识别等序列标注任务中取得了很好的效果。

https://mp.weixin.qq.com/s/heYpeVLrZBRjXtMQou2BAA

如何轻松愉快的理解条件随机场（CRF）？

http://www.chokkan.org/software/crfsuite/

CRFsuite: A fast implementation of Conditional Random Fields (CRFs)

https://github.com/scrapinghub/python-crfsuite

A python binding for crfsuite

http://taku910.github.io/crfpp/

CRF++: Yet Another CRF toolkit

# 异常点检测

http://chuansong.me/n/377440751130

异常点检测算法（一）

http://jiangshuxia.9.blog.163.com/blog/static/3487586020083662621887/

异常(Outlier)检测算法综述

http://www.cnblogs.com/fengfenggirl/p/iForest.html

异常检测算法--Isolation Forest

https://mp.weixin.qq.com/s/xsuLIMPJVCThBGMRlz09Hg

Isolation Forest算法原理详解

# 自适应滤波器

《自适应滤波器原理》，Simon Haykin著。

## 基本估计

三种基本的信息处理运算：

**滤波（Filter）**：利用$$[0,t]$$的数据，来估计t时刻信息的运算过程。

**平滑（Smoothing）**：利用$$[0,t]$$的数据，来估计$$t'(t'<t)$$时刻信息的运算过程。

**预测（Prediction）**：利用$$[0,t]$$的数据，来估计$$t+\tau(\tau>0)$$时刻信息的运算过程。

可见，滤波和预测是实时运算，而平滑是非实时运算。

# Robust PCA

http://www.cnblogs.com/quarryman/p/robust_pca.html

最优化之Robust PCA

http://www.aiuxian.com/article/p-2634727.html

Robust PCA

http://blog.csdn.net/abcjennifer/article/details/8572994

Robust PCA学习笔记

http://patternrecognition.cn/~jin/gs/seminar/20140515_jinzhong.ppt

Robust PCA-模式识别

# 热传导推荐算法

https://www.zhihu.com/question/20184666

推荐系统中用到的热传导算法和物质扩散是怎么用的？

http://tis.hrbeu.edu.cn/oa/pdfdow.aspx?Sid=20160307

基于影响力控制的热传导算法

http://www.doc88.com/p-7082821463697.html

改进的热传导和物质扩散混合推荐算法

# 强连通分量算法

http://ishare.iask.sina.com.cn/f/34626295.html

矩阵不可约的充要条件

http://www.cnblogs.com/saltless/archive/2010/11/08/1871430.html

求强连通分量的Tarjan算法

http://blog.csdn.net/dm_vincent/article/details/8554244

求解强连通分量算法之---Kosaraju算法

http://www.cnblogs.com/luweiseu/archive/2012/07/14/2591370.html

强连通分支算法--Kosaraju算法、Tarjan算法和Gabow算法

