---
layout: post
title:  深度学习（十一）——花式卷积（2）
category: DL 
---

* toc
{:toc}

# 花式卷积

## 分组卷积（续）

https://mp.weixin.qq.com/s/0MvCnm46pgeMGEw-EdNv_w

CNN模型之ShuffleNet

https://mp.weixin.qq.com/s/tceLrEalafgL8R44DZYP9g

旷视科技提出新型轻量架构ShuffleNet V2：从理论复杂度到实用设计准则

https://mp.weixin.qq.com/s/Yhvuog6NZOlVWEZURyqWxA

ShuffleNetV2：轻量级CNN网络中的桂冠

https://mp.weixin.qq.com/s/zLf0aKeMYwqMwC1TymMxgQ

移动端高效网络，卷积拆分和分组的精髓

https://zhuanlan.zhihu.com/p/86095608

Learnable Group Convolutions:可以学习的分组卷积

https://mp.weixin.qq.com/s/liCS3JoRj1scpc0jXFA4-w

分组卷积最新进展，全自动学习的分组有哪些经典模型？

## Separable convolution

前面介绍的都是正方形的卷积核，实际上长条形的卷积核也是很常用的。比如可分离卷积。

我们知道卷积的计算量和卷积核的面积成正比。对于k x k的卷积核K来说，计算复杂度就是$$O(k^2)$$。

如果我们能找到1 x k的卷积核H和k x 1的卷积核V，且$$K = V * H$$，则称K是可分离的卷积核。

根据卷积运算满足结合律，可得：

$$f * K = f * (V * H) = f * V * H$$

这样就将一个k x k的卷积运算，转换成1 x k + k x 1的卷积运算，从而大大节省了参数和计算量。

显然，不是所有的卷积核都满足可分离条件。但是不要紧，NN有自动学习并逼近函数的能力。经过训练之后：$$K \approx V * H$$

## 1x1卷积

1、升维或降维。

如果卷积的输出输入都只是一个平面，那么1x1卷积核并没有什么意义，它是完全不考虑像素与周边其他像素关系。 但卷积的输出输入是长方体，所以1x1卷积实际上是对每个像素点，在不同的channels上进行线性组合（信息整合），且保留了图片的原有平面结构，调控depth，从而完成升维或降维的功能。

![](/images/article/conv_1x1.png)

2、加入非线性。卷积层之后经过激励层，1x1的卷积在前一层的学习表示上添加了非线性激励（non-linear activation），提升网络的表达能力；

3.促进不同通道之间的信息交换。

参考：

https://www.zhihu.com/question/56024942

卷积神经网络中用1x1卷积有什么作用或者好处呢？

## depthwise separable convolution

在传统的卷积网络中，卷积层会同时寻找跨空间和跨深度的相关性。

然而Xception指出：跨通道的相关性和空间相关性是完全可分离的，最好不要联合映射它们。

>Xception是Francois Chollet于2016年提出的。

![](/images/article/Xception.png)

上图是Xception中的卷积运算depthwise separable convolution的示意图。

它包含一个深度方面的卷积（一个为每个通道单独执行的空间卷积，depthwise convolution），后面跟着一个逐点的卷积（一个跨通道的1×1卷积，pointwise convolution）。我们可以将其看作是首先求跨一个2D空间的相关性，然后再求跨一个1D空间的相关性。可以看出，这种2D+1D映射学起来比全 3D 映射更加简单。

在ImageNet数据集上，Xception的表现稍稍优于Inception v3，而且在一个有17000类的更大规模的图像分类数据集上的表现更是好得多。而它的模型参数的数量仅和Inception一样多。

上面展示的是1个input channel对应1个output channel的depthwise convolution，实际使用中，也可以1个input channel对应N个output channel，这里的N一般被称作multipler参数。

更一般的，如果是N个input channel对应M个output channel的话，就是之前介绍过的Grouped Convolution了。

论文：

《Xception: Deep Learning with Depthwise Separable Convolutions》

代码：

https://github.com/fchollet/keras/blob/master/keras/applications/xception.py

>Francois Chollet，法国人。现为Google研究员。Keras的作者。

参考：

http://blog.csdn.net/mao_xiao_feng/article/details/78003476

tf.nn.depthwise_conv2d如何实现深度卷积?

http://blog.csdn.net/mao_xiao_feng/article/details/78002811

tf.nn.separable_conv2d如何实现深度可分卷积?

https://blog.csdn.net/tintinetmilou/article/details/81607721

Depthwise卷积与Pointwise卷积

https://mp.weixin.qq.com/s/KEWEC6s0wYQhYpyh6dKvQQ

MixConv：来自Google Brain的混合Depthwise卷积核

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

(a) 所示的正常卷积规律的采样 9 个点（绿点），(b)(c)(d) 为可变形卷积，在正常的采样坐标上加上一个位移量（蓝色箭头），其中(c)(d) 作为 (b) 的特殊情况，展示了可变形卷积可以作为尺度变换，比例变换和旋转变换的特殊情况。

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

## 3D卷积

3D卷积一般用于视频（2D图像+1D时序）和医学影像（3D立体图像）的分析处理中。

![](/images/article/conv_3d.png)

![](/images/img2/conv3d.gif)

如上两图所示，3D卷积的kernel不再是2D的，而是3D的。

它和多通道卷积的区别在于：

多通道卷积不同的通道上的卷积核的参数是不同的，而3D卷积则由于卷积核本身是3D的，所以这个由于“深度”造成的看似不同通道上用的就是同一个卷积

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

## 参考

https://github.com/vdumoulin/conv_arithmetic

Convolution arithmetic

http://deeplearning.net/software/theano_versions/dev/tutorial/conv_arithmetic.html

Convolution arithmetic

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
