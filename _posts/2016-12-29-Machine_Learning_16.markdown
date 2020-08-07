---
layout: post
title:  机器学习（十六）——协同过滤的ALS算法
category: ML 
---

* toc
{:toc}

# 协同过滤的ALS算法

## 协同过滤概述

>注：最近研究商品推荐系统的算法，因此，Andrew Ng讲义的内容，后续再写。

协同过滤是目前很多电商、社交网站的用户推荐系统的算法基础，也是目前工业界应用最广泛的机器学习领域。

协同过滤是利用集体智慧的一个典型方法。要理解什么是协同过滤 (Collaborative Filtering,简称CF)，首先想一个简单的问题，如果你现在想看个电影，但你不知道具体看哪部，你会怎么做？大部分的人会问问周围的朋友，看看最近有什么好看的电影推荐，而我们一般更倾向于从口味比较类似的朋友那里得到推荐。这就是协同过滤的核心思想。

如何找到相似的用户和物品呢？其实就是计算用户间以及物品间的相似度。以下是几种计算相似度的方法：

### 欧氏距离

$$d(x,y)=\sqrt{\sum(x_i-y_i)^2},sim(x,y)=\frac{1}{1+d(x,y)}$$

### Cosine相似度

$$\cos(x,y)=\frac{\langle x,y\rangle}{\mid x\mid \mid y\mid }=\frac{\sum x_iy_i}{\sqrt{\sum x_i^2}~\sqrt{\sum y_i^2}}$$

### 皮尔逊相关系数（Pearson product-moment correlation coefficient，PPMCC or PCC）：

$$\begin{align}
p(x,y)&=\frac{cov(X,Y)}{\sigma_X\sigma_Y}=\frac{\operatorname{E}[XY]-\operatorname{E}[X]\operatorname{E}[Y]}{\sqrt{\operatorname{E}[X^2]-\operatorname{E}[X]^2}~\sqrt{\operatorname{E}[Y^2]- \operatorname{E}[Y]^2}}
\\&=\frac{n\sum x_iy_i-\sum x_i\sum y_i}{\sqrt{n\sum x_i^2-(\sum x_i)^2}~\sqrt{n\sum y_i^2-(\sum y_i)^2}}
\end{align}$$

