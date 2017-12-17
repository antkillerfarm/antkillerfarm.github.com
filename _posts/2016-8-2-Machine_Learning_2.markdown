---
layout: post
title:  机器学习（二）——分类与逻辑回归, 广义线性模型
category: ML 
---

# 分类与逻辑回归

## 二分类

结果集y的取值只有0和1的分类问题被称为二分类，其中0被称为negative class，1被称为positive class，也可用“-”和“+”来表示。

## 逻辑回归

为了将线性回归的结果约束到$$[0,1]$$区间，我们将公式1修改为：

$$h_\theta(x)=g(\theta^Tx)=\frac{1}{1+e^{-\theta^Tx}} \tag{6}$$

公式6又被称为logistic function或sigmoid function。函数$$g(z)$$的图像如下所示：

![](/images/article/sigmoid_function.png)

事实上，任何$$[0,1]$$区间的平滑增函数，都可以作为$$g(z)$$，但公式6的好处在于

$$g'(z)=\frac{1}{(1+e^{-z})^2}e^{-z}=\frac{1}{(1+e^{-z})}\left(1-\frac{1}{(1+e^{-z})}\right)=g(z)(1-g(z))\tag{7}$$

评估逻辑回归（Logistic regression）的质量，需要用到最大似然估计（maximum likelihood estimator）方法（由Ronald Aylmer Fisher提出）。最大似然估计是在“模型已定，参数未知”的情况下，寻找使模型出现的概率最大的参数集$$\theta$$的方法。显然，参数集$$\theta$$所确定的模型，其出现概率越大，模型的准确度越高。

最大似然估计中采样需满足一个很重要的假设，就是所有的采样都是独立同分布的（independent and identically distributed，IID），即：

$$f(x_1,\dots,x_n;\theta)=f(x_1;\theta)\times \dots \times f(x_n;\theta)$$

似然估计函数如下所示：

$$L(\theta)=\prod_{i=1}^mp(y^{(i)}\vert x^{(i)};\theta)$$

>注：Ronald Aylmer Fisher，1890～1962，英国人，毕业于剑桥大学。英国皇家学会会员，皇家统计学会主席。尽管他被称作“一位几乎独自建立现代统计科学的天才”，然而他的本职工作是遗传学。他最大的贡献是利用统计分析的方法，揭示了孟德尔的遗传定律在达尔文自然选择学说中的作用，为后来遗传物质DNA的发现奠定了理论基础。   
>虽然对于Fisher来说，数理统计只是他研究工作的一个副产品，但他在1925年所著《研究工作者的统计方法》（Statistical Methods for Research Workers），其影响力超过了半个世纪，几乎当代所有自然科学和社会科学领域都在应用他所创立的理论。F分布就是以他的名字命名的。

>Karl Pearson，1857~1936，英国人，毕业于剑桥大学。英国皇家学会会员。发现了$$\chi^2$$分布。

>William Sealy Gosset，1876~1937，英国人，毕业于牛津大学。笔名Student，发现了Student's t-distribution。

>这三人被后人合称现代统计学的三大创始人。他们都不是博士，毕业后从事的职业，也不是数学。Fisher和Pearson研究遗传学，Gosset研究化学。可见，统计学的诞生，有着很强的应用属性。

我们假设：

$$P(y=1\vert x;\theta)=h_\theta(x),P(y=0\vert x;\theta)=1-h_\theta(x)$$

则该伯努利分布（Bernoulli distribution）的概率密度函数为：

$$p(y\vert x;\theta)=(h_\theta(x))^y(1-h_\theta(x))^{1-y}$$

其似然估计函数为：

$$L(\theta)=p(\vec{y}\vert X;\theta)=\prod_{i=1}^m(h_\theta(x^{(i)}))^{y^{(i)}}(1-h_\theta(x^{(i)}))^{1-y^{(i)}}$$

两边都取对数，得到对数化的似然估计函数：

$$\ell(\theta)=\log L(\theta)=\sum_{i=1}^my^{(i)}\log h_\theta(x^{(i)})+(1-y^{(i)})\log(1-h_\theta(x^{(i)}))$$

$$\frac{\partial \ell(\theta)}{\partial \theta_j}=(y-h_\theta(x))x_j$$

按照随机梯度下降法，计算迭代公式：

$$\theta_j:=\theta_j+\alpha(y^{(i)}-h_{\theta}(x^{(i)}))x^{(i)}_j$$

可以看出，这和线性回归的迭代公式（公式4）完全相同。

$$g(z)$$还可以取以下函数（阶跃函数）：

$$g(z)=\begin{cases}
1, & z\ge 0 \\
0, & z<0 \\
\end{cases}$$

这时又被叫做感知器学习（perceptron learning）算法。

参考：

https://mp.weixin.qq.com/s/Y1_smqwmLPQzJ202JVq7zw

一文搞懂极大似然估计

https://mp.weixin.qq.com/s/MuV_kamfsUgcradKIaXGbw

逻辑回归（Logistic Regression）模型简介

https://mp.weixin.qq.com/s/YQ8l87EPw6EEhNy8MIU6Cg

区分识别机器学习中的分类与回归

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

其中，$$\log(\frac{\phi}{1-\phi})$$被称为Logit函数，从上面的推导可看出，它和sigmoid函数是有相当深的渊源的。

