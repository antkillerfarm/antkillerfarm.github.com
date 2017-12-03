---
layout: post
title:  机器学习（二十三）——单分类SVM&多分类SVM, 时间序列分析, 推荐算法中的常用排序算法
category: ML 
---

# Optimizer

## 参考（续）

https://mp.weixin.qq.com/s/q7BI-YyhtmNzUfBMTKVdqQ

Hitting time analysis of SGLD！

https://mp.weixin.qq.com/s/vt7BEHbwJrAzlL2Pc-6QFg

掌握机器学习数学基础之优化（上）

https://mp.weixin.qq.com/s/6NBLLLa-S625iaehR8zDfQ

掌握机器学习数学基础之优化（下）

https://mp.weixin.qq.com/s/o10Fp2VCwoLqgzirbGL9LQ

如何估算深度神经网络的最优学习率

https://mp.weixin.qq.com/s/T4f4W0V6YNBbjWqWBF19mA

目标函数的经典优化算法介绍

https://mp.weixin.qq.com/s/6u5W7Lm81Wtczdzp5WCJWw

DeepMind提出新型超参数最优化方法：性能超越手动调参和贝叶斯优化

https://mp.weixin.qq.com/s/0V8B-u5_bRM5Fu9oOAYjqw

清华大学：通过在单纯形上软门限投影的加速随机贪心坐标下降

https://mp.weixin.qq.com/s/fXlbB7KmiX0iIv6xwSxNIA

梯度下降法的三种形式BGD、SGD以及MBGD

https://mp.weixin.qq.com/s/R_0_E5Ieaj9KiWgg1prxeg

为什么梯度的方向与等高线切线方向垂直？

https://mp.weixin.qq.com/s/LuuvvL9yZ3ucXxRq0pZfsg

优化策略：Label Smoothing Regularization_LSR原理分析

https://zhuanlan.zhihu.com/p/23866364

从梯度下降到Hessian-Free优化

https://mp.weixin.qq.com/s/0gdGNv98DytB8KxwVu_M0A

通俗易懂讲解Deep Learning最优化方法之AdaGrad

# 单分类SVM&多分类SVM

原始的SVM主要用于二分类，然而稍加变化，也可用于单分类和多分类。

## 单分类SVM

单分类任务是一类特殊的分类任务。在该任务中，大多数样本只有positive一类标签，而其他样本则笼统的划为另一类。

单分类SVM（也叫Support Vector Domain Description(SVDD)）是一种单分类算法。和普通SVM相比，它不再使用maximum margin了，因为这里并没有两类的data。

单分类SVM的目标，实际上是确定positive样本的boundary。boundary之外的数据，会被分为另一类。这实际上就是一种异常检测的算法了。它主要适用于negative样本的特征不容易确定的场景。

![](/images/article/one_class_svm.png)

这里可以假设最好的boundary要远离feature space中的原点。左边是在original space中的boundary，可以看到有很多的boundary都符合要求，但是比较靠谱的是找一个比较紧（closeness）的boundary（红色的）。这个目标转换到feature space就是找一个离原点比较远的boundary，同样是红色的直线。

当然这些约束条件都是人为加上去的，你可以按照你自己的需要采取相应的约束条件。比如让data的中心离原点最远。

下面我们讨论一下SVDD的算法实现。

首先定义需要最小化的目标函数：

$$\begin{align}
&\operatorname{min}& & F(R,a,\xi_i) = R^2 + C \sum_{i=1}^N \xi_i\\
&\operatorname{s.t.}& & (x_i - a)^T (x_i - a) \leq R^2 + \xi_i\text{,} \qquad \xi_i \geq 0
\end{align}$$

这里a表示形状的中心，R表示半径，C和$$\xi$$的含义与普通SVM相同。

Lagrangian算子：

