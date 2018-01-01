---
layout: post
title:  深度学习（二十一）——DRCN, VDSR, ESPCN, FSRCNN, VESPCN, SRGAN, DemosaicNet
category: DL 
---

# SRCNN（续）

**SSIM（structural similarity， 结构相似性）**，也是一种全参考的图像质量评价指标，它分别从亮度、对比度、结构三方面度量图像相似性。

$$\mu_X=\frac{1}{H\times W}\sum_{i=1}^H\sum_{j=1}^WX(i,j),\, \sigma_X^2=\frac{1}{H\times W}\sum_{i=1}^H\sum_{j=1}^W(X(i,j)-\mu_X)^2$$

$$\sigma_{XY}=\frac{1}{H\times W}\sum_{i=1}^H\sum_{j=1}^W((X(i,j)-\mu_X)(Y(i,j)-\mu_Y))$$

$$l(X,Y)=\frac{2\mu_X\mu_Y+C_1}{\mu_X^2+\mu_Y^2+C_1},\, c(X,Y)=\frac{2\sigma_X\sigma_Y+C_2}{\sigma_X^2+\sigma_Y^2+C_2},\, s(X,Y)=\frac{\sigma_{XY}+C_3}{\sigma_X\sigma_Y+C_3}$$

$$SSIM(X,Y)=l(X,Y)\cdot c(X,Y)\cdot s(X,Y)$$

$$C_1,C_2,C_3$$为常数，为了避免分母为0的情况，通常取$$C_1=(K_1\cdot L)^2, C_2=(K_2\cdot L)^2, C_3=C_2/2$$，一般地$$K1=0.01, K2=0.03, L=255$$。

SSIM取值范围[0,1]，值越大，表示图像失真越小。

在实际应用中，可以利用滑动窗将图像分块，令分块总数为N，考虑到窗口形状对分块的影响，采用高斯加权计算每一窗口的均值、方差以及协方差，然后计算对应块的结构相似度SSIM，最后将平均值作为两图像的结构相似性度量，即平均结构相似性MSSIM：

$$MSSIM(X,Y)=\frac{1}{N}\sum_{k=1}^NSSIM(x_k,y_k)$$

参考：

http://blog.csdn.net/u011692048/article/details/77496861

超分辨率重建之SRCNN

http://www.cnblogs.com/vincent2012/archive/2012/10/13/2723152.html

PSNR和SSIM

# DRCN

DRCN（deeply-recursive convolutional network）是韩国首尔国立大学的作品。

论文：

《Deeply-Recursive Convolutional Network for Image Super-Resolution》

SRCNN的层数较少，同时感受野也较小（13x13）。DRCN提出使用更多的卷积层增加网络感受野（41x41），同时为了避免过多网络参数，该文章提出使用递归神经网络（RNN）。网络的基本结构如下：

![](/images/img2/DRCN.jpg)

与SRCNN类似，该网络分为三个模块，第一个是Embedding network，相当于特征提取，第二个是Inference network, 相当于特征的非线性变换，第三个是Reconstruction network,即从特征图像得到最后的重建结果。其中的Inference network是一个递归网络，即数据循环地通过该层多次。将这个循环进行展开，就等效于使用同一组参数的多个串联的卷积层，如下图所示：

![](/images/img2/DRCN_2.jpg)

其中的$$H_1$$到$$H_D$$是D个共享参数的卷积层。DRCN将每一层的卷积结果都通过同一个Reconstruction Net得到一个重建结果，从而共得到D个重建结果，再把它们加权平均得到最终的输出。另外，受到ResNet的启发，DRCN通过skip connection将输入图像与H_d的输出相加后再作为Reconstruction Net的输入，相当于使Inference Net去学习高分辨率图像与低分辨率图像的差，即恢复图像的高频部分。

参考：

http://blog.csdn.net/u011692048/article/details/77500764

超分辨率重建之DRCN

# VDSR

VDSR是DRCN的原班人马的新作。

论文：

《Accurate Image Super-Resolution Using Very Deep Convolutional Networks》

代码：

code：https://github.com/huangzehao/caffe-vdsr

SRCNN存在三个问题需要进行改进：

1、依赖于小图像区域的内容；

2、训练收敛太慢；

3、网络只对于某一个比例有效。

![](/images/img2/VDSR.png)

VDSR模型主要有以下几点贡献：

1、增加了感受野，在处理大图像上有优势，由SRCNN的13*13变为41*41。（20层的3x3卷积）

