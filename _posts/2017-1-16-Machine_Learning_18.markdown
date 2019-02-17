---
layout: post
title:  机器学习（十八）——独立成分分析, 时间序列分析, 异常检测
category: ML 
---

# 独立成分分析（续）

## 密度函数和线性变换

在讨论ICA的具体算法之前，我们先来回顾一下概率和线性代数里的知识。

假设我们的随机变量s有概率密度（probability density）函数$$p_s(s)$$。为了简单，我们再假设s是实数，还有一个随机变量$$x=As$$，A和x都是实数。令$$p_x$$是x的概率密度，那么怎么求$$p_x$$呢？

令$$W=A^{-1}$$，则$$s=Wx$$。然而$$p_x(x)\neq p_s(Wx)$$。

这里以均匀分布（Uniform）为例讨论一下。令$$s\sim \text{Uniform}[0,1]$$，则$$p_s(s)=1$$。令$$A=2$$，则$$W=0.5$$，$$x=2s\sim \text{Uniform}[0,2]$$，因此$$p_x(x)=p_s(Wx)\lvert W \rvert$$。

## 累积分布函数

累积分布函数（cumulative distribution function，CDF）是概率论中的一个基本概念。它的定义如下：

$$F(z_0)=P(z\le z_0)=\int_{-\infty}^{z_0}p_z(z)\mathrm{d}z$$

可以看出：

$$p_z(z)=F'(z)$$

## ICA算法

ICA算法归功于Bell 和 Sejnowski，这里使用最大似然估计来解释算法。（原始论文中使用的是一个复杂的方法Infomax principal，这在最新的推导中已经不需要了。）

>注：Terrence (Terry) Joseph Sejnowski，1947年生，美国科学家。普林斯顿大学博士，导师是神经网络界的大神John Hopfield。ICA算法和Boltzmann machine的发现人。

>Tony Bell的个人主页：
>http://cnl.salk.edu/~tony/index.html

我们假定每个$$s_i$$有概率密度$$p_s$$，那么给定时刻原信号的联合分布就是：

$$p(s)=\prod_{i=1}^np_s(s_i)$$

因此：

$$p(x)=\prod_{i=1}^np_s(w_i^Tx)\cdot \mid W\mid \tag{2}$$

为了确定$$s_i$$的概率密度，我们首先要确定它的累计分布函数$$F(x)$$。而这需要满足两个性质：单调递增和在$$[0,1]$$区间。

我们发现sigmoid函数很适合，它的定义域负无穷到正无穷，值域0到1，缓慢递增。因此，可以假定s的累积分布函数符合sigmoid函数：

$$g(s)=\frac{1}{1+e^{-s}}$$

求导，可得：

$$p_s(s)=g'(s)=g(s)(1-g(s))$$

这里的推导参见《机器学习（一）》的公式7。

>注：如果有其他先验信息的话，这里的$$g(s)$$也可以使用其他函数。否则的话，sigmoid函数能够在大多数问题上取得不错的效果。

公式2的对数似然估计函数为：

