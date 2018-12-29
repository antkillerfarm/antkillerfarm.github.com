---
layout: post
title:  深度学习（十五）——Style Transfer, 人脸检测/识别（1）
category: DL 
---

# Style Transfer

## Deep Visualization（续）

https://github.com/yosinski/deep-visualization-toolbox

这是作者Jason Yosinski提供的Deep Visualization的工具的代码。

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/deep-visualization-toolbox

原始版本是基于python 2编写的，这是我修改的python 3版本

https://github.com/Sunghyo/revacnn

这是另一个CNN可视化工具

参考：

https://zhuanlan.zhihu.com/p/24833574

Deep Visualization:可视化并理解CNN

http://www.cnblogs.com/jesse123/p/7101649.html

Tool-Deep Visualization

## DL方法

受到上述事实的启发，2015年德国University of Tuebingen的Leon A. Gatys写了如下两篇论文：

《Texture Synthesis Using Convolutional Neural Networks》

《A Neural Algorithm of Artistic Style》

代码：

https://github.com/jcjohnson/neural-style

![](/images/img2/style_transfer.png)

在第一篇论文中，Gatys使用Gramian matrix从各层CNN中提取纹理信息，于是就有了一个不用手工建模就能生成纹理的方法。

Gramian matrix（由Jørgen Pedersen Gram提出）中的元素定义如下：

$$G_{ij}=\langle v_i, v_j \rangle$$

这里的$$v_i$$表示向量，$$G_{ij}$$是向量的内积。可以看出Gramian matrix是一个半正定的对称矩阵。

在第二篇论文中，Gatys更进一步指出：**纹理能够描述一个图像的风格。**

既然第一篇论文解决了从图片B中提取纹理的任务，那么还有一个关键点就是：**如何只提取图片内容而不包括图片风格?**

## Cost Function

神经风格迁移生成图片G的cost function由两部分组成：C与G的相似程度和S与G的相似程度。

$$J(G)=\alpha \cdot J_{content}(C,G)+\beta \cdot J_{style}(S,G)$$

其中，$$\alpha, \beta$$是超参数，用来调整$$J_{content}(C,G)$$与$$J_{style}(S,G)$$的相对比重。

神经风格迁移的基本算法流程是：首先令G为随机像素点，然后使用梯度下降算法，不断修正G的所有像素点，使得J(G)不断减小，从而使G逐渐有C的内容和G的风格，如下图所示：

![](/images/img2/style_transfer_3.png)

我们先来看J(G)的第一部分$$J_{content}(C,G)$$，它表示内容图片C与生成图片G之间的相似度。

使用的CNN网络是之前预训练好的模型，例如Alex-Net。C，S，G共用相同模型和参数。首先，需要选择合适的层数l来计算$$J_{content}(C,G)$$。

如前所述，CNN的每个隐藏层分别提取原始图片的不同深度特征，由简单到复杂。如果l太小，则G与C在像素上会非常接近，没有迁移效果；如果l太深，则G上某个区域将直接会出现C中的物体。因此，l既不能太浅也不能太深，一般选择网络中间层。

若C和G在l层的激活函数输出$$a^{[l](C)}$$与$$a^{[l](G)}$$，则相应的$$J_{content}(C,G)$$的表达式为：

$$J_{content}(C,G)=\frac12||a^{[l](C)}-a^{[l](G)}||^2$$

接下来，我们定义图片的风格矩阵（style matrix）为：

$$G_{kk'}^{[l]}=\sum_{i=1}^{n_H^{[l]}}\sum_{j=1}^{n_W^{[l]}}a_{ijk}^{[l]}a_{ijk'}^{[l]}$$

风格矩阵$$G_{kk'}^{[l]}$$计算第l层隐藏层不同通道对应的所有激活函数输出和。若两个通道之间相似性高，则对应的$$G_{kk'}^{[l]}$$较大。从数学的角度来说，这里的风格矩阵实际上就是两个tensor的**互相关矩阵**。

Gram矩阵描述的是全局特征的自相关，如果输出图与风格图的这种自相关相近，那么差不多是我们所理解的”风格”。当然，其实也可以用很多其他的统计信息进行描绘风格。比如有用直方图的, 甚至还可以直接简化成”均值+方差”的。

风格矩阵$$G_{kk'}^{[l][S]}$$表征了风格图片S第l层隐藏层的“风格”。相应地，生成图片G也有$$G_{kk'}^{[l][G]}$$。那么，$$G_{kk'}^{[l][S]}$$与$$G_{kk'}^{[l][G]}$$越相近，则表示G的风格越接近S。这样，我们就可以定义出$$J^{[l]}_{style}(S,G)$$的表达式：

