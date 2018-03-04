---
layout: post
title:  机器学习（二十六）——K-摇臂赌博机
category: ML 
---

# 强化学习

## MDP（续）

注意State和Observation区别：State是Environment的私有表达，我们往往不会直接得到。

在MDP中，当前状态State包含了所有历史信息，即将来只和现在有关，与过去无关，因为现在状态包含了所有历史信息。只有满足这样条件的状态才叫做马尔科夫状态（Markov state）。当然这只是理想状况，现实往往不会那么简单。

正是因为State太过于复杂，我们往往可以需要一个对Environment的观察来间接获得信息，因此就有了Observation。不过Observation是可以等于State的，此时叫做Full Observability。

>这里可以类比围棋和星际争霸。前者的所有信息都在明面上，因此是Full Observability，而后者由于战争迷雾的存在，显然就不是Full Observability的了。

状态、动作、状态转移概率组成了MDP，一个MDP周期（episode）由一个有限的状态、动作、奖励队列组成：

$$s_0,a_0,r_1,s_1,a_1,r_2,s_2,\dots,s_{n-1},a_{n-1},r_n,s_n$$

这里$$s_i$$代表状态，$$a_i$$代表行动，$$r_{i+1}$$是执行动作后的奖励。最终状态为$$s_n$$。

## 折扣未来奖励（Discounted Future Reward）

为了获得更多的奖励，我们往往不能只看当前奖励，更要看将来的奖励。

给定一个MDP周期，总的奖励显然为：

$$R=r_1+r_2+\dots+r_n$$

那么，从当前时间t开始，总的将来的奖励为：

$$R_t=r_t+r_{t+1}+\dots+r_n$$

但是Environment往往是随机的，执行特定的动作不一定得到特定的状态，因此将来的奖励所占的权重要依次递减，因此使用discounted future reward代替：

$$R_t=r_t+\gamma r_{t+1}+\gamma^2 r_{t+2}+\dots+\gamma^{n-t}r_n$$

这里$$\gamma$$是0和1之间的折扣因子——越是未来的奖励，折扣越多，权重越小。而明显上式是个迭代过程，因此可以写作：

$$R_t=r_t+\gamma(r_{t+1}+\gamma (r_{t+2}+\dots))=r_t+\gamma R_{t+1}$$

即当前时刻的奖励等于当前时刻的即时奖励加上下一时刻的奖励乘上折扣因子$$\gamma$$。

如果$$\gamma$$等于0，意味着只看当前奖励；

如果$$\gamma$$等于1，意味着环境是确定的，相同的动作总会获得相同的奖励（也就是cyclic Markov processes）。

因此实际中$$\gamma$$往往取类似0.9这样的值。因此我们的任务变成了找到一个策略，最大化将来的奖励R。

## Policy, Value, Transition Model

增强学习中，比较重要的几个概念：

**Policy**就是我们的算法追求的目标，可以看做一个函数，在输入state的时候，能够返回此时应该执行的action或者action的概率分布。

$$\pi(a \mid  s) = P[A_t = a \mid  S_t = s]$$

**Value**，价值函数，表示在输入state，action的时候，能够返回在state下，执行这个action能得到的Discounted future reward的（期望）值。

Value function一般有两种。

state-value function：

$$v_{\pi}(s) = E_{\pi} [G_t \mid  S_t = s]$$

action-value function：

$$q_{\pi}(s; a) = E_{\pi} [G_t \mid  S_t = s; A_t = a]$$

后者由于和state、action都有关系，也被称作state-action pair value function。

**Transition model**是说环境本身的结构与特性：当在state执行action的时候，系统会进入的下一个state，也包括可能收到的reward。

很显然，以上三者互相关联：

如果能得到一个好的Policy function的话，那算法的目的已经达到了。

如果能得到一个好的Value function的话，那么就可以在这个state下，选取value值高的那个action，自然也是一个较好的策略。

如果能得到一个好的transition model的话，一方面，有可能可以通过这个transition model直接推演出最佳的策略；另一方面，也可以用来指导policy function或者value function 的学习过程。

因此，增强学习的方法，大体可以分为三类：

**Value-based RL，值方法。**显式地构造一个model来表示值函数Q，找到最优策略对应的Q函数，自然就找到了最优策略。

**Policy-based RL，策略方法。**显式地构造一个model来表示策略函数,然后去寻找能最大化discounted future reward。

**Model-based RL，基于环境模型的方法。**先得到关于environment transition的model，然后再根据这个model去寻求最佳的策略。

以上三种方法并不是一个严格的划分，很多RL算法同时具有一种以上的特性。

![](/images/article/RL_3.png)

## 参考

https://mp.weixin.qq.com/s/f6sq8cSaU1cuzt7jhsK8Ig

强化学习（Reinforcement Learning）基础介绍

https://mp.weixin.qq.com/s/TGN6Zhrea2LPxdkspVTlAw

穆黎森：算法工程师入门——增强学习

https://mp.weixin.qq.com/s/laKJ_jfNR5L1uMML9wkS1A

强化学习（Reinforcement Learning）算法基础及分类

https://mp.weixin.qq.com/s/Cvk_cePK9iQd8JIKKDDrmQ

强化学习的核心基础概念及实现

http://mp.weixin.qq.com/s/gHM7qh7UTKzatdg34cgfDQ

强化学习全解

https://mp.weixin.qq.com/s/B6ZpJ0Yw9GBZ9_MyNwjlXQ

构建强化学习系统，你需要先了解这些背景知识

https://mp.weixin.qq.com/s/AKuuIJnESMmck8k210CnWg

易忽略的强化学习知识之基础知识及MDP（上）

https://mp.weixin.qq.com/s/phuCKNj_a4CPq6w51Md-9A

易忽略的强化学习知识之基础知识及MDP（下）

https://mp.weixin.qq.com/s/QHAnpGsr1sSaUgOXTJjVjQ

李飞飞高徒带你一文读懂RL来龙去脉

https://mp.weixin.qq.com/s/iN8q24ka762LqY74zoVFsg

3万字剖析强化学习在电商环境下应用

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

## Gradient-Bandit算法

Gradient-Bandit算法的定义如下：

$$Pr\{A_t=a\}=\frac{e^{H_t(a)}}{\sum_{b=1}^ke^{H_t(b)}}=\pi_t(a)$$

$$H_{t+1}(a)=H_t(a)+\alpha(R_t-\overline R_t)(1\{A_t=a\}-\pi_t(a))$$

$$\overline R_t=\frac{1}{t}\sum_{i=1}^tR_i$$

其中，$$H_t(a)$$被称作策略偏好（preference）。这实际上是一个Softmax算法的变种。

## 多步强化学习

对于多步强化学习任务，虽然可以将其中的每一步看作一个k-armed Bandit问题，然而由于这种方法忽视了决策过程之间的联系，存在很多局限，因此不如MDP相关的算法。

## 参考

http://www.xfyun.cn/share/?p=2606

Bandit算法与推荐系统

