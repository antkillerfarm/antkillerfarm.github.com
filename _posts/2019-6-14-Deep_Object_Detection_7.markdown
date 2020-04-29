---
layout: post
title:  深度目标检测（七）——CenterNet, Anchor-Free, 其它目标检测网络, 目标检测进阶（1）
category: Deep Object Detection 
---

# CornerNet（续）

## corner pooling

corner pooling是CornerNet新提出的一种操作。其步骤如下图所示：

![](/images/img3/corner_pooling.png)

依top-left corner pooling为例，对每个channel，分别提取特征图的水平和垂直方向的最大值，然后求和。具体的计算如下图所示：

![](/images/img3/corner_pooling_2.png)

论文认为corner pooling之所以有效，是因为：

- 目标定位框的中心难以确定，和边界框的4条边相关，但是每个顶点只与边界框的两条边相关，所以corner更容易提取。

- 顶点更有效提供离散的边界空间，使用$$O(w\times h)$$顶点可以表示$$O(w^2\times h^2)$$个anchor boxes。

## 参考

https://mp.weixin.qq.com/s/e74-zFcMZzn67KaFXb_fdQ

CornerNet目标检测开启预测“边界框”到预测“点对”的新思路

https://zhuanlan.zhihu.com/p/41865617

CornerNet：目标检测算法新思路

https://mp.weixin.qq.com/s/e6B22xpue_xZwrXmIlZodw

ECCV-2018最佼佼者CornerNet的目标检测算法

https://mp.weixin.qq.com/s/9ldLaYKGkgq-MnJZw7CrDQ

CornerNet为什么有别于其他目标检测领域的主流算法？

https://mp.weixin.qq.com/s/ZhfnZ4IwOnTQlqeB6Ilr3A

CornerNet: Detecting Objects as Paired Keypoints解读

https://zhuanlan.zhihu.com/p/63134919

普林斯顿大学提出：CornerNet-Lite，基于关键点的目标检测算法，已开源！

https://mp.weixin.qq.com/s/8hN1RdYVJQWOqPpejjfXeQ

CornerNet

# CenterNet

CenterNet是中科院、牛津、Huawei Noah’s Ark Lab的一个联合团队的作品。（2019.4）

论文：

《CenterNet: Keypoint Triplets for Object Detection》

![](/images/img3/CenterNet.png)

上图是CenterNet的网络结构图。

正如之前提到的，**框对于物体来说不是一个最好的表示**。同理，Corner也不是什么特别好的表示：绝大多数情况下，Corner同样是远离物体的。

也正是由于Corner和物体的关联度不大，CornerNet才发明了corner pooling操作，用以提取Corner。

但是即使这样，由于没有anchor的限制，使得任意两个角点都可以组成一个目标框，这就对判断两个角点是否属于同一物体的算法要求很高，一但准确度差一点，就会产生很多错误目标框。

有鉴于此，CenterNet除了Corner之外，还添加了Center的预测分支，也就是上图中的center pooling+center heatmap。这主要基于以下假设：**如果目标框是准确的，那么在其中心区域能够检测到目标中心点的概率就会很高，反之亦然。**

因此，首先利用左上和右下两个角点生成初始目标框，对每个预测框定义一个中心区域，然后判断每个目标框的中心区域是否含有中心点，若有则保留该目标框，若无则删除该目标框。

为了和CornerNet做比较，CenterNet同样使用了Hourglass Network作为骨干网络。并针对中心点和角点的提取，提出了Center pooling和Cascade corner pooling操作。这里不再赘述。

除此之外，下面的这篇论文提出的网络也叫CenterNet，思路也是类似的：

《Objects as Points》

参考：

https://mp.weixin.qq.com/s/wWqdjsJ6U86lML0rSohz4A

CenterNet：将目标视为点

https://zhuanlan.zhihu.com/p/62789701

中科院牛津华为诺亚提出CenterNet，one-stage detector可达47AP，已开源！

https://mp.weixin.qq.com/s/CEcN5Aljvs7AyOLPRFjUaw

真Anchor Free目标检测----CenterNet详解

https://mp.weixin.qq.com/s/7lwEn49G-3RDnBKBv5c7Ag

论文也撞衫，你更喜欢哪个无锚点CenterNet？

# Anchor-Free

