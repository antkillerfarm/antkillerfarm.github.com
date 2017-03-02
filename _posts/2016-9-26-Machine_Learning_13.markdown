---
layout: post
title:  机器学习（十三）——机器学习中的矩阵方法（3）病态矩阵, 协同过滤的ALS算法（1）
category: theory 
---

## 奇异值分解（续）

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

机器学习领域，经常用到各种损失函数（loss function），也称花费函数（cost function）。这里我们用：

$$\min_f \sum_{i=1}^nV(f(\hat x_i),\hat y_i)$$

表示损失函数。

当样本数远小于特征向量维数时，损失函数所表示的矩阵是一个稀疏矩阵，而且往往还是一个病态矩阵。这时，就需要引入规则化因子用以改善损失函数的稳定性：

$$\min_f \sum_{i=1}^nV(f(\hat x_i),\hat y_i)+\lambda R(f)$$

其中的$$\lambda$$表示规则化因子的权重。

>注：稀疏矩阵并不一定是病态矩阵，比如单位阵就不是病态的。但是从系统论的角度，高维空间中样本量的稀疏，的确会带来很大的不确定性。

函数V（又叫做Fit measure）和R（又叫做Entropy measure），在不同的算法中，有不同的取值。

比如，在Ridge regression问题中：

$$\text{Fit measure}:\|Y-X\beta\|_2,\text{Entropy measure}:\|\beta\|_2$$

Ridge regression问题中规则化方法，又被称为$$L_2$$ regularization，或Tikhonov regularization。

>注：Andrey Nikolayevich Tikhonov，1906~1993，苏联数学家和地球物理学家。大地电磁学的发明人之一。苏联科学院院士。著有《Solutions of Ill-posed problems》一书。

更多的V和R取值参见：

https://en.wikipedia.org/wiki/Regularization_(mathematics)

从形式上来看，对比之前提到的拉格朗日函数，我们可以发现规则化因子，实际上就是给损失函数增加了一个约束条件。它的好处是增加了解向量的稳定度，缺点是增加了数值解和真实解之间的误差。

为了更便于理解规则化，这里以二维向量空间为例，给出了规则化因子对损失函数的约束效应。

![](/images/article/L1_vs_L2.png)

上图中的圆圈是损失函数的等高线，坐标原点是规则化因子的约束中心，左图的方形和右图的圆形是$$l_p$$ ball。图中的黑点是等高线和$$l_p$$ ball的焦点，实际上也就是这个带约束的优化问题的解。

可以看出$$L_1$$ regularization的解一般出现在坐标轴上，因而其他坐标上的值就是0，因此，$$L_1$$ regularization会导致矩阵的稀疏。

$$L_1$$ regularization又被称为Lasso（least absolute shrinkage and selection operator） regression。

参见：

https://en.wikipedia.org/wiki/Tikhonov_regularization

http://www.mit.edu/~cuongng/Site/Publication_files/Tikhonov06.pdf

http://blog.csdn.net/zouxy09/article/details/24971995

# 协同过滤的ALS算法

## 协同过滤概述

>注：最近研究商品推荐系统的算法，因此，Andrew Ng讲义的内容，后续再写。

协同过滤是目前很多电商、社交网站的用户推荐系统的算法基础，也是目前工业界应用最广泛的机器学习领域。

协同过滤是利用集体智慧的一个典型方法。要理解什么是协同过滤 (Collaborative Filtering,简称CF)，首先想一个简单的问题，如果你现在想看个电影，但你不知道具体看哪部，你会怎么做？大部分的人会问问周围的朋友，看看最近有什么好看的电影推荐，而我们一般更倾向于从口味比较类似的朋友那里得到推荐。这就是协同过滤的核心思想。

如何找到相似的用户和物品呢？其实就是计算用户间以及物品间的相似度。以下是几种计算相似度的方法：

### 欧氏距离

$$d(x,y)=\sqrt{\sum(x_i-y_i)^2},sim(x,y)=\frac{1}{1+d(x,y)}$$

### Cosine相似度

$$\cos(x,y)=\frac{\langle x,y\rangle}{|x||y|}=\frac{\sum x_iy_i}{\sqrt{\sum x_i^2}~\sqrt{\sum y_i^2}}$$

### 皮尔逊相关系数（Pearson product-moment correlation coefficient，PPMCC or PCC）：

$$\begin{align}
p(x,y)&=\frac{cov(X,Y)}{\sigma_X\sigma_Y}=\frac{\operatorname{E}[XY]-\operatorname{E}[X]\operatorname{E}[Y]}{\sqrt{\operatorname{E}[X^2]-\operatorname{E}[X]^2}~\sqrt{\operatorname{E}[Y^2]- \operatorname{E}[Y]^2}}
\\&=\frac{n\sum x_iy_i-\sum x_i\sum y_i}{\sqrt{n\sum x_i^2-(\sum x_i)^2}~\sqrt{n\sum y_i^2-(\sum y_i)^2}}
\end{align}$$
