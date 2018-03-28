---
layout: post
title:  深度学习（二十九）——迁移学习, Spiking Neuron Networks, DNC, OCR, CRNN
category: DL 
---

# VAE（续）

## VAE的另一个介绍

以下章节的内容主要摘自：

https://www.jeremyjordan.me/variational-autoencoders/

Variational autoencoders

该文中文版：

https://mp.weixin.qq.com/s/tRB85VF8XH9TTXZsiNVLhA

深入理解变分自编码器

自编码器是发现数据的一些隐状态（不完整，稀疏，去噪，收缩）表示的模型。 更具体地说，输入数据被转换成一个编码向量，其中每个维度表示从数据学到的属性。 最重要的是编码器为每个编码维度输出单个值， 解码器随后接收这些值并尝试重新创建原始输入。

变分自编码器（VAE）提供了描述隐空间观察的概率方式。 因此，我们不需要构建一个输出单个值来描述每个隐状态属性的编码器，而是要用编码器描述每个隐属性的概率分布。

举个例子，假设我们已经在一个大型人脸数据集上训练了一个Autoencoder模型, encoder的维度是6。理想情况下, 我们希望自编码器学习面部的描述性属性，比如肤色，人是否戴眼镜，从而能够用一些特征值来表示这些属性。

![](/images/img2/VAE_7.png)

在上面的示例中，我们使用单个值来描述输入图像的隐属性。 但是，我们其实更愿意用一个分布去表示每个隐属性。 比如, 输入蒙娜丽莎的照片，我们很难非常自信的为微笑属性分配一个具体值, 但是用了变分自编码器, 我们有能比较自信的说微笑属性服从什么分布。

![](/images/img2/VAE_8.png)

通过这种方法，我们现在将给定输入的每个隐属性表示为概率分布。 当从隐状态解码时，我们将从每个隐状态分布中随机采样，来生成向量作为解码器的输入。

![](/images/img2/VAE_9.png)

通过构造我们的编码器来输出一系列可能的值（统计分布），然后随机采样该值作为解码器的输入，我们能够学习到一个连续，平滑的隐空间。因此，在隐空间中彼此相邻的值应该与非常类似的重建相对应。而从隐分布中采样到的任何样本，我们都希望解码器理解, 并准确重构出来。

![](/images/img2/VAE_10.png)

我们可以进一步将此模型构造成神经网络架构：

![](/images/img2/VAE_11.png)

下图是VAE的结构图：

![](/images/img2/VAE_12.png)

Reparameterization Trick的图示：

![](/images/img2/VAE_13.png)

Reparameterization Trick的反向传播：

![](/images/img2/VAE_14.png)

## 数值计算 vs 采样计算

VAE的基本概念到此差不多了，苏剑林君趁热打铁又写了以下理论文章：

https://kexue.fm/archives/5343

变分自编码器：从贝叶斯观点出发

特将要点摘录如下。

对于不是很熟悉概率统计的读者，容易混淆数值计算和采样计算的概念。

已知概率密度函数p(x)，那么x的期望也就定义为：

$$\mathbb{E}[x] = \int x p(x)dx\tag{1}$$

如果要对它进行数值计算，也就是数值积分，那么可以选若干个有代表性的点$$x_0 < x_1 < \dots < x_n$$，然后得到：

$$\mathbb{E}[x] \approx \sum_{i=1}^n x_i p(x_i) \left(\frac{x_i - x_{i-1}}{x_n - x_0}\right)\tag{2}$$

如果从p(x)中采样若干个点$$x_1,x_2,\dots,x_n$$，那么我们有：

$$\mathbb{E}[x] \approx \frac{1}{n}\sum_{i=1}^n x_i,\quad x_i \sim p(x)\tag{3}$$

我们可以比较(2)跟(3)，它们的主要区别是(2)中包含了概率的计算而(3)中仅有x的计算，这是因为在(3)中$$x_i$$是从p(x)中依概率采样出来的，概率大的$$x_i$$出现的次数也多，所以可以说采样的结果已经包含了p(x)在里边，就不用再乘以$$p(x_i)$$了。

## 生成模型近似

对于二值数据，我们可以对decoder用sigmoid函数激活，然后用交叉熵作为损失函数，这对应于$$q(x\mid z)$$为伯努利分布；而对于一般数据，我们用MSE作为损失函数，这对应于$$q(x\mid z)$$为固定方差的正态分布。

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

https://kexue.fm/archives/5332

基于CNN和VAE的作诗机器人：随机成诗

# RBM



参考：

https://mp.weixin.qq.com/s/bCFoEN0-Ey2dGcsFGpmR8w

受限玻尔兹曼机基础教程

# 迁移学习

https://mp.weixin.qq.com/s/HmkTkv7QT08lGtJsHD7EvQ

迁移学习（Transfer Learning）技术概述

https://zhuanlan.zhihu.com/wjdml

《小王爱迁移》系列blog

https://mp.weixin.qq.com/s/BWBrso7O1O3Rfxa4QWZH4g

分分钟学会基于深度学习的图像真实风格迁移！

https://mp.weixin.qq.com/s/5DtTgc9bIrdXQkmuqRm8CA

谷歌大脑迁移学习：减少调参，直接在数据集中学习最佳图像架构

https://mp.weixin.qq.com/s/fEKc6yFZwTPAHjXJlcHA-w

香港科技大学提出L2T框架：学习如何迁移学习

https://mp.weixin.qq.com/s/pbyByPoZ9SVoP9B7pJMxXg

