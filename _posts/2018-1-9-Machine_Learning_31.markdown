---
layout: post
title:  机器学习（三十一）——价值函数的近似表示, t-SNE
category: ML 
---

# 价值函数的近似表示

之前的内容都是讲解一些强化学习的基础理论，这些知识只能解决一些中小规模的问题，很多价值函数需要用一张大表来存储，获取某一状态或行为价值的时候通常需要一个查表操作（Table Lookup），这对于那些状态空间或行为空间很大的问题几乎无法求解。

在实际应用中，对于状态和行为空间都比较大的情况下，精确获得各种v(S)和q(s,a)几乎是不可能的。这时候需要找到近似的函数，具体可以使用线性组合、神经网络以及其他方法来近似价值函数：

$$\hat v(s,w)\approx v_\pi(s)\\
\hat q(s,a,w)\approx q_\pi(s,a)
$$

其中w是该近似函数的参数。

## 线性函数

这里仍然从最简单的线性函数说起：

$$\hat v(S,w)=x(S)^Tw=\sum_{j=1}^nx_j(S)w_j$$

目标函数为：

$$J(w)=E_\pi[(v_\pi(S)-x(S)^Tw)^2]$$

更新公式为：

$$\Delta w=\alpha (v_\pi(S)-\hat v(S,w))x(S)$$

上述公式都是基本的ML方法，这里不再赘述。既然是传统ML方法，自然少不了特征工程。

比如Table Lookup Features：

$$x^{table}(S)=\begin{pmatrix} 1(S=s_1) \\ \vdots \\ 1(S=s_n) \end{pmatrix}$$

则：

$$\hat v(S,w)=\begin{pmatrix} 1(S=s_1) \\ \vdots \\ 1(S=s_n) \end{pmatrix}\begin{pmatrix} w_1 \\ \vdots \\ w_n \end{pmatrix}$$

## Incremental Prediction Algorithms

事实上，之前所列的公式都不能直接用于强化学习，因为公式里都有一个实际价值函数$$v_\pi(S)$$，或者是一个具体的数值，而强化学习没有监督数据，因此不能直接使用上述公式。

因此，我们需要找到一个替代$$v_\pi(S)$$的目标。

|:--:|:--:|:--:|:--:|
| MC | $$\Delta w=\alpha (\color{red}{G_t}-\hat v(S_t,w))\nabla_w \hat v(S_t,w)$$ | 有噪声、无偏采样 | 收敛至一个局部最优解 |
| TD(0) | $$\Delta w=\alpha (\color{red}{R_{t+1}+\gamma\hat v(S_{t+1},w)}-\hat v(S_t,w))\nabla_w \hat v(S_t,w)$$ | 有噪声、有偏采样 | 收敛至全局最优解 |
| TD($$\lambda$$) | $$\Delta w=\alpha (\color{red}{G_t^\lambda}-\hat v(S_t,w))\nabla_w \hat v(S_t,w)$$ | 有噪声、有偏采样 |  |

上面公式中，红色的部分就是目标函数。

对于$$\hat q(S,A,w)$$，我们也有类似的结论：

|:--:|:--:|:--:|:--:|
| MC | $$\Delta w=\alpha (\color{red}{G_t}-\hat q(S_t,A_t,w))\nabla_w \hat q(S_t,A_t,w)$$ | 有噪声、无偏采样 | 收敛至一个局部最优解 |
| TD(0) | $$\Delta w=\alpha (\color{red}{R_{t+1}+\gamma\hat q(S_{t+1},A_{t+1},w)}-\hat q(S_t,A_t,w))\nabla_w \hat q(S_t,A_t,w)$$ | 有噪声、有偏采样 | 收敛至全局最优解 |
| TD($$\lambda$$) | $$\Delta w=\alpha (\color{red}{q_t^\lambda}-\hat q(S_t,A_t,w))\nabla_w \hat q(S_t,A_t,w)$$ | 有噪声、有偏采样 |  |

## Batch Methods

前面所说的递增算法都是基于数据流的，经历一步，更新算法后，我们就不再使用这步的数据了，这种算法简单，但有时候不够高效。与之相反，批方法则是把一段时期内的数据集中起来，通过学习来使得参数能较好地符合这段时期内所有的数据。这里的训练数据集“块”相当于个体的一段经验。

# t-SNE

## 概述

t-SNE(t-distributed stochastic neighbor embedding)是用于降维的一种机器学习算法，是由Laurens van der Maaten和Geoffrey Hinton在08年提出来。此外，t-SNE 是一种非线性降维算法，非常适用于高维数据降维到2维或者3维，进行可视化。

论文：

《Visualizing Data using t-SNE》

以下是几种常见的降维算法：

1.主成分分析（线性）

2.t-SNE（非参数/非线性）

3.萨蒙映射（非线性）

4.等距映射（非线性）

5.局部线性嵌入（非线性）

6.规范相关分析（非线性）

7.SNE（非线性）

8.最小方差无偏估计（非线性）

9.拉普拉斯特征图（非线性）

PCA的相关内容参见《机器学习（十六）》。

## SNE

在介绍t-SNE之前，我们首先介绍一下SNE（Stochastic Neighbor Embedding）的原理。

