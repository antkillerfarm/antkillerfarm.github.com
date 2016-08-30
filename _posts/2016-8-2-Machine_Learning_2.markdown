---
layout: post
title:  机器学习（二）——广义线性模型、生成学习算法
category: technology 
---

## 逻辑回归(续)

我们假设：

$$P(y=1\vert x;\theta)=h_\theta(x),P(y=0\vert x;\theta)=1-h_\theta(x)$$

则该伯努利分布（Bernoulli distribution）的概率密度函数为：

$$p(y\vert x;\theta)=(h_\theta(x))^y(1-h_\theta(x))^{1-y}$$

其似然估计函数为：

$$L(\theta)=p(\vec{y}\vert X;\theta)=\prod_{i=1}^m(h_\theta(x^{(i)}))^{y^{(i)}}(1-h_\theta(x^{(i)}))^{1-y^{(i)}}$$

两边都取对数，得到对数化的似然估计函数：

$$l(\theta)=\log L(\theta)=\sum_{i=1}^my^{(i)}\log h_\theta(x^{(i)})+(1-y^{(i)})\log(1-h_\theta(x^{(i)}))$$

$$\frac{\partial l(\theta)}{\partial \theta_j}=(y-h_\theta(x))x_j$$

按照随机梯度下降法，计算迭代公式：

$$\theta_j:=\theta_j+\alpha(y^{(i)}-h_{\theta}(x^{(i)}))x^{(i)}_j$$

可以看出，这和线性回归的迭代公式（公式4）完全相同。

$$g(z)$$还可以取以下函数：

$$g(z)=\begin{cases}
1, & z\ge 0 \\
0, & z<0 \\
\end{cases}$$

这时又被叫做感知器学习（perceptron learning）算法。

## 指数类分布

线性回归和对数回归的迭代公式相同不是偶然的，它们都是指数类分布的特例。

指数类分布（exponential family distributions）的标准形式如下：

$$p(y;\eta)=b(y)\exp(\eta^TT(y)-a(\eta))$$

其中，$$\eta$$被称作自然参数（natural parameter）或正准参数（canonical parameter），$$T(y)$$被称作充分统计量（sufficient statistic）。

$$a(\eta)$$是对数配分函数（log partition function），它存在的目的是利用$$e^{-a(\eta)}$$进行约束，以使:

$$\sum_Y p(y;\eta)=1或\int_Y p(y;\eta)=1$$

伯努利分布到指数类分布的变换过程如下：

$$\begin{align}p(y:\phi)&=\phi^y(1-\phi)^{1-y}=\exp(\log(\phi^y(1-\phi)^{1-y}))
\\&=\exp(y\log(\phi)+(1-y)\log(1-\phi))
\\&=\exp(y\log(\frac{\phi}{1-\phi})+\log(1-\phi))
\end{align}$$

可见：

$$\begin{align}& b(y)=1 \\& \eta=\log(\frac{\phi}{1-\phi})\Rightarrow \phi=\frac{1}{1+e^{-\eta}} \\& T(y)=y \\& a(\eta)=-\log(1-\phi) \\\end{align}$$

高斯分布到指数类分布的变换过程如下：

$$\begin{align}p(y;\mu)&=\frac{1}{\sqrt{2\pi}}\exp\left(-\frac{1}{2}(y-\mu)^2\right)
\\&=\frac{1}{\sqrt{2\pi}}\exp\left(-\frac{1}{2}y^2\right)\cdot\exp\left(\mu y-\frac{1}{2}\mu^2\right)\dot\
\end{align}$$

可见：

$$\begin{align}& \eta=\mu \\& T(y)=y \\& a(\eta)=\frac{\mu^2}{2} \\& b(y)=\frac{1}{\sqrt{2\pi}}\exp\left(-\frac{1}{2}y^2\right) \\\end{align}$$

除此之外，Dirichlet分布、Poisson分布、多项分布、$$\beta$$分布、$$\gamma$$分布都是指数类分布。

## 广义线性模型

广义线性模型（Generalized Linear Model，GLM）是解决指数类分布的回归问题的通用模型。它基于以下三个假设：

$$y\vert x;\theta \sim ExponentialFamily(\eta) \tag{1}$$

$$h(x)=E[T(y)\vert x] \tag{2}$$

$$\eta=\theta^Tx \tag{3}$$

