---
layout: post
title:  深度学习（七）—— GAN
category: theory 
---

# GAN

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

为此，我们用神经网络L定义距离：

$$L\Big(\{y_i\}_{i=1}^M, \{z_i\}_{i=1}^N, \Theta\Big)$$

其中，$$\Theta$$为神经网络的参数。

对于特定的任务来说，$$\{z_i\}_{i=1}^N$$是给定的，并非变量，因此上式可简写成：

$$L\Big(\{y_i\}_{i=1}^M, \Theta\Big)$$

通常，我们采用如下的L实现：

$$L=\frac{1}{M}\sum_{i=1}^M D\Big(y_i,\Theta\Big)$$

上式可以简单的理解为：**分布之间的距离，等于单个样本的距离的平均**。

这里的神经网络$$D(Y,\Theta)$$，实际上就是GAN的另一个主角——**鉴别者**。这里的D是**Discriminator**的意思。

## 如何对抗

因为$$D(Y,\Theta)$$的均值，也就是L，是度量两个分布的差异程度，这就意味着，L要能够将两个分布区分开来，即L越大越好；但是我们最终的目的，是希望通过均匀分布而生成我们指定的分布，所以$$G(X,\theta)$$则希望两个分布越来越接近，即L越小越好。

形式化的描述就是：

$$\arg \min_G \max_D V(G,D)$$

具体的做法是：

### Step1

随机初始化$$G(X,\theta)$$，固定它，然后生成一批Y，这时候我们要训练$$D(Y,\Theta)$$，既然L代表的是“与指定样本Z的差异”，那么，如果将指定样本Z代入L，结果应该是越小越好，而将Y代入L，结果应该是越大越好，所以

$$\begin{aligned}\Theta =& \mathop{\arg\min}_{\Theta} L = \mathop{\arg\min}_{\Theta} \frac{1}{N}\sum_{i=1}^N D\Big(z_i,\Theta\Big)\\ 
\Theta =& \mathop{\arg\max}_{\Theta} L = \mathop{\arg\max}_{\Theta} \frac{1}{M}\sum_{i=1}^M D\Big(y_i,\Theta\Big)\end{aligned}$$

然而有两个目标并不容易平衡，所以干脆都取同样的样本数B（一个batch），然后一起训练就好：

$$\begin{aligned}\Theta =& \mathop{\arg\min}_{\Theta} L_1\\ 
=&\mathop{\arg\min}_{\Theta} \frac{1}{B}\sum_{i=1}^B\left[D\Big(z_i,\Theta\Big)-D\Big(y_i,\Theta\Big)\right]\end{aligned}$$

### Step2

$$G(X,\theta)$$希望它生成的样本越接近真实样本越好，因此这时候把$$\Theta$$固定，只训练$$\theta$$让L越来越小：

$$\begin{aligned}\theta =& \mathop{\arg\min}_{\theta} L_2\\ 
=&\mathop{\arg\min}_{\theta} \frac{1}{B}\sum_{i=1}^B\left[D\Big(G(x_i,\theta),\Theta\Big)\right]\end{aligned}$$

## Lipschitz约束

稍微思考一下，我们就发现，问题还没完。我们目前还没有对D做约束，不难发现，无约束的话Loss基本上会直接跑到负无穷去了～

最简单的方案就是采用Lipschitz约束：

