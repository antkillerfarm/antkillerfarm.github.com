---
layout: post
title:  深度学习（十九）——FCN, SegNet, DeconvNet, DeepLab, ENet, GCN
category: DL 
---

## Grab cut

Grab cut是微软剑桥研究院于2004年提出的著名交互式图像语义分割方法。与N-cut一样，grab cut同样也是基于图划分，不过grab cut是其改进版本，可以看作迭代式的语义分割算法。Grab cut利用了图像中的纹理（颜色）信息和边界（反差）信息，只要少量的用户交互操作即可得到比较好的前后背景分割结果。

在Grab cut中，RGB图像的前景和背景分别用一个高斯混合模型（Gaussian mixture model, GMM）来建模。两个GMM分别用以刻画某像素属于前景或背景的概率，每个GMM高斯部件（Gaussian component）个数一般设为k=5。接下来，利用吉布斯能量方程（Gibbs energy function）对整张图像进行全局刻画，而后迭代求取使得能量方程达到最优值的参数作为两个GMM的最优参数。GMM确定后，某像素属于前景或背景的概率就随之确定下来。

在与用户交互的过程中，Grab cut提供两种交互方式：一种以包围框（Bounding box）为辅助信息；另一种以涂写的线条（Scribbled line）作为辅助信息。以下图为例，用户在开始时提供一个包围框，grab cut默认的认为框中像素中包含主要物体／前景，此后经过迭代图划分求解，即可返回扣出的前景结果，可以发现即使是对于背景稍微复杂一些的图像，grab cut仍有不俗表现。

![](/images/article/grab_cut.jpg)

不过，在处理下图时，grab cut的分割效果则不能令人满意。此时，需要额外人为的提供更强的辅助信息：用红色线条／点标明背景区域，同时用白色线条标明前景区域。在此基础上，再次运行grab cut算法求取最优解即可得到较为满意的语义分割结果。Grab cut虽效果优良，但缺点也非常明显，一是仅能处理二类语义分割问题，二是需要人为干预而不能做到完全自动化。

![](/images/article/grab_cut_2.jpg)

不难看出，前DL时代的语义分割工作多是根据图像像素自身的低阶视觉信息（Low-level visual cues）来进行图像分割。由于这样的方法没有算法训练阶段，因此往往计算复杂度不高，但是在较困难的分割任务上（如果不提供人为的辅助信息），其分割效果并不能令人满意。

# FCN

Fully Convolutional Networks是Jonathan Long和Evan Shelhamer于2015年提出的网络结构。

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

参考：

http://www.cnblogs.com/gujianhan/p/6030639.html

全卷积网络FCN详解

https://zhuanlan.zhihu.com/p/32506912

FCN的简单实现

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

类似这样的encoder-decoder对称结构的还有U-Net（因为它们的形状像U字形）：

![](/images/article/U_Net.jpg)

论文：

《U-Net: Convolutional Networks for Biomedical Image Segmentation》

官网：

https://lmb.informatik.uni-freiburg.de/people/ronneber/u-net/

>Olaf Ronneberger，弗莱堡大学教授，DeepMind研究员。

U-Net使用center crop和concat操作实现了不同层次特征的upsample，这和后面介绍的DenseNet十分类似。

参考：

https://mp.weixin.qq.com/s/ZNNwK1pkL4e0KeYw-UycgA

Kaggle车辆边界识别第一名解决方案：使用预训练权重轻松改进U-Net

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

参考：

https://mp.weixin.qq.com/s/ald9Dq_VV3PYuN6JoY3E5Q

DeepLabv3+：语义分割领域的新高峰

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

