---
layout: post
title:  机器学习（二十八）——Monte-Carlo, Temporal-Difference Learning
category: ML 
---

# Monte-Carlo（续）

## Monte Carlo method与RL

MDP的缺点在于model是已知的，但在实际应用中，更多的是Model未知（或部分未知）或者建模困难的情况，这种情况下就需要使用MC method来生成相应的Model。

MC method在RL中主要有两种使用方式：

model-free：完全不依赖Model。

Simulated：简单的模拟，而不需要完整的Model。

MC method用experience替代了MDP中的transitions/rewards（也可以说是用empirical mean替代了expected），但需要注意这些experience不能是重复采样的，而且它只适用于周期性的MDP。

## Monte Carlo Policy Evaluation

Monte Carlo Policy Evaluation的目标是对状态s进行估值。它的步骤是：

>当s被访问到(visited)时:   
>>增加计数：$$N(s)\leftarrow N(s) + 1$$   
>>增加总奖励：$$S(s)\leftarrow S(s) + G_t$$   
>>$$V(s) = S(s)/N(s)$$   
>反复多次：$$N(s)\to \infty,V(s)\to v_{\pi}(s)$$

根据Visit的策略不同，Monte Carlo Policy Evaluation又可分为：First-visit MC和Every-Visit MC。

两者的差别在于：First-visit MC的每次探索，一旦抵达状态s，就结束了，Every-Visit MC到达状态s之后还可以继续探索。

## Incremental Monte-Carlo Updates

借用《机器学习（二十六）》中，求均值的小技巧，我们可以得到Incremental Mean。

用Incremental Mean进行更新，被称作Incremental Monte-Carlo Updates：

$$V(S_t)\leftarrow V(S_t)+\frac{1}{N(S_t)}(G(t)-V(S_t))$$

对于非平稳（non-stationary）问题，我们也可采用如下公式更新：

$$V(S_t)\leftarrow V(S_t)+\alpha(G(t)-V(S_t))$$

## 参考

https://mp.weixin.qq.com/s/F9VlxVV4nXELyKxdRo9RPA

强化学习——蒙特卡洛

https://www.zhihu.com/question/20254139

蒙特卡罗算法是什么？

http://www.cnblogs.com/2010Freeze/archive/2011/09/19/2181016.html

概率算法-sherwood算法

http://www.cnblogs.com/chinazhangjie/archive/2010/11/11/1874924.html

概率算法

https://mp.weixin.qq.com/s/wfCyii6bS-GxMZPg2TPaLA

蒙特卡洛树搜索是什么？如何将其用于规划星际飞行？

# Temporal-Difference Learning

时序差分学习和蒙特卡洛学习一样，它也从Episode学习，不需要了解模型本身；但是它可以学习不完整的Episode，通过自身的引导（bootstrapping），猜测Episode的结果，同时持续更新这个猜测。

最简单的TD算法——TD(0)的更新公式如下：

$$V(S_t)\leftarrow V(S_t)+\alpha(R_{t+1}+\gamma V(S_{t+1})-V(S_t))$$

其中，$$R_{t+1}+\gamma V(S_{t+1})$$被称作TD target，而$$\delta_t=R_{t+1}+\gamma V(S_{t+1})-V(S_t)$$被称作TD error。

下面用驾车返家的例子直观解释蒙特卡洛策略评估和TD策略评估的差别。

想象一下你下班后开车回家，需要预估整个行程花费的时间。假如一个人在驾车回家的路上突然碰到险情：对面迎来一辆车感觉要和你相撞，严重的话他可能面临死亡威胁，但是最后双方都采取了措施没有实际发生碰撞。如果使用蒙特卡洛学习，路上发生的这一险情可能引发的负向奖励不会被考虑进去，不会影响总的预测耗时；但是在TD学习时，碰到这样的险情，这个人会立即更新这个状态的价值，随后会发现这比之前的状态要糟糕，会立即考虑决策降低速度赢得时间，也就是说你不必像蒙特卡洛学习那样直到他死亡后才更新状态价值，那种情况下也无法更新状态价值。

TD算法相当于在整个返家的过程中（一个Episode），根据已经消耗的时间和预期还需要的时间来不断更新最终回家需要消耗的时间。

| 状态 | 已消耗时间 | 预计仍需耗时 | 预测总耗时 |
|:--:|:--:|:--:|:--:|
| 离开办公室 | 0 | 30 | 30 |
| 取车，发现下雨 | 5 | 35 | 40 |
| 离开高速公路 | 20 | 15 | 35 |
| 被迫跟在卡车后面 | 30 | 10 | 40 |
| 到达家所在街区 | 40 | 3 | 43 |
| 到家 | 43 | 0 | 43 |

基于上表所示的数据，下图展示了蒙特卡洛学习和TD学习两种不同的学习策略来更新价值函数（各个状态的价值）。这里使用的是从某个状态预估的到家还需耗时来间接反映某状态的价值：某位置预估的到家时间越长，该位置价值越低，在优化决策时需要避免进入该状态。对于蒙特卡洛学习过程，驾驶员在路面上碰到各种情况时，他不会更新对于回家的预估时间，等他回到家得到了真实回家耗时后，他会重新估计在返家的路上着每一个主要节点状态到家的时间，在下一次返家的时候用新估计的时间来帮助决策。

而对于TD学习，在一开始离开办公室的时候你可能会预估总耗时30分钟，但是当你取到车发现下雨的时候，你会立刻想到原来的预计过于乐观，因为既往的经验告诉你下雨会延长你的返家总时间，此时你会更新目前的状态价值估计，从原来的30分钟提高到40分钟。同样当你驾车离开高速公路时，会一路根据当前的状态（位置、路况等）对应的预估返家剩余时间，直到返回家门得到实际的返家总耗时。这一过程中，你会根据状态的变化实时更新该状态的价值。

![](/images/img2/TD.png)

## TD vs. MC---1

通过这个例子，我们可以直观的了解到：

1.TD在知道结果之前可以学习，MC必须等到最后结果才能学习；

2.TD可以在没有结果时学习，可以在持续进行的环境里学习。

## TD vs. MC---2

$$G_t = R_{t+1} + \gamma R_{t+2} + \dots + \gamma^{T−1}R_T$$是$$V_{\pi}(S_t)$$的无偏估计值。

True TD target：$$R_{t+1}+\gamma V_{\pi}(S_{t+1})$$，也是$$V_{\pi}(S_t)$$的无偏估计值。

TD target：$$R_{t+1}+\gamma V(S_{t+1})$$，是$$V_{\pi}(S_t)$$的有偏估计值。

MC没有bias，但有着较高的Variance，且对初始值不敏感；

TD低variance, 但有一定程度的bias，对初始值较敏感，通常比MC更高效；

## TD vs. MC---3

再来看如下示例：

已现有两个状态(A和B)，MDP未知，衰减系数为1，有如下表所示8个完整Episode的经验及对应的即时奖励，其中除了第1个Episode有状态转移外，其余7个均只有一个状态。

| Episode | 状态转移及奖励 |
|:--:|:--:|
| 1 | A:0,B:0 |
| 2 | B:1 |
| 3 | B:1 |
| 4 | B:1 |
| 5 | B:1 |
| 6 | B:1 |
| 7 | B:1 |
| 8 | B:0 |


