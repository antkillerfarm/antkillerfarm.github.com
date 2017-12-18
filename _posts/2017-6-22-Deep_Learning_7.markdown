---
layout: post
title:  深度学习（七）——seq2seq, DMN, CNN进化史
category: DL 
---

# Attention（续）

https://mp.weixin.qq.com/s/xr_1ZYbvADMMwgxLEAflCw

如何在语言翻译中理解Attention Mechanism？

https://mp.weixin.qq.com/s/Nyq_36aFmQYRWdpgbgxpuA

将注意力机制引入RNN，解决5大应用领域的序列预测问题

https://mp.weixin.qq.com/s/2gxp7A38epQWoy7wK8Nl6A

谷歌翻译最新突破，“关注机制”让机器读懂词与词的联系

https://mp.weixin.qq.com/s/g2PcmsDW9ixUCh_yP8W-Vg

各类Seq2Seq模型对比及《Attention Is All You Need》中技术详解

https://mp.weixin.qq.com/s/FtI94xY6a8TEvFCHfjMnmA

小组讨论谷歌机器翻译Attention is All You Need

https://mp.weixin.qq.com/s/SqIMkiP1IZMGWzwZWGOI7w

谈谈神经网络的注意机制和使用方法

# seq2seq

seq2seq最早用于Neural Machine Translation领域（与之相对应的有Statistical Machine Translation）。训练后的seq2seq模型，可以根据输入语句，自动生成翻译后的输出语句。

![](/images/article/seq2seq.png)

上图是seq2seq的结构图。可以看出seq2seq实际上是一种Encoder-Decoder结构。

在Encoder阶段，RNN依次读入输入序列。但由于这时，没有输出序列与之对应，因此这仅仅相当于一个对隐层的编码过程，即将句子的语义编码为隐层的状态向量。

从中发现一个问题：状态向量的维数决定了存储的语义的内容上限（显然不能指望，一个200维的向量，能够表示一部百科全书。）因此，seq2seq通常只用于短文本的翻译。

在Decoder阶段，我们根据输出序列，反向修正RNN的参数，以达到训练神经网络的目的。

参考：

https://github.com/ematvey/tensorflow-seq2seq-tutorials

一步步的seq2seq教程

http://blog.csdn.net/sunlylorn/article/details/50607376

seq2seq模型

http://datartisan.com/article/detail/120.html

Seq2Seq的DIY简介

http://www.cnblogs.com/Determined22/p/6650373.html

DL4NLP——seq2seq+attention机制的应用：文档自动摘要（Automatic Text Summarization）

http://blog.csdn.net/young_gy/article/details/73412285

基于RNN的语言模型与机器翻译NMT

http://karpathy.github.io/2015/05/21/rnn-effectiveness/

The Unreasonable Effectiveness of Recurrent Neural Networks

https://mp.weixin.qq.com/s/8u3v9XzECkwcNn5Ay-kYQQ

基于Depthwise Separable Convolutions的Seq2Seq模型_SliceNet原理解析

https://mp.weixin.qq.com/s/H6eYxS7rXGDH_B8Znrxqsg

seq2seq中的beam search算法过程

https://mp.weixin.qq.com/s/U1yHIc5Zq0yKCezRm185VA

Attentive Sequence to Sequence Networks

https://mp.weixin.qq.com/s/cGXANj7BB2ktTdPAL4ZEWA

图解神经网络机器翻译原理：LSTM、seq2seq到Zero-Shot

https://mp.weixin.qq.com/s/jYUAKyTpm69J6Q34A06E-w

百度提出冷聚变方法：使用语言模型训练Seq2Seq模型

https://mp.weixin.qq.com/s/Fp6G1aI_utDd_kTbdHvEVQ

完全基于卷积神经网络的seq2seq

http://localhost:4500/theory/2017/06/21/Deep_Learning_6.html

从2017年顶会论文看Attention Model

# DMN

Question answering是自然语言处理领域的一个复杂问题。它需要对文本的理解力和推理能力。大部分NLP问题都可以转化为一个QA问题。Dynamic Memory Networks可以用来处理QA问题。DMN的输入包含事实输入，问题输入，经过内部处理形成片段记忆，最终产生问题的答案。

DMN可进行端到端的训练，并在多种任务上取得了state-of-the-art的效果：包括QA（Facebook 的 bAbI 数据集），情感分析文本分类（Stanford Sentiment Treebank）和词性标注（WSJ-PTB）。

![](/images/article/DMN.png)

参考：

http://blog.csdn.net/javafreely/article/details/71994247

动态记忆网络

# CNN进化史

## 计算机视觉

![](/images/article/computer_vision.jpg)

6大关键技术：

![](/images/article/computer_vision_2.jpg)

**图像分类**：根据图像的主要内容进行分类。数据集：MNIST, CIFAR, ImageNet

**物体定位**：预测包含主要物体的图像区域，以便识别区域中的物体。数据集：ImageNet

**物体识别**：定位并分类图像中出现的所有物体。这一过程通常包括：划出区域然后对其中的物体进行分类。数据集：PASCAL, COCO

**语义分割**：把图像中的每一个像素分到其所属物体类别，在样例中如人类、绵羊和草地。数据集：PASCAL, COCO

**实例分割**：把图像中的每一个像素分到其所属物体实例。数据集：PASCAL, COCO

**关键点检测**：检测物体上一组预定义关键点的位置，例如人体上或者人脸上的关键点。数据集：COCO

参考：

https://mp.weixin.qq.com/s/nK__d-PV6DY5mDfA_UgDmQ

全解：目标检测，图像分类、分割、生成……

## CNN简史

![](/images/article/computer_vision_3.jpg)

![](/images/article/CNN_3.png)

完整版本参见：

https://github.com/Nikasa1889/HistoryObjectRecognition/blob/master/HistoryOfObjectRecognition.pdf

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

## VGG

Visual Geometry Group是牛津大学的一个科研团队。他们推出的一系列深度模型，被称作VGG模型。

代码：

http://www.robots.ox.ac.uk/~vgg/research/very_deep/

VGG的结构图如下：

![](/images/article/vgg.png)

该系列包括A/A-LRN/B/C/D/E等6个不同的型号。其中的D/E，根据其神经网络的层数，也被称为VGG16/VGG19。

从原理角度，VGG相比AlexNet并没有太多的改进。其最主要的意义就是实践了“**神经网络越深越好**”的理念。也是自那时起，神经网络逐渐有了“深度学习”这个别名。

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



