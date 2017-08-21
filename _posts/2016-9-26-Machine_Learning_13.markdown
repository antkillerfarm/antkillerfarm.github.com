---
layout: post
title:  机器学习（十三）——机器学习中的矩阵方法（2）特征值和奇异值, 病态矩阵
category: theory 
---

## QR算法（续）

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

$$z=\left(\frac{z}{|z|}\right)|z|=\left(\frac{z}{|z|}\right)\sqrt{\overline z z}$$

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

参见：

http://www.doc88.com/p-089411326888.html

>Gene Howard Golub，1932～2007，美国数学家，斯坦福大学教授。

>William Morton Kahan，1933年生，加拿大数学家，多伦多大学博士，UCB教授。图灵奖获得者（1989）。IEEE-754标准（即浮点数标准）的主要制订者，被称为“浮点数之父”。ACM院士。

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

## 正定矩阵

positive definite matrix的定义：

一个n阶的实对称矩阵M是正定的的条件是当且仅当对于所有的非零实系数向量z，都有$$z^TMz>0$$。

正定矩阵A的性质：

1.正定矩阵的任一主子矩阵也是正定矩阵。

2.A的特征值和各阶顺序主子式全为正。

3.若A为n阶正定矩阵，则A为n阶可逆矩阵。

类似的还可以定义负定矩阵、半正定矩阵（非负定矩阵）。

## 向量的范数

范数（norm，也叫模）的定义比较抽象，这里我们使用闵可夫斯基距离，进行一个示意性的介绍。

Minkowski distance的定义：

$$d(x,y)=\sqrt[\lambda]{\sum_{i=1}^{n}\lvert x_i-y_i\lvert^{\lambda}}$$

显然，当$$\lambda=2$$时，该距离为欧氏距离。当$$\lambda=1$$时，也被称为CityBlock Distance或Manhattan Distance（曼哈顿距离）。

这里的$$\lambda$$就是范数。

范数可用符号$$\|x\|_\lambda$$表示。常用的有：

$$\|x\|_1=|x_1|+\dots+|x_n|$$

$$\|x\|_2=\sqrt{x_1^2+\dots+x_n^2}$$

$$\|x\|_\infty=max(|x_1|,\dots,|x_n|)$$

这里不做解释的给出如下示意图：

![](/images/article/lp_ball.png)

其中，0范数表示向量中非0元素的个数。上图中的图形被称为$$l_p$$ ball。表征在同一范数条件下，具有相同距离的点的集合。

范数满足如下不等式：

$$\|A+B\|\le \|A\|+\|B\|(三角不等式)$$

向量范数推广可得到矩阵范数。某些矩阵范数满足如下公式：

$$\|A\cdot B\|\le \|A\|\cdot\|B\|$$

这种范数被称为相容范数。

>注：矩阵范数要比向量范数复杂的多，还包含一些不可以由向量范数来诱导的范数，如Frobenius范数。而且只有极少数矩阵范数，可由简单表达式来表达。这里篇幅有限，不再赘述。

## 病态矩阵

现在有线性系统$$Ax = b$$：

$$\begin{bmatrix} 400 & -201 \\-800 & 201 \end{bmatrix}\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}=\begin{bmatrix} 200 \\ -200 \end{bmatrix}$$

很容易得到解为：$$x_1=-100,x_2=-200$$。如果在样本采集时存在一个微小的误差，比如，将 A矩阵的系数400改变成401：

$$\begin{bmatrix} 401 & -201 \\-800 & 201 \end{bmatrix}\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}=\begin{bmatrix} 200 \\ -200 \end{bmatrix}$$

则得到一个截然不同的解：$$x_1=40000,x_2=79800$$。

当解集x对A和b的系数高度敏感，那么这样的方程组就是病态的 (ill-conditioned/ill-posed)。

从上例的情况来看，矩阵的行向量$$\begin{bmatrix} 400 & -201\end{bmatrix}$$和$$\begin{bmatrix} -800 & 401\end{bmatrix}$$实际上是过于线性相关了，从而导致矩阵已经接近奇异矩阵（near singular matrix）。

病态矩阵实际上就是奇异矩阵和近奇异矩阵的另一个说法。

参见：

http://www.cnblogs.com/daniel-D/p/3219802.html

## 矩阵的条件数

我们首先假设向量b受到扰动，导致解集x产生偏差，即：

$$A(x+\Delta x)=b+\Delta b$$

也就是：

$$A\Delta x=\Delta b$$

因此，由矩阵相容性可得：

$$\|\Delta x\|\le \|A^{-1}\|\cdot\|\Delta b\|$$

同时，由于：

$$\|A\|\cdot\|x\|\ge\|b\|$$

所以：

$$\frac{\|\Delta x\|}{\|A\|\cdot\|x\|}\le \frac{\|A^{-1}\|\cdot\|\Delta b\|}{\|b\|}$$

即：

$$\frac{\|\Delta x\|}{\|x\|}\le \frac{\|A\|\cdot\|A^{-1}\|\cdot\|\Delta b\|}{\|b\|}$$

我们定义矩阵的条件数$$K(A)=\|A\|\cdot\|A^{-1}\|$$，则上式可写为：

$$\frac{\|\Delta x\|}{\|x\|}\le K(A)\frac{\|\Delta b\|}{\|b\|}$$

同样的，我们针对A的扰动，所导致的x的偏差，也可得到类似的结论：

$$\frac{\|\Delta x\|}{\|x+\Delta x\|}\le K(A)\frac{\|\Delta A\|}{\|A\|}$$

可见，矩阵的条件数是描述输入扰动对输出结果影响的量度。显然，条件数越大，矩阵越病态。

然而这个定义，在病态矩阵的条件下，并不能直接用于数值计算。因为浮点数所引入的微小的量化误差，也会导致求逆结果的很大误差。所以通常情况下，一般使用矩阵的特征值或奇异值来计算条件数。

假设A是2阶方阵，它有两个单位特征向量$$x_1,x_2$$和相应的特征值$$\lambda_1,\lambda_2$$。

由之前的讨论可知，$$x_1,x_2$$是相互正交的。因此，向量b能够被$$x_1,x_2$$的线性组合所表示，即：

$$b=mx_1+nx_2=\frac{m}{\lambda_1}\lambda_1x_1+\frac{n}{\lambda_2}\lambda_2x_2=A(\frac{m}{\lambda_1}x_1+\frac{n}{\lambda_2}x_2)$$

从这里可以看出，b在$$x_1,x_2$$上的扰动，所带来的影响，和特征值$$\lambda_1,\lambda_2$$有很密切的关系。奇异值实际上也有类似的特点。

因此，一般情况下，条件数也可以由最大奇异值与最小奇异值之间的比值，或者最大特征值和最小特征值之间的比值来表示。这里的最大和最小，都是针对绝对值而言的。

参见：

https://en.wikipedia.org/wiki/Condition_number

## 矩阵规则化

病态矩阵处理方法有很多，这里只介绍矩阵规则化（regularization）方法。

机器学习领域，经常用到各种损失函数（loss function）。这里我们用：

$$\min_f \sum_{i=1}^nV(f(\hat x_i),\hat y_i)$$

表示损失函数。

当样本数远小于特征向量维数时，损失函数所表示的矩阵是一个稀疏矩阵，而且往往还是一个病态矩阵。这时，就需要引入规则化因子用以改善损失函数的稳定性：

$$\min_f \sum_{i=1}^nV(f(\hat x_i),\hat y_i)+\lambda R(f)$$

其中的$$\lambda$$表示规则化因子的权重。


