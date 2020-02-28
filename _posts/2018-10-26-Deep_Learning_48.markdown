---
layout: post
title:  深度学习（四十八）——花式U-NET, 语义分割进阶
category: DL 
---

# 花式U-NET

## U-NET的另类用法

U-NET除了用于语义分割之外，还可用于语音分离——将人声/音乐从原始混合声音数据中分离出来。比如卡拉OK中的常见的原声抑制功能。

论文：

《Singing Voice Separation With Deep U-net Convolutional Networks》

代码：

https://github.com/Xiao-Ming/UNet-VocalSeparation-Chainer

Chainer版本的实现

https://github.com/Jeongseungwoo/Singing-Voice-Separation

Tensorflow版本的实现

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/Singing-Voice-Separation

Jeongseungwoo的版本一次加载了全部的数据集到内存中，对PC的要求较高（估计起码要32GB内存才能跑），这是我修改后的版本。用户可以根据自己PC的性能，调整batch size。

https://github.com/f90/Wave-U-Net

另一个Tensorflow版本的实现。这哥们还有个使用Semi-supervised adversarial学习分离人声的项目（他也是该项目论文的一作）：

https://github.com/f90/AdversarialAudioSeparation

![](/images/img2/wave_u_net.png)

这种用途的U-NET和原始U-NET的区别在于：

1.输入和输出是音频数据的时序频谱图，从某种意义上来说，其实就是一张二维图片。

2.输入是包含混音的数据，而输出是纯净的人声/音乐的Mask。混音数据*Mask=纯净声音。由于标注数据比较难获得，因此通常的做法是使用纯音和若干噪声进行合成得到混音数据。

3.由于最终结果不再是像素级的分类问题，因此Loss采用了absolute difference。

从上面的论述可以看出，该论文主要是用到了语义分割网络中**输入和输出的尺寸等大**这个特点，算是一种很灵巧的构思了。

这方面的数据集主要有：

***CCMixter：***

https://members.loria.fr/ALiutkus/kam/

这个数据集的每个文件夹下有3个wav文件：

source-01.wav：纯音乐。

source-02.wav：人声。

mix.wav：混合后的声音。

***MUSDB18：***

https://sigsep.github.io/datasets/musdb.html

类似这样用法的还有：

《Learning to See in the Dark》

代码：

https://github.com/cchen156/Learning-to-See-in-the-Dark

![](/images/img2/SID.png)

如上图所示，该文的目标是使用神经网络替换传统的相机ISP过程。由于输入和输出的尺寸等大，照例又到了U-NET出场的时间。

为了实现这一目标，作者收集了一个新的原始图像数据集，在弱光条件下快速曝光。同时，每个微光图像都有相应的长曝光、高质量的参考图像。

参考：

https://mp.weixin.qq.com/s/cr0BJLkyN2kW35-w1pebGQ

学习在黑暗中看世界（Learning to See in the Dark）

https://mp.weixin.qq.com/s/iv4ixoXvyPMp60hp2XhK8A

港中文&腾讯优图等提出：暗光下的图像增强

https://mp.weixin.qq.com/s/p2Vr9Y9vl4BlHZB_DIzTbw

基于深度学习的低光照图像增强方法总结（2017-2019）

https://mp.weixin.qq.com/s/E20ucf5bfexKYH4R7zK-WA

最好用的音轨分离软件spleeter

## 花式U-Net

本节主要摘抄自：

https://zhuanlan.zhihu.com/p/57530767

图像分割的U-Net系列方法

### 3D U-Net

论文：

《3D U-Net: Learning Dense Volumetric Segmentation from Sparse Annotation》

![](/images/img3/U-Net_3D.png)

这个算是3D领域的base-line了，而且效果还不错。好多新网络还未必比得过它。

### ResUnet

论文：

《Road Extraction by Deep Residual U-Net》

![](/images/img3/ResUnet.png)

### DenseUnet

论文：

《Fully Dense UNet for 2D Sparse Photoacoustic Tomography Artifact Removal》

![](/images/img3/DenseUnet.png)

![](/images/img3/DenseUnet_2.png)

### MultiResUNet

论文：

《MultiResUNet : Rethinking the U-Net Architecture for Multimodal Biomedical Image Segmentation》

ResUnet和DenseUnet基本属于排列组合式的灌水。下面的MultiResUNet还是有些干货的。

![](/images/img3/MultiResUNet.png)

上图是该论文提出的MultiRes block结构。

![](/images/img3/MultiResUNet_2.png)

