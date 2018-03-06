---
layout: post
title:  深度学习（二十一）——SRCNN, DRCN, VDSR, ESPCN, FSRCNN
category: DL 
---

# 图像超分辨率算法（续）

## 参考

https://zhuanlan.zhihu.com/p/25532538

深度学习在图像超分辨率重建中的应用

https://zhuanlan.zhihu.com/p/25201511

深度对抗学习在图像分割和超分辨率中的应用

https://mp.weixin.qq.com/s/uK0L5RV0bB2Jnr5WCZasfw

深度学习在单图像超分辨率上的应用：SRCNN、Perceptual loss、SRResNet

https://mp.weixin.qq.com/s/xpvGz1HVo9eLNDMv9v7vqg

NTIRE2017夺冠论文：用于单一图像超分辨率的增强型深度残差网络

https://www.zhihu.com/question/25401250

如何通过多帧影像进行超分辨率重构？

https://www.zhihu.com/question/38637977

超分辨率重建还有什么可以研究的吗？

https://zhuanlan.zhihu.com/p/25912465

胎儿MRI高分辨率重建技术：现状与趋势

https://mp.weixin.qq.com/s/i-im1sy6MNWP1Fmi5oWMZg

华为推出新型HiSR：移动端的超分辨率算法

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

1、增加了感受野，在处理大图像上有优势，由SRCNN的13x13变为41x41。（20层的3x3卷积）

2、采用残差图像进行训练，收敛速度变快，因为残差图像更加稀疏，更加容易收敛（换种理解就是LR携带者低频信息，这些信息依然被训练到HR图像，然而HR图像和LR图像的低频信息相近，这部分花费了大量时间进行训练）。

3、考虑多个尺度，一个卷积网络可以处理多尺度问题。

训练的策略：

1、采用残差的方式进行训练，避免训练过长的时间。

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