2、采用残差图像进行训练，收敛速度变快，因为残差图像更加稀疏，更加容易收敛（换种理解就是LR携带者低频信息，这些信息依然被训练到HR图像，然而HR图像和LR图像的低频信息相近，这部分花费了大量时间进行训练）。

3、考虑多个尺度，一个卷积网络可以处理多尺度问题。

训练的策略：

1、才用残差的方式进行训练，避免训练过长的时间。

2、使用大的学习进行训练。

3、自适应梯度裁剪，将梯度限制在某一个范围。

4、多尺度，多种尺度样本一起训练可以提高大尺度的准确率。

对于边界问题，由于卷积的操作导致图像变小的问题，本文作者提出一个新的策略，就是每次卷积后，图像的size变小，但是，在下一次卷积前，对图像进行补0操作，恢复到原来大小，这样不仅解决了网络深度的问题，同时，实验证明对边界像素的预测结果也得到了提升。

参考：

http://blog.csdn.net/u011692048/article/details/77512310

超分辨率重建之VDSR

# ESPCN

ESPCN（efficient sub-pixel convolutional neural network）是创业公司Magic Pony Technology的Wenzhe Shi和Jose Caballero作品。该创业团队主要来自Imperial College London，目前已被Twitter收购。

论文：

《Real-Time Single Image and Video Super-Resolution Using an Efficient Sub-Pixel Convolutional Neural Network》

代码：

https://github.com/wangxuewen99/Super-Resolution/tree/master/ESPCN

在SRCNN和DRCN中，低分辨率图像都是先通过上采样插值得到与高分辨率图像同样的大小，再作为网络输入，意味着卷积操作在较高的分辨率上进行，相比于在低分辨率的图像上计算卷积，会降低效率。

ESPCN提出一种在低分辨率图像上直接计算卷积得到高分辨率图像的高效率方法。

![](/images/img2/ESPCN.jpg)

ESPCN的核心概念是亚像素卷积层(sub-pixel convolutional layer)。如上图所示，网络的输入是原始低分辨率图像，通过两个卷积层以后，得到的特征图像大小与输入图像一样，但是特征通道为$$r^2$$(r是图像的目标放大倍数)。将每个像素的$$r^2$$个通道重新排列成一个$$r \times r$$的区域，对应于高分辨率图像中的一个$$r \times r$$大小的子块，从而大小为$$r^2 \times H \times W$$的特征图像被重新排列成$$1 \times rH \times rW$$大小的高分辨率图像。这个变换虽然被称作sub-pixel convolution, 但实际上并没有卷积操作。

通过使用sub-pixel convolution, 图像从低分辨率到高分辨率放大的过程，插值函数被隐含地包含在前面的卷积层中，可以自动学习到。只在最后一层对图像大小做变换，前面的卷积运算由于在低分辨率图像上进行，因此效率会较高。

参考：

http://blog.csdn.net/zuolunqiang/article/details/52401802

super-resolution技术日记——ESPCN

# FSRCNN

FSRCNN（Fast Super-Resolution CNN）是Chao Dong继SRCNN之后的又一作品。

论文：

《Accelerating the Super-Resolution Convolutional Neural Network》

代码：

https://github.com/66wangxuewen99/Super-Resolution/tree/master/FSRCNN

![](/images/img2/FSRCNN.png)

上图是FSRCNN和SRCNN的网络结构对比图。其中的$$Conv(f_i,n_i,c_i)$$中的$$f_i,n_i,c_i$$分别表示filter的大小、数量和通道的个数。

FSRCNN主要做了如下改进：

1.直接输入LR的图片。这和ESPCN思路一致。

2.将SRCNN中的Non-linear mapping分为Shrinking、Mapping、Expanding三个阶段。

3.使用Deconv重建HR图像。

>ESPCN的论文中指出他们的sub-pixel convolution效果优于Deconv。但由于ESPCN和FSRCNN出来的时间都差不多，尚未有他们两个正式PK的成绩。

参考：

http://blog.csdn.net/zuolunqiang/article/details/52411673

super-resolution技术日记——FSRCNN

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

$$(\theta^*,\theta^*_\Delta)$$

# SRGAN

SRGAN还是ESPCN原班人马的作品。

论文：

《Photo-Realistic Single Image Super-Resolution Using a Generative Adversarial Network》

![](/images/img2/SRGAN.jpg)

# DemosaicNet

论文：

《Deep Joint Demosaicking and Denoising》

代码：

https://github.com/mgharbi/demosaicnet

![](/images/img2/DemosaicNet.png)

![](/images/img2/ISP_pipeline_2.png)

