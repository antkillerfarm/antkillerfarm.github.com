---
layout: post
title:  深度学习（十二）——MobileNet
category: DL 
---

* toc
{:toc}

# 花式卷积

## depthwise separable convolution（续）

参考：

http://blog.csdn.net/mao_xiao_feng/article/details/78003476

tf.nn.depthwise_conv2d如何实现深度卷积?

http://blog.csdn.net/mao_xiao_feng/article/details/78002811

tf.nn.separable_conv2d如何实现深度可分卷积?

https://blog.csdn.net/tintinetmilou/article/details/81607721

Depthwise卷积与Pointwise卷积

https://mp.weixin.qq.com/s/KEWEC6s0wYQhYpyh6dKvQQ

MixConv：来自Google Brain的混合Depthwise卷积核

https://www.cnblogs.com/itmorn/p/11250371.html

depthwise_conv

## 感受野

Receptive Field本来是神经科学领域的概念，后来才被推广到DL（尤其是CNN）领域。

Receptive Field的大小实际上就是采样范围的大小，例如一个kernel为9x9，stride为1的普通卷积，其采样范围为13x13。（即kernel size+pad size, 9+4=13）

其他卷积的情况，可以依此类推。

对于多层神经网络的感受野，一般用Layer N上的一个点，在Input中的采样范围表示。所以层数越多，感受野越大。

需要注意的是，感受野中心的点，由于几乎每层卷积计算都会被采样到，因此它们的采样率是大于边缘点的。换句话说，就是对结果有更大的影响。因此，这又引入了**有效感受野**的概念。从实践角度来看，有效感受野的半径通常为感受野半径的1/3～1/5。

参考：

https://mp.weixin.qq.com/s/R8rEngNI31w0DQwjjeS6kw

关于感受野的总结

https://zhuanlan.zhihu.com/p/44106492

卷积神经网络的感受野

## 可变形卷积核

MSRA于2017年提出了可变形卷积核的概念。

论文：

《Deformable Convolutional Networks》

![](/images/article/Deformable_convolution.png)

(a)所示的正常卷积规律的采样9个点（绿点），(b)(c)(d)为可变形卷积，在正常的采样坐标上加上一个位移量（蓝色箭头），其中(c)(d)作为(b)的特殊情况，展示了可变形卷积可以作为尺度变换，比例变换和旋转变换的特殊情况。

![](/images/img2/Deformable_convolution.png)

如上图所示，位移量也成为了网络中待学习的参数。

参考：

https://mp.weixin.qq.com/s/okI3MT3E2o2PKCeokE7Niw

MSRA视觉组最新研究：可变形卷积网络

http://mp.weixin.qq.com/s/dvuX3Ih_DZrv0kgqFn8-lg

卷积神经网络结构变化——可变形卷积网络deformable convolutional networks

https://mp.weixin.qq.com/s/iN2LDAQ2ee-rQnlD3N1yaw

变形卷积核、可分离卷积？CNN中十大拍案叫绝的操作！

http://www.msra.cn/zh-cn/news/features/deformable-convolutional-networks-20170609

可变形卷积网络：计算机新“视”界

https://www.jianshu.com/p/940d21c79aa3

Deformable Convolution Networks

https://mp.weixin.qq.com/s/aLvlLi97JTd_cCfCZfraIg

“不正经”的卷积神经网络

https://mp.weixin.qq.com/s/gsoVFiG3tKhHAU7OCGfLPg

如何评价MSRA视觉组最新提出的Deformable ConvNets V2？

https://mp.weixin.qq.com/s/VmcxU7ZgNJbNUy-Feiz3ig

目标检测：Deformable ConvNets v2算法笔记

https://mp.weixin.qq.com/s/sIeQ9VpQae-eWpkcx3S7Mw

可变形卷积系列(一) 打破常规，MSRA提出DCNv1

https://mp.weixin.qq.com/s/PGXyuKMj4FV3Kk53l7sfQw

可变形卷积系列(二) MSRA提出升级版DCNv2，变形能力更强

https://mp.weixin.qq.com/s/aezywo9xtqpiV7SEXBMdmA

