---
layout: post
title:  机器学习（二十三）——数据不平衡问题, Probabilistic Robotics, 概率图模型
category: theory 
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

# 概率图模型

## 资料

probabilistic graphical model（PGM）最早由Judea Pearl发明。

这方面比较重要的文章和书籍有：

http://www.cis.upenn.edu/~mkearns/papers/barbados/jordan-tut.pdf

Michael Irwin Jordan著。

《Probabilistic Graphical Models: Principles and Techniques》，Daphne Koller，Nir Friedman著（2009年）。

>注：Judea Pearl，1936年生，以色列-美国计算机科学家，UCLA教授。2011年获得图灵奖。

>Michael Irwin Jordan，1956年生，美国计算机科学家。UCSD博士，先后执教于MIT和UCB。吴恩达的导师。

>Daphne Koller，女，1968年生，以色列-美国计算机科学家。斯坦福大学博士及教授。和吴恩达共同创立在线教育平台Coursera。

>Nir Friedman，1967年生，以色列计算机科学家。斯坦福大学博士，耶路撒冷希伯来大学教授。

http://www.cs.cmu.edu/~epxing/Class/10708-14/lectures/

CMU的邢波（Eric Xing）所开的概率图模型课程。

## 概述

概率图模型的三要素：Graph：$$\mathcal{G}$$、Model：$$\mathcal{M}$$和Data：$$\mathcal{D}\equiv\{X^{(i)}_1,\dots,X^{(i)}_m\}^N_{i=1}$$。

它要解决的三大问题：

1.**表示**。如何获取或定义真实世界的不确定度？如何对领域知识/假设/约束编码？

2.**推断**。根据模型/数据，推断答案。

$$\text{e.g.}:P(x_i|\mathcal{D})$$

3.**学习**。根据数据确定哪个模型是正确的。

$$\text{e.g.}:\mathcal{M}=\arg\max_{\mathcal{M}\in M}F(\mathcal{D};\mathcal{M})$$

![](/images/article/PGM.png)

上图是PGM的一个示例。其中$$X_i$$表示随机变量，图中共有8个随机变量，假设它们均为二值变量，则整个状态空间共有$$2^8$$种组合。遍历这样大的状态空间无疑是一件极为费力的事情。

如果$$X_i$$是条件独立的话，则由上图可得：

$$P(X_1,\dots,X_8)=P(X_2)P(X_4|X_2)P(X_5|X_2)P(X_1)P(X_3|X_1)\\P(X_6|X_3,X_4)P(X_7|X_6)P(X_8|X_5,X_6)$$

这样，状态空间就缩小为$$2+4+4+2+4+8+4+8=36$$种组合了。

根据边的类型，PGM可分为两类：

1.有向边表示变量间的**因果**关系。这样的PGM，常称为Bayesia Network（BN）或Directed Graphical Model（DGM）。

2.无向边表示变量间的**相关**关系。这样的PGM，常称为Markov Random Field（MRF）或Undirected Graphical Model（UGM）。

>注：因果关系是一种强逻辑关系，需要变量间有深刻的内在联系。而相关关系要弱的多，典型的例子就是《机器学习（十七）》中的尿布和啤酒的故事。尿布和啤酒虽然正相关，然而它们本身却没有多大的联系。

根据模型的不同，PGM又可分为生成模型（Generative Model, GM）和判别模型（Discriminative Model, DM）。两者的区别在《机器学习（二）》中已经简单提到过，这里做一个扩展。

之前提到的机器学习算法，主要是建立特征向量X和标签Y之间的联系。但是实际情况下，X中的状态不一定都能得到，因此可以根据可见性，将X分为可观测变量集合O和其他变量集合R，Y也不一定是一个标签，而可能是一个变量集合。即：

$$GM:P(Y,R,O)\to P(Y|O)$$

$$DM:P(Y,R|O)\to P(Y|O)$$

注意，在贝叶斯学派的观点中，模型的参数也是随机变量，因此，R在某些情况下，不仅包含不可观测的变量，也包含模型参数。

## 贝叶斯网络

贝叶斯网络是最简单的有向图模型。

首先给出几个术语的定义：

**有向无环图(Directed Acyclic Graph, DAG)**：这个术语的字面意思很清楚，不解释。

**马尔可夫毯(Markov Blanket, MB)**：有向图——结点A的父结点+A的子结点+A的子结点的其他父结点。如下图所示：

![](/images/article/Markov_blanket.png)

无向图——结点A的邻近结点。

下图是图模型的部分变种之间的关系图。

![](/images/article/Generative_Models.png)

# DL参考资源

https://mp.weixin.qq.com/s/xnbNkKOomwvwYC_ThKPJ_g

田渊栋报告：游戏中的人工智能：成就与挑战

https://mp.weixin.qq.com/s/W4zwKqkVQN4v-IKzGrkudg

通过传递不变性实现自监督视觉表征学习

