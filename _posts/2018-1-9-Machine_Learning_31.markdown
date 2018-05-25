---
layout: post
title:  机器学习（三十一）——价值函数的近似表示, 数据不平衡问题, Linear Discriminant Analysis
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

# 数据不平衡问题

https://mp.weixin.qq.com/s/e0jXXCIhbaZz7xaCZl-YmA

如何处理不均衡数据？

https://mp.weixin.qq.com/s/2j_6hdq-MhybO_B0S7DRCA

如何解决机器学习中数据不平衡问题

https://mp.weixin.qq.com/s/gEq7opXLukWD5MVhw_buGA

七招教你处理非平衡数据

http://blog.csdn.net/u013709270/article/details/72967462

机器学习中的数据不平衡解决方案大全

https://mlr-org.github.io/mlr-tutorial/devel/html/over_and_undersampling/index.html

Imbalanced Classification Problems

https://mp.weixin.qq.com/s/QEHAV_rW25E0b0N7POr6tw

关于处理样本不平衡问题的Trick整理

https://mp.weixin.qq.com/s/5csfnBWZ2MQsnWZnNj9b8w

机器学习中样本比例不平衡的处理方法

https://mp.weixin.qq.com/s/ZL6UWrBB7qr8jp2QRA1MAQ

方法总结：教你处理机器学习中不平衡类问题

https://mp.weixin.qq.com/s/V5d3kbpXBf4883TQ_sq37A

遇到有这六大缺陷的数据集该怎么办？这有一份数据处理急救包

https://mp.weixin.qq.com/s/zLgD8DjnW1DfeqL_xITisQ

教你如何用python解决非平衡数据建模

# Linear Discriminant Analysis

在《机器学习（十七）》中，我们已经讨论了一个LDA，这里我们来看看另一个LDA。

Linear Discriminant Analysis是Ronald Fisher于1936年提出的方法，因此又叫做Fisher's linear discriminant。正如之前在《知名数据集》中提到的，Iris flower Data Set也是出自该论文。

之前我们讨论的PCA、ICA也好，对样本数据来言，可以是没有类别标签y的。回想我们做回归时，如果特征太多，那么会产生不相关特征引入、过度拟合等问题。我们可以使用PCA来降维，但**PCA没有将类别标签考虑进去，属于无监督的**。

比如回到上次提出的文档中含有“learn”和“study”的问题，使用PCA后，也许可以将这两个特征合并为一个，降了维度。但假设我们的类别标签y是判断这篇文章的topic是不是有关学习方面的。那么这两个特征对y几乎没什么影响，完全可以去除。

再举一个例子，假设我们对一张100*100像素的图片做人脸识别，每个像素是一个特征，那么会有10000个特征，而对应的类别标签y仅仅是0/1值，1代表是人脸。这么多特征不仅训练复杂，而且不必要特征对结果会带来不可预知的影响，但我们想得到降维后的一些最佳特征（与y关系最密切的），怎么办呢？

给定特征为d维的N个样例$$x^{(i)}\{x_1^{(i)},x_2^{(i)},\dots,x_d^{(i)}\}$$，其中有$$N_1$$个样例属于类别$$w_1$$，另外$$N_2$$个样例属于类别$$w_2$$。

现在我们觉得原始特征数太多，想将d维特征降到只有一维，而又要保证类别能够“清晰”地反映在低维数据上，也就是这一维就能决定每个样例的类别。

我们将这个最佳的向量称为w（d维），那么样例x（d维）到w上的投影可以用下式来计算：

$$y=w^Tx$$

这里得到的y值不是0/1值，而是x投影到直线上的点到原点的距离。

![](/images/img2/LDA.jpg)

我们首先看看x是二维的情况，从直观上来看，右图比较好，可以很好地将不同类别的样本点分离。这实际上就是LDA的思想：**最大化类间方差与最小化类内方差，即减少分类内部之间的差异，而扩大不同分类之间的差异。**

接下来我们从定量的角度来找到这个最佳的w。

首先我们寻找每类样例的均值（中心点），这里i只有两个：

$$\mu_i=\frac{1}{N_i}\sum_{x\in \omega_i}x$$

由于x到w投影后的样本点均值为：

$$\tilde{\mu_i}=\frac{1}{N_i}\sum_{y\in \omega_i}y=\frac{1}{N_i}\sum_{y\in \omega_i}w^Tx=w^T\mu_i$$