还有下采样和上采样之间的Res path结构。

![](/images/img3/MultiResUNet_3.png)

这是最终的网络结构图。基本上把concat和add的各种组合都撸了一遍。

### R2U-Net

论文：

《Recurrent Residual Convolutional Neural Network based on U-Net (R2U-Net) for Medical Image Segmentation》

![](/images/img3/R2U-Net.png)

注意，这里的Recurrent Convolutional是Convolutional的一个变种，和RNN没有关系。

### Attention U-Net

论文：

《Attention U-Net: Learning Where to Look for the Pancreas》

![](/images/img3/AttU-Net.png)

Attention也难逃毒手。

### Attention R2U-Net

![](/images/img3/AttR2U-Net.png)

继续组合。不过作者还是有廉耻的，这个没有写论文灌水。

R2U-Net, Attention U-Net, Attention R2U-Net的代码都在这里：

https://github.com/LeeJunHyun/Image_Segmentation

## 参考

https://www.zhihu.com/question/269914775

Unet神经网络为什么会在医学图像分割表现好？

https://mp.weixin.qq.com/s?__biz=MzIwMTE1NjQxMQ==&mid=2247488752&idx=3&sn=23f223fb82c64c14eb925ebd19d16fc5

图像分割中的深度学习：U-Net体系结构

https://zhuanlan.zhihu.com/p/55193930

Attention U-Net论文笔记

https://zhuanlan.zhihu.com/p/109514384

医学图像分割的Non-local U-Nets

# 语义分割进阶

https://mp.weixin.qq.com/s/_q8N8keNpcD-jXLrRd_8dw

深度学习时代下的语义分割综述

https://blog.csdn.net/yorkhunter/article/details/104057159

arXiv综述论文“Image Segmentation Using Deep Learning: A Survey”

https://mp.weixin.qq.com/s/BSJ-F_Inp1-eHxSJDsQ3AQ

12篇文章带你逛遍主流分割网络

https://mp.weixin.qq.com/s/cANlqQAI-A2mC9vnd3imQA

Instance-Aware图像语义分割

https://mp.weixin.qq.com/s/v_TLYYq6cFWuwR9tXM8m-A

如何通过CRF-RNN模型实现图像语义分割任务

https://mp.weixin.qq.com/s/ceCC7Q6yr0QKESeZXi6lWQ

堆叠解卷积网络实现图像语义分割顶尖效果

https://mp.weixin.qq.com/s/V4_euZRcyyxeimXAA_waAg

贾佳亚：最有效的COCO物体分割算法

https://mp.weixin.qq.com/s/M1Oo4ST2aspgZF8UeSUDww

如何妙笔勾檀妆：像素级语义理解

https://mp.weixin.qq.com/s/xalo2XtKtzR5tA_dPFzaJw

一文介绍3篇无需Proposal的实例分割论文

https://mp.weixin.qq.com/s/BL1xZ_YuuPe9frIc9E1fkA

南开大学提出新物体分割评价指标

https://mp.weixin.qq.com/s/3rfZUhio4Bk1RUGkEk5xoQ

ETH Zurich提出新型网络“ROAD-Net”，解决语义分割域适配问题

https://mp.weixin.qq.com/s/qMLCi-CghxvTcwyPnvFxnQ

ConvCRF：一种结合条件随机场与CNN的高效语义分割方法

https://mp.weixin.qq.com/s/deepxMWCpIEe3jk_kanfMg

金字塔注意力网络：一种利用底层像素与高级特征的语义分割网络

https://mp.weixin.qq.com/s/5n3jpvv_LxnHB0w4hsCEzQ

NVIDIA ECCV18论文:超像素采样网络助力语义分割与光流估计

https://mp.weixin.qq.com/s/MiChpWim5pGlRj88rcQtaA

谷歌等祭出图像语义理解分割神器，PS再也不用专业设计师！

https://mp.weixin.qq.com/s/J6UMzWSpcmSQVGwWKtm2Hw

UC伯克利提出基于自适应相似场的语义分割

https://zhuanlan.zhihu.com/p/43774180

提升密集预测的平滑的空洞卷积

https://mp.weixin.qq.com/s/sjD36kUDQ5iCmIKpR8_rlA

双重注意力网络：中科院自动化所提出新的自然场景图像分割框架

https://mp.weixin.qq.com/s/Sn3N8IxHtgp53Y0VLIPJCQ

17毫秒每帧！实时语义分割与深度估计