可变形卷积系列(三) 可变形卷积核，大开眼界

https://mp.weixin.qq.com/s/w43wfF1dKMu65as6lAlrsg

可变形卷积在视频学习中的应用:如何利用带有稀疏标记数据的视频帧

https://mp.weixin.qq.com/s/PKSrgy7KdtVUv4EXVDyiOw

再思考可变形卷积

## 3D卷积

3D卷积一般用于视频（2D图像+1D时序）和医学影像（3D立体图像）的分析处理中。

![](/images/article/conv_3d.png)

![](/images/img2/conv3d.gif)

如上两图所示，3D卷积的kernel不再是2D的，而是3D的。

它和多通道卷积的区别在于：

多通道卷积不同的通道上的卷积核的参数是不同的，而3D卷积则由于卷积核本身是3D的，所以这个由于“深度”造成的看似不同通道上用的就是同一个卷积。

3D卷积可以转化为2D卷积，方法如下图：

Prepare Input：

![](/images/img4/conv3d.png)

Prepare Kernel：

![](/images/img4/conv3d_2.png)

Compute Output：

![](/images/img4/conv3d_3.png)

论文：

《A two-stage 3D Unet framework for multi-class segmentation on full resolution image》

![](/images/img2/UNet-3D.jpg)

上图是一个用于CT图像的语义分割网络。其结构仿照UNet，故被称作UNet-3D。

处理大型高分辨率3D数据时的一个常见问题是，由于计算设备的存储容量有限，输入深度CNN的体积必须进行裁剪（crop）或降采样（downsample）。这些操作会导致输入数据 batches 中分辨率的降低和类不平衡的增加，从而降低分割算法的性能。

受到图像超分辨率CNN（SRCNN）和self-normalization（SNN）的架构的启发，我们开发了一个两阶段修改的Unet框架，它可以同时学习检测整个体积内的ROI并对体素进行分类而不会丢失原始图像解析度。对各种多模式音量的实验表明，当用简单加权的模子系数和我们定制的学习程序进行训练时，该框架显示比具有高级相似性度量标准的最先进的深CNN更好的分割性能。

除了方形的3D卷积之外，还有球形的3D卷积：

![](/images/img3/sph3d.png)

上图是球卷积在点云处理中的应用。论文：

《Spherical Kernel for Efficient Graph Convolution on 3D Point Clouds》

参考：

https://zhuanlan.zhihu.com/p/21325913

3D卷积神经网络Note01

https://zhuanlan.zhihu.com/p/21331911

3D卷积神经网络Note02

https://zhuanlan.zhihu.com/p/31841353

3D CNN阅读笔记

https://mp.weixin.qq.com/s/tcuyp4SK_9zXZKZtUu8h9Q

从2D卷积到3D卷积，都有什么不一样

https://zhuanlan.zhihu.com/p/25912625

C3D network: 用于视频特征提取的3维卷积网络

https://zhuanlan.zhihu.com/p/26350774

SCNN-用于时序动作定位的多阶段3D卷积网络

https://www.jiqizhixin.com/articles/2016-08-03

FusionNet融合三个卷积网络：识别对象从二维升级到三维

http://blog.csdn.net/zouxy09/article/details/9002508

基于3D卷积神经网络的人体行为理解

https://mp.weixin.qq.com/s/YdON6Yzddq2f_QGbQsOY8w

深度三维残差神经网络：视频理解新突破

https://mp.weixin.qq.com/s/MfDQXTSoe0GnaEFfyLJQ1w

点云处理不得劲？球卷积了解一下

https://paddlepedia.readthedocs.io/en/latest/tutorials/CNN/convolution_operator/3D_Convolution.html

3D卷积

## 参考

https://github.com/vdumoulin/conv_arithmetic

Convolution arithmetic

http://deeplearning.net/software/theano_versions/dev/tutorial/conv_arithmetic.html

Convolution arithmetic

https://www.jarvis73.com/2019/06/06/Convolution-Guide/

Convolution Arithmetic for Deep Learning

https://mp.weixin.qq.com/s/dR2nhGqpz7OdmxKPYSaaxw

如何理解空洞卷积（dilated convolution）？

