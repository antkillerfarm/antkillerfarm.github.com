---
layout: post
title:  深度学习（十一）——Batch Normalization, Softmax详解, 目标检测
category: DL 
---

# 花式池化（续）

## UnPooling

UnPooling是一种常见的上采样操作。其过程如下图所示：

![](/images/article/unpool.png)

1.在Pooling（一般是Max Pooling）时，保存最大值的位置（Max Location）。

2.中间经历若干网络层的运算。

3.上采样阶段，利用第1步保存的Max Location，重建下一层的feature map。

从上面的描述可以看出，UnPooling不完全是Pooling的逆运算：

1.Pooling之后的feature map，要经过若干运算，才会进行UnPooling操作。

2.对于非Max Location的地方以零填充。然而这样并不能完全还原信息。

参考：

http://blog.csdn.net/u012938704/article/details/52831532

caffe反卷积

## 参考

http://www.cnblogs.com/tornadomeet/p/3432093.html

Stochastic Pooling简单理解

http://mp.weixin.qq.com/s/XzOri12hwyOCdI1TgGQV3w

新型池化层sort_pool2d实现更快更好的收敛：表现优于最大池化层

# Batch Normalization

在《深度学习（二）》中，我们已经简单的介绍了Batch Normalization的基本概念。这里主要讲述一下它的实现细节。

我们知道在神经网络训练开始前，都要对输入数据做一个归一化处理，那么具体为什么需要归一化呢？归一化后有什么好处呢？

原因在于神经网络学习过程本质就是为了学习数据分布，一旦训练数据与测试数据的分布不同，那么网络的泛化能力也大大降低；另外一方面，一旦每批训练数据的分布各不相同(batch梯度下降)，那么网络就要在每次迭代都去学习适应不同的分布，这样将会大大降低网络的训练速度，这也正是为什么我们需要对数据都要做一个归一化预处理的原因。

对输入数据归一化，早就是一种基本操作了。然而这样只对神经网络的输入层有效。更好的办法是对每一层都进行归一化。

然而简单的归一化，会破坏神经网络的特征。（归一化是线性操作，但神经网络本身是非线性的，不具备线性不变性。）因此，如何归一化，实际上是个很有技巧的事情。

首先，我们回顾一下归一化的一般做法：

$$\hat x^{(k)} = \frac{x^{(k)} - E[x^{(k)}]}{\sqrt{Var[x^{(k)}]}}$$

其中，$$x = (x^{(0)},x^{(1)},…x^{(d)})$$表示d维的输入向量。

接着，定义归一化变换函数：

$$y^{(k)}=\gamma^{(k)}\hat x^{(k)}+\beta^{(k)}$$

这里的$$\gamma^{(k)},\beta^{(k)}$$是待学习的参数。

BN的主要思想是用同一batch的样本分布来近似整体的样本分布。显然，batch size越大，这种近似也就越准确。

用$$\mathcal{B}=\{x_{1,\dots,m}\}$$表示batch，则BN的计算过程如下：

**Step 1**.计算mini-batch mean。

$$\mu_\mathcal{B}\leftarrow \frac{1}{m}\sum_{i=1}^mx_i$$

**Step 2**.计算mini-batch variance。

$$\sigma_\mathcal{B}^2\leftarrow \frac{1}{m}\sum_{i=1}^m(x_i-\mu_\mathcal{B})^2$$

**Step 3**.normalize。

$$\hat x_i\leftarrow \frac{x_i-\mu_\mathcal{B}}{\sqrt{\sigma_\mathcal{B}^2+\epsilon}}$$

这里的$$\epsilon$$是为了数值的稳定性而添加的常数。

**Step 4**.scale and shift。

$$y_i=\gamma\hat x_i+\beta\equiv BN_{\gamma,\beta}(x_i)$$

在实际使用中，BN计算和卷积计算一样，都被当作神经网络的其中一层。即：

$$z=g(Wu+b)\rightarrow z=g(BN(Wu+b))=g(BN(Wu))$$

从另一个角度来看，BN的均值、方差操作，相当于去除一阶和二阶信息，而只保留网络的高阶信息，即非线性部分。因此，上式最后一步中b被忽略，也就不难理解了。

BN的误差反向算法相对复杂，这里不再赘述。

在inference阶段，BN网络忽略Step 1和Step 2，只计算后两步。其中,$$\beta,\gamma$$由之前的训练得到。$$\mu,\sigma$$原则上要求使用全体样本的均值和方差，但样本量过大的情况下，也可使用训练时的若干个mini batch的均值和方差的FIR滤波值。

# Instance Normalization

Instance Normalization主要用于CV领域。

论文：

《Instance Normalization: The Missing Ingredient for Fast Stylization》

首先我们列出对图片Batch Normalization的公式：

$$y_{tijk}=\frac{x_{tijk}-\mu_i}{\sqrt{\sigma_i^2+\epsilon}}, \mu_i=\frac{1}{HWT}\sum_{t=1}^T \sum_{l=1}^W \sum_{m=1}^Hx_{tilm}, \sigma_i^2=\frac{1}{HWT}\sum_{t=1}^T \sum_{l=1}^W \sum_{m=1}^H(x_{tilm}-m\mu_i)^2$$