该系数由Karl Pearson发明。参见[《机器学习（二）》](/ml/2016/08/02/Machine_Learning_2.html#Pearson)中对Karl Pearson的简介。Fisher对该系数也有研究和贡献。

![](/images/article/pearson.png)

如上图所示，Cosine相似度计算的是两个样本点和坐标原点之间的直线的夹角，而PCC计算的是两个样本点和数学期望点之间的直线的夹角。

PCC能够有效解决，在协同过滤数据集中，不同用户评分尺度不一的问题。

参见：

https://en.wikipedia.org/wiki/Pearson_product-moment_correlation_coefficient

https://mp.weixin.qq.com/s/RjpH7XD5SCMkrSdcmG394g

从PCC到MIC，一文教你如何计算变量之间的相关性

### Spearman秩相关系数（Spearman's rank correlation coefficient）

对秩变量（ranked variables）套用PCC公式，即可得Spearman秩相关系数。

秩变量是一类不在乎值的具体大小，而只关心值的大小关系的统计量。

| $$X_i$$ | $$Y_i$$ | $$x_i$$ | $$y_i$$ | $$d_i$$ | $$d_i^2$$ |
|:--:|:--:|:--:|
| 86 | 0 | 1 | 1 | 0 | 0 |
| 97 | 20 | 2 | 6 | -4 | 16 |
| 99 | 28 | 3 | 8 | -5 | 25 |
| 100 | 27 | 4 | 7 | -3 | 9 |
| 101 | 50 | 5 | 10 | -5 | 25 |
| 103 | 29 | 6 | 9 | -3 | 9 |
| 106 | 7 | 7 | 3 | 4 | 16 |
| 110 | 17 | 8 | 5 | 3 | 9 |
| 112 | 6 | 9 | 2 | 7 | 49 |
| 113 | 12 | 10 | 4 | 6 | 36 |

如上表所示，$$X_i$$和$$Y_i$$是原始的变量值，$$x_i$$和$$y_i$$是rank之后的值，$$d_i=x_i-y_i$$。

当$$X_i$$和$$Y_i$$没有重复值的时候，也可用如下公式计算相关系数：

$$r_s = {1- \frac {6 \sum d_i^2}{n(n^2 - 1)}}$$

>注：Charles Spearman，1863～1945，英国心理学家。这个人的经历比较独特，20岁从军，15年之后退役。然后，进入德国莱比锡大学读博，中间又被军队征召，参加了第二次布尔战争，因此，直到1906年才拿到博士学位。伦敦大学学院心理学教授。   
>尽管他的学历和教职，都是心理学方面的。但他最大的贡献，却是在统计学领域。他也是因为在统计学方面的成就，得以当选皇家学会会员。   
>话说那个时代的统计学大牛，除了Fisher之外，基本都是副业比主业强。只有Fisher，主业方面也是那么牛逼，不服不行啊。

![](/images/article/spearman.png)

由上图可见，Pearson系数关注的是两个变量之间的线性相关度，而Spearman系数可以应用到非线性或者难以量化的领域。

参见：

https://en.wikipedia.org/wiki/Spearman%27s_rank_correlation_coefficient

### Kendall秩相关系数（Kendall rank correlation coefficient）

对于秩变量对$$(x_i,y_i),(x_j,y_j)$$：

$$(x_i-x_j)(y_i-y_j)\begin{cases}
>0, & \text{concordant} \\
=0, & \text{neither concordant nor discordant} \\
<0, & \text{discordant} \\
\end{cases}$$

$$\tau = \frac{(\text{number of concordant pairs}) - (\text{number of discordant pairs})}{n (n-1) /2}$$

>注：Sir Maurice George Kendall，1907~1983，英国统计学家。这个人职业生涯的大部分时间都是一个公务员，二战期间出任英国船运协会副总经理。1949年以后担任伦敦大学教授。

参见：

https://en.wikipedia.org/wiki/Kendall_rank_correlation_coefficient

### Tanimoto系数

$$T(x,y)=\frac{\mid X\cap Y\mid }{\mid X\cup Y\mid }=\frac{\mid X\cap Y\mid }{\mid X\mid +\mid Y\mid -\mid X\cap Y\mid }=\frac{\sum x_iy_i}{\sqrt{\sum x_i^2}+\sqrt{\sum y_i^2}-\sum x_iy_i}$$

该系数由Taffee T. Tanimoto于1960年提出。Tanimoto生平不详，从名字来看，应该是个日本人。在其他领域，它还有另一个名字Jaccard similarity coefficient。（两者的系数公式一致，但距离公式略有差异。）

如果向量的每个维度取值是二值（0或1），那么Tanimoto系数就等同Jaccard距离。

>注：Paul Jaccard，1868～1944，苏黎世联邦理工学院（ETH Zurich）博士，苏黎世联邦理工学院植物学教授。ETH Zurich可是出了24个诺贝尔奖得主的。

参见：

https://en.wikipedia.org/wiki/Jaccard_index

https://www.cnblogs.com/daniel-D/p/3244718.html

漫谈：机器学习中距离和相似性度量方法

https://mp.weixin.qq.com/s/rrIdxEEwFJMWbvbhn_DYDw

协同过滤算法分布式实现

https://zhuanlan.zhihu.com/p/138107999

常见的距离算法和相似度计算方法

## ALS算法原理

http://www.cnblogs.com/luchen927/archive/2012/02/01/2325360.html

机器学习相关——协同过滤

https://mp.weixin.qq.com/s/Xgg61ICqkUo9ovdEV7G8Cw

推荐算法综述

https://mp.weixin.qq.com/s/lBj0Z9vY9FcEFQnk0VrdOw

协同过滤推荐算法

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

不懂这个空间变换的同学，可参见[《机器学习（十四）》](/ml/2016/09/28/Machine_Learning_14.html)中的“奇异值分解”的内容，或是[《机器学习（十七）》](/ml/2017/01/12/Machine_Learning_17.html#PCA)中的“主成分分析”的内容。

一般情况下，k的值远小于n和m的值，从而达到了数据降维的目的。

![](/images/article/ALS_2.png)

幸运的是，我们并不需要显式的定义这些关联维度，而只需要假定它们存在即可，因此这里的关联维度又被称为Latent factor。k的典型取值一般是20～200。

这种方法被称为概率矩阵分解算法(probabilistic matrix factorization，PMF)。ALS算法是PMF在数值计算方面的应用。

为了使低秩矩阵X和Y尽可能地逼近R，需要最小化下面的平方误差损失函数：

$$\min_{x_*,y_*}\sum_{u,i\text{ is known}}(r_{ui}-x_u^Ty_i)^2$$

考虑到矩阵的稳定性问题，使用Tikhonov regularization，则上式变为：

$$\min_{x_*,y_*}L(X,Y)=\min_{x_*,y_*}\sum_{u,i\text{ is known}}(r_{ui}-x_u^Ty_i)^2+\lambda(\mid x_u\mid ^2+\mid y_i\mid ^2)\tag{2}$$

优化上式，得到训练结果矩阵$$X_{m\times k},Y_{n\times k}$$。预测时，将User和Item代入$$r_{ui}=x_u^Ty_i$$，即可得到相应的评分预测值。

![](/images/article/ALS_3.png)

同时，矩阵X和Y，还可以用于比较不同的User（或Item）之间的相似度，如下图所示：

![](/images/article/ALS_4.png)

ALS算法的缺点在于：

1.它是一个离线算法。

2.无法准确评估新加入的用户或商品。这个问题也被称为Cold Start问题。

## ALS算法优化过程的推导

公式2的直接优化是很困难的，因为X和Y的二元导数并不容易计算，这时可以使用类似坐标下降法的算法，固定其他维度，而只优化其中一个维度。

对$$x_u$$求导，可得：

$$\begin{align}
\frac{\partial L}{\partial x_u}&=-2\sum_i(r_{ui}-x_u^Ty_i)y_i+2\lambda x_u
\\&=-2\sum_i(r_{ui}-y_i^Tx_u)y_i+2\lambda x_u
\\&=-2Y^Tr_u+2Y^TYx_u+2\lambda x_u
\end{align}$$

令导数为0，可得：

$$Y^TYx_u+\lambda Ix_u=Y^Tr_u\Rightarrow x_u=(Y^TY+\lambda I)^{-1}Y^Tr_u\tag{3}$$

同理，对$$y_i$$求导，由于X和Y是对称的，因此可得类似的结论：

$$y_i=(X^TX+\lambda I)^{-1}X^Tr_i\tag{4}$$

因此整个优化迭代的过程为：

>1.随机生成X、Y。（相当于对迭代算法给出一个初始解。）   
>Repeat until convergence {   
>>2.固定Y，使用公式3更新$$x_u$$。    
>>3.固定X，使用公式4更新$$y_i$$。    
>
>}

一般使用RMSE（root-mean-square error）评估误差是否收敛，具体到这里就是：

$$RMSE=\sqrt{\frac{\sum(R-XY^T)^2}{N}}$$

其中，N为三元组<User,Item,Rating>的个数。当RMSE值变化很小时，就可以认为结果已经收敛。

算法复杂度：

1.求$$x_u$$：$$O(k^2N+k^3m)$$

2.求$$y_i$$：$$O(k^2N+k^3n)$$

可以看出当k一定的时候，这个算法的复杂度是**线性**的。

因为这个迭代过程，交替优化X和Y，因此又被称作交替最小二乘算法（Alternating Least Squares，ALS）。