$$J^{[l]}_{style}(S,G)=\frac{1}{(2n_H^{[l]}n_W^{[l]}n_C^{[l]})}\sum_{k=1}^{n_C^{[l]}}\sum_{k'=1}^{n_C^{[l]}}||G_{kk'}^{[l][S]}-G_{kk'}^{[l][G]}||^2$$

为了提取的“风格”更多，也可以使用多层隐藏层，然后相加，表达式为：

$$J_{style}(S,G)=\sum_l\lambda^{[l]}\cdot J^{[l]}_{style}(S,G)$$

## 缺点

Gatys的方法虽然是里程碑式的进步，但仍然有如下缺点：

1.渲染速度缓慢。渲染一张图片需要从随机噪声开始，反复迭代若干次，才能得到最终的结果。

2.需要根据风格的不同，调整不同的超参数。换句话说，就是一个Style Transfer的模型就只能用于转换一种Style，通用型不强。

因此，之后的研究主要集中在对这两方面的改进上。针对前者的改进可称作fast style transfer，而后者可称作Universal Style Transfer。

此外，**不是所有的style transfer都是DL方法，很多新特效往往还是用传统的滤镜实现的**。比如最近比较火的“新海诚风格”。

参考：

https://blog.csdn.net/Trent1985

一个滤镜/美颜方面的blog

https://www.zhihu.com/question/29594460

新海诚风格的画面是手绘的还是Photoshop就可以达到的？后期过程是怎样的？

## Texture Networks: Feed-forward Synthesis of Textures and Stylized Images

这篇论文属于fast style transfer类的改进。它是Skolkovo Institute of Science and Technology & Yandex的Dmitry Ulyanov的作品。

Dmitry Ulyanov的个人主页：

https://dmitryulyanov.github.io

>Skolkovo位于莫斯科郊外，相当于俄国的硅谷。

代码：

https://github.com/DmitryUlyanov/texture_nets

![](/images/img2/Texture_Networks.png)



## Perceptual Losses for Real-Time Style Transfer and Super-Resolution

这篇论文是李飞飞组的Justin Johnson的作品。

Justin Johnson的个人主页：

https://cs.stanford.edu/people/jcjohns/

代码：

https://github.com/OlavHN/fast-neural-style

https://github.com/lengstrom/fast-style-transfer/

![](/images/img2/RTST.png)


参考：

https://blog.csdn.net/Hungryof/article/details/61195783

超越fast style transfer----任意风格图和内容图0.1秒出结果

https://zhuanlan.zhihu.com/p/35798776

快速风格迁移（fast-style-transfer）

## 参考

https://zhuanlan.zhihu.com/p/26746283

图像风格迁移(Neural Style)简史

https://blog.csdn.net/red_stone1/article/details/79055467

人脸识别与神经风格迁移

https://blog.csdn.net/cicibabe/article/details/70885715

卷积神经网络图像风格转移

https://blog.csdn.net/stdcoutzyx/article/details/53771471

图像风格转换(Image style transfer)

https://blog.csdn.net/u011534057/article/details/78935202

风格迁移学习笔记(1):Multimodal Transfer: A Hierarchical Deep Convolutional Neural Network for Fast

https://blog.csdn.net/u011534057/article/details/78935230

风格迁移学习笔记(2):Universal Style Transfer via Feature Transforms

https://mp.weixin.qq.com/s/l3hQCQWh5NgihzTs2A049w

风格迁移原理及tensorflow实现

https://mp.weixin.qq.com/s/5Omfj-fYRDt9j2VZH1XXkQ

如何用Keras打造出“风格迁移”的AI艺术作品

https://mp.weixin.qq.com/s/4q-9QsXD04mD-f2ke7ql8A

tensorflow风格迁移网络训练与使用

https://blog.csdn.net/hungryof/article/details/53981959

谈谈图像的Style Transfer（一）

https://blog.csdn.net/Hungryof/article/details/71512406

谈谈图像的style transfer（二）

https://mp.weixin.qq.com/s/8Fz6Q-6VgJsAko0K7HDsow

一个模型搞定所有风格转换，直接在浏览器实现（demo+代码）

https://github.com/cysmith/neural-style-tf

TensorFlow (Python API) implementation of Neural Style.这个项目实现了两张图片的画风融合，非常牛。

https://github.com/jinfagang/pytorch_style_transfer

这个和上面的一样，不过是用pytorch实现的。

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

1、按照一般的方法组织正负样本训练第一阶段的12-net和12-calibration-net网络；

2、 利用上述的1层网络在AFLW数据集上作人脸检测，在保证99%的召回率的基础上确定判别阈值T1。

3、将在AFLW上判为人脸的非人脸窗口作为负样本，将所有真实人脸作为正样本，训练第二阶段的24-net和24-calibration-net网络；

4、重复2和3，完成最后阶段的训练。

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

参考：

http://blog.csdn.net/qq_14845119/article/details/52680940

MTCNN（Multi-task convolutional neural networks）人脸对齐

http://blog.csdn.net/shuzfan/article/details/52668935

人脸检测——MTCNN

https://mp.weixin.qq.com/s/IrZEQ69RNUdcs0Fl8fHmmQ

如何应用MTCNN和FaceNet模型实现人脸检测及识别

https://mp.weixin.qq.com/s/NfqFj5iCIkbRD34Eu2Lb5g

MTCNN实时人脸检测网络详解与代码演示
