---
layout: post
title:  深度学习（九）——GAN
category: DL 
---

# CNN进化史

## 总结（续）

前深度学习时代的人脸识别的标准流程：

第一步是人脸检测，其结果就是在图片中的脸部区域打一个矩形框。

第二步是寻找眼睛、鼻子、嘴等特征点，目的是把脸对齐，也就是把眼鼻嘴放在近乎相同的位置，好像所有的脸都能“串成一串”一样，且只保留脸的中心区域，甚至连头发不要。

第三步是光照的预处理，通过高斯平滑、直方图均衡化等来进行亮度调节、偏光纠正等。

第四步是做Y=f(X)的变换。

第五步，是计算两张照片得到的Y的相似程度，如果超过特定的阈值，就认定是同一个人。

深度学习的到来对整个流程有一个巨大的冲击。

一开始，研究者用深度学习完成人脸检测、特征点定位、预处理、特征提取和识别等每个独立的步骤。而后首先被砍掉的是预处理，我们发现这个步骤是完全不必要的。理论上来解释，深度学习学出来的底层滤波器本身就可以完成光照的预处理，而且这个预处理是以“识别更准确”为目标进行的，而不是像原来的预处理一样，以“让人看得更清楚”为目标。人的知识和机器的知识其实是有冲突的，人类觉得好的知识不一定对机器识别有利。

而最近的一些工作就是把第二步特征点定位砍掉。因为神经网络也可以进行对齐变换，所以我们的工作通过空间变换（spatial transform），将图片自动按需进行矫正。并且我有一个猜测：传统的刻意把非正面照片转成正面照片的做法，也未必是有利于识别的。因为一个观察结果是，同一个人的两张正面照相似度可能小于一张正面、一张稍微转向的照片的相似度。最终，我们希望进行以识别为目标的对齐（recognition oriented alignment）。

在未来，或许检测和识别也可能合二为一。现在的检测是对一个通用的人脸的检测，未来或许可以实现检测和识别全部端到端完成：只有特定的某个人脸出现，才会触发检测框出现。

第五步的相似度（或距离测度）的计算方法存在一定的争议。我认为特征提取的过程已经通过损失函数暗含了距离测度的计算，所以深度特征提取与深度测度学习有一定的等价性。但也有不少学者在研究特征之间距离测度的学习，乃至于省略掉特征提取，直接学习输入两张人脸图片时的距离测度。

总体来说，深度学习的引入体现了端到端、数据驱动的思想：**尽可能少地对流程进行干预、尽可能少地做人为假设。**

## 参考

http://mp.weixin.qq.com/s/ZKMi4gRfDRcTxzKlTQb-Mw

计算机视觉识别简史：从AlexNet、ResNet到Mask RCNN

http://mp.weixin.qq.com/s/kbHzA3h-CfTRcnkViY37MQ

详解CNN五大经典模型:Lenet，Alexnet，Googlenet，VGG，DRL

https://zhuanlan.zhihu.com/p/22094600

Deep Learning回顾之LeNet、AlexNet、GoogLeNet、VGG、ResNet

https://mp.weixin.qq.com/s/28GtBOuAZkHs7JLRVLlSyg

深度卷积神经网络演化历史及结构改进脉络

http://www.leiphone.com/news/201609/303vE8MIwFC7E3DB.html

Google最新开源Inception-ResNet-v2，借助残差网络进一步提升图像分类水准

https://mp.weixin.qq.com/s/x3bSu9ecl3dldCbvS1rT1g

站在巨人的肩膀上，深度学习的9篇开山之作

http://mp.weixin.qq.com/s/2TUw_2d36uFAiJTkvaaqpA

解读Keras在ImageNet中的应用：详解5种主要的图像识别模型

https://zhuanlan.zhihu.com/p/27642620

YJango的卷积神经网络——介绍

https://www.zybuluo.com/coolwyj/note/202469

ImageNet Classification with Deep Convolutional Neural Networks

http://simtalk.cn/2016/09/20/AlexNet/

AlexNet简介

