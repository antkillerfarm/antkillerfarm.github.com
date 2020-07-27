---
layout: post
title:  深度学习（二十三）——MemNet, RDN, ShuffleSeg, SVDF, LCNN
category: DL 
---

# DemosaicNet（续）

和之前的网络不同，DemosaicNet的输入是原始的Bayer Array数据，而输出是处理好的图片。

由于并没有那么多图片的Bayer Array数据，因此通常的做法是使用HR图片经采样得到Bayer Array数据。

>注意，如果训练数据有原始的Bayer Array的Raw data，那是最好的。降采样或者Raw data的RGB化，都有一定的高频信号的损失。

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

# ShuffleSeg

ShuffleSeg是开罗大学的Mostafa Gamal和Mennatullah Siam的作品（2018.3）。看名字应该是阿拉伯人，而且一男一女。

论文：

《ShuffleSeg: Real-time Semantic Segmentation Network》

代码：

https://github.com/MSiam/TFSegmentation

![](/images/img2/ShuffleSeg.png)

这是一个语义分割的网络，本来不该放在这里。然而既然要灌水，那就灌的更猛一些吧。ShuffleNet也难逃毒手。

参考：

https://mp.weixin.qq.com/s/W2reKR5prcf3_DMp53-2yw

新型实时形义分割网络ShuffleSeg：可用于嵌入式设备

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

这实际上就是2维tensor的SVD，只不过SVD是线性变换，而这里是非线性变换而已。（参见《机器学习（十六）》中的ALS算法部分）

实际上，SVDF和《深度目标检测（三）》中提到的Fast R-CNN的FC加速，原理是基本一致的。

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
