---
layout: post
title:  深度学习（九）——SPPNet, Fast R-CNN
category: theory 
---

# RCNN（续）

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

## 正负样本问题

一张照片我们得到了2000个候选框。然而人工标注的数据一张图片中就只标注了正确的bounding box，我们搜索出来的2000个矩形框也不可能会出现一个与人工标注完全匹配的候选框。因此在CNN阶段我们需要用IOU为2000个bounding box打标签。

如果用selective search挑选出来的候选框与物体的人工标注矩形框的重叠区域IoU大于0.5，那么我们就把这个候选框标注成物体类别（正样本），否则我们就把它当做背景类别（负样本）。

## 总结

![](/images/article/rcnn_p.png)

## 参考

https://zhuanlan.zhihu.com/p/23006190

RCNN-将CNN引入目标检测的开山之作

http://www.cnblogs.com/edwardbi/p/5647522.html

Tensorflow tflearn编写RCNN

http://blog.csdn.net/u011534057/article/category/6178027

RCNN系列blog

https://zhuanlan.zhihu.com/p/24916624

Faster R-CNN

https://www.zhihu.com/question/35887527

如何评价rcnn、fast-rcnn和faster-rcnn这一系列方法？

http://blog.csdn.net/tangwei2014/article/details/50915317

论文阅读笔记：You Only Look Once: Unified, Real-Time Object Detection

http://blog.csdn.net/shenxiaolu1984/article/details/51066975

RCNN算法详解

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

https://mp.weixin.qq.com/s/JPCQqyzR8xIUyAdk_RI5dA

RCNN, Fast-RCNN, Faster-RCNN那些你必须知道的事！

http://blog.csdn.net/messiran10/article/details/49132053

Caffe matlab之基于Alex network的特征提取

# SPPNet

SPPNet是何恺明2014年的作品。

论文：

《Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition》

在RCNN算法中，一张图片会有1~2k个候选框，每一个都要单独输入CNN做卷积等操作很费时。而且这些候选框可能很多都是重合的，重复的CNN操作从信息论的角度，也是相当冗余的。

![](/images/article/rcnn_vs_spp.png)

SPPNet的核心思想如上图所示：在feature map上提取ROI特征，这样就只需要在整幅图像上做一次卷积。

这个想法说起来简单，但落到实地，还有如下问题需要解决：

**Problem 1**：原始图像的ROI如何映射到特征图（一系列卷积层的最后输出）。

**Problem 2**：ROI的在特征图上的对应的特征区域的维度不满足全连接层的输入要求怎么办（又不可能像在原始ROI图像上那样进行截取和缩放）？

对于Problem 2我们分析一下：

这个问题涉及的流程主要有: 图像输入->卷积层1->池化1->...->卷积层n->池化n->全连接层。

引发问题的原因主要有：全连接层的输入维度是固定死的，导致池化n的输出必须与之匹配，继而导致图像输入的尺寸必须固定。

解决办法可能有：

1.想办法让不同尺寸的图像也可以使池化n产生固定的输出维度。（打破图像输入的固定性）

2.想办法让全连接层（罪魁祸首）可以接受非固定的输入维度。（打破全连接层的固定性，继而也打破了图像输入的固定性）

以上的方法1就是SPPnet的思想。

![](/images/article/spp.png)

**Step 1**：为图像建立不同尺度的图像金字塔。上图为3层。

**Step 2**：将图像金字塔中包含的feature映射到固定尺寸的向量中。上图为$$(16+4+1)\times 256$$维向量。

总结：

![](/images/article/spp_p.png)

参考：

https://zhuanlan.zhihu.com/p/24774302

SPPNet-引入空间金字塔池化改进RCNN

# Fast R-CNN



参考：

https://zhuanlan.zhihu.com/p/24780395

Fast R-CNN

http://blog.csdn.net/shenxiaolu1984/article/details/51036677

Fast RCNN算法详解

