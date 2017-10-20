---
layout: post
title:  深度学习（十七）——视频目标分割, Fast Image Processing, OpenPose, Mask R-CNN
category: theory 
---

# Ultra Deep Network（续）

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

DenseNet的想法很大程度上源于我们去年发表在ECCV上的一个叫做随机深度网络（Deep networks with stochastic depth）工作。当时我们提出了一种类似于Dropout的方法来改进ResNet。我们发现在训练过程中的每一步都随机地「扔掉」（drop）一些层，可以显著的提高ResNet的泛化性能。这个方法的成功至少带给我们两点启发：

>首先，它说明了神经网络其实并不一定要是一个递进层级结构，也就是说网络中的某一层可以不仅仅依赖于紧邻的上一层的特征，而可以依赖于更前面层学习的特征。想像一下在随机深度网络中，当第l层被扔掉之后，第l+1层就被直接连到了第l-1层；当第2到了第l层都被扔掉之后，第l+1层就直接用到了第1层的特征。因此，随机深度网络其实可以看成一个具有随机密集连接的DenseNet。

>其次，我们在训练的过程中随机扔掉很多层也不会破坏算法的收敛，说明了ResNet具有比较明显的冗余性，网络中的每一层都只提取了很少的特征（即所谓的残差）。实际上，我们将训练好的ResNet随机的去掉几层，对网络的预测结果也不会产生太大的影响。既然每一层学习的特征这么少，能不能降低它的计算量来减小冗余呢？

DenseNet 的设计正是基于以上两点观察。我们让网络中的每一层都直接与其前面层相连，实现特征的重复利用；同时把网络的每一层设计得特别「窄」，即只学习非常少的特征图（最极端情况就是每一层只学习一个特征图），达到降低冗余性的目的。这两点也是DenseNet与其他网络最主要的不同。需要强调的是，第一点是第二点的前提，没有密集连接，我们是不可能把网络设计得太窄的，否则训练会出现欠拟合（under-fitting）现象，即使 ResNet 也是如此。

### DenseNet的优点

**省参数。**在 ImageNet 分类数据集上达到同样的准确率，DenseNet 所需的参数量不到 ResNet 的一半。对于工业界而言，小模型可以显著地节省带宽，降低存储开销。

**省计算。**达到与 ResNet 相当的精度，DenseNet 所需的计算量也只有 ResNet 的一半左右。

**抗过拟合。**DenseNet 具有非常好的抗过拟合性能，尤其适合于训练数据相对匮乏的应用。这一点从论文中 DenseNet 在不做数据增强（data augmentation）的 CIFAR 数据集上的表现就能看出来。

由于DenseNet不容易过拟合，在数据集不是很大的时候表现尤其突出。在一些图像分割和物体检测的任务上，基于DenseNet的模型往往可以省略在ImageNet上的预训练，直接从随机初始化的模型开始训练，最终达到相同甚至更好的效果。由于在很多应用中实际数据跟预训练的ImageNet自然图像存在明显的差别，这种不需要预训练的方法在医学图像，卫星图像等任务上都具有非常广阔的应用前景。

### DenseNet的优化问题

当前的深度学习框架对DenseNet的密集连接没有很好的支持，我们只能借助于反复的拼接（Concatenation）操作，将之前层的输出与当前层的输出拼接在一起，然后传给下一层。对于大多数框架（如 Torch 和 TensorFlow），每次拼接操作都会开辟新的内存来保存拼接后的特征。这样就导致一个L层的网络，要消耗相当于 L(L+1)/2 层网络的内存（第l层的输出在内存里被存了(L-l+1) 份）。

解决这个问题的思路其实并不难，我们只需要预先分配一块缓存，供网络中所有的拼接层（Concatenation Layer）共享使用，这样DenseNet对内存的消耗便从平方级别降到了线性级别。

### 参考

https://www.leiphone.com/news/201708/0MNOwwfvWiAu43WO.html

CVPR 2017最佳论文作者解读：DenseNet 的“what”、“why”和“how”

https://zhuanlan.zhihu.com/p/28124810

为什么ResNet和DenseNet可以这么深？一文详解残差块为何有助于解决梯度弥散问题

https://mp.weixin.qq.com/s?__biz=MzI3MTA0MTk1MA==&mid=2651988934&idx=2&sn=0e5ffa195ef67a1371f3b5b223519121

ResNets、HighwayNets、DenseNets：用 TensorFlow 实现超深度神经网络

# 视频目标分割

视频目标分割任务和语义分割有两个基本区别：

1.视频目标分割任务分割的是一般的、非语义的目标；

2.视频目标分割添加了一个时序模块：它的任务是在视频的每一连续帧中寻找感兴趣目标的对应像素。

![](/images/article/Segmentation.png)

上图是Segmentation的细分，其中的每一个叶子都有一个示例数据集。

基于视频任务的特性，我们可以将问题分成两个子类：

无监督（亦称作视频显著性检测）：寻找并分割视频中的主要目标。这意味着算法需要自行决定哪个物体才是「主要的」。

半监督：在输入中（只）给出视频第一帧的正确分割掩膜，然后在之后的每一连续帧中分割标注的目标。


参考：

http://mp.weixin.qq.com/s/pGrzmq5aGoLb2uiJRYAXVw

一文概览视频目标分割

https://www.zhihu.com/question/52185576

视频中的目标检测与图像中的目标检测具体有什么区别？

# Fast Image Processing

![](/images/article/FIP.png)

上图是照片界常用的几种修图方式之一。一般将这些图片风格转换的算法，称为图像处理算子（image processing operators）。如何加速image processing operators的计算，就成为了学界研究的课题之一。

本文提出的模型就是用来加速image processing operators计算的。它是Intel Lab的Qifeng Chen和Jia Xu于2017年提出的。

论文：

《Fast Image Processing with Fully-Convolutional Networks》

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

1.采用Multi-Scale Context Aggregation作为基础网络。MCA的内容参见《深度学习（九）》。

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

# OpenPose

OpenPose是一个实时多人关键点检测的库，基于OpenCV和Caffe编写。它是CMU的Yaser Sheikh小组的作品。

>Yaser Ajmal Sheikh，巴基斯坦信德省易司哈克工程科学与技术学院本科（2001年）+中佛罗里达大学博士（2006年）。现为CMU副教授。

![](/images/article/openpose.png)

OpenPose的使用效果如上图所示。

论文：

《Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields》

《Hand Keypoint Detection in Single Images using Multiview Bootstrapping》

《Convolutional pose machines》

代码：

https://github.com/CMU-Perceptual-Computing-Lab/openpose

# Mask R-CNN

Mask R-CNN虽然挂着R-CNN的名头，但却是一个对象实例分割（不仅要分出对象的类别，连同一类对象的不同实例也要分出来）的NN。它是何恺明2017年的新作。

论文：

《Mask R-CNN》

只有非官方的代码：

Caffe版本：

https://github.com/jasjeetIM/Mask-RCNN

TensorFlow版本：

https://github.com/hillox/TFMaskRCNN

参考：

https://zhuanlan.zhihu.com/p/25954683

Mask R-CNN个人理解


