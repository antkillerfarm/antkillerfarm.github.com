---
layout: post
title:  深度学习（四十九）——Fast Image Processing, DMN, 图像超分辨率进阶
category: DL 
---

* toc
{:toc}

# Fast Image Processing

论文：

《Fast Image Processing with Fully-Convolutional Networks》

![](/images/article/FIP.png)

上图是照片界常用的几种修图方式之一。一般将这些图片风格转换的算法，称为图像处理算子（image processing operators）。如何加速image processing operators的计算，就成为了学界研究的课题之一。

本文提出的模型就是用来加速image processing operators计算的。它是Intel Lab的Qifeng Chen和Jia Xu于2017年提出的。

代码：

https://github.com/CQFIO/FastImageProcessing

Demo网站：

http://cqf.io/ImageProcessing/

这个课题一般使用MIT-Adobe FiveK Dataset作为基准数据集。网址：

http://groups.csail.mit.edu/graphics/fivek_dataset/

这个数据集包含了5K张原始照片，并雇用了5个专业修图师，对每张图片进行修图。

众所周知，多层神经网络只要有足够的深度和宽度，就可以任意逼近任意连续函数。然而从Fast Image Processing的目的来说，神经网络的深度和宽度注定是有限的，否则肯定快不了。而这也是该课题的研究意义所在。

本文只使用了MIT-Adobe数据集中的原始图片，并使用了10种常用的算子对图片进行处理。因此，该网络训练时的输入是原始图片，而输出是处理后的图片。

![](/images/article/MCA.png)

上图是本文模型的网络结构图。它的设计特点如下：

1.采用Multi-Scale Context Aggregation作为基础网络。MCA的内容参见《深度学习（十）》中的Dilated convolution一节。

2.传统MCA一般有下采样的过程，但这里由于网络输入和输出的尺寸维度是一样的，因此，所有的feature maps都是等大的。

3.借鉴FCN的思想，去掉了池化层和全连接层。

4.L1~L3主要用于图片的特征提取和升维，而L4~L5则用于特征的聚合和降维，并最终和输出数据的尺寸维度相匹配。

在normalization方面，作者发现有的operators经过normalization之后，精度会上升，而有的精度反而会下降，因此为了统一模型，定义如下的normalization运算：

$$\Psi^s(x)=\lambda_sx+\mu_sBN(x)$$

Loss函数为：

$$\mathcal{l(K,B)}=\sum_i\frac{1}{N_i}\|\hat f (I_i;\mathcal{K,B})-f(I_i)\|^2$$

这实际上就是RGB颜色空间的MSE误差。

为了检验模型的泛化能力，本文还使用RAISE数据集作为交叉验证的数据集。该数据集的网址：

http://mmlab.science.unitn.it/RAISE/

RAISE数据集包含了8156张高分辨率原始照片，由3台不同的相机拍摄，并给出了相机的型号和参数。

# DMN

Question answering是自然语言处理领域的一个复杂问题。它需要对文本的理解力和推理能力。大部分NLP问题都可以转化为一个QA问题。Dynamic Memory Networks可以用来处理QA问题。DMN的输入包含事实输入，问题输入，经过内部处理形成片段记忆，最终产生问题的答案。

DMN可进行端到端的训练，并在多种任务上取得了state-of-the-art的效果：包括QA（Facebook的bAbI数据集），情感分析文本分类（Stanford Sentiment Treebank）和词性标注（WSJ-PTB）。

![](/images/article/DMN.png)

参考：

http://blog.csdn.net/javafreely/article/details/71994247

动态记忆网络

# 图像超分辨率进阶

亚像素尺度上对物体进行计数，是一类和超分辨率很相关的CV任务。

https://mp.weixin.qq.com/s/Zp5jlUspiocEZ6CI1cWJZw

深度学习能看到的比你更多，亚像素物体计数方法介绍

---

![](/images/img4/SR.jpg)

https://zhuanlan.zhihu.com/p/266472242

一文带你入门超分网络及其渐进式上采样方法

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

https://mp.weixin.qq.com/s/h4Xzt-aS1_-5zjTB0ypTLg

普通视频转高清：10个基于深度学习的超分辨率神经网络

