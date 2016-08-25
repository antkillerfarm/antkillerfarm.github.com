---
layout: post
title:  机器学习（六）——SVM（4）、学习理论
category: technology 
---

## SMO算法在线性SVM时的简化

由《机器学习（四）》一节的公式3，可得：

$$w^{new}-y^{(1)}\alpha_1^{new}x^{(1)}-y^{(2)}\alpha_2^{new}x^{(2)}=w-y^{(1)}\alpha_1x^{(1)}-y^{(2)}\alpha_2x^{(2)}$$

$$w^{new}=w+y^{(1)}(\alpha_1^{new}-\alpha_1)x^{(1)}+y^{(2)}(\alpha_2^{new}-\alpha_2)x^{(2)}$$

需要注意的是，虽然我们在之前的讨论中，使用$$K_{ij}$$代替$$\langle x^{(i)},x^{(j)}\rangle$$进行扩展，然而两者只有在线性的情况下，才是等价的，即$$\phi(x)=x$$。

K函数最大的作用不在于计算线性SVM，而在于计算非线性的SVM。

## SMO中拉格朗日乘子的启发式选择方法

所谓的启发式选择方法主要思想是每次选择拉格朗日乘子的时候，优先选择$$0<\alpha_i<C$$的$$\alpha_i$$作优化，因为边界上的$$\alpha_i$$在迭代之后通常不会更改。

给定初始值$$\alpha_i=0$$后，先对所有样例进行循环，循环中碰到违背KKT条件的（不管边界上还是边界内）都进行迭代更新。等这轮过后，如果没有收敛，第二轮就只针对$$0<\alpha_i<C$$的样例进行迭代更新。

注：软边距SVM问题的KKT条件为：

$$\alpha_i=0 \Leftrightarrow y^{(i)}u_i\ge 1$$

$$0<\alpha_i<C \Leftrightarrow y^{(i)}u_i=1$$

$$\alpha_i=C \Leftrightarrow y^{(i)}u_i\le 1$$

在第一个乘子选择后，第二个乘子也使用启发式方法选择， 第二个乘子的迭代步长大致正比于$$\lvert E_1-E_2\rvert$$，选择能使$$\lvert E_1-E_2\rvert$$最大的$$\alpha_2$$即可。

最后的收敛条件是在界内（$$0<\alpha_i<C$$）的样例都能够遵循KKT条件，且其对应的$$\alpha_i$$只在极小的范围内变动。

# 学习理论

## 偏差和方差

![](/images/article/interpolation.png)

回到之前的欠拟合与过拟合的例子。我们把预测值和实际值之间的误差称为泛化误差。注意：泛化误差不是拟合模型和训练样本值之间的差，后者通常被称作模型误差。

偏差（bias）和方差（variance）都是泛化误差（generalization error）的组成部分。但遗憾的是，这两个名词至今也没有公认的严格定义，这里只做定性的描述，即：欠拟合的误差主要是偏差，而过拟合的误差主要是方差。

## 学习理论的预备知识

学习理论（learning theory）主要解决三大问题：

1.偏差和方差的权衡。这实际上是模型选择的问题。如何才能自动确定模型的阶数呢？

2.如何关联模型误差和泛化误差？

3.如何确定我们的学习算法是有效的？

首先介绍两个定理：

**The union bound定理**：

如果$$A_1,\dots,A_k$$是k个不同的事件，则：

$$P(A_1\cup \dots\cup A_k)\le P(A_1)+\dots+P(A_k)$$

**Hoeffding不等式**：

$$Z_1,\dots,Z_m$$是m个独立且具有相同分布的随机变量（independent and identically distributed，IID）。如果它们满足Bernoulli($$\phi$$)分布，即$$P(Z_i=1)=\phi,P(Z_i=0)=1-\phi$$，则：

$$P(\lvert\phi-\hat\phi\rvert>\gamma)\le 2\exp(-2\gamma^2m)$$

其中，$$\hat\phi=(1/m)\sum_{i=1}^mZ_i$$，$$\gamma$$是大于0的任意常数。

这个不等式是Wassily Hoeffding于1963年证明的。它表明样本数量越大，则随机变量的平均值越接近其数学期望值。

注：Wassily Hoeffding，1914~1991，芬兰统计学家，柏林大学博士，无偏统计学（U-statistics）创始人。

这个不等式在统计学领域也叫做Chernoff bound，但实际上是Herman Chernoff的朋友Herman Rubin发现的。他们的关系有点像苹果公司的那两个Steve，都是统计学领域的巨擘。

