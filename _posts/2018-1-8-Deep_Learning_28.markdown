---
layout: post
title:  深度学习（二十八）——VAE（2）
category: DL 
---

# VAE（续）

## VAE初现

其实，在整个VAE模型中，我们并没有去使用p(Z)（先验分布）是正态分布的假设，我们用的是假设$$p(Z\mid X)$$（后验分布）是正态分布！！

具体来说，给定一个真实样本$$X_k$$，我们假设存在一个专属于$$X_k$$的分布$$p(Z\mid X_k)$$（学名叫后验分布），并进一步假设这个分布是（独立的、多元的）正态分布。为什么要强调“专属”呢？因为我们后面要训练一个生成器$$X=g(Z)$$，希望能够把从分布$$p(Z\mid X_k)$$采样出来的一个$$Z_k$$还原为$$X_k$$。如果假设p(Z)是正态分布，然后从p(Z)中采样一个Z，那么我们怎么知道这个Z对应于哪个真实的X呢？现在$$p(Z\mid X_k)$$专属于$$X_k$$，我们有理由说从这个分布采样出来的Z应该要还原到$$X_k$$中去。

这样有多少个X就有多少个正态分布了。我们知道正态分布有两组参数：均值$$\mu$$和方差$$\sigma^2$$（多元的话，它们都是向量），那我怎么找出专属于$$X_k$$的正态分布$$p(Z\mid X_k)$$的均值和方差呢？好像并没有什么直接的思路。那好吧，就用神经网络拟合出来吧！

![](/images/img2/VAE_3.png)

于是我们构建两个神经网络$$\mu_k = f_1(X_k),\log \sigma^2 = f_2(X_k)$$来算它们了。我们选择拟合$$\log \sigma^2$$而不是直接拟合$$\sigma^2$$，是因为$$\sigma^2$$总是非负的，需要加激活函数处理，而拟合$$\log \sigma^2$$不需要加激活函数，因为它可正可负。到这里，知道专属于$$X_k$$的均值和方差，也就知道它的正态分布长什么样了，然后从这个专属分布中采样一个$$Z_k$$出来，经过一个生成器得到$$\hat{X}_k=g(Z_k)$$，现在我们可以放心地最小化$$\mathcal{D}(\hat{X}_k,X_k)^2$$，因为$$Z_k$$是从专属$$X_k$$的分布中采样出来的，这个生成器应该要把开始的$$X_k$$还原回来。

## 分布标准化

让我们来思考一下，根据上图的训练过程，最终会得到什么结果。

首先，我们希望重构X，也就是最小化$$\mathcal{D}(\hat{X}_k,X_k)^2$$，但是这个重构过程受到噪声的影响，因为$$Z_k$$是通过重新采样过的，不是直接由encoder算出来的。显然噪声会增加重构的难度，不过好在这个噪声强度（也就是方差）通过一个神经网络算出来的，所以最终模型为了重构得更好，肯定会想尽办法让方差为0。而方差为0的话，也就没有随机性了，所以不管怎么采样其实都只是得到确定的结果（也就是均值）。说白了，**模型会慢慢退化成普通的AutoEncoder，噪声不再起作用。**

为了使模型具有生成能力，VAE决定让所有的$$p(Z\mid X)$$都向标准正态分布看齐。如果所有的$$p(Z\mid X)$$都很接近标准正态分布$$\mathcal{N}(0,I)$$，那么根据定义：

$$p(Z)=\sum_X p(Z|X)p(X)=\sum_X \mathcal{N}(0,I)p(X)=\mathcal{N}(0,I) \sum_X p(X) = \mathcal{N}(0,I)$$

这样我们就能达到我们的先验假设：p(Z)是标准正态分布。然后我们就可以放心地从$$\mathcal{N}(0,I)$$中采样来生成图像了。

那怎么让所有的p(Z|X)都向$$\mathcal{N}(0,I)$$看齐呢？如果没有外部知识的话，其实最直接的方法应该是在重构误差的基础上中加入额外的loss：

## 参考

https://mp.weixin.qq.com/s/TqZnlXLKHhZn3U29PlqetA

变分自编码器VAE面临的挑战与发展方向

https://mp.weixin.qq.com/s/mtZ4_pwl8_GhitgImAU0VA

一文读懂什么是变分自编码器

https://mp.weixin.qq.com/s/LQFuXgI7uZK2UKRfZvlVbA

Variational AutoEncoder

https://mp.weixin.qq.com/s/lnSMdOk8fYfdU4aGeI5j7Q

未标注的数据如何处理？一文读懂变分自编码器VAE

https://zhuanlan.zhihu.com/p/27549418

花式解释AutoEncoder与VAE

https://mp.weixin.qq.com/s/ZlLuhu08m_RnD-h86df8sA

清华大学提出SA-VAE框架，通过单样本/少样本学习生成任意风格的汉字

https://mp.weixin.qq.com/s/t4YYIl4o_TAPG7737ZfiaA

面向无监督任务：DeepMind提出神经离散表示学习生成模型VQ-VAE