https://mp.weixin.qq.com/s/WmqagSGRy98USgnz21W3Pg

WDSR

https://mp.weixin.qq.com/s/oNavFLOPskHNxWIyBbFzHw

WDSR (NTIRE2018 超分辨率冠军)

https://mp.weixin.qq.com/s/AxHTaT-G5_Y6Iw_3aIIxCg

超分辨率技术如何发展？这6篇ECCV 18论文带你一次尽览

https://mp.weixin.qq.com/s/zWoQCKbZNz2td3cZxEsqKQ

腾讯优图提出SRN-DeblurNet：高效高质量去除复杂图像模糊

https://mp.weixin.qq.com/s/eJkkbGBYxWlngkT5gjjW7g

效果惊人：上古卷轴III等经典游戏也能使用超分辨率GAN重制了

https://mp.weixin.qq.com/s/M8gCrQDtjT1lszsxV2QQKg

FSRNet：端到端深度可训练人脸超分辨网络

https://mp.weixin.qq.com/s/fkHfzkHRFpEGc-L4uGlqcg

从网络设计到实际应用，深度学习图像超分辨率综述

https://mp.weixin.qq.com/s/C9Ku4MvU2QuZ_GJcs8FelA

基于深度学习的图像超分辨率最新进展与趋势

https://mp.weixin.qq.com/s/qNHbs2Bd4buVlu0j3Rtj0g

Adobe提出新型超分辨率方法：用神经网络迁移参照图像纹理

https://mp.weixin.qq.com/s/N-kAIK_qMowsFqEtWTqt4w

图像超分辨率重建--工程应用

https://mp.weixin.qq.com/s/JaPYUWvh7RBHVUYgKlTeHw

旷视提出超分辨率新方法Meta-SR：单一模型实现任意缩放因子

https://mp.weixin.qq.com/s/aEu0q04pgXUE1mqAS5kewQ

不用P30 Pro，普通手机也能变身望远镜：陈启峰团队新作，登上CVPR 2019

https://mp.weixin.qq.com/s/CXDzPSSyObPHYc40YictKQ

CVPR 2019 神奇的超分辨率算法DPSR：应对图像模糊降质

https://mp.weixin.qq.com/s/Rr4AKGjZNyV3PDoRBVH-lw

低清视频也能快速转高清：超分辨率算法TecoGAN

https://mp.weixin.qq.com/s/PIq78vhNQxAKntnZilL4pQ

分割、检测与定位，高分辨率网络显神威！这会是席卷深度学习的通用结构吗？

https://mp.weixin.qq.com/s/G55dxHfMYxWzjz4_8YnUaw

深度学习超分辨率最新综述：一文道尽技术分类与效果评测

https://zhuanlan.zhihu.com/p/67613641

基于多级神经纹理迁移的图像超分辨方法(Adobe Research)

https://mp.weixin.qq.com/s/IStOD22WgZ6EBFCvIHkNSg

图像超分辨率网络：RCAN

https://mp.weixin.qq.com/s/4LIq3kZaXgaoEEyb8TQvwg

图像超分辨率重建--AI研究

https://mp.weixin.qq.com/s/ETGPXVDGRHOq0W-ef_q2_A

RankSRGAN:排序学习+GAN用于超分辨率

https://zhuanlan.zhihu.com/p/140507840

图像超分：USRNet

https://mp.weixin.qq.com/s/sju4SYFxzDkJevk3mi68Rw

图像超分辨率网络：EDSR

https://mp.weixin.qq.com/s/uZQK0oQbV7wm0WVz_QxVQg

DRN：用于单图像超分辨率的对偶回归网络

https://mp.weixin.qq.com/s/tE_-gUBmt_0h75CnkViUHA

超分辨率技术:Adobe Photoshop与深度神经网络对比

# Transformer进阶+

https://mp.weixin.qq.com/s/MjCIAlDWyHPLj_sGSPc4rg

复旦邱锡鹏组最新综述：A Survey of Transformers

https://mp.weixin.qq.com/s/-Y7Qy-5aJNJ5bx8QJf3k2w

Transformer及其变种

https://mp.weixin.qq.com/s/nSokDcIkOSSrRnhHCuu4Mg

Transformer家族简史（PART I）

