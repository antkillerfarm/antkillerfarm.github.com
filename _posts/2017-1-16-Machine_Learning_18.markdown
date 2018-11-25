---
layout: post
title:  机器学习（十八）——独立成分分析, 贝叶斯线性回归, 强连通分量算法, 异常点检测, Beam Search
category: ML 
---

## 累积分布函数

累积分布函数（cumulative distribution function，CDF）是概率论中的一个基本概念。它的定义如下：

$$F(z_0)=P(z\le z_0)=\int_{-\infty}^{z_0}p_z(z)\mathrm{d}z$$

可以看出：

$$p_z(z)=F'(z)$$

## ICA算法

ICA算法归功于Bell 和 Sejnowski，这里使用最大似然估计来解释算法。（原始论文中使用的是一个复杂的方法Infomax principal，这在最新的推导中已经不需要了。）

>注：Terrence (Terry) Joseph Sejnowski，1947年生，美国科学家。普林斯顿大学博士，导师是神经网络界的大神John Hopfield。ICA算法和Boltzmann machine的发现人。

>Tony Bell的个人主页：
>http://cnl.salk.edu/~tony/index.html

我们假定每个$$s_i$$有概率密度$$p_s$$，那么给定时刻原信号的联合分布就是：

$$p(s)=\prod_{i=1}^np_s(s_i)$$

因此：

$$p(x)=\prod_{i=1}^np_s(w_i^Tx)\cdot \mid W\mid \tag{2}$$

为了确定$$s_i$$的概率密度，我们首先要确定它的累计分布函数$$F(x)$$。而这需要满足两个性质：单调递增和在$$[0,1]$$区间。

我们发现sigmoid函数很适合，它的定义域负无穷到正无穷，值域0到1，缓慢递增。因此，可以假定s的累积分布函数符合sigmoid函数：

$$g(s)=\frac{1}{1+e^{-s}}$$

求导，可得：

$$p_s(s)=g'(s)=g(s)(1-g(s))$$

这里的推导参见《机器学习（一）》的公式7。

>注：如果有其他先验信息的话，这里的$$g(s)$$也可以使用其他函数。否则的话，sigmoid函数能够在大多数问题上取得不错的效果。

公式2的对数似然估计函数为：