下面以多项分布为例展示一下GLM的处理方法。

$$y$$的取值为$$k$$个离散值的分布，被称为$$k$$项分布。显然$$k=2$$时，就是二项分布了。

这里将$$k$$个离散值出现的概率记作$$\phi_1,\dots,\phi_k$$。由于$$\sum_{i=1}^k=1$$，因此，$$k$$项分布的自由度为$$k-1$$。

定义$$k-1$$维空间上的向量$$T(y)$$：

$$T(1)=\begin{bmatrix} 1 \\ 0 \\ 0 \\ \vdots \\ 0 \end{bmatrix},
T(2)=\begin{bmatrix} 0 \\ 1 \\ 0 \\ \vdots \\ 0 \end{bmatrix},
T(k-1)=\begin{bmatrix} 0 \\ 0 \\ 0 \\ \vdots \\ 1 \end{bmatrix},
T(k)=\begin{bmatrix} 0 \\ 0 \\ 0 \\ \vdots \\ 0 \end{bmatrix}
$$

我们使用$$(T(y))_i$$表示$$T(y)$$的第$$i$$个元素。

定义函数$$1\{True\}=1,1\{False\}=0$$，则$$(T(y))_i=1\{y=i\},E[(T(y))_i]=P(y=i)=\phi_i$$。

$$\begin{align}p(y:\phi)&=\phi_1^{1\{y=1\}}\phi_2^{1\{y=2\}}\cdots \phi_k^{1\{y=k\}}=\phi_1^{1\{y=1\}}\phi_2^{1\{y=2\}}\cdots \phi_k^{1-\sum_{i=1}^{k-1}1\{y=i\}}
\\&=\phi_1^{(T(y))_1}\phi_2^{(T(y))_2}\cdots \phi_k^{1-\sum_{i=1}^{k-1}(T(y))_i}
\\&=\exp((T(y))_1\log(\phi_1)+(T(y))_2\log(\phi_2)+\cdots+(1-\sum_{i=1}^{k-1}(T(y))_i)\log(\phi_k))
\\&=\exp((T(y))_1\log(\frac{\phi_1}{\phi_k})+(T(y))_2\log(\frac{\phi_2}{\phi_k})+\cdots+(T(y))_{k-1}\log(\frac{\phi_{k-1}}{\phi_k})+\log(\phi_k))
\end{align}$$

可见，

$$\eta=\begin{bmatrix} \log(\frac{\phi_1}{\phi_k}) \\ \log(\frac{\phi_2}{\phi_k}) \\ \vdots \\ \log(\frac{\phi_{k-1}}{\phi_k}) \end{bmatrix},a(\eta)=-\log(\phi_k),b(y)=1$$

$$\eta_i=\log(\frac{\phi_i}{\phi_k})\tag{4}$$

$$\Rightarrow e^{\eta_i}=\frac{\phi_i}{\phi_k}\Rightarrow \phi_ke^{\eta_i}=\phi_i$$

$$\Rightarrow \phi_k\sum_{i=1}^ke^{\eta_i}=\sum_{i=1}^k\phi_i=1$$

$$\Rightarrow \phi_k=\frac{1}{\sum_{i=1}^ke^{\eta_i}}\tag{5}$$

由公式4、5可得：

$$\phi_i=\frac{e^{\eta_i}}{\sum_{j=1}^ke^{\eta_j}}=\frac{e^{\theta_i^Tx}}{\sum_{j=1}^ke^{\theta_j^Tx}}$$

这种从$$\eta$$映射到$$\phi$$的函数，被称作softmax函数。

$$h_\theta(x)=E[T(y)\vert x;\theta]=\begin{bmatrix} \phi_1 \\ \phi_2 \\ \vdots \\ \phi_{k-1} \end{bmatrix}
=\begin{bmatrix} \frac{\exp(\theta_1^Tx)}{\sum_{j=1}^k\exp(\theta_j^Tx)} \\ \frac{\exp(\theta_2^Tx)}{\sum_{j=1}^k\exp(\theta_j^Tx)} \\ \vdots \\ \frac{\exp(\theta_{k-1}^Tx)}{\sum_{j=1}^k\exp(\theta_j^Tx)} \end{bmatrix}$$

最大似然估计对数函数：

