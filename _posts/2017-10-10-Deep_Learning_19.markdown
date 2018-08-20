---
layout: post
title:  深度学习（十九）——FCN, SegNet, DeconvNet, DeepLab
category: DL 
---

# 前DL时代的语义分割

## Normalized cut

Normalized cut （N-cut）方法是基于图划分（Graph partitioning）的语义分割方法中最著名的方法之一，于2000年Jianbo Shi和Jitendra Malik发表于相关领域顶级期刊TPAMI。

通常，传统基于图划分的语义分割方法都是将图像抽象为图（Graph）的形式$$\bf{G}=(\bf{V},\bf{E})$$（$$\bf{V}$$为图节点，$$\bf{E}$$为图的边），然后借助图理论（Graph theory）中的理论和算法进行图像的语义分割。

常用的方法为经典的最小割算法（Min-cut algorithm）。不过，在边的权重计算时，经典min-cut算法只考虑了局部信息。如下图所示，以二分图为例（将$$\bf{G}$$分为不相交的$$\bf{A},\bf{B}$$两部分），若只考虑局部信息，那么分离出一个点显然是一个min-cut，因此图划分的结果便是类似$$n_1$$或$$n_2$$这样离群点，而从全局来看，实际想分成的组却是左右两大部分。

![](/images/article/N_cut.jpg)

针对这一情形，N-cut则提出了一种考虑全局信息的方法来进行图划分（Graph partitioning），即，将两个分割部分$$\bf{A},\bf{B}$$与全图节点的连接权重（$${\rm assoc(\bf{A},\bf{V})}$$和$$\rm assoc(\bf{B},\bf{V})$$）考虑进去：

$$N_{cut}(\bf{A},\bf{B})=\frac{cut(\bf{A},\bf{B})}{assoc(\bf{A},\bf{V})}+\frac{cut(\bf{A},\bf{B})}{assoc(\bf{B},\bf{V})}$$

如此一来，在离群点划分中，$$N_{cut}(\bf{A},\bf{B})$$中的某一项会接近1，而这样的图划分显然不能使得$$N_{cut}(\bf{A},\bf{B})$$是一个较小的值，故达到考虑全局信息而摒弃划分离群点的目的。这样的操作类似于机器学习中特征的规范化（Normalization）操作，故称为Normalized cut。N-cut不仅可以处理二类语义分割，而且将二分图扩展为K路（K-way）图划分即可完成多语义的图像语义分割，如下图例。

![](/images/article/N_cut_2.jpg)

## Grab cut

Grab cut是微软剑桥研究院于2004年提出的著名交互式图像语义分割方法。与N-cut一样，grab cut同样也是基于图划分，不过grab cut是其改进版本，可以看作迭代式的语义分割算法。Grab cut利用了图像中的纹理（颜色）信息和边界（反差）信息，只要少量的用户交互操作即可得到比较好的前后背景分割结果。

在Grab cut中，RGB图像的前景和背景分别用一个高斯混合模型（Gaussian mixture model, GMM）来建模。两个GMM分别用以刻画某像素属于前景或背景的概率，每个GMM高斯部件（Gaussian component）个数一般设为k=5。接下来，利用吉布斯能量方程（Gibbs energy function）对整张图像进行全局刻画，而后迭代求取使得能量方程达到最优值的参数作为两个GMM的最优参数。GMM确定后，某像素属于前景或背景的概率就随之确定下来。

在与用户交互的过程中，Grab cut提供两种交互方式：一种以包围框（Bounding box）为辅助信息；另一种以涂写的线条（Scribbled line）作为辅助信息。以下图为例，用户在开始时提供一个包围框，grab cut默认的认为框中像素中包含主要物体／前景，此后经过迭代图划分求解，即可返回扣出的前景结果，可以发现即使是对于背景稍微复杂一些的图像，grab cut仍有不俗表现。

![](/images/article/grab_cut.jpg)

不过，在处理下图时，grab cut的分割效果则不能令人满意。此时，需要额外人为的提供更强的辅助信息：用红色线条／点标明背景区域，同时用白色线条标明前景区域。在此基础上，再次运行grab cut算法求取最优解即可得到较为满意的语义分割结果。Grab cut虽效果优良，但缺点也非常明显，一是仅能处理二类语义分割问题，二是需要人为干预而不能做到完全自动化。

![](/images/article/grab_cut_2.jpg)

不难看出，前DL时代的语义分割工作多是根据图像像素自身的低阶视觉信息（Low-level visual cues）来进行图像分割。由于这样的方法没有算法训练阶段，因此往往计算复杂度不高，但是在较困难的分割任务上（如果不提供人为的辅助信息），其分割效果并不能令人满意。

参考：

https://mp.weixin.qq.com/s/AiuwMytfux9BMt__eVtj6w

基于图割算法的木材表面缺陷图像分析

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

DeepLab共有4个版本（v1, v2, v3, v3+），分别对应4篇论文：

《Semantic Image Segmentation with Deep Convolutional Nets and Fully Connected CRFs》

《DeepLab: Semantic Image Segmentation with Deep Convolutional Nets, Atrous Convolution, and Fully Connected CRFs》

《Rethinking Atrous Convolution for Semantic Image Segmentation》

《Encoder-Decoder with Atrous Separable Convolution for Semantic Image Segmentation》

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

https://mp.weixin.qq.com/s/zLyAwtG49A-vwWa1iPziew

谷歌最新语义图像分割模型DeepLab-v3+今日开源

https://mp.weixin.qq.com/s/L1JtK9eUhVJMGWm-SjWW5w

DeepLab v1

https://zhuanlan.zhihu.com/p/34783156

读Xception和DeepLab V3+

https://mp.weixin.qq.com/s/_D9jFc1suFhmkm9v2-_PTQ

DeepLab v2及调试过程

https://mp.weixin.qq.com/s/Za_h1BWR-mQKtgArxwsjRw

语义分割网络DeepLab-v3的架构设计思想和TensorFlow实现

https://mp.weixin.qq.com/s/JidoZ9-V8JGWfB2gyO2AlA

Deeplab v2安装及调试全过程

https://mp.weixin.qq.com/s/pL1Wvd3oBRVtqG9I4O4oDg

DeepLab V3