http://simtalk.cn/2016/09/12/CNNs/

CNN简介

https://mp.weixin.qq.com/s/I94gGXXW_eE5hSHIBOsJFQ

无需数学背景，读懂ResNet、Inception和Xception三大变革性架构

https://mp.weixin.qq.com/s/ToogpkDo-DpQaSoRoalnPg

没看过这5个模型，不要说你玩过CNN!

https://zhuanlan.zhihu.com/p/31006686

从LeNet-5到DenseNet

https://mp.weixin.qq.com/s/L9JY875UjXMv_slAakBrGA

从AlexNet到残差网络，理解卷积神经网络的不同架构

http://www.sohu.com/a/222873093_651893

从LeNet到SENet——卷积神经网络回顾

https://mp.weixin.qq.com/s/-d4T2hgjy6kGdd_ig_J9eg

LeCun亲授的深度学习入门课：从飞行器的发明到卷积神经网络

https://mp.weixin.qq.com/s/z26bXb8eAelrwj6Tkvm_-A

卷积神经网络常见架构AlexNet、ZFNet、VGGNet、GoogleNet和ResNet模型的理论与实践

# GAN

## 概况

GAN是“生成对抗网络”（Generative Adversarial Networks）的简称，由2014年还在蒙特利尔读博士的Ian Goodfellow引入深度学习领域。

>注：Ian J. Goodfellow，斯坦福大学本硕+蒙特利尔大学博士。导师是Yoshua Bengio。现为Google研究员。   
>个人主页：   
>http://www.iangoodfellow.com/

论文：

《Generative Adversarial Nets》

教程：

http://www.iangoodfellow.com/slides/2016-12-04-NIPS.pdf

## 通俗解释

对于GAN来说，最通俗的解释就是**“伪造者-鉴别者”**的解释，如艺术画的伪造者和鉴别者。一开始伪造者和鉴别者的水平都不高，但是鉴别者还是比较容易鉴别出伪造者伪造出来的艺术画。但随着伪造者对伪造技术的学习后，其伪造的艺术画会让鉴别者识别错误；或者随着鉴别者对鉴别技术的学习后，能够很简单的鉴别出伪造者伪造的艺术画。这是一个双方不断学习技术，以达到最高的伪造和鉴别水平的过程。

从上面的解释可以看出，GAN实际上一种**零和游戏上的无监督算法**。

![](/images/article/GAN.png)

## 基本原理

上面的解释虽然通俗，却并未涉及算法的实现。要实现上述原理，至少要解决三个问题：

**1.什么是伪造者。**

**2.什么是鉴别者。**

**3.如何对抗。**

以下文章的组织顺序，主要参考下文：

http://kexue.fm/archives/4439/

互怼的艺术：从零直达WGAN-GP

老规矩，摘要+点评。

## 伪造者

伪造者在这里实际上是一种Generative算法。伪造的内容是：**将随机噪声映射为我们所希望的正样本**。

随机噪声我们一般定义为**均匀分布**，于是上面的问题可以转化为：**如何将均匀分布X映射为正样本分布Y**。

首先，我们思考一个简单的问题：如何将$$U[0,1]$$映射为$$N(0,1)$$？

理论上的做法是：将$$X∼U[0,1]$$经过函数$$Y=f(X)$$映射之后，就有$$Y∼N(0,1)$$了。设$$\rho(x)$$是$$U[0,1]$$是概率密度函数，那么$$[x,x+dx]$$和$$[y,y+dy]$$这两个区间的概率应该相等，而根据概率密度定义，$$\rho(x)$$不是概率，$$\rho(x)dx$$才是概率，因此有：

$$\rho(x)dx=\frac{1}{\sqrt{2\pi}}\exp\left(-\frac{y^2}{2}\right)dy$$

即：

$$\int_{0}^x \rho(t)dt=\int_{-\infty}^{y}\frac{1}{\sqrt{2\pi}}\exp\left(-\frac{t^2}{2}\right)dt=\Phi(y)$$

