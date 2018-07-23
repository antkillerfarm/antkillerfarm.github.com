---
layout: post
title:  机器学习（十二）——机器学习中的矩阵方法（1）LU分解
category: ML 
---

## 因子分析模型（续）

又因为$$E[zz^T]=Cov(z)=I$$，所以$$\Sigma_{zx}=\Lambda^T$$。

$$\begin{align}
\Sigma_{xx}&=E[(x-E[x])(x-E[x])^T]=E[(\mu+\Lambda z+\epsilon-\mu)(\mu+\Lambda z+\epsilon-\mu)^T]
\\&=E[(\Lambda z+\epsilon)(\Lambda z^T+\epsilon^T)]=E[\Lambda z(\Lambda z)^T+\epsilon(\Lambda z)^T+\Lambda z\epsilon^T+\epsilon\epsilon^T]
\\&=E[\Lambda zz^T\Lambda^T+\epsilon z^T\Lambda^T+\Lambda z\epsilon^T+\epsilon\epsilon^T]
\\&=\Lambda E[zz^T]\Lambda^T+E[\epsilon z^T]\Lambda^T+\Lambda E[z\epsilon^T]+E[\epsilon\epsilon^T]
\\&=\Lambda I\Lambda^T+0+0+\Psi=\Lambda \Lambda^T+\Psi
\end{align}$$

把这些结果合在一起，可得：

$$\begin{bmatrix} z \\ x \end{bmatrix}\sim N\left(\begin{bmatrix} \vec{0} \\ \mu \end{bmatrix},\begin{bmatrix} I & \Lambda^T \\ \Lambda & \Lambda \Lambda^T+\Psi \end{bmatrix}\right)\tag{1}$$

从这个结论可以看出：$$x\sim N(\mu,\Lambda \Lambda^T+\Psi)$$

因此它的对数似然函数为：

$$\ell(\mu,\Lambda,\Psi)=\log\prod_{i=1}^m\frac{1}{(2\pi)^{n/2}\lvert\Lambda \Lambda^T+\Psi\rvert^{1/2}}\exp\left(-\frac{1}{2}(x^{(i)}-\mu)^T(\Lambda \Lambda^T+\Psi)^{-1}(x^{(i)}-\mu)\right)$$

但这个函数是很难最大化的，需要使用EM算法解决之。

## 因子分析的EM估计

E-step比较简单。由《机器学习（十）》公式1、2和公式1，可得：

$$\mu_{z^{(i)}\mid x^{(i)}}=\Lambda^T(\Lambda \Lambda^T+\Psi)^{-1}(x^{(i)}-\mu)$$

$$\Sigma_{z^{(i)}\mid x^{(i)}}=I-\Lambda^T(\Lambda \Lambda^T+\Psi)^{-1}\Lambda$$

因此：

$$Q_i(z^{(i)})=\frac{1}{(2\pi)^{n/2}\lvert\Sigma_{z^{(i)}\mid x^{(i)}}\rvert^{1/2}}\exp\left(-\frac{1}{2}(x^{(i)}-\mu_{z^{(i)}\mid x^{(i)}})^T\Sigma_{z^{(i)}\mid x^{(i)}}^{-1}(x^{(i)}-\mu_{z^{(i)}\mid x^{(i)}})\right)$$

M-step的最大化的目标是：

$$\sum_{i=1}^m\int_{z^{(i)}}Q_i(z^{(i)})\log\frac{p(x^{(i)},z^{(i)};\mu,\Lambda,\Psi)}{Q_i(z^{(i)})}\mathrm{d}z^{(i)}$$

下面我们重点求$$\Lambda$$的估计公式。

首先将上式简化为:

$$\begin{align}
&\sum_{i=1}^m\int_{z^{(i)}}Q_i(z^{(i)})\log\frac{p(x^{(i)}\mid z^{(i)};\mu,\Lambda,\Psi)p(z^{(i)})}{Q_i(z^{(i)})}\mathrm{d}z^{(i)}
\\&=\sum_{i=1}^m\int_{z^{(i)}}Q_i(z^{(i)})\left[\log p(x^{(i)}\mid z^{(i)};\mu,\Lambda,\Psi)+\log p(z^{(i)})-\log Q_i(z^{(i)})\right]\mathrm{d}z^{(i)}
\\&=\sum_{i=1}^m E_{z^{(i)}\sim Q_i}\left[\log p(x^{(i)}\mid z^{(i)};\mu,\Lambda,\Psi)+\log p(z^{(i)})-\log Q_i(z^{(i)})\right]
\end{align}$$

去掉和各参数无关的部分后，可得：