$$L(R,a,\alpha_i,\xi_i) = R^2 + C \sum_{i=1}^N \xi_i - \sum_{i=1}^N \gamma_i \xi_i - \sum_{i=1}^N \alpha_i \left(  R^2 + \xi_i - (x_i - c)^T (x_i - c) \right)$$

对偶问题：

$$L = \sum_{i=1}^N \alpha_i (x_i^T \cdot x_i) - \sum_{i,j=1}^N \alpha_i \alpha_j (x_i^T \cdot x_i)$$

使用核函数：

$$L = \sum_{i=1}^N \alpha_i K(x_i,x_i) - \sum_{i,j=1}^N \alpha_i \alpha_j K(x_i,x_j)$$

预测函数：

$$y(x) = \sum_{i=1}^N \alpha_i K(x,x_n) + b$$

根据计算结果的符号，来判定是正常样本，还是异常样本。

参考：

https://www.projectrhea.org/rhea/index.php/One_class_svm

One-Class Support Vector Machines for Anomaly Detection

https://www.zhihu.com/question/22365729

什么是一类支持向量机（one class SVM）

## 多分类SVM

多分类任务除了使用多分类算法之外，也可以通过对两分类算法的组合来实施多分类。常用的方法有两种：one-against-rest和DAG SVM。

### one-against-rest

比如我们有5个类别，第一次就把类别1的样本定为正样本，其余2，3，4，5的样本合起来定为负样本，这样得到一个两类分类器，它能够指出一篇文章是还是不是第1类的；第二次我们把类别2的样本定为正样本，把1，3，4，5的样本合起来定为负样本，得到一个分类器，如此下去，我们可以得到5个这样的两类分类器（总是和类别的数目一致）。

但有时也会出现两种很尴尬的情况，例如拿一篇文章问了一圈，每一个分类器都说它是属于它那一类的，或者每一个分类器都说它不是它那一类的，前者叫分类重叠现象，后者叫不可分类现象。

分类重叠倒还好办，随便选一个结果都不至于太离谱，或者看看这篇文章到各个超平面的距离，哪个远就判给哪个。不可分类现象就着实难办了，只能把它分给第6个类别了……

更要命的是，本来各个类别的样本数目是差不多的，但“其余”的那一类样本数总是要数倍于正类（因为它是除正类以外其他类别的样本之和嘛），这就人为的造成了“数据集偏斜”问题。

### DAG SVM

![](/images/article/dag_svm.png)

DAG SVM（也称one-against-one）的分类思路如上图所示。

粗看起来DAG SVM的分类次数远超one-against-rest，然而由于每次分类都只使用了部分数据，因此，DAG SVM的计算量反而更小。

其次，DAG SVM的误差上限有理论保障，而one-against-rest则不然（准确率可能降为0）。

显然，上面提到的两种方法，不仅可用于SVM，也适用于其他二分类算法。

参考：

http://www.blogjava.net/zhenandaci/archive/2009/03/26/262113.html

将SVM用于多类分类

## Hinge Loss

在之前的SVM的推导中，我们主要是从解析几何的角度，给出了SVM的计算公式。但SVM实际上也是有loss function的：

$$\ell(y) = \max(0, 1-t \cdot y)$$

其中，$$t=\pm 1$$表示分类标签，$$y=w \cdot x+b$$表示分类超平面计算的score。可以看出当t和y有相同的符号时（意味着 y 预测出正确的分类），loss为0。反之，则会根据y线性增加one-sided error。

![](/images/article/hinge_loss.png)

由于其函数形状像个合叶，因此又名Hinge Loss函数。

多分类SVM的Hinge Loss公式：

$$\ell(y) = \sum_{t \ne y} \max(0, 1 + \mathbf{w}_t \mathbf{x} - \mathbf{w}_y \mathbf{x})$$

参见：

http://www.jianshu.com/p/4a40f90f0d98

Hinge loss

http://mp.weixin.qq.com/s/96_u9QM63SISwTbimmH6wA

