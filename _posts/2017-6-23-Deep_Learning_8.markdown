---
layout: post
title:  深度学习（八）——目标检测, RCNN
category: theory 
---

# 目标检测

## 概述（续）

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
>个人主页：http://www.rossgirshick.info/

论文：

《Rich feature hierarchies for accurate object detection and semantic segmentation》

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

## Selective Search

论文：

https://www.koen.me/research/pub/uijlings-ijcv2013-draft.pdf

Selective Search for Object Recognition

Selective Search的主要思想:

**Step 1**：使用一种过分割手段，将图像分割成小区域 (1k~2k 个)。

这里的步骤实际上并不简单，可参考论文：

《Efficient Graph-Based Image Segmentation》

中文版：

http://blog.csdn.net/surgewong/article/details/39008861

**Step 2**：查看现有小区域，按照合并规则合并可能性最高的相邻两个区域。重复直到整张图像合并成一个区域位置。

**Step 3**：输出所有曾经存在过的区域，所谓候选区域。

其中合并规则如下：优先合并以下四种区域：

1.颜色（颜色直方图）相近的。

2.纹理（梯度直方图）相近的。

3.合并后总面积小的：保证合并操作的尺度较为均匀，避免一个大区域陆续“吃掉”其他小区域（例：设有区域a-b-c-d-e-f-g-h。较好的合并方式是：ab-cd-ef-gh -> abcd-efgh -> abcdefgh。不好的合并方法是：ab-c-d-e-f-g-h ->abcd-e-f-g-h ->abcdef-gh -> abcdefgh）

4.合并后，总面积在其bounding box中所占比例大的：保证合并后形状规则。

Step2和Step3可参考论文：

《Selective Search for Object Recognition》

中文版：

http://blog.csdn.net/surgewong/article/details/39316931

http://blog.csdn.net/charwing/article/details/27180421

![](/images/article/rcnn_3.png)

上图中的那些方框，就是bounding box。

一般使用IOU（Intersection over Union，交并比）指标，来衡量两个bounding box的重叠度：

$$IOU(A,B)=\frac{A \cap B}{A \cup B}$$

## 非极大值抑制（NMS）

Non-Maximum Suppression顾名思义就是抑制不是极大值的元素，搜索局部的极大值。这个局部代表的是一个邻域，邻域有两个参数可变，一是邻域的维数，二是邻域的大小。

下面举例说明NMS的做法：

假设有6个矩形框，根据分类器的类别和分类概率做排序，假设从小到大属于车辆的概率分别为A、B、C、D、E、F。

**Step 1**：从最大概率矩形框F开始，分别判断A~E与F的重叠度IOU是否大于某个设定的阈值。（**确定领域**）

**Step 2**：假设B、D与F的重叠度超过阈值，那么就扔掉B、D；并标记第一个矩形框F，是我们保留下来的。（**抑制领域内的非极大值**）

**Step 3**：从剩下的矩形框A、C、E中，选择概率最大的E，然后判断E与A、C的重叠度，重叠度大于一定的阈值，那么就扔掉；并标记E是我们保留下来的第二个矩形框。（**确定下一个领域，并抑制该领域内的非极大值**）



## 参考

https://zhuanlan.zhihu.com/p/23006190

RCNN-将CNN引入目标检测的开山之作

http://www.cnblogs.com/edwardbi/p/5647522.html

Tensorflow tflearn编写RCNN

https://zhuanlan.zhihu.com/p/24774302

SPPNet-引入空间金字塔池化改进RCNN

https://zhuanlan.zhihu.com/p/24780395

Fast R-CNN

https://zhuanlan.zhihu.com/p/24916624

Faster R-CNN

https://zhuanlan.zhihu.com/p/24916786

图解YOLO

https://zhuanlan.zhihu.com/p/24954433

SSD

https://zhuanlan.zhihu.com/p/25167153

YOLO2

https://www.zhihu.com/question/35887527

如何评价rcnn、fast-rcnn和faster-rcnn这一系列方法？

http://blog.csdn.net/tangwei2014/article/details/50915317

论文阅读笔记：You Only Look Once: Unified, Real-Time Object Detection

http://blog.csdn.net/shenxiaolu1984/article/details/51066975

RCNN算法详解

http://blog.csdn.net/shenxiaolu1984/article/details/51036677

Fast RCNN算法详解

http://blog.csdn.net/shenxiaolu1984/article/details/51152614

Faster RCNN算法详解

https://mp.weixin.qq.com/s/XorPkuIdhRNI1zGLwg-55A

斯坦福新深度学习系统 NoScope：视频对象检测快1000倍

https://mp.weixin.qq.com/s/XbgmLmlt5X4TX5CP59gyoA

目标检测算法精彩集锦

https://mp.weixin.qq.com/s/BgTc1SE2IzNH27OC2P2CFg

CVPR-I

https://mp.weixin.qq.com/s/qMdnp9ZdlYIja2vNEKuRNQ

CVPR—II

https://mp.weixin.qq.com/s/tc1PsIoF1RN1sx_IFPmtWQ

CVPR—III

https://mp.weixin.qq.com/s/bpCn2nREHzazJYq6B9vMHg

目标识别算法的进展

https://mp.weixin.qq.com/s/YzxaS4KQmpbUSnyOwccn4A

基于深度学习的目标检测技术进展与展望

https://mp.weixin.qq.com/s/VKQufVUQ3TP5m7_2vOxnEQ

通过Faster R-CNN实现当前最佳的目标计数

## YOLO

YOLO: Real-Time Object Detection，是一个基于神经网络的实时对象检测软件。

官网：

https://pjreddie.com/darknet/yolo/

参考：

https://mp.weixin.qq.com/s/n51XtGAsaDDAatXYychXrg

YOLO比R-CNN快1000倍，比Fast R-CNN快100倍的实时对象检测！

## SSD

论文：

《SSD: Single Shot MultiBox Detector》

参考：

http://www.jianshu.com/p/ebebfcd274e6

Caffe-SSD 训练自己的数据集教程

## ENet

https://github.com/TimoSaemann/ENet

《ENet: A Deep Neural Network Architecture for Real-Time Semantic Segmentation》

http://blog.csdn.net/zijinxuxu/article/details/67638290