在前面的章节，我们已经简要的分析了Anchor Free和Anchor Base模型的差异，并介绍了两个Anchor-Free的模型——CornerNet和CenterNet。

这里对其他比较重要的Anchor-Free模型做一个简单介绍。

## ExtremeNet

ExtremeNet是UT Austin的Xingyi Zhou的作品。（2019.1）

论文：

《Bottom-up Object Detection by Grouping Extreme and Center Points》

代码：

https://github.com/xingyizhou/ExtremeNet

![](/images/img3/ExtremeNet.png)

上图是ExtremeNet的网络结构图。它预测是关键点就不光是角点和中心点了，事实上它预测了9个点。具体的方法和CenterNet类似，也是heatmap抽取关键点。

显然，这类关键点算法是受到人脸/姿态关键点算法的启发，因此它们采用Hourglass Network作为骨干网络也就顺理成章了，后者正是比较经典的关键点算法模型之一。

## FoveaBox

论文：

《FoveaBox: Beyond Anchor-based Object Detector》

![](/images/img3/FoveaBox.png)

![](/images/img3/FoveaBox_2.png)

上两图是FoveaBox的网络结构图。

它的主要思路是：直接学习目标存在的概率和目标框的坐标位置，其中包括预测类别相关的语义图和生成类别无关的候选目标框。

事实上这和YOLOv1的思路是一致的。但FoveaBox比YOLOv1精度高，主要在于FPN提供了多尺度的信息，而YOLOv1只有单尺度的信息。

此外，Focal loss也是Anchor-Free模型的常用手段。

## 总结

Anchor-Free模型主要是为了解决Two-stage模型运算速度较慢的问题而提出的，因此它们绝大多数都是One-stage模型。从目前的效果来看，某些Anchor-Free模型其精度已经接近Two-stage模型，但运算速度相比YOLOv3等传统One-stage模型，仍有较大差距，尚无太大的实用优势（可以使用，但优势不大）。

其他比较知名的Anchor-Free模型还有：

- FCOS

《FCOS: Fully Convolutional One-Stage Object Detection》

- FSAF

《Feature Selective Anchor-Free Module》

- DenseBox

《DenseBox: Unifying Landmark Localization and Object Detection》

## 参考

https://mp.weixin.qq.com/s/T7DDWvtvCULfjcDmljvx5Q

Anchor-free的对象检测网络汇总

https://zhuanlan.zhihu.com/p/63024247

锚框：Anchor box综述

https://mp.weixin.qq.com/s/dYV446meJXtCQVFrLzWV8A

目标检测中Anchor的认识及理解

https://mp.weixin.qq.com/s/WAx3Zazx9Pq7Lb3vKa510w

目标检测最新方向：推翻固有设置，不再一成不变Anchor

https://zhuanlan.zhihu.com/p/64563186

Anchor free深度学习的目标检测方法

https://mp.weixin.qq.com/s/DoN-vha1H-2lHhbFOaVS8w

FoveaBox：目标检测新纪元，无Anchor时代来临！

https://zhuanlan.zhihu.com/p/62198865

最新的Anchor-Free目标检测模型FCOS，现已开源！

https://mp.weixin.qq.com/s/N93TrVnUuvAgfcoHXevTHw

FCOS: 最新的one-stage逐像素目标检测算法

https://mp.weixin.qq.com/s/04h80ubIxjJbT9BxQy5FSw

目标检测：Anchor-Free时代

https://zhuanlan.zhihu.com/p/66156431

从Densebox到Dubox：更快、性能更优、更易部署的anchor-free目标检测

https://zhuanlan.zhihu.com/p/63273342

聊聊Anchor的"前世今生"（上）

https://zhuanlan.zhihu.com/p/68291859

聊聊Anchor的"前世今生"（下）

https://zhuanlan.zhihu.com/p/62372897

物体检测的轮回：anchor-based与anchor-free

https://mp.weixin.qq.com/s/m_PvEbq2QbTXNmj_gObKmQ

Anchor-free目标检测之ExtremeNet

https://mp.weixin.qq.com/s/nI_3kilFCsDHhtjFhRKytA

RepPoints:替代边界框，基于点集的物体表示新方法

https://mp.weixin.qq.com/s/E8FXhe5H9VqXkkGSUsxYqA

目标检测中Anchor的本质分析

# 其它目标检测网络

## A-Fast-RCNN

