---
layout: post
title:  深度学习（十一）——目标检测, RCNN
category: theory 
---

# 目标检测（续）

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

## 参考

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

https://mp.weixin.qq.com/s/ZHRP5xnQxex7lQJsCxwblA

深度学习目标检测的主要问题和挑战！

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

https://mp.weixin.qq.com/s/JPCQqyzR8xIUyAdk_RI5dA

RCNN, Fast-RCNN, Faster-RCNN那些你必须知道的事！

https://www.zhihu.com/question/35887527

如何评价rcnn、fast-rcnn和faster-rcnn这一系列方法？

http://blog.csdn.net/messiran10/article/details/49132053

Caffe matlab之基于Alex network的特征提取

https://mp.weixin.qq.com/s/YovhKYeGGLqSxxSqMNsbKg

基于深度学习的目标检测学习总结

https://mp.weixin.qq.com/s/nGSaQXm8AczYodtmHD1qNA

深度学习目标检测模型全面综述：Faster R-CNN、R-FCN和SSD

https://mp.weixin.qq.com/s/c2oMJfE95I1ciEtvdTlb4A

完全脱离预训练模型的目标检测方法

https://mp.weixin.qq.com/s/NV2hWofOCractLt45-wI1A

山世光：基于深度学习的目标检测技术进展与展望

https://mp.weixin.qq.com/s/zJ3EN175_9num2OknVvnyA

邬书哲：物体检测算法的革新与传承

https://mp.weixin.qq.com/s/1vOdOMyByBacSBMVrscq5Q

黄畅：基于DenesBox的目标检测在自动驾驶中的应用

https://mp.weixin.qq.com/s/6rSeJOqbKyrDj3FpS8J5eg

黄李超讲物体检测

https://mp.weixin.qq.com/s/JjsAnB_OxKS1Af9XAtw5sA

一文带你读懂深度学习框架下的目标检测

https://mp.weixin.qq.com/s/dcrBQ-t3tLOTouEyofOBxg

间谍卫星：利用卷积神经网络对卫星影像进行多尺度目标检测

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

## Selective Search

论文：

https://www.koen.me/research/pub/uijlings-ijcv2013-draft.pdf

Selective Search for Object Recognition

Selective Search的主要思想:

**Step 1**：使用一种过分割手段，将图像分割成小区域 (1k~2k个)。

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

Selective Search的效果类似下图：

![](/images/article/selective_search.png)

![](/images/article/rcnn_3.png)

上图中的那些方框，就是bounding box。

一般使用IOU（Intersection over Union，交并比）指标，来衡量两个bounding box的重叠度：

$$IOU(A,B)=\frac{A \cap B}{A \cup B}$$

## 非极大值抑制（NMS）

RCNN会从一张图片中找出n个可能是物体的矩形框，然后为每个矩形框为做类别分类概率（如上图所示）。我们需要判别哪些矩形框是没用的。

Non-Maximum Suppression顾名思义就是抑制不是极大值的元素，搜索局部的极大值。这个局部代表的是一个邻域，邻域有两个参数可变，一是邻域的维数，二是邻域的大小。

下面举例说明NMS的做法：

假设有6个矩形框，根据分类器的类别和分类概率做排序，假设从小到大属于车辆的概率分别为A、B、C、D、E、F。

**Step 1**：从最大概率矩形框F开始，分别判断A~E与F的重叠度IOU是否大于某个设定的阈值。（**确定领域**）

**Step 2**：假设B、D与F的重叠度超过阈值，那么就扔掉B、D；并标记第一个矩形框F，是我们保留下来的。（**抑制领域内的非极大值**）

**Step 3**：从剩下的矩形框A、C、E中，选择概率最大的E，然后判断E与A、C的重叠度，重叠度大于一定的阈值，那么就扔掉；并标记E是我们保留下来的第二个矩形框。（**确定下一个领域，并抑制该领域内的非极大值**）

参考：

http://mp.weixin.qq.com/s/Cg9tHG1YgDCdI3NPYl5-vQ

如何用Soft-NMS实现目标检测并提升准确率

## ground truth

在有监督学习中，数据是有标注的，以(x,t)的形式出现，其中x是输入数据，t是标注。正确的t标注是ground truth，错误的标记则不是。（也有人将所有标注数据都叫做ground truth）

在目标检测任务中，ground truth主要包括box和category两类信息。

## 正负样本问题

一张照片我们得到了2000个候选框。然而人工标注的数据一张图片中就只标注了正确的bounding box，我们搜索出来的2000个矩形框也不可能会出现一个与人工标注完全匹配的候选框。因此在CNN阶段我们需要用IOU为2000个bounding box打标签。

如果用selective search挑选出来的候选框与物体的人工标注矩形框的重叠区域IoU大于0.5，那么我们就把这个候选框标注成物体类别（正样本），否则我们就把它当做背景类别（负样本）。

