---
layout: post
title:  机器学习（九）——K-Means算法
category: ML 
---

## 特征选择（续）

>1.初始化特征集$$\mathcal{F}=\emptyset$$。   
>2.Repeat {   
>>(a)for 特征i=1 to n, {   
>>>如果$$i\notin\mathcal{F}$$，则$$\mathcal{F}_i=\mathcal{F}\cup\{i\}$$。   
>>>在$$\mathcal{F}_i$$上使用交叉验证方法评估它的泛化误差。   
>>   
>>}   
>>(b)将第(a)步中最优的$$\mathcal{F}_i$$设为新的$$\mathcal{F}$$。   
>
>}   
>3.选择并输出搜索过程中得到的最优子集。

这个算法被称为前向搜索（forward search）。其外部循环的终止条件为$$\lvert\mathcal{F}\rvert$$达到n或者事先设定的门限值。

前向搜索属于wrapper model特征选择方法的一种。 Wrapper这里指不断地使用不同的特征子集来测试学习的算法。

除了前向搜索之外，还有后向搜索（backward search）算法。它和前者的区别在于，它的初始集合为全集，然后每次删除一个特征，并评价，直到$$\lvert\mathcal{F}\rvert$$达到阈值或者为空，然后选择最佳的$$\mathcal{F}$$即可。

可以看出无论前向搜索，还是后向搜索，其算法复杂度都是$$O(n^2)$$。

参考：

https://mp.weixin.qq.com/s/lbku7wQK1k8nuC2QqIJrug

基于非负谱学习和稀疏回归的双图特征选择算法

https://mp.weixin.qq.com/s/6JberaY4H7it_UQiBls8wg

为你介绍7种流行的线性回归收缩与选择方法

https://mp.weixin.qq.com/s/7u0Jf5HR4N6G-6Sol3PyVA

一款功能强大的特征选择工具（Feature Selector）

## KL散度

KL散度（Kullback–Leibler divergence）是两个随机分布间距离的度量。其定义如下：

$$D_{KL}(P\|Q)=\sum_iP(i)\log\frac{P(i)}{Q(i)}$$

其中，P和Q是离散概率分布，$$P(i)$$和$$Q(i)$$是相应分布的概率密度函数。如果P和Q是连续随机变量的话，将上式中的累加符号换成积分符号即可。

**KL散度越小，两个分布之间的匹配就越好。**

但KL散度并不是真正的度量（metric）。它既不满足三角不等式(两边之和$$\ge$$第三边)，也不满足对称性（即$$D_{KL}(P\|Q)\neq D_{KL}(Q\|P)$$）。

>注：Solomon Kullback，1907～1994，美国数学家和密码学家。乔治·华盛顿大学博士。NSA首任首席科学家。二战期间，参与破解德国的Enigma机器。

>Richard Leibler，1914～2003，美国数学家和密码学家。伊利诺伊大学博士。NSA高级主管，入选NSA名人堂。

参考：

https://mp.weixin.qq.com/s/e7YQfG578vWxApD-4gsWIg

如何理解KL散度的不对称性

https://mp.weixin.qq.com/s/PPz5iKY5Kb0vGqlfHvCJHA

初学机器学习：直观解读KL散度的数学概念

https://www.zhihu.com/question/345907033

KL散度衡量的是两个概率分布的距离吗？

## 过滤特征选择方法

过滤特征选择（Filter feature selection）方法，是另一种启发式的特征选择算法，计算量比较小。它的思路是计算特征$$x_i$$和类别标签y之间的相关度的评分$$S(i)$$。

可以使用$$x_i$$和y之间的互信息量（mutual information），作为评分依据。

$$MI(x_i,y)=\sum_{x_i\in X_i}\sum_{y\in Y}p(x_i,y)\log\frac{p(x_i,y)}{p(x_i)p(y)}$$

其中，$$p(x_i,y)$$是$$x_i$$和y的联合概率密度，$$p(x_i)$$和$$p(y)$$是相应的边缘概率密度。

