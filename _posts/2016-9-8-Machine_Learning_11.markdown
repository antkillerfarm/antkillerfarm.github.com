---
layout: post
title:  机器学习（十一）——因子分析（2）
category: theory 
---

## 边缘和条件高斯分布

假设x由两个随机向量组成（可以看作是将之前的$$x^{(i)}$$分成了两部分）。

$$x=\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}$$

其中$$x_1\in R^r,x_1\in R^s$$，则x实际上是$$r+s$$维向量。

假设$$x\sim N(\mu,\Sigma)$$，其中：

$$\mu=\begin{bmatrix} \mu_1 \\ \mu_2 \end{bmatrix},\Sigma=\begin{bmatrix} \Sigma_{11} & \Sigma_{12} \\ \Sigma_{21} & \Sigma_{22} \end{bmatrix}$$

因为协方差矩阵是对称矩阵，因此$$\Sigma_{12}=\Sigma_{21}^T$$。

$$\begin{align}
Cov(x)&=\Sigma=\begin{bmatrix} \Sigma_{11} & \Sigma_{12} \\ \Sigma_{21} & \Sigma_{22} \end{bmatrix}
\\&=E\begin{bmatrix} (x-\mu)(x-\mu)^T \end{bmatrix}=E\begin{bmatrix} \begin{pmatrix} x_1-\mu_1 \\ x_2-\mu_2 \end{pmatrix} & \begin{pmatrix} x_1-\mu_1 \\ x_2-\mu_2 \end{pmatrix} \end{bmatrix}
\\&=E\begin{bmatrix} (x_1-\mu_1)(x_1-\mu_1)^T & (x_1-\mu_1)(x_2-\mu_2)^T \\ (x_2-\mu_2)(x_1-\mu_1)^T & (x_2-\mu_2)(x_2-\mu_2)^T \end{bmatrix}
\end{align}$$

因此，$$E[x_1]=\mu_1,Cov(x_1)=E[(x_1-\mu_1)(x_1-\mu_1)^T]=\Sigma_{11}$$。可见，多元高斯分布的边缘分布仍然是多元高斯分布。

下面讨论一下条件高斯分布。

$$\begin{align}
p(x_1\vert x_2)&=\frac{p(x_1,x_2)}{p(x_2)}=\frac{\frac{1}{(2\pi)^{n/2}\lvert\Sigma\rvert^{1/2}}\exp\left(-\frac{1}{2}(x-\mu)^T\Sigma^{-1}(x-\mu)\right)}{\int_{x_1}p(x_1,x_2;\mu,\Sigma)\mathrm{d}x_1}
\\&=\frac{1}{Z_1}\exp\left\{-\frac{1}{2}\left(\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}-\begin{bmatrix} \mu_1 \\ \mu_2 \end{bmatrix}\right)^T\begin{bmatrix} V_{11} & V_{12} \\ V_{21} & V_{22} \end{bmatrix}\left(\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}-\begin{bmatrix} \mu_1 \\ \mu_2 \end{bmatrix}\right)\right\}
\end{align}$$

其中的$$Z_1$$是和$$x_1$$无关的部分，可看作常数，下面的$$Z_i$$也是同理。

$$\Sigma^{-1}=V=\begin{bmatrix} V_{11} & V_{12} \\ V_{21} & V_{22} \end{bmatrix}$$

因为：

$$\begin{align}
&\left(\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}-\begin{bmatrix} \mu_1 \\ \mu_2 \end{bmatrix}\right)^T\begin{bmatrix} V_{11} & V_{12} \\ V_{21} & V_{22} \end{bmatrix}\left(\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}-\begin{bmatrix} \mu_1 \\ \mu_2 \end{bmatrix}\right)
\\&=(x_1-\mu_1)^TV_{11}(x_1-\mu_1)+(x_1-\mu_1)^TV_{12}(x_2-\mu_2)
\\&\qquad+(x_2-\mu_2)^TV_{21}(x_1-\mu_1)+(x_2-\mu_2)^TV_{22}(x_2-\mu_2)
\end{align}$$

保留上式中与$$x_1$$有关的部分，可得：

$$p(x_1\vert x_2)=\frac{1}{Z_2}\exp\left(-\frac{1}{2}\left(x_1^TV_{11}x_1-2x_1^TV_{11}\mu_1+2x_1^TV_{12}(x_2-\mu_2)\right)\right)$$

使用上一节中的完全配方技巧，可得：

