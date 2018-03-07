---
layout: post
title:  机器学习（三十一）——价值函数的近似表示, Linear Discriminant Analysis
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

# Linear Discriminant Analysis

在《机器学习（十七）》中，我们已经讨论了一个LDA，这里我们来看看另一个LDA。

Linear Discriminant Analysis是Ronald Fisher于1936年提出的方法，因此又叫做Fisher's linear discriminant。正如之前在《知名数据集》中提到的，Iris flower Data Set也是出自该论文。

之前我们讨论的PCA、ICA也好，对样本数据来言，可以是没有类别标签y的。回想我们做回归时，如果特征太多，那么会产生不相关特征引入、过度拟合等问题。我们可以使用PCA来降维，但PCA没有将类别标签考虑进去，属于无监督的。

比如回到上次提出的文档中含有“learn”和“study”的问题，使用PCA后，也许可以将这两个特征合并为一个，降了维度。但假设我们的类别标签y是判断这篇文章的topic是不是有关学习方面的。那么这两个特征对y几乎没什么影响，完全可以去除。

再举一个例子，假设我们对一张100*100像素的图片做人脸识别，每个像素是一个特征，那么会有10000个特征，而对应的类别标签y仅仅是0/1值，1代表是人脸。这么多特征不仅训练复杂，而且不必要特征对结果会带来不可预知的影响，但我们想得到降维后的一些最佳特征（与y关系最密切的），怎么办呢？

给定特征为d维的N个样例$$x^{(i)}\{x_1^{(i)},x_2^{(i)},\dots,x_d^{(i)}\}$$，其中有$$N_1$$个样例属于类别$$w_1$$，另外$$N_2$$个样例属于类别$$w_2$$。

现在我们觉得原始特征数太多，想将d维特征降到只有一维，而又要保证类别能够“清晰”地反映在低维数据上，也就是这一维就能决定每个样例的类别。

我们将这个最佳的向量称为w（d维），那么样例x（d维）到w上的投影可以用下式来计算：

$$y=w^Tx$$

这里得到的y值不是0/1值，而是x投影到直线上的点到原点的距离。

![](/images/img2/LDA.jpg)

我们首先看看x是二维的情况，从直观上来看，右图比较好，可以很好地将不同类别的样本点分离。

接下来我们从定量的角度来找到这个最佳的w。

首先我们寻找每类样例的均值（中心点）：

$$\mu_i=$$

对LDA稍加扩展就得到了《图像处理理论（一）》中的Otsu法。

参考：

https://mp.weixin.qq.com/s/u-6nPrb4r9AS2gtrl5s-FA

LDA(Linear Discriminant Analysis)算法介绍

http://www.cnblogs.com/jerrylead/archive/2011/04/21/2024384.html

线性判别分析（Linear Discriminant Analysis）（一）

http://www.cnblogs.com/jerrylead/archive/2011/04/21/2024389.html

线性判别分析（Linear Discriminant Analysis）（二）



