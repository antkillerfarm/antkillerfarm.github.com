---
layout: post
title:  深度学习（二十二）——VESPCN, SRGAN, DemosaicNet, MemNet, RDN, Fast Image Processing, SVDF
category: DL 
---

# VESPCN

看名字就知道，VESPCN（Video ESPCN）仍然是ESPCN原班人马Wenzhe Shi和Jose Caballero作品。

论文：

《Real-Time Video Super-Resolution with Spatio-Temporal Networks and Motion Compensation》

在视频图像的SR问题中，相邻几帧具有很强的关联性，上述几种方法都只在单幅图像上进行处理，而VESPCN提出使用视频中的时间序列图像进行高分辨率重建，并且能达到实时处理的效率要求。其方法示意图如下，主要包括三个方面： 

![](/images/img2/VESPCN.png)

一是纠正相邻帧的位移偏差，即先通过Motion estimation估计出位移，然后利用位移参数对相邻帧进行空间变换，将二者对齐。二是把对齐后的相邻若干帧叠放在一起，当做一个三维数据，在低分辨率的三维数据上使用三维卷积，得到的结果大小为$$r^2\times H\times W$$。三是利用ESPCN的思想将该卷积结果重新排列得到大小为$$1\times rH\times rW$$的高分辨率图像。

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

# MemNet

MemNet是南京理工大学的作品。

论文：

《MemNet: A Persistent Memory Network for Image Restoration》

代码：

https://github.com/tyshiwo/MemNet

![](/images/img2/MemNet.png)

![](/images/img2/MemNet_2.png)

![](/images/img2/MemNet_3.png)

没啥好讲的，无非RNN和Resnet在原理上是等价的而已。结构上和DRCN几乎一样，不知道谁抄谁。。。

参考：

https://mp.weixin.qq.com/s/KxQ-GRnEYEdmS2H-DHIHOg

南京理工大学ICCV 2017论文：图像超分辨率模型MemNet

# RDN

Residual Dense Network是美国东北大学的张宇伦的作品。

>Yulun Zhang，西安电子科技大学本科（2013年）+清华硕士（2017年），现为博士一年级。   
>个人主页：   
>http://yulunzhang.com/

论文：

《Residual Dense Network for Image Super-Resolution》

![](/images/img2/RDN.png)

该论文在比较Residual block和Dense block的基础之上，提出了Residual dense block。

![](/images/img2/RDN_2.png)

![](/images/img2/RDN_3.png)

中规中矩的论文吧，熟悉Residual block和Dense block的人应该能秒懂，不多说了。

这类基本结构的SR应用除了MemNet和RDN之外，还有更早的SRResnet和SRDensenet，光听名字估计就知道是怎么回事了，灌水利器啊！

参考：

https://mp.weixin.qq.com/s/_r3MKxMTIR856ezEozFOGA

残差密集网络：利用所有分层特征的图像超分辨率网络

# Fast Image Processing

![](/images/article/FIP.png)

上图是照片界常用的几种修图方式之一。一般将这些图片风格转换的算法，称为图像处理算子（image processing operators）。如何加速image processing operators的计算，就成为了学界研究的课题之一。

本文提出的模型就是用来加速image processing operators计算的。它是Intel Lab的Qifeng Chen和Jia Xu于2017年提出的。

论文：

《Fast Image Processing with Fully-Convolutional Networks》

代码：

https://github.com/CQFIO/FastImageProcessing

Demo网站：

http://cqf.io/ImageProcessing/

这个课题一般使用MIT-Adobe FiveK Dataset作为基准数据集。网址：

http://groups.csail.mit.edu/graphics/fivek_dataset/

这个数据集包含了5K张原始照片，并雇用了5个专业修图师，对每张图片进行修图。

众所周知，多层神经网络只要有足够的深度和宽度，就可以任意逼近任意连续函数。然而从Fast Image Processing的目的来说，神经网络的深度和宽度注定是有限的，否则肯定快不了。而这也是该课题的研究意义所在。

本文只使用了MIT-Adobe数据集中的原始图片，并使用了10种常用的算子对图片进行处理。因此，该网络训练时的输入是原始图片，而输出是处理后的图片。

![](/images/article/MCA.png)

上图是本文模型的网络结构图。它的设计特点如下：

1.采用Multi-Scale Context Aggregation作为基础网络。MCA的内容参见《深度学习（九）》。

2.传统MCA一般有下采样的过程，但这里由于网络输入和输出的尺寸维度是一样的，因此，所有的feature maps都是等大的。

3.借鉴FCN的思想，去掉了池化层和全连接层。

4.L1~L3主要用于图片的特征提取和升维，而L4~L5则用于特征的聚合和降维，并最终和输出数据的尺寸维度相匹配。

在normalization方面，作者发现有的operators经过normalization之后，精度会上升，而有的精度反而会下降，因此为了统一模型，定义如下的normalization运算：

$$\Psi^s(x)=\lambda_sx+\mu_sBN(x)$$

Loss函数为：

$$\mathcal{l(K,B)}=\sum_i\frac{1}{N_i}\|\hat f (I_i;\mathcal{K,B})-f(I_i)\|^2$$

这实际上就是RGB颜色空间的MSE误差。

为了检验模型的泛化能力，本文还使用RAISE数据集作为交叉验证的数据集。该数据集的网址：

http://mmlab.science.unitn.it/RAISE/

RAISE数据集包含了8156张高分辨率原始照片，由3台不同的相机拍摄，并给出了相机的型号和参数。

# TNG

Tiny Network Graphics是图鸭科技推出一种基于深度学习的图片压缩技术。由于商业因素，这里没有论文，技术细节也不详，但是下图应该还是有些用的。

![](/images/img2/TNG.png)

参考：

https://mp.weixin.qq.com/s/WYsxFX4LyM562bZD8rO95w

图鸭发布图片压缩TNG，节省55%带宽

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