$$\ell(W)=\sum_{i=1}^m\left(\sum_{j=1}^m\log g'(w_j^Tx^{(i)})+\log\mid W\mid \right)\tag{3}$$

因为：

$$\begin{align}
(\log g'(s))'&=(\log [g(s)(1-g(s))])'=(\log g(s))'+(\log (1-g(s)))'
\\&=\frac{g'(s)}{g(s)}+\frac{(1-g(s))'}{(1-g(s))}=\frac{g(s)(1-g(s))}{g(s)}-\frac{g(s)(1-g(s))}{(1-g(s))}
\\&=1-2g(s)
\end{align}$$

又因为《机器学习（十）》的公式5.11，可得公式3的导数为：

$$\nabla_W\ell(W)=\begin{bmatrix}
1-2g(w_1^Tx^{(i)}) \\
1-2g(w_2^Tx^{(i)}) \\
\vdots \\
1-2g(w_n^Tx^{(i)})
\end{bmatrix}x^{(i)^T}+(W^T)^{-1}$$

最后，用通常的随机梯度上升算法，求得$$\ell(W)$$的最大值即可。

>注意：我们计算最大似然估计时,假设了$$x^{(i)}$$和$$x^{(j)}$$之间是独立的，然而对于语音信号或者其他具有时间连续依赖特性(比如温度)上，这个假设不能成立。但是在数据足够多时，假设独立对效果影响不大。

# 贝叶斯线性回归

https://mp.weixin.qq.com/s/szTmHY-Yvn7N3s_GzTDiEA

解开贝叶斯黑暗魔法：通俗理解贝叶斯线性回归

https://mp.weixin.qq.com/s/1JSxjkKEUlWOzXCQPTve3A

贝叶斯线性回归简介

https://mp.weixin.qq.com/s/NTK-u4aVrTTmvi-4ZBa8RQ

数十亿用户的Facebook如何进行贝叶斯系统调优？

https://mp.weixin.qq.com/s/g24mcZjQ25sQJx9mqE_XSA

怎样判断漂亮女孩是不是单身的？美国海军在汪洋大海里搜索丢失的氢弹、失踪的核潜艇都用过这种方法。

# 强连通分量算法

http://ishare.iask.sina.com.cn/f/34626295.html

矩阵不可约的充要条件

http://www.cnblogs.com/saltless/archive/2010/11/08/1871430.html

求强连通分量的Tarjan算法

http://blog.csdn.net/dm_vincent/article/details/8554244

求解强连通分量算法之---Kosaraju算法

http://www.cnblogs.com/luweiseu/archive/2012/07/14/2591370.html

强连通分支算法--Kosaraju算法、Tarjan算法和Gabow算法

# 异常点检测

http://chuansong.me/n/377440751130

异常点检测算法（一）

http://jiangshuxia.9.blog.163.com/blog/static/3487586020083662621887/

异常(Outlier)检测算法综述

http://www.cnblogs.com/fengfenggirl/p/iForest.html

异常检测算法--Isolation Forest

https://mp.weixin.qq.com/s/xsuLIMPJVCThBGMRlz09Hg

Isolation Forest算法原理详解

http://www.bigdata8.top/front/article/466

异常值检测-滑动均值实现智能告警

https://mp.weixin.qq.com/s/ujG6Fr161kZh3S-lEiTJUg

异常检测（Anomaly Detection）

https://mp.weixin.qq.com/s/_Sds7O1wonVARKkb7qscww

腾讯：机器学习构建通用的数据异常检测平台

https://mp.weixin.qq.com/s/UcMPIf6ZRAhPjn79H2n1ig

异常点检测算法小结

https://mp.weixin.qq.com/s?__biz=MzIxODM4MjA5MA==&mid=2247487342&idx=3&sn=1981a6caec4591a19043d7a6176d359f

异常检测的阈值，你怎么选？给你整理好了...

https://mp.weixin.qq.com/s/ReQpT9KT6_tE8vXM-F_Ejw

从“马蜂窝事件”看，投资人如何避免数据尽职调查背后的交易风险？新时代数据造假特征及应对方法

https://www.zhihu.com/question/30508773

反欺诈(Fraud Detection)中所用到的机器学习模型有哪些？

# Beam Search

## 概述

Beam Search（集束搜索）是一种启发式图搜索算法，通常用在图的解空间比较大的情况下，为了减少搜索所占用的空间和时间，在每一步深度扩展的时候，剪掉一些质量比较差的结点，保留下一些质量较高的结点。保留下来的结点个数一般叫做Beam Width。

这样减少了空间消耗，并提高了时间效率，但缺点就是有可能存在潜在的最佳方案被丢弃，因此Beam Search算法是不完全的，一般用于解空间较大的系统中。

![](/images/article/beam_search.png)

上图是一个Beam Width为2的Beam Search的剪枝示意图。每一层只保留2个最优的分支，其余分支都被剪掉了。

显然，Beam Width越大，找到最优解的概率越大，相应的计算复杂度也越大。因此，设置合适的Beam Width是一个工程中需要trade off的事情。

当Beam Width为1时，也就是著名的A*算法了。

Beam Search主要用于机器翻译、语音识别等系统。这类系统虽然从理论来说，也就是个多分类系统，然而由于分类数等于词汇数，简单的套用softmax之类的多分类方案，明显是计算量过于巨大了。

PS：中文验证码识别估计也可以采用该技术。

## Beam Search与Viterbi算法

Beam Search与Viterbi算法虽然都是解空间的剪枝算法，但它们的思路是不同的。

Beam Search是对状态迁移的路径进行剪枝，而Viterbi算法是合并不同路径到达同一状态的概率值，用最大值作为对该状态的充分估计值，从而在后续计算中，忽略历史信息（这种以偏概全也就是所谓的Markov性），以达到剪枝的目的。

从状态转移图的角度来说，Beam Search是空间剪枝，而Viterbi算法是时间剪枝。

## 参考

http://people.csail.mit.edu/srush/optbeam.pdf

Optimal Beam Search for Machine Translation

http://www.cnblogs.com/xxey/p/4277181.html

Beam Search（集束搜索/束搜索）

http://blog.csdn.net/girlhpp/article/details/19400731

束搜索算法（Andrew Jungwirth 初稿）BEAM Search

http://hongbomin.com/2017/06/23/beam-search/

Beam Search算法及其应用

https://mp.weixin.qq.com/s/GTtjjBgCDdLRwPrUqfwlVA

如何使用贪婪搜索和束搜索解码算法进行自然语言处理
