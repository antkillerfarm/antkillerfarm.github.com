---
layout: post
title:  机器学习（二）
category: technology 
---

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

$$y\vert x;\theta \sim ExponentialFamily(\eta)(公式1)$$

$$h(x)=E[T(y)\vert x](公式2)$$

$$\eta=\theta^Tx(公式3)$$

下面以多项分布为例展示一下GLM的处理方法。

$$y$$的取值为$$k$$个离散值的分布，被称为$$k$$项分布。显然$$k=2$$时，就是二项分布了。

这里将$$k$$个离散值出现的概率记作$$\phi_1,...,\phi_k$$。由于$$\sum_{i=1}^k=1$$，因此，$$k$$项分布的自由度为$$k-1$$。

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

$$\eta_i=\log(\frac{\phi_i}{\phi_k})(公式4)\Rightarrow e^{\eta_i}=\frac{\phi_i}{\phi_k}\Rightarrow \phi_ke^{\eta_i}=\phi_i$$

$$\Rightarrow \phi_k\sum_{i=1}^ke^{\eta_i}=\sum_{i=1}^k\phi_i=1\Rightarrow \phi_k=\frac{1}{\sum_{i=1}^ke^{\eta_i}}（公式5）$$

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

$$p(y\vert x)=\frac{p(x\vert y)p(y)}{p(x)}(公式1)$$

其中，$$p(x\vert y)$$称为后验概率，$$p(y)$$称为先验概率。

由于我们关注的是y的离散值结果中哪个概率大（比如山羊概率和绵羊概率哪个大），而并不是关心具体的概率，因此公式1可改写为：

$$\arg\max_yp(y\vert x)=\arg\max_y\frac{p(x\vert y)p(y)}{p(x)}=\arg\max_yp(x\vert y)p(y)$$

## 高斯判别分析

高斯分布的向量形式$$N(\mu,\Sigma)$$的概率密度函数为：

$$p(x;\mu,\Sigma)=\frac{1}{(2\pi)^{n/2}\lvert\Sigma\rvert^{n/2}}\exp\left(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)\right)$$

其中，$$\mu$$表示均值向量（Mean Vector），$$\Sigma$$表示协方差矩阵（Covariance Matrix）

高斯判别分析（GDA，Gaussian Discriminant Analysis）


