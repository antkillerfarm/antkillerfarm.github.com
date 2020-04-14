---
layout: post
title:  机器学习（十）——高斯混合模型和EM算法
category: ML 
---

# K-Means算法（续）

由于畸变函数J是非凸函数，意味着我们不能保证算法取得的最小值是全局最小值，也就是说k-means对质心初始位置的选取比较敏感。但一般情况下k-means达到的局部最优已经满足需求。但如果你怕陷入局部最优，那么可以选取不同的初始值跑多遍k-means，然后取其中最小的J对应的$$\mu$$和c输出。

参考：

http://www.csdn.net/article/2012-07-03/2807073-k-means

深入浅出K-Means算法

http://www.cnblogs.com/leoo2sk/archive/2010/09/20/k-means.html

k均值聚类(K-means)

http://www.cnblogs.com/jerrylead/archive/2011/04/06/2006910.html

K-means聚类算法

https://mp.weixin.qq.com/s/86MlCMKAG0ax3gvfsgmYtg

K-Means聚类算法详解

https://mp.weixin.qq.com/s/Sl3lTS1zvq0BgAayAUcOXg

K-Means实战与调优详解

https://mp.weixin.qq.com/s/6ErpBtVg0r2dhsBGbIkvDg

Bisecting K-Means算法

https://mp.weixin.qq.com/s/oAvNzxENTfaxUkSRypCo1g

K-Means算法的10个有趣用例

https://zhuanlan.zhihu.com/p/45408671

K-Means小谈

## 聚类结果的评价

可考虑用以下几个指标来评价聚类效果：

1.聚类中心之间的距离。距离值大，通常可考虑分为不同类。

2.聚类域中的样本数目。样本数目少且聚类中心距离远，可考虑是否为噪声。

3.聚类域内样本的距离方差。方差过大的样本可考虑是否属于这一类。

## Elbow Method

K-means算法中，K值的选取除了依赖领域知识之外，还可以采用Elbow Method（肘部原则）来确定。

![](/images/img2/elbow.png)

上图中的蓝线是畸变率，数值越低越好，而绿线是计算时间，也是数值越低越好。可以看出K=8的时候，效果最佳。实际上即使只有一根线，我们也可以判断K取值的优劣：折线的斜率在K=8处，变化较大，形如人的肘部，故名。

参考：

https://www.scikit-yb.org/en/latest/api/cluster/elbow.html

Elbow Method

https://bl.ocks.org/rpgove/0060ff3b656618e9136b

Using the elbow method to determine the optimal number of clusters for k-means clustering

# 高斯混合模型和EM算法

本篇讨论使用期望最大化算法（Expectation-Maximization）进行密度估计（density estimation）。

从上面的讨论可以形象的看出，聚类问题实际就是在数据集上，找出一个个数据密度较高的“圆圈”。我们可以反过来思考这个问题：如果我们已知圆圈的圆心和半径，那么也可以根据圆心、半径以及样本分布模型，来随机生成这些数据。显然，这和前一种做法在效果上是等效的。

首先我们假设样本数据满足联合概率分布

$$p(x^{(i)},z^{(i)})=p(x^{(i)}\mid z^{(i)})p(z^{(i)})\tag{5}$$

其中，$$z^{(i)}\sim Multinomial(\phi)$$（这里的$$\phi_j=p(z^{(i)}=j)$$，因此$$\phi_j\ge 0,\sum_{j=1}^k\phi_j=1$$），$$z^{(i)}$$的值为k个聚类之一。

假定$$x^{(i)}\mid z^{(i)}=j\sim \mathcal{N}(\mu_j,\Sigma_j)$$，则该模型被称为高斯混合模型（mixture of Gaussians model）。

整个模型简单描述为对于每个样例$$x^{(i)}$$，我们先从k个类别中按多项分布抽取一个$$z^{(i)}$$，然后根据$$z^{(i)}$$所对应的k个多值高斯分布中的一个生成样例$$x^{(i)}$$。注意的是这里的$$z^{(i)}$$是隐含的随机变量（latent random variables）。

![](/images/article/EM.png)

因此，由全概率公式可得：

$$p(x^{(i)};\phi,\mu,\Sigma)=\sum_{z^{(i)}=1}^kp(x^{(i)}\mid z^{(i)};\mu,\Sigma)p(z^{(i)};\phi)\tag{6}$$

该模型的对数化似然函数为：

$$\ell(\phi,\mu,\Sigma)=\sum_{i=1}^m\log p(x^{(i)};\phi,\mu,\Sigma)=\sum_{i=1}^m\log \sum_{z^{(i)}=1}^kp(x^{(i)}\mid z^{(i)};\mu,\Sigma)p(z^{(i)};\phi)$$

