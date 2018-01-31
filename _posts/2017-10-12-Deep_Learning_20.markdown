---
layout: post
title:  深度学习（二十）——Fast Image Processing, 图像超分辨率算法
category: DL 
---

## DenseNet

### DenseNet的设计思想（续）

DenseNet 的设计正是基于以上两点观察。我们让网络中的每一层都直接与其前面层相连，实现特征的重复利用；同时把网络的每一层设计得特别「窄」，即只学习非常少的特征图（最极端情况就是每一层只学习一个特征图），达到降低冗余性的目的。这两点也是DenseNet与其他网络最主要的不同。需要强调的是，第一点是第二点的前提，没有密集连接，我们是不可能把网络设计得太窄的，否则训练会出现欠拟合（under-fitting）现象，即使 ResNet 也是如此。

### DenseNet的优点

**省参数。**在 ImageNet 分类数据集上达到同样的准确率，DenseNet 所需的参数量不到 ResNet 的一半。对于工业界而言，小模型可以显著地节省带宽，降低存储开销。

**省计算。**达到与 ResNet 相当的精度，DenseNet 所需的计算量也只有 ResNet 的一半左右。

**抗过拟合。**DenseNet 具有非常好的抗过拟合性能，尤其适合于训练数据相对匮乏的应用。这一点从论文中 DenseNet 在不做数据增强（data augmentation）的 CIFAR 数据集上的表现就能看出来。

由于DenseNet不容易过拟合，在数据集不是很大的时候表现尤其突出。在一些图像分割和物体检测的任务上，基于DenseNet的模型往往可以省略在ImageNet上的预训练，直接从随机初始化的模型开始训练，最终达到相同甚至更好的效果。由于在很多应用中实际数据跟预训练的ImageNet自然图像存在明显的差别，这种不需要预训练的方法在医学图像，卫星图像等任务上都具有非常广阔的应用前景。

### DenseNet的优化问题

当前的深度学习框架对DenseNet的密集连接没有很好的支持，我们只能借助于反复的拼接（Concatenation）操作，将之前层的输出与当前层的输出拼接在一起，然后传给下一层。对于大多数框架（如Torch和TensorFlow），每次拼接操作都会开辟新的内存来保存拼接后的特征。这样就导致一个L层的网络，要消耗相当于L(L+1)/2层网络的内存（第l层的输出在内存里被存了(L-l+1)份）。

解决这个问题的思路其实并不难，我们只需要预先分配一块缓存，供网络中所有的拼接层（Concatenation Layer）共享使用，这样DenseNet对内存的消耗便从平方级别降到了线性级别。

### 参考

https://www.leiphone.com/news/201708/0MNOwwfvWiAu43WO.html

CVPR 2017最佳论文作者解读：DenseNet 的“what”、“why”和“how”

https://zhuanlan.zhihu.com/p/28124810

为什么ResNet和DenseNet可以这么深？一文详解残差块为何有助于解决梯度弥散问题

https://mp.weixin.qq.com/s?__biz=MzI3MTA0MTk1MA==&mid=2651988934&idx=2&sn=0e5ffa195ef67a1371f3b5b223519121

ResNets、HighwayNets、DenseNets：用TensorFlow实现超深度神经网络

## Dual Path Networks

DPN是冯佳时和颜水成团队的Yunpeng Chen的作品。

>冯佳时，中国科学技术大学自动化系学士，新加坡国立大学电子与计算机工程系博士。现任新加坡国立大学电子与计算机工程系助理教授。

论文：

《Dual Path Networks》

代码：

https://github.com/cypw/DPNs

这篇论文首先从拓扑关系的角度分析了ResNet、DenseNet和HORNN（Higher Order RNN）之间的联系。

![](/images/img2/DPN_3.png)

如上所示，RNN相当于共享权值的串联的ResNet，而DenseNet则相当于并联的RNN。

更进一步的，上述三者都可表述为以下通式：

$$h^k=g^k\left[\sum_{t=0}^{k-1}f_t^k(h^t)\right]$$

其中，$$h^t$$表示t时刻的隐层状态；索引k表示当前时刻；$$x^t$$表示t时刻的输入；$$f_t^k(⋅)$$表示特征提取；$$g^k$$表示对提取特征做输出前的变换。