高斯分布到指数类分布的变换过程如下：

$$\begin{align}p(y;\mu)&=\frac{1}{\sqrt{2\pi}}\exp\left(-\frac{1}{2}(y-\mu)^2\right)
\\&=\frac{1}{\sqrt{2\pi}}\exp\left(-\frac{1}{2}y^2\right)\cdot\exp\left(\mu y-\frac{1}{2}\mu^2\right)\dot\
\end{align}$$

可见：

$$\begin{align}& \eta=\mu \\& T(y)=y \\& a(\eta)=\frac{\mu^2}{2} \\& b(y)=\frac{1}{\sqrt{2\pi}}\exp\left(-\frac{1}{2}y^2\right) \\\end{align}$$

除此之外，Dirichlet分布、Poisson分布、多项分布、$$\beta$$分布、$$\gamma$$分布都是指数类分布。

## 广义线性模型

广义线性模型（Generalized Linear Model，GLM）是解决指数类分布的回归问题的通用模型。

它的建模过程如下：

1.首先弄清楚y服从什么分布：

$$y\vert x;\theta \sim ExponentialFamily(\eta) \tag{1}$$

2.为参数$$\eta$$设置linear predictor：

$$\eta=\theta^Tx \tag{2}$$

公式2实际上就是通常意义上的线性模型，即simple linear model。

3.寻找一个合适的link function，将Y的均值映射到linear predictor上，使得：

$$g(h(x))=\eta \tag{3}$$

其中，$$h(x)=E[T(y)\vert x]$$

可见，上一节中的$$\eta$$实际上就是link function。

从上一节的推导还可看出，simple linear model对应的是高斯分布，而其他分布则需要link function进行扩展，这也是广义线性模型得名的由来。

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

>注：由于softmax函数给出的是分类结果的概率，因此对于某些分类结果中，所有类别概率都很低的情况，我们有理由认为遇到了未知的类别。softmax函数的这种概率可解释性，是它优于其他函数的地方。

$$h_\theta(x)=E[T(y)\vert x;\theta]=\begin{bmatrix} \phi_1 \\ \phi_2 \\ \vdots \\ \phi_{k-1} \end{bmatrix}
=\begin{bmatrix} \frac{\exp(\theta_1^Tx)}{\sum_{j=1}^k\exp(\theta_j^Tx)} \\ \frac{\exp(\theta_2^Tx)}{\sum_{j=1}^k\exp(\theta_j^Tx)} \\ \vdots \\ \frac{\exp(\theta_{k-1}^Tx)}{\sum_{j=1}^k\exp(\theta_j^Tx)} \end{bmatrix}$$

最大似然估计对数函数：

$$\ell(\theta)=\sum_{i=1}^m\log p(y^{(i)}\vert x^{(i)};\theta)=\sum_{i=1}^m\log\prod_{l=1}^k\left(\frac{\exp(\theta_{l}^Tx^{(i)})}{\sum_{j=1}^k\exp(\theta_j^Tx^{(i)})}\right)^{1\{y^{(i)}=l\}}$$

参考：

http://statmath.wu.ac.at/courses/heather_turner/

INTRODUCTION TO GENERALIZED LINEAR MODELS

https://mp.weixin.qq.com/s/jeloJDgfa3eFXUPduhesVA

logistic函数和softmax函数

## 机器学习的优化问题

优化理论和算法是机器学习用于处理问题的重要工具，但是机器学习有自己独特的看待问题的视角，并且其中也有很多和 Optimization 并不直接相关的部分，反过来Machine Learning也对Optimization产生影响。

例如，Interior Point Method的发明被认为是Optimization中的重要里程碑，这一类的方法能够保证在多项式次迭代内收敛。但是在机器学习中，特别是现在的所谓“大数据”的趋势下，这类算法却没法工作，一方面由于机器学习中所处理的数据通常维度非常高，从而相应的优化问题的变量个数变得很巨大，传统的方法虽然保证多项式迭代收敛，但是其中每一步迭代的计算代价却是随着变量个数的平方甚至三次方增长，结果是连算法的一次迭代都无法在可接受的时间内完成。于是（机器学习方面的）人们逐渐将注意力集中到主要基于first-order oracle的单次迭代计算量非常小的算法上。另一方面，数据点的个数的爆炸性增长也使得stochastic类的算法受到更多的关注——同样是降低单次迭代的计算复杂度。

# 生成学习算法

比如说，要确定一只羊是山羊还是绵羊。从历史数据中学习到模型，然后通过提取这只羊的特征，来预测出这只羊是山羊还是绵羊。这种方法叫做判别学习算法（DLA，Discriminative Learning Algorithm）。其形式化的写法是：$$p(y\vert x)$$。

换一种思路，我们可以根据山羊的特征首先学习出一个山羊模型，然后根据绵羊的特征学习出一个绵羊模型。然后从这只羊中提取特征，放到山羊模型中看概率是多少，再放到绵羊模型中看概率是多少，哪个大就是哪个。这种方法叫做生成学习算法（GLA，Generative Learning Algorithms）。其形式化的写法是：建立模型——$$p(x\vert y)$$，应用模型——$$p(y)$$。