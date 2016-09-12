---
layout: post
title:  机器学习（十一）——机器学习中的矩阵方法
category: technology 
---

## 因子分析的EM估计（续）

因为《机器学习（十）》的公式4中$$E[\cdot]$$部分的结果实际上是个实数，因此该公式可变形为：

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
\\&=\sum_{i=1}^m\nabla_\Lambda E\left[-\frac{1}{2}\operatorname{tr}\left(\Lambda^T \Psi^{-1}\Lambda z^{(i)}(z^{(i)})^T\right)+\operatorname{tr}\left(\Lambda^T \Psi^{-1}(x^{(i)}-\mu)(z^{(i)})^T\right)\right]\tag{1}
\end{align}$$

因为：

$$\nabla_A\operatorname{tr}ABA^TC=CAB+C^TAB^T$$

所以：

$$\begin{align}
&\nabla_A\operatorname{tr}\left(\Lambda^T \Psi^{-1}\Lambda z^{(i)}(z^{(i)})^T\right)=z^{(i)}(z^{(i)})^T\Lambda^T \Psi^{-1}+(z^{(i)}(z^{(i)})^T)^T\Lambda^T(\Psi^{-1})^T
\\&=z^{(i)}(z^{(i)})^T\Lambda^T \Psi^{-1}+((z^{(i)})^T)^T(z^{(i)})^T\Lambda^T\Psi^{-1}=2z^{(i)}(z^{(i)})^T\Lambda^T \Psi^{-1}
\end{align}$$

代入公式1，可得：

$$\sum_{i=1}^mE\left[-\Psi^{-1}\Lambda z^{(i)}(z^{(i)})^T+\Psi^{-1}(x^{(i)}-\mu)(z^{(i)})^T\right]$$

由上式等于0，可得：

$$\sum_{i=1}^m\Lambda E_{z^{(i)}\sim Q_i}\left[z^{(i)}(z^{(i)})^T\right]=\sum_{i=1}^m(x^{(i)}-\mu)E_{z^{(i)}\sim Q_i}\left[(z^{(i)})^T\right]$$

因此：

$$\Lambda=\left(\sum_{i=1}^m(x^{(i)}-\mu)E_{z^{(i)}\sim Q_i}\left[(z^{(i)})^T\right]\right)\left(\sum_{i=1}^m E_{z^{(i)}\sim Q_i}\left[z^{(i)}(z^{(i)})^T\right]\right)^{-1}\tag{2}$$

因为：

$$\operatorname{Cov}(Y)=E[YY^T]-E[Y]E[Y]^T$$

所以：

$$E[YY^T]=E[Y]E[Y]^T+\operatorname{Cov}(Y)$$

因此根据之前的讨论可得：

$$E_{z^{(i)}\sim Q_i}\left[(z^{(i)})^T\right]=\mu_{z^{(i)}\vert x^{(i)}}^T$$

$$E_{z^{(i)}\sim Q_i}\left[z^{(i)}(z^{(i)})^T\right]=\mu_{z^{(i)}\vert x^{(i)}}\mu_{z^{(i)}\vert x^{(i)}}^T+\Sigma_{z^{(i)}\vert x^{(i)}}$$

将上式代入公式2，可得：

$$\Lambda=\left(\sum_{i=1}^m(x^{(i)}-\mu)\mu_{z^{(i)}\vert x^{(i)}}^T\right)\left(\sum_{i=1}^m \left(\mu_{z^{(i)}\vert x^{(i)}}\mu_{z^{(i)}\vert x^{(i)}}^T+\Sigma_{z^{(i)}\vert x^{(i)}}\right)\right)^{-1}$$

这里需要注意的是，和之前的混合高斯模型相比，我们不仅要计算$$\Sigma_{z^{(i)}\vert x^{(i)}}$$，还要计算$$E[z]$$和$$E[zz^T]$$。

此外，我们还可得出：（推导过程略）

$$\mu=\frac{1}{m}\sum_{i=1}^mx^{(i)}$$

$$\begin{split}\Phi=\frac{1}{m}\sum_{i=1}^m\left(x^{(i)}(x^{(i)})^T-x^{(i)}\mu_{z^{(i)}\vert x^{(i)}}^T\Lambda^T-\Lambda\mu_{z^{(i)}\vert x^{(i)}}(x^{(i)})^T
\\+\Lambda\left(\mu_{z^{(i)}\vert x^{(i)}}\mu_{z^{(i)}\vert x^{(i)}}^T+\Sigma_{z^{(i)}\vert x^{(i)}}\right)\Lambda^T\right)\end{split}$$

# 机器学习中的矩阵方法

在继续Andrew Ng的讲义之前，我们需要加强一些矩阵的相关知识。虽然Andrew Ng的讲义中已经包含了一个线性代数方面的简介，然而真的就只是简介而已，好多内容都没有。

这里推荐一本书《Matrix Methods in Data Mining and Pattern Recognition》。作者：Lars Eld´en，执教于Linköping University数学系。

http://www.cnblogs.com/daniel-D/p/3204508.html

这是daniel-D写的中文笔记。

## LU分解



# 主成分分析

## 矩阵的特征值和特征向量

设A是一个n阶方阵，$$\lambda$$是一个数，如果方程$$AX=\lambda X$$存在非零解向量，则称$$\lambda$$为A的一个特征值，相应的非零解向量X称为属于特征值$$\lambda$$的特征向量。

特征值和特征向量的特性包括：

1.特征向量属于特定的特征值，离开特征值讨论特征向量是没有意义的。不同特征值对应的特征向量不会相等，但特征向量不能由特征值惟一确定。

2.在复数范围内，n阶矩阵A有n个特征值。

更多内容参见：

http://course.tjau.edu.cn/xianxingdaishu/jiao/5.htm

## QR算法

任意实数方阵A，都能被分解为$$A=QR$$。这里的Q为正交单位阵，即$$Q^TQ=I$$。R是一个上三角矩阵。

https://en.wikipedia.org/wiki/QR_algorithm

http://www.netlib.org/na-digest-html/07/v07n34.html

>注：

Vera Nikolaevna Kublanovskaya，1920~2012，苏联数学家，女。终身供职于苏联科学院列宁格勒斯塔克罗夫数学研究所。52岁才拿到博士学位。


