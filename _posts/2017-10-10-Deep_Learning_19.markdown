---
layout: post
title:  深度学习（十九）——SegNet, DeconvNet, DeepLab, ENet, GCN, Ultra Deep Network
category: DL 
---

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

# DeconvNet

DeconvNet是韩国的Hyeonwoo Noh于2015年提出的。

论文：

《Learning Deconvolution Network for Semantic Segmentation》

代码：

https://github.com/HyeonwooNoh/DeconvNet

![](/images/article/DeconvNet.jpg)

从上图可见，DeconvNet和SegNet的结构非常类似，只不过DeconvNet在encoder和decoder之间使用了FC层作为中继。

这样的encoder-decoder对称结构也被称为U-Net（因为它们的形状像U字形）：

![](/images/article/U_Net.jpg)

# DeepLab

DeepLab共有3个版本，分别对应3篇论文：

《Semantic Image Segmentation with Deep Convolutional Nets and Fully Connected CRFs》

《DeepLab: Semantic Image Segmentation with Deep Convolutional Nets, Atrous Convolution, and Fully Connected CRFs》

《Rethinking Atrous Convolution for Semantic Image Segmentation》

>Liang-Chieh(Jay) Chen，台湾国立交通大学本科（2004年）+密歇根大学硕士（2010年）+UCLA博士（2015年）。现为Google研究员。   
>个人主页：   
>http://liangchiehchen.com/

DeepLab针对FCN主要做了如下改进：

1.用Dilated convolution取代Pooling操作。因为前者能够更好的保持空间结构信息。

2.使用全连接条件随机场（Dense Conditional Random Field）替换最后的Softmax层。这里的CRF或者Softmax，也被称为语义分割网络的后端。

常见的后端还有Markov Random Field、Gaussian CRF等。这些都与概率图模型（Probabilistic Graphical Models）有关。

总之，目前的主流一般是**FCN+PGM**的模式。然而后端的计算模式和普通的NN有所差异，因此如何将后端NN化，也是当前研究的关键点。

# ENet

ENet是波兰的Adam Paszke于2016年提出的。

论文：

《ENet: A Deep Neural Network Architecture for Real-Time Semantic Segmentation》

代码：

https://github.com/TimoSaemann/ENet

![](/images/article/ENet.png)

ENet的网络结构如上图所示。其中的initial和bottleneck结构分别见下图的(a)和(b)：

![](/images/article/ENet_2.png)

从大的结构来看，ENet的设计主要参考了Resnet和SqueezeNet。

ENet对Pooling操作进行了一定的修改：

1.下采样时，除了输出Pooling值之外，还输出Pooling值的位置，即所谓的Pooling Mask。

2.上采样时，利用第1步的Pooling Mask信息，获得更好的精确度。

显然这个修改在思路上和Dilated convolution是非常类似的。

参考：

http://blog.csdn.net/zijinxuxu/article/details/67638290

论文中文版blog

# Global Convolutional Network

Global Convolutional Network是孙剑团队的Chao Peng于2017年提出的。

论文：

《Large Kernel Matters -- Improve Semantic Segmentation by Global Convolutional Network》

>孙剑，西安交通大学博士（2003年）。后一直在微软亚洲研究院工作，担任首席研究员。2016年7月正式加入旷视科技担任首席科学家。

![](/images/article/GCN.png)

上图是论文的关键结构GCN，它主要用于计算超大卷积核。这里借鉴了Separable convolution的思想（将一个k x k的卷积运算，转换成1 x k + k x 1的卷积运算）。

然而正如我们在《深度学习（九）》中指出的，不是所有的卷积核都满足可分离条件。单纯采用先1 x k后k x 1，或者先k x 1后1 x k，效果都是不好的。而将两者结合起来，可以有效提高计算的精度。

![](/images/article/GCN_2.png)

这是GCN提出的另一个新结构。

![](/images/article/GCN_3.png)

上图是GCN的整体结构图。

参考：

http://blog.csdn.net/bea_tree/article/details/60977512

旷视最新：Global Convolutional Network

# 语义分割的展望

俗话说，“没有免费的午餐”（“No free lunch”）。基于深度学习的图像语义分割技术虽然可以取得相比传统方法突飞猛进的分割效果，但是其对数据标注的要求过高：不仅需要海量图像数据，同时这些图像还需提供精确到像素级别的标记信息（Semantic labels）。因此，越来越多的研究者开始将注意力转移到弱监督（Weakly-supervised）条件下的图像语义分割问题上。在这类问题中，图像仅需提供图像级别标注（如，有“人”，有“车”，无“电视”）而不需要昂贵的像素级别信息即可取得与现有方法可比的语义分割精度。

