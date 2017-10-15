---
layout: post
title:  深度学习（十一）——RCNN
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

## 使用SVM的问题

CNN训练的时候，本来就是对bounding box的物体进行识别分类训练，在训练的时候，最后一层softmax就是分类层。那么为什么作者闲着没事干要先用CNN做特征提取（提取fc7层数据），然后再把提取的特征用于训练SVM分类器？

这个是因为SVM训练和cnn训练过程的正负样本定义方式各有不同，导致最后采用CNN softmax输出比采用SVM精度还低。

事情是这样的，cnn在训练的时候，对训练数据做了比较宽松的标注，比如一个bounding box可能只包含物体的一部分，那么我也把它标注为正样本，用于训练cnn；采用这个方法的主要原因在于因为CNN容易过拟合，所以需要大量的训练数据，所以在CNN训练阶段我们是对Bounding box的位置限制条件限制的比较松(IOU只要大于0.5都被标注为正样本了)；

然而SVM训练的时候，因为SVM适用于少样本训练，所以对于训练样本数据的IOU要求比较严格，我们只有当bounding box把整个物体都包含进去了，我们才把它标注为物体类别，然后训练SVM。

## CNN base

目标检测任务不是一个独立的任务，而是在目标分类基础之上的进一步衍生。因此，无论何种目标检测框架都需要一个目标分类的CNN作为base，仅对其最上层的FC层做一定的修改。

VGG、AlexNet都是常见的CNN base。

## 评价标准

目标检测一般采用mAP（mean Average Precision）作为评价标准。AP的含义参见《机器学习（二十一）》。

对于多分类任务来说，每个分类都有一个AP，将这些AP平均（或加权平均）之后，就得到了mAP。

目前，目标检测领域的mAP，一般以PASCAL VOC 2012的标准为准。文档参见：

http://host.robots.ox.ac.uk/pascal/VOC/voc2012/devkit_doc.pdf

对于目标检测任务来说，除了分类之外，还有box准确度的问题。一般IOU大于0.5的被认为是正样本，反之则是负样本。

PASCAL VOC还对P-R曲线的采样做出规定。2012之前的标准中，P-R曲线只需要对recall值进行10等分采样即可。而2012标准规定，对每个recall值都要进行采样。

参考：

http://blog.sina.com.cn/s/blog_9db078090102whzw.html

多标签图像分类任务的评价方法-mAP

https://www.zhihu.com/question/41540197

mean average precision（MAP）在计算机视觉中是如何计算和应用的？

## 总结

![](/images/article/rcnn_p_2.png)

![](/images/article/rcnn_p.png)

## 参考

https://zhuanlan.zhihu.com/p/23006190

RCNN-将CNN引入目标检测的开山之作

http://www.cnblogs.com/edwardbi/p/5647522.html

Tensorflow tflearn编写RCNN

http://blog.csdn.net/u011534057/article/category/6178027

RCNN系列blog

http://blog.csdn.net/shenxiaolu1984/article/details/51066975

RCNN算法详解

http://mp.weixin.qq.com/s/_U6EJBP_qmx68ih00IhGjQ

Object Detection R-CNN

