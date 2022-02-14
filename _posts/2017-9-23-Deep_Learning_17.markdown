---
layout: post
title:  深度学习（十七）——人脸检测/识别（1）, 自动求导, 无监督/半监督/自监督深度学习（1）
category: DL 
---

* toc
{:toc}

# Style Transfer（续）

https://mp.weixin.qq.com/s/9AEYcY04lAl3dCRK9LPBeQ

人脸风格化核心技术与数据集总结

https://mp.weixin.qq.com/s/ylfeWiOrftCB823_gjQNmA

图像风格迁移也有框架了：使用Python编写，与PyTorch完美兼容，外行也能用

# 人脸检测/识别

## Cascade CNN

论文：

《A Convolutional Neural Network Cascade for Face Detection》

![](/images/img2/CascadeCNN.jpg)

这篇可以说是对经典的Viola-Jones方法的深度卷积网络实现，可以明显看出是3阶级联（12-net、24-net、48-net）。

前2阶的网络都非常简单，只有第3阶才比较复杂。这不是重点，重点是我们要从上图中学习多尺度特征组合。

以第2阶段的24-net为例，首先把上一阶段剩下的窗口resize为24*24大小，然后送入网络，得到全连接层的特征。同时，将之前12-net的全连接层特征取出与之拼接在一起。最后对组合后的特征进行softmax分类。

除了分类网络之外，Cascade CNN还包含了3个修正bounding box的CNN网络，分别叫做12-calibration-net，24-calibration-net和48-calibration-net。他们的结构与12-net等类似。

网络结构方面也就这样了，该论文最牛之处在于给出了这类级联网络的训练方法。

![](/images/img2/CascadeCNN_2.jpg)

1.按照一般的方法组织正负样本训练第一阶段的12-net和12-calibration-net网络；

2.利用上述的1层网络在AFLW数据集上作人脸检测，在保证99%的召回率的基础上确定判别阈值T1。

3.将在AFLW上判为人脸的非人脸窗口作为负样本，将所有真实人脸作为正样本，训练第二阶段的24-net和24-calibration-net网络；

4.重复2和3，完成最后阶段的训练。

参考：

http://blog.csdn.net/shuzfan/article/details/50358809

人脸检测——CascadeCNN

## MTCNN

论文：

《Joint Face Detection and Alignment using Multi-task Cascaded Convolutional Networks》

![](/images/img2/MTCNN.png)

上面是该方法的流程图，可以看出也是三阶级联，和CascadeCNN很像。

stage1: 在构建图像金字塔的基础上，利用fully convolutional network来进行检测，同时利用boundingbox regression和NMS来进行修正。

stage2: 将通过stage1的所有窗口输入作进一步判断，同时也要做boundingbox regression和NMS。

stage3: 和stage2相似，只不过增加了更强的约束：5个人脸关键点（landmark）。

![](/images/img3/P-Net.jpg)

![](/images/img3/R-Net.jpg)

![](/images/img3/O-Net.jpg)

上面三图分别是P-Net、R-Net和O-Net的网络结构图。

需要注意的是，Cascade CNN和MTCNN都是比较早期的方案了，这里的人脸候选框，一般是用**滑动窗口**的方式生成的，这种方法的效率不高，不仅比不上Faster RCNN以后的RPN Layer，就连RCNN的Selective Search也颇有不如，完全就是Viola-Jones方法的简单翻版。

参考：

http://blog.csdn.net/qq_14845119/article/details/52680940

MTCNN（Multi-task convolutional neural networks）人脸对齐

http://blog.csdn.net/shuzfan/article/details/52668935

人脸检测——MTCNN

https://mp.weixin.qq.com/s/IrZEQ69RNUdcs0Fl8fHmmQ

如何应用MTCNN和FaceNet模型实现人脸检测及识别

https://mp.weixin.qq.com/s/NfqFj5iCIkbRD34Eu2Lb5g

MTCNN实时人脸检测网络详解与代码演示

## 人脸识别

人脸检测是从一张图片中，识别出人脸，这和通常的目标检测没有太大的差别。**而人脸识别，则是精确到具体的人。**

人脸识别通常的做法是：

1.使用人脸检测，得到人脸区域的图像。

