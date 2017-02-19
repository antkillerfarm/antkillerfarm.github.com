---
layout: post
title:  机器学习（八）——在线学习, K-Means算法
category: theory 
---

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

## 贝叶斯统计和规则化

前面提到最大似然（maximum likelihood）估计方法的公式如下：

$$\theta_{ML}=\arg\max_\theta\prod_{i=1}^mp(y^{(i)}\vert x^{(i)};\theta)$$

从频率统计（frequentist statistic）学派的观点来看，这里的$$\theta$$是一个未知的常数，我们的任务就是求出这个常数。然而从贝叶斯学派的观点来看，$$\theta$$是一个未知的随机变量。

也就是说似然函数，对于前者来说，是这样的：$$\prod_{i=1}^mp(y^{(i)}\vert x^{(i)};\theta)$$；但对于后者来说，却是这样的：$$\prod_{i=1}^mp(y^{(i)}\vert x^{(i)},\theta)$$

我们首先假定$$\theta$$的分布为$$p(\theta)$$，这种假定由于没有事实根据，通常被称作先验分布（prior distribution）。

我们针对训练集$$S=\{(x^{(i)},y^{(i)})\}_{i=1}^m$$，训练得到预测函数。并按照如下公式计算后验分布（posterior distribution）：

$$p(\theta\vert S)=\frac{p(S\vert\theta)p(\theta)}{p(S)}\tag{1}$$

由全概率公式可得：

$$p(S)=p(S\vert\theta_1)p(\theta_1)+\dots+p(S\vert\theta_n)p(\theta_n)$$

上式的$$\theta_i$$表示$$\theta$$的各个取值区间，然而由于$$\theta$$是连续随机变量，根据微积分原理可得：

$$p(S)=\int_\theta p(S\vert\theta)p(\theta)\mathrm{d}\theta\tag{2}$$

将公式2代入公式1可得：

$$p(\theta\vert S)=\frac{p(S\vert\theta)p(\theta)}{\int_\theta p(S\vert\theta)p(\theta)\mathrm{d}\theta}=\frac{\left(\prod_{i=1}^mp(y^{(i)}\vert x^{(i)},\theta)\right)p(\theta)}{\int_\theta\left(\prod_{i=1}^mp(y^{(i)}\vert x^{(i)},\theta)\right)p(\theta)\mathrm{d}\theta}$$

当我们针对新的样本x进行预测时，和上面的推导类似，可得：

$$p(y\vert x,S)=\int_\theta p(y\vert x,\theta,S)p(\theta\vert S)\mathrm{d}\theta$$

因为预测样本集和训练样本集的分布是独立的，因此上式又可写为：

$$p(y\vert x,S)=\int_\theta p(y\vert x,\theta)p(\theta\vert S)\mathrm{d}\theta$$

这个公式又被称作后验预测分布（Posterior predictive distribution）。

$$p(\theta\vert S)$$可由前面的公式得到。

假若我们要求期望值的话，那么套用求期望的公式即可：

$$E[y\vert x,S]=\int_y yp(y\vert x,S)\mathrm{d}y$$

由上可见，贝叶斯估计将$$\theta$$视为随机变量，$$\theta$$的值满足一定的分布，不是固定值，我们无法通过计算获得其值，只能在预测时计算积分。

上述贝叶斯估计方法，虽然公式合理优美，但后验概率$$p(\theta\vert S)$$通常是很难计算的，因为它是$$\theta$$上的高维积分函数。

观察$$p(\theta\vert S)$$的公式，在分母$$P(S)$$一定的情况下，分子越大则值越大，也就是$$p(\theta\vert S)$$的概率越大。

因此，可得如下算法：

$$\theta_{MAP}=\arg\max_\theta\left(\prod_{i=1}^mp(y^{(i)}\vert x^{(i)},\theta)\right)p(\theta)$$

这个算法叫做最大后验概率估计（maximum a posteriori）。

和ML相比，MAP算法只是在最后多了一项$$p(\theta)$$。通常使用中，我们认为$$p(\theta)$$符合$$\theta\sim N(0,\tau^2I)$$。

由于$$p(\theta)$$实际上就是先验分布，它会对最后结果进行一定的修正。因此，实际上最大后验概率估计相对于最大似然估计来说，较容易克服过度拟合的问题。

