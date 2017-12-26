---
layout: post
title:  机器学习（二十五）——数据不平衡问题, 强化学习, K-摇臂赌博机
category: ML 
---

# 数据不平衡问题

https://mp.weixin.qq.com/s/e0jXXCIhbaZz7xaCZl-YmA

如何处理不均衡数据？

https://mp.weixin.qq.com/s/2j_6hdq-MhybO_B0S7DRCA

如何解决机器学习中数据不平衡问题

https://mp.weixin.qq.com/s/gEq7opXLukWD5MVhw_buGA

七招教你处理非平衡数据

http://blog.csdn.net/u013709270/article/details/72967462

机器学习中的数据不平衡解决方案大全

https://mlr-org.github.io/mlr-tutorial/devel/html/over_and_undersampling/index.html

Imbalanced Classification Problems

https://mp.weixin.qq.com/s/QEHAV_rW25E0b0N7POr6tw

关于处理样本不平衡问题的Trick整理

# 强化学习

## 监督学习的局限

举个例子，如果我们想让机器学会开车，一个很直接的想法是观察人类行为，并且模仿人类，在相应观测下做出人类所做行为。将这个想法实现起来也很简单，只需要收集该任务的一些观测（路面的画面），以及每个观测人类会做出的反应（转动方向盘），然后像监督学习一样训练一个神经网络，以观测为输入，人类行为为标签，其中行为是离散时是分类任务，连续时是回归任务。

![](/images/img2/RL.png)

## 概述

强化学习是一个多学科交叉的领域。它的主要组成以及和其他学科的关系如下图所示：

![](/images/article/RL_2.png)

![](/images/article/RL.png)

上图是Reinforcement Learning和其他类型算法的关系图。

![](/images/img2/RL_2.png)

不像监督学习，对于每一个样本，都有一个确定的标签与之对应，而强化学习没有标签，只有一个时间延迟的奖励，而且游戏中我们往往牺牲当前的奖励来获取将来更大的奖励。这就是**信用分配问题（Credit Assignment Problem）**，即当前的动作要为将来获得更多的奖励负责。

而且在我们找到一个策略，让游戏获得不错的奖励时，我们是选择继续坚持当前的策略，还是探索新的策略以求更多的奖励？这就是**探索与开发（Explore-exploit Dilemma）**的问题。

因此，**强化学习某种意义上可看做具有延迟标记信息的监督学习**。

![](/images/article/reinforcement_learning.png)

上图是强化学习的基本流程图。从控制论的角度来说，这是一个反馈控制系统，和经典的Kalman filters系统非常类似。因此，目前强化学习的主要用途，也多数和系统控制相关，例如机器人和自动驾驶。

在推荐系统领域，由于有用户的反馈信息，亦可使用相关强化学习算法。

## MDP

强化学习任务通常用**马尔可夫决策过程（Markov Decision Process）**来描述：

$$<\mathcal{S},\mathcal{A},\mathcal{P},\mathcal{R},\gamma>$$

这个五元组依次代表：states、actions、state transition probability matrix、reward function、discount factor。

>MDP中有两个对象：Agent和Environment。   
>1.Environment处于一个特定的状态（State）（如打砖块游戏中挡板的位置、各个砖块的状态等）。   
>2.Agent可以通过执行特定的动作（Actions）（如向左向右移动挡板）来改变Environment的状态。   
>3.Environment状态改变之后会返回一个观察（Observation）给Agent，同时还会得到一个奖励（Reward）（可以为负，就是惩罚）。   
>4.Agent根据返回的信息采取新的动作，如此反复下去。Agent如何选择动作叫做策略（Policy）。MDP的任务就是找到一个策略，来最大化奖励。

注意State和Observation区别：State是Environment的私有表达，我们往往不会直接得到。

在MDP中，当前状态State包含了所有历史信息，即将来只和现在有关，与过去无关，因为现在状态包含了所有历史信息。只有满足这样条件的状态才叫做马尔科夫状态（Markov state）。当然这只是理想状况，现实往往不会那么简单。

正是因为State太过于复杂，我们往往可以需要一个对Environment的观察来间接获得信息，因此就有了Observation。不过Observation是可以等于State的，此时叫做Full Observability。

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

$$\pi(a | s) = P[A_t = a | S_t = s]$$

**Value**，价值函数，表示在输入state，action的时候，能够返回在state下，执行这个action能得到的Discounted future reward的（期望）值。

state-value function：

$$v_{\pi}(s) = E_{\pi} [G_t | S_t = s]$$

action-value function：

$$q_{\pi}(s; a) = E_{\pi} [G_t | S_t = s; A_t = a]$$

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

https://zhuanlan.zhihu.com/p/25688750

模仿学习（Imitation Learning）完全介绍

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