$$\| D(y,\theta) - D(y' , \theta) \| \leq C \|y-y'\|$$

也可写作：

$$\left\| \frac{\partial D(y,\Theta)}{\partial y}\right\| \leq C$$

>注：Rudolf Otto Sigismund Lipschitz，1832～1903，德国数学家，先后就读于柯尼斯堡大学和柏林大学，导师Dirichlet。波恩大学教授。

## WGAN

KL散度和JS散度由于不是距离，数学特性并不够好。因此，Martín Arjovsky于2017年1月，提出了Wasserstein GAN。

其中的一项改进就是使用Wasserstein距离替代KL散度和JS散度。Wasserstein距离的定义参看《机器学习（二十）》。

WGAN极大程度的改善了GAN训练困难的问题，成为当前GAN研究的主流。

参考：

https://zhuanlan.zhihu.com/p/25071913

令人拍案叫绝的Wasserstein GAN

## GAN的发展

最早的GAN出现在2014年6月，但直到2015年底，也只有5个变种，发展并不迅速。

2016年，GAN开始发力，年底时已有52个变种。2017年6月底，更达到142个变种。

![](/images/article/GAN_structure.png)

上图的源地址：

https://github.com/hwalsuklee/tensorflow-generative-model-collections

参考：

https://github.com/hindupuravinash/the-gan-zoo

GAN的各种变种。

## 参考

https://mp.weixin.qq.com/s/xa3F3kCprE6DEQclas4umg

GAN的数学原理

http://www.jianshu.com/p/e2d2d7cbbe49

50行代码实现GAN

https://mp.weixin.qq.com/s/YnOF9CCUFvtaiTY8HXYOuw

深入浅出：GAN原理与应用入门介绍

http://blog.csdn.net/u011534057/article/category/6396518

GAN系列blog

http://mp.weixin.qq.com/s/bzwG0QxnP2drqS4RwcZlBg

微软详解：到底什么是生成式对抗网络GAN？

https://mp.weixin.qq.com/s/oCDlhzjOYTIhsr5JuoRCJQ

IRGAN：大一统信息检索模型的博弈竞争

https://mp.weixin.qq.com/s/QacQCrjh3KmrQSMp-G_rEg

贝叶斯生成对抗网络

https://zhuanlan.zhihu.com/p/24897387

GAN的基本原理、应用和走向

https://mp.weixin.qq.com/s/E28lA-fcAQ6Sp6Qv64H3TQ

GAN in NLP

https://mp.weixin.qq.com/s/7-oHa-8Q8ThcctaVOZFfew

Facebook创意生成网络CAN，比GAN更有创造力

https://mp.weixin.qq.com/s/aSQ2-QxbToGF0ROyjxw2yw

萌物生成器：如何使用四种GAN制造猫图

https://mp.weixin.qq.com/s/YUMIL-f019vKpQ84mKS-8g

这篇TensorFlow实例教程文章告诉你GANs为何引爆机器学习？

http://mp.weixin.qq.com/s/UkZdUcdz7h4DqcyjSbNncw

zi2zi：用条件生成对抗网络玩转中文书法，绝妙汉字字体自动生成

http://blog.csdn.net/v_JULY_v/article/details/52683959

没GPU也能玩梵高作画：Ubuntu tensorflow CPU版

https://github.com/cysmith/neural-style-tf

TensorFlow (Python API) implementation of Neural Style.这个项目实现了两张图片的画风融合，非常牛。

https://github.com/jinfagang/pytorch_style_transfer

这个和上面的一样，不过是用pytorch实现的。

http://mp.weixin.qq.com/s/zNmJuevHaagKbyGFdKTwoQ

tensorflow实现基于深度学习的图像补全

https://zhuanlan.zhihu.com/p/25204020

条条大路通罗马LS-GAN：把GAN建立在Lipschitz密度上

https://zhuanlan.zhihu.com/p/27199954

用GAN去除动作片中的马赛克和衣服

https://zhuanlan.zhihu.com/p/27012520

从头开始GAN

https://mp.weixin.qq.com/s/Qzlg1MzRT3josy2RJpQSVg

Image to Image Translation Using GAN

https://mp.weixin.qq.com/s/AswdyjPeKbX7yhAPloP2og

基于对抗学习的生成式对话模型

https://mp.weixin.qq.com/s/uyn41vKKoptXPZXBP2vVDQ

生成对抗网络（GAN）之MNIST数据生成

https://mp.weixin.qq.com/s/sxa0BfXtylHXzjq0YBn-Kg

伯克利图像迁移cycleGAN，猫狗互换效果感人

https://mp.weixin.qq.com/s/aMfPBl6E5SxckQdSAGTkBg

Pytorch教程：Facebook发布的LR-GAN如何生成图像？

