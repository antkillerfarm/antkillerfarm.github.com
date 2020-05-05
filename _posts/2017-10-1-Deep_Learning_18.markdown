---
layout: post
title:  深度学习（十八）——FCN, SegNet
category: DL 
---

# 无监督/半监督/自监督深度学习（续）

https://zhuanlan.zhihu.com/p/80815225

Image-Level弱监督图像语义分割汇总简析

https://mp.weixin.qq.com/s/5czWf0xpqva5pmuvJDn5AQ

Google研究院提出FixMatch，简单粗暴却极其有效的半监督学习方法，附14页PDF下载

https://zhuanlan.zhihu.com/p/108088719

SSL:Self-Supervised Learning(自监督学习)

https://zhuanlan.zhihu.com/p/108625273

Self-Supervised Learning入门介绍

https://zhuanlan.zhihu.com/p/108906502

Self-supervised Learning再次入门

https://mp.weixin.qq.com/s/VvUj0S2OTf8BowGRjDuVag

图解自监督学习，人工智能蛋糕中最大的一块

https://mp.weixin.qq.com/s/df51T24mBVycBeI_M7QqOQ

无标记数据学习, 83ppt

https://mp.weixin.qq.com/s/2FxD6ga6b_WOdAni16wd2Q

自监督学习在计算机视觉应用最新概述，108页ppt Self-supervised learning

https://mp.weixin.qq.com/s/3kwLoojFjJoPz4pUuEVA8g

神奇的自监督场景去遮挡

# 语义分割进阶（续）

https://mp.weixin.qq.com/s/LZNla-NKAu8wmi-OaJD8yA

旷视研究院推出可学习的树状滤波器，实现保留结构信息的特征变换

https://mp.weixin.qq.com/s/lTPm229gzV_9RU-aiY-xQg

2019年5篇图像分割算法最佳综述

https://mp.weixin.qq.com/s/n9Xdj5RKGR9cyXXNzxzvSw

基于深度学习的图像分割在高德的实践

https://blog.csdn.net/sanshibayuan/article/details/103642419

单阶段实例分割（Single Shot Instance Segmentation）

https://mp.weixin.qq.com/s/v_TTwTWx0lu2rJmxvzQQ4g

北航、旷视联合，打造最强实时语义分割网络

https://mp.weixin.qq.com/s/aSHhpMtgzV4_6NTElTvPIA

语义分割－多层特征融合

https://mp.weixin.qq.com/s/6EOkYdiVm0Lvkc24WiRbFg

基于自适应显着性的图像分割

https://mp.weixin.qq.com/s/WuWZS25aAWDLpVZQKuP2Tw

美团无人配送CVPR2020论文CenterMask解读

https://zhuanlan.zhihu.com/p/134111177

实例分割新思路: Deep Snake

# FCN

深度学习最初流行的分割方法是，打补丁式的分类方法 (patch classification) 。逐像素地抽取周围像素对中心像素进行分类。由于当时的卷积网络末端都使用全连接层 (full connected layers) ，所以只能使用这种逐像素的分割方法。

Fully Convolutional Networks是Jonathan Long和Evan Shelhamer于2014年提出的网络结构。

论文：

《Fully Convolutional Networks for Semantic Segmentation》

代码：

https://github.com/shelhamer/fcn.berkeleyvision.org

>Jonathan Long，CMU本科（2010年）+UCB博士在读。   
>个人主页：   
>https://people.eecs.berkeley.edu/~jonlong/

>Evan Shelhamer，UCB博士在读。   
>个人主页：   
>http://imaginarynumber.net/

>Trevor Darrell，University of Pennsylvania本科（1988年）+MIT硕博（1992年、1996年）。MIT教授（1999～2008）。UCB教授。   
>个人主页：   
>https://people.eecs.berkeley.edu/~trevor/

通常CNN网络在卷积层之后会接上若干个全连接层, 将卷积层产生的特征图(feature map)映射成一个固定长度的特征向量。以AlexNet为代表的经典CNN结构适合于图像级的分类和回归任务，因为它们最后都期望得到整个输入图像的一个数值描述（概率），比如AlexNet的ImageNet模型输出一个1000维的向量表示输入图像属于每一类的概率(softmax归一化)。

示例：下图中的猫, 输入AlexNet, 得到一个长为1000的输出向量, 表示输入图像属于每一类的概率, 其中在“tabby cat”这一类统计概率最高。

![](/images/article/FCN_2.png)

然而CNN网络的问题在于：全连接层会将原来二维的矩阵（图片）压扁成一维的，从而丢失了空间信息。这对于分类是没有问题的，但对于语义分割显然就不行了。

![](/images/article/FCN.png)

上图是FCN的网络结构图，它的主要思想包括：

1.采用end-to-end的结构。

2.取消FC层。当图片的feature map缩小（下采样）到一定程度之后，进行反向的上采样操作，以匹配图片的语义分割标注图。这里的上采样所采用的方法，就是《深度学习（九）》中提到的transpose convolution。

4.由于上采样会丢失信息。因此，为了更好的预测图像中的细节部分，FCN还将网络中浅层的响应也考虑进来。具体来说，就是将Pool4和Pool3的响应也拿来，分别作为模型FCN-16s和FCN-8s的输出，与原来FCN-32s的输出结合在一起做最终的语义分割预测（如下图所示）。

![](/images/article/FCN_3.png)

上图的结构在论文中被称为Skip Layer。

FCN的另一贡献是**支持任意大小的图像**。在CNN的常见操作中，Conv和Pooling都不在意图像大小。一组参数可以应用于任意大小的图像，但FC要求固定的输入维度，这决定了输入的图像的大小必须是固定的。因此，现代的CNN为了支持任意大小的图像，都有意减少或避免使用FC。

参考：

http://www.cnblogs.com/gujianhan/p/6030639.html

全卷积网络FCN详解

https://zhuanlan.zhihu.com/p/32506912

FCN的简单实现

https://mp.weixin.qq.com/s/kc0tTcTzRAT0p7q6ejXbqQ

重新发现语义分割，一文简述全卷积网络

https://www.zhihu.com/question/56688854

卷积神经网络里输入图像大小何时是固定，何时是任意？

https://mp.weixin.qq.com/s/AXfyMeFnCENIMc2qS8hNtA

10分钟看懂全卷积神经网络（FCN）：语义分割深度模型先驱

# SegNet

SegNet是Vijay Badrinarayanan于2015年提出的。

论文：

《SegNet: A Deep Convolutional Encoder-Decoder Architecture for Robust Semantic Pixel-Wise Labelling》

代码：

https://github.com/alexgkendall/caffe-segnet

除此之外，还有一个demo网站：

http://mi.eng.cam.ac.uk/projects/segnet/

>Vijay Badrinarayanan，印度人，班加罗尔大学本科（2001年）+Georgia理工硕士（2005年）+法国INRIA博士（2009年）。剑桥大学讲师。

>Alex Kendall，新西兰奥克兰大学本科（2014年）+剑桥大学博士在读。本文二作，但是代码和demo都是他写的。

>Roberto Cipolla，剑桥大学本科（1984年）+宾夕法尼亚大学硕士（1985年）+牛津大学博士（1991年）。剑桥大学教授。

![](/images/article/SegNet.png)

相比于CNN下采样阶段的结构规整，FCN上采样时的结构就显得凌乱了。因此，SegNet采用了几乎和下采样对称的上采样结构。

参考：

http://blog.csdn.net/fate_fjh/article/details/53467948

SegNet

https://mp.weixin.qq.com/s/YwmHiQ0vyFAx_dhjsmOlAQ

编解码结构SegNet
