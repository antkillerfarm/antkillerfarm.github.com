---
layout: post
title:  机器学习（二十六）——Probabilistic Robotics
category: ML 
---

# 动态规划（续）

## RL与Dynamic programming

RL领域的Dynamic programming主要包括以下算法：（为了抓住问题的本质，这里仅列出各算法最关键的Bellman equation，至于流程参照Q-learning算法即可。）

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

# Probabilistic Robotics

这篇心得主要根据Sebastian Thrun的Probabilistic Robotics课程的ppt来写。

>注：Sebastian Thrun，德国波恩大学博士（1995年）。先后执教于CMU和Stanford。

网址：

http://robots.stanford.edu/probabilistic-robotics/ppt/

## 贝叶斯过滤器

假定我们需要根据测量值z来判断门的开关。显然，这里的$$P(open\vert z)$$是诊断式（**diagnostic**）问题，而$$P(z\vert open)$$是因果式（**causal**）问题。通常来说，后者比较容易获取，而前者可以基于后者使用贝叶斯公式计算得到。

一般将$$P(z\vert x)$$称为**Sensor model**。

针对多相关测量值问题，这里有一个和朴素贝叶斯假设相仿的**Markov assumption**——假设$$z_n$$独立于$$z_1,\dots,z_{n-1}$$（即“现在”不依赖于“过去”），则：

$$P(x|z_1,\dots,z_n)=\frac{P(z_n|x)P(x|z_1,\dots,z_{n-1})}{P(z_n|z_1,\dots,z_{n-1})}(\text{Bayes})
\\=\eta P(z_n|x)P(x|z_1,\dots,z_{n-1})=\eta_{1,\dots,n}\prod_{i=1}^nP(z_i|x)P(x)(\text{Markov})$$

>注：以下的推导过程注释中，如无特别说明。均以Bayes指代Bayes' theorem，以Markov指代Markov assumption。

上式中的$$\eta$$表示概率的归一化系数。

除了测量值z之外，一般的控制系统中还有动作（Action）的概念。比如打开门就是一个Action。Action会导致系统的状态发生改变（也可不变）。如下图所示：

![](/images/article/state_trans.png)

通常，将$$P(x\vert u,x')$$称作**Action Model**。其中，u表示Action，而x'表示系统的上一个状态。

一般的，**新的测量值会减少系统的不确定度，而新的Action会增加系统的不确定度。**

综上，一个贝叶斯过滤器（Bayes Filters）的框架包括：

输入：

1.观测值z和Action u的序列：$$d_t=\{u_1,z_1,\dots,u_t,z_t\}$$

2.Sensor model：$$P(z\vert x)$$

3.Action model：$$P(x\vert u,x')$$

4.系统状态的先验概率：$$P(x)$$

输出：

1.估计动态系统的状态X。

2.状态的后验概率，也叫**Belief**：

$$\begin{align}
\mathbf{Bel(x_t)}&=P(x_t\vert u_1,z_1,\dots,u_t,z_t)
\\&=\eta P(z_t\vert x_t,u_1,z_1,\dots,u_t)P(x_t\vert u_1,z_1,\dots,u_t)(\text{Bayes})
\\&=\eta P(z_t\vert x_t)P(x_t\vert u_1,z_1,\dots,u_t)(\text{Markov})
\\&=\eta P(z_t\vert x_t)\int P(x_t\vert u_1,z_1,\dots,u_t,x_{t-1})P(x_{t-1}\vert u_1,z_1,\dots,u_t)\mathrm{d}x_{t-1}(\text{Total prob.})
\\&=\eta P(z_t\vert x_t)\int P(x_t\vert u_t,x_{t-1})P(x_{t-1}\vert u_1,z_1,\dots,u_t)\mathrm{d}x_{t-1}(\text{Markov})
\\&=\eta P(z_t\vert x_t)\int P(x_t\vert u_t,x_{t-1})P(x_{t-1}\vert u_1,z_1,\dots,z_{t-1})\mathrm{d}x_{t-1}(\text{Markov})
\\&=\eta P(z_t\vert x_t)\int P(x_t\vert u_t,x_{t-1})\mathbf{Bel(x_{t-1})}\mathrm{d}x_{t-1}
\end{align}$$

上式也可以写作：

**预测**：

$$\overline{\mathbf{Bel(x_t)}}=\int P(x_t\vert u_t,x_{t-1})\mathbf{Bel(x_{t-1})}\mathrm{d}x_{t-1}$$

**修正**：

$$\mathbf{Bel(x_t)}=\eta P(z_t\vert x_t)\overline{\mathbf{Bel(x_t)}}$$

熟悉卡尔曼滤波的同学大概已经看出来了。没错！贝叶斯过滤器是一大类算法的统称。这些算法包括Kalman filters、Particle filters、Hidden Markov models、Dynamic Bayesian networks、Partially Observable Markov Decision Processes (POMDPs)等。

## 递归最小二乘法

http://www.blog.huajh7.com/adaptive-filters-lms-rls-kalman-filter-1/

## 卡尔曼滤波

>注：Rudolf (Rudi) Emil Kálmán，1930～2016，匈牙利出生的美国科学家。哥伦比亚大学博士（1957），先后执教于斯坦福大学和佛罗里达大学。现代控制理论的里程碑人物，美国科学院院士。   
>卡尔曼滤波从纯数学的角度讲，并没有多大意义。因此，主流数学家们在很长一段时间内，并不承认Kálmán是数学家。只是由于卡尔曼滤波在工程界的巨大影响力，才不得不于2012年，授予其美国数学协会院士。

Kalman filters是一种高斯线性滤波器。

参考：

http://www.cs.unc.edu/~welch/media/pdf/kalman_intro.pdf

Gregory Francis Welch写的卡尔曼滤波科普文。

>注：Gregory Francis Welch，北卡罗莱娜大学博士（1997）。中佛罗里达大学教授。

http://www.cs.unc.edu/~welch/kalman/media/misc/kalman_intro_chinese.zip

上文的中文版。

https://zhuanlan.zhihu.com/p/21294526

知乎诸位大神的科普文。

http://www.docin.com/p-976961701.html

动态相对定位中自适应滤波方法的研究

《自适应动态导航定位》，杨元喜著。

>注：杨元喜，1956年生，大地测量学家。中国科学院院士。