其中，T为图片数量，i为通道，j、k为图片的宽、高。

Instance Normalization的公式：

$$y_{tijk}=\frac{x_{tijk}-\mu_{ti}}{\sqrt{\sigma_{ti}^2+\epsilon}}, \mu_{ti}=\frac{1}{HW} \sum_{l=1}^W \sum_{m=1}^Hx_{tilm}, \sigma_{ti}^2=\frac{1}{HW} \sum_{l=1}^W \sum_{m=1}^H(x_{tilm}-m\mu_{ti})^2$$

从中可以看出Instance Normalization实际上就是对一张图片的一个通道内的值进行归一化，因此又叫做对比度归一化（contrast normalization）。

参考：

http://www.jianshu.com/p/d77b6273b990

论文中文版

# Softmax详解

首先给出Softmax function的定义:

$$y_c=\zeta(\textbf{z})_c = \dfrac{e^{z_c}}{\sum_{d=1}^C{e^{z_d}}} \text{  for } c=1, \dots, C$$

从中可以很容易的发现，如果$$z_c$$的值过大，朴素的直接计算会上溢出或下溢出。

解决办法：

$$z_c\leftarrow z_c-a,a=\max\{z_1,\dots,z_C\}$$

证明：

$$\zeta(\textbf{z-a})_c = \dfrac{e^{z_c}\cdot e^{-a}}{\sum_{d=1}^C{e^{z_d}\cdot e^{-a}}} = \dfrac{e^{z_c}}{\sum_{d=1}^C{e^{z_d}}} = \zeta(\textbf{z})_c$$

Softmax的损失函数是cross entropy loss function：

$$\xi(X, Y) = \sum_{i=1}^n \xi(\textbf{t}_i, \textbf{y}_i) = - \sum_{i=1}^n \sum_{i=c}^C t_{ic} \cdot \log(y_{ic})$$

Softmax的反向传播算法：

$$\begin{align}
\dfrac{\partial\xi}{\partial z_i} &= - \sum_{j=1}^C \dfrac{\partial t_j \log(y_j)}{\partial z_i} \\
&= - \sum_{j=1}^C t_j \dfrac{\partial \log(y_j)}{\partial z_i} \\
&= - \sum_{j=1}^C t_j \dfrac{1}{y_j} \dfrac{\partial y_j}{\partial z_i} \\
&= - \dfrac{t_i}{y_i} \dfrac{\partial y_i}{\partial z_i} - \sum_{j \neq i}^C \dfrac{t_j}{y_j} \dfrac{\partial y_j}{\partial z_i} \\
&= - \dfrac{t_i}{y_i} y_i(1-y_i) - \sum_{j \neq i}^{C} \dfrac{t_j}{y_j}(-y_jy_j) \\
&= -t_i + t_iy_i + \sum_{j \neq i}^{C} t_jy_i \\
&= -t_i + \sum_{j=1}^C t_jy_i \\
&= -t_i + y_i \sum_{j=1}^C t_j \\
&= y_i - t_i
\end{align}$$

参考：

https://mp.weixin.qq.com/s/2xYgaeLlmmUfxiHCbCa8dQ

softmax函数计算时候为什么要减去一个最大值？

http://shuokay.com/2016/07/20/softmax-loss/

Softmax输出及其反向传播推导

# 目标检测

## 概述

object detection是计算机视觉的一个重要的分支。类似的分支还有目标分割、目标识别和目标跟踪。

以下摘录自Sensetime CTO曹旭东的解读：

传统方法使用滑动窗口的框架，把一张图分解成几百万个不同位置不同尺度的子窗口，针对每一个窗口使用分类器判断是否包含目标物体。传统方法针对不同的类别的物体，一般会设计不同的特征和分类算法，比如人脸检测的经典算法是**Harr特征+Adaboosting分类器**；行人检测的经典算法是**HOG(histogram of gradients)+Support Vector Machine**；一般性物体的检测的话是**HOG特征+DPM(deformable part model)的算法**。

基于深度学习的物体检测的经典算法是RCNN系列：RCNN，fast RCNN(Ross Girshick)，faster RCNN(少卿、凯明、孙剑、Ross)。这三个工作的核心思想是分别是：使用更好的CNN模型判断候选区域的类别；复用预计算的sharing feature map加快模型训练和物体检测的速度；进一步使用sharing feature map大幅提高计算候选区域的速度。其实基于深度学习的物体检测也可以看成对海量滑动窗口分类，只是用全卷积的方式。

RCNN系列算法还是将物体检测分为两个步骤。现在还有一些工作是端到端(end-to-end)的物体检测，比如说YOLO(You Only Look Once: Unified, Real-Time Object Detection)和SSD(SSD: Single Shot MultiBox Detector)这样的算法。这两个算法号称和faster RCNN精度相似但速度更快。物体检测正负样本极端非均衡，two-stage cascade可以更好的应对非均衡。端到端学习是否可以超越faster RCNN还需要更多研究实验。

## 进化史

DPM(2007)->RCNN(2014)->Fast RCNN->Faster RCNN

![](/images/article/rcnn_2.png)

![](/images/article/rcnn_4.jpg)

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