支持向量回归机

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

其中，p为阶数，$$\varepsilon_t$$为白噪声。上式又记作AR(p)。显然，AR模型是一个系统状态模型。

### MA模型

$$X_t = \mu + \varepsilon_t + \sum_{i=1}^q \theta_i \varepsilon_{t-i}$$

上式记作MA(q)，其中q和$$\varepsilon_t$$的含义与上同。MA模型是一个噪声模型。

### ARMA模型

AR模型和MA模型合起来，就是ARMA模型：

$$X_t = c + \varepsilon_t +  \sum_{i=1}^p \varphi_i X_{t-i} + \sum_{i=1}^q \theta_i \varepsilon_{t-i}$$

### Lag operator

在继续下面的描述之前，我们先来定义一下Lag operator--L。

$$L X_t = X_{t-1} \; \text{or} \; X_t = L X_{t+1}$$

### I模型

$$(1-L)^d X_t$$

上式中d为阶数，因此上式也记作I(d)。显然$$I(0)=X_t$$。

I模型有什么用呢？我们观察一下I(1)：

$$(1-L) X_t = X_t - X_{t-1} = \Delta X$$

有的时候，虽然I(0)不是平稳序列，但I(1)是平稳序列，这时我们称该序列是**1阶平稳序列**。n阶的情况，可依此类推。

### ARIMA模型

$$Y_t = (1-L)^d X_t$$

$$\left( 1 - \sum_{i=1}^p \phi_i L^i \right) Y_t = \left( 1 + \sum_{i=1}^q \theta_i L^i \right) \varepsilon_t$$

从上式可以看出，ARIMA模型实际上就是利用I模型，将时间序列转化为平稳序列之后的ARMA模型。

>注：上面的内容只是对ARIMA模型给出一个简单的定义。实际的假设检验、参数估计的步骤，还是比较复杂的，完全可以写本书来说。

参考：

https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average

https://en.wikipedia.org/wiki/Autoregressive%E2%80%93moving-average_model

https://zhuanlan.zhihu.com/p/23534595

时间序列分析：结合ARMA的卡尔曼滤波算法（该文的参考文献中有不少好文）

http://blog.csdn.net/aliceyangxi1987/article/details/71079522

用ARIMA模型做需求预测

http://blog.csdn.net/kicilove/article/details/78315335

时间序列初级理论篇

# 推荐算法中的常用排序算法

## Pointwise方法

Pranking (NIPS 2002), OAP-BPM (EMCL 2003), Ranking with Large Margin Principles (NIPS 2002), Constraint Ordinal Regression (ICML 2005)。

## Pairwise方法

Learning to Retrieve Information (SCC 1995), Learning to Order Things (NIPS 1998), Ranking SVM (ICANN 1999), RankBoost (JMLR 2003), LDM (SIGIR 2005), RankNet (ICML 2005), Frank (SIGIR 2007), MHR(SIGIR 2007), Round Robin Ranking (ECML 2003), GBRank (SIGIR 2007), QBRank (NIPS 2007), MPRank (ICML 2007), IRSVM (SIGIR 2006)。

## Listwise方法

LambdaRank (NIPS 2006), AdaRank (SIGIR 2007), SVM-MAP (SIGIR 2007), SoftRank (LR4IR 2007), GPRank (LR4IR 2007), CCA (SIGIR 2007), RankCosine (IP&M 2007), ListNet (ICML 2007), ListMLE (ICML 2008) 。

## 参考

https://mp.weixin.qq.com/s/YjYVE6jzySVsZmXSPivB5w

达观数据搜索引擎排序实践（上篇）

https://mp.weixin.qq.com/s/UpN7tAMjbFLSPcDYsWaykg

达观数据搜索引擎排序实践（下篇）

https://mp.weixin.qq.com/s/xigME-griWFwEvvPNqWuvg

美团点评联盟广告的场景化定向排序机制