另外，示例级别（Instance level）的图像语义分割问题也同样热门。该类问题不仅需要对不同语义物体进行图像分割，同时还要求对同一语义的不同个体进行分割（例如需要对图中出现的九把椅子的像素用不同颜色分别标示出来）。

![](/images/article/Instance_level.jpg)

最后，基于视频的前景／物体分割（Video segmentation）也是今后计算机视觉语义分割领域的新热点之一，这一设定其实更加贴合自动驾驶系统的真实应用环境。

# Ultra Deep Network

## FractalNet

论文：

《FractalNet: Ultra-Deep Neural Networks without Residuals》

![](/images/article/FractalNet.png)

## Resnet in Resnet

论文：

《Resnet in Resnet: Generalizing Residual Architectures》

![](/images/article/RiR.png)

## Highway

论文：

《Training Very Deep Networks》

![](/images/article/highway.png)

Resnet对于残差的跨层传递是无条件的，而Highway则是有条件的。这种条件开关被称为gate，它也是由网络训练得到的。

## DenseNet

DenseNet是康奈尔大学博士后黄高（Gao Huang）、清华大学本科生刘壮（Zhuang Liu）、Facebook人工智能研究院研究科学家Laurens van der Maaten 及康奈尔大学计算机系教授Kilian Q. Weinberger于2016年提出的。论文当选CVPR 2017最佳论文。

论文：

《Densely Connected Convolutional Networks》

代码：

https://github.com/liuzhuang13/DenseNet

原始版本是Torch写的，官网上列出了其他框架的实现代码的网址。

![](/images/article/DenseNet_2.png)

上图是DenseNet的整体网络结构图。从整体层面来看，DenseNet主要由3个dense block组成。

![](/images/article/DenseNet.png)

上图就是dense block的结构图。与Resnet的跨层加法不同，这里采用的是Concatenation，也就是将不同层的几个tensor组合成一个大的tensor。

### DenseNet的设计思想

以下是原作者的访谈片段：

DenseNet的想法很大程度上源于我们去年发表在ECCV上的一个叫做随机深度网络（Deep networks with stochastic depth）工作。当时我们提出了一种类似于Dropout的方法来改进ResNet。我们发现在训练过程中的每一步都随机地“扔掉”（drop）一些层，可以显著的提高ResNet的泛化性能。这个方法的成功至少带给我们两点启发：

>首先，它说明了神经网络其实并不一定要是一个递进层级结构，也就是说网络中的某一层可以不仅仅依赖于紧邻的上一层的特征，而可以依赖于更前面层学习的特征。想像一下在随机深度网络中，当第l层被扔掉之后，第l+1层就被直接连到了第l-1层；当第2到了第l层都被扔掉之后，第l+1层就直接用到了第1层的特征。因此，随机深度网络其实可以看成一个具有随机密集连接的DenseNet。

>其次，我们在训练的过程中随机扔掉很多层也不会破坏算法的收敛，说明了ResNet具有比较明显的冗余性，网络中的每一层都只提取了很少的特征（即所谓的残差）。实际上，我们将训练好的ResNet随机的去掉几层，对网络的预测结果也不会产生太大的影响。既然每一层学习的特征这么少，能不能降低它的计算量来减小冗余呢？

DenseNet 的设计正是基于以上两点观察。我们让网络中的每一层都直接与其前面层相连，实现特征的重复利用；同时把网络的每一层设计得特别「窄」，即只学习非常少的特征图（最极端情况就是每一层只学习一个特征图），达到降低冗余性的目的。这两点也是DenseNet与其他网络最主要的不同。需要强调的是，第一点是第二点的前提，没有密集连接，我们是不可能把网络设计得太窄的，否则训练会出现欠拟合（under-fitting）现象，即使 ResNet 也是如此。

### DenseNet的优点

**省参数。**在 ImageNet 分类数据集上达到同样的准确率，DenseNet 所需的参数量不到 ResNet 的一半。对于工业界而言，小模型可以显著地节省带宽，降低存储开销。

**省计算。**达到与 ResNet 相当的精度，DenseNet 所需的计算量也只有 ResNet 的一半左右。

**抗过拟合。**DenseNet 具有非常好的抗过拟合性能，尤其适合于训练数据相对匮乏的应用。这一点从论文中 DenseNet 在不做数据增强（data augmentation）的 CIFAR 数据集上的表现就能看出来。

由于DenseNet不容易过拟合，在数据集不是很大的时候表现尤其突出。在一些图像分割和物体检测的任务上，基于DenseNet的模型往往可以省略在ImageNet上的预训练，直接从随机初始化的模型开始训练，最终达到相同甚至更好的效果。由于在很多应用中实际数据跟预训练的ImageNet自然图像存在明显的差别，这种不需要预训练的方法在医学图像，卫星图像等任务上都具有非常广阔的应用前景。


