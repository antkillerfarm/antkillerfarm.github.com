---
layout: post
title:  机器学习（十一）——因子分析（1）
category: ML 
---

## EM算法的一般形式（续）

由之前的讨论可以看出，E-Step中的步骤是使上式的等号成立的条件，即：

$$\ell(\theta^{(t)})=\sum_i\sum_{z^{(i)}}Q_i^{(t)}(z^{(i)})\log\frac{p(x^{(i)},z^{(i)};\theta^{(t)})}{Q_i^{(t)}(z^{(i)})}$$

因为公式3对于任意$$Q_i$$和$$\theta$$都成立，因此：

$$\ell(\theta^{(t+1)})\ge\sum_i\sum_{z^{(i)}}Q_i^{(t)}(z^{(i)})\log\frac{p(x^{(i)},z^{(i)};\theta^{(t+1)})}{Q_i^{(t)}(z^{(i)})}$$

因为M-Step的最大化过程，可得：

$$\sum_i\sum_{z^{(i)}}Q_i^{(t)}(z^{(i)})\log\frac{p(x^{(i)},z^{(i)};\theta^{(t+1)})}{Q_i^{(t)}(z^{(i)})}\ge\sum_i\sum_{z^{(i)}}Q_i^{(t)}(z^{(i)})\log\frac{p(x^{(i)},z^{(i)};\theta^{(t)})}{Q_i^{(t)}(z^{(i)})}$$

综上可得：$$\ell(\theta^{(t)})\le \ell(\theta^{(t+1)})$$

事实上，如果我们定义：

$$J(Q,\theta)=\sum_i\sum_{z^{(i)}}Q_i(z^{(i)})\log\frac{p(x^{(i)},z^{(i)};\theta)}{Q_i(z^{(i)})}$$

则EM算法可以看作是J函数的坐标上升法。E-Step固定$$\theta$$，优化Q；M-Step固定Q，优化$$\theta$$。

参考：

https://mp.weixin.qq.com/s/NbM4sY93kaG5qshzgZzZIQ

EM算法的九层境界：​Hinton和Jordan理解的EM算法

https://mp.weixin.qq.com/s/FSID_FLMVxrKEj_padg_bQ

EM算法是炼金术吗？

https://mp.weixin.qq.com/s/9G_7Ax9cPcQcYVqEfc-pyw

从基础知识到实际应用，一文了解“机器学习非凸优化技术”

https://mp.weixin.qq.com/s/JSnPTVRgXO3aih5srU1eZQ

EM算法原理总结

https://mp.weixin.qq.com/s/192sLXAvLKzwsTKCZs-AuA

一文详尽系列之EM算法

## 重新审视混合高斯模型

下面给出混合高斯模型各参数的推导过程。

E-Step很简单：

$$w_j^{(i)}=Q_i(z^{(i)}=j)=p(z^{(i)}=j\mid x^{(i)};\phi,\mu,\Sigma)$$

在M-Step中，我们将各个变量和分布的概率密度函数代入，可得：

$$\begin{align}
&\sum_{i=1}^m\sum_{z^{(i)}}Q_i(z^{(i)})\log\frac{p(x^{(i)},z^{(i)};\theta)}{Q_i(z^{(i)})}
\\&=\sum_{i=1}^m\sum_{j=1}^kQ_i(z^{(i)}=j)\log\frac{p(x^{(i)}\mid z^{(i)}=j;\mu,\Sigma)p(z^{(i)}=j;\phi)}{Q_i(z^{(i)}=j)}
\\&=\sum_{i=1}^m\sum_{j=1}^kw_j^{(i)}\log\frac{\frac{1}{(2\pi)^{n/2}\lvert\Sigma_j\rvert^{1/2}}\exp\left(-\frac{1}{2}(x^{(i)}-\mu_j)^T\Sigma_j^{-1}(x^{(i)}-\mu_j)\right)\cdot \phi_j}{w_j^{(i)}}
\end{align}$$

对$$\mu_l$$求导，可得：

