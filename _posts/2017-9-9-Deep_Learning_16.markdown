---
layout: post
title:  深度学习（十六）——语义分割, FCN
category: DL 
---

# YOLOv2

## Stronger（续）

### Hierarchical classiﬁcation（层次式分类）

ImageNet的标签参考WordNet（一种结构化概念及概念之间关系的语言数据库）。例如：

![](/images/article/WordNet.png)

很多分类数据集采用扁平化的标签。而整合数据集则需要结构化标签。

WordNet是一个有向图结构（而非树结构），因为语言是复杂的（例如“dog”既是“canine”又是“domestic animal”），为了简化问题，作者从ImageNet的概念中构建了一个层次树结构（hierarchical tree）来代替图结构方案。这也就是作者论文中提到的WordTree。

WordTree的细节，更偏NLP一些，这里不再赘述。

## 参考

https://zhuanlan.zhihu.com/p/25167153

YOLO2

http://blog.csdn.net/jesse_mx/article/details/53925356

YOLOv2论文笔记

http://lanbing510.info/2017/09/04/YOLOV2.html

目标检测之YOLOv2

# 其它目标检测网络

## A-Fast-RCNN

A-Fast-RCNN首次将对抗学习引入到了目标检测领域，idea是非常创新的。

http://blog.csdn.net/jesse_mx/article/details/72955981

A-Fast-RCNN论文笔记

## R-FCN

FCN在目标检测领域的应用。

http://blog.csdn.net/zijin0802034/article/details/53411041

R-FCN: Object Detection via Region-based Fully Convolutional Networks

## G-CNN

G-CNN是MaryLand大学的工作，论文主要的思路也是消除region proposal，和YOLO，SSD不同，G-CNN的工作借鉴了迭代的想法，把边框检测等价于找到初始边框到最终目标的一个路径。但是使用one-step regression不能处理这个非线性的过程，所以作者采用迭代的方法逐步接近最终的目标。

http://blog.csdn.net/zijin0802034/article/details/53535647

G-CNN: an Iterative Grid Based Object Detector

# 语义分割

Semantic segmentation是图像理解的基石性技术，在自动驾驶系统（具体为街景识别与理解）、无人机应用（着陆点判断）以及穿戴式设备应用中举足轻重。

我们都知道，图像是由许多像素（Pixel）组成，而“语义分割”顾名思义就是将像素按照图像中表达语义含义的不同进行分组（Grouping）/分割（Segmentation）。

![](/images/article/image_enet.png)

上图是语义分割网络ENet的实际效果图。其中，左图为原始图像，右图为分割任务的真实标记（Ground truth）。

显然，在图像语义分割任务中，其输入为一张HxWx3的三通道彩色图像，输出则是对应的一个HxW矩阵，矩阵的每一个元素表明了原图中对应位置像素所表示的语义类别（Semantic label）。

因此，图像语义分割也称为“图像语义标注”（Image semantic labeling）、“像素语义标注”（Semantic pixel labeling）或“像素语义分组”（Semantic pixel grouping）。

由于图像语义分割不仅要识别出对象，还要标出每个对象的边界。因此，与分类目的不同，相关模型要具有像素级的密集预测能力。

目前用于语义分割研究的两个最重要数据集是PASCAL VOC和MSCOCO。

参考：

https://zhuanlan.zhihu.com/p/21824299

从特斯拉到计算机视觉之“图像语义分割”

https://zhuanlan.zhihu.com/SemanticSegmentation

一个语义分割的专栏

https://zhuanlan.zhihu.com/p/22308032

图像语义分割之FCN和CRF

https://zhuanlan.zhihu.com/p/25515361

图像语义分割之特征整合和结构预测

https://zhuanlan.zhihu.com/p/27794982

语义分割中的深度学习方法全解：从FCN、SegNet到各代DeepLab

https://mp.weixin.qq.com/s/cANlqQAI-A2mC9vnd3imQA

Instance-Aware图像语义分割

https://mp.weixin.qq.com/s/v_TLYYq6cFWuwR9tXM8m-A

如何通过CRF-RNN模型实现图像语义分割任务

https://mp.weixin.qq.com/s/ceCC7Q6yr0QKESeZXi6lWQ

堆叠解卷积网络实现图像语义分割顶尖效果

https://mp.weixin.qq.com/s/4BvvwV11f9MrrYyLwUrX9w

还在用ps抠图抠瞎眼？机器学习通用背景去除产品诞生记

https://zhuanlan.zhihu.com/p/24738319

“见微知著”——细粒度图像分析进展综述

https://mp.weixin.qq.com/s/V4_euZRcyyxeimXAA_waAg

贾佳亚：最有效的COCO物体分割算法

# 前DL时代的语义分割

从最简单的像素级别“阈值法”（Thresholding methods）、基于像素聚类的分割方法（Clustering-based segmentation methods）到“图划分”的分割方法（Graph partitioning segmentation methods），在DL“一统江湖”之前，图像语义分割方面的工作可谓“百花齐放”。在此，我们仅以“Normalized cut”和“Grab cut”这两个基于图划分的经典分割方法为例，介绍一下前DL时代语义分割方面的研究。

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