https://mp.weixin.qq.com/s/iNz82GUULxDndBIiGSArmQ

新开源！实时语义分割算法Light-Weight RefineNet

https://mp.weixin.qq.com/s/2QBifDubR5mMoJNKkdBNIw

多分辨率特征融合—RefineNet

https://mp.weixin.qq.com/s/1wqguIqDS4FNsS67Yj77Qw

牛津大学&Emotech首次严谨评估语义分割模型对对抗攻击的鲁棒性

https://zhuanlan.zhihu.com/p/48198502

Non-local Neural Networks论文笔记

https://mp.weixin.qq.com/s/EzfvKzs8Ue8i9x9TFgZ-CQ

爱奇艺蒙版AI：弹幕穿人过，爱豆心中坐

https://mp.weixin.qq.com/s/19uMhoNXEygLRTYT2PbsYQ

图像分割技术介绍

https://mp.weixin.qq.com/s/-wwDenAWRGCvJmprxsl15Q

基于多特征地图和深度学习的实时交通场景分割

https://mp.weixin.qq.com/s/ygWCfLnakHIwLVk7hRAKNg

全景分割任务介绍及其最新进展

https://mp.weixin.qq.com/s/ZN9ZYPTcgVP2c9mCx9Ox3g

全景分割这一年，端到端之路

https://mp.weixin.qq.com/s/x95XWQW2euTEcUW5vkIEoA

何恺明组又出神作！最新论文提出全景分割新方法（Panoptic FPN）

https://zhuanlan.zhihu.com/p/54510782

DANet论文笔记

https://mp.weixin.qq.com/s/94XHMHejqKmkFVA0Vzfrsw

有关语义分割的奇技淫巧有哪些？

https://zhuanlan.zhihu.com/p/55263898

语义分割江湖的那些事儿——从旷视说起

https://mp.weixin.qq.com/s/x6MJqCz3Yvcbc9onEO8OMQ

语义分割：context relation

https://mp.weixin.qq.com/s/eLU-YNvV_QGc8HDK15P7Og

Fast-OCNet:更快更好的OCNet

https://zhuanlan.zhihu.com/p/56887843

MIT和Google等提出：新全景分割算法DeeperLab

https://mp.weixin.qq.com/s/zHZO1pmY8PCoI9vkDOaUgw

CCNet--于"阡陌交通"处超越恺明的Non-local

https://mp.weixin.qq.com/s/1tohID6SM3weS476XU5okw

全景分割：Attention-guided Unified Network

https://mp.weixin.qq.com/s/ilQzQ-5RyiOYzS0RRMihLg

语义分割原理与CNN架构变迁

https://zhuanlan.zhihu.com/p/59141570

漫谈全景分割

https://mp.weixin.qq.com/s/fSaBskCxMkNwVXDtCI7ZZg

Decoders对于语义分割的重要性

https://mp.weixin.qq.com/s/eackv0HypWm3mTW9OAHv8g

旷视实时语义分割技术DFANet：高清虚化无需双摄

https://zhuanlan.zhihu.com/p/62652145

加州大学提出：实时实例分割算法YOLACT，可达33 FPS/30mAP！现已开源！

https://mp.weixin.qq.com/s/i6Qj5YyjLINxHSYuHckKzg

YOLACT++：更强的实时实例分割网络，可达33.5 FPS/34.1mAP！

https://mp.weixin.qq.com/s/8I7Lm5DMkVVlvw8v1L_HBA

多感受野的金字塔结构—PSPNet

https://mp.weixin.qq.com/s/TNHTvXmefRBlc6zfHW7C8A

全局特征与局部特征的交响曲—ParseNet

https://mp.weixin.qq.com/s/riS79dU5Zpzsl8vB64sPIg

BRNN下的RGB-D分割—LSTM-CF

https://zhuanlan.zhihu.com/p/67423280

无监督域适应语义分割

https://mp.weixin.qq.com/s/XTTzAkgVF-WMJT7IYWxJYg

病理图像的全景分割

https://mp.weixin.qq.com/s/HJzbMoCa7GenNm78sz7YAg

全景分割是什么？

https://mp.weixin.qq.com/s/PUHytS_nLKPjlrv8RYxo8A

一文看懂实时语义分割

https://zhuanlan.zhihu.com/p/77834369

语义分割中的Attention和低秩重建

https://mp.weixin.qq.com/s/wAWB2SLaqD2SmP9-j0Ut7g

速度提升一倍，无需实例掩码预测即可实现全景分割
