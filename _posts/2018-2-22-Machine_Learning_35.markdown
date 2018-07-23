---
layout: post
title:  机器学习（三十五）——Probabilistic Robotics, 推荐算法中的常用排序算法, NLP机器翻译常用评价度量, 价值函数的近似表示
category: ML 
---

# Probabilistic Robotics

这篇心得主要根据Sebastian Thrun的Probabilistic Robotics课程的ppt来写。

>注：Sebastian Thrun，德国波恩大学博士（1995年）。先后执教于CMU和Stanford。

网址：

http://robots.stanford.edu/probabilistic-robotics/ppt/

## 贝叶斯过滤器

假定我们需要根据测量值z来判断门的开关。显然，这里的$$P(open\mid z)$$是诊断式（**diagnostic**）问题，而$$P(z\mid open)$$是因果式（**causal**）问题。通常来说，后者比较容易获取，而前者可以基于后者使用贝叶斯公式计算得到。

一般将$$P(z\mid x)$$称为**Sensor model**。

针对多相关测量值问题，这里有一个和朴素贝叶斯假设相仿的**Markov assumption**——假设$$z_n$$独立于$$z_1,\dots,z_{n-1}$$（即“现在”不依赖于“过去”），则：

$$P(x\mid z_1,\dots,z_n)=\frac{P(z_n\mid x)P(x\mid z_1,\dots,z_{n-1})}{P(z_n\mid z_1,\dots,z_{n-1})}(\text{Bayes})
\\=\eta P(z_n\mid x)P(x\mid z_1,\dots,z_{n-1})=\eta_{1,\dots,n}\prod_{i=1}^nP(z_i\mid x)P(x)(\text{Markov})$$

>注：以下的推导过程注释中，如无特别说明。均以Bayes指代Bayes' theorem，以Markov指代Markov assumption。

上式中的$$\eta$$表示概率的归一化系数。

除了测量值z之外，一般的控制系统中还有动作（Action）的概念。比如打开门就是一个Action。Action会导致系统的状态发生改变（也可不变）。如下图所示：

![](/images/article/state_trans.png)