深度卷积网络迁移学习的脸部表情识别

https://www.zhihu.com/question/50996014

什么是One/zero-shot learning？

https://mp.weixin.qq.com/s/SZlFgnUBL0T6yNa-i_WLvg

领域适应性Domain Adaptation、One-shot/zero-shot Learning概述

https://mp.weixin.qq.com/s/sAf2fLLnKHOs433pV_6bSQ

One-shot Learning：孪生网络少样本精准分类

https://mp.weixin.qq.com/s/J8ZmIVKd-4X3hMGGIJWoDQ

一文看懂迁移学习：从基础概念到技术研究！

https://mp.weixin.qq.com/s/qYoTgqwjaUlEycuk9LlonA

迁移学习：6张图像vs13000张图像，超越2013 Kaggle猫狗识别竞赛领先水平

http://mp.weixin.qq.com/s/6Urv6TfUfc-BWV1YqTM1PQ

迁移学习+BPE，改进低资源语言的神经翻译结果

https://zhuanlan.zhihu.com/p/30242073

人脸识别中的迁移学习简介（Transfer Learning）

https://mp.weixin.qq.com/s/rVYWV-LsbmA4QhC6207SWA

14篇论文为你呈现“迁移学习”研究全貌

https://mp.weixin.qq.com/s/l-l1xbUaPNKc-w5XndjCbQ

通过网络结构迁移学习提高图像识别任务的拓展性

https://mp.weixin.qq.com/s/-KssC3yXsG3ZuV8-I6D_nQ

学习迁移架构用于Scalable图像的识别

https://mp.weixin.qq.com/s/NQED6DdCJNpNyzURUOZPnA

迁移学习：机器学习的下一个前沿阵地！

https://mp.weixin.qq.com/s/Hok9D8dAzYrBz7XoFmGE2A

AliExpress：在检索式问答系统中应用迁移学习

https://mp.weixin.qq.com/s/f_vB2AXCytnvoZaqfMeIpw

应用TF-Slim快速实现迁移学习

https://mp.weixin.qq.com/s/R1bKmhADfhQAZmhXL9ObiQ

多重预训练视觉模型的迁移学习

https://mp.weixin.qq.com/s/pDK4qBWArtETARE1fjbbmA

迁移学习在深度学习中的应用

https://mp.weixin.qq.com/s/mB1AEFVdM_s1rk0irST4Ww

迁移学习在图像分类中的简单应用策略

https://mp.weixin.qq.com/s/PDyp_GO0ovWV0KoGTwp_gQ

简述迁移学习在深度学习中的应用

https://mp.weixin.qq.com/s/FHmijTVqQ26osp6PzZsbvQ

付彦伟：零样本、小样本以及开集条件下的社交媒体分析

https://mp.weixin.qq.com/s/109hJaWsL4mcr9dc9vdDMg

结合主动学习与迁移学习：让医学图像标注工作量减少一半

https://mp.weixin.qq.com/s/A7PAu6-B1JRUfGmb2Fm_vA

中国科学院大学Oral论文：使用鉴别性特征实现零样本识别

# Spiking Neuron Networks

除了基于BP算法的NN之外，Spiking Neuron Networks也是一大类NN。Spiking NN和人脑结构更相似，功耗也更小，但是相关训练和数据量化的算法尚不成熟，属于潜力股。

参考：

https://homepages.cwi.nl/~sbohte/publication/paugam_moisy_bohte_SNNChapter.pdf

Computing with Spiking Neuron Networks

https://mp.weixin.qq.com/s/6dpKSaLFVo-ge4gtbG8GQg

简述脉冲神经网络SNN：下一代神经网络

# DNC

https://zhuanlan.zhihu.com/p/27773709

浅析至强RNN可微分神经计算机(DNC)

https://zhuanlan.zhihu.com/p/27964341

浅析至强RNN可微分神经计算机(DNC)-2

https://zhuanlan.zhihu.com/p/28209628

DNC-3滚动分类的模式识别

https://zhuanlan.zhihu.com/p/28433712

DNC4广义线性回归

# OCR

## tesseract

linux下可以使用tesseract作为OCR工具。安装方法：

`sudo apt install tesseract-ocr libtesseract-dev`

使用方法：

`tesseract ./111.png 1 -l chi_sim+eng`

## 参考

https://mp.weixin.qq.com/s/h7HVyGbmtLmNVJp4p0rCRQ

字符识别(OCR)相关工具/库/教材/论文等资源整理

https://zhuanlan.zhihu.com/p/21344595

端到端的OCR：验证码识别(LSTM+CTC)

http://www.jianshu.com/p/86489f1afd36

端到端的OCR：基于CNN的实现

http://www.jianshu.com/p/4fadf629895b

端到端的OCR：LSTM＋CTC的实现

https://mp.weixin.qq.com/s/axpA7Y_Rhiols5bDIdc6jg

Tesseract-OCR 3.0.1训练自己的语言库之图像文字识别

http://mp.weixin.qq.com/s/n8C80a3B54FhrCe-GhhcDA

文档扫描：深度神经网络在移动端的实践

https://mp.weixin.qq.com/s/MYhQt9uC16BadiZKWjPTzA

华中科技大学提出多向文本检测方法：基于角定位与区域分割

# CRNN

论文：

《An End-to-End Trainable Neural Network for Image-based Sequence Recognition and Its Application to Scene Text Recognition》

代码：

https://github.com/bgshih/crnn

