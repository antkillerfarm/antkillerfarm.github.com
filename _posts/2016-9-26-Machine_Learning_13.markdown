---
layout: post
title:  机器学习（十三）——机器学习中的矩阵方法（3）病态矩阵、协同过滤的ALS算法
category: technology 
---

## 病态矩阵（续）

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

欧氏距离：

$$d(x,y)=\sqrt{\sum(x_i-y_i)^2},sim(x,y)=\frac{1}{1+d(x,y)}$$

Cosine相似度：

$$\cos(x,y)=\frac{\langle x,y\rangle}{|x||y|}=\frac{\sum x_iy_i}{\sqrt{\sum x_i^2}~\sqrt{\sum y_i^2}}$$

皮尔逊相关系数（Pearson product-moment correlation coefficient，PPMCC or PCC）：

$$\begin{align}
p(x,y)&=\frac{cov(X,Y)}{\sigma_X\sigma_Y}=\frac{\operatorname{E}[XY]-\operatorname{E}[X]\operatorname{E}[Y]}{\sqrt{\operatorname{E}[X^2]-\operatorname{E}[X]^2}~\sqrt{\operatorname{E}[Y^2]- \operatorname{E}[Y]^2}}
\\&=\frac{n\sum x_iy_i-\sum x_i\sum y_i}{\sqrt{n\sum x_i^2-(\sum x_i)^2}~\sqrt{n\sum y_i^2-(\sum y_i)^2}}
\end{align}$$

该系数由Karl Pearson发明。参见《机器学习（二）》中对Karl Pearson的简介。Fisher对该系数也有研究和贡献。

![](/images/article/pearson.png)

如上图所示，Cosine相似度计算的是两个样本点和坐标原点之间的直线的夹角，而PCC计算的是两个样本点和数学期望点之间的直线的夹角。

PCC能够有效解决，在协同过滤数据集中，不同用户评分尺度不一的问题。

参见：

https://en.wikipedia.org/wiki/Pearson_product-moment_correlation_coefficient

Tanimoto系数：

$$T(x,y)=\frac{|X\cap Y|}{|X\cup Y|}=\frac{|X\cap Y|}{|X|+|Y|-|X\cap Y|}=\frac{\sum x_iy_i}{\sqrt{\sum x_i^2}+\sqrt{\sum y_i^2}-\sum x_iy_i}$$

该系数由Taffee T. Tanimoto于1960年提出。Tanimoto生平不详，从名字来看，应该是个日本人。在其他领域，它还有另一个名字Jaccard similarity coefficient。（两者的系数公式一致，但距离公式略有差异。）

>Paul Jaccard，1868～1944，苏黎世联邦理工学院（ETH Zurich）博士，苏黎世联邦理工学院植物学教授。ETH Zurich可是出了24个诺贝尔奖得主的。

参见：

https://en.wikipedia.org/wiki/Jaccard_index

## ALS算法原理

http://www.cnblogs.com/luchen927/archive/2012/02/01/2325360.html

上面的网页概括了ALS算法出现之前的协同过滤算法的概况。

ALS算法是2008年以来，用的比较多的协同过滤算法。它已经集成到Spark的Mllib库中，使用起来比较方便。

从协同过滤的分类来说，ALS算法属于User-Item CF，也叫做混合CF。它同时考虑了User和Item两个方面。

用户和商品的关系，可以抽象为如下的三元组：<User,Item,Rating>。其中，Rating是用户对商品的评分，表征用户对该商品的喜好程度。

假设我们有一批用户数据，其中包含m个User和n个Item，则我们定义Rating矩阵$$R_{m\times n}$$，其中的元素$$r_{ui}$$表示第u个User对第i个Item的评分。

在实际使用中，由于n和m的数量都十分巨大，因此R矩阵的规模很容易就会突破1亿项。这时候，传统的矩阵分解方法对于这么大的数据量已经是很难处理了。

另一方面，一个用户也不可能给所有商品评分，因此，R矩阵注定是个稀疏矩阵。矩阵中所缺失的评分，又叫做missing item。

![](/images/article/ALS.png)

针对这样的特点，我们可以假设用户和商品之间存在若干关联维度（比如用户年龄、性别、受教育程度和商品的外观、价格等），我们只需要将R矩阵投射到这些维度上即可。这个投射的数学表示是：

$$R_{m\times n}\approx X_{m\times k}Y_{n\times k}^T\tag{1}$$

这里的$$\approx$$表明这个投射只是一个近似的空间变换。

一般情况下，k的值远小于n和m的值，从而达到了数据降维的目的。

![](/images/article/ALS_2.png)

幸运的是，我们并不需要显式的定义这些关联维度，而只需要假定它们存在即可，因此这里的关联维度又被称为Latent factor。k的典型取值一般是20～200。

这种方法被称为概率矩阵分解算法(probabilistic matrix factorization，PMF)。ALS算法是PMF在数值计算方面的应用。

为了使低秩矩阵X和Y尽可能地逼近R，需要最小化下面的平方误差损失函数：

$$\min_{x_*,y_*}\sum_{u,i\text{ is known}}(r_{ui}-x_u^Ty_i)^2$$

考虑到矩阵的稳定性问题，使用Tikhonov regularization，则上式变为：

$$\min_{x_*,y_*}L(X,Y)=\min_{x_*,y_*}\sum_{u,i\text{ is known}}(r_{ui}-x_u^Ty_i)^2+\lambda(|x_u|^2+|y_i|^2)\tag{1}$$

优化上式，得到训练结果矩阵$$X_{m\times k},Y_{n\times k}$$。预测时，将User和Item代入$$r_{ui}=x_u^Ty_i$$，即可得到相应的评分预测值。

![](/images/article/ALS_3.png)

![](/images/article/ALS_4.png)

ALS算法的缺点在于：

1.它是一个离线算法。

2.无法准确评估新加入的用户或商品。这个问题也被称为Cold Start问题。

## ALS算法优化过程的推导

公式1的直接优化是很困难的，因为X和Y的二元导数并不容易计算，这时可以使用类似坐标下降法的算法，固定其他维度，而只优化其中一个维度。

对$$x_u$$求导，可得：

$$\begin{align}
\frac{\partial L}{\partial x_u}&=-2\sum_i(r_{ui}-x_u^Ty_i)y_i+2\lambda x_u
\\&=-2\sum_i(r_{ui}-y_i^Tx_u)y_i+2\lambda x_u
\\&=-2Y^Tr_u+2Y^TYx_u+2\lambda x_u
\end{align}$$

