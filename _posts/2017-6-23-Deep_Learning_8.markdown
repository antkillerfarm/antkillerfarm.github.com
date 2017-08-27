---
layout: post
title:  深度学习（八）——fine-tuning, 李飞飞, 目标检测, RCNN
category: theory 
---

# fine-tuning

fine-tuning和迁移学习虽然是两个不同的概念。但局限到CNN的训练领域，基本可以将fine-tuning看作是一种迁移学习的方法。

举个例子，假设今天老板给你一个新的数据集，让你做一下图片分类，这个数据集是关于Flowers的。问题是，数据集中flower的类别很少，数据集中的数据也不多，你发现从零训练开始训练CNN的效果很差，很容易过拟合。怎么办呢，于是你想到了使用Transfer Learning，用别人已经训练好的Imagenet的模型来做。

由于ImageNet数以百万计带标签的训练集数据，使得如CaffeNet之类的预训练的模型具有非常强大的泛化能力，这些预训练的模型的中间层包含非常多一般性的视觉元素，我们只需要对他的后几层进行微调，再应用到我们的数据上，通常就可以得到非常好的结果。最重要的是，**在目标任务上达到很高performance所需要的数据的量相对很少**。

虽然从理论角度尚无法完全解释fine-tuning的原理，但是还是可以给出一些直观的解释。我们知道，CNN越靠近输入端，其抽取的图像特征越原始。比如最初的一层通常只能抽取一些线条之类的元素。越上层，其特征越抽象。

而现实的图像无论多么复杂，总是由简单特征拼凑而成的。因此，无论最终的分类结果差异如何巨大，其底层的图像特征却几乎一致。

参考：

https://zhuanlan.zhihu.com/p/22624331

fine-tuning:利用已有模型训练其他数据集

http://www.cnblogs.com/louyihang-loves-baiyan/p/5038758.html

Caffe fine-tuning微调网络

http://blog.csdn.net/sinat_26917383/article/details/54999868

caffe中fine-tuning模型三重天（函数详解、框架简述）+微调技巧

http://yongyuan.name/blog/layer-selection-and-finetune-for-cbir.html

图像检索：layer选择与fine-tuning性能提升验证

h1ttps://www.zhihu.com/question/49534423

迁移学习与fine-tuning有什么区别？

# 李飞飞

## AI大佬

李飞飞是吴恩达之后的华裔AI新大佬。巧合的是，他们都是斯坦福AP+AI lab的主任，只不过吴是李的前任而已。

**李飞飞（Fei-Fei Li）**，1976年生，成都人，16岁移民美国。普林斯顿大学本科（1995～1999）+加州理工学院博士（2001～2005）。先后执教于UIUC、普林斯顿、斯坦福等学校。

个人主页：

http://vision.stanford.edu/feifeili/

## 大佬的门徒

比如可爱的妹子**Serena Yeung**。这个妹子是斯坦福的本硕博。出身不详，但从姓名的英文拼法来看，应该是美国土生的华裔。Yeung是杨、阳、羊等姓的传统英文拼法，但显然不是大陆推行的拼音拼法。（可以对比的是Fei-Fei Li和Bruce Lee，对于姓的不同拼法）

个人主页：

http://ai.stanford.edu/~syyeung/

还有当红的“辣子鸡”：**Andrej Karpathy**，多伦多大学本科（2009）+英属不列颠哥伦比亚大学硕士（2011）+斯坦福博士（2015）。现任特斯拉AI总监。

吐槽一下：英属不列颠哥伦比亚大学其实是加拿大的一所大学。

个人主页：

http://cs.stanford.edu/people/karpathy/

Andrej Karpathy建了一个检索arxiv的网站，主要搜集了近3年来的ML/DL领域的论文。网址：

http://www.arxiv-sanity.com/

**李佳（Jia Li）**，李飞飞的开山大弟子，追随她从UIUC、普林斯顿到斯坦福。目前又追随其到Google。大约是知道自己的名字是个大路货，她的笔名叫做Li-Jia Li。

个人主页：

http://vision.stanford.edu/lijiali/

## 学神

应该说李飞飞和吴恩达都是万里挑一的超卓人物，但是和学神还是有所差距。下面是两个80后的华裔学神，他们都已经是正教授了：

**尹希**，1983年生，哈佛大学物理系教授。

**张锋**，1982年生，MIT教授，生物学家。

这两个人都是有机会挑战诺奖的人，而李和吴暂时还没有这个可能性。

## 网红

这里收录了一些非李飞飞门下的AI网红。

**Zachary Chase Lipton**，1985年生，哥伦比亚大学本科+UCSD博士，CMU的AP。他的另一身份——Jazz歌手，可比他的学术成就知名多了。

个人主页：

http://zacklipton.com/

# 目标检测

## 概述

object detection是计算机视觉的一个重要的分支。类似的分支还有目标分割、目标识别和目标跟踪。

以下摘录自Sensetime CTO曹旭东的解读：

传统方法使用滑动窗口的框架，把一张图分解成几百万个不同位置不同尺度的子窗口，针对每一个窗口使用分类器判断是否包含目标物体。传统方法针对不同的类别的物体，一般会设计不同的特征和分类算法，比如人脸检测的经典算法是**Harr特征+Adaboosting分类器**；行人检测的经典算法是**HOG(histogram of gradients)+Support Vector Machine**；一般性物体的检测的话是**HOG特征+DPM(deformable part model)的算法**。

基于深度学习的物体检测的经典算法是RCNN系列：RCNN，fast RCNN(Ross Girshick)，faster RCNN(少卿、凯明、孙剑、Ross)。这三个工作的核心思想是分别是：使用更好的CNN模型判断候选区域的类别；复用预计算的sharing feature map加快模型训练和物体检测的速度；进一步使用sharing feature map大幅提高计算候选区域的速度。其实基于深度学习的物体检测也可以看成对海量滑动窗口分类，只是用全卷积的方式。