这个式子的最大值不能通过求导数为0的方法解决的，因为它不是close form。（多项分布的概率密度函数包含阶乘运算，不满足close form的定义。）

为了简化问题，我们假设已经知道每个样例的$$z^{(i)}$$值。这实际上就转化成《机器学习（二）》所提到的GDA模型。和之前模型的区别在于，$$z^{(i)}$$是多项分布，而且每个聚类的$$\Sigma$$都不相同，但结论是类似的。

这里的直接推导非常复杂，可参考以下文章：

http://www.cse.psu.edu/~rtc12/CSE586/lectures/EMLectureFeb3.pdf

上面这篇文章比较直观，比Andrew讲义的Problem Set详细的多。然而Andrew这样写是有原因的，在后面的章节，借助Jensen不等式，Andrew给出一个更简单且一般化的推导过程。

接下来的问题就是：$$z^{(i)}$$的值我们是不知道的，该怎么办呢？

EM算法的思路是：

>1.猜测$$z^{(i)}$$的值。（这一步即所谓的Expectation，简称E-Step。）   
>2.最大化计算，以更新模型的参数。（这一步即所谓的Maximization，简称M-Step。）

具体到这里就是：

>Repeat until convergence {   
>>(E-step) For each i, j：   
>>>$$w_j^{(i)}:=p(z^{(i)}=j\mid x^{(i)};\phi,\mu,\Sigma)$$   
>>
>>(M-step) Update the parameters：   
>>>$$\phi_j:=\frac{1}{m}\sum_{i=1}^mw_j^{(i)}$$   
>>>$$\mu_j:=\frac{\sum_{i=1}^mw_j^{(i)}x^{(i)}}{\sum_{i=1}^mw_j^{(i)}}$$   
>>>$$\Sigma_j:=\frac{\sum_{i=1}^mw_j^{(i)}(x^{(i)}-\mu_j)(x^{(i)}-\mu_j)^T}{\sum_{i=1}^mw_j^{(i)}}$$   
>
>}

E-Step中，根据贝叶斯公式可得：

$$p(z^{(i)}=j\mid x^{(i)};\phi,\mu,\Sigma)=\frac{p(x^{(i)}\mid z^{(i)}=j;\mu,\Sigma)p(z^{(i)}=j;\phi)}{\sum_{l=1}^kp(x^{(i)}\mid z^{(i)}=l;\mu,\Sigma)p(z^{(i)}=l;\phi)}$$

将假设模型的各概率密度函数代入上式，即可计算得到$$w_j^{(i)}$$。

相比于K-means算法，GMM算法中的$$z^{(i)}$$是个概率值，而非确定值，因此也被称为soft assignments。

K-means算法各个聚类的特征都是一样的，也就是“圆圈”的半径一致。而GMM算法的“圆圈”半径可以不同。如下面两图所示：

| ![](/images/article/EM_2.png) | ![](/images/article/EM_3.png) |
| K-means算法 | GMM算法 |

>注意：这里的圆圈是先验估计值，它和最后的聚类形状无关。

GMM算法结果也是局部最优解。对其他参数取不同的初始值进行多次计算同样适用于GMM算法。

![](/images/img2/Clustering.jpg)

![](/images/img2/Clustering_2.jpg)

## GMM与概率分布

Andrew讲义在介绍GMM的时候，由于要和K-means做对比，因此主要介绍了GMM在聚类中的应用，但GMM的应用并不仅于此。

我们知道函数有泰勒级数和傅立叶级数等展开方式，同样的**任意随机变量的概率分布，也可以展开为若干高斯分布的和**：

$$p(x)=\sum_{m=1}^M c_m \mathcal{N}(x;\mu_m,\sigma_m^2)$$

不难看出上式和上节的公式5是等价的。

GMM的这种用法在语音识别领域用的较多，是GMM-HMM算法的核心思想之一。

## 参考

http://cseweb.ucsd.edu/~atsmith/project1_253.pdf

Clustering With EM and K-Means

https://mp.weixin.qq.com/s/wk3_wG1xSuMX1HjJeEgErQ

kmeans聚类理论篇K的选择（轮廓系数）

https://mp.weixin.qq.com/s/tLcF7_jjl3_FmjM7L1kcfw

一文详解高斯混合模型原理

https://mp.weixin.qq.com/s/RpEgTMRUM4GWhIdIPvkGTg