和KL散度类似，如果x和y是连续随机变量的话，将上式中的累加符号换成积分符号即可。

MI也可以用KL散度来表示：

$$MI(x_i,y)=KL(p(x_i,y)\|p(x_i)p(y))$$

过滤特征选择方法的算法复杂度为$$O(n)$$。

最后一个问题，选择多少个特征合适呢？按照$$S(i)$$从高到低的顺序，依次选择1到n个特征进行交叉验证，直到效果达到预期为止。

## 交叉验证与融合

前面提到的交叉验证的过程，一般如下图所示：

![](/images/img2/Cross_validation.png)

模型融合能大幅提升准确率，是打比赛的“杀器”。把交叉验证训练的n个模型直接求一下平均，就是最简单的模型融合方案。

![](/images/img2/Cross_validation_2.png)

假如有m种不同的模型，每种模型做n划分交叉验证，可以得到mn个不同的模型，通过一个新模型来融合这mn个模型。

![](/images/img2/Cross_validation_3.png)

## 贝叶斯统计和规则化ML

前面提到最大似然（maximum likelihood）估计方法的公式如下：

$$\theta_{ML}=\arg\max_\theta\prod_{i=1}^mp(y^{(i)}\mid x^{(i)};\theta)$$

从频率统计（frequentist statistic）学派的观点来看，这里的$$\theta$$是一个未知的常数，我们的任务就是求出这个常数。然而从贝叶斯学派的观点来看，$$\theta$$是一个未知的随机变量。

也就是说似然函数，对于前者来说，是这样的：$$\prod_{i=1}^mp(y^{(i)}\mid x^{(i)};\theta)$$；但对于后者来说，却是这样的：$$\prod_{i=1}^mp(y^{(i)}\mid x^{(i)},\theta)$$

我们首先假定$$\theta$$的分布为$$p(\theta)$$，这种假定由于没有事实根据，通常被称作先验分布（prior distribution）。

我们针对训练集$$S=\{(x^{(i)},y^{(i)})\}_{i=1}^m$$，训练得到预测函数。并按照如下公式计算后验分布（posterior distribution）：

$$p(\theta\mid S)=\frac{p(S\mid\theta)p(\theta)}{p(S)}\tag{1}$$

由全概率公式可得：

$$p(S)=p(S\mid\theta_1)p(\theta_1)+\dots+p(S\mid\theta_n)p(\theta_n)$$

上式的$$\theta_i$$表示$$\theta$$的各个取值区间，然而由于$$\theta$$是连续随机变量，根据微积分原理可得：

$$p(S)=\int_\theta p(S\mid\theta)p(\theta)\mathrm{d}\theta\tag{2}$$

将公式2代入公式1可得：

$$p(\theta\mid S)=\frac{p(S\mid\theta)p(\theta)}{\int_\theta p(S\mid\theta)p(\theta)\mathrm{d}\theta}=\frac{\left(\prod_{i=1}^mp(y^{(i)}\mid x^{(i)},\theta)\right)p(\theta)}{\int_\theta\left(\prod_{i=1}^mp(y^{(i)}\mid x^{(i)},\theta)\right)p(\theta)\mathrm{d}\theta}$$

当我们针对新的样本x进行预测时，和上面的推导类似，可得：

$$p(y\mid x,S)=\int_\theta p(y\mid x,\theta,S)p(\theta\mid S)\mathrm{d}\theta$$

因为预测样本集和训练样本集的分布是独立的，因此上式又可写为：

$$p(y\mid x,S)=\int_\theta p(y\mid x,\theta)p(\theta\mid S)\mathrm{d}\theta$$

这个公式又被称作后验预测分布（Posterior predictive distribution）。

$$p(\theta\mid S)$$可由前面的公式得到。

假若我们要求期望值的话，那么套用求期望的公式即可：

$$E[y\mid x,S]=\int_y yp(y\mid x,S)\mathrm{d}y$$