$$p(x_1\vert x_2)=\frac{1}{Z_3}\exp\left(-\frac{1}{2}(x_1-\mu_{1\vert 2})^TV_{11}(x_1-\mu_{1\vert 2})\right)$$

其中：

$$\mu_{1\vert 2}=\mu_1-V_{11}^{-1}V_{12}(x_2-\mu_2)\tag{1}$$

即：

$$x_1\vert x_2\sim N(\mu_1-V_{11}^{-1}V_{12}(x_2-\mu_2),V_{11}^{-1})$$

另，根据分块矩阵的求逆法则，可得：

$$\Sigma^{-1}=\begin{bmatrix} \Sigma_{11} & \Sigma_{12} \\ \Sigma_{21} & \Sigma_{22} \end{bmatrix}^{-1}=\begin{bmatrix} (\Sigma_{11}-\Sigma_{12}\Sigma_{22}^{-1}\Sigma_{21})^{-1} & -(\Sigma_{11}-\Sigma_{12}\Sigma_{22}^{-1}\Sigma_{21})^{-1}\Sigma_{12}\Sigma_{22}^{-1} \\ -\Sigma_{22}^{-1}\Sigma_{21}(\Sigma_{11}-\Sigma_{12}\Sigma_{22}^{-1}\Sigma_{21})^{-1} & (\Sigma_{22}-\Sigma_{21}\Sigma_{11}^{-1}\Sigma_{12})^{-1} \end{bmatrix}$$

因此：

$$\Sigma_{1\vert 2}=V_{11}^{-1}=\Sigma_{11}-\Sigma_{12}\Sigma_{22}^{-1}\Sigma_{21}\tag{2}$$

## 因子分析的例子

下面通过一个简单例子，来引出因子分析背后的思想。

假设我们有m=5个2维的样本点$$x^{i}$$，如下：

![](/images/article/factor_analysis.png)

按照因子分析模型，样本点的生成过程如下：

![](/images/article/factor_analysis_1.png)

1.我们首先认为在1维空间（这里k=1），存在着按正态分布生成的m个点$$z^{(i)}$$，即：

$$z^{(i)}\sim N(0,I)$$

这里的I是单位矩阵。

![](/images/article/factor_analysis_2.png)

2.使用变换矩阵$$\Lambda\in R^{n\times k}$$，将$$z^{(i)}$$映射到n维空间中，即$$\Lambda z^{(i)}$$。

![](/images/article/factor_analysis_3.png)

3.使用n维向量$$\mu$$，将$$\Lambda z^{(i)}$$移动到样本的中心点$$\mu$$，即$$\mu+\Lambda z^{(i)}$$

![](/images/article/factor_analysis_4.png)

4.样本点不可能这么规则，在模型上会有一定偏差，因此我们需要将上步生成的点做一些扰动（误差）。这里添加一个n维的扰动向量$$\epsilon \sim N(0,\Psi)$$。

综上可得:

$$x^{(i)}=\mu+\Lambda z^{(i)}+\epsilon$$

$$x \vert z\sim N(\mu+\Lambda z,\Psi)$$

由以上的直观分析，我们知道了因子分析其实就是认为：高维样本点实际上是由低维样本点经过高斯分布、线性变换、误差扰动生成的，因此高维数据可以使用低维来表示。

## 线性回归的概率模型

在进一步讨论因子分析模型之前，我们首先讨论一下，和它类似的线性回归的概率模型。

从概率的角度看，线性回归中的$$y^{(i)}$$可以看作是预测函数$$h_\theta(x)$$加上扰动后的结果。即：

$$y^{(i)}=\theta^Tx^{(i)}+\epsilon^{(i)},\epsilon^{(i)}\sim N(0,\sigma^2)$$

$$p(\epsilon^{(i)})=\frac{1}{\sqrt{2\pi}\sigma}\exp\left(-\frac{(\epsilon^{(i)})^2}{2\sigma^2}\right)$$

$$p(y^{(i)}\vert x^{(i)};\theta)=\frac{1}{\sqrt{2\pi}\sigma}\exp\left(-\frac{(y^{(i)}-\theta^Tx^{(i)})^2}{2\sigma^2}\right)$$

