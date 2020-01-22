---
layout: post
title:  深度学习（十）——深度图像压缩, 花式卷积（1）
category: DL 
---

# CNN进化史

## 参考（续）

https://zhuanlan.zhihu.com/p/22094600

Deep Learning回顾之LeNet、AlexNet、GoogLeNet、VGG、ResNet

https://mp.weixin.qq.com/s/28GtBOuAZkHs7JLRVLlSyg

深度卷积神经网络演化历史及结构改进脉络

http://www.leiphone.com/news/201609/303vE8MIwFC7E3DB.html

Google最新开源Inception-ResNet-v2，借助残差网络进一步提升图像分类水准

https://mp.weixin.qq.com/s/x3bSu9ecl3dldCbvS1rT1g

站在巨人的肩膀上，深度学习的9篇开山之作

http://mp.weixin.qq.com/s/2TUw_2d36uFAiJTkvaaqpA

解读Keras在ImageNet中的应用：详解5种主要的图像识别模型

https://zhuanlan.zhihu.com/p/27642620

YJango的卷积神经网络——介绍

https://www.zybuluo.com/coolwyj/note/202469

ImageNet Classification with Deep Convolutional Neural Networks

http://simtalk.cn/2016/09/20/AlexNet/

AlexNet简介

http://simtalk.cn/2016/09/12/CNNs/

CNN简介

https://mp.weixin.qq.com/s/I94gGXXW_eE5hSHIBOsJFQ

无需数学背景，读懂ResNet、Inception和Xception三大变革性架构

https://mp.weixin.qq.com/s/ToogpkDo-DpQaSoRoalnPg

没看过这5个模型，不要说你玩过CNN!

https://zhuanlan.zhihu.com/p/31006686

从LeNet-5到DenseNet

https://mp.weixin.qq.com/s/L9JY875UjXMv_slAakBrGA

从AlexNet到残差网络，理解卷积神经网络的不同架构

http://www.sohu.com/a/222873093_651893

从LeNet到SENet——卷积神经网络回顾

https://mp.weixin.qq.com/s/-d4T2hgjy6kGdd_ig_J9eg

LeCun亲授的深度学习入门课：从飞行器的发明到卷积神经网络

https://mp.weixin.qq.com/s/z26bXb8eAelrwj6Tkvm_-A

卷积神经网络常见架构AlexNet、ZFNet、VGGNet、GoogleNet和ResNet模型的理论与实践

https://mp.weixin.qq.com/s/MlEGnUPhomQn0oGGEpF9ig

通俗易懂：图解10大CNN网络架构

https://mp.weixin.qq.com/s/acvpHt4zVQPI0H5nHcg3Bw

67页综述深度卷积神经网络架构：从基本组件到结构创新

https://zhuanlan.zhihu.com/p/66215918

CNN系列模型发展简述

https://zhuanlan.zhihu.com/p/68411179

CNN网络结构的发展

# 深度图像压缩

Tiny Network Graphics是图鸭科技推出一种基于深度学习的图片压缩技术。由于商业因素，这里没有论文，技术细节也不详，但是下图应该还是有些用的。

![](/images/img2/TNG.png)

还有视频压缩：

论文：

《Deep Learning-Based Video Coding: A Review and A Case Study》

参考：

https://mp.weixin.qq.com/s/YBJwLqqL7aVUTG0LaUbwxw

深度学习助力数据压缩，一文读懂相关理论

https://mp.weixin.qq.com/s/WYsxFX4LyM562bZD8rO95w

图鸭发布图片压缩TNG，节省55%带宽

https://mp.weixin.qq.com/s/meK8UBnVHzA9YspQ2RFp6Q

体积减半画质翻倍，他用TensorFlow实现了这个图像极度压缩模型

https://mp.weixin.qq.com/s/_5tyt7pU0gIXbkmTOVEtDw

嫌图片太大？！卷积神经网络轻松实现无损压缩到20%！