2.提取人脸特征。一般采用CNN+FC+loss的结构。其中，CNN+FC用于提取特征，而loss仅用于训练阶段。在推理阶段，我们使用CNN+FC得到人脸的特征向量即可。

3.特征的对比。比较两个特征向量的相似度（可以使用LMS或者cos相似度）。超过阈值，即认为是同一张脸。

## 生物特征识别

生物特征模态：人脸、虹膜、指纹、掌纹、静脉、声纹、步态、行人重识别。

https://mp.weixin.qq.com/s/t80KjO54AQyNSU1UgRJ5og

生物特征识别学科发展报告

# 自动求导

DL发展到现在，其基本运算单元早就不止CNN、RNN之类的简单模块了。针对新运算层出不穷的现状，各大DL框架基本都实现了自动求导的功能。

论文：

《Automatic Differentiation in Machine Learning: a Survey》

## Manual differentiation

手动推出导数是什么样，然后硬编码。这种做法既耗时也容易出错，还没有灵活性。

## Numerical differentiation

数值微分最大的特点就是很直观，好计算，它直接利用了导数定义：

$$f'(x)=\lim_{h\to 0}{f(x+h)-f(x)\over h}$$

不过这里有一个很大的问题：h怎么选择？选大了，误差会很大；选小了，不小心就陷进了浮点数的精度极限里，造成舍入误差。

第二个问题是对于参数比较多时，对深度学习模型来说，上面的计算是不够高效的，因为每计算一个参数的导数，你都需要重新计算$$f(x+h)$$。

因此，这种方法并不常用，而主要用于做梯度检查（Gradient check），你可以用这种不高效但简单的方法去检查其他方法得到的梯度是否正确。

## Symbolic differentiation

符号微分的主要步骤如下：

1.需要预置基本运算单元的求导公式。

2.遍历计算图，得到运算表达式。

3.根据导数的代入法则和四则运算法则，求出复杂运算的求导公式。

这种方法没有误差，是目前的主流，但遍历比较费时间。

## Automatic differentiation

除此之外，常用的自动求导技术，还有Automatic differentiation。（请注意这里的AD是一个很狭义的概念。）

类比复数的概念：

$$x = a + bi \quad (i^2 = -1)$$

我们定义Dual number：

$$x \mapsto x = x + \dot{x} d \quad (d^2=0)$$

定义Dual number的运算法则：

$$(x + \dot{x}d) + ( y + \dot{y}d) = x + y + (\dot{x} + \dot{y})d$$

$$(x + \dot{x}d) ( y + \dot{y}d) = xy + \dot{x}yd + x\dot{y}d  +  \dot{x}\dot{y}d^2 = xy + (\dot{x}y+ x\dot{y})d$$

$$-(x + \dot{x}d) = - x - \dot{x}d$$

$$\frac{1}{x + \dot{x}d} = \frac{1}{x} - \frac{\dot{x}}{x^2}d$$

dual number有很多非常不错的性质。以下面的指数运算多项式为例：

$$f(x) = p_0 + p_1x + p_2x^2 + ... + p_nx^n$$

用$$x + \dot{x}d$$替换x，则有：

$$
\begin{array}\\
f(x + \dot{x}d) =   p_0 + p_1(x + \dot{x}d) + ... +  p_n(x + \dot{x}d)^n \\ 
= p_0 + p_1x + p_2x^2 + ... + p_nx^n + \\ 
p_1\dot{x}d + 2p_2x\dot{x}d + ... + np_{n-1}x\dot{x}d\\ 
= f(x) + f'(x)\dot{x}d
\end{array}$$

可以看出d的系数就是$$f'(x)$$。

## 不可导函数的求导

不可导函数的求导，一般采用泰勒展开的方式。典型的算法有PGD（Proximal Gradient Descent）。

参考：

https://blog.csdn.net/bingecuilab/article/details/50628634

Proximal Gradient Descent for L1 Regularization

## 动态图

针对动态图的自动求导，TF提出了GradientTape方法。

https://zhuanlan.zhihu.com/p/102207302

tensorflow计算图与自动求导——tf.GradientTape

## 其他

Jacobian-vector product function,JVP

vector-Jacobian product function,VJP

Hessian Vector Product,HVP

