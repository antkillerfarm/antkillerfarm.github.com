---
layout: post
title:  机器学习（十）——因子分析
category: technology 
---

## 利用多元高斯分布密度函数计算积分的技巧（续）

公式右侧的被积分函数，正好是多元高斯分布密度函数，因此该积分值为1。于是：

$$I(A,b,c)=\frac{(2\pi)^{n/2}\lvert\Sigma\rvert^{1/2}}{\exp(\frac{1}{2}k)}$$

>注：原始讲义里，Chuong B. Do写的《Gaussian processes》的附录A.1和本节内容类似，但推导过程有问题，疑似笔误，特更换为维基百科中的例子。（矩阵的完全配方那块的变换，我能推导出维基百科的结果，但推导不出Chuong B. Do的结果。）如有错误，望读者指出。

## 边缘和条件高斯分布

假设x由两个随机向量组成（可以看作是将之前的$$x^{(i)}$$分成了两部分）。

$$x=\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}$$

其中$$x_1\in R^r,x_1\in R^s$$，则x实际上是$$r+s$$维向量。

假设$$x\sim N(\mu,\Sigma)$$，其中：

$$\mu=\begin{bmatrix} \mu_1 \\ mu_2 \end{bmatrix},\Sigma=\begin{bmatrix} \Sigma_{11} & \Sigma_{12} \\ \Sigma_{21} & \Sigma_{22} \end{bmatrix}$$

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
\\&\qquad+(x_2-\mu_2)^TV_{21}(x_1-\mu_1)+(x_2-\mu_2)^TV_{22}(x_2-\mu_2)\tag{1}
\end{align}$$

保留公式1中与$$x_1$$有关的部分，可得：

$$p(x_1\vert x_2)=\frac{1}{Z_2}\exp\left(-\frac{1}{2}\left(x_1^TV_{11}x_1-2x_1^TV_{11}\mu_1+2x_1^TV_{12}(x_2-\mu_2)\right)\right)$$

使用上一节中的完全配方技巧，可得：

$$p(x_1\vert x_2)=\frac{1}{Z_3}\exp\left(-\frac{1}{2}(x_1-\mu_{1\vert 2})^TV_{11}(x_1-\mu_{1\vert 2})\right)$$

其中：

$$\mu_{1\vert 2}=\mu_1-V_{11}^{-1}V_{12}(x_2-\mu_2)\tag{2}$$

即：

$$x_1\vert x_2\sim N(\mu_1-V_{11}^{-1}V_{12}(x_2-\mu_2),V_{11}^{-1})$$

另，根据分块矩阵的求逆法则，可得：

$$\Sigma^{-1}=\begin{bmatrix} \Sigma_{11} & \Sigma_{12} \\ \Sigma_{21} & \Sigma_{22} \end{bmatrix}^{-1}=\begin{bmatrix} (\Sigma_{11}-\Sigma_{12}\Sigma_{22}^{-1}\Sigma_{21})^{-1} & \Sigma_{12} \\ \Sigma_{21} & \Sigma_{22} \end{bmatrix}$$

## 因子分析模型