![](/images/article/MAP.png)

参见：

http://www.cs.cornell.edu/courses/cs5540/2010sp/lectures/Lec9.Estimation-continued.pdf

# 在线学习

我们之前讨论的算法，都是给定一个训练集S，经训练之后，得到预测函数h，然后再在新的样本集上进行预测。这种方法被称为批量学习（batch learning）。

与之相对的，还有一种边学习边预测的在线学习（online learning）算法。其步骤如下：

>1.$$i:=0$$。   
>2.输入$$x^{(i)}$$，算法预测$$y^{(i)}$$。   
>3.根据$$y^{(i)}$$的真实值，修正算法模型。这一步也被称作更新过程（update procedure）   
>4.令$$i:=i+1$$，以处理下一个数据样本。

在线学习的优点：
1.算法在学习过程中，即可预测。
2.随着数据样本的增多，预测会更加准确，即具有自我完善的的能力。

下面以感知器（perceptron）算法为例，讨论一下在线学习的误差问题。

首先回忆一下感知器算法：

$$h_\theta(x)=g(\theta^Tx),y\in\{1,-1\}$$

$$g(z)=\begin{cases}
1, & z\ge 0 \\
-1, & z<0 \\
\end{cases}$$

对于训练样本$$(x,y)$$，其更新过程为：

$$\theta:=\begin{cases}
\theta, & h_\theta(x)=y \\
\theta+yx, & otherwise \\
\end{cases}\tag{3}$$

这里给出一个和感知器算法有关的定理：

对于给定的m个样本**序列**（注意“序列”二字，在线算法对于样本的顺序是敏感的）$$x^{(i)}$$，如果所有的$$\|x^{(i)}\|\le D$$，并且存在单位向量u，使得所有样本的$$y^{(i)}(u^Tx^{(i)})\ge \gamma$$，则感知器算法在该序列上的预测错误个数最多为$$(D/\gamma)^2$$。

这个定理是Henry David Block和Albert B. J. Novikoff于1962年提出的。

>注：Henry David Block，1920～1978，美国数学家。   
>这个人的经历有点非典型。20岁本科毕业，由于专业是文学和心理学，结果找不到工作。只好回炉，又读了个土木工程的本科。   
>二战期间在Goodyear的飞机工厂（没错，就是那个卖轮胎的Goodyear）担任测试工程师。在那里碰到一个当医生的英国妹子，搞定之。   
>1946年，他老婆在Iowa State University获得了一个职位，于是他也跟着搬了过去。估计是无所事事，他经常到大学里蹭课，然后就发现自己对数学很感兴趣。   
>于是继续读书，1949年拿到博士学位。经过几年助教生涯之后，最终成为康奈尔大学应用数学教授。   
>话说，根据缩写找人真是太痛苦了，很多资料都不是你要找的人，Block又是个大路货。我最后是在他的一位同事的论文中找到他的全名的。那篇论文发表于1985年，距离他去世已经7年，但他仍是作者之一，可见人缘不错。

>Albert B. J. Novikoff，全名不详，只知道是纽约大学教授。从岁数来看应该已经退休了。

下面给出这个定理的证明过程：

由公式3可知，$$\theta$$的值只有在发生预测错误的时候才会改变，因此我们可以使用$$\theta^{(k)}$$表示第k个错误。同时令$$\theta^{{1}}=0$$。

根据更新公式可得：

$$(\theta^{(k+1)})^Tu=(\theta^{(k)})^Tu+y^{(i)}(x^{(i)})^Tu\ge (\theta^{(k)})^Tu+\gamma$$

所以：

$$(\theta^{(2)})^Tu\ge (\theta^{(1)})^Tu+\gamma=\gamma$$

$$(\theta^{(3)})^Tu\ge (\theta^{(2)})^Tu+\gamma\ge 2\gamma$$

由数学归纳法可得：

$$(\theta^{(k+1)})^Tu\ge k\gamma\tag{4}$$

另外，

$$\|\theta^{(k+1)}\|^2=\|\theta^{(k)}+y^{(i)}x^{(i)}\|^2=\|\theta^{(k)}\|^2+\|y^{(i)}x^{(i)}\|^2+2y^{(i)}(x^{(i)})^T\theta^{(k)}$$