https://blog.csdn.net/apache/article/details/113925886

Pytorch,Tensorflow Autograd/AutoDiff nutshells: Jacobian,Gradient,Hessian,JVP,VJP,etc

## 参考

https://mp.weixin.qq.com/s/7Z2tDhSle-9MOslYEUpq6g

从概念到实践，我们该如何构建自动微分库

https://mp.weixin.qq.com/s/bigKoR3IX_Jvo-re9UjqUA

机器学习之——自动求导

https://www.jianshu.com/p/4c2032c685dc

自动求导框架综述

http://txshi-mt.com/2018/10/04/NMT-Tutorial-3b-Autodiff/

自动微分

https://mp.weixin.qq.com/s/WiZ00mkEB7CND3VyIM5Swg

最新《自动微分手册》77页pdf

https://mp.weixin.qq.com/s/xXwbV46-kTobAMRwfKyk_w

自动求导--Deep Learning框架必备技术二三事

https://mp.weixin.qq.com/s/f0xFfA1inOVOdJnSZR4k6Q

自动微分技术

https://zhuanlan.zhihu.com/p/79801410

PyTorch的自动求导机制详细解析，PyTorch的核心魔法

https://zhuanlan.zhihu.com/p/29904755

Autograd:PyTorch中的梯度计算

https://zhuanlan.zhihu.com/p/69294347

PyTorch的Autograd

https://zhuanlan.zhihu.com/p/83172023

Pytorch autograd,backward详解

https://mp.weixin.qq.com/s/PELBuCvu-7KQ33XBtlYfYQ

深度学习中的微分

https://zhuanlan.zhihu.com/p/24709748

矩阵求导术（上）

https://zhuanlan.zhihu.com/p/24863977

矩阵求导术（下）

https://mp.weixin.qq.com/s/2hu6a0wScJedwk3a5aKbIw

自动微分到底是什么？这里有一份自我简述

https://zhuanlan.zhihu.com/p/347385418

AI框架基础技术之自动求导机制 (Autograd)

# 无监督/半监督/自监督深度学习

自监督学习是一种特殊目的的无监督学习。不同于传统的AutoEncoder等方法，仅仅以重构输入为目的，而是希望通过surrogate task学习到和高层语义信息相关联的特征。

## 对比学习

![](/images/img4/SimCLR.jpg)

https://mp.weixin.qq.com/s/r1uXn2jGsHZcZ8Nk7GnGFA

语义表征的无监督对比学习：一个新理论框架

https://zhuanlan.zhihu.com/p/346686467

对比学习（Contrastive Learning）综述

https://mp.weixin.qq.com/s/SOaA9XNnymLgGgJ5JNSdBg

对比学习（Contrastive Learning）相关进展梳理

https://mp.weixin.qq.com/s/U0pTQkW55evm94iQORwGeA

图解SimCLR框架，用对比学习得到一个好的视觉预训练模型

https://mp.weixin.qq.com/s/1RJ4bbfDC5LiN2PNIxdzew

SimCLR框架的理解和代码实现以及代码讲解

https://mp.weixin.qq.com/s/-Vtl_8nND7WCPLdL5bNlMw

探索孪生神经网络：请停止你的梯度传递

https://zhuanlan.zhihu.com/p/321642265

《探索简单孪生网络表示学习》阅读笔记

https://mp.weixin.qq.com/s/6qqFAQBaOFuXtaeRSmQgsQ

一文梳理2020年大热的对比学习模型

https://mp.weixin.qq.com/s/SeAZERYdfqDbtqTLnuWfGg

盘点近期大热对比学习模型：MoCo/SimCLR/BYOL/SimSiam

https://mp.weixin.qq.com/s/jHVg-BMRRVNjAf6ZFEoPxQ

自监督学习的SimCLRv2框架

https://mp.weixin.qq.com/s/7iBC_n6EARW3V8bNuKUqQA

Hinton团队力作：SimCLR系列

https://mp.weixin.qq.com/s/sH-G4g0EyQLu2l91Xvdefw

Neighbor2Neighbor：无需干净图像的自监督图像降噪

https://mp.weixin.qq.com/s/xYlCAUIue_z14Or4oyaCCg

对比学习研究进展精要