优于VAE，为万能近似器高斯混合模型加入Wasserstein距离

https://zhuanlan.zhihu.com/p/85338773

高斯混合模型（GMM）推导及实现

# EM算法

本节将进一步讨论EM算法的性质，并将之应用到使用latent random variables的一大类估计问题中。

<a name="Jensen"/>

## Jensen不等式

首先我们给出凸函数的定义：

设f是定义域为实数的函数，如果对于所有的实数x，$$f''(x)\ge 0$$，那么f是凸函数。当x是向量时，如果其Hessian矩阵H是半正定的（$$H\ge 0$$），那么f是凸函数。如果$$f''(x)>0$$或$$H>0$$，那么称f是严格凸函数。

Jensen不等式表述如下:

如果f是凸函数，X是随机变量，那么$$E[f(X)]\ge f(EX)$$。

特别地，如果f是严格凸函数，那么$$E[f(X)]=f(EX)$$，当且仅当$$p(X=EX)=1$$。也就是说X是常量。

在不引起误会的情况下，$$E[X]$$简写做$$EX$$。

这个不等式的含义如下图所示：

![](/images/article/Jensen.png)

Jensen不等式应用于凹函数时，不等号方向反向，也就是$$E[f(X)]\le f(EX)$$

>注：Johan Ludwig William Valdemar Jensen，1859～1925，丹麦人。主业工程师，副业数学家。

## EM算法的一般形式

EM算法一般形式的似然函数为：

$$\ell(\theta)=\sum_{i=1}^m\log p(x^{(i)};\theta)=\sum_{i=1}^m\log \sum_zp(x^{(i)},z^{(i)};\theta)\tag{1}$$

根据这个公式直接求$$\theta$$一般比较困难，但是如果确定了z之后，求解就容易了。

EM算法是一种解决存在隐含变量优化问题的有效方法。既然不能直接最大化$$\ell(\theta)$$，我们可以不断地建立$$\ell(\theta)$$的下界(E-Step),然后优化下界(M-Step)。

这里首先假设$$z^{(i)}$$的分布为$$Q_i$$。显然：

$$\sum_zQ_i(z)=1,Q_i(z)\ge 0$$

$$\begin{align}
\ell(\theta)&=\sum_{i=1}^m\log \sum_{z^{(i)}}p(x^{(i)},z^{(i)};\theta)
\\&=\sum_{i=1}^m\log \sum_{z^{(i)}}Q_i(z^{(i)})\frac{p(x^{(i)},z^{(i)};\theta)}{Q_i(z^{(i)})}
\\&\ge\sum_{i=1}^m\sum_{z^{(i)}}Q_i(z^{(i)})\log\frac{p(x^{(i)},z^{(i)};\theta)}{Q_i(z^{(i)})}\tag{2}
\end{align}$$

这里解释一下最后一步的不等式变换。

首先，根据数学期望的定义公式：

$$E[X]=\sum_{i=1}^nx_ip_i$$

可知：

$$\sum_{z^{(i)}}Q_i(z^{(i)})\frac{p(x^{(i)},z^{(i)};\theta)}{Q_i(z^{(i)})}=E\left[\frac{p(x^{(i)},z^{(i)};\theta)}{Q_i(z^{(i)})}\right]$$

又因为$$f(x)=\log x$$是凹函数，根据Jensen不等式可得：

$$f\left(E_{z^{(i)}\sim Q_i}\left[\frac{p(x^{(i)},z^{(i)};\theta)}{Q_i(z^{(i)})}\right]\right)\ge E_{z^{(i)}\sim Q_i}\left[f\left(\frac{p(x^{(i)},z^{(i)};\theta)}{Q_i(z^{(i)})}\right)\right]$$

综上，公式2给出了$$\ell(\theta)$$的下界。对于$$Q_i$$的选择，有多种可能，那种更好的？

假设$$\theta$$已经给定，那么$$\ell(\theta)$$的值就取决于$$Q_i(z^{(i)})$$和$$p(x^{(i)},z^{(i)})$$了。我们可以通过调整这两个概率，使下界不断上升，以逼近$$\ell(\theta)$$的真实值。当不等式变成等式时，下界达到最大值。

由Jensen不等式相等的条件可知，$$\ell(\theta)$$的下界达到最大值的条件为：

$$\frac{p(x^{(i)},z^{(i)};\theta)}{Q_i(z^{(i)})}=c$$

其中c为常数。

这实际上表明：

$$Q_i(z^{(i)})\propto p(x^{(i)},z^{(i)};\theta)$$

其中的$$\propto$$符号是两者成正比例的意思。