$$\begin{align}
&\sum_{i=1}^mE\left[\log p(x^{(i)}\mid z^{(i)};\mu,\Lambda,\Psi)\right]
\\&=\sum_{i=1}^mE\left[\frac{1}{(2\pi)^{n/2}\lvert\Psi\rvert^{1/2}}\exp\left(-\frac{1}{2}(x^{(i)}-\mu-\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu-\Lambda z^{(i)})\right)\right]
\\&=\sum_{i=1}^mE\left[-\frac{1}{2}\log\lvert\Psi\rvert-\frac{n}{2}\log(2\pi)-\frac{1}{2}(x^{(i)}-\mu-\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu-\Lambda z^{(i)})\right]
\end{align}$$

去掉和$$\Lambda$$无关的部分，并求导可得：

$$\nabla_\Lambda\sum_{i=1}^m-E\left[\frac{1}{2}(x^{(i)}-\mu-\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu-\Lambda z^{(i)})\right]\tag{2}$$

因为公式2中$$E[\cdot]$$部分的结果实际上是个实数，因此该公式可变形为：

$$\nabla_\Lambda\sum_{i=1}^m-E\left[\operatorname{tr}\left(\frac{1}{2}(x^{(i)}-\mu-\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu-\Lambda z^{(i)})\right)\right]$$

而：

$$\begin{align}
&\frac{1}{2}(x^{(i)}-\mu-\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu-\Lambda z^{(i)})
\\&=\frac{1}{2}\left[((x^{(i)}-\mu)^T-(\Lambda z^{(i)})^T)\Psi^{-1}((x^{(i)}-\mu)-\Lambda z^{(i)})\right]
\\&=\frac{1}{2}\left[(x^{(i)}-\mu)^T\Psi^{-1}(x^{(i)}-\mu)-(x^{(i)}-\mu)^T\Psi^{-1}\Lambda z^{(i)}
\\-(\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu)+(\Lambda z^{(i)})^T\Psi^{-1}\Lambda z^{(i)}\right]
\end{align}$$

去掉和$$\Lambda$$无关的部分，可得：

$$\frac{1}{2}\left[(\Lambda z^{(i)})^T\Psi^{-1}\Lambda z^{(i)}-(x^{(i)}-\mu)^T\Psi^{-1}\Lambda z^{(i)}-(\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu)\right]$$

所以：

$$\begin{align}
&\nabla_\Lambda\sum_{i=1}^m-E\left[\operatorname{tr}\left(\frac{1}{2}\left[(\Lambda z^{(i)})^T\Psi^{-1}\Lambda z^{(i)}-(x^{(i)}-\mu)^T\Psi^{-1}\Lambda z^{(i)}-(\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu)\right]\right)\right]
\\&\begin{split}=\nabla_\Lambda\sum_{i=1}^m-E\left[\frac{1}{2}\operatorname{tr}\left((\Lambda z^{(i)})^T\Psi^{-1}\Lambda z^{(i)}\right)-\frac{1}{2}\operatorname{tr}\left((x^{(i)}-\mu)^T\Psi^{-1}\Lambda z^{(i)}\right)
\\-\frac{1}{2}\operatorname{tr}\left((\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu)\right)\right]\end{split}
\\&=\sum_{i=1}^m\nabla_\Lambda E\left[-\frac{1}{2}\operatorname{tr}\left(\Lambda^T \Psi^{-1}\Lambda z^{(i)}(z^{(i)})^T\right)+\operatorname{tr}\left(\Lambda^T \Psi^{-1}(x^{(i)}-\mu)(z^{(i)})^T\right)\right]\tag{3}
\end{align}$$

因为：

$$\nabla_A\operatorname{tr}ABA^TC=CAB+C^TAB^T$$

所以：

$$\begin{align}
&\nabla_A\operatorname{tr}\left(\Lambda^T \Psi^{-1}\Lambda z^{(i)}(z^{(i)})^T\right)=z^{(i)}(z^{(i)})^T\Lambda^T \Psi^{-1}+(z^{(i)}(z^{(i)})^T)^T\Lambda^T(\Psi^{-1})^T
\\&=z^{(i)}(z^{(i)})^T\Lambda^T \Psi^{-1}+((z^{(i)})^T)^T(z^{(i)})^T\Lambda^T\Psi^{-1}=2z^{(i)}(z^{(i)})^T\Lambda^T \Psi^{-1}
\end{align}$$

代入公式3，可得：

$$\sum_{i=1}^mE\left[-\Psi^{-1}\Lambda z^{(i)}(z^{(i)})^T+\Psi^{-1}(x^{(i)}-\mu)(z^{(i)})^T\right]$$

由上式等于0，可得：

$$\sum_{i=1}^m\Lambda E_{z^{(i)}\sim Q_i}\left[z^{(i)}(z^{(i)})^T\right]=\sum_{i=1}^m(x^{(i)}-\mu)E_{z^{(i)}\sim Q_i}\left[(z^{(i)})^T\right]$$

