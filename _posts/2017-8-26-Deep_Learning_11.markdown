---
layout: post
title:  深度学习（十一）——SPPNet, Fast R-CNN, Faster R-CNN
category: theory 
---

# RCNN（续）

## 非极大值抑制（NMS）

RCNN会从一张图片中找出n个可能是物体的矩形框，然后为每个矩形框为做类别分类概率（如上图所示）。我们需要判别哪些矩形框是没用的。

Non-Maximum Suppression顾名思义就是抑制不是极大值的元素，搜索局部的极大值。这个局部代表的是一个邻域，邻域有两个参数可变，一是邻域的维数，二是邻域的大小。

下面举例说明NMS的做法：

假设有6个矩形框，根据分类器的类别和分类概率做排序，假设从小到大属于车辆的概率分别为A、B、C、D、E、F。

**Step 1**：从最大概率矩形框F开始，分别判断A~E与F的重叠度IOU是否大于某个设定的阈值。（**确定领域**）

**Step 2**：假设B、D与F的重叠度超过阈值，那么就扔掉B、D；并标记第一个矩形框F，是我们保留下来的。（**抑制领域内的非极大值**）

**Step 3**：从剩下的矩形框A、C、E中，选择概率最大的E，然后判断E与A、C的重叠度，重叠度大于一定的阈值，那么就扔掉；并标记E是我们保留下来的第二个矩形框。（**确定下一个领域，并抑制该领域内的非极大值**）

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

http://blog.sina.com.cn/s/blog_9db078090102whzw.html

多标签图像分类任务的评价方法-mAP

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

# SPPNet

SPPNet是何恺明2014年的作品。

论文：

《Spatial Pyramid Pooling in Deep Convolutional Networks for Visual Recognition》

在RCNN算法中，一张图片会有1~2k个候选框，每一个都要单独输入CNN做卷积等操作很费时。而且这些候选框可能很多都是重合的，重复的CNN操作从信息论的角度，也是相当冗余的。

![](/images/article/rcnn_vs_spp.png)

SPPNet的核心思想如上图所示：在feature map上提取ROI特征，这样就只需要在整幅图像上做一次卷积。

这个想法说起来简单，但落到实地，还有如下问题需要解决：

**Problem 1**：原始图像的ROI如何映射到特征图（一系列卷积层的最后输出）。

这里的计算比较复杂，要点在于：选择原始图像ROI的左上角和右下角，将之映射到feature map上的两个对应点，从而得到feature map上的ROI。

![](/images/article/roi_original_to_feature.png)

参见：

https://zhuanlan.zhihu.com/p/24780433

原始图片中的ROI如何映射到到feature map?

http://www.cnblogs.com/objectDetect/p/5947169.html

卷积神经网络物体检测之感受野大小计算

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

从上图可以看出，由于卷积策略的不同，SPPnet的流程和RCNN也有一点微小的差异：

1.RCNN是先选择区域，然后对区域进行卷积，并检测。

2.SPPnet是先统一卷积，然后应用选择区域，做区域检测。

参考：

https://zhuanlan.zhihu.com/p/24774302

SPPNet-引入空间金字塔池化改进RCNN

http://kaiminghe.com/iccv15tutorial/iccv2015_tutorial_convolutional_feature_maps_kaiminghe.pdf

何恺明：Convolutional Feature Maps

# Fast R-CNN

Fast R-CNN是Ross Girshick于2015年祭出的又一大招。

论文：

《Fast R-CNN》

代码：

https://github.com/rbgirshick/fast-rcnn

![](/images/article/fast_rcnn_p_2.png)

上图是Fast R-CNN的结构图。从该图可以看出Fast R-CNN和SPPnet的主要差异在于：

1.使用ROI（Region of interest） Pooling，替换SPP。

2.去掉了SVM分类。

以下将对这两个方面，做一个简述。

## ROI Pooling

SPP将图像pooling成多个固定尺度，而RoI只将图像pooling到单个固定的尺度。（虽然多尺度学习能提高一点点mAP，不过计算量成倍的增加。）

普通pooling操作中，pooling区域的大小一旦选定，就不再变化。

而ROI Pooling中，为了将ROI区域pooling到单个固定的目标尺度，我们需要根据ROI区域和目标尺度的大小，动态计算pooling区域的大小。

## Bounding-box Regression

从Fast R-CNN的结构图可以看出，与一般的CNN不同，它在FC之后，实际上有两个输出层：第一个是针对每个ROI区域的分类概率预测（上图中的Linear+softmax），第二个则是针对每个ROI区域坐标的偏移优化（上图中的Linear）。

然后，这两个输出层（即两个Task）再合并为一个multi-task，并定义统一的loss。

![](/images/article/fast_rcnn_multi_task.png)

由于两个Task的信息互为补充，使得分类预测任务的softmax准确率大为提升，SVM也就没有存在的必要了。

## 全连接层提速

Fast R-CNN的论文中还提到了全连接层提速的概念。这个概念本身和Fast R-CNN倒没有多大关系。因此，完全可以将之推广到其他场合。

![](/images/article/fc_svd.png)

它的主要思路是，在两个大的FC层（假设尺寸为u、v）之间，利用SVD算法加入一个小的FC层（假设尺寸为t），从而减少了计算量。

$$u*v\to u*t+t*v$$

## 总结

![](/images/article/fast_rcnn_p.png)

参考：

https://zhuanlan.zhihu.com/p/24780395

Fast R-CNN

http://blog.csdn.net/shenxiaolu1984/article/details/51036677

Fast RCNN算法详解

# Faster R-CNN

Faster-RCNN是任少卿2016年在MSRA提出的新算法。Ross Girshick和何恺明也是论文的作者之一。

>注：任少卿，中科大本科（2011年）+博士（2016年）。Momenta联合创始人+技术总监。   
>个人主页：   
>http://shaoqingren.com/

论文：

《Faster R-CNN: Towards Real-Time Object Detection with Region Proposal Networks》

![](/images/article/faster_rcnn_p_2.png)

上图是Faster R-CNN的结构图。

Fast R-CNN尽管已经很优秀了，然而还有一个最大的问题在于：proposal阶段没有整合到CNN中。

这个问题带来了两个不良影响：

1.非end-to-end模型导致程序流程比较复杂。

2.随着后续CNN步骤的简化，生成2k个候选bbox的Selective Search算法成为了整个计算过程的性能瓶颈。（无法利用GPU）

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