由上可见，贝叶斯估计将$$\theta$$视为随机变量，$$\theta$$的值满足一定的分布，不是固定值，我们无法通过计算获得其值，只能在预测时计算积分。

上述贝叶斯估计方法，虽然公式合理优美，但后验概率$$p(\theta\mid S)$$通常是很难计算的，因为它是$$\theta$$上的高维积分函数。

观察$$p(\theta\mid S)$$的公式，在分母$$P(S)$$一定的情况下，分子越大则值越大，也就是$$p(\theta\mid S)$$的概率越大。

因此，可得如下算法：

$$\theta_{MAP}=\arg\max_\theta\left(\prod_{i=1}^mp(y^{(i)}\mid x^{(i)},\theta)\right)p(\theta)$$

这个算法叫做最大后验概率估计（maximum a posteriori）。

和ML相比，MAP算法只是在最后多了一项$$p(\theta)$$。通常使用中，我们认为$$p(\theta)$$符合$$\theta\sim N(0,\tau^2I)$$。

由于$$p(\theta)$$实际上就是先验分布，它会对最后结果进行一定的修正。因此，实际上最大后验概率估计相对于最大似然估计来说，较容易克服过度拟合的问题。

![](/images/article/MAP.png)

参见：

http://www.cs.cornell.edu/courses/cs5540/2010sp/lectures/Lec9.Estimation-continued.pdf

Statistical Estimation: Least Squares, Maximum Likelihood and Maximum A Posteriori Estimators

https://mp.weixin.qq.com/s/XnsbCb7H9jHmJ4dV9p2Oug

频率学派还是贝叶斯学派？聊一聊机器学习中的MLE和MAP

https://mp.weixin.qq.com/s/dQxN46wEbFrpvV369uOHdA

详解最大后验概率估计（MAP）的理解

# K-Means算法

聚类算法属于无监督学习算法的一种。它的训练样本中只有$$x^{(i)}$$，而没有$$y^{(i)}$$。聚类的目的是找到每个样本x潜在的类别y，并将同类别y的样本x放在一起，形成一个聚类（clusters）。样本$$x^{(i)}$$所属的聚类用$$c^{(i)}$$表示。

K-Means算法的步骤如下:

>1.随机选取k个聚类质心点（cluster centroids）$$\mu_1,\dots,\mu_k$$   
>2.重复下面过程直到收敛 {   
>>对于每一个样例i，计算其应该属于的聚类：$$c^{(i)}:=\arg\min_j\|x^{(i)}-\mu_j\|^2$$   
>>对于每一个聚类j，重新计算该聚类的质心：$$\mu_j:=\frac{\sum_{i=1}^m1\{c^{(i)}=j\}x^{(i)}}{\sum_{i=1}^m1\{c^{(i)}=j\}}$$。   
>
>}

其中，k是我们事先定义的聚类个数。下图展示了对n个样本点进行K-means聚类的效果，这里k取2。

![](/images/article/k-means.png)

K-means算法面对的第一个问题是如何保证收敛。前面的算法中强调结束条件就是收敛，可以证明的是K-means完全可以保证收敛性。下面我们定性的描述一下收敛性，我们定义畸变函数（distortion function）如下：

$$J(c,\mu)=\sum_{i=1}^m\|x^{(i)}-\mu_{c^{(i)}}\|^2$$

J函数表示每个样本点到其质心的距离平方和。K-means算法的目的是要将J调整到最小。假设当前J没有达到最小值，那么首先可以固定每个类的质心$$\mu_j$$，调整每个样例的所属的类别$$c^{(i)}$$来让J函数减小，同样，固定$$c^{(i)}$$，调整每个类的质心$$\mu_j$$，也可以使J减小。 这两个过程就是内循环中使J单调递减的过程。当J递减到最小时，$$\mu$$和c也同时收敛。（在理论上，可以有多组不同的$$\mu$$和c值，能够使得J取得最小值，但这种现象实际上很少见。）

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
