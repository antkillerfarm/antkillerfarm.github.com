---
layout: post
title:  机器学习（九）——K-Means算法, 高斯混合模型和EM算法
category: ML 
---

# 在线学习（续）

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

深入浅出K-Means算法

http://www.cnblogs.com/leoo2sk/archive/2010/09/20/k-means.html

k均值聚类(K-means)

http://www.cnblogs.com/jerrylead/archive/2011/04/06/2006910.html

K-means聚类算法

https://mp.weixin.qq.com/s/86MlCMKAG0ax3gvfsgmYtg

K-Means聚类算法详解

https://mp.weixin.qq.com/s/Sl3lTS1zvq0BgAayAUcOXg

K-Means实战与调优详解

## 聚类结果的评价

可考虑用以下几个指标来评价聚类效果：

1.聚类中心之间的距离。距离值大，通常可考虑分为不同类。

2.聚类域中的样本数目。样本数目少且聚类中心距离远，可考虑是否为噪声。

3.聚类域内样本的距离方差。方差过大的样本可考虑是否属于这一类。

# 高斯混合模型和EM算法

本篇讨论使用期望最大化算法（Expectation-Maximization）进行密度估计（density estimation）。

从上面的讨论可以形象的看出，聚类问题实际就是在数据集上，找出一个个数据密度较高的“圆圈”。我们可以反过来思考这个问题：如果我们已知圆圈的圆心和半径，那么也可以根据圆心、半径以及样本分布模型，来随机生成这些数据。显然，这和前一种做法在效果上是等效的。

首先我们假设样本数据满足联合概率分布

$$p(x^{(i)},z^{(i)})=p(x^{(i)}\vert z^{(i)})p(z^{(i)})\tag{5}$$

其中，$$z^{(i)}\sim Multinomial(\phi)$$（这里的$$\phi_j=p(z^{(i)}=j)$$，因此$$\phi_j\ge 0,\sum_{j=1}^k\phi_j=1$$），$$z^{(i)}$$的值为k个聚类之一。

假定$$x^{(i)}\vert z^{(i)}=j\sim \mathcal{N}(\mu_j,\Sigma_j)$$，则该模型被称为高斯混合模型（mixture of Gaussians model）。

整个模型简单描述为对于每个样例$$x^{(i)}$$，我们先从k个类别中按多项分布抽取一个$$z^{(i)}$$，然后根据$$z^{(i)}$$所对应的k个多值高斯分布中的一个生成样例$$x^{(i)}$$。注意的是这里的$$z^{(i)}$$是隐含的随机变量（latent random variables）。

![](/images/article/EM.png)

因此，由全概率公式可得：

$$p(x^{(i)};\phi,\mu,\Sigma)=\sum_{z^{(i)}=1}^kp(x^{(i)}\vert z^{(i)};\mu,\Sigma)p(z^{(i)};\phi)\tag{6}$$

该模型的对数化似然函数为：

$$\ell(\phi,\mu,\Sigma)=\sum_{i=1}^m\log p(x^{(i)};\phi,\mu,\Sigma)=\sum_{i=1}^m\log \sum_{z^{(i)}=1}^kp(x^{(i)}\vert z^{(i)};\mu,\Sigma)p(z^{(i)};\phi)$$

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
><span style="white-space: pre">	</span>(E-step) For each i, j：   
><span style="white-space: pre">			</span>$$w_j^{(i)}:=p(z^{(i)}=j\vert x^{(i)};\phi,\mu,\Sigma)$$   
><span style="white-space: pre">	</span>(M-step) Update the parameters：   
><span style="white-space: pre">			</span>$$\phi_j:=\frac{1}{m}\sum_{i=1}^mw_j^{(i)}$$   
><span style="white-space: pre">			</span>$$\mu_j:=\frac{\sum_{i=1}^mw_j^{(i)}x^{(i)}}{\sum_{i=1}^mw_j^{(i)}}$$   
><span style="white-space: pre">			</span>$$\Sigma_j:=\frac{\sum_{i=1}^mw_j^{(i)}(x^{(i)}-\mu_j)(x^{(i)}-\mu_j)^T}{\sum_{i=1}^mw_j^{(i)}}$$   
>}

E-Step中，根据贝叶斯公式可得：

$$p(z^{(i)}=j\vert x^{(i)};\phi,\mu,\Sigma)=\frac{p(x^{(i)}\vert z^{(i)}=j;\mu,\Sigma)p(z^{(i)}=j;\phi)}{\sum_{l=1}^kp(x^{(i)}\vert z^{(i)}=l;\mu,\Sigma)p(z^{(i)}=l;\phi)}$$

将假设模型的各概率密度函数代入上式，即可计算得到$$w_j^{(i)}$$。

相比于K-means算法，GMM算法中的$$z^{(i)}$$是个概率值，而非确定值，因此也被称为soft assignments。

K-means算法各个聚类的特征都是一样的，也就是“圆圈”的半径一致。而GMM算法的“圆圈”半径可以不同。如下面两图所示：

| ![](/images/article/EM_2.png) | ![](/images/article/EM_3.png) |
| K-means算法 | GMM算法 |

GMM算法结果也是局部最优解。对其他参数取不同的初始值进行多次计算同样适用于GMM算法。

参考：

http://cseweb.ucsd.edu/~atsmith/project1_253.pdf

# EM算法

本节将进一步讨论EM算法的性质，并将之应用到使用latent random variables的一大类估计问题中。

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