$$\ell(W)=\sum_{i=1}^m\left(\sum_{j=1}^m\log g'(w_j^Tx^{(i)})+\log\mid W\mid \right)\tag{3}$$

因为：

$$\begin{align}
(\log g'(s))'&=(\log [g(s)(1-g(s))])'=(\log g(s))'+(\log (1-g(s)))'
\\&=\frac{g'(s)}{g(s)}+\frac{(1-g(s))'}{(1-g(s))}=\frac{g(s)(1-g(s))}{g(s)}-\frac{g(s)(1-g(s))}{(1-g(s))}
\\&=1-2g(s)
\end{align}$$

又因为《机器学习（十）》的公式5.11，可得公式3的导数为：

$$\nabla_W\ell(W)=\begin{bmatrix}
1-2g(w_1^Tx^{(i)}) \\
1-2g(w_2^Tx^{(i)}) \\
\vdots \\
1-2g(w_n^Tx^{(i)})
\end{bmatrix}x^{(i)^T}+(W^T)^{-1}$$

最后，用通常的随机梯度上升算法，求得$$\ell(W)$$的最大值即可。

>注意：我们计算最大似然估计时,假设了$$x^{(i)}$$和$$x^{(j)}$$之间是独立的，然而对于语音信号或者其他具有时间连续依赖特性(比如温度)上，这个假设不能成立。但是在数据足够多时，假设独立对效果影响不大。

# 时间序列分析

## 书籍和教程

http://www.stat.berkeley.edu/~bartlett/courses/153-fall2010/

berkeley的时间序列分析课程

http://people.duke.edu/%7Ernau/411home.htm

回归和时间序列分析

《应用时间序列分析》，王燕著。

## 概述

时间序列，就是按时间顺序排列的，随时间变化的数据序列。

生活中各领域各行业太多时间序列的数据了，销售额，顾客数，访问量，股价，油价，GDP，气温...

随机过程的特征有均值、方差、协方差等。

如果随机过程的特征随着时间变化，则此过程是非平稳的；相反，如果随机过程的特征不随时间而变化，就称此过程是平稳的。

下图所示，左边非稳定，右边稳定。

![](/images/article/time_series.png)

非平稳时间序列分析时，若导致非平稳的原因是确定的，可以用的方法主要有趋势拟合模型、季节调整模型、移动平均、指数平滑等方法。

若导致非平稳的原因是随机的，方法主要有ARIMA及自回归条件异方差模型等。

## ARIMA

ARIMA模型全称为差分自回归移动平均模型(Autoregressive Integrated Moving Average Model,简记ARIMA)，也叫求和自回归移动平均模型，是由George Edward Pelham Box和Gwilym Meirion Jenkins于70年代初提出的一著名时间序列预测方法，所以又称为box-jenkins模型、博克思-詹金斯法。

>注：Gwilym Meirion Jenkins，1932～1982，英国统计学家。伦敦大学学院博士，兰卡斯特大学教授。

同《数学狂想曲（三）》中的PID算法一样，ARIMA模型实际上是三个简单模型的组合。

### AR模型

$$X_t = c + \sum_{i=1}^p \varphi_i X_{t-i}+ \varepsilon_t$$

其中，p为阶数，$$\varepsilon_t$$为白噪声。上式又记作**AR(p)**。显然，AR模型是一个系统状态模型。

### MA模型

$$X_t = \mu + \varepsilon_t + \sum_{i=1}^q \theta_i \varepsilon_{t-i}$$

上式记作**MA(q)**，其中q和$$\varepsilon_t$$的含义与上同。MA模型是一个噪声模型。

### ARMA模型

AR模型和MA模型合起来，就是ARMA模型：

$$X_t = c + \varepsilon_t +  \sum_{i=1}^p \varphi_i X_{t-i} + \sum_{i=1}^q \theta_i \varepsilon_{t-i}$$

同理，上式也被记作**ARMA(p,q)**。

### Lag operator

在继续下面的描述之前，我们先来定义一下Lag operator--L。

$$L X_t = X_{t-1} \; \text{or} \; X_t = L X_{t+1}$$

### I模型

$$(1-L)^d X_t$$

上式中d为阶数，因此上式也记作**I(d)**。显然$$I(0)=X_t$$。

I模型有什么用呢？我们观察一下I(1)：

$$(1-L) X_t = X_t - X_{t-1} = \Delta X$$

有的时候，虽然I(0)不是平稳序列，但I(1)是平稳序列，这时我们称该序列是**1阶平稳序列**。n阶的情况，可依此类推。

### ARIMA模型

ARIMA模型可以看作是两个随机过程的组合。

首先是非平稳过程：

$$Y_t = (1-L)^d X_t$$

接着是一个广义平稳过程：

$$\left( 1 - \sum_{i=1}^p \phi_i L^i \right) Y_t = \left( 1 + \sum_{i=1}^q \theta_i L^i \right) \varepsilon_t$$

最后得到ARIMA模型的公式：

$$\left( 1 - \sum_{i=1}^p \phi_i L^i\right)
(1-L)^d X_t = \delta + \left( 1 + \sum_{i=1}^q \theta_i L^i \right) \varepsilon_t$$

上式也被记作**ARIMA(p,d,q)**。从上式可以看出，ARIMA模型实际上就是利用I模型，将时间序列转化为平稳序列之后的ARMA模型。

>注：上面的内容只是对ARIMA模型给出一个简单的定义。实际的假设检验、参数估计的步骤，还是比较复杂的，完全可以写本书来说。

## 其它

除了ARIMA系列模型之外，ARCH系列模型也用的比较多：

autoregressive conditional heteroskedasticity, ARCH

generalized autoregressive conditional heteroskedasticity, GARCH

## 参考

https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average

https://en.wikipedia.org/wiki/Autoregressive%E2%80%93moving-average_model

https://zhuanlan.zhihu.com/p/23534595

时间序列分析：结合ARMA的卡尔曼滤波算法（该文的参考文献中有不少好文）

http://blog.csdn.net/aliceyangxi1987/article/details/71079522

用ARIMA模型做需求预测

http://blog.csdn.net/kicilove/article/details/78315335

时间序列初级理论篇

https://mp.weixin.qq.com/s/K-XGuaWTcF6BDPJagaJDPQ

时序数据与事件的关联分析

https://mp.weixin.qq.com/s/JR-GIXwHF45OysoE0qvwzw

时间序列异常检测机制的研究

https://mp.weixin.qq.com/s/2hpQ_7Ih58d1RKYb1oW_Sg

时间序列简介（一）

https://mp.weixin.qq.com/s/05WAZcklXnL_hFPLZW9t7Q

时间序列模型之相空间重构模型

https://zhuanlan.zhihu.com/p/34407471

如何理解时间序列？—从Riemann积分和Lebesgue积分谈起

https://zhuanlan.zhihu.com/p/35093835

时间序列的自回归模型—从线性代数的角度来看

https://mp.weixin.qq.com/s/lmJk-iIzxxPmnZa6D8i_nw

一文简述如何使用嵌套交叉验证方法处理时序数据

https://zhuanlan.zhihu.com/p/39105270

时间序列的表示与信息提取

https://mp.weixin.qq.com/s/iah8PvIC0oZngSaNHw7gJw

从上帝视角看透时间序列和数据挖掘

https://zhuanlan.zhihu.com/p/38130622

时间序列的相似性

https://mp.weixin.qq.com/s/p8oN4xh-FHnay2eTsk6Gng

基于高阶模糊认知图与小波变换的时间序列预测

https://mp.weixin.qq.com/s/DGGuAYsoa6DPD6FBf2Hc4g

时间序列分析之理论篇

https://zhuanlan.zhihu.com/p/50698719

两篇关于时间序列的论文

https://mp.weixin.qq.com/s/iKM6zMSm1F2icjy79F9Hcg

季节性的分析才不简单，小心不要在随机数据中也分析出季节性

https://zhuanlan.zhihu.com/p/55129654

时间序列的单调性

https://zhuanlan.zhihu.com/p/55903495

时间序列的聚类

https://mp.weixin.qq.com/s/avLWHXj2JkjXOomCipj8kA

使用希尔伯特-黄变换（HHT）进行时间序列分析

https://mp.weixin.qq.com/s/2teyejpbpM6x5UCiYL8s-Q

关于时间序列你需要了解的一切

https://mp.weixin.qq.com/s/FRSe1mJTvk9U66ta-r9iCQ

手把手教你用Python玩转时序数据，从采样、预测到聚类

https://mp.weixin.qq.com/s/Q82YzANWDMkKWm5k2XmPkA

严谨解决5种机器学习算法在预测股价的应用

https://mp.weixin.qq.com/s/PMsAjk7WbGRu2n3s6Q8prQ

Facebook时间序列预测算法Prophet的研究

# 异常检测

1.基于聚类算法的异常检测。

2.基于孤立森林算法的异常检测。

3.基于支持向量机算法的异常检测。

4.基于高斯分布的异常检测。

5.基于马尔可夫链的异常检测。

参考：

http://chuansong.me/n/377440751130

异常点检测算法（一）

http://jiangshuxia.9.blog.163.com/blog/static/3487586020083662621887/

异常(Outlier)检测算法综述

http://www.cnblogs.com/fengfenggirl/p/iForest.html

异常检测算法--Isolation Forest

https://mp.weixin.qq.com/s/xsuLIMPJVCThBGMRlz09Hg

Isolation Forest算法原理详解

http://www.bigdata8.top/front/article/466

异常值检测-滑动均值实现智能告警

https://mp.weixin.qq.com/s/ujG6Fr161kZh3S-lEiTJUg

异常检测（Anomaly Detection）

https://mp.weixin.qq.com/s/_Sds7O1wonVARKkb7qscww

腾讯：机器学习构建通用的数据异常检测平台

https://mp.weixin.qq.com/s/UcMPIf6ZRAhPjn79H2n1ig

异常点检测算法小结

https://mp.weixin.qq.com/s?__biz=MzIxODM4MjA5MA==&mid=2247487342&idx=3&sn=1981a6caec4591a19043d7a6176d359f

异常检测的阈值，你怎么选？给你整理好了...

https://mp.weixin.qq.com/s/ReQpT9KT6_tE8vXM-F_Ejw

从“马蜂窝事件”看，投资人如何避免数据尽职调查背后的交易风险？新时代数据造假特征及应对方法

https://www.zhihu.com/question/30508773

反欺诈(Fraud Detection)中所用到的机器学习模型有哪些？

https://mp.weixin.qq.com/s/jB5o4a6O4rrhMYD0GhKclw

基于机器学习算法的时间序列价格异常检测

https://mp.weixin.qq.com/s/PKX13Fv5fWmgX5IHLYgjmQ

基于无监督学习的期权定价异常检测