$$\begin{align}
\ell(\theta)&=\log\prod_{i=1}^m\frac{1}{\sqrt{2\pi}\sigma}\exp\left(-\frac{(y^{(i)}-\theta^Tx^{(i)})^2}{2\sigma^2}\right)=\sum_{i=1}^m\log\frac{1}{\sqrt{2\pi}\sigma}\exp\left(-\frac{(y^{(i)}-\theta^Tx^{(i)})^2}{2\sigma^2}\right)
\\&=m\log\frac{1}{\sqrt{2\pi}\sigma}-\frac{1}{\sigma^2}\cdot\frac{1}{2}\sum_{i=1}^m\left(y^{(i)}-\theta^Tx^{(i)}\right)^2
\end{align}$$

从上式可以看出采用极大似然估计和采用代价函数$$J(\theta)$$的效果是一样的。其中：

$$J(\theta)=\frac{1}{2}\sum_{i=1}^m\left(y^{(i)}-\theta^Tx^{(i)}\right)^2$$

## 因子分析模型

假设z和x的联合分布为：

$$\begin{bmatrix} z \\ x \end{bmatrix}\sim N(\mu_{zx},\Sigma)$$

我们的任务就是求出$$\mu_{zx}$$和$$\Sigma$$。

因为：

$$E[x]=E[\mu+\Lambda z+\epsilon]=\mu+\Lambda E[z]+E[\epsilon]=\mu$$

所以：

$$\mu_{zx}=\begin{bmatrix} \vec{0} \\ \mu \end{bmatrix}$$

因为：

$$\Sigma=\begin{bmatrix} \Sigma_{zz} & \Sigma_{zx} \\ \Sigma_{xz} & \Sigma_{xx} \end{bmatrix}$$

所以我们只要分别计算这四个值即可。

因为$$z\sim N(0,I)$$，所以$$\Sigma_{zz}=I$$。

$$\begin{align}
\Sigma_{zx}&=E[(z-E[z])(x-E[x])^T]=E[(z-0)(\mu+\Lambda z+\epsilon-\mu)^T]=E[z(\Lambda z+\epsilon)^T]
\\&=E[z(\Lambda z)^T+z\epsilon^T]=E[zz^T\Lambda^T+z\epsilon^T]=E[zz^T]\Lambda^T+E[z\epsilon^T]
\end{align}$$

因为z和$$\epsilon$$是相互独立的随机变量，因此$$E[z\epsilon^T]=E[z]E[\epsilon^T]=0$$。

又因为$$E[zz^T]=Cov(z)=I$$，所以$$\Sigma_{zx}=\Lambda^T$$。

$$\begin{align}
\Sigma_{xx}&=E[(x-E[x])(x-E[x])^T]=E[(\mu+\Lambda z+\epsilon-\mu)(\mu+\Lambda z+\epsilon-\mu)^T]
\\&=E[(\Lambda z+\epsilon)(\Lambda z^T+\epsilon^T)]=E[\Lambda z(\Lambda z)^T+\epsilon(\Lambda z)^T+\Lambda z\epsilon^T+\epsilon\epsilon^T]
\\&=E[\Lambda zz^T\Lambda^T+\epsilon z^T\Lambda^T+\Lambda z\epsilon^T+\epsilon\epsilon^T]
\\&=\Lambda E[zz^T]\Lambda^T+E[\epsilon z^T]\Lambda^T+\Lambda E[z\epsilon^T]+E[\epsilon\epsilon^T]
\\&=\Lambda I\Lambda^T+0+0+\Psi=\Lambda \Lambda^T+\Psi
\end{align}$$

把这些结果合在一起，可得：

$$\begin{bmatrix} z \\ x \end{bmatrix}\sim N\left(\begin{bmatrix} \vec{0} \\ \mu \end{bmatrix},\begin{bmatrix} I & \Lambda^T \\ \Lambda & \Lambda \Lambda^T+\Psi \end{bmatrix}\right)\tag{1}$$

从这个结论可以看出：$$x\sim N(\mu,\Lambda \Lambda^T+\Psi)$$

因此它的对数似然函数为：

$$\ell(\mu,\Lambda,\Psi)=\log\prod_{i=1}^m\frac{1}{(2\pi)^{n/2}\lvert\Lambda \Lambda^T+\Psi\rvert^{1/2}}\exp\left(-\frac{1}{2}(x^{(i)}-\mu)^T(\Lambda \Lambda^T+\Psi)^{-1}(x^{(i)}-\mu)\right)$$

但这个函数是很难最大化的，需要使用EM算法解决之。

## 因子分析的EM估计

E-step比较简单。由《机器学习（十）》公式1、2和公式1，可得：

$$\mu_{z^{(i)}\vert x^{(i)}}=\Lambda^T(\Lambda \Lambda^T+\Psi)^{-1}(x^{(i)}-\mu)$$

