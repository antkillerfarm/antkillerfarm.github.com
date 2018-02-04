---
layout: post
title:  机器学习（十三）——机器学习中的矩阵方法（2）QR分解, 特征值和奇异值
category: ML 
---

## QR分解（续）

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
><span style="white-space: pre">	</span>1.$$A_k=Q_kR_k$$（QR分解）   
><span style="white-space: pre">	</span>2.$$A_{k+1}=Q_k^TA_kQ_k=Q_k^{-1}Q_kR_kQ_k=R_kQ_k$$   
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

>注：John G.F. Francis，1934年生，英国计算机科学家，剑桥大学肄业生。   
>2000年，QR算法被IEEE计算机学会评为20世纪的top 10算法之一。然而直到那时，计算机界的数学家们竟然都没有见过Francis本尊，连这位大神是活着还是死了都不知道，仿佛他在发表完这篇惊世之作后就消失了一般。   
>2007年，学界的两位大牛：Gene Howard Golub（SVD算法发明人之一，后文会提到。）和Frank Detlev Uhlig（1972年获加州理工学院博士，Auburn University数学系教授），经过不懈努力和人肉搜索终于联系上了他。   
>他一点都不知道自己N年前的研究被引用膜拜了无数次，得知自己的QR算法是二十世纪最NB的十大算法还有点小吃惊。这位神秘大牛竟然连TeX和Matlab都不知道。现在这位大牛73岁了，活到老学到老，还在远程教育大学Open University里补修当年没有修到的学位。   
>2015年，University of Sussex授予他荣誉博士学位。   
>相关内容参见：   
>http://www.netlib.org/na-digest-html/07/v07n34.html

>Vera Nikolaevna Kublanovskaya，1920~2012，苏联数学家，女。终身供职于苏联科学院列宁格勒斯塔克罗夫数学研究所。52岁才拿到博士学位。

需要指出的是，QR算法可求出矩阵的所有特征值，如果只求某一个特征值的话，还有其他一些更快的算法。详见：

https://en.wikipedia.org/wiki/Eigenvalue_algorithm

## 矩阵的奇异值

在进一步讨论之前，我们首先介绍一下矩阵特征值的几何意义。

首先，矩阵是对线性变换的表示，确定了定义域空间V与目标空间W的两组基，就可以很自然地得到该线性变换的矩阵表示。

线性空间变换的几何含义如下图所示：

![](/images/article/svd_2.png)

图中的坐标轴，就是线性空间的**基**。

线性变换主要有三种几何效果：**旋转、缩放、投影**。

其中，旋转和缩放不改变向量的维数。矩阵特征值运算，实际上就是将向量V旋转缩放到一个正交基W上。因为V和W等维，所以要求矩阵必须是方阵。

正交化过程，代表旋转变换，又被称为**等距同构**。（旋转变换，可以理解为向量的正向旋转，也可以理解为坐标轴的反向旋转，这里理解为后者，会容易一些。）**特征值代表缩放变换的缩放因子。**

而对于一般矩阵而言，我们还需要进行投影变换，将n维向量V映射为m维向量W。那么投影变换选择什么矩阵呢？

我们知道，对于复数z，可写成：

$$z=\left(\frac{z}{\mid z\mid }\right)\mid z\mid =\left(\frac{z}{\mid z\mid }\right)\sqrt{\overline z z}$$

其中$$\overline z$$是z的共轭复数。也就是说，一个复数可以表示为一个单位向量乘以一个模。

类似的，我们定义共轭矩阵$$M^*_{ij}=\overline{M_{ji}}$$，这实际上就是矩阵M转置之后，再将每个元素值设为它的共轭复数。因此：

$$M^*=(\overline M)^T=\overline{M^T}$$

仿照着复数的写法，矩阵M可以表示为：$$M=S\sqrt{M^*M}$$

这里的S表示等距同构。（单位向量相当于给模一个旋转变换，也就是等距同构。）由于$$\sqrt{M^*M}$$是正定对称方阵，因此它实际上也是能够被正交化的。所以对于一般矩阵来说，我们总能够找到两个正交基，并在这两个基之间进行投影变换。