由此可知，投影后的的均值也就是样本中心点的投影。

什么是最佳的直线（w）呢？我们首先发现，能够使投影后的两类样本中心点尽量分离的直线是好的直线，定量表示就是：

$$J(w)=\mid \tilde{\mu_1}-\tilde{\mu_2} \mid=\mid w^T(\mu_1-\mu_2) \mid$$

J(w)越大越好。

但是只考虑J(w)行不行呢？不行，看下图：

![](/images/img2/LDA_2.png)

样本点均匀分布在椭圆里，投影到横轴$$x_1$$上时能够获得更大的中心点间距J(w)，但是由于有重叠，$$x_1$$不能分离样本点。投影到纵轴$$x_2$$上，虽然J(w)较小，但是能够分离样本点。因此我们还需要考虑样本点之间的方差，方差越大，样本点越难以分离。

我们使用另外一个度量值，称作散列值（scatter），对投影后的类求散列值，如下：

$$\tilde{s_i}^2=\sum_{y\in \omega_i}(y-\tilde{\mu_i})^2$$

从公式中可以看出，只是少除以样本数量的方差值，散列值的几何意义是样本点的密集程度，值越大，越分散，反之，越集中。

而我们想要的投影后的样本点的样子是：不同类别的样本点越分开越好，同类的越聚集越好，也就是均值差越大越好，散列值越小越好。正好，我们可以使用J(w)和S来度量，最终的度量公式是：

$$J(w)=\frac{\mid \tilde{\mu_1}-\tilde{\mu_2} \mid}{\tilde{s_1}^2+\tilde{s_2}^2}$$

接下来的事就比较明显了，我们只需寻找使J(w)最大的w即可。

先把散列值公式展开：

$$\tilde{s_i}^2=\sum_{y\in \omega_i}(y-\tilde{\mu_i})^2=\sum_{x\in \omega_i}(w^Tx-w^T\mu_i)^2=\sum_{x\in \omega_i}w^T(x-\mu_i)(x-\mu_i)^Tw$$

我们定义上式中间部分：

$$S_i=\sum_{x\in \omega_i}(x-\mu_i)(x-\mu_i)^T$$

这也被称为散列矩阵（scatter matrices）。

我们继续定义：

$$S_W=S_1+S_2$$

$$S_W$$称为**Within**-class scatter matrix。

$$S_B=(\mu_1-\mu_2)(\mu_1-\mu_2)^T$$

$$S_B$$称为**Between**-class scatter matrix。

那么J(w)最终可以表示为：

$$J(w)=\frac{w^TS_Bw}{w^TS_Ww}$$

在我们求导之前，需要对分母进行归一化，因为不做归一化的话，w扩大任何倍，公式都成立，我们就无法确定w。因此，我们打算令$$\|w^TS_Ww\|=1$$，那么加入拉格朗日乘子后，求导：

$$c(w)=w^TS_Bw-\lambda(w^TS_Ww-1)\Rightarrow \frac{\text{d}c}{\text{d}w}=2S_Bw-2\lambda S_Ww=0\\
\Rightarrow S_Bw=\lambda S_Ww\Rightarrow S_W^{-1}S_Bw=\lambda w$$

可见，w实际上就是矩阵$$S_W^{-1}S_B$$的特征向量。

因为：

$$S_Bw=(\mu_1-\mu_2)(\mu_1-\mu_2)^Tw$$

其中，后面两项的积是一个常数，记做$$\lambda_w$$，则：

$$S_W^{-1}S_Bw=S_W^{-1}(\mu_1-\mu_2)\lambda_w=\lambda w$$

由于对w扩大缩小任何倍不影响结果，因此可以约去两边的未知常数$$\lambda,\lambda_w$$，得到：

$$w=S_W^{-1}(\mu_1-\mu_2)$$

至此，我们只需要求出原始样本的均值和方差就可以求出最佳的方向w。

![](/images/img2/LDA_3.png)

上述结论虽然来自2维，但对于多维也是成立的。大特征值所对应的特征向量分割性能最好。由于$$S_W^{-1}S_B$$不一定是对称阵，因此得到的K个特征向量不一定正交，这也是与PCA不同的地方。

![](/images/img2/LDA_4.jpg)

**PCA选择样本点投影具有最大方差的方向，LDA选择分类性能最好的方向。**