$$l(\theta)=\sum_{i=1}^m\log p(y^{(i)}\vert x^{(i)};\theta)=\sum_{i=1}^m\log\prod_{l=1}^k\left(\frac{\exp(\theta_{l}^Tx^{(i)})}{\sum_{j=1}^k\exp(\theta_j^Tx^{(i)})}\right)^{1\{y^{(i)}=l\}}$$

## 机器学习的优化问题

优化理论和算法是机器学习用于处理问题的重要工具，但是机器学习有自己独特的看待问题的视角，并且其中也有很多和 Optimization 并不直接相关的部分，反过来Machine Learning也对Optimization产生影响。

例如，Interior Point Method的发明被认为是Optimization中的重要里程碑，这一类的方法能够保证在多项式次迭代内收敛。但是在机器学习中，特别是现在的所谓“大数据”的趋势下，这类算法却没法工作，一方面由于机器学习中所处理的数据通常维度非常高，从而相应的优化问题的变量个数变得很巨大，传统的方法虽然保证多项式迭代收敛，但是其中每一步迭代的计算代价却是随着变量个数的平方甚至三次方增长，结果是连算法的一次迭代都无法在可接受的时间内完成。于是（机器学习方面的）人们逐渐将注意力集中到主要基于first-order oracle的单次迭代计算量非常小的算法上。另一方面，数据点的个数的爆炸性增长也使得stochastic类的算法受到更多的关注——同样是降低单次迭代的计算复杂度。

# 生成学习算法

比如说，要确定一只羊是山羊还是绵羊。从历史数据中学习到模型，然后通过提取这只羊的特征，来预测出这只羊是山羊还是绵羊。这种方法叫做判别学习算法（DLA，Discriminative Learning Algorithm）。其形式化的写法是：$$p(y\vert x)$$。

换一种思路，我们可以根据山羊的特征首先学习出一个山羊模型，然后根据绵羊的特征学习出一个绵羊模型。然后从这只羊中提取特征，放到山羊模型中看概率是多少，再放到绵羊模型中看概率是多少，哪个大就是哪个。这种方法叫做生成学习算法（GLA，Generative Learning Algorithms）。其形式化的写法是：建立模型——$$p(x\vert y)$$，应用模型——$$p(y)$$。

由贝叶斯（Bayes）公式可知：

$$p(y\vert x)=\frac{p(x\vert y)p(y)}{p(x\vert y=1)p(y=1)+p(x\vert y=0)p(y=0)}=\frac{p(x\vert y)p(y)}{p(x)} \tag{6}$$

其中，$$p(x\vert y)$$称为后验概率，$$p(y)$$称为先验概率。

注：Thomas Bayes，1701~1761，英国统计学家。

由于我们关注的是y的离散值结果中哪个概率大（比如山羊概率和绵羊概率哪个大），而并不是关心具体的概率，因此公式6可改写为：

$$\arg\max_yp(y\vert x)=\arg\max_y\frac{p(x\vert y)p(y)}{p(x)}=\arg\max_yp(x\vert y)p(y) \tag{7}$$

## 高斯分布的向量形式

高斯分布的向量形式$$N(\mu,\Sigma)$$的概率密度函数为：

$$p(x;\mu,\Sigma)=\frac{1}{(2\pi)^{n/2}\lvert\Sigma\rvert^{n/2}}\exp\left(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)\right)$$

其中，$$\mu$$表示均值向量（Mean Vector），$$\Sigma$$表示协方差矩阵（Covariance Matrix），$$\lvert\Sigma\rvert$$表示协方差矩阵的行列式。

## 矩阵行列式计算

对于高阶矩阵行列式，一般采用莱布尼茨公式（Leibniz Formula）或拉普拉斯公式（Laplace Formula）计算。

首先，定义排列A的反序向量V（Inversion Vector）。下面举一个包含6个元素的例子：

序列 | 4 1 5 2 6 3
反序向量 | 0 1 0 2 0 3

$$V_i=\sum_{j=1}^{i-1}f(i,j),
f(i,j)=\begin{cases}
1, & A_i<A_j \\
0, & A_i>A_j \\
\end{cases}$$

反序向量的模被称为总序数（Total Order），例如上面例子的总序数为$$1+2+3=6$$。

总序数为奇数的排列被称为奇排列（Odd Permutations），为偶数的排列被称为偶排列（Even Permutations）。

定义勒维奇维塔符号(Levi-Civita symbol)如下：

