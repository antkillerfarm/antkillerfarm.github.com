---
layout: post
title:  机器学习（十八）——独立成分分析, 压缩感知
category: ML 
---

* toc
{:toc}

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

# 经济学+

交叉违约是基于英美法系的一种特殊制度设计。它的含义是：如果本合同项下的债务人在其他贷款合同项下出现违约，则也视为对本合同的违约。

通俗地类比就是：张三分别向李四、王五借债。一旦张三和王五的借款合同出现了问题，哪怕与李四之间的合同没有违约，我李四也有权以违约论处，要求张三提前还款或者追加担保。

这在大陆法系背景的国家是比较难以理解的（也是不太可能出现的合同条款）：所谓“一码归一码”，只要你我之间的债券没有出现纠纷，本息还在按时支付，为什么你可以基于其他人的违约前提让我加速还款？你这属于“用前朝的尚方宝剑斩本朝的官”啊。

但在英美法系之下，基于“预期违约”概念的交叉违约条款，在解读和应用上就自然多了：只要你发生了同类违约事件，我就有权避免“后下手遭殃”的悲剧，使自己不至于落到比其他债权人更不利的境地。

---

长期以来，期货的价格就不是被垄断着的，而是由法定供求关系所决定的。任何操纵手段都不能把股价一直压低并始终使之保持低价。因虚构的理由来支持或反对期货市场的某种趋势，成功是暂时性的，因为最终获胜的是事实。 -- by Jesse Livermore

---

https://mp.weixin.qq.com/s/_rO9I2M-504oZdnGQVsRcQ

消费金融相关术语(一)

https://mp.weixin.qq.com/s/SJyoMAedEOlSS2m28-OUlA

消费金融相关术语(二)

https://mp.weixin.qq.com/s/k9W1UGQ0brdgPtC764Pd8w

消费金融相关术语(三)

https://mp.weixin.qq.com/s/5-TxzL3dSXNFoPQKKn07uw

为什么可乐十多年了还是三元一瓶？

https://mp.weixin.qq.com/s/Int88nDhtePtr8TgEx-nmw

基金大起大落，“养鸡”怎么赚钱？

https://zhuanlan.zhihu.com/p/45535482

《金融投资400年》之南海公司泡沫---300年前的那场股票雪崩

https://mp.weixin.qq.com/s/2JjE-RmNNEAH_lfIEugJlA

冯小刚赔偿2.35亿，开心的笑了起来

https://mp.weixin.qq.com/s/b8AJEHBlaXminxHVjhr-8Q

不为人知的国家级别的剥削

https://mp.weixin.qq.com/s/W53CCYhfNKVhzA1mBhG_MA

美国陷入严重通胀，滥印美元的报应来了

https://mp.weixin.qq.com/s/MKoDWfoq_h06VsJMIl-39Q

五百年来的白银贸易和全球化

https://www.zhihu.com/question/56864369

为什么白银越来越稀有而价格却这么低？

https://www.zhihu.com/question/23055810

为什么很多创始人要用另一个公司来持股自己的公司呢？

https://mp.weixin.qq.com/s/oGRO2PpcVqw3Bg1QN2a8qA

美联储加息，为什么会让我们的钱变少？

https://mp.weixin.qq.com/s/ruLnbQTck9TYS1q402d6fg

世界三大泡沫之一的日本债市，下跌熔断了

https://zhuanlan.zhihu.com/p/339996249

怎么查询自己开设的证券账户
