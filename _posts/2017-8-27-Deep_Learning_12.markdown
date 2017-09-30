---
layout: post
title:  深度学习（十二）——Faster R-CNN, YOLO, SSD
category: theory 
---

# Faster R-CNN（续）

## Region Proposal Networks

Faster R-CNN最重要的改进就是使用区域生成网络（Region Proposal Networks）替换Selective Search。因此，faster RCNN也可以简单地看做是“**RPN+fast RCNN**”。

![](/images/article/rpn.png)

上图是RPN的结构图。和SPPNet的ROI映射做法类似，RPN直接在feature map，而不是原图像上，生成区域。

由于Faster R-CNN最后会对bbox位置进行精调，因此这里生成区域的时候，只要大致准确即可。

![](/images/article/rpn_feature_map.png)

由于CNN所生成的feature map的尺寸，通常小于原图像。因此将feature map的点映射回原图像，就变成了上图所示的稀疏网点。这些网点也被称为原图感受野的中心点。

把网点当成基准点，然后围绕这个基准点选取k个不同scale、aspect ratio的anchor。论文中用了3个scale（三种面积$$\left\{ 128^2, 256^2, 521^2  \right\}$$，如上图的红绿蓝三色所示），3个aspect ratio（{1:1,1:2,2:1}，如上图的同色方框所示）。

![](/images/article/Anchors.png)

anchor的后处理如上图所示。

![](/images/article/Anchor_Pyramid.png)

上图展示了Image/Feature Pyramid、Filter Pyramid和Anchor Pyramid的区别。

## 定义损失函数

![](/images/article/faster_rcnn_p_3.png)

对于每个anchor，首先在后面接上一个二分类softmax（上图左边的Softmax），有2个score输出用以表示其是一个物体的概率与不是一个物体的概率 ($$p_i$$)。这个概率也可以理解为前景与后景，或者物体和背景的概率。

然后再接上一个bounding box的regressor输出代表这个anchor的4个坐标位置（$$t_i$$），因此RPN的总体Loss函数可以定义为 ：

$$L(\{p_i\},\{t_i\})=\frac{1}{N_{cls}}\sum_iL_{cls}(p_i,p_i^*)+\lambda \frac{1}{N_{reg}}\sum_ip_i^*L_{reg}(t_i,t_i^*)$$

该公式的含义和计算都比较复杂，这里不再赘述。

上图中，二分类softmax前后各添加了一个reshape layer，是什么原因呢？

这与caffe的实现的有关。bg/fg anchors的矩阵，其在caffe blob中的存储形式为[batch size, 2*9, H, W]。这里的2代表二分类，9是anchor的个数。因为这里的softmax只分两类，所以在进行计算之前需要将blob变为[batch size, 2, 9*H, W]。之后再reshape回复原状。

## RPN和Fast R-CNN协同训练

我们知道，如果是分别训练两种不同任务的网络模型，即使它们的结构、参数完全一致，但各自的卷积层内的卷积核也会向着不同的方向改变，导致无法共享网络权重，论文作者提出了几种可能的方式。

### Alternating training

此方法其实就是一个不断迭代的训练过程，既然分别训练RPN和Fast-RCNN可能让网络朝不同的方向收敛，a)那么我们可以先独立训练RPN，然后用这个RPN的网络权重对Fast-RCNN网络进行初始化并且用之前RPN输出proposal作为此时Fast-RCNN的输入训练Fast R-CNN。b) 用Fast R-CNN的网络参数去初始化RPN。之后不断迭代这个过程，即循环训练RPN、Fast-RCNN。

![](/images/article/alternating_training.png)

### Approximate joint training or Non-approximate training

这两种方式，不再是串行训练RPN和Fast-RCNN，而是尝试把二者融入到一个网络内训练。融合方式和上面的Faster R-CNN结构图类似。细节不再赘述。

### 4-Step Alternating Training

这是作者发布的源代码中采用的方法。

第一步：用ImageNet模型初始化，独立训练一个RPN网络；

第二步：仍然用ImageNet模型初始化，但是使用上一步RPN网络产生的proposal作为输入，训练一个Fast-RCNN网络，至此，两个网络每一层的参数完全不共享；

第三步：使用第二步的Fast-RCNN网络参数初始化一个新的RPN网络，但是把RPN、Fast-RCNN共享的那些卷积层的learning rate设置为0，也就是不更新，仅仅更新RPN特有的那些网络层，重新训练，此时，两个网络已经共享了所有公共的卷积层；

第四步：仍然固定共享的那些网络层，把Fast-RCNN特有的网络层也加入进来，形成一个unified network，继续训练，fine tune Fast-RCNN特有的网络层，此时，该网络已经实现我们设想的目标，即网络内部预测proposal并实现检测的功能。

![](/images/article/4_Step_Alternating_Training.png)

## 总结

![](/images/article/faster_rcnn_p.png)

参考：

https://zhuanlan.zhihu.com/p/24916624

Faster R-CNN

http://blog.csdn.net/shenxiaolu1984/article/details/51152614

Faster RCNN算法详解

https://mp.weixin.qq.com/s/VKQufVUQ3TP5m7_2vOxnEQ

通过Faster R-CNN实现当前最佳的目标计数

http://blog.csdn.net/zy1034092330/article/details/62044941

Faster RCNN详解

# YOLO

YOLO: Real-Time Object Detection，是一个基于神经网络的实时对象检测软件。它的原理基于Joseph Chet Redmon 2016年的论文：

《You Only Look Once: Unified, Real-Time Object Detection》

这也是Ross Girshick去Facebook之后，参与的又一力作。

官网：

