---
layout: post
title:  机器学习（二十六）——贝叶斯线性回归, 强连通分量算法, K-摇臂赌博机
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