注：Herman Chernoff，1923年生，美国数学家、物理学家，布朗大学博士，先后执教于MIT和哈佛。

Herman Rubin，1926年生，美国数学家，21岁获得芝加哥大学的博士学位。现为普渡大学教授。

Herman Chernoff写过一篇文章回忆他和Herman Rubin的友谊，其中提到后者IQ 180，比他牛多了。其实，Herman Chernoff 24岁获得博士学位，也是妥妥的学霸级人物。

以下假定y的取值为0或1，则：

$$\hat\varepsilon(h)=\frac{1}{m}\sum_{i=1}^m1\{h(x^{(i)})\neq y^{(i)}\}$$

$$\hat\varepsilon(h)$$被称作训练误差（training error），也叫做经验风险（empirical risk）或经验误差（empirical error），它表征的是在训练样本集上，预测函数误分类的比率。

$$\varepsilon(h)=P^{(x,y)~\mathcal{D}}(h(x)\neq y)$$

$$\varepsilon(h)$$表示泛化误差，$$(x,y)$$表示被预测的样本，$$mathcal{D}$$表示样本所遵循的概率分布。

这里我们假设：训练数据和预测数据都具有相同的概率分布$$\mathcal{D}$$。这个假设是PAC理论的假设之一。

PAC（Probably approximately correct）理论是Leslie Valiant于1984年提出的。这里的大部分讨论都和PAC有关。

注：Leslie Valiant，1949年生，英国计算机科学家，华威大学博士。哈佛大学教授，英国皇家学会会员，图灵奖获得者（2010）。

对于线性分类$$h_\theta(x)=1\{\theta^Tx\ge 0\}$$来说，寻找合适的参数$$\theta$$，还有另一种方法，即最小化训练误差，并令：

$$\hat\theta=\arg\min_\theta\hat\varepsilon(h_\theta)$$

我们将这个过程称为经验风险最小化（empirical risk minimization，ERM）。其最终的预测函数为$$\hat h=h_{\hat\theta}$$。

ERM是一类基本的学习算法，也是本节关注的焦点。

我们定义预测函数类（hypothesis class ）$$\mathcal{H}$$，用以表示解决某类学习问题的所有分类器的集合。则ERM算法可表示为：

$$\hat h=\arg\min_{h\in \mathcal{H}}\hat\varepsilon(h)$$

## $$\mathcal{H}$$为有限集的情况

根据之前的讨论，我们做如下定义：

$$Z=1\{h_i(x)\neq y\}$$

$$Z_j=1\{h_i(x^{(j)})\neq y^{(j)}\}$$

$$\hat\varepsilon(h_i)=\frac{1}{m}\sum_{j=1}^mZ_j$$

其中，$$h_i\in \mathcal{H}$$。

由Hoeffding不等式可知：

$$P(\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert>\gamma)\le 2\exp(-2\gamma^2m)$$

这个公式表明：对于特定的$$h_i$$，在m很大的情况下，训练误差有很大的概率接近于泛化误差。

如果我们用$$A_i$$表示事件$$\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert>\gamma$$，则上式可改为$$P(A_i)\le 2\exp(-2\gamma^2m)$$。

$$P(\exists h\in\mathcal{H}.\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert>\gamma)=P(A_1\cup \dots\cup A_k)$$

$$\le \sum_{i=1}^kP(A_i)\le \sum_{i=1}^k2\exp(-2\gamma^2m)=2k\exp(-2\gamma^2m)$$

$$\begin{align}
&1-P(\exists h\in\mathcal{H}.\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert>\gamma)=P(\lnot\exists h\in\mathcal{H}.\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert>\gamma)
\\&=P(\forall h\in\mathcal{H}.\lvert\varepsilon(h_i)-\hat\varepsilon(h_i)\rvert\le\gamma)\ge 1-2k\exp(-2\gamma^2m)
\end{align}$$

上面的结果表明，对于所有的$$h\in \mathcal{H}$$，实际上也有一个收敛限制。

上式变形可得：

$$m\ge \frac{1}{2\gamma^2}\log\frac{2k}{\delta}$$

其中，$$\delta=2k\exp(-2\gamma^2m)$$。

上面的公式表明，在给定$$\gamma$$和$$\delta$$的情况，至少需要多少训练样本

## $$\mathcal{H}$$为无限集的情况

