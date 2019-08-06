---
layout: post
title:  深度学习（二十一）——图像超分辨率, SRCNN, DRCN
category: DL 
---

# Ultra Deep Network（续）

## Dual Path Networks

DPN是冯佳时和颜水成团队的Yunpeng Chen的作品。

>冯佳时，中国科学技术大学自动化系学士，新加坡国立大学电子与计算机工程系博士。现任新加坡国立大学电子与计算机工程系助理教授。

论文：

《Dual Path Networks》

代码：

https://github.com/cypw/DPNs

这篇论文首先从拓扑关系的角度分析了ResNet、DenseNet和HORNN（Higher Order RNN）之间的联系。

![](/images/img2/DPN_3.png)

如上所示，RNN相当于共享权值的串联的ResNet，而DenseNet则相当于并联的RNN。

更进一步的，上述三者都可表述为以下通式：

$$h^k=g^k\left[\sum_{t=0}^{k-1}f_t^k(h^t)\right]$$

其中，$$h^t$$表示t时刻的隐层状态；索引k表示当前时刻；$$x^t$$表示t时刻的输入；$$f_t^k(⋅)$$表示特征提取；$$g^k$$表示对提取特征做输出前的变换。

如果$$f_t^k(\cdot)$$和$$g^k(\cdot)$$每个Step都共享，那么就是HORNN，如果只有$$f_t^k(\cdot)$$共享，那么就是ResNet，两者都不共享，那就是DenseNet。

![](/images/img2/DPN.png)

上图展示的是ResNet和DenseNet的示意图。图中用线填充的柱状体，表示的是主干结点的tensor的大小。

ResNet由于跨层和主干之间是element-wise的加法运算，因此每个主干结点的tensor都是一样大的。

而DenseNet的跨层和主干之间是Concatenation运算，因此主干越往下，tensor越大。

通过上面的分析，我们可以认识到 ：

ResNet： 侧重于特征的再利用，但不善于发掘新的特征；

DenseNet: 侧重于新特征的发掘，但又会产生很多冗余；

为了综合二者的优点，作者设计了DPN网络：

![](/images/img2/DPN_2.png)

参考：

http://blog.csdn.net/scutlihaoyu/article/details/75645551

《Dual Path Networks》笔记

http://www.cnblogs.com/mrxsc/p/7693316.html

Dual Path Networks

http://blog.csdn.net/u014380165/article/details/75676216

DPN（Dual Path Network）算法详解

# 图像超分辨率

![](/images/img2/Super_Resolution.png)

如上图所示，一张低分辨率的小图（Low Resolution，LR）如果采用简单的插值算法进行图片放大的话，图像中物体的边缘会比较模糊。如何用算法将这种LR的图片放大成HR的图片，这就是Super Resolution（SR）的目标了。

SR目前主要有两个用途：

1.提升设备的测量精度。这个在天文和医疗图像方面用的比较多，比如Google和NASA利用AI探测太阳系外的行星，还有癌症的早期诊断。

2.Image Signal Processor。上面的两个应用比较高端，SR最主要的用途恐怕还是相机的ISP领域。ISP的基本概念参见《图像处理理论（五）》。

这里主要讨论DL在SR领域的应用。

## 前DL时代的SR

从信号处理的角度来说，LR之所以无法恢复成HR，主要在于丢失了图像的高频信息。（Nyquist采样定理）

>Harry Nyquist，1889~1976，University of North Dakota本硕（1914,1915）+耶鲁博士（1917）。AT&T贝尔实验室电子工程师。IEEE Medal of Honor获得者（1960）。

>IEEE Medal of Honor是IEEE的最高奖，除了1963年之外，每年只有1人得奖，个别年份甚至会轮空。

最简单的当然是《图像处理理论（二）》中提到的梯度锐化和拉普拉斯锐化，这种简单算法当然不要指望有什么好效果，聊胜于无而已。这是1995年以前的主流做法。

稍微复杂的方法，如同CV的其它领域经历了“信号处理->ML->DL”的变迁一样，SR也进入了ML阶段。