A-Fast-RCNN首次将对抗学习引入到了目标检测领域，idea是非常创新的。

http://blog.csdn.net/jesse_mx/article/details/72955981

A-Fast-RCNN论文笔记

## G-CNN

G-CNN是MaryLand大学的工作，论文主要的思路也是消除region proposal，和YOLO，SSD不同，G-CNN的工作借鉴了迭代的想法，把边框检测等价于找到初始边框到最终目标的一个路径。但是使用one-step regression不能处理这个非线性的过程，所以作者采用迭代的方法逐步接近最终的目标。

http://blog.csdn.net/zijin0802034/article/details/53535647

G-CNN: an Iterative Grid Based Object Detector

# 花式IOU

https://zhuanlan.zhihu.com/p/57992040

使用GIoU作为检测任务的Loss

https://mp.weixin.qq.com/s/ZbryNlV3EnODofKs2d01RA

目标检测回归损失函数简介：Smooth L1/IoU/GIoU/DIoU/CIoU Loss

https://mp.weixin.qq.com/s/F07Wp-cXIOE4-qdL55WtJQ

基于DIou改进的YOLOv3目标检测

https://mp.weixin.qq.com/s/YeyxuN6RCgF2TcxctXiJQg

使用GIoU作为目标检测新loss

https://mp.weixin.qq.com/s/m8D5uutwlvkQNucFX_TnGw

DIoU和CIoU：IoU在目标检测中的正确打开方式

https://mp.weixin.qq.com/s/VtBfIVj74dhg9HjpxN7rhw

DIoU损失函数详解

https://zhuanlan.zhihu.com/p/109677830

Distance-IoU Loss

# 目标检测进阶

https://mp.weixin.qq.com/s/1nlOJ7X9ogBHTl1j2adqyg

83页《目标分类和目标检测综述（2D和3D数据）》论文

https://mp.weixin.qq.com/s/HmUhlw90b2aTsoEwBdYbdQ

目标检测二十年技术综述

https://mp.weixin.qq.com/s/S1IrgEqS1Q4xqGl5adNrlg

目标检测近年综述

https://mp.weixin.qq.com/s/7QT7n9MpbXjo5-r-aY2Yvg

深度学习目标检测方法综述

https://mp.weixin.qq.com/s/5I9uzGCNFD93L1mzakTl0Q

目标检测网络学习总结（RCNN-->YOLO V3）

https://mp.weixin.qq.com/s/8Vac8MRpmviVDKRrAeFR0A

后R-CNN时代，Faster R-CNN、SSD、YOLO各类变体统治下的目标检测综述：Faster R-CNN系列胜了吗？

https://mp.weixin.qq.com/s/zeruKQOye_QNWgluVIN0BA

从R-CNN到RFBNet，目标检测架构5年演进全盘点

https://mp.weixin.qq.com/s/sCGNUI-mUSYxD69uBDQNoQ

基于深度学习的目标检测算法综述：算法改进

https://mp.weixin.qq.com/s/yswy7VwEapQJ9M5n_Uo93w

目标检测最新进展总结与展望

https://mp.weixin.qq.com/s/s1qmCA8djEEanwCxeLSV2Q

63页《深度CNN-目标检测》综述

https://mp.weixin.qq.com/s/j-arl6qiD6mei4crfQPrgw

《深度学习显著目标检测综述》

https://mp.weixin.qq.com/s/2PLp2xNfhkHB3fPQr5Ts6g

密歇根大学40页《20年目标检测综述》最新论文，带你全面了解目标检测方法

https://mp.weixin.qq.com/s/Pl8HABuVN27CZv-lvGROTw

基于深度学习的目标检测算法近5年发展历史

https://mp.weixin.qq.com/s/S6sz5dPgGNcJvrIAZ3ZjGg

基于深度学习的通用物体检测算法对比探索

https://zhuanlan.zhihu.com/p/59915784

目标检测中的Consistent Optimization

https://mp.weixin.qq.com/s/ts4WFnuN4cHLfUh8--96Kw

Libra R-CNN：全面平衡的目标检测器

https://mp.weixin.qq.com/s/groq55Cbts272k1mfhJwaQ

超越bounding box的代表性点集：视觉物体表示的新方法

https://mp.weixin.qq.com/s/BXwL33qOf3f7BtJvHsi23Q

目标检测：Segmentation is All You Need？
