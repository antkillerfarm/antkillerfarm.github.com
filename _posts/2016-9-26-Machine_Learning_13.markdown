---
layout: post
title:  机器学习（十三）——机器学习中的矩阵方法（1）LU分解, QR分解
category: ML 
---

* toc
{:toc}

## 因子分析的EM估计（续）

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

---

https://mp.weixin.qq.com/s/4NRkrkV_M2b_IpnpCTn05w

一图胜千言，这本交互式线代教科书让你分分钟理解复杂概念，佐治亚理工出品

https://mp.weixin.qq.com/s/xWiRzBmK1JiGldMiq3BkTg

伯克利一份简明《机器学习数学基础》丝滑入门手册，47页pdf

---



《Matrix Decomposition and Applications》

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

## QR分解

任意实数方阵A，都能被分解为$$A=QR$$。这里的Q为正交单位阵，即$$Q^TQ=I$$。R是一个上三角矩阵。这种分解被称为QR分解。

QR分解也有若干种算法，常见的包括Gram–Schmidt、Householder和Givens算法。

>注：Jørgen Pedersen Gram，1850～1916，丹麦数学家，在矩阵、数论、泛函等领域皆有贡献。他居然是被自行车撞死的...

>Erhard Schmidt，1876～1959，德国数学家，哥廷根大学博士，柏林大学教授。David Hilbert的学生。20世纪数学界的几位超级大神之一。1933年前的哥廷根大学数学系，秒杀其他所有学校。所谓“一流的学生去哥廷根，智商欠费才去藤校”。

>Alston Scott Householder，1904～1993，美国数学家，芝加哥大学博士，田纳西大学教授。ACM主席。

>James Wallace Givens, Jr.，1910～1993，美国数学家，普林斯顿大学博士，西北大学教授。参与UNIVAC I机器项目（1951年），这是最早的商用计算机。

这里只介绍Gram–Schmidt算法，这个算法虽然名为Gram–Schmidt，然而拉普拉斯和柯西早就已经用过了。

首先介绍一下向量的投影运算的符号表示。

![](/images/article/Projection_and_rejection.png)

如上图所示，根据余弦定理和向量点乘的定义可得：

$$a\cdot b=\mid a\mid \mid b\mid \cos \theta$$

因此，向量a在向量b上的投影向量$$a_1$$，可表示为：

$$a_1=\mid a\mid \cos \theta\hat b=\mid a\mid \frac{a\cdot b}{\mid a\mid \mid b\mid }\frac{b}{\mid b\mid }=\frac{a\cdot b}{\mid b\mid ^2}b=\frac{a\cdot b}{b\cdot b}b=\frac{\langle a,b\rangle}{\langle b,b\rangle}b$$

特别的，当b为单位向量时：

$$a_1=\langle a,b\rangle\tag{1}$$

我们定义投影符号如下：

$$\mathrm{proj}_{\mathbf{e}}\mathbf{a}
= \frac{\left\langle\mathbf{e},\mathbf{a}\right\rangle}{\left\langle\mathbf{e},\mathbf{e}\right\rangle}\mathbf{e}$$

令$$A=[\mathbf{a}_1, \cdots, \mathbf{a}_n]$$，其中$$a_i$$为列向量。则：

$$\begin{align}
 \mathbf{u}_1 &= \mathbf{a}_1,
  & \mathbf{e}_1 &= {\mathbf{u}_1 \over \|\mathbf{u}_1\|} \\
 \mathbf{u}_2 &= \mathbf{a}_2-\mathrm{proj}_{\mathbf{u}_1}\,\mathbf{a}_2,
  & \mathbf{e}_2 &= {\mathbf{u}_2 \over \|\mathbf{u}_2\|} \\
 \mathbf{u}_3 &= \mathbf{a}_3-\mathrm{proj}_{\mathbf{u}_1}\,\mathbf{a}_3-\mathrm{proj}_{\mathbf{u}_2}\,\mathbf{a}_3,
  & \mathbf{e}_3 &= {\mathbf{u}_3 \over \|\mathbf{u}_3\|} \\
 & \vdots &&\vdots \\
 \mathbf{u}_k &= \mathbf{a}_k-\sum_{j=1}^{k-1}\mathrm{proj}_{\mathbf{u}_j}\,\mathbf{a}_k,
  &\mathbf{e}_k &= {\mathbf{u}_k\over\|\mathbf{u}_k\|}
\end{align}$$

即：

$$\begin{align}
 \mathbf{a}_1 &= \langle\mathbf{e}_1,\mathbf{a}_1 \rangle \mathbf{e}_1  \\
 \mathbf{a}_2 &= \langle\mathbf{e}_1,\mathbf{a}_2 \rangle \mathbf{e}_1
  + \langle\mathbf{e}_2,\mathbf{a}_2 \rangle \mathbf{e}_2 \\
 \mathbf{a}_3 &= \langle\mathbf{e}_1,\mathbf{a}_3 \rangle \mathbf{e}_1
  + \langle\mathbf{e}_2,\mathbf{a}_3 \rangle \mathbf{e}_2
  + \langle\mathbf{e}_3,\mathbf{a}_3 \rangle \mathbf{e}_3 \\
 &\vdots \\
 \mathbf{a}_k &= \sum_{j=1}^{k} \langle \mathbf{e}_j, \mathbf{a}_k \rangle \mathbf{e}_j
\end{align}$$

这个过程又被称为Gram–Schmidt正交化过程。

因此：

$$Q = \left[ \mathbf{e}_1, \cdots, \mathbf{e}_n\right] \qquad \text{and} \qquad
R = \begin{bmatrix}
\langle\mathbf{e}_1,\mathbf{a}_1\rangle & \langle\mathbf{e}_1,\mathbf{a}_2\rangle &  \langle\mathbf{e}_1,\mathbf{a}_3\rangle  & \ldots \\
0 & \langle\mathbf{e}_2,\mathbf{a}_2\rangle &  \langle\mathbf{e}_2,\mathbf{a}_3\rangle  & \ldots \\
0 & 0 & \langle\mathbf{e}_3,\mathbf{a}_3\rangle & \ldots \\
\vdots & \vdots & \vdots & \ddots \end{bmatrix}$$
