---
layout: post
title:  机器学习（二十七）——动态规划
category: ML 
---

# 动态规划（续）

## 重复子问题

![](/images/article/Fibonacci_dynamic_programming.png)

重复子问题是指通过相同的子问题可以解决不同的较大问题。例如，在Fibonacci序列中，F3 = F1 + F2和F4 = F2 + F3都包含计算F2。由于计算F5需要计算F3和F4，一个比较笨的计算F5的方法可能会重复计算F2两次甚至两次以上。

为避免重复计算，可将已经得到的子问题的解保存起来，当我们要解决相同的子问题时，重用即可。该方法即所谓的**缓存（memoization）**。

动态规划通常采用以下两种方式中的一种：

**自顶向下**：将问题划分为若干子问题，求解这些子问题并保存结果以免重复计算。该方法将递归和缓存结合在一起。

**自下而上**：先行求解所有可能用到的子问题，然后用其构造更大问题的解。该方法在节省堆栈空间和减少函数调用数量上略有优势，但有时想找出给定问题的所有子问题并不那么直观。

需要注意的是：动态规划更多的看作是一种解决问题的方法论，而非具体的数值算法，因此，很多不同领域的算法都可看做是动态规划算法的实例。参考文献中，就列出了不少这样的算法。显然，动态规划是一种**迭代（Iteration）算法**。

由前文的描述可知，MDP正好具备overlapping subproblems和optimal substructure的特性，因此也可以通过DP求解。

在继续下文之前，推荐一波资源：

>David Poole，加拿大不列颠哥伦比亚大学教授。加拿大AI协会终身成就奖（2013年）。   
>个人主页：   
>http://www.cs.ubc.ca/~poole/index.html

David Poole的主页上有很多好东西：

http://www.cs.ubc.ca/~poole/demos/

该网页上有一些RL方面的用java applet做的可视化demo。由于年代比较久远，这些demo无法在目前的浏览器上运行。所以，我对其做了一些改造，使之能够使用。相关代码参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/RL

David Poole还写了一本书《Artificial Intelligence: foundations of computational agents》，目前已经是第2版了。

其中的代码资源参见：

http://artint.info/AIPython/

从该书所用编程语言的变迁，亦可感受到Poole教授不断学习的脚步。要知道Poole教授刚进入学术界的时代（1985年前后），就连Java也还没被发明出来呢。

http://uhaweb.hartford.edu/compsci/ccli/samplep.htm

Hartford大学的这个网站也有些不错的资料，偏重RL、机器人、ML for Game等领域。

## RL与DP

在继续讲述之前，我们首先来明确几个概念：

![](/images/img2/RL_3.png)

$$v_{\pi}$$和$$q_{\pi}$$的定义参见《机器学习（二十五）》。

$$v_*(s)=\max_{\pi}v_{\pi}(s)$$

$$q_*(s,a)=\max_{\pi}q_{\pi}(s,a)$$

RL领域的DP算法的主要思想是：利用value function构建搜索Good Policy的方法。这里用$$v_*(s)$$或$$q_*(s, a)$$表示最优的value function。

RL DP主要包括以下算法：（为了抓住问题的本质，这里仅列出各算法最关键的Bellman equation，至于流程参照Q-learning算法即可。）

### Iterative Policy Evaluation：

$$v_{k+1}(s) = \sum_{a \in \mathcal{A}}\pi(a | s)\left(\mathcal{R}_s^a + \gamma \sum_{s'\in \mathcal{S}}\mathcal{P}_{ss'}^a v_k(s')\right)$$

### Policy Iteration

Policy Iteration包含如下两步：

Policy Evaluation：

$$v_{k+1}(s) = \sum_{a \in \mathcal{A}}\pi(a | s)\left(\mathcal{R}_s^a + \gamma \sum_{s'\in \mathcal{S}}\mathcal{P}_{ss'}^a v_k(s')\right)$$

Policy Improvement：

$$\pi_{k+1}(s) = \arg \max_{a \in \mathcal{A}}\left(\mathcal{R}_s^a + \gamma \sum_{s'\in \mathcal{S}}\mathcal{P}_{ss'}^a v_k(s')\right)$$

![](/images/article/Policy_Iteration.png)

![](/images/article/Policy_Iteration_2.png)

### Value Iteration

$$v_{k+1}(s) = \max_{a \in \mathcal{A}}\left(\mathcal{R}_s^a + \gamma \sum_{s'\in \mathcal{S}}\mathcal{P}_{ss'}^a v_k(s')\right)$$



## 参考

http://www.cppblog.com/Fox/archive/2008/05/07/Dynamic_programming.html

动态规划算法（Wikipedia中文翻译版）

https://www.zhihu.com/question/23995189

什么是动态规划？动态规划的意义是什么？

http://www.cnblogs.com/steven_oyj/archive/2010/05/22/1741374.html

动态规划算法

http://www.cppblog.com/menjitianya/archive/2015/10/23/212084.html

动态规划

https://segmentfault.com/a/1190000004454832

动态规划

http://www.cnblogs.com/jmzz/archive/2011/06/26/2090702.html

动态规划

http://www.cnblogs.com/jmzz/archive/2011/07/01/2095158.html

DP之Warshall算法和Floyd算法

http://www.cnblogs.com/jmzz/archive/2011/07/02/2096050.html

DP之最优二叉查找树

http://www.cnblogs.com/jmzz/archive/2011/07/02/2096188.html

DP之矩阵连乘问题

http://www.cnblogs.com/jmzz/archive/2011/07/05/2098630.html

DP之背包问题+记忆递归

http://www.cs.upc.edu/~mmartin/Ag4-4x.pdf

Bellman equations and optimal policies

https://mp.weixin.qq.com/s/a1C1ezL59azNfdM3TFGaGw

递推与储存，是动态规划的关键

# Monte-Carlo

参考：

https://mp.weixin.qq.com/s/F9VlxVV4nXELyKxdRo9RPA

强化学习——蒙特卡洛