因此：

$$\Lambda=\left(\sum_{i=1}^m(x^{(i)}-\mu)E_{z^{(i)}\sim Q_i}\left[(z^{(i)})^T\right]\right)\left(\sum_{i=1}^m E_{z^{(i)}\sim Q_i}\left[z^{(i)}(z^{(i)})^T\right]\right)^{-1}\tag{4}$$

因为：

$$\operatorname{Cov}(Y)=E[YY^T]-E[Y]E[Y]^T$$

所以：

$$E[YY^T]=E[Y]E[Y]^T+\operatorname{Cov}(Y)$$

因此根据之前的讨论可得：

$$E_{z^{(i)}\sim Q_i}\left[(z^{(i)})^T\right]=\mu_{z^{(i)}\mid x^{(i)}}^T$$

$$E_{z^{(i)}\sim Q_i}\left[z^{(i)}(z^{(i)})^T\right]=\mu_{z^{(i)}\mid x^{(i)}}\mu_{z^{(i)}\mid x^{(i)}}^T+\Sigma_{z^{(i)}\mid x^{(i)}}$$

将上式代入公式4，可得：

$$\Lambda=\left(\sum_{i=1}^m(x^{(i)}-\mu)\mu_{z^{(i)}\mid x^{(i)}}^T\right)\left(\sum_{i=1}^m \left(\mu_{z^{(i)}\mid x^{(i)}}\mu_{z^{(i)}\mid x^{(i)}}^T+\Sigma_{z^{(i)}\mid x^{(i)}}\right)\right)^{-1}$$

这里需要注意的是，和之前的混合高斯模型相比，我们不仅要计算$$\Sigma_{z^{(i)}\mid x^{(i)}}$$，还要计算$$E[z]$$和$$E[zz^T]$$。

此外，我们还可得出：（推导过程略）

$$\mu=\frac{1}{m}\sum_{i=1}^mx^{(i)}$$

$$\begin{split}\Phi=\frac{1}{m}\sum_{i=1}^m\left(x^{(i)}(x^{(i)})^T-x^{(i)}\mu_{z^{(i)}\mid x^{(i)}}^T\Lambda^T-\Lambda\mu_{z^{(i)}\mid x^{(i)}}(x^{(i)})^T
\\+\Lambda\left(\mu_{z^{(i)}\mid x^{(i)}}\mu_{z^{(i)}\mid x^{(i)}}^T+\Sigma_{z^{(i)}\mid x^{(i)}}\right)\Lambda^T\right)\end{split}$$

# 机器学习中的矩阵方法

在继续Andrew Ng的讲义之前，我们需要加强一些矩阵的相关知识。虽然Andrew Ng的讲义中已经包含了一个线性代数方面的简介文章，然而真的就只是简介而已，好多内容都没有。

这里推荐一本书《Matrix Methods in Data Mining and Pattern Recognition》。

>作者：Lars Eld´en，执教于Linköping University数学系。

http://www.cnblogs.com/daniel-D/p/3204508.html

这是daniel-D写的中文笔记。

这一部分的内容属于数值计算领域，涉及的概念虽然不复杂，但提出一个高效算法，仍然不是件容易的事情。

还有另外一本书《Liner Algebra Done Right》，也值得推荐。这本书从定义矩阵算子，而不是通过行列式，来解释各种线性代数原理，提供了一种独特的视角。因为算子是有明确的几何或物理意义的，而行列式则不然。

>作者：Sheldon Jay Axler，1949年生，美国数学家。普林斯顿大学本科，UCB博士，MIT博士后，San Francisco State University教授。美国的数学系基本就是本科和博士，很少有硕士。因为数学，尤其是理论数学，需要高度的抽象思维能力，半调子的硕士，既不好找工作，也不好搞科研。

《Linear Algebra Done Wrong》，这是布朗大学的Sergei Treil教授的著作，不知道书名是否有恶搞前书的意味...

该书电子版：

https://www.math.brown.edu/~treil/papers/LADW/LADW.html

## 三角矩阵的求逆问题

$$\begin{bmatrix}
l_{11} & 0 & 0 \\
l_{21} & l_{22} & 0 \\
l_{31} & l_{32} & l_{33} \\  
\end{bmatrix}
\begin{bmatrix}
u_{11} & u_{12} & u_{13} \\
0 & u_{22} & u_{23} \\
0 & 0 & u_{33} \\  
\end{bmatrix}
$$

以3阶方阵为例，上面左边的矩阵被称为下三角矩阵（lower triangular matrix），而右边的矩阵被称为上三角矩阵（upper triangular matrix）。

对于矩阵求逆问题来说，下三角矩阵是一类比较简单的矩阵，求逆难度仅高于对角阵。