$$\begin{align}
&\nabla_{\mu_l}\sum_{i=1}^m\sum_{j=1}^kw_j^{(i)}\log\frac{\frac{1}{(2\pi)^{n/2}\lvert\Sigma_j\rvert^{1/2}}\exp\left(-\frac{1}{2}(x^{(i)}-\mu_j)^T\Sigma_j^{-1}(x^{(i)}-\mu_j)\right)\cdot \phi_j}{w_j^{(i)}}
\\&=-\nabla_{\mu_l}\sum_{i=1}^m\sum_{j=1}^kw_j^{(i)}\frac{1}{2}(x^{(i)}-\mu_j)^T\Sigma_j^{-1}(x^{(i)}-\mu_j)
\\&=\frac{1}{2}\sum_{i=1}^mw_l^{(i)}\nabla_{\mu_l}(2\mu_l^T\Sigma_l^{-1}x^{(i)}-\mu_l^T\Sigma_l^{-1}\mu_l)=\sum_{i=1}^mw_l^{(i)}(\Sigma_l^{-1}x^{(i)}-\Sigma_l^{-1}\mu_l)\tag{4}
\end{align}$$

这里对最后一步的推导，做一个说明。为了便于以下的讨论，我们引入符号“tr”，该符号表示矩阵的主对角线元素之和，也被叫做矩阵的“迹”（Trace）。按照通常的写法，在不至于误会的情况下，tr后面的括号会被省略。

tr的其他性质还包括：

$$\operatorname{tr}\,a=a,a\in R\tag{5.1}$$

$$\operatorname{tr}(A + B) = \operatorname{tr}(A) + \operatorname{tr}(B)\tag{5.2}$$

$$\operatorname{tr}(cA) = c \operatorname{tr}(A)\tag{5.3}$$

$$\operatorname{tr}(A) = \operatorname{tr}(A^{\mathrm T})\tag{5.4}$$

$$\operatorname{tr}(AB) = \operatorname{tr}(BA)\tag{5.5}$$

$$\operatorname{tr}(ABCD) = \operatorname{tr}(BCDA) = \operatorname{tr}(CDAB) = \operatorname{tr}(DABC)\tag{5.6}$$

$$\nabla_A\operatorname{tr}(AB)=B^T\tag{5.7}$$

$$\nabla_{A^T}f(A)=(\nabla_Af(A))^T\tag{5.8}$$

$$\nabla_A\operatorname{tr}(ABA^TC)=CAB+C^TAB^T\tag{5.9}$$

$$\nabla_{A^T}\operatorname{tr}(ABA^TC)=B^TA^TC^T+BA^TC\tag{5.10}$$

$$\nabla_A\mid A\mid =\mid A\mid (A^{-1})^T\tag{5.11}$$

因为$$\mu_l^T\Sigma_l^{-1}\mu_l$$是实数，由公式5.1可得：

$$\nabla_{\mu_l}\mu_l^T\Sigma_l^{-1}\mu_l=\nabla_{\mu_l}\operatorname{tr}(\mu_l^T\Sigma_l^{-1}\mu_l)$$

由公式5.10可得：

$$\nabla_{\mu_l}\operatorname{tr}(\mu_l^T\Sigma_l^{-1}\mu_l)=(\Sigma_l^{-1})^T(\mu_l^T)^TI^T+\Sigma_l^{-1}(\mu_l^T)^TI=(\Sigma_l^{-1})^T\mu_l+\Sigma_l^{-1}\mu_l$$

因为$$\Sigma_l^{-1}$$是对称矩阵，因此，综上可得：

$$\nabla_{\mu_l}\mu_l^T\Sigma_l^{-1}\mu_l=2\Sigma_l^{-1}\mu_l\tag{5.12}$$

回到正题，令公式4等于0，可得：

$$\mu_j:=\frac{\sum_{i=1}^mw_j^{(i)}x^{(i)}}{\sum_{i=1}^mw_j^{(i)}}$$

同样的，对$$\phi_j$$求导，可得：

$$\sum_{i=1}^m\sum_{j=1}^kw_j^{(i)}\log\phi_j$$

因为存在$$\sum_{j=1}^k\phi_j=1$$这样的约束，因此需要使用拉格朗日函数：

$$\mathcal{L}(\phi)=\sum_{i=1}^m\sum_{j=1}^kw_j^{(i)}\log\phi_j+\beta(\sum_{j=1}^k\phi_j-1)$$

这里的$$\beta$$是拉格朗日乘子，$$\phi_j\ge 0$$的约束条件不用考虑，因为对$$\log\phi_j$$求导已经隐含了这个条件。

