---
layout: post
title:  深度学习（二十二）——VESPCN, SRGAN, DemosaicNet
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

传统的方法使用的代价函数一般是最小均方差（MSE），即

$$l_{MSE}^{SR}=\frac{1}{}$$

![](/images/img2/SRGAN.jpg)

# DemosaicNet

论文：

《Deep Joint Demosaicking and Denoising》

代码：

https://github.com/mgharbi/demosaicnet



![](/images/img2/DemosaicNet.png)

![](/images/img2/ISP_pipeline_2.png)