$$\varepsilon_{a_1 a_2 a_3 \ldots a_n} =
\begin{cases}
+1 & \text{if }(a_1 , a_2 , a_3 , \ldots , a_n) \text{ is an even permutation of } (1,2,3,\dots,n) \\
-1 & \text{if }(a_1 , a_2 , a_3 , \ldots , a_n) \text{ is an odd permutation of } (1,2,3,\dots,n) \\
0 & \text{otherwise}
\end{cases}$$

注：Tullio Levi-Civita，1873~1941，意大利数学家。他在张量微积分领域的贡献，帮助了相对论的确立。

莱布尼茨公式：

$$\det(A) = \sum_{i_1,i_2,\ldots,i_n=1}^n \varepsilon_{i_1\cdots i_n}  a_{1,i_1} \cdots a_{n,i_n}$$

## 高斯判别分析

高斯判别分析（GDA，Gaussian Discriminant Analysis）模型需要满足以下条件：

$$y\sim Bernoulli(\phi)$$

$$x\vert y=0\sim N(\mu_0,\Sigma)$$

$$x\vert y=1\sim N(\mu_1,\Sigma)$$

注：这里只讨论y有两种分类的情况，且假设两种分类的$$\Sigma$$相同。

相应的概率密度函数为：

$$p(y)=\phi^y(1-\phi)^{1-y}$$

$$p(x\vert y=0)=\frac{1}{(2\pi)^{n/2}\lvert\Sigma\rvert^{n/2}}\exp\left(-\frac{1}{2}(x-\mu_0)^T\Sigma^{-1}(x-\mu_0)\right)=\frac{1}{A}\exp(f(\mu_0,\Sigma,x))$$

$$p(x\vert y=1)=\frac{1}{(2\pi)^{n/2}\lvert\Sigma\rvert^{n/2}}\exp\left(-\frac{1}{2}(x-\mu_1)^T\Sigma^{-1}(x-\mu_1)\right)=\frac{1}{A}\exp(f(\mu_1,\Sigma,x))$$

将上面三个分布的概率密度函数代入公式7，可求得$$\arg\max_yp(y\vert x)$$，然后进行最大似然估计，可得该GDA的最大似然估计参数为：（过程略）

$$\phi=\frac{1}{m}\sum_{i=1}^m1\{y^{(i)}=1\}$$

$$\mu_0=\frac{\sum_{i=1}^m1\{y^{(i)}=0\}x^{(i)}}{\sum_{i=1}^m1\{y^{(i)}=0\}}$$

$$\mu_1=\frac{\sum_{i=1}^m1\{y^{(i)}=1\}x^{(i)}}{\sum_{i=1}^m1\{y^{(i)}=1\}}$$

$$\Sigma=\frac{1}{m}\sum_{i=1}^m(x^{(i)}-\mu_{y^{(i)}})(x^{(i)}-\mu_{y^{(i)}})^T$$

![](/images/article/GDA.png)

上图中的直线就是分界线$$p(y=1\vert x)=0.5$$。

## GDA vs 逻辑回归

$$\begin{align}p(y=1\vert x)&=\frac{p(x\vert y=1)p(y=1)}{p(x\vert y=1)p(y=1)+p(x\vert y=0)p(y=0)}
\\&=\frac{\frac{1}{A}\exp(f(\mu_0,\Sigma,x))\phi}{\frac{1}{A}\exp(f(\mu_0,\Sigma,x))\phi + \frac{1}{A}\exp(f(\mu_1,\Sigma,x))(1-\phi)}
\\&=\frac{1}{1+\frac{\exp(f(\mu_1,\Sigma,x))(1-\phi)}{\exp(f(\mu_0,\Sigma,x))\phi}}
\\&=\frac{1}{1+\exp(f(\mu_1,\Sigma,x)+\log(1-\phi)-f(\mu_0,\Sigma,x)-\log(\phi))}
\\&=\frac{1}{1+\exp(-\theta^Tx)}
\end{align}$$

从上面的变换可以看出，GDA是逻辑回归的特例。

一般来说，GDA的条件比逻辑回归严格。因此，如果模型符合GDA的话，采用GDA方法，收敛速度（指所需训练集的数量）比较快。

而逻辑回归的鲁棒性较好，对于非GDA模型或者模型不够准确的情况，仍能收敛。