如果$$f_t^k(\cdot)$$和$$g^k(\cdot)$$每个Step都共享，那么就是HORNN，如果只有$$f_t^k(\cdot)$$共享，那么就是ResNet，两者都不共享，那就是DenseNet。

![](/images/img2/DPN.png)

上图展示的是ResNet和DenseNet的示意图。图中用线填充的柱状体，表示的是主干结点的tensor的大小。

ResNet由于跨层和主干之间是element-wise的加法运算，因此每个主干结点的tensor都是一样大的。

而DenseNet的跨层和主干之间是Concatenation运算，因此主干越往下，tensor越大。

通过上面的分析，我们可以认识到 ：

ResNet： 侧重于特征的再利用，但不善于发掘新的特征；

DenseNet: 侧重于新特征的发掘，但又会产生很多冗余；

为了综合二者的优点，作者设计了DPN网络：

![](/images/img2/DPN_2.png)

参考：

http://blog.csdn.net/scutlihaoyu/article/details/75645551

《Dual Path Networks》笔记

http://www.cnblogs.com/mrxsc/p/7693316.html

Dual Path Networks

http://blog.csdn.net/u014380165/article/details/75676216

DPN（Dual Path Network）算法详解

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

# 图像超分辨率算法

![](/images/img2/Super_Resolution.png)

如上图所示，一张低分辨率的小图（Low Resolution，LR）如果采用简单的插值算法进行图片放大的话，图像中物体的边缘会比较模糊。如何用算法将这种LR的图片放大成HR的图片，这就是Super Resolution（SR）的目标了。

SR目前主要有两个用途：

1.提升设备的测量精度。这个在天文和医疗图像方面用的比较多，比如Google和NASA利用AI探测太阳系外的行星，还有癌症的早期诊断。

2.Image Signal Processor。上面的两个应用比较高端，SR最主要的用途恐怕还是相机的ISP领域。ISP的基本概念参见《图像处理理论（五）》。

这里主要讨论DL在SR领域的应用。

## 前DL时代的SR

从信号处理的角度来说，LR之所以无法恢复成HR，主要在于丢失了图像的高频信息。（Nyquist采样定理）

>Harry Nyquist，1889~1976，University of North Dakota本硕（1914,1915）+耶鲁博士（1917）。AT&T贝尔实验室电子工程师。IEEE Medal of Honor获得者（1960）。

>IEEE Medal of Honor是IEEE的最高奖，除了1963年之外，每年只有1人得奖，个别年份甚至会轮空。

最简单的当然是《图像处理理论（二）》中提到的梯度锐化和拉普拉斯锐化，这种简单算法当然不要指望有什么好效果，聊胜于无而已。这是1995年以前的主流做法。

稍微复杂的方法，如同CV的其它领域经历了“信号处理->ML->DL”的变迁一样，SR也进入了ML阶段。

![](/images/img2/SR.png)

上图是两种典型的SR算法。

左图算法的中心思想是从图片中找出相似的大尺度区域，然后利用这个大区域的边缘信息进行SR。但这个方法对于那些只出现一次的边缘信息是没什么用的。

于是就有了右图的算法。对各种边缘信息建立一个数据库，使用时从数据库中挑一个最类似的边缘信息进行SR。这个方法比上一个方法好一些，但不够鲁棒，图片稍作改动，就有可能无法检索到匹配的边缘信息了。

ML时代的代表算法还有：

《Image Super-Resolution via Sparse Representation》

这篇论文是黄煦涛和马毅小组的Jianchao Yang的作品。

>黄煦涛（Thomas Huang），1936年生。生于上海，国立台湾大学本科（1956）+MIT硕博（1960,1963）。UIUC教授。美国工程院院士，中国科学院+中国工程院外籍院士。

>马毅，清华本科（1995）+UCB硕博（1997,2000）。UCB教授。IEEE fellow。   
>个人主页：   
>http://yima.csl.illinois.edu/

这篇论文提出的算法，在形式上和后文这些DL算法已经非常类似了，也是基于HR和LR配对的有监督训练。区别只在于这篇论文使用矩阵的稀疏表示来拟合SR函数，而DL算法使用神经网络拟合SR函数。前者是线性变换，而后者是非线性变换。

