---
layout: post
title:  机器学习（十八）——独立成分分析, 压缩感知
category: ML 
---

* toc
{:toc}

# 主成分分析（续）

## Other

常见的降维算法还有：

https://www.cnblogs.com/lochan/p/6627511.html

数据降维之多维缩放MDS（Multiple Dimensional Scaling）

https://mp.weixin.qq.com/s/cfeILnMsWlMC_T6lcSEW7A

图像降维之MDS特征抽取方法

https://mp.weixin.qq.com/s/C-tZRvHKcpO5jQArZi_GQA

数据降维算法-从PCA到LargeVis

https://mp.weixin.qq.com/s/HGBB1RLr5eux9xLtXJpokg

哈工大硕士生用Python实现了11种经典数据降维算法，源代码库已开放

PCA还可用于升维：

https://www.cnblogs.com/lochan/p/7011831.html

核化主成分分析（Kernel PCA）应用及调参

https://zhuanlan.zhihu.com/p/59644996

Kernel Principal Component Analysis(KPCA核主成分分析)

## 参考

https://mp.weixin.qq.com/s/tJ_FbL2nFQfkvKqpQJ8kmg

从特征分解到协方差矩阵：详细剖析和实现PCA算法

https://mp.weixin.qq.com/s/dDdyaA7Nxqa8tBE_qQ80Dw

典型相关性分析(CCA)详解

https://mp.weixin.qq.com/s/JDWgw3OOdBurDAShrPHJ7Q

从最大方差来看主成分分析PCA

https://mp.weixin.qq.com/s/ZDipXGPOxKhxtAx2Dc9RjA

主成分分析（PCA）以及在图像上的应用

https://mp.weixin.qq.com/s/9-nNNhhDWSYWy46u0hTazQ

降维：PCA真的能改善分类结果吗？

https://mp.weixin.qq.com/s/vkBSextwFQv8-DUwAxgVyA

图像降维之Isomap特征抽取方法

https://zhuanlan.zhihu.com/p/78193297

PCA和SVD的联系和区别？

https://mp.weixin.qq.com/s/Uj9AFbyFRO6jIBoC3Gy8nA

小孩都看得懂的主成分分析

https://mp.weixin.qq.com/s/N-JtuayRYRrZ-_P67u7rvA

如何使用PCA去除数据集中的多重共线性

# 独立成分分析

这一节我们将讲述独立成分分析（Independent Components Analysis，ICA）算法。

首先，我们介绍一下经典的鸡尾酒宴会问题(cocktail party problem)。

假设在party中有n个人，他们可以同时说话，我们也在房间中放置了n个声音接收器(Microphone)用来记录声音。宴会过后，我们从n个麦克风中得到了m组数据$$x^{(i)}$$，其中的i表示采样的时间顺序。由于宴会上人们的说话声是混杂在一起的，因此，采样得到的声音也是混杂不清的，那么我们是否有办法从混杂的数据中，提取出每个人的声音呢？

为了更为正式的描述这个问题，我们假设数据$$s\in R^n$$是由n个独立的源生成的。我们接收到的信号可写作：$$x=As$$。其中，A被称为混合矩阵（mixing matrix）。在这个问题中，$$s^{(i)}$$是一个n维向量，$$s_j^{(i)}$$表示第j个说话者在i时刻的声音。同理，$$x_j^{(i)}$$表示第j个麦克风在i时刻的记录下的数据。

我们把$$W=A^{-1}$$称作unmixing matrix。我们的目标就是找到W，然后利用$$s=Wx$$，求得s。我们使用$$w_i^T$$表示W矩阵的第i行，因此：$$s_j^{(i)}=w_j^Tx^{(i)}$$。

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

这里的推导参见[《机器学习（二）》](/ml/2016/08/02/Machine_Learning_2.html#LG)的公式7。

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

# 压缩感知

https://blog.csdn.net/jbb0523

一个压缩感知+贝叶斯网络方面的blog

http://blog.csdn.net/abcjennifer/article/details/7721834

初识压缩感知Compressive Sensing

http://blog.csdn.net/abcjennifer/article/details/7724360

中国压缩传感资源（China Compressive Sensing Resources）

http://blog.csdn.net/xiahouzuoxin/article/details/38820925

白话压缩感知（含Matlab代码）

http://blog.csdn.net/abcjennifer/article/details/7748833

压缩感知进阶——有关稀疏矩阵

https://zhuanlan.zhihu.com/p/85558304

深度学习压缩感知（DCS）历史最全资源汇总分享

## Robust PCA

http://www.cnblogs.com/quarryman/p/robust_pca.html

最优化之Robust PCA

http://www.aiuxian.com/article/p-2634727.html

Robust PCA

http://blog.csdn.net/abcjennifer/article/details/8572994

Robust PCA学习笔记

http://patternrecognition.cn/~jin/gs/seminar/20140515_jinzhong.ppt

Robust PCA-模式识别

# 关联规则挖掘

## 基本概念

关联规则挖掘（Association rule mining）是机器学习的一个子领域。它最早的案例就是以下的**尿布和啤酒**的故事：

>沃尔玛曾今对数据仓库中一年多的原始交易数据进行了详细的分析，发现与尿布一起被购买最多的商品竟然是啤酒。   
>借助数据仓库和关联规则，发现了这个隐藏在背后的事实：**美国妇女经常会嘱咐丈夫下班后为孩子买尿布，而30%~40%的丈夫在买完尿布之后又要顺便购买自己爱喝的啤酒。**   
>根据这个发现，沃尔玛调整了货架的位置，把尿布和啤酒放在一起销售，大大增加了销量。

这里借用一个引例来介绍关联规则挖掘的基本概念。

| 交易号TID | 顾客购买的商品 | 交易号TID | 顾客购买的商品 |
|:--:|:--|:--:|:--|
| T1 | bread, cream, milk, tea | T6 | bread, tea |
| T2 | bread, cream, milk | T7 | beer, milk, tea |
| T3 | cake, milk | T8 | bread, tea |
| T4 | milk, tea | T9 | bread, cream, milk, tea |
| T5 | bread, cake, milk | T10 | bread, milk, tea |

**定义一**：设$$I=\{i_1,i_2,\dots,i_m\}$$，是m个不同的项目的集合，每个$$i_k$$称为一个**项目**。项目的集合I称为**项集**。其元素的个数称为项集的长度，长度为k的项集称为k-项集。引例中每个商品就是一个项目，项集为$$I=\{bread, beer, cake,cream, milk, tea\}$$，I的长度为6。

**定义二**：每笔**交易T**是项集I的一个子集。对应每一个交易有一个唯一标识交易号，记作TID。交易全体构成了**交易数据库D**，$$\mid D\mid$$等于D中交易的个数。引例中包含10笔交易，因此$$\mid D\mid=10$$。
