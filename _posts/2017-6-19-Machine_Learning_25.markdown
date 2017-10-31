---
layout: post
title:  机器学习（二十五）——强化学习
category: ML 
---

# 强化学习

## 折扣未来奖励（Discounted Future Reward）（续）

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

**Value**，价值函数，表示在输入state，action的时候，能够返回在state下，执行这个action能得到的Discounted future reward的（期望）值。

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

## Partial observability

部分可见的情况下，agent state $$\neq$$ environment state，这时一般叫做partially observable Markov decision process(POMDP)。

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

# Q-learning

Q-learning是强化学习中很重要的算法，也是最早被引入DL领域的强化学习算法，对它的研究催生了Deep Q-learning Networks。

下面用一个例子来讲述Q-learning算法。

![](/images/article/q_learning.gif)

上图中有5个房间，编号为0～4，将户外定义为编号5，房间之间通过门相连，则房间的联通关系可抽象为下图：

![](/images/article/q_learning_1.gif)

这里我们将每个房间称为一个**state**，将agent从一个房间到另一个房间称为一个**action**。

开始时，我们将agent放置在任意房间中，并设定目标——走到户外（即房间5），则上图可变为：

![](/images/article/q_learning_2.gif)

这里的每条边上的数值就是reward值。Q-Learning的目标就是达到reward值最大的state。因此当agent到达户外之后，它就停留在那里了，这样的目标被称作**吸收目标**。

如果以state为行，action为列，则上图又可转化为如下的reward矩阵：

![](/images/article/r_matrix.gif)

其中，-1表示两个state之间没有action。

类似的，我们可以构建一个和R同阶的矩阵Q，来表示Q-Learning算法学到的知识。

开始时，agent对外界一无所知，所以Q可以初始化为零矩阵。

Q-Learning算法的**transition rule**为：

$$Q(s,a)=R(s,a)+\gamma \max(Q(\tilde s,\tilde a))tag{1}$$

其中，(s,a)表示当前的state和action，$$(\tilde s,\tilde a)$$表示下一个state和action，$$0 \le \gamma < 1$$为学习参数。

在无监督的情况下，agent不断从一个状态转至另一状态进行探索，直到到达目标。我们将agent的每一次探索（从任意初始状态到目标状态的过程）称为一个**episode**。

Q-Learning算法的计算步骤如下：

>**Step 1**：给定参数$$\gamma$$和reward矩阵R。   
>**Step 2**：初始化Q=0。   
>**Step 3**：For each episode:
>>3.1 随机选择一个初始状态s。   
>>3.2 若未达到目标状态，则：
>>>(1)在当前s的所有action中，选择一个行为a。   
>>>(2)利用a，得到下一个状态$$\tilde s$$。   
>>>(3)利用公式1，计算$$Q(s,a)$$。   
>>>(4)令$$s=\tilde s$$。

由马尔可夫过程的性质（参见《机器学习（十六、二十）》）可知，Q矩阵最终会趋于一个极限，如下所示：

![](/images/article/q_matrix5.gif)



![](/images/article/map5.gif)


参考：

http://blog.csdn.net/itplus/article/details/9361915

一个Q-learning算法的简明教程

http://blog.csdn.net/young_gy/article/details/73485518

强化学习之Q-learning简介