https://mp.weixin.qq.com/s/a4oU8UK_hLMrKXNRQizAag

图鸭科技获CVPR 2018图像压缩挑战赛单项冠军，技术解读端到端图像压缩框架

https://mp.weixin.qq.com/s/VDyPjzXdwMGEsoXQmhrp9g

图鸭科技斩获CVPR图像压缩挑战赛冠军，TNGcnn4p技术全解读

https://mp.weixin.qq.com/s/B7reSwa9sCZqbkYVM5-VOA

图像压缩哪家强？请看这份超详细对比

https://mp.weixin.qq.com/s/K17wlC3tueNBfHkYBUFcQg

基于深度学习的HEVC复杂度优化。这是篇视频压缩的blog。

https://mp.weixin.qq.com/s/exUYS2v5VyRaMdFylWlobw

用循环神经网络进行文件无损压缩：斯坦福大学提出DeepZip

https://mp.weixin.qq.com/s/GEMOfh04XR5IyWWlvZeeng

CLIC图像压缩挑战赛冠军方案解读

https://zhuanlan.zhihu.com/p/78050429

基于深度学习的视频压缩方案介绍

https://mp.weixin.qq.com/s/gNtxBI0Alk70cEujxQmSFQ

如何将图像压缩10倍？阿里工程师有个大胆的想法！这是一篇传统算法的blog。

https://mp.weixin.qq.com/s/OkywKX4XygM8VqkL8A1fcA

TIP 2019开源论文：基于深度学习的HEVC多帧环路滤波方法

# 花式卷积

在DL中，卷积实际上是一大类计算的总称。除了原始的卷积、反卷积（Deconvolution）之外，还有各种各样的花式卷积。

## 卷积在CNN和数学领域中的概念差异

首先需要明确一点，CNN中的卷积和反卷积，实际上和数学意义上的卷积、反卷积是有差异的。

数学上的**卷积**主要用于傅立叶变换，在计算的过程中，有一个时域上的反向操作，并不是简单的向量内积运算。在信号处理领域，卷积主要用作**信号的采样**。

数学上的**反卷积**主要作为卷积的逆运算，相当于**信号的重建**，或者解微分方程。因此，它的难度远远大于卷积运算，常见的有Wiener deconvolution、Richardson–Lucy deconvolution等。

CNN的反卷积就简单多了，它只是误差的反向算法而已。因此，也有人用back convolution, transpose convolution, Fractionally Strided Convolution这样更精确的说法，来描述CNN的误差反向算法。

## Deconvolution

在《深度学习（三）》中，我们已经给出了Deconvolution的推导公式，但是并不直观。这里补充说明一下。

![](/images/article/no_padding_strides_transposed.gif)

上图是transpose convolution的操作。或者也可以看下图：

![](/images/img2/deconv.png)

上面的图主要是直观理解。实际计算中，除了使用GEMM之外，更常见的方法不是input padding，而是采用下图的办法：

![](/images/img2/deconv.jpg)

这个方法的步骤如下：

1.kernel padding。把[2, 2, Input, Output]的kernel，pad成[1, 1, Input, Output x 4]。

2.正常的conv运算，得到[W， H， Output x 4]大小的output tensor。

3.使用depth_to_space操作，将output tensor的大小变为[W x 2， H x 2， Output]。

显然，kernel padding只用算一次，且可预计算，因此计算效率上比input padding高得多。

## Deconvolution & Image Resize

Deconvolution提供了比普通的Image Resize更丰富的上采样方式。因此，常规的Image Resize操作，实际上都可用Deconvolution来做。这在某些拥有NN硬件加速的设备上是很有用的。

参考：

https://cv-tricks.com/image-segmentation/transpose-convolution-in-tensorflow/

Image Segmentation using deconvolution layer in Tensorflow

https://zhuanlan.zhihu.com/p/32414293

双线性插值的两种实现方法