![](/images/img2/SR.png)

上图是两种典型的SR算法。

左图算法的中心思想是从图片中找出相似的大尺度区域，然后利用这个大区域的边缘信息进行SR。但这个方法对于那些只出现一次的边缘信息是没什么用的。

于是就有了右图的算法。对各种边缘信息建立一个数据库，使用时从数据库中挑一个最类似的边缘信息进行SR。这个方法比上一个方法好一些，但不够鲁棒，图片稍作改动，就有可能无法检索到匹配的边缘信息了。

ML时代的代表算法还有：

《Image Super-Resolution via Sparse Representation》

这篇论文是黄煦涛和马毅小组的Jianchao Yang的作品。

>黄煦涛（Thomas Huang），1936年生。生于上海，国立台湾大学本科（1956）+MIT硕博（1960,1963）。UIUC教授。美国工程院院士，中国科学院+中国工程院外籍院士。

>马毅，清华本科（1995）+UCB硕博（1997,2000）。UCB教授。IEEE fellow。   
>个人主页：   
>http://yima.csl.illinois.edu/

这篇论文提出的算法，在形式上和后文这些DL算法已经非常类似了，也是基于HR和LR配对的有监督训练。区别只在于这篇论文使用矩阵的稀疏表示来拟合SR函数，而DL算法使用神经网络拟合SR函数。前者是线性变换，而后者是非线性变换。

## 参考

https://mp.weixin.qq.com/s/93HfbJxG-5hFeEm1li-0tw

超分辨率相关资源大列表

https://mp.weixin.qq.com/s/0BNpiyIdLCjJlmTkRNNLrA

深度学习图像超分辨率最新综述：从模型到应用

https://zhuanlan.zhihu.com/p/25532538

深度学习在图像超分辨率重建中的应用

https://zhuanlan.zhihu.com/p/25201511

深度对抗学习在图像分割和超分辨率中的应用

https://mp.weixin.qq.com/s/uK0L5RV0bB2Jnr5WCZasfw

深度学习在单图像超分辨率上的应用：SRCNN、Perceptual loss、SRResNet

https://mp.weixin.qq.com/s/tg0Qnlf2WyG99J449KZTZQ

图像超分辨率重建--基础原理

https://zhuanlan.zhihu.com/p/76820438

基于深度学习的超分辨率图像技术一览

# SRCNN

SRCNN（Super-Resolution CNN）是汤晓鸥小组的Chao Dong的作品。

>汤晓鸥，中国科学技术大学本科（1990）+罗切斯特大学硕士（1991）+麻省理工学院博士（1996）。香港中文大学教授，商汤科技联合创始人。

论文：

《Learning a Deep Convolutional Network for Image Super-Resolution》

![](/images/article/SRCNN.jpg)

该方法对于一个低分辨率图像，先使用双三次（bicubic）插值将其放大到目标大小，再通过三层卷积网络做非线性映射，得到的结果作为高分辨率图像输出。作者将三层卷积的结构解释成与传统SR方法对应的三个步骤：图像块的提取和特征表示，特征非线性映射和最终的重建。

三个卷积层使用的卷积核的大小分为为9x9, 1x1和5x5，前两个的输出特征个数分别为64和32。

以下是论文的效果表格：

![](/images/img2/SRCNN.jpg)

>吐槽一下，这种表格属于论文必须有，但是却没什么营养的部分，且不乏造假的例子。原因很简单，一个idea，如果没有好效果，paper连发都发不了。但是，没有好效果的idea，未必没有价值，不说是否能启发人们的思维，至少能让后来者，不用再掉到同一个坑里。   
>比如化学领域，失败的实验远远多于成功的实验。在计算能力不发达的时代，人们主要关注成功的案例，但现在大家逐渐意识到：失败的案例才是更大的财富。

这里对其中的指标做一个简介。

**PSNR（Peak Signal to Noise Ratio，峰值信噪比）**

$$MSE=\frac{1}{H\times W}\sum_{i=1}^H\sum_{j=1}^W(X(i,j)-Y(i,j))^2$$

