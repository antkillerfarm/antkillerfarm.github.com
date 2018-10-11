---
layout: post
title:  机器学习（十三）——机器学习中的矩阵方法（1）LU分解, QR分解
category: ML 
---

# 机器学习中的矩阵方法（续）

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

## 矩阵的特征值和特征向量

设A是一个n阶方阵，$$\lambda$$是一个数，如果方程$$Ax=\lambda x$$存在非零解向量，则称$$\lambda$$为A的一个特征值（Eigenvalue），相应的非零解向量x称为属于特征值$$\lambda$$的特征向量（eigenvector）。

上面这个描述也可以记作：

$$(A-\lambda I)x=0\tag{2}$$

这个公式本身通常用于：已知特征值，求解对应的特征向量。

其中，$$A-\lambda I$$被称为特征矩阵，而$$\lvert A-\lambda I \rvert=0$$被称为特征方程。求解特征方程可得到特征值。

特征值和特征向量在有的书上也被称为本征值和本征向量。

特征值和特征向量的特性包括：

1.特征向量属于特定的特征值，离开特征值讨论特征向量是没有意义的。不同特征值对应的特征向量不会相等，但特征向量不能由特征值唯一确定。

2.在复数范围内，n阶矩阵A有n个特征值。在这些特征值中，模最大的那个特征值即主特征值（对于实数阵即绝对值最大的特征值），主特征值对应的特征向量称为主特征向量。

更多内容参见：

http://course.tjau.edu.cn/xianxingdaishu/jiao/5.htm

矩阵的特征值和特征向量

https://mp.weixin.qq.com/s/mZ4AeCcoU0LhWRWfa9_kvw

花了10分钟，终于弄懂了特征值和特征向量到底有什么意义

https://mp.weixin.qq.com/s/uL5fCNTegK9ST2LOO13-4g

图说幂法求特征值和特征向量

## QR算法

对矩阵A进行QR分解可得：$$A=QR$$

因为Q是正交阵（$$Q^T=Q^{-1}$$），所以正交相似变换$$Q^TAQ$$和A有相同的特征值。

证明：

$$\mid Q^TAQ-\lambda I\mid =\mid Q^TAQ-Q^T(\lambda I)Q\mid =\mid Q^T(A-\lambda I)Q\mid \\=\mid Q^T\mid \cdot\mid A-\lambda I\mid \cdot\mid Q\mid =\mid Q^TQ\mid \cdot\mid A-\lambda I\mid =\mid I\mid \cdot\mid A-\lambda I\mid =\mid A-\lambda I\mid $$

这里的证明，用到了行列式的如下性质：

$$\mid I\mid =1$$

$$\mid AB\mid =\mid A\mid \cdot\mid B\mid $$

因为$$Q^TAQ$$和A的特征方程相同，所以它们的特征值也相同。证毕。

由此产生如下迭代算法：

>Repeat until convergence {   
>>1.$$A_k=Q_kR_k$$（QR分解）   
>>2.$$A_{k+1}=Q_k^TA_kQ_k=Q_k^{-1}Q_kR_kQ_k=R_kQ_k$$   
>
>}

这个算法的收敛性证明比较复杂，这里只给出结论：

$$\lim_{k\to\infty}A_k=\begin{bmatrix}
\lambda_1 & u_{12} &  \dots  & u_{1n} \\
0 & \lambda_2 &  \dots  & u_{2n} \\
\dots & \dots & \ddots & \dots \\
0 & 0 & \dots & \lambda_n 
\end{bmatrix}$$

其中，$$\lambda_i$$为矩阵的特征值。$$u_{ij}$$表示任意值，它们的极限可能并不存在。

QR算法于1961年，由John G.F. Francis和Vera Nikolaevna Kublanovskaya发现。

