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

----

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

# 人脸检测/识别+

https://mp.weixin.qq.com/s/pmnb_WnncL12T1IZoso5DA

DeepID

https://mp.weixin.qq.com/s/kH3-WUX4rc2SaLJcGzVLcQ

如何检测极小人脸？试试超分辨率

https://github.com/ShiqiYu/libfacedetection

libfacedetection算法开源

https://mp.weixin.qq.com/s/DkvQ86tgU4DarsmY_qTLdg

最快人脸检测遇敌手！ZQCNN vs libfacedetection

https://mp.weixin.qq.com/s/PsToV8Z17tRzXW_AbaMNmg

Blazeface人脸检测器

https://mp.weixin.qq.com/s/5ANMZNaR2ayYMUQEJ6aM3w

人脸识别基于通用表示学习

https://mp.weixin.qq.com/s/jrynQZgoljFi6zdVKWo5nw

在浏览器中实现AR试妆

https://mp.weixin.qq.com/s/lr9OVNiIN0x7k4cmNZoCUw

用于人脸检测的SSH算法

https://mp.weixin.qq.com/s/A5-Z6APwS1iFCUgYxtYNyA

重磅！GroupFace人脸识别，刷新9个数据集SOTA

https://mp.weixin.qq.com/s/-VknGwKKY14cT-lZ7sC43A

人脸识别--基于深度学习以人类为中心的图像理解

https://zhuanlan.zhihu.com/p/69542283

人脸防伪检测挑战赛-俄初创公司夺冠,中美企业位列二三

https://mp.weixin.qq.com/s/ROJtk8MSTlHJe-42-f1OYw

部分遮挡下的人脸识别技术

https://mp.weixin.qq.com/s/VOclUEBXnAtmAZmZrzQQoA

面向大规模无标注视频的人脸对齐方法

https://zhuanlan.zhihu.com/p/112140909

基于深度学习的人脸识别方法介绍

https://mp.weixin.qq.com/s/CR4Nf_RklcXA45mswPv8Zw

腾讯优图开源人脸检测算法DSFD，刷新两项数据集纪录

https://mp.weixin.qq.com/s/B8r2YEd9UbKMTT8Tup2Y_w

腾讯（优图）新技术的人脸检测（DSFD）

https://mp.weixin.qq.com/s/iI-q0s8aSojeDFbasiFVKw

人脸检测器：对DSFD的理解

https://zhuanlan.zhihu.com/p/62954487

旷视研究院新出8000点人脸关键点，堪比电影级表情捕捉

https://mp.weixin.qq.com/s/bAlaCWg4OEprpoZqQFQg1w

百度提出PyramidBox人脸检测算法

https://mp.weixin.qq.com/s/CYZvFb7kryE-8HV3teGRzA

有效遮挡检测的鲁棒人脸识别

https://mp.weixin.qq.com/s/XTIl505glmfCItuiwuPLwg

人脸聚类——Linkage Based Face Clustering via GCN

https://mp.weixin.qq.com/s/fpnq3rkcmckip30a4SlysQ

三维人脸识别研究进展综述

https://mp.weixin.qq.com/s/QEUiq5CbASd4eAM6eDPa6A

换脸算法和人脸驱动都有哪些核心技术，如何对其长期深入学习

https://mp.weixin.qq.com/s/ROTKi-Ycm0ypIi0FhCW4Xw

视频人脸识别进展综述

https://mp.weixin.qq.com/s/9d6EhZEdn6XY-wFC22LysQ

最新“深度学习人脸检测”综述论文，17页pdf概述50种人脸检测SOTA方法
