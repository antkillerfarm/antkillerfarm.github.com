---
layout: post
title:  机器学习（十七）——隐式狄利克雷划分
category: ML 
---

## ICA的不确定性

不幸的是，在没有源和混合矩阵的先验知识的情况下，仅凭$$x^{(i)}$$是没有办法求出W的。为了说明这一点，我们引入置换矩阵的概念。

置换矩阵（permutation matrix）是一种元素只由0和1组成的方块矩阵。置换矩阵的每一行和每一列都恰好只有一个1，其余的系数都是0。它的例子如下：

$$P=\begin{bmatrix}0 & 1 & 0 \\ 1 & 0 & 0 \\ 0 & 0 & 1 \end{bmatrix};
P=\begin{bmatrix}0 & 1 \\ 1 & 0 \end{bmatrix};
P=\begin{bmatrix}1 & 0 \\ 0 & 1 \end{bmatrix}$$

在线性代数中，每个n阶的置换矩阵都代表了一个对n个元素（n维空间的基）的置换。当一个矩阵乘上一个置换矩阵时，所得到的是原来矩阵的横行（置换矩阵在左）或纵列（置换矩阵在右）经过置换后得到的矩阵。

ICA的不确定性(ICA ambiguities)包括以下几种情形：

1.无法区分W和WP。比如改变说话人的编号，会改变$$s^{(i)}$$的值，但却不会改变$$x^{(i)}$$的值，因此也就无法确定$$s^{(i)}$$的值了。

2.无法确定W的尺度。比如$$x^{(i)}$$还可以写作$$x^{(i)}=2A \cdot (0.5)s^{(i)}$$，因此在不知道A的情况下，同样无法确定$$s^{(i)}$$的值。

3.信号不能是高斯分布的。

假设两个人发出的声音信号符合多值正态分布$$s\sim \mathcal{N}(0,I)$$，这里的I是一个2阶单位阵，则$$E[xx^T]=E[Ass^TA^T]=AA^T$$。

假设R是正交矩阵，$$A'=AR,x'=A's$$，则：

$$E[xx^T]=E[A'ss^T(A')^T]=E[ARss^T(AR)^T]=ARR^TA^T=AA^T$$

可见，无论是A还是A'，观测值x都是一个$$\mathcal{N}(0,AA^T)$$的正态分布，也就是说A的值无法确定，那么W和s也就无法求出了。

## 密度函数和线性变换

在讨论ICA的具体算法之前，我们先来回顾一下概率和线性代数里的知识。

假设我们的随机变量s有概率密度（probability density）函数$$p_s(s)$$。为了简单，我们再假设s是实数，还有一个随机变量$$x=As$$，A和x都是实数。令$$p_x$$是x的概率密度，那么怎么求$$p_x$$呢？

令$$W=A^{-1}$$，则$$s=Wx$$。然而$$p_x(x)\neq p_s(Wx)$$。

这里以均匀分布（Uniform）为例讨论一下。令$$s\sim \text{Uniform}[0,1]$$，则$$p_s(s)=1$$。令$$A=2$$，则$$W=0.5$$，$$x=2s\sim \text{Uniform}[0,2]$$，因此$$p_x(x)=p_s(Wx)\lvert W \rvert$$。

## 累积分布函数

累积分布函数（cumulative distribution function，CDF）是概率论中的一个基本概念。它的定义如下：

$$F(z_0)=P(z\le z_0)=\int_{-\infty}^{z_0}p_z(z)\mathrm{d}z$$

可以看出：

$$p_z(z)=F'(z)$$

## ICA算法

ICA算法归功于Bell 和 Sejnowski，这里使用最大似然估计来解释算法。（原始论文中使用的是一个复杂的方法Infomax principal，这在最新的推导中已经不需要了。）

>注：Terrence (Terry) Joseph Sejnowski，1947年生，美国科学家。普林斯顿大学博士，导师是神经网络界的大神John Hopfield。ICA算法和Boltzmann machine的发现人。

>Tony Bell的个人主页：
>http://cnl.salk.edu/~tony/index.html

我们假定每个$$s_i$$有概率密度$$p_s$$，那么给定时刻原信号的联合分布就是：

$$p(s)=\prod_{i=1}^np_s(s_i)$$

因此：

$$p(x)=\prod_{i=1}^np_s(w_i^Tx)\cdot \mid W\mid \tag{2}$$