由感知器算法的定义可得:

$$y^{(i)}(x^{(i)})^T\theta^{(k)}\le 0$$

所以：

$$\|\theta^{(k+1)}\|^2\le \|\theta^{(k)}\|^2+\|x^{(i)}\|^2\le \|\theta^{(k)}\|^2+D^2$$

同样，由数学归纳法可得：

$$\|\theta^{(k+1)}\|^2\le kD^2\tag{5}$$

因为：

$$(\theta^{(k+1)})^Tu=\|\theta^{(k+1)}\|\cdot\|u\|\cdot \cos\phi\le \|\theta^{(k+1)}\|\cdot\|u\|=\|\theta^{(k+1)}\|\tag{6}$$

由公式4、5、6可得：

$$\sqrt{k}D\ge \|\theta^{(k+1)}\|\ge (\theta^{(k+1)})^Tu\ge k\gamma$$

所以：

$$k\le (D/\gamma)^2$$

# K-Means算法

聚类算法属于无监督学习算法的一种。它的训练样本中只有$$x^{(i)}$$，而没有$$y^{(i)}$$。聚类的目的是找到每个样本x潜在的类别y，并将同类别y的样本x放在一起，形成一个聚类（clusters）。样本$$x^{(i)}$$所属的聚类用$$c^{(i)}$$表示。

K-Means算法的步骤如下:

>1.随机选取k个聚类质心点（cluster centroids）$$\mu_1,\dots,\mu_k$$   
>2.重复下面过程直到收敛 {   
><span style="white-space: pre">	</span>对于每一个样例i，计算其应该属于的聚类：$$c^{(i)}:=\arg\min_j\|x^{(i)}-\mu_j\|^2$$   
><span style="white-space: pre">	</span>对于每一个聚类j，重新计算该聚类的质心：$$\mu_j:=\frac{\sum_{i=1}^m1\{c^{(i)}=j\}x^{(i)}}{\sum_{i=1}^m1\{c^{(i)}=j\}}$$。   
>}

其中，k是我们事先定义的聚类个数。下图展示了对n个样本点进行K-means聚类的效果，这里k取2。

![](/images/article/k-means.png)

K-means算法面对的第一个问题是如何保证收敛。前面的算法中强调结束条件就是收敛，可以证明的是K-means完全可以保证收敛性。下面我们定性的描述一下收敛性，我们定义畸变函数（distortion function）如下：

$$J(c,\mu)=\sum_{i=1}^m\|x^{(i)}-\mu_{c^{(i)}}\|^2$$

J函数表示每个样本点到其质心的距离平方和。K-means算法的目的是要将J调整到最小。假设当前J没有达到最小值，那么首先可以固定每个类的质心$$\mu_j$$，调整每个样例的所属的类别$$c^{(i)}$$来让J函数减小，同样，固定$$c^{(i)}$$，调整每个类的质心$$\mu_j$$，也可以使J减小。 这两个过程就是内循环中使J单调递减的过程。当J递减到最小时，$$\mu$$和c也同时收敛。（在理论上，可以有多组不同的$$\mu$$和c值，能够使得J取得最小值，但这种现象实际上很少见。）

由于畸变函数J是非凸函数，意味着我们不能保证算法取得的最小值是全局最小值，也就是说k-means对质心初始位置的选取比较敏感。但一般情况下k-means达到的局部最优已经满足需求。但如果你怕陷入局部最优，那么可以选取不同的初始值跑多遍k-means，然后取其中最小的J对应的$$\mu$$和c输出。

参考：

http://www.csdn.net/article/2012-07-03/2807073-k-means

http://www.cnblogs.com/leoo2sk/archive/2010/09/20/k-means.html

http://www.cnblogs.com/jerrylead/archive/2011/04/06/2006910.html

## 聚类结果的评价

可考虑用以下几个指标来评价聚类效果：

1.聚类中心之间的距离。距离值大，通常可考虑分为不同类。

2.聚类域中的样本数目。样本数目少且聚类中心距离远，可考虑是否为噪声。

3.聚类域内样本的距离方差。方差过大的样本可考虑是否属于这一类。


