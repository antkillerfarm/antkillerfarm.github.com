---
layout: post
title:  深度目标检测（七）——CornerNet, CenterNet, Anchor-Free
category: Deep Object Detection 
---

* toc
{:toc}

# CornerNet（续）

## Bottom-right corners & Top-left Corners Prediction Module

CornerNet堆叠两个Hourglass Network生成Top-left和Bottom-right corners，每一个corners都包括corners Pooling，以及对应的Heatmaps, Embeddings vector和offsets。

![](/images/img3/CornerNet.png)

上图是Heatmaps, Embeddings vector的示意图。

- heatmaps包含C channels（C是目标的类别，没有background channel），每个channel是二进制掩膜，表示相应类别的顶点位置。

- embedding vector使相同目标的两个顶点（左上角和右下角）距离最短。或者也可以反过来说，**两个顶点的embedding vector越相近，则它们越有可能配对。**

- offsets用于调整生成更加紧密的边界定位框。

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

https://mp.weixin.qq.com/s/X1FCnSIr-iMwU9Sessxkfw

CentripetalNet：更合理的角点匹配，多方面改进CornerNet

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

真Anchor Free目标检测---CenterNet详解

https://mp.weixin.qq.com/s/7lwEn49G-3RDnBKBv5c7Ag

论文也撞衫，你更喜欢哪个无锚点CenterNet？

https://mp.weixin.qq.com/s/lE_HoLUfz8ehNmuj0fmstg

PyTorch版CenterNet训练自己的数据集

https://mp.weixin.qq.com/s/MZDtlZogXvgYNqlmc87LYg

CenterNet的骨干网络之DLASeg

https://mp.weixin.qq.com/s/CcUlqZA2KG5P42sCpsj94g

CenterNet之loss计算代码解析

https://mp.weixin.qq.com/s/Vu_qfJkM36v7f8VQPLCU6w

Objects as Points：预测目标中心，无需NMS等后处理操作

# Anchor-Free

![](/images/img4/OD.png)

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

## FCOS

《FCOS: Fully Convolutional One-Stage Object Detection》

https://zhuanlan.zhihu.com/p/62198865

最新的Anchor-Free目标检测模型FCOS，现已开源！

https://mp.weixin.qq.com/s/N93TrVnUuvAgfcoHXevTHw

FCOS: 最新的one-stage逐像素目标检测算法

https://mp.weixin.qq.com/s/ebTbbo-IeqyzDqSn2bgjsQ

带你捋一捋anchor-free的检测模型：FCOS

https://zhuanlan.zhihu.com/p/156112318

FCOS算法的原理与实现

https://mp.weixin.qq.com/s/Q-DSG-ZAW0z0X6LHDLmFFA

FCOS：全卷积一阶段Anchor Free物体检测器，多种视觉任务的统一框架

https://mp.weixin.qq.com/s/5YIBEmDAevosxEs-jRJNVg

anchor-free的回归之作：FCOS

## 总结

对于基于anchors的目标检测器，为了获得最优的检测性能，需要在训练前进行聚类分析来确定一组最优锚点，但是这也带来了一些问题：

- 聚类得到的锚框一般只能用在该数据集上，并不是通用的；
- 锚框增加了检测头的复杂度以及生成结果的数量。

Anchor-free机制显著减少了需要启发式调整的设计参数的数量和涉及的许多技巧(例如，锚定群集、网格敏感)。使得检测器，特别是其训练和解码阶段变得相当简单。

---

Anchor-Free模型主要是为了解决Two-stage模型运算速度较慢的问题而提出的，因此它们绝大多数都是One-stage模型。

从目前的效果来看，某些Anchor-Free模型其精度已经接近Two-stage模型，但运算速度相比YOLOv3等传统One-stage模型，仍有较大差距，尚无太大的实用优势（可以使用，但优势不大）。

其他比较知名的Anchor-Free模型还有：

- FSAF

《Feature Selective Anchor-Free Module》

- DenseBox

《DenseBox: Unifying Landmark Localization and Object Detection》

## Sparse R-CNN

https://zhuanlan.zhihu.com/p/310058362

Sparse R-CNN

https://mp.weixin.qq.com/s/wLESASiP3hofrDKH7yk1aA

Sparse R-CNN: 在dense（单阶段），dense2sparse（二阶段）之外的另一种物体检测模式

https://mp.weixin.qq.com/s/LRSY2YsZGB_Vye7EbvhXpA

Sparse R-CNN: 稀疏的目标检测，武装Fast RCNN

## RepPoints

https://mp.weixin.qq.com/s/aWv7_yiX5BFKpL21SRV1MQ

RepPoints：可形变卷积的进阶

https://mp.weixin.qq.com/s/nI_3kilFCsDHhtjFhRKytA

RepPoints:替代边界框，基于点集的物体表示新方法

https://mp.weixin.qq.com/s/VTb6CUOWnPpyU6WnYdYJ-g

RepPoints V2：将角点检测和前景热图引入纯回归目标检测算法

https://mp.weixin.qq.com/s/gnTZ-q2-lm8QPH6JEPylnw

RepPointv2：使用点集合表示来做目标检测

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

https://mp.weixin.qq.com/s/LGeNgnXfYaVVAc_37j6-2A

Anchor Free及Transformer时代

https://mp.weixin.qq.com/s/52YBmlHioRkUgetZWHMZOw

Anchor-free目标检测：工业应用更友好的新网络

https://zhuanlan.zhihu.com/p/84398108

目标检测中Anchor的本质分析

https://mp.weixin.qq.com/s/LQOzrlaEOsrsMHj-V8l3hQ

FreeAnchor：抛弃单一的IoU匹配，更自由的anchor匹配方法

https://zhuanlan.zhihu.com/p/163266388

Anchor-free应用一览：目标检测、实例分割、多目标跟踪