https://pjreddie.com/darknet/yolo/

>注：Joseph Chet Redmon，Middlebury College本科+华盛顿大学博士（在读）。网名：pjreddie。

pjreddie不仅是个算法达人，也是个造轮子的高手。YOLO的原始代码基于他本人编写的DL框架——darknet。

darknet代码：

https://github.com/pjreddie/darknet/

YOLO的caffe版本有很多（当然都是非官方的），这里推荐：

https://github.com/yeahkun/caffe-yolo

## 概述

从R-CNN到Fast R-CNN一直采用的思路是proposal+分类（proposal提供位置信息，分类提供类别信息），这也被称作two-stage cascade。

YOLO不仅是end-to-end，而且还提供了另一种更为直接的思路：直接在输出层回归bounding box的位置和bounding box所属的类别(整张图作为网络的输入，把Object Detection的问题转化成一个Regression问题)。

![](/images/article/yolo.png)

上图是YOLO的大致流程：

**Step 1**：Resize成448*448，图片分割得到7*7网格(cell)。

**Step 2**：CNN提取特征和预测：卷积部分负责提特征。全连接部分负责预测。

a) $$7\times 7\times 2=98$$个bounding box(bbox) 的坐标$$x_{center},y_{center},w,h$$和是否有物体的confidence。

b) $$7\times 7=49$$个cell所属20个物体分类的概率。

![](/images/article/yolo_2.png)

![](/images/article/yolo_3.png)

上图是YOLO的网络结构图。从表面看，YOLO的输出只有一个，似乎比Faster RCNN两个输出少一个，然而这个输出本身，实际上要复杂的多。

YOLO的输出是一个$$7\times 7\times 30$$的tensor，其中$$7\times 7$$对应图片分割的7*7网格。30表明每个网格对应一个30维的向量。

![](/images/article/yolo_4.png)

上图是这个30维向量的编码方式：2个bbox+confidence是$$5\times 2=10$$维，20个物体分类的概率占其余20维。

总结一下，输出tersor的维度是：$$S\times S \times (B \times 5 + C)$$

这里的confidence代表了所预测的box中含有object的置信度和这个box预测的有多准两重信息：

$$\text{confidence} = \text{Pr}(Object) ∗ \text{IOU}_{pred}^{truth}$$

在loss函数设计方面，简单的把结果堆在一起，然后认为它们的重要程度都一样，这显然是不合理的，每个loss项之前的参数$$\lambda$$就是用来设定权重的。

**Step 3**：过滤bbox（通过NMS）。

![](/images/article/yolo_5.png)

上图是Test阶段的NMS的过程示意图。

## 参考

https://zhuanlan.zhihu.com/p/24916786

图解YOLO

https://mp.weixin.qq.com/s/n51XtGAsaDDAatXYychXrg

YOLO比R-CNN快1000倍，比Fast R-CNN快100倍的实时对象检测！

http://blog.csdn.net/tangwei2014/article/details/50915317

论文阅读笔记：You Only Look Once: Unified, Real-Time Object Detection

https://mp.weixin.qq.com/s/Wqj6EM33p-rjPIHnFKtmCw

计算机是怎样快速看懂图片的：比R-CNN快1000倍的YOLO算法

http://lanbing510.info/2017/08/28/YOLO-SSD.html

目标检测之YOLO，SSD

http://www.yeahkun.com/2016/09/06/object-detection-you-only-look-once-caffe-shi-xian/

Object detection: You Look Only Once(YOLO)

http://blog.csdn.net/zy1034092330/article/details/72807924

YOLO

# SSD

SSD是Wei Liu于2016年提出的算法。

论文：

《SSD: Single Shot MultiBox Detector》

代码：

https://github.com/weiliu89/caffe

>Wei Liu，南京大学本科（2009）+北卡罗莱娜大学博士（在读）。   
>个人主页：   
>http://www.cs.unc.edu/~wliu/

## 网络结构

YOLO有一些缺陷：每个网格只预测一个物体，容易造成漏检；对于物体的尺度相对比较敏感，对于尺度变化较大的物体泛化能力较差。

针对YOLO中的这些不足，SSD在这两方面都有所改进，同时兼顾了mAP和实时性的要求。其思路就是Faster R-CNN+YOLO，利用YOLO的思路和Faster R-CNN的anchor box的思想。

![](/images/article/ssd.png)

上图是SSD的网络结构图。其特点为：

1.采用VGG16的基础网络结构，使用前面的前5层。

2.使用Dilated convolution将fc6和fc7层转化成两个卷积层。

3.再额外增加了3个卷积层，和一个average pool层。不同层次的feature map分别用于default box的偏移以及不同类别得分的预测。

4.通过NMS得到最终的检测结果。

这些增加的卷积层的feature map的大小变化比较大，允许能够检测出不同尺度下的物体：在低层的feature map，感受野比较小，高层的感受野比较大，在不同的feature map进行卷积，可以达到多尺度的目的。

![](/images/article/ssd_2.png)

上图是从另一个角度观察SSD，可以看出SSD可检出8372个default box。这里沿用Faster R-CNN的Anchor方法生成default box。

![](/images/article/ssd_3.png)

和YOLO一样，卷积层的每个点都是一个vector，含义也和YOLO类似，只是分类的时候，多了一个背景的类别，所以就成了20+1类。

在YOLO中，由于每个格子只有1个default box，所以对于一个格子中包含两个物体的情况是无能为力的。SSD的Anchor方法略微改善了这方面的性能，但对于超过Anchor数量的情况，仍然无能为力。因此，这两者对于小目标的检测，没有RCNN系列算法的效果好。


