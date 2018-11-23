---
layout: post
title:  机器学习（二十六）——贝叶斯线性回归, 强连通分量算法, 异常点检测, Beam Search, K-摇臂赌博机
category: ML 
---

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

# K-摇臂赌博机

![](/images/article/Multi-Armed_Bandit.jpg)

k-armed Bandit（也叫Multi-Armed Bandit）是赌场里的一种赌具。它有K个摇臂，投币后摇动摇臂，会有一定的概率吐出硬币。每个摇臂的吐币概率和数量有所不同。（有的机器只有1个摇臂，但可通过按钮设置不同的方案。）赌徒的目标是通过一定的策略获得最多的奖励（硬币）。

尽管在有的赌场中，每个摇臂的吐币概率和数量是已知的，但在本问题中，**吐币概率和数量都是未知的。**

由于每次摇臂都是独立事件，因此k-armed Bandit问题的另一个约束是：**最大化单步奖励**，即不考虑未来的奖励。

此外，k-armed Bandit亦不可能无限进行下去，其**尝试总数是一定的**（即投币数是一定的）。这也是该问题的一个隐含约束。

这里显然有两个最简单的策略：

**exploration-only**：将所有尝试机会平均分配给每个摇臂。这种策略可以很好的估计每个摇臂的奖励，然而却会失去很多选择最优摇臂的机会。

**exploitation-only**：只按下目前最优的摇臂。这种策略下有可能选不到最优的摇臂。

显然，欲奖励最大，需要在exploration和exploitation之间达成较好的折中。

## $$\epsilon$$-贪心算法

$$\epsilon$$-greedy基于概率对exploration和exploitation进行折中，即：**以$$\epsilon$$进行exploration，而以$$1-\epsilon$$进行exploitation。**

若摇臂奖励的不确定性较大，即概率分布较宽时，需要较大的$$\epsilon$$值，反之则小。

另外，开始时由各种信息较少，$$\epsilon$$需要设置的大一些，随着探索的深入，各摇臂的奖励基本弄清楚之后，$$\epsilon$$就可以小一些了。因此通常令$$\epsilon=1/\sqrt{t}$$。

当$$\epsilon=0$$时，该算法也被称为greedy算法，也就是之前提到的exploitation-only策略。

必须指出的是，k-armed Bandit只是真实问题的一个极度简化模型：它只有action和reward，没有input或sequentiality，也没有state。

## Softmax算法

Softmax算法和$$\epsilon$$-贪心算法类似，其公式为：

$$P(k)=\frac{e^{\frac{Q(k)}{\tau}}}{\sum_{i=1}^{K}e^{\frac{Q(i)}{\tau}}}$$

这实际上是一个Boltzmann分布的公式，其中$$\tau$$表示温度，$$\tau \to 0$$表示优先利用，$$\tau \to \infty$$表示优先探索。

## 求均值的小技巧

在k-armed Bandit问题中，一般用reward的均值来近似它的数学期望值。由于action是个序列，因此相应的算法一般是个不断迭代的过程，或者也可以说是一个增量（incremental）过程。

在增量过程中，保存所有的reward值来计算均值显然是个笨办法。这里常用的办法是：

$$Q_{n+1}=Q_n+\frac{1}{n}[R_n-Q_n]$$

不光reward值可以这样更新，其他均值也可采用这个方法：

$$NewEstimate \leftarrow OldEstimate + StepSize [Target - OldEstimate]$$

## 非平稳问题

在之前的假设中，我们认为每个摇臂的吐币概率和数量是不随时间变化的，这样的问题被称为Stationary Problem。

如果每个摇臂的吐币概率和数量随时间**缓慢变化**的话，则称之为Non-stationary Problem。

>注：快速变化的系统，不光RL无能，其他方法估计也没什么好效果。所以系统保持一定的惯性，对于研究问题是很重要的。

这时一般采用如下公式滤波：

$$Q_{n+1}=Q_n+\alpha[R_n-Q_n]$$

详细内容参见《数学狂想曲（四）》的“软件滤波算法”一节的“一阶滞后滤波法”和“加权递推平均滤波法”。