>注意：我们刚才是用与复数类比的方式，得到投影变换矩阵$$\sqrt{M^*M}$$。但是类比不能代替严格的数学证明。幸运的是，上述结论已经被严格证明了。

我们将矩阵$$\sqrt{M^*M}$$的特征值，称作奇异值（Singular value）。可以看出，如果M是对称方阵的话，则M的奇异值等于M的特征值的绝对值。

参见：

https://www.zhihu.com/question/22237507/answer/53804902

http://www.ams.org/samplings/feature-column/fcarc-svd

## 奇异值分解

奇异值分解（Singular value decomposition，SVD）定理：

设$$M\in R^{m\times n}$$，则必存在正交矩阵$$U=[u_1,\dots,u_m]\in R^{m\times m}$$和$$V=[v_1,\dots,v_n]\in R^{n\times n}$$使得：

$$U^TMV=\begin{bmatrix}
\Sigma_r & 0 \\
0 & 0 
\end{bmatrix}$$

其中，$$\Sigma_r=diag(\sigma_1,\dots,\sigma_r),\sigma_1\ge \dots\ge \sigma_r>0$$。

当M为复矩阵时，将U、V改为酉矩阵（unitary matrix）即可。（吐槽一下，酉矩阵这个翻译真的好烂，和天干地支半毛钱关系都没有。）

奇异值分解也可写为另一种形式：

$$M=U\Sigma V^*$$

其几何意义如下图所示：

![](/images/article/Singular-Value-Decomposition.png)

虽然，我们可以通过计算矩阵$$\sqrt{M^*M}$$的特征值的方法，计算奇异值，然而这个方法的计算量十分巨大。1965年，Gene Howard Golub和William Morton Kahan发明了目前较为通用的算法。但该方法比较复杂，这里不作介绍。

>Gene Howard Golub，1932～2007，美国数学家，斯坦福大学教授。

>William Morton Kahan，1933年生，加拿大数学家，多伦多大学博士，UCB教授。图灵奖获得者（1989）。IEEE-754标准（即浮点数标准）的主要制订者，被称为“浮点数之父”。ACM院士。

参见：

http://www.doc88.com/p-089411326888.html

SVD(奇异值分解)算法及其评估

https://mp.weixin.qq.com/s/46oOYoL486WZ4oPwgLrrrQ

奇异值分解SVD原理与应用详解

https://mp.weixin.qq.com/s/1pg8jY1R-8kJKu1L_RPLkg

奇异值分解(SVD)原理

## 矩阵的秩

一个矩阵A的列（行）秩是A的线性独立的列（行）的极大数。

下面不加证明的给出矩阵的秩的性质：

1.矩阵的行秩等于列秩，因此可统称为矩阵的秩。

2.秩是n的$$m\times n$$矩阵为列满秩阵；秩是n的$$n\times p$$矩阵为行满秩阵。

3.设$$A\in M_{m\times n}(F)$$，若A是行满秩阵，则$$m\le n$$；若A是列满秩阵 ，则$$n\le m$$。

4.设A为$$m\times n$$列满秩阵，则n元齐次线性方程组$$AX=0$$只有零解。

5.线性方程组$$AX=B$$对任一m维列向量B都有解$$\Leftrightarrow$$系数矩阵A为行满秩阵。

参见：

http://wenku.baidu.com/view/9ce143eb81c758f5f61f6730.html

## 奇异矩阵

对应的行列式等于0的方阵，被称为奇异矩阵（singular matrix）。

奇异矩阵和线性相关、秩等概念密切相关。

下面不加证明的给出奇异矩阵的性质：

1.如果A为非奇异矩阵$$\Leftrightarrow$$A满秩。

2.如果A为奇异矩阵，则AX=0有无穷解，AX=b有无穷解或者无解。如果A为非奇异矩阵，则AX=0有且只有唯一零解，AX=b有唯一解。

对于A不是方阵的情况，一般使用$$A^TA$$来评估矩阵是否是奇异矩阵。