下三角矩阵的逆矩阵也是下三角矩阵，因此：

$$AA^{-1}=\begin{bmatrix}
a_{11} & 0 & \dots & 0 \\
a_{21} & a_{22} & \dots & 0 \\
\dots & \dots & \dots & \dots \\
a_{n1} & a_{n2} & \dots & a_{nn} \\  
\end{bmatrix}
\begin{bmatrix}
b_{11} & 0 & \dots & 0 \\
b_{21} & b_{22} & \dots & 0 \\
\dots & \dots & \dots & \dots \\
b_{n1} & b_{n2} & \dots & b_{nn} \\  
\end{bmatrix}
=\begin{bmatrix}
1 & 0 & \dots & 0 \\
0 & 1 & \dots & 0 \\
\dots & \dots & \dots & \dots \\
0 & 0 & \dots & 1 \\  
\end{bmatrix}
$$

由矩阵乘法定义，可得：

$$c_{ij}=\sum_{k=j}^ia_{ik}b_{kj}$$

由$$c_{ij}=1,i=j$$，可得：$$b_{ii}=\frac{1}{a_{ii}}$$

由$$c_{ij}=0,i\neq j$$，可得：

$$c_{ij}=\sum_{k=j}^{i-1}a_{ik}b_{kj}+a_{ii}b_{ij}=0$$

因此：

$$b_{ij}=-\frac{1}{a_{ii}}\sum_{k=j}^{i-1}a_{ik}b_{kj}=-b_{ii}\sum_{k=j}^{i-1}a_{ik}b_{kj}$$

上三角矩阵求逆，可通过转置转换成下三角矩阵求逆。这里会用到以下性质:

$$(A^T)^{-1}=(A^{-1})^T$$

## LU分解

LU分解可将矩阵A分解为$$A=LU$$，其中L是下三角矩阵，U是上三角矩阵。

LU分解的用途很多，其中之一是求逆：

$$A^{-1}=(LU)^{-1}=U^{-1}L^{-1}$$

LU分解有若干种算法，常见的包括Doolittle、Cholesky、Crout算法。

>注：Myrick Hascall Doolittlee，1830~1913。

>Andr´e-Louis Cholesky，1875~1918，法国数学家、工程师、军官。死于一战战场。Cholesky分解法又称平方根法，是当A为实对称正定矩阵时，LU三角分解法的变形。

>Prescott Durand Crout，1907~1984，美国数学家，22岁获MIT博士。

这里只介绍一下Doolittle算法。

$$A=\begin{bmatrix}
a_{11} & a_{12} & \dots & a_{1n} \\
a_{21} & a_{22} & \dots & a_{2n} \\
\dots & \dots & \dots & \dots \\
a_{n1} & a_{n2} & \dots & a_{nn} \\  
\end{bmatrix}=LU=
\begin{bmatrix}
1 & 0 & \dots & 0 \\
l_{21} & 1 & \dots & 0 \\
\dots & \dots & \dots & \dots \\
l_{n1} & l_{n2} & \dots & 1 \\  
\end{bmatrix}
\begin{bmatrix}
u_{11} & u_{12} & \dots & u_{1n} \\
0 & u_{22} & \dots & u_{2n} \\
\dots & \dots & \dots & \dots \\
0 & 0 & \dots & u_{nn} \\  
\end{bmatrix}
$$

由矩阵乘法定义，可知：

$$a_{1j}=u_{1j},j=1,2,\dots,n$$

$$a_{ij}=\begin{cases}
\sum_{t=1}^jl_{it}u_{tj}, & j<i \\
\sum_{t=1}^{i-1}l_{it}u_{tj}+u_{ij}, & j\ge i \\
\end{cases}$$

因此：

>$$u_{1j}=a_{1j},j=1,2,\dots,n$$（U的第1行）   
>$$l_{j1}=a_{j1}/u_{11},j=1,2,\dots,n$$（L的第1列）   
>For $$i=2,3,\dots,n$$ do   
>>$$u_{ii}=a_{ii}-\sum_{t=1}^{i-1}l_{it}u_{tj}$$   
>>$$u_{ij}=a_{ij}-\sum_{t=1}^{i-1}l_{it}u_{tj}$$,for $$j=i+1,\dots,n$$（U的第i行）   
>>$$l_{ji}=\frac{a_{ji}-\sum_{t=1}^{i-1}l_{jt}u_{ti}}{u_{ii}}$$,for $$j=i+1,\dots,n$$（L的第i列）   
>
>End   
>$$u_{nn}=a_{nn}-\sum_{t=1}^{n-1}l_{nt}u_{tn}$$

参见：

http://www3.nd.edu/~zxu2/acms40390F11/Alg-LU-Crout.pdf