其中，$$\Phi(y)$$是标准正态分布的累积分布函数，所以

$$y=\Phi^{-1}\left(\int_0^x \rho(t)dt\right)$$

注意到累积分布函数是无法用初等函数显式表示出来的，更不用说它的逆函数了。说白了，$$Y=f(X)$$的f的确是存在的，但很复杂，以上解只是一个记号，该算的还是要用计算机算。

正态分布是常见的、相对简单的分布，但这个映射已经这么复杂了。如果换了任意分布，甚至概率密度函数都不能显式写出来，那么复杂度可想而知～

考虑到我们**总可以用一个神经网络来拟合任意函数**。这里不妨用一个带有多个参数的神经网络$$G(X,\theta)$$去拟合f？只要把参数$$\theta$$训练好，就可以认为$$Y=G(X,\theta)$$了。这里的G是**Generator**的意思。

## 正样本分布

如上所述，一般的正样本分布是很难给出概率密度函数的。然而，我们可以换个角度思考问题。

假设有一批服从某个指定分布的数据$$Z=(z_1,z_2,\dots,z_N)$$，根据概率论的相关定义，我们至少可以使用**离散采样**的方法，根据Z中的样本分布，来近似求出Z的指定分布。下文如无特殊指出，**均以Z中的样本分布来代替Z的指定分布，简称Z的分布**。

那么接着就有另一个问题：**如何评估$$G(X,\theta)$$生成的样本的分布和Z的分布之间的差异呢？**

## KL散度

比较两个分布的差异的最常用指标是KL散度。其定义参见《机器学习（八）》。

## JS散度

因为KL散度不是对称的，有时候将它对称化，即得到JS散度（Jensen–Shannon divergence）：

$$JS\Big(p_1(x),p_2(x)\Big)=\frac{1}{2}KL\Big(p_1(x)\|p_2(x)\Big)+\frac{1}{2}KL\Big(p_2(x)\|p_1(x)\Big)$$

>注：Claude Elwood Shannon，1916～2001，美国数学家，信息论之父。密歇根大学双学士+MIT博士。先后供职于贝尔实验室和MIT。

KL散度和JS散度，也是Ian Goodfellow在原始GAN论文中，给出的评价指标。

虽然KL散度和JS散度，在这里起着距离的作用，但它们**不是距离**，它们不满足距离的三角不等式，因此只能叫“散度”。

## 神经距离

假设我们可以将实数域分成若干个不相交的区间$$I_1,I_2,\dots,I_K$$，那么就可以估算一下给定分布Z的概率分布：

$$p_z(I_i)=\frac{1}{N}\sum_{j=1}^{N}\#(z_j\in I_i)$$

其中$$\#(z_j\in I_i)$$表示如果$$z_j\in I_i$$，那么取值为1，否则为0。

接着我们生成M个均匀随机数$$x_1,x_2,\dots,x_M$$（这里不一定要$$M=N$$，还是那句话，我们比较的是分布，不是样本本身，因此多一个少一个样本，对分布的估算也差不了多少。），根据$$Y=G(X,\theta)$$计算对应的$$y_1,y_2,\dots,y_M$$，然后根据公式可以计算：

$$p_y(I_i)=\frac{1}{M}\sum_{j=1}^{M}(y_j\in I_i)$$

现在有了$$p_z(I_i)$$和$$p_y(I_i)$$，那么我们就可以算它们的差距了，比如可以选择JS距离

$$\text{Loss} = JS\Big(p_y(I_i), p_z(I_i)\Big)$$

假如我们只研究单变量概率分布之间的变换，那上述过程完全够了。然而，很多真正有意义的事情都是多元的，比如在MNIST上做实验，想要将随机噪声变换成手写数字图像。要注意MNIST的图像是28*28=784像素的，假如每个像素都是随机的，那么这就是一个784元的概率分布。按照我们前面分区间来计算KL距离或者JS距离，哪怕每个像素只分两个区间，那么就有$$2^{784}\approx 10^{236}$$个区间，这是何其巨大的计算量！