通常，将$$P(x\mid u,x')$$称作**Action Model**。其中，u表示Action，而x'表示系统的上一个状态。

一般的，**新的测量值会减少系统的不确定度，而新的Action会增加系统的不确定度。**

综上，一个贝叶斯过滤器（Bayes Filters）的框架包括：

输入：

1.观测值z和Action u的序列：$$d_t=\{u_1,z_1,\dots,u_t,z_t\}$$

2.Sensor model：$$P(z\mid x)$$

3.Action model：$$P(x\mid u,x')$$

4.系统状态的先验概率：$$P(x)$$

输出：

1.估计动态系统的状态X。

2.状态的后验概率，也叫**Belief**：

$$\begin{align}
\mathbf{Bel(x_t)}&=P(x_t\mid u_1,z_1,\dots,u_t,z_t)
\\&=\eta P(z_t\mid x_t,u_1,z_1,\dots,u_t)P(x_t\mid u_1,z_1,\dots,u_t)(\text{Bayes})
\\&=\eta P(z_t\mid x_t)P(x_t\mid u_1,z_1,\dots,u_t)(\text{Markov})
\\&=\eta P(z_t\mid x_t)\int P(x_t\mid u_1,z_1,\dots,u_t,x_{t-1})P(x_{t-1}\mid u_1,z_1,\dots,u_t)\mathrm{d}x_{t-1}(\text{Total prob.})
\\&=\eta P(z_t\mid x_t)\int P(x_t\mid u_t,x_{t-1})P(x_{t-1}\mid u_1,z_1,\dots,u_t)\mathrm{d}x_{t-1}(\text{Markov})
\\&=\eta P(z_t\mid x_t)\int P(x_t\mid u_t,x_{t-1})P(x_{t-1}\mid u_1,z_1,\dots,z_{t-1})\mathrm{d}x_{t-1}(\text{Markov})
\\&=\eta P(z_t\mid x_t)\int P(x_t\mid u_t,x_{t-1})\mathbf{Bel(x_{t-1})}\mathrm{d}x_{t-1}
\end{align}$$

上式也可以写作：

**预测**：

$$\overline{\mathbf{Bel(x_t)}}=\int P(x_t\mid u_t,x_{t-1})\mathbf{Bel(x_{t-1})}\mathrm{d}x_{t-1}$$

**修正**：

$$\mathbf{Bel(x_t)}=\eta P(z_t\mid x_t)\overline{\mathbf{Bel(x_t)}}$$

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

https://blog.csdn.net/tiandijun/article/details/72469471

通俗理解卡尔曼滤波及其算法实现

# 推荐算法中的常用排序算法

## Pointwise方法

Pranking (NIPS 2002), OAP-BPM (EMCL 2003), Ranking with Large Margin Principles (NIPS 2002), Constraint Ordinal Regression (ICML 2005)。

## Pairwise方法

Learning to Retrieve Information (SCC 1995), Learning to Order Things (NIPS 1998), Ranking SVM (ICANN 1999), RankBoost (JMLR 2003), LDM (SIGIR 2005), RankNet (ICML 2005), Frank (SIGIR 2007), MHR(SIGIR 2007), Round Robin Ranking (ECML 2003), GBRank (SIGIR 2007), QBRank (NIPS 2007), MPRank (ICML 2007), IRSVM (SIGIR 2006)。

## Listwise方法

LambdaRank (NIPS 2006), AdaRank (SIGIR 2007), SVM-MAP (SIGIR 2007), SoftRank (LR4IR 2007), GPRank (LR4IR 2007), CCA (SIGIR 2007), RankCosine (IP&M 2007), ListNet (ICML 2007), ListMLE (ICML 2008) 。

## LambdaMART

https://www.zhihu.com/question/41418093

求解LambdaMART的疑惑？

https://liam0205.me/2016/07/10/a-not-so-simple-introduction-to-lambdamart/

LambdaMART不太简短之介绍

http://blog.csdn.net/huagong_adu/article/details/40710305

Learning To Rank之LambdaMART的前世今生

## 参考

https://mp.weixin.qq.com/s/YjYVE6jzySVsZmXSPivB5w

达观数据搜索引擎排序实践（上篇）

https://mp.weixin.qq.com/s/UpN7tAMjbFLSPcDYsWaykg

达观数据搜索引擎排序实践（下篇）

https://mp.weixin.qq.com/s/xigME-griWFwEvvPNqWuvg

美团点评联盟广告的场景化定向排序机制

https://blog.csdn.net/stdcoutzyx/article/details/50879219

Learning to Rank简介

http://www.cnblogs.com/wentingtu/archive/2012/03/13/2393993.html

Learning to Rank入门小结

# NLP机器翻译常用评价度量

机器翻译的评价指标主要有：BLEU、NIST、Rouge、METEOR等。

参考：

http://blog.csdn.net/joshuaxx316/article/details/58696552

BLEU，ROUGE，METEOR，ROUGE-浅述自然语言处理机器翻译常用评价度量

http://blog.csdn.net/guolindonggld/article/details/56966200

机器翻译评价指标之BLEU

http://blog.csdn.net/han_xiaoyang/article/details/10118517

机器翻译评估标准介绍和计算方法

http://blog.csdn.net/lcj369387335/article/details/69845385

自动文档摘要评价方法---Edmundson和ROUGE

https://mp.weixin.qq.com/s/XiZ6Uc5cHZjczn-qoupQnA

对话系统评价方法综述

https://zhuanlan.zhihu.com/p/33088748

数据集和评价指标介绍

# 价值函数的近似表示

之前的内容都是讲解一些强化学习的基础理论，这些知识只能解决一些中小规模的问题，很多价值函数需要用一张大表来存储，获取某一状态或行为价值的时候通常需要一个查表操作（Table Lookup），这对于那些状态空间或行为空间很大的问题几乎无法求解。

在实际应用中，对于状态和行为空间都比较大的情况下，精确获得各种v(S)和q(s,a)几乎是不可能的。这时候需要找到近似的函数，具体可以使用线性组合、神经网络以及其他方法来近似价值函数：

$$\hat v(s,w)\approx v_\pi(s)\\
\hat q(s,a,w)\approx q_\pi(s,a)
$$

其中w是该近似函数的参数。

## 线性函数

这里仍然从最简单的线性函数说起：

$$\hat v(S,w)=x(S)^Tw=\sum_{j=1}^nx_j(S)w_j$$

目标函数为：

$$J(w)=E_\pi[(v_\pi(S)-x(S)^Tw)^2]$$

更新公式为：

$$\Delta w=\alpha (v_\pi(S)-\hat v(S,w))x(S)$$

上述公式都是基本的ML方法，这里不再赘述。既然是传统ML方法，自然少不了特征工程。

比如Table Lookup Features：

$$x^{table}(S)=\begin{pmatrix} 1(S=s_1) \\ \vdots \\ 1(S=s_n) \end{pmatrix}$$

则：

$$\hat v(S,w)=\begin{pmatrix} 1(S=s_1) \\ \vdots \\ 1(S=s_n) \end{pmatrix}\begin{pmatrix} w_1 \\ \vdots \\ w_n \end{pmatrix}$$

## Incremental Prediction Algorithms

事实上，之前所列的公式都不能直接用于强化学习，因为公式里都有一个实际价值函数$$v_\pi(S)$$，或者是一个具体的数值，而强化学习没有监督数据，因此不能直接使用上述公式。

因此，我们需要找到一个替代$$v_\pi(S)$$的目标。

|:--:|:--:|:--:|:--:|
| MC | $$\Delta w=\alpha (\color{red}{G_t}-\hat v(S_t,w))\nabla_w \hat v(S_t,w)$$ | 有噪声、无偏采样 | 收敛至一个局部最优解 |
| TD(0) | $$\Delta w=\alpha (\color{red}{R_{t+1}+\gamma\hat v(S_{t+1},w)}-\hat v(S_t,w))\nabla_w \hat v(S_t,w)$$ | 有噪声、有偏采样 | 收敛至全局最优解 |
| TD($$\lambda$$) | $$\Delta w=\alpha (\color{red}{G_t^\lambda}-\hat v(S_t,w))\nabla_w \hat v(S_t,w)$$ | 有噪声、有偏采样 |  |

上面公式中，红色的部分就是目标函数。

对于$$\hat q(S,A,w)$$，我们也有类似的结论：

|:--:|:--:|:--:|:--:|
| MC | $$\Delta w=\alpha (\color{red}{G_t}-\hat q(S_t,A_t,w))\nabla_w \hat q(S_t,A_t,w)$$ | 有噪声、无偏采样 | 收敛至一个局部最优解 |
| TD(0) | $$\Delta w=\alpha (\color{red}{R_{t+1}+\gamma\hat q(S_{t+1},A_{t+1},w)}-\hat q(S_t,A_t,w))\nabla_w \hat q(S_t,A_t,w)$$ | 有噪声、有偏采样 | 收敛至全局最优解 |
| TD($$\lambda$$) | $$\Delta w=\alpha (\color{red}{q_t^\lambda}-\hat q(S_t,A_t,w))\nabla_w \hat q(S_t,A_t,w)$$ | 有噪声、有偏采样 |  |

## Batch Methods

前面所说的递增算法都是基于数据流的，经历一步，更新算法后，我们就不再使用这步的数据了，这种算法简单，但有时候不够高效。与之相反，批方法则是把一段时期内的数据集中起来，通过学习来使得参数能较好地符合这段时期内所有的数据。这里的训练数据集“块”相当于个体的一段经验。