https://mp.weixin.qq.com/s/CLFbhWMcat4rN8YS_7q25g

这12张图生动的告诉你，深度学习中的卷积网络是怎么一回事？

https://mp.weixin.qq.com/s/kJEeKzC9pC375EjIJpTuzg

一文全解深度学习中的卷积

http://cs.nyu.edu/~fergus/drafts/utexas2.pdf

Deconvolutional Networks

https://zhuanlan.zhihu.com/p/22245268

CNN-反卷积

http://buptldy.github.io/2016/10/29/2016-10-29-deconv/

Transposed Convolution, Fractionally Strided Convolution or Deconvolution（中文blog）

https://mp.weixin.qq.com/s/ybI8kJPRn7sH-hJbc5uqnw

CMU研究者探索新卷积方法：在实验中可媲美基准CNN

https://zhuanlan.zhihu.com/p/46633171

深度卷积神经网络中的降采样

https://mp.weixin.qq.com/s/1gBC-bp4Q4dPr0XMYPStXA

万字长文带你看尽深度学习中的各种卷积网络

https://mp.weixin.qq.com/s/qReN6z8s45870HSMCMNatw

微软亚洲研究院：逐层集中Attention的卷积模型

http://blog.csdn.net/shuzfan/article/details/77964370

不规则卷积神经网络

https://mp.weixin.qq.com/s/rXr_XBc2Psh3NSA0pj4ptQ

常建龙：深度卷积网络中的卷积算子研究进展

https://mp.weixin.qq.com/s/i8vOeAVEYX-hRAvPSe6DEA

一文看尽神经网络中不同种类的卷积层

https://mp.weixin.qq.com/s/hZc8MgHoE010hnzLU-trIA

高性能涨点的动态卷积DyNet与CondConv、DynamicConv有什么区别联系？

https://www.yuque.com/yahei/hey-yahei/condconv

CondConv：按需定制的卷积权重

https://mp.weixin.qq.com/s/eRZ3jNuceMYKE3lEj-g1aw

动态卷积：自适应调整卷积参数，显著提升模型表达能力

https://mp.weixin.qq.com/s/_GOXBYyyYnridILemNRDqA

ChannelNets: 省力又讨好的channel-wise卷积，在channel维度进行卷积滑动 

https://mp.weixin.qq.com/s/HMLKUL3_3MhWmJ8ub-Yfcg

一文速览Deep Learning中的11种卷积

https://zhuanlan.zhihu.com/p/540426043

51x51的kernelsize暴力美学：SLaK论文解读

# MobileNet

论文：

《MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications》

代码：

https://github.com/Zehaos/MobileNet

![](/images/article/dwl_pwl.png)

上述结构根据通道数的变化，又可以分为两种：

Bottleneck：

![](/images/img5/BottleNeck.png)

Inverted Bottleneck

![](/images/img5/Inverted_Bottleneck.png)

参考：

https://mp.weixin.qq.com/s/f3bmtbCY5BfA4v3movwLVg

向手机端神经网络进发：MobileNet压缩指南

https://mp.weixin.qq.com/s/mcK8M6pnHiZZRAkYVdaYGQ

MobileNet在手机端上的速度评测：iPhone 8 Plus竟不如iPhone 7 Plus

https://mp.weixin.qq.com/s/2XqBeq3N4mvu05S1Jo2UwA

CNN模型之MobileNet

https://mp.weixin.qq.com/s/fdgaDoYm2sfjqO2esv7jyA

Google论文解读：轻量化卷积神经网络MobileNetV2

https://mp.weixin.qq.com/s/7vFxmvRZuM2DqSYN7C88SA

谷歌发布MobileNetV2：可做语义分割的下一代移动端计算机视觉架构

https://mp.weixin.qq.com/s/lu0GHCpWCmogkmHRKnJ8zQ

浅析两代MobileNet

https://mp.weixin.qq.com/s/T6S1_cFXPEuhRAkJo2m8Ig

轻量级CNN网络之MobileNetv2

https://mp.weixin.qq.com/s/RRu3r_dokORhpSq3eyrPDQ

为什么MobileNet及其变体如此之快？