假设我们有数据集X，它共有N个数据点。每一个数据点$$x_i$$的维度为D，我们希望降低为d维。在一般用于可视化的条件下，d的取值为 2，即在平面上表示出所有数据。

SNE将数据点间的欧几里德距离转化为条件概率来表征相似性：

$$p_{j\mid i}=\frac{\exp(-\|x_i-x_j\|^2/2\sigma^2)}{\sum_{k\neq i}\exp(-\|x_i-x_k\|^2/2\sigma^2)}$$

如果以数据点在$$x_i$$为中心的高斯分布所占的概率密度为标准选择近邻，那么$$p_{j\mid i}$$就代表$$x_i$$将选择$$x_j$$作为它的近邻。对于相近的数据点，条件概率$$p_{j\mid i}$$是相对较高的，然而对于分离的数据点，$$p_{j\mid i}$$几乎是无穷小量（若高斯分布的方差$$\sigma_i$$选择合理）。

现在引入矩阵Y，Y是N*2阶矩阵，即输入矩阵X的2维表征。基于矩阵Y，我们可以构建一个分布q，其形式与p类似。

对于高维数据点$$x_i$$和$$x_j$$在低维空间中的映射点$$y_i$$和$$y_j$$，计算一个相似的条件概率$$q_{j\mid i}$$是可以实现的。我们将计算条件概率$$q_{i\mid j}$$中用到的高斯分布的方差设置为1/2。因此我们可以对映射的低维数据点$$y_j$$和$$y_i$$之间的相似度进行建模：

$$q_{j\mid i}=\frac{\exp(-\|y_i-y_j\|^2)}{\sum_{k\neq i}\exp(-\|y_i-y_k\|^2)}$$

我们的总体目标是选择Y中的一个数据点，然后其令条件概率分布q近似于p。这一步可以通过最小化两个分布之间的KL散度而实现，这一过程可以定义为：

$$C = \sum_i KL(P_i  \|  Q_i) = \sum_i \sum_j p_{j \mid i} \log \frac{p_{j \mid i}}{q_{j \mid i}}$$

这里的$$P_i$$表示了给定点$$x_i$$下，其他所有数据点的条件概率分布。需要注意的是**KL散度具有不对称性**，在低维映射中不同的距离对应的惩罚权重是不同的，具体来说：距离较远的两个点来表达距离较近的两个点会产生更大的cost，相反，用较近的两个点来表达较远的两个点产生的cost相对较小(注意：类似于回归容易受异常值影响，但效果相反)。即用较小的$$q_{j\mid i}=0.2$$来建模较大的$$p_{j\mid i}=0.8$$，$$cost=p\log(p/q)=1.11$$,同样用较大的$$q_{j\mid i}=0.8$$来建模较小的$$p_{j\mid i}=0.2$$,$$cost=-0.277$$。因此，**SNE会倾向于保留数据中的局部特征**。

## 如何确定$$\sigma$$

下面介绍一下SNE的超参$$\sigma$$的确定方法。

首先，不同的点具有不同的$$\sigma_i$$，$$P_i$$的熵(entropy)会随着$$\sigma_i$$的增加而增加。SNE使用困惑度(perplexity)的概念，用二分搜索的方式来寻找一个最佳的$$\sigma$$。其中困惑度指:

$$Perp(P_i) = 2^{H(P_i)}$$

这里的$${H(P_i)}$$是$$P_i$$的熵，即:

$$H(P_i) = -\sum_j p_{j \mid i} \log_2 p_{j \mid i}$$

困惑度可以解释为一个点附近的有效近邻点个数。SNE对困惑度的调整比较有鲁棒性，通常选择5-50之间。

在初始优化的阶段，每次迭代中可以引入一些高斯噪声，之后像模拟退火一样逐渐减小该噪声，可以用来避免陷入局部最优解。因此，SNE在选择高斯噪声，以及学习速率，什么时候开始衰减，动量选择等等超参数上，需要跑多次优化才可以。

## Symmetric SNE

优化$$p_{i \mid j}$$和$$q_{i \mid j}$$的KL散度的一种替换思路是，使用联合概率分布来替换条件概率分布，即P是高维空间里各个点的联合概率分布，Q是低维空间下的，目标函数为:

$$C = KL(P \mid  \mid Q) = \sum_i \sum_j p_{i,j} \log \frac{p_{ij}}{q_{ij}}$$

如果我们假设对于任意i，有$$p_{ij} = p_{ji}, q_{ij} = q_{ji}$$，则概率分布可以改写为:

$$p_{ij} = \frac{\exp(- \mid  \mid x_i - x_j \mid  \mid ^2 / 2\sigma^2)}{\sum_{k \neq l} \exp(- \mid  \mid x_k-x_l \mid  \mid ^2 / 2\sigma^2)}  \ \ \ \  q_{ij} = \frac{\exp(- \mid  \mid y_i - y_j \mid  \mid ^2)}{\sum_{k \neq l} \exp(- \mid  \mid y_k-y_l \mid  \mid ^2)}$$

我们将这种SNE称之为symmetric SNE(对称SNE)。