RCNN系列算法还是将物体检测分为两个步骤。现在还有一些工作是端到端(end-to-end)的物体检测，比如说YOLO(You Only Look Once: Unified, Real-Time Object Detection)和SSD(SSD: Single Shot MultiBox Detector)这样的算法。这两个算法号称和faster RCNN精度相似但速度更快。物体检测正负样本极端非均衡，two-stage cascade可以更好的应对非均衡。端到端学习是否可以超越faster RCNN还需要更多研究实验。

参考：

https://www.zhihu.com/question/34223049

从近两年的CVPR会议来看，目标检测的研究方向是怎么样的？

https://zhuanlan.zhihu.com/p/21533724

对话CVPR2016：目标检测新进展

https://mp.weixin.qq.com/s/r9tXvKIN-eqKW_65yFyOew

谷歌开源TensorFlow Object Detection API

https://mp.weixin.qq.com/s/-PeXMU_gkcT5YnMcLoaKag

CVPR清华大学研究，高效视觉目标检测框架RON

https://mp.weixin.qq.com/s/_cOuhToH8KvZldNfraumSQ

什么促使了候选目标的有效检测？

https://mp.weixin.qq.com/s/LAy1LKGj5HOh_e9jPgvfQw

视觉目标检测和识别之过去，现在及可能

## 进化史

DPM(2007)->RCNN(2014)->Fast RCNN->Faster RCNN

![](/images/article/rcnn_2.png)

参考：

http://blog.csdn.net/ttransposition/article/details/12966521

DPM(Deformable Parts Model)--原理

## CV实践的难点

从理论上说，无论是传统CV，还是新近崛起的DL CV，其本质都是通过比对目标图片和训练图片的相似度，从而得到识别的结果。

CV的输入一般是由像素组成的矩阵。相比其他领域的数据挖掘而言，CV的实践难点主要包括：

1.视角的改变。照相机的移动会导致像素矩阵发生平移、旋转等变换。

2.光照影响。同一物体在不同光照条件下的影像有所不同。

3.形变。典型的例子是动物的运动，会导致外观的改变。

4.遮挡。待识别物体通常不是完全可见的。

5.背景。雪地、沙滩等不同场景，会影响物体的识别。

6.同类差异。比如各种猫都是猫，但它们的外观有细微的差异。

![](/images/article/cv_problem.png)

# RCNN

《深度学习（五）》中提到的AlexNet、VGG、GoogleNet主要用于图片分类。而这里介绍的RCNN(Regions with CNN)主要用于目标检测。

## 车牌识别的另一种思路

在介绍RCNN之前，我首先介绍一下2013年的一个车牌识别项目的解决思路。

车牌识别差不多是深度学习应用到CV领域之前，CV领域少数几个达到实用价值的应用之一。国内在2010～2015年前后，有许多公司都做过类似的项目。其产品更是随处可见，很多停车场已经利用该技术，自动识别车辆信息。

车牌识别的难度不高——无论是目标字符集，还是目标字体，都很有限。但也有它的技术难点：

1.计算资源有限。通常就是PC，甚至嵌入式设备，不可能用大规模集群来计算。

2.有实时性的要求，通常处理时间不超过3s。

因此，如何快速的在图片中找到车牌所在区域，就成为了关键问题。

常规的做法，通常是根据颜色、形状找到车牌所在区域，但鲁棒性不佳。后来，有个同事提出了改进方法：

**Step 1**：在整个图片中，基于haar算子，寻找疑似数字的区域。

**Step 2**：将数字聚集的区域设定为疑似车牌所在区域。

**Step 3**：投入更大运算量，以识别车牌上的文字。（这一步是常规做法。）

## RCNN的基本原理

RCNN是Ross Girshick于2014年提出的深度模型。

>注：Ross Girshick（网名：rbg），芝加哥大学博士（2012），Facebook研究员。他和何恺明被誉为CV界深度学习的**双子新星**。   
>个人主页：   
>http://www.rossgirshick.info/

论文：

《Rich feature hierarchies for accurate object detection and semantic segmentation》

代码：

https://github.com/rbgirshick/rcnn

RCNN相对传统方法的改进：

**速度**：经典的目标检测算法使用滑动窗法依次判断所有可能的区域。RCNN则(采用Selective Search方法)预先提取一系列较可能是物体的候选区域，之后仅在这些候选区域上(采用CNN)提取特征，进行判断。

**训练集**：经典的目标检测算法在区域中提取人工设定的特征。RCNN则采用深度网络进行特征提取。

使用两个数据库：

一个较大的识别库（ImageNet ILSVC 2012）：标定每张图片中物体的类别。一千万图像，1000类。

一个较小的检测库（PASCAL VOC 2007）：标定每张图片中，物体的类别和位置，一万图像，20类。

RCNN使用识别库进行预训练得到CNN（有监督预训练），而后用检测库调优参数，最后在检测库上评测。

这实际上就是《深度学习（七）》中提到的fine-tuning的思想。

## RCNN算法的基本流程

![](/images/article/rcnn.png)

RCNN算法分为4个步骤：

**Step 1**：候选区域生成。一张图像生成1K~2K个候选区域（采用Selective Search方法）。

**Step 2**：特征提取。对每个候选区域，使用深度卷积网络提取特征（CNN）。

**Step 3**：类别判断。特征送入每一类的SVM分类器，判别是否属于该类。

**Step 4**：位置精修。使用回归器精细修正候选框位置。