$$PSNR=10\log_{10}\left(\frac{(2^n-1)^2}{MSE}\right)$$

其中，MSE表示当前图像X和参考图像Y的均方误差（Mean Square Error），H、W分别为图像的高度和宽度；n为每像素的比特数，一般取8，即像素灰阶数为256. PSNR的单位是dB，数值越大表示失真越小。

虽然PSNR和人眼的视觉特性并不完全一致，但是一般认为PSNR在38以上的时候，人眼就无法区分两幅图片了。

**SSIM（structural similarity， 结构相似性）**，也是一种全参考的图像质量评价指标，它分别从亮度、对比度、结构三方面度量图像相似性。

$$\mu_X=\frac{1}{H\times W}\sum_{i=1}^H\sum_{j=1}^WX(i,j),\, \sigma_X^2=\frac{1}{H\times W}\sum_{i=1}^H\sum_{j=1}^W(X(i,j)-\mu_X)^2$$

$$\sigma_{XY}=\frac{1}{H\times W}\sum_{i=1}^H\sum_{j=1}^W((X(i,j)-\mu_X)(Y(i,j)-\mu_Y))$$

$$l(X,Y)=\frac{2\mu_X\mu_Y+C_1}{\mu_X^2+\mu_Y^2+C_1},\, c(X,Y)=\frac{2\sigma_X\sigma_Y+C_2}{\sigma_X^2+\sigma_Y^2+C_2},\, s(X,Y)=\frac{\sigma_{XY}+C_3}{\sigma_X\sigma_Y+C_3}$$

$$SSIM(X,Y)=l(X,Y)\cdot c(X,Y)\cdot s(X,Y)$$

$$C_1,C_2,C_3$$为常数，为了避免分母为0的情况，通常取$$C_1=(K_1\cdot L)^2, C_2=(K_2\cdot L)^2, C_3=C_2/2$$，一般地$$K1=0.01, K2=0.03, L=255$$。

SSIM取值范围[0,1]，值越大，表示图像失真越小。

在实际应用中，可以利用滑动窗将图像分块，令分块总数为N，考虑到窗口形状对分块的影响，采用高斯加权计算每一窗口的均值、方差以及协方差，然后计算对应块的结构相似度SSIM，最后将平均值作为两图像的结构相似性度量，即平均结构相似性MSSIM：

$$MSSIM(X,Y)=\frac{1}{N}\sum_{k=1}^NSSIM(x_k,y_k)$$

需要指出的是，PSNR和SSIM都是一些物理指标，它和人眼的视觉感受有一定的差异，不见得指标差的图就一定不如指标好的图（比如SRGAN）。

主观得分一般采用MOS（mean opinion score）作为评价指标。

参考：

http://blog.csdn.net/u011692048/article/details/77496861

超分辨率重建之SRCNN

http://www.cnblogs.com/vincent2012/archive/2012/10/13/2723152.html

PSNR和SSIM

https://mp.weixin.qq.com/s/hGum0fXrKUHCoaInVVS3rQ

除了MSE loss，也可以试试用它：SSIM的原理和代码实现

# DRCN

DRCN（deeply-recursive convolutional network）是韩国首尔国立大学的作品。

论文：

《Deeply-Recursive Convolutional Network for Image Super-Resolution》

SRCNN的层数较少，同时感受野也较小（13x13）。DRCN提出使用更多的卷积层增加网络感受野（41x41），同时为了避免过多网络参数，该文章提出使用递归神经网络（RNN）。网络的基本结构如下：

![](/images/img2/DRCN.jpg)

与SRCNN类似，该网络分为三个模块，第一个是Embedding network，相当于特征提取，第二个是Inference network, 相当于特征的非线性变换，第三个是Reconstruction network,即从特征图像得到最后的重建结果。其中的Inference network是一个递归网络，即数据循环地通过该层多次。将这个循环进行展开，就等效于使用同一组参数的多个串联的卷积层，如下图所示：

![](/images/img2/DRCN_2.jpg)