因此：

$$\frac{\partial\mathcal{L}(\phi)}{\partial\phi_j}=\sum_{i=1}^m\frac{w_j^{(i)}}{\phi_j}+\beta$$

令上式等于0，可得：

$$\frac{\sum_{i=1}^mw_j^{(i)}}{\phi_j}=-\beta=\frac{\sum_{i=1}^m\sum_{j=1}^kw_j^{(i)}}{\sum_{j=1}^k\phi_j}$$

因为$$\sum_{j=1}^k\phi_j=1,\sum_{j=1}^kw_j^{(i)}=1$$，所以：

$$-\beta=\frac{\sum_{i=1}^m1}{1}=m$$

因此：

$$\phi_j:=\frac{1}{m}\sum_{i=1}^mw_j^{(i)}$$  

# 因子分析

之前的讨论都是基于样本个数m远大于特征数n的，现在来看看$$m\ll n$$的情况。

这种情况本质上意味着，样本只覆盖了很小一部分的特征空间。当我们应用高斯模型的时候，会发现协方差矩阵$$\Sigma$$根本就不存在，自然也就没法利用之前的方法了。

那么我们应该怎么办呢？

## 对$$\Sigma$$的限制

有两种方法可以对$$\Sigma$$进行限制。

方法一：

设定$$\Sigma$$为对角线矩阵，即所有非对角线元素都是0。其对角线元素为：

$$\Sigma_{jj}=\frac{1}{m}\sum_{i=1}^m(x_j^{(i)}-\mu_j)^2$$

二维多元高斯分布在平面上的投影是个椭圆，中心点由$$\mu$$决定，椭圆的形状由$$\Sigma$$决定。$$\Sigma$$如果变成对角阵，就意味着椭圆的两个轴都和坐标轴平行了。

方法二：

还可以进一步约束$$\Sigma$$，可以假设对角线上的元素都是相等的，即：

$$\Sigma=\sigma^2I$$

其中：

$$\sigma=\frac{1}{mn}\sum_{j=1}^n\sum_{i=1}^m(x_j^{(i)}-\mu_j)^2$$

这实际上也就是方法一中对角线元素的均值，反映到二维高斯分布图上就是椭圆变成圆。

当我们要估计出完整的$$\Sigma$$时，我们需要$$m\ge n+1$$才能保证在最大似然估计下得出的$$\Sigma$$是非奇异的。然而在上面的任何一种假设限定条件下，只要$$m\ge 2$$就可以估计出限定的$$\Sigma$$。

这样做的缺点也是显而易见的，我们认为特征间相互独立，这个假设太强。接下来，我们给出一种称为因子分析（factor analysis）的方法，使用更多的参数来分析特征间的关系，并且不需要计算一个完整的$$\Sigma$$。

## 利用多元高斯分布密度函数计算积分的技巧

$$I(A,b,c)=\int_x\exp\left(-\frac{1}{2}(x^TAx+x^Tb+c)\right)\mathrm{d}x$$

其中A为对称正定矩阵，b为向量。对于上面这样的积分，可以使用“完全配方法”（completion-of-squares）的数学技巧求解。

因为

$$x^TAx+x^Tb+c=(x-h)^TA(x-h)+k$$

其中$$h=-\frac{A^{-1}b}{2},k=c-\frac{b^TA^{-1}b}{4}$$。

所以

$$\begin{align}
I(A,b,c)&=\int_x\exp\left(-\frac{1}{2}((x - h)^TA(x - h)+k)\right)\mathrm{d}x
\\&=\int_x\exp\left(-\frac{1}{2}(x - h)^TA(x - h)-k/2\right)\mathrm{d}x
\\&=\exp(-k/2)\cdot\int_x\exp\left(-\frac{1}{2}(x - h)^TA(x - h)\right)\mathrm{d}x
\end{align}$$

令$$\mu=h,\Sigma=A^{-1}$$，则：

$$I(A,b,c)=\frac{(2\pi)^{n/2}\lvert\Sigma\rvert^{1/2}}{\exp(k/2)}\cdot\int_x\frac{1}{(2\pi)^{n/2}\lvert\Sigma\rvert^{1/2}}\exp\left(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)\right)\mathrm{d}x$$

