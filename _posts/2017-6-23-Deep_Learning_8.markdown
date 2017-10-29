---
layout: post
title:  深度学习（八）——GAN（2）
category: DL 
---

# GAN（续）

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

## GAN的理论解释

顾险峰教授对GAN提出了自己的理论解释。

>顾险峰，1989年考入清华大学计算机科学与技术系。1992年获得清华大学特等奖学金。后于美国哈佛大学获得计算机博士学位，师从国际著名微分几何大师丘成桐先生。目前为美国纽约州立大学石溪分校计算机系终身教授。

参考：

https://mp.weixin.qq.com/s/7O0AKIUVYK7HRyvdRbUVkg

虚构的对抗，GAN with the wind

https://mp.weixin.qq.com/s/trvMOTXNs7L6fSmTkZXwsA

看穿机器学习（W-GAN模型）的黑箱

https://mp.weixin.qq.com/s/thcxsBVttSIEzVNLQlAVCA

看穿机器学习的黑箱（II）

https://mp.weixin.qq.com/s/Jx0o17CwlIVcRV22PXk4wQ

看穿机器学习的黑箱（III）

## 参考

https://mp.weixin.qq.com/s/xa3F3kCprE6DEQclas4umg

GAN的数学原理

http://www.jianshu.com/p/e2d2d7cbbe49

50行代码实现GAN

https://mp.weixin.qq.com/s/YnOF9CCUFvtaiTY8HXYOuw

深入浅出：GAN原理与应用入门介绍

http://blog.csdn.net/u011534057/article/category/6396518

GAN系列blog

https://mp.weixin.qq.com/s/4CypEZscTfmUzOk-p_rZog

生成对抗网络初学入门：一文读懂GAN的基本原理

http://mp.weixin.qq.com/s/bzwG0QxnP2drqS4RwcZlBg

微软详解：到底什么是生成式对抗网络GAN？

https://mp.weixin.qq.com/s/GobKiuxgZv0-ufSRBpTcIA

Ian Goodfellow ICCV2017演讲：解读GAN的原理与应用

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

https://zhuanlan.zhihu.com/p/28342644

CycleGAN的原理与实验详解

https://mp.weixin.qq.com/s/YXWTslQXIKVihBb2Bgtafg

GAN在信息检索领域的应用

http://mp.weixin.qq.com/s/21CN4hAA6p7ZjWsO1sT2rA

一文看懂生成式对抗网络GANs：介绍指南及前景展望

https://mp.weixin.qq.com/s/YLys6L9WT7eCC-xGr1j0Iw

带多分类判别器的GAN模型

https://mp.weixin.qq.com/s/0tTLotV-8w2j3VdkH-qjCQ

让机器告诉你故事的结局应该是什么：利用GAN进行故事型常识阅读理解

https://mp.weixin.qq.com/s/lqQeCpLQVqSdJPWx0oxs2g

例解生成对抗网络

https://mp.weixin.qq.com/s/fMtuJbWG_d9zyCZ0oYyX_w

经得住考验的“假图片”：用TensorFlow为神经网络生成对抗样本

https://zhuanlan.zhihu.com/p/28488946

AI可能真的要代替插画师了……

https://mp.weixin.qq.com/s/OXN8Y5truLeslX8-UWgqmg

宅男的福音：用GAN自动生成二次元萌妹子

https://mp.weixin.qq.com/s/LAS0KgPiVekGdQXbqlw1cQ

深度学习的三大生成模型：VAE、GAN、GAN的变种

https://mp.weixin.qq.com/s/N7YU-YeXiVX7gSB-mzYgnw

生成式对抗网络GAN的研究进展与展望

https://mp.weixin.qq.com/s/gDzti2DISq_cwGbP5T7ICQ

聊聊对抗自编码器

https://mp.weixin.qq.com/s/3Aq1HXpBzgNdcB130tCKbQ

GAN网络图像翻译机：图像复原、模糊变清晰、素描变彩图

https://mp.weixin.qq.com/s/mPtv1fQd0NBgdY2b_ALNTQ

机器之心GitHub项目：GAN完整理论推导与实现，Perfect！

https://mp.weixin.qq.com/s/NFqTpSXtFdP43MBeb3Ovrw

GAN眼中的图像翻译

http://blog.csdn.net/amds123/article/details/70199708

生成对抗网络GAN最近在NLP领域有哪些应用？

https://mp.weixin.qq.com/s/uUSq3irEIcBM35JCYGDPfw

生成对抗网络综述：从架构到训练技巧，看这篇论文就够了

https://mp.weixin.qq.com/s/nuT6Glyx0-tU7WJyoPji9w

GANs正在多个层面有所突破

https://zhuanlan.zhihu.com/p/30532830

眼见已不为实，迄今最真实的GAN：Progressive Growing of GANs

https://mp.weixin.qq.com/s/d_W0O7LNqlBuZV87Ou9uqw

训练GAN的16个技巧



