---
layout: post
title:  深度学习（九）——Bi-directional RNN, CNN进化史
category: DL 
---

* toc
{:toc}

# ResNet（续）

https://mp.weixin.qq.com/s/2JwgiCuBoluBNYesYp4zAA

ResNet及其变种的结构梳理、有效性分析与代码解读

https://mp.weixin.qq.com/s/CFKRzF9WuDrNVSivMf3YNw

目标检测新突破！了解Res2Net深度多尺度目标检测架构

https://mp.weixin.qq.com/s/1R7XWPqiDBNcUjIE-sF08Q

ResNeXt深入解读与模型实现

https://zhuanlan.zhihu.com/p/100122970

基于Keras框架的深度残差收缩网络代码

https://mp.weixin.qq.com/s/scFnuqx0zOtBvFh0JYA0UA

来聊聊ResNet及其变种

https://mp.weixin.qq.com/s/W4IqXMRZJbQ-7fGEF43-sA

真正的最强ResNet改进，高性能“即插即用”金字塔卷积

# Bi-directional RNN

众所周知，RNN在处理长距离依赖关系时会出现问题。LSTM虽然改进了一些，但也只能缓解问题，而不能解决该问题。

研究人员发现将原文倒序（将其倒序输入编码器）产生了显著改善的结果，因为从解码器到编码器对应部分的路径被缩短了。同样，两次输入同一个序列似乎也有助于网络更好地记忆。

基于这样的实验结果，1997年Mike Schuster提出了Bi-directional RNN模型。

>注：Mike Schuster，杜伊斯堡大学硕士（1993）+奈良科技大学博士。语音识别专家，尤其是日语、韩语方面。Google研究员。

论文：

《Bidirectional Recurrent Neural Networks》

下图是Bi-directional RNN的结构示意图：

![](/images/article/Bi_directional_RNN.png)

从图中可以看出，Bi-directional RNN有两个隐层，分别处理前向和后向的时序信息。

除了原始的Bi-directional RNN之外，后来还出现了Deep Bi-directional RNN。

![](/images/article/Deep_Bi_RNN.png)

上图是包含3个隐层的Deep Bi-directional RNN。

参见：

https://mp.weixin.qq.com/s/_CENjzEK1kjsFpvX0H5gpQ

结合堆叠与深度转换的新型神经翻译架构：爱丁堡大学提出BiDeep RNN

# CNN进化史

## 计算机视觉

6大关键技术：

![](/images/article/computer_vision_2.jpg)

**图像分类**：根据图像的主要内容进行分类。数据集：MNIST, CIFAR, ImageNet

**物体定位**：预测包含主要物体的图像区域，以便识别区域中的物体。数据集：ImageNet

**物体识别**：定位并分类图像中出现的所有物体。这一过程通常包括：划出区域然后对其中的物体进行分类。数据集：PASCAL, COCO

**语义分割**：把图像中的每一个像素分到其所属物体类别，在样例中如人类、绵羊和草地。数据集：PASCAL, COCO

**实例分割**：把图像中的每一个像素分到其所属物体实例。数据集：PASCAL, COCO

**关键点检测**：检测物体上一组预定义关键点的位置，例如人体上或者人脸上的关键点。数据集：COCO

参考：

https://github.com/weiaicunzai/awesome-image-classification

GitHub：图像分类最全资料集锦

https://mp.weixin.qq.com/s/nK__d-PV6DY5mDfA_UgDmQ

全解：目标检测，图像分类、分割、生成……

https://mp.weixin.qq.com/s/Go8AQay7tgykXLRtfHGLmg

改变你对世界看法的五大计算机视觉技术！

https://mp.weixin.qq.com/s/WNkzfvYtEO5zJoe_-yAPow

一文览尽计算机视觉研究方向

https://zhuanlan.zhihu.com/p/55747295

深度学习在计算机视觉领域（包括图像，视频，3-D点云，深度图）的应用一览

## CNN简史

![](/images/article/computer_vision_3.jpg)

![](/images/article/CNN_3.png)

完整版本参见：

https://github.com/Nikasa1889/HistoryObjectRecognition/blob/master/HistoryOfObjectRecognition.pdf

![](/images/img3/CNN.jpg)

参考：

https://mp.weixin.qq.com/s/K68CpueI4e4y7o1uZ28KMQ

从神经科学到计算机视觉：人类与计算机视觉五十年回顾

https://mp.weixin.qq.com/s/FzCrOiFuutqSQSp4VcydoQ

计算机视觉简介：历史、现状和发展趋势

## AlexNet

2012年，ILSVRC比赛冠军的model——Alexnet（以第一作者Alex命名）的结构图如下：

![](/images/article/AlexNet.png)

换个视角：

![](/images/article/AlexNet_2.png)

AlexNet的caffe模板：

https://github.com/BVLC/caffe/blob/master/models/bvlc_alexnet/deploy.prototxt

其中的LRN（Local Response Normalization）层也是当年的遗迹，被后来的实践证明，对于最终效果和运算量没有太大帮助，因此也就慢慢废弃了。

虽然，LeNet-5是CNN的开山之作（它不是最早的CNN，但却是奠定了现代CNN理论基础的模型），但是毕竟年代久远，和现代实用的CNN相比，结构实在过于原始。

AlexNet作为第一个现代意义上的CNN，它的意义主要包括：

1.Data Augmentation。包括水平翻转、随机裁剪、平移变换、颜色、光照变换等。

2.Dropout。

3.ReLU激活函数。

4.多GPU并行计算。

