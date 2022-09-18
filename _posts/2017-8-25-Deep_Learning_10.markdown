---
layout: post
title:  深度学习（十）——花式卷积（1）
category: DL 
---

* toc
{:toc}

# CNN进化史（续）

### ZF Net

论文：

《Visualizing and understandingConvolutional Networks》

本文是Matthew D.Zeiler 和Rob Fergus于（纽约大学）2013年撰写的论文，主要通过Deconvnet（反卷积）来可视化卷积网络，来理解卷积网络，并调整卷积网络；本文通过Deconvnet技术，可视化Alex-net，并指出了Alex-net的一些不足，最后修改网络结构，使得分类结果提升。

参考：

http://blog.csdn.net/u011534057/article/details/51274862

论文阅读笔记

http://blog.csdn.net/whiteinblue/article/details/43312059

另一篇论文阅读笔记

## 总结

以下内容摘自中科视拓CEO山世光的演讲。

以让小区里的巡逻机器人学会检测狗屎为例。

在**前深度学习时代**，这个过程大概分三步：

第一步，花几个月时间收集和标注几百或上千张图；

第二步，观察并人为设计形状、颜色、纹理等特征；

第三步，尝试各种分类器做测试，如果测试结果不好，返回第二步不断地迭代。

人脸检测就是这样进行的，从上世纪八十年代开始做，大量研究者花了大概二十年时间，才得到了一个基本可用的模型，能较好地解决人脸检测的问题。而后在监控场景下做行人和车辆的检测，前后也花了大概十年的时间。就算基于这些经验，做出好用的狗屎检测器，至少还是需要一年左右的时间。

在**深度学习时代**，开发一个狗屎检测器的流程被大大缩短了。尽管深度学习需要收集大量的数据并进行标注（用矩形把图中的狗屎位置框出来），但由于众包平台的繁荣，收集一万张左右的数据可能只需要两星期。

接下来，我们只需要挑几个已经被证明有效的深度学习模型进行优化训练就可以了，训练优化大概需要一个星期，就算换几个模型再试试看。这样完成整个过程只需要一两个月而已。

而在**后深度学习时代**，我们期待先花几分钟时间，在网上随便收集几张狗屎照片，交给机器去完成余下所有的模型选择与优化工作，或许最终只需要一、两星期解决这个问题。

前深度学习时代的人脸识别的标准流程：

第一步是人脸检测，其结果就是在图片中的脸部区域打一个矩形框。

第二步是寻找眼睛、鼻子、嘴等特征点，目的是把脸对齐，也就是把眼鼻嘴放在近乎相同的位置，好像所有的脸都能“串成一串”一样，且只保留脸的中心区域，甚至连头发不要。

第三步是光照的预处理，通过高斯平滑、直方图均衡化等来进行亮度调节、偏光纠正等。

第四步是做Y=f(X)的变换。

第五步，是计算两张照片得到的Y的相似程度，如果超过特定的阈值，就认定是同一个人。

深度学习的到来对整个流程有一个巨大的冲击。

一开始，研究者用深度学习完成人脸检测、特征点定位、预处理、特征提取和识别等每个独立的步骤。而后首先被砍掉的是预处理，我们发现这个步骤是完全不必要的。理论上来解释，深度学习学出来的底层滤波器本身就可以完成光照的预处理，而且这个预处理是以“识别更准确”为目标进行的，而不是像原来的预处理一样，以“让人看得更清楚”为目标。人的知识和机器的知识其实是有冲突的，人类觉得好的知识不一定对机器识别有利。

而最近的一些工作就是把第二步特征点定位砍掉。因为神经网络也可以进行对齐变换，所以我们的工作通过空间变换（spatial transform），将图片自动按需进行矫正。并且我有一个猜测：传统的刻意把非正面照片转成正面照片的做法，也未必是有利于识别的。因为一个观察结果是，同一个人的两张正面照相似度可能小于一张正面、一张稍微转向的照片的相似度。最终，我们希望进行以识别为目标的对齐（recognition oriented alignment）。

在未来，或许检测和识别也可能合二为一。现在的检测是对一个通用的人脸的检测，未来或许可以实现检测和识别全部端到端完成：只有特定的某个人脸出现，才会触发检测框出现。

第五步的相似度（或距离测度）的计算方法存在一定的争议。我认为特征提取的过程已经通过损失函数暗含了距离测度的计算，所以深度特征提取与深度测度学习有一定的等价性。但也有不少学者在研究特征之间距离测度的学习，乃至于省略掉特征提取，直接学习输入两张人脸图片时的距离测度。

总体来说，深度学习的引入体现了端到端、数据驱动的思想：**尽可能少地对流程进行干预、尽可能少地做人为假设。**

## 参考

http://mp.weixin.qq.com/s/ZKMi4gRfDRcTxzKlTQb-Mw

计算机视觉识别简史：从AlexNet、ResNet到Mask RCNN

http://mp.weixin.qq.com/s/kbHzA3h-CfTRcnkViY37MQ

详解CNN五大经典模型:Lenet，Alexnet，Googlenet，VGG，DRL

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