http://warmspringwinds.github.io/tensorflow/tf-slim/2016/11/22/upsampling-and-image-segmentation-with-tensorflow-and-tf-slim/

Upsampling and Image Segmentation with Tensorflow and TF-Slim

## Dilated convolution

![](/images/article/dilation.gif)

上图是Dilated convolution的操作。又叫做多孔卷积(atrous convolution)。

可以看出，它和Deconvolution的差别在于，前者是kernel上有洞，而后者是Input上有洞。

和池化相比，Dilated convolution实际上也是一种下采样，只不过采样的位置是固定的，因而能够更好的保持空间结构信息。

Dilated convolution在CNN方面的应用主要是Fisher Yu的贡献。

论文：

《Multi-Scale Context Aggregation by Dilated Convolutions》

《Dilated Residual Networks》

代码：

https://github.com/fyu/dilation

https://github.com/fyu/drn

>Fisher Yu，密歇根大学本硕+普林斯顿大学博士。   
>个人主页：   
>http://www.yf.io/

和Deconvolution类似，Dilated convolution也可以采用space_to_batch和batch_to_space操作，将之转换为普通卷积。

参考：

https://zhuanlan.zhihu.com/p/28822428

Paper笔记：Dilated Residual Networks

https://mp.weixin.qq.com/s/1lMlSMS5xKc8k0QMAou45g

重新思考扩张卷积！中科院&深睿提出新型上采样模块JPU

https://mp.weixin.qq.com/s/NjFdu6iP3Sn9GbDhrbpisQ

感受野与分辨率的控制术—空洞卷积

## 分组卷积

![](/images/article/AlexNet.png)

Grouped Convolution最早在AlexNet中出现，由于当时的硬件资源有限，训练AlexNet时卷积操作不能全部放在同一个GPU处理，因此作者把feature maps分给2个GPU分别进行处理，最后把2个GPU的结果进行融合。

![](/images/img3/group_conv.png)

上图是Grouped Convolution的具体运算图：

- input分成了g组。每组的channel数只有全部的1/g。（上图中g=2）

- weight和bias也分成了g组。每组weight的input_num和output_num都只有普通卷积的1/g。也就是每组weight的尺寸只有原来的$$1/g^2$$，g组weight的总尺寸就是原来的1/g。

- 每组input和相应的weight/bias进行普通的conv运算得到一个结果。g组结果合并在一起得到一个最终结果。

可以看出，Grouped Convolution和普通Convolution的input/output的尺寸是完全一致的，只是运算方式有差异。由于group之间没有数据交换，总的计算量只有普通Convolution的1/g。

在AlexNet的Group Convolution当中，特征的通道被平均分到不同组里面，最后再通过两个全连接层来融合特征，这样一来，就只能在最后时刻才融合不同组之间的特征，对模型的泛化性是相当不利的。

为了解决这个问题，ShuffleNet在每一次层叠这种Group conv层前，都进行一次channel shuffle，shuffle过的通道被分配到不同组当中。进行完一次group conv之后，再一次channel shuffle，然后分到下一层组卷积当中，以此循环。

![](/images/img2/ShuffleNet.png)

论文：

《ShuffleNet: An Extremely Efficient Convolutional Neural Network for Mobile Devices》

代码：

https://github.com/megvii-model/ShuffleNet-Series

![](/images/img2/ShuffleNet_2.png)

上图是ShuffleNet的Unit结构图，DWConv表示depthwise convolution，GConv表示pointwise group convolution。a是普通的Deep Residual Unit，b的进化用以提高精度，c的进一步进化用以减少计算量。

参考：

https://blog.yani.io/filter-group-tutorial/

A Tutorial on Filter Groups (Grouped Convolution)

https://mp.weixin.qq.com/s/b0dRvkMKSkq6ZPm3liiXxg

旷视科技提出新型卷积网络ShuffleNet，专为移动端设计

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