https://mp.weixin.qq.com/s/p919Kfv-1GSDM6u6FpnBsA

Transformer家族简史（PART II）

https://mp.weixin.qq.com/s/M0zLw9hA5xzontKB7Zj23Q

Memory Transformer，一种简单明了的Transformer改造方案

https://mp.weixin.qq.com/s/FJeZ8X9gtyciqCTs9zvlLA

Transformer是CNN是GNN是RNN，Attention is all you need！

https://mp.weixin.qq.com/s/d1qqRw7sWyLdoyfnqMBdJQ

深度自适应Transformer

https://mp.weixin.qq.com/s/UowNtBm_hqnes-Lz3POXGQ

Transformers中的Beam Search高效实现

https://mp.weixin.qq.com/s/KdKbOrjeeo7Db095V7mSFA

Transformer之自适应宽度注意力

https://mp.weixin.qq.com/s/EuCCeWz_rkktwLuFJ75BXA

Transformer+AutoML: 遗传搜索在序列式任务上的应用

https://mp.weixin.qq.com/s/OEpLpWzkdfFUQf4cKNuG4w

Performer:基于正交随机特征的快速注意力计算

https://mp.weixin.qq.com/s/eWQLkiJ_XIo7LpTUE9c0qA

Transformer中的相对位置编码

https://mp.weixin.qq.com/s/mZBHjuHJG9Ffd0nSoJ2ISQ

什么是Transformer位置编码？

https://mp.weixin.qq.com/s/V0NAOgluyZN9P8iuhMKRwQ

Transformer为啥在NER上表现不好

https://mp.weixin.qq.com/s/ANFSNW1-mcjPqjcroNHeZQ

RealFormer：Real简单，Real有效

https://mp.weixin.qq.com/s/u-Twg6Cj6VfL6m4K0seBlw

谷歌研究院出品：高效Transformer模型最新综述

https://mp.weixin.qq.com/s/2S_2Z5-ioCNxH1kqFcUuQA

竞赛中的Transformer家族

https://mp.weixin.qq.com/s/mc6M2vEcPG6oMfKe3_apzQ

Transformer变体层出不穷，它们都长什么样？

https://mp.weixin.qq.com/s/IWUxVzpdGIX1Oxn4KxjhHA

一个Transformer，很强；两个，更强？（TransGAN）

https://mp.weixin.qq.com/s/IWUxVzpdGIX1Oxn4KxjhHA

TransGAN：两个Transformer可以构造一个强大的GAN

# 并行 & 框架 & 优化+

https://mp.weixin.qq.com/s/HQW2bPyDY_3ecZWP6NYr-w

大规模机器学习在LinkedIn预测模型中的应用实践

https://mp.weixin.qq.com/s/i1PLA1xr3CefKx1EcVUVIg

谷歌破世界纪录！圆周率计算到小数点后31.4万亿位

https://mp.weixin.qq.com/s/rX8L63-jDGJT6lCAj04I3Q

独家解读！阿里重磅发布机器学习平台PAI 3.0

https://mp.weixin.qq.com/s/Ye2GVTFIrX3SbU1-4cDLoQ

你天天叫的外卖，你知道这里面深度学习的水有多深吗

https://mp.weixin.qq.com/s/FIWfbCLgckVzeNvfThIl4Q

阿里线下智能方案进化史

https://mp.weixin.qq.com/s/pqxiF6yEZzrw8qXu2hEsaA

单机训练速度提升640倍！独家解读快手商业广告模型GPU训练平台Persia

https://mp.weixin.qq.com/s/Jcz4XWDjMmbhmAiI_zBQXQ

流式计算优化：时效性

https://zhuanlan.zhihu.com/p/33351291

基于忆阻器（ReRAM），Computing-in-Memory的DLA

https://mp.weixin.qq.com/s/UbZtUL6Iveb4S3nTU0liGw

深度神经网络的分布式训练概述：常用方法和技巧全面总结

https://mp.weixin.qq.com/s/kLXJsHbBnRIFC3NLChPhzA

如何高效进行大规模分类？港中文联合商汤提出新方法

https://www.zhihu.com/question/454589636

为什么模型和数据都在gpu上，却打不满GPU的使用率？