5.当然最应该感谢的是李飞飞团队搞出来的标注数据集合ImageNet。

>注：ILSVRC（Large Scale Visual Recognition Challenge）大赛，在2016年以前，一直是CV界的顶级赛事。但随着技术的成熟，目前的科研重点已经从物体识别转移到了物体理解领域。2017年将是该赛事的最后一届。WebVision有望接替该赛事，成为下一个目标。

参考：

https://zhuanlan.zhihu.com/p/22538465

运用CNN对ImageNet进行图像分类

## VGG

Visual Geometry Group是牛津大学的一个科研团队。他们推出的一系列深度模型，被称作VGG模型。

代码：

http://www.robots.ox.ac.uk/~vgg/research/very_deep/

VGG的结构图如下：

![](/images/article/vgg.png)

该系列包括A/A-LRN/B/C/D/E等6个不同的型号。其中的D/E，根据其神经网络的层数，也被称为VGG16/VGG19。

从原理角度，VGG相比AlexNet并没有太多的改进。其最主要的意义就是实践了“**神经网络越深越好**”的理念。也是自那时起，神经网络逐渐有了“深度学习”这个别名。

参考：

https://zhuanlan.zhihu.com/p/37706726

VGG论文笔记

https://zhuanlan.zhihu.com/p/344324470

RepVGG：极简架构，SOTA性能，让VGG式模型再次伟大

## GoogleNet

GoogleNet的进化道路和VGG有所不同。VGG实际上就是“大力出奇迹”的暴力模型，其他地方不足称道。

而GoogleNet不仅继承了VGG“越深越好”的理念，对于网络结构本身也作了大胆的创新。可以对比的是，AlexNet有60M个参数，而GoogleNet只有4M个参数。

因此，在ILSVRC 2014大赛中，GoogleNet获得第一名，而VGG屈居第二。

![](/images/article/GoogleNet.jpg)

上图是GoogleNet的结构图。从中可以看出，GoogleNet除了AlexNet的基本要素之外，还有被称作Inception的结构。

![](/images/article/inception.png)

上图是Inception的结构图。它的原理实际上就是**将不同尺寸的卷积组合起来，以提供不同尺寸的特征**。

原始的GoogleNet也被称作Inception-v1。在后面的几年，GoogleNet还提出了几种改进的版本，最新的一个是Inception-v4（2016.8）。

论文：

《Inception-v4, Inception-ResNet and the Impact of Residual Connections on Learning》

代码：

https://github.com/BVLC/caffe/tree/master/models/bvlc_googlenet

Inception系列的改进方向基本都集中在构建不同的Inception模型上。

GoogleNet的另一个改进是**减少了全连接层**（Full Connection, FC），这是减少模型参数的一个重要改进。事实上，在稍后的实践中，人们发现去掉VGG的第一个FC层，对于效果几乎没有任何影响。

参考：

https://mp.weixin.qq.com/s/ceOxFS3Iwf3iLWY73ueoNw

GoogLeNet中的inception结构，你看懂了吗

http://www.cnblogs.com/Allen-rg/p/5833919.html

GoogLeNet学习心得

## SqueezeNet

GoogleNet之后，最有名的CNN模型当属何恺明的Deep Residual Network。DRN在《深度学习（六）》中已有提及，这里不再赘述。

DRN之后，学界的研究重点，由如何提升精度，转变为如何用更少的参数和计算量来达到同样的精度。SqueezeNet就是其中的代表。

论文：

《SqueezeNet: AlexNet-level accuracy with 50x fewer parameters and <0.5MB model size》

代码：

https://github.com/DeepScale/SqueezeNet

Caffe版本

https://github.com/vonclites/squeezenet

TensorFlow版本

![](/images/article/SqueezeNet_2.png)

上图是SqueezeNet的网络结构图，其最大的创新点在于使用Fire Module替换大尺寸的卷积层。

![](/images/article/SqueezeNet.png)

上图是Fire Module的结构示意图。它采用squeeze层+expand层两个小卷积层，替换了AlexNet的大尺寸卷积层。其中，$$N_{squeeze}<N_{expand}$$，N表示每层的卷积个数。

这里需要特别指出的是：expand层采用了2种不同尺寸的卷积，这也是当前设计的一个趋势。

这个趋势在GoogleNet中已经有所体现，在ResNet中也间接隐含。

![](/images/article/expand_ResNet.png)

上图是ResNet的展开图，可见展开之后的ResNet，实际上等效于一个多尺寸交错混编的复杂卷积网。其思路和GoogleNet实际上是一致的。

参见：

http://blog.csdn.net/xbinworld/article/details/50897870

最新SqueezeNet模型详解，CNN模型参数降低50倍，压缩461倍！

http://www.jianshu.com/p/8e269451795d

神经网络瘦身：SqueezeNet

http://blog.csdn.net/shenxiaolu1984/article/details/51444525

超轻量级网络SqueezeNet算法详解

https://mp.weixin.qq.com/s/euppu_2rhujlWz1z5S5nYA

纵览轻量化卷积神经网络：SqueezeNet、MobileNet、ShuffleNet、Xception

https://mp.weixin.qq.com/s/rkTL1cj7tG5FaLP9cYRb4A

CNN模型之SqueezeNet

https://mp.weixin.qq.com/s/kE2WW3EaLsEoYTQdzL30RA

SqueezeNet/SqueezeNext简述

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

https://mp.weixin.qq.com/s/H_KY_JbqiZ1q7VLwAf2EDA

致敬Network in Network，华为诺亚提出Transformer-in-Transformer