$$\Sigma_{z^{(i)}\vert x^{(i)}}=I-\Lambda^T(\Lambda \Lambda^T+\Psi)^{-1}\Lambda$$

因此：

$$Q_i(z^{(i)})=\frac{1}{(2\pi)^{n/2}\lvert\Sigma_{z^{(i)}\vert x^{(i)}}\rvert^{1/2}}\exp\left(-\frac{1}{2}(x^{(i)}-\mu_{z^{(i)}\vert x^{(i)}})^T\Sigma_{z^{(i)}\vert x^{(i)}}^{-1}(x^{(i)}-\mu_{z^{(i)}\vert x^{(i)}})\right)$$

M-step的最大化的目标是：

$$\sum_{i=1}^m\int_{z^{(i)}}Q_i(z^{(i)})\log\frac{p(x^{(i)},z^{(i)};\mu,\Lambda,\Psi)}{Q_i(z^{(i)})}\mathrm{d}z^{(i)}$$

下面我们重点求$$\Lambda$$的估计公式。

首先将上式简化为:

$$\begin{align}
&\sum_{i=1}^m\int_{z^{(i)}}Q_i(z^{(i)})\log\frac{p(x^{(i)}\vert z^{(i)};\mu,\Lambda,\Psi)p(z^{(i)})}{Q_i(z^{(i)})}\mathrm{d}z^{(i)}
\\&=\sum_{i=1}^m\int_{z^{(i)}}Q_i(z^{(i)})\left[\log p(x^{(i)}\vert z^{(i)};\mu,\Lambda,\Psi)+\log p(z^{(i)})-\log Q_i(z^{(i)})\right]\mathrm{d}z^{(i)}
\\&=\sum_{i=1}^m E_{z^{(i)}\sim Q_i}\left[\log p(x^{(i)}\vert z^{(i)};\mu,\Lambda,\Psi)+\log p(z^{(i)})-\log Q_i(z^{(i)})\right]
\end{align}$$

去掉和各参数无关的部分后，可得：

$$\begin{align}
&\sum_{i=1}^mE\left[\log p(x^{(i)}\vert z^{(i)};\mu,\Lambda,\Psi)\right]
\\&=\sum_{i=1}^mE\left[\frac{1}{(2\pi)^{n/2}\lvert\Psi\rvert^{1/2}}\exp\left(-\frac{1}{2}(x^{(i)}-\mu-\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu-\Lambda z^{(i)})\right)\right]
\\&=\sum_{i=1}^mE\left[-\frac{1}{2}\log\lvert\Psi\rvert-\frac{n}{2}\log(2\pi)-\frac{1}{2}(x^{(i)}-\mu-\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu-\Lambda z^{(i)})\right]
\end{align}$$

去掉和$$\Lambda$$无关的部分，并求导可得：

$$\nabla_\Lambda\sum_{i=1}^m-E\left[\frac{1}{2}(x^{(i)}-\mu-\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu-\Lambda z^{(i)})\right]\tag{2}$$

因为公式2中$$E[\cdot]$$部分的结果实际上是个实数，因此该公式可变形为：

$$\nabla_\Lambda\sum_{i=1}^m-E\left[\operatorname{tr}\left(\frac{1}{2}(x^{(i)}-\mu-\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu-\Lambda z^{(i)})\right)\right]$$

而：

$$\begin{align}
&\frac{1}{2}(x^{(i)}-\mu-\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu-\Lambda z^{(i)})
\\&=\frac{1}{2}\left[((x^{(i)}-\mu)^T-(\Lambda z^{(i)})^T)\Psi^{-1}((x^{(i)}-\mu)-\Lambda z^{(i)})\right]
\\&=\frac{1}{2}\left[(x^{(i)}-\mu)^T\Psi^{-1}(x^{(i)}-\mu)-(x^{(i)}-\mu)^T\Psi^{-1}\Lambda z^{(i)}
\\-(\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu)+(\Lambda z^{(i)})^T\Psi^{-1}\Lambda z^{(i)}\right]
\end{align}$$

去掉和$$\Lambda$$无关的部分，可得：

$$\frac{1}{2}\left[(\Lambda z^{(i)})^T\Psi^{-1}\Lambda z^{(i)}-(x^{(i)}-\mu)^T\Psi^{-1}\Lambda z^{(i)}-(\Lambda z^{(i)})^T\Psi^{-1}(x^{(i)}-\mu)\right]$$

