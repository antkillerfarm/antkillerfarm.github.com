---
layout: post
title:  深度学习（二十二）——VESPCN, SRGAN, DemosaicNet, SVDF, LCNN
category: DL 
---

# VESPCN（续）

Motion estimation这个过程可以通过传统的光流算法来计算，DeepMind提出了一个Spatial Transformer Networks, 通过CNN来估计空间变换参数。VESPCN使用了这个方法，并且使用多尺度的Motion estimation：先在比输入图像低的分辨率上得到一个初始变换，再在与输入图像相同的分辨率上得到更精确的结果，如下图所示：

![](/images/img2/VESPCN_2.png)

由于SR重建和相邻帧之间的位移估计都通过神经网路来实现，它们可以融合在一起进行端到端的联合训练。为此，VESPCN使用的损失函数如下：

$$(\theta^*,\theta_\Delta^*)=\mathop{\arg\min}_{\theta,\theta_\Delta}\|I_t^{HR}-f(I_{t-1:t+1}^{'LR};\theta)\|_2^2
\\+\sum_{i=\pm 1}[\beta\|I_{t+i}^{'LR}-I_t^{LR}\|_2^2+\lambda\mathcal{H}(\partial_{x,y}\Delta_{t+i})]$$

第一项是衡量重建结果和Golden标准之间的差异，第二项是衡量相邻输入帧在空间对齐后的差异，第三项是平滑化空间位移场。

# SRGAN

SRGAN还是ESPCN原班人马的作品。

论文：

《Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network》

SRGAN将生成式对抗网络（GAN)用于SR问题。其出发点是传统的方法一般处理的是较小的放大倍数，当图像的放大倍数在4以上时，很容易使得到的结果显得过于平滑，而缺少一些细节上的真实感。因此SRGAN使用GAN来生成图像中的细节。

传统的方法使用的代价函数一般是最小均方差（MSE），即：

$$l_{MSE}^{SR}=\frac{1}{r^2WH}\sum_{x=1}^{rW}\sum_{y=1}^{rH}(I_{x,y}^{HR}-G_{\theta_G}(I^{LR})_{x,y})^2$$

该代价函数使重建结果有较高的信噪比，但是缺少了高频信息，出现过度平滑的纹理。SRGAN认为，应当使重建的高分辨率图像与真实的高分辨率图像无论是低层次的像素值上，还是高层次的抽象特征上，和整体概念和风格上，都应当接近。

![](/images/img2/SRGAN.png)

上图展示了MSE和GAN方法的区别。

整体概念和风格如何来评估呢？可以使用一个判别器，判断一副高分辨率图像是由算法生成的还是真实的。如果一个判别器无法区分出来，那么由算法生成的图像就达到了以假乱真的效果。

因此，该文章将代价函数改进为：

$$l^{SR}=l_X^{SR}+10^{-3}l_{Gen}^{SR}$$

第一部分是基于内容的代价函数（content loss），第二部分是基于对抗学习的代价函数（adversarial loss）。

论文中以VGG作为基础网络，因此content loss又可表述为：

$$l_{VGG/i,j}^{SR}=\frac{1}{W_{i,j}H_{i,j}}\sum_{x=1}^{W_{i,j}}\sum_{y=1}^{H_{i,j}}(\phi_{i,j}(I^{HR})_{x,y}-\phi_{i,j}(G_{\theta_G}(I^{LR}))_{x,y})^2$$

adversarial loss为：

$$l_{Gen}^{SR}=\sum_{n=1}^N-\log D_{\theta_D}(G_{\theta_G}(I^{LR}))$$

SRGAN的网络结构如下图所示：

![](/images/img2/SRGAN.jpg)

由于SRGAN的目标不在于最小化MSE，因此通常情况下，它的PSNR和SSIM都不是太好，但的确能提供一些其它方法无法提供的细节。

# DemosaicNet

DemosaicNet是MIT CSAIL的在读博士生Michaël Gharbi的作品。

论文：

《Deep Joint Demosaicking and Denoising》

代码：

https://github.com/mgharbi/demosaicnet

在《图像处理理论（五）》中，我们提到了ISP处理的一般流程。而SR的一大用途就在于ISP。

![](/images/img2/ISP_pipeline_2.png)

上图是ISP处理的一般流程，其中的Demosaic和Image Enhancement，都可以通过NN的端到端学习一次性完成。DemosaicNet就是其中的代表，它的网络结构如下：

![](/images/img2/DemosaicNet.png)

和之前的网络不同，DemosaicNet的输入是原始的Bayer Array数据，而输出是处理好的图片。

由于并没有那么多图片的Bayer Array数据，因此通常的做法是使用HR图片经采样得到Bayer Array数据。

DemosaicNet的设计借鉴了ResNet的Skip Connection的方案，只不过使用Concat代替了ResNet的Add操作而已。

这里再额外补充两点：

1.Demosaic处理不当，会导致如下问题：

![](/images/img2/Demosaic_2.png)

2.将出错的mine hard case，进行retrain，可以有效的提升模型的效果。

![](/images/img2/Demosaic_3.png)

# SVDF

SVDF是UCB和Google Speech Group的作品，主要用于简化Speech模型的计算量。

论文：

《Compressing Deep Neural Networks using a Rank-Constrained Topology》

代码：

https://github.com/tensorflow/tensorflow/blob/master/tensorflow/examples/speech_commands/models.py

音频数据通常是一个[time, frequency]的二维tensor，直接放入FC网络，会导致较大的计算量。（下图左半部分所示）

![](/images/img2/SVDF.png)

SVDF将每个time的frequency作为一组，进行FC之后，再和其他组的结果进一步FC。上图右半部分所示的是time的filters为1的时候的SVDF。当然filters也可以为其他值，和CNN类似，filters越多，提取的特征越多。

从原理来说，SVDF相当于用两层FC来拟合1层FC，即：

$$w_{i,j}^{(m)}\approx \alpha_i^{(m)}\beta_i^{(m)}$$

SVDF将运算量从$$Cd$$变为$$(C+d)k$$，这里的k为filters numbers。

这实际上就是2维tensor的SVD，只不过SVD是线性变换，而这里是非线性变换而已。（参见《机器学习（十五）》中的ALS算法部分）

实际上，SVDF和之前在《深度学习（十六）》中提到的Fast R-CNN的FC加速，原理是基本一致的。

# LCNN

LCNN是华盛顿大学和Allen AI研究所的作品。后者是微软创始人Paul Allen投资兴建的研究机构。

论文：

《LCNN: Lookup-based Convolutional Neural Network》

代码：

https://github.com/hessamb/lcnn

我们知道一个Conv层的weight是一个$$n\times m\times k_w \times k_h$$的tensor，这里的m,n分别是input和output的channel数，$$k_w,k_h$$则是kernel的宽和高。

LCNN将这个巨大的weight tensor拆解成若干小tensor的运算：

1.建立一个包含k个$$m\times k_w \times k_h$$大小的tensor的字典D。

2.一个用于选择字典条目的矩阵I。

3.权值矩阵C。

然后按照下图所示的方法，计算得到W：

![](/images/img2/LCNN.png)

用数学公式表示，则为：

$$W_{[:,r,c]}=\sum_{t=1}^sC_{[t,r,c]}\cdot D_{[I_{[t,r,c]},:]},\forall r,c$$

![](/images/img2/LCNN_2.png)

上图是LCNN的前向运算示意图，其中:

$$S_{[i,:,:]}=X*D_{[i,:]}$$

这个过程实际上等效于$$S*P$$，而参数P就是我们需要训练的模型参数。

可以看出LCNN和SVDF都是采用稀疏表示的方法来减少运算量，只是实现方式和用途略有不同而已。

参考：

http://blog.csdn.net/feynman233/article/details/69785592

LCNN论文阅读笔记