公式右侧的被积分函数，正好是多元高斯分布密度函数，因此该积分值为1。于是：

$$I(A,b,c)=\frac{(2\pi)^{n/2}\lvert\Sigma\rvert^{1/2}}{\exp(k/2)}$$

>注：原始讲义里，Chuong B. Do写的《Gaussian processes》的附录A.1和本节内容类似，但推导过程有问题，疑似笔误，特更换为维基百科中的例子。（矩阵的完全配方那块的变换，我能推导出维基百科的结果，但推导不出Chuong B. Do的结果。）如有错误，望读者指出。

## 边缘和条件高斯分布

假设x由两个随机向量组成（可以看作是将之前的$$x^{(i)}$$分成了两部分）。

$$x=\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}$$

其中$$x_1\in R^r,x_1\in R^s$$，则x实际上是$$r+s$$维向量。

假设$$x\sim N(\mu,\Sigma)$$，其中：

$$\mu=\begin{bmatrix} \mu_1 \\ \mu_2 \end{bmatrix},\Sigma=\begin{bmatrix} \Sigma_{11} & \Sigma_{12} \\ \Sigma_{21} & \Sigma_{22} \end{bmatrix}$$

因为协方差矩阵是对称矩阵，因此$$\Sigma_{12}=\Sigma_{21}^T$$。

$$\begin{align}
Cov(x)&=\Sigma=\begin{bmatrix} \Sigma_{11} & \Sigma_{12} \\ \Sigma_{21} & \Sigma_{22} \end{bmatrix}
\\&=E\begin{bmatrix} (x-\mu)(x-\mu)^T \end{bmatrix}=E\begin{bmatrix} \begin{pmatrix} x_1-\mu_1 \\ x_2-\mu_2 \end{pmatrix} & \begin{pmatrix} x_1-\mu_1 \\ x_2-\mu_2 \end{pmatrix} \end{bmatrix}
\\&=E\begin{bmatrix} (x_1-\mu_1)(x_1-\mu_1)^T & (x_1-\mu_1)(x_2-\mu_2)^T \\ (x_2-\mu_2)(x_1-\mu_1)^T & (x_2-\mu_2)(x_2-\mu_2)^T \end{bmatrix}
\end{align}$$

因此，$$E[x_1]=\mu_1,Cov(x_1)=E[(x_1-\mu_1)(x_1-\mu_1)^T]=\Sigma_{11}$$。可见，多元高斯分布的边缘分布仍然是多元高斯分布。

下面讨论一下条件高斯分布。

$$\begin{align}
p(x_1\mid x_2)&=\frac{p(x_1,x_2)}{p(x_2)}=\frac{\frac{1}{(2\pi)^{n/2}\lvert\Sigma\rvert^{1/2}}\exp\left(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)\right)}{\int_{x_1}p(x_1,x_2;\mu,\Sigma)\mathrm{d}x_1}
\\&=\frac{1}{Z_1}\exp\left\{-\frac{1}{2}\left(\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}-\begin{bmatrix} \mu_1 \\ \mu_2 \end{bmatrix}\right)^T\begin{bmatrix} V_{11} & V_{12} \\ V_{21} & V_{22} \end{bmatrix}\left(\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}-\begin{bmatrix} \mu_1 \\ \mu_2 \end{bmatrix}\right)\right\}
\end{align}$$

其中的$$Z_1$$是和$$x_1$$无关的部分，可看作常数，下面的$$Z_i$$也是同理。

$$\Sigma^{-1}=V=\begin{bmatrix} V_{11} & V_{12} \\ V_{21} & V_{22} \end{bmatrix}$$

因为：

$$\begin{align}
&\left(\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}-\begin{bmatrix} \mu_1 \\ \mu_2 \end{bmatrix}\right)^T\begin{bmatrix} V_{11} & V_{12} \\ V_{21} & V_{22} \end{bmatrix}\left(\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}-\begin{bmatrix} \mu_1 \\ \mu_2 \end{bmatrix}\right)
\\&=(x_1-\mu_1)^TV_{11}(x_1-\mu_1)+(x_1-\mu_1)^TV_{12}(x_2-\mu_2)
\\&\qquad+(x_2-\mu_2)^TV_{21}(x_1-\mu_1)+(x_2-\mu_2)^TV_{22}(x_2-\mu_2)
\end{align}$$