为了确定$$s_i$$的概率密度，我们首先要确定它的累计分布函数$$F(x)$$。而这需要满足两个性质：单调递增和在$$[0,1]$$区间。

我们发现sigmoid函数很适合，它的定义域负无穷到正无穷，值域0到1，缓慢递增。因此，可以假定s的累积分布函数符合sigmoid函数：

$$g(s)=\frac{1}{1+e^{-s}}$$

求导，可得：

$$p_s(s)=g'(s)=g(s)(1-g(s))$$

这里的推导参见《机器学习（一）》的公式7。

>注：如果有其他先验信息的话，这里的$$g(s)$$也可以使用其他函数。否则的话，sigmoid函数能够在大多数问题上取得不错的效果。

公式2的对数似然估计函数为：

$$\ell(W)=\sum_{i=1}^m\left(\sum_{j=1}^m\log g'(w_j^Tx^{(i)})+\log\mid W\mid \right)\tag{3}$$

因为：

$$\begin{align}
(\log g'(s))'&=(\log [g(s)(1-g(s))])'=(\log g(s))'+(\log (1-g(s)))'
\\&=\frac{g'(s)}{g(s)}+\frac{(1-g(s))'}{(1-g(s))}=\frac{g(s)(1-g(s))}{g(s)}-\frac{g(s)(1-g(s))}{(1-g(s))}
\\&=1-2g(s)
\end{align}$$

又因为《机器学习（十）》的公式5.11，可得公式3的导数为：

$$\nabla_W\ell(W)=\begin{bmatrix}
1-2g(w_1^Tx^{(i)}) \\
1-2g(w_2^Tx^{(i)}) \\
\vdots \\
1-2g(w_n^Tx^{(i)})
\end{bmatrix}x^{(i)^T}+(W^T)^{-1}$$

最后，用通常的随机梯度上升算法，求得$$\ell(W)$$的最大值即可。

>注意：我们计算最大似然估计时,假设了$$x^{(i)}$$和$$x^{(j)}$$之间是独立的，然而对于语音信号或者其他具有时间连续依赖特性(比如温度)上，这个假设不能成立。但是在数据足够多时，假设独立对效果影响不大。

# 隐式狄利克雷划分

Latent Dirichlet Allocation，简称LDA。注意不要和Linear Discriminant Analysis搞混了。

这方面的文章，首推rickjin（靳志辉）写的《LDA数学八卦》一文。全文篇幅长达55页，我实在没有能力写的比他更好，因此这里就做一个摘要好了。

>注：靳志辉，北京大学计算机系计算语言所硕士，日本东京大学情报理工学院统计自然语言处理方向博士。2008年加入腾讯，主要工作内容涉及统计自然语言处理和大规模并行机器学习工具的研发工作。目前为腾讯社交与效果广告部质量研发中心总监，主要负责腾讯用户数据挖掘、精准广告定向、广告语义特征挖掘、广告转化率预估等工作。   
>他写的另一篇文章《正态分布的前世今生》，也是统计界著名的科普文，非常值得一看。

## 马氏链及其平稳分布

Markov chain的定义如下：

$$P(X_{t+1}=x\mid X_t, X_{t-1}, \cdots) =P(X_{t+1}=x\mid X_t)$$

即状态转移的概率只依赖于前一个状态的随机过程。

>注：上面是1阶Markov过程的定义。类似的还可以定义n阶Markov过程，即状态转移的概率只依赖于前n个状态的随机过程。

>Andrey(Andrei) Andreyevich Markov，1856~1922，俄国数学家。圣彼得堡大学博士，导师Pafnuty Chebyshev。最早研究随机过程的数学家之一。圣彼得堡学派的第二代领军人物，俄罗斯科学院院士。   
>虽然现在将随机过程（stochastic process）划为数理统计科学的一部分，然而在19世纪末期，相关的研究者在学术界分属两个不同的团体。其中最典型的就是英国的剑桥学派（Pearson、Fisher等）和俄国的圣彼得堡学派（Chebyshev、Markov等）。因此将Markov称为统计学家是非常错误的观点。

一些非周期的马氏链，经过若干次的状态转移之后，其状态概率会收敛到一个特定的数值，即平稳分布（stationary distribution）。

如各参考文献所示，这里一般会举社会学上的人口分层问题，引出马氏链的极限和平稳分布的概念。这里要特别注意马氏链收敛定理以及它的具体含义。

**细致平稳条件**：如果非周期马氏链的转移矩阵P和分布$$\pi(x)$$满足

$$\pi(i)P_{ij} = \pi(j)P_{ji} \quad\quad \text{for all} \quad i,j$$

则$$\pi(x)$$是马氏链的平稳分布，上式被称为细致平稳条件(detailed balance condition)。

满足细致平稳条件的马氏链，其后续状态都是平稳分布状态，不会再改变。

>注:细致平稳条件，比平稳分布的条件要强，因此它是平稳分布的充分条件，而非必要条件。

参考：

http://blog.csdn.net/lanchunhui/article/details/50451620

重温马尔科夫随机过程

http://blog.csdn.net/pipisorry/article/details/46618991

马尔科夫模型

## MCMC

对于给定的概率分布$$p(x)$$，我们希望能有便捷的方式生成它对应的样本。由于马氏链能收敛到平稳分布，于是一个很的漂亮想法是：如果我们能构造一个转移矩阵为P的马氏链，使得该马氏链的平稳分布恰好是$$p(x)$$，那么我们从任何一个初始状态$$x_0$$出发沿着马氏链转移，得到一个转移序列$$x_0, x_1, x_2, \cdots x_n, x_{n+1}\cdots$$，如果马氏链在第n步已经收敛了，于是我们就得到了$$\pi(x)$$的样本$$x_n, x_{n+1}\cdots$$。

Markov Chain Monte Carlo算法的核心是引入一个参数$$\alpha(i,j)$$使得一个普通的马氏链，变成一个满足细致平稳条件的马氏链。即：

$$p(i) \underbrace{q(i,j)\alpha(i,j)}_{Q'(i,j)} 
= p(j) \underbrace{q(j,i)\alpha(j,i)}_{Q'(j,i)}  \quad$$

以上即为Metropolis算法。

>注：Nicholas Constantine Metropolis，1915~1999，希腊裔美籍物理学家。芝加哥大学博士，反复供职于Los Alamos和芝加哥大学。（其实也就这俩地方，只不过这边干几年到那边去，那边教几年书再回这边来，这么进进出出好几个来回而已）“曼哈顿计划”的主要科学家之一，战后主持MANIAC计算机的研制。

$$\alpha$$的大小，决定了马氏链的收敛速度。$$\alpha$$越大，收敛越快。因此又有Metropolis–Hastings算法，其关键公式为：

$$\alpha(i,j) = \min\left\{\frac{p(j)q(j,i)}{p(i)q(i,j)},1\right\}$$

>注：Wilfred Keith Hastings，1930~2016，美国统计学家，多伦多大学博士，维多利亚大学教授。

## Gibbs Sampling

这个算法虽然以Gibbs命名，但却是Geman兄弟于1984年研究Gibbs random field时，发现的算法。

>注：Josiah Willard Gibbs，1839~1903，美国科学家。他在物理、化学和数学方面都有重大理论贡献。耶鲁大学博士和教授。统计力学的创始人。

>Donald Jay Geman，1943年生，美国数学家。美国西北大学博士，布朗大学教授。随机森林算法的早期研究者之一。

>Stuart Alan Geman，1949年生，美国数学家。MIT博士，约翰霍普金斯大学教授。美国科学院院士。最早将Markov random field（MRF）应用于机器视觉和机器学习。

因为高维空间中，沿坐标轴方向上的两点之间的转移，满足细致平稳条件。因此，Gibbs Sampling的核心就是沿坐标轴循环迭代采样，该算法收敛之后的采样点即符合指定概率分布。

这里需要特别指出的是，Gibbs Sampling比Metropolis–Hastings算法高效的原因在于：Gibbs Sampling每次沿坐标轴的转移是必然会被接受的，即$$\alpha=1$$。

## Unigram Model

假设我们的词典中一共有V个词$$v_1,v_2,\cdots v_V$$，那么最简单的Unigram Model是定义一个V面的骰子，每抛一次骰子，抛出的面就对应产生一个词。

频率学派的Unigram Model如下图：

![](/images/article/unigram-model.jpg)

贝叶斯学派的Unigram Model如下图：

![](/images/article/dirichlet-multinomial-unigram.jpg)

这里使用Dirichlet分布的原因在于，数据采用多项分布，而Dirichlet分布正好是多项分布的共轭先验分布。

$$Dir(\overrightarrow{p}\mid \overrightarrow{\alpha})+MultCount(\overrightarrow{n})=Dir(\overrightarrow{p}\mid \overrightarrow{\alpha}+\overrightarrow{n})$$

