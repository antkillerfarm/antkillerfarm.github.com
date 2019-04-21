---
layout: post
title:  深度学习（九）——CNN进化史（2）, 并行 & 框架
category: DL 
---

# CNN进化史

### SqueezeNet（续）

http://blog.csdn.net/shenxiaolu1984/article/details/51444525

超轻量级网络SqueezeNet算法详解

https://mp.weixin.qq.com/s/euppu_2rhujlWz1z5S5nYA

纵览轻量化卷积神经网络：SqueezeNet、MobileNet、ShuffleNet、Xception

https://mp.weixin.qq.com/s/rkTL1cj7tG5FaLP9cYRb4A

CNN模型之SqueezeNet

## 其他知名CNN

### Network In Network

Network In Network是颜水成团队于2013年提出的。

论文：

《Network In Network》

![](/images/article/nin.png)

>颜水成，北京大学博士，新加坡国立大学副教授。奇虎360AI研究院院长。

Network In Network最重要的贡献是使用Global Average Pooling替换了Full Connection。这直接促进了之后Fully Convolutional Networks的发展。

参考：

http://blog.csdn.net/sheng_ai/article/details/41313883

Network In Network(精读)

http://blog.csdn.net/zhufenghao/article/details/52526611

Network In Network

http://www.cnblogs.com/dmzhuo/p/5868346.html

读论文“Network in Network”——ICLR 2014

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

# 并行 & 框架

## 论文

《Demystifying Parallel and Distributed Deep Learning: An In-Depth Concurrency Analysis》

## 参考

https://zhuanlan.zhihu.com/p/58806183

深度学习的分布和并行处理系统

https://zhuanlan.zhihu.com/p/26552293

Dataflow架构和神经网络加速器

https://zhuanlan.zhihu.com/p/28445511

浅析深度学习框架设计中的关键技术

https://mp.weixin.qq.com/s/B4aQp_0YvS0jyUHNLQ5rRA

IBM发布新型分布式深度学习系统：结合软硬件实现当前最优性能

http://engineering.skymind.io/distributed-deep-learning-part-1-an-introduction-to-distributed-training-of-neural-networks

神经网络的分布式训练

https://mp.weixin.qq.com/s/nvuflLfOolidDDXJVe2DZA

美团深度学习系统的工程实践

https://mp.weixin.qq.com/s/IE6blClvhYlq3-QAGHo5ww

TensorFlow分布式计算机制解读：以数据并行为重

https://mp.weixin.qq.com/s/7JGLm-hkCZBNDLA98qvWNA

自动生成硬件优化内核：陈天奇等人发布深度学习编译器TVM

https://mp.weixin.qq.com/s/4Ii3um3jqfm5yKKxZAFdmA

继1小时训练ImageNet之后，大批量训练扩展到了3万2千个样本

https://mp.weixin.qq.com/s/kOCftzSbHe2mvDmlRp-ihA

Jeff Dean：AI对计算机系统设计的影响

https://mp.weixin.qq.com/s/XjNPaL6PC9LHX1PEGn5UZg

微软实时AI系统“脑波计划”有多牛？看完秒懂！

https://mp.weixin.qq.com/s/OkqUulFYHQSdgAbf9Fi9LA

CoCoA：大规模机器学习的分布式优化通用框架

https://mp.weixin.qq.com/s/ToIDncp9dS_qk47PsdZm5A

杜克大学：分布式深度学习训练算法TernGrad

https://mp.weixin.qq.com/s/rhtrN2qDspGkpJYDAVSX7w

UC Berkeley展示全新并行处理方法

https://mp.weixin.qq.com/s/ASqpPSIgW_bcFPBfRYz7Xg

哈佛大学提出在云、边缘与终端设备上的分布式深度神经网络DDNN

http://blog.sina.com.cn/s/blog_81f72ca70101kuk9.html

《Large Scale Distributed Deep Networks》中译文

https://mp.weixin.qq.com/s/X7XG51yohLnEZ_Jg6XK9oQ

Caffe作者贾扬清教你怎样打造更加优秀的深度学习架构

https://mp.weixin.qq.com/s/_mrYI7McMBUx0lEh4rNiYQ

百度开源移动端深度学习框架MDL，手机部署CNN支持iOS GPU

https://mp.weixin.qq.com/s/qOjGrR59Mf0Mzgh4bpDhrA

详解Horovod：Uber开源的TensorFlow分布式深度学习框架

https://mp.weixin.qq.com/s/ZCNSq5FC2REoVTKAK2mJQg

分布式深度学习原理、算法详细介绍

https://mp.weixin.qq.com/s/Ewiil56vMkzhO2xDWgo-Wg

苹果发布Turi Create机器学习框架，5行代码开发图像识别

https://mp.weixin.qq.com/s/jOVUPhrCBI9W9vPvD9eKYg

UC Berkeley提出新型分布式框架Ray：实时动态学习的开端
