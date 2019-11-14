---
layout: post
title:  机器学习（三十五）——Probabilistic Robotics, 推荐算法中的常用排序算法, 运筹学
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

Recursive Least Squares

http://www.doc88.com/p-6836102414531.html

RLS自适应算法基本原理

https://blog.csdn.net/HJ199404182515/article/details/52504150

浅谈自适应滤波器

## 卡尔曼滤波

>注：Rudolf (Rudi) Emil Kálmán，1930～2016，匈牙利出生的美国科学家。哥伦比亚大学博士（1957），先后执教于斯坦福大学和佛罗里达大学。现代控制理论的里程碑人物，美国科学院院士。   
>卡尔曼滤波从纯数学的角度讲，并没有多大意义。因此，主流数学家们在很长一段时间内，并不承认Kálmán是数学家。只是由于卡尔曼滤波在工程界的巨大影响力，才不得不于2012年，授予其美国数学协会院士。

| 名称 | 使用场景 |
|:--:|:--:|
| Kalman filters | Linear Gaussian |
| Extended Kalman filter | Nonlinear Gaussian |
| Iterated EKF | Nonlinear Gaussian |
| Unscented Kalman filters | Nonlinear Gaussian |
| Particle filter | Nonlinear Non-Gaussian |

unscented transformation

![](/images/img2/UKF.png)

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

https://zhuanlan.zhihu.com/p/40413223

卡尔曼滤波器实现详解

https://mp.weixin.qq.com/s/kBGhHCxq6idOOSGoLX5Kaw

手把手教你写卡尔曼滤波器

https://mp.weixin.qq.com/s/x0twRIdONCp3-qjhFJuCEQ

手把手教你写扩展卡尔曼滤波器

https://mp.weixin.qq.com/s/Nta9ksUkAVoX8arBGm7qqg

手把手教你实现多传感器融合技术

https://zhuanlan.zhihu.com/p/41767489

概率机器人——扩展卡尔曼滤波、无迹卡尔曼滤波

https://mp.weixin.qq.com/s/J27fVvYMRdoAgQtfXsywxg

OpenCV卡尔曼滤波介绍与代码演示

https://zhuanlan.zhihu.com/p/64007212

卡尔曼滤波家族

https://zhuanlan.zhihu.com/p/66646519

IKF(IEKF)推导

https://blog.csdn.net/baidu_21807307/article/details/51843079

浅谈卡尔曼滤波（Kalman Filter）

https://mp.weixin.qq.com/s/ZlyF1GcmpwZoT-3o4T567A

卡尔曼滤波算法及其应用

https://zhuanlan.zhihu.com/p/77327349

如何理解那个能造导弹军舰还把嫦娥送上天的卡尔曼滤波算法Kalman filter?

https://mp.weixin.qq.com/s/vAm0QDp_i2ec5pZbTmdHNA

傻瓜也能懂的卡尔曼滤波器

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

https://mp.weixin.qq.com/s/dRaiYPIdh_oJcQD-UxAlkA

优秀的排序算法如何成就了伟大的机器学习技术

https://mp.weixin.qq.com/s/XT4_E2d2gr1T8jCo82ix4A

深入浅出排序学习：写给程序员的算法系统开发实践

https://zhuanlan.zhihu.com/p/64952093

排序评价指标

https://zhuanlan.zhihu.com/p/64970393

PointwiseRank

https://zhuanlan.zhihu.com/p/65224450

PairwiseRank

https://zhuanlan.zhihu.com/p/66514492

ListwiseRank

https://zhuanlan.zhihu.com/p/69246361

基于query的排序

# 运筹学

http://www.cnblogs.com/6DAN_HUST/archive/2010/11/11/1874681.html

运筹学——线性规划及单纯形法求解

https://mp.weixin.qq.com/s/2qOjE0B6x9xuyoBw94NqQA

线性规划基础

https://mp.weixin.qq.com/s/zV6zi79c1Q2dfCaywuK6Pw

内点法六十年再回首

https://mp.weixin.qq.com/s/aryMyP0r6vov0pUvkoFYng

过去，现在和未来：运筹学为航空业保驾护航

https://mp.weixin.qq.com/s?__biz=MzUxMTYwMzI0OQ==&mid=2247486937&idx=1&sn=6be69679390f59516ee5f077adb8ccfa

运筹优化的剖析与应用

https://mp.weixin.qq.com/s/Ofn-p8NnnO4kLYqTlL0bJg

二次规划（QP）样条路径

https://mp.weixin.qq.com/s/zqesjLOeWIkiq68tLVVmJQ

混合整数规划模型在页岩气开采中的应用：EQT公司的案例

https://mp.weixin.qq.com/s/Ve_Gvp1Y0nIX4DV9_w0S3g

半正定规划(SDP)的形象理解和基本原理

https://mp.weixin.qq.com/s/wspfngdFNq-GeCTUJAse0A

在单纯形法之前

https://mp.weixin.qq.com/s/sGRNiAl7tvPc1tl0Xk2Idw

深度学习和强化学习在组合优化方面有哪些应用？

https://mp.weixin.qq.com/s/MuODCRWqolnh62D4t-hRvQ

临(邻)近增量累积梯度法(PIAG)

https://mp.weixin.qq.com/s/AHM60k3_Th3HD-VPBaXhuw

线性规划和整数规划的若干建模技巧

https://mp.weixin.qq.com/s/Pvu7JAHvsGJdL0mJIhVwng

多工序、多机台(产线)环境下的排程要点

https://mp.weixin.qq.com/s/oMiNGJOCqjEHoTmfBQv6EA

使用神经网络为A*搜索算法赋能：以个性化路径推荐为例

https://mp.weixin.qq.com/s/D_CQIw1372RTYBeSqPEl9A

鲁棒优化基础

https://mp.weixin.qq.com/s/7x2LS94i7te7JAeWo-d5kA

什么样的整数规划模型是一个好的模型

https://mp.weixin.qq.com/s/hzpnHvqaP9sxQQlLq-_3wg

Robust Optimization in 3D Vision

https://mp.weixin.qq.com/s/T20YMbTBbn1GC3fvInhkkA

深度学习如何影响运筹学？

https://mp.weixin.qq.com/s/VcA1SZNS4LvMny_9dCUByQ

混合整数规划/离散优化的精确算法--分支定界法及优化求解器

https://mp.weixin.qq.com/s/Ol69W-T67eGJW6RDjQJQ6w

胡武华博士：运筹优化理论在物流行业中的应用实践

https://mp.weixin.qq.com/s/cd08Lk4LcxV-qAVLq7-Kjg

整数规划经典方法--割平面法（Cutting Plane Method）
