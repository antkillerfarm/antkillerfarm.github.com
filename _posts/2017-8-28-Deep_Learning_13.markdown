---
layout: post
title:  深度学习（十三）——花式池化, Batch Normalization
category: DL 
---

# Winograd（续）

## 参考

https://colfaxresearch.com/falcon-library/

FALCON Library: Fast Image Convolution in Neural Networks on Intel Architecture

https://www.intelnervana.com/winograd/

"Not so fast, FFT": Winograd

https://www.encyclopediaofmath.org/index.php/Winograd_small_convolution_algorithm

Winograd small convolution algorithm

https://mp.weixin.qq.com/s/VcGwT0rKYQr13MQ_PRVmyg

每个AI程序员都应该知道的基础数论

# Machine Learning之Python篇

## 参考

https://mp.weixin.qq.com/s?__biz=MzIxNjA2ODUzNg==&mid=2651435914&idx=1&sn=b0b6f1cda08798dc35bf0930dc9d9096

盘膝而坐--跟你聊聊APP市场的数据分析

https://mp.weixin.qq.com/s/AVSFbfikczjm7flsQQPC1g

2017年最受数据科学欢迎的Top15个Python库

https://mp.weixin.qq.com/s/jBULfMR0_7Ms9nc5CLLIFQ

Python的开源人脸识别库：离线识别率高达99.38%

https://zhuanlan.zhihu.com/p/30630608

Python和MATLAB交互的基本操作

https://mp.weixin.qq.com/s/drpd1O502TcL54cHSEdl_g

如何为时间序列数据优化K-均值聚类速度？

https://mp.weixin.qq.com/s/Di4xVdD1jOGGCiIB-vdkkg

Python可以被用来做哪些神奇好玩的事情

https://mp.weixin.qq.com/s/-SHD9rXUGQ-yp21RtFhJpg

手把手教你 Python挖掘用户评论典型意见并自动生产报告

https://mp.weixin.qq.com/s/OxNj9fWaEMh8SuQiK52HWg

sklearn中PCA库讲解与实战

https://mp.weixin.qq.com/s/ZbT5ASvnTODkodTJjBGhnw

Python环境下的8种简单线性回归算法

# 大数据平台参考资源

https://mp.weixin.qq.com/s/O8pj4pChyw8PK-G_4A3DWg

发布订阅消息系统Apache Pulsar简介

https://mp.weixin.qq.com/s/ToJJTBWJXlpj3LkDj5YYvw

为什么要选择Apache Pulsar

https://mp.weixin.qq.com/s/crluKkEdvfZlHyF_gQm1ZA

漫谈推荐系统及数据库技术

https://mp.weixin.qq.com/s/CwUW-Ntb4qphrqha24P-Og

漫谈推荐系统及数据库技术（二）——分布式数据库技术

https://mp.weixin.qq.com/s/3oQ2yYGNuEz1cuEo4xLZzA

如何解决大规模机器学习的三大痛点？

http://www.jianshu.com/p/7a720bd7d6b5

如何架构一个数据工程

https://mp.weixin.qq.com/s/znr_oRlG_Y30K1XUwupwpg

技术如何秒懂你？阿里百万级QPS资源调度系统揭秘

https://mp.weixin.qq.com/s/1pXMCcO6NR1SMBzsyES-cw

分布式数据库又支持关系数据模型了？

https://mp.weixin.qq.com/s/jMWuMuIvI1cFThC-WQGbHQ

大家一直在谈的领域驱动设计（DDD），我们在互联网业务系统是这么实践的

https://mp.weixin.qq.com/s/WzfzTIPAYlQjkHwmNjiInA

Shield：支撑美团点评品类最丰富业务的移动端模块化框架开源了

https://mp.weixin.qq.com/s/Zockp0wCfL6t-LckaLCQlg

如何实现金服业务流程动态化

https://mp.weixin.qq.com/s/HhabrgpbWSn_f2Q-QfgaAA

智能投放系统之场景分析最佳实践

# 花式池化

池化和卷积一样，都是信号采样的一种方式。

## 普通池化

池化的一般步骤是：选择区域P，令$$Y=f(P)$$。这里的f为池化函数。

![](/images/article/max_pooling.png)

上图是Max Pooling的示意图。除了max之外，常用的池化函数还有mean、min等。

ICLR2013上，Zeiler提出了另一种pooling手段stochastic pooling。只需对Pooling区域中的元素按照其概率值大小随机选择，即元素值大的被选中的概率也大。而不像max-pooling那样，永远只取那个最大值元素。

根据相关理论，特征提取的误差主要来自两个方面：

（1）邻域大小受限造成的估计值方差增大；

（2）卷积层参数误差造成估计均值的偏移。

一般来说，mean-pooling能减小第一种误差，更多的保留图像的背景信息，max-pooling能减小第二种误差，更多的保留纹理信息。

Stochastic-pooling则介于两者之间，通过对像素点按照数值大小赋予概率，再按照概率进行亚采样，在平均意义上，与mean-pooling近似，在局部意义上，则服从max-pooling的准则。

## 池化的反向传播

池化的反向传播比较简单。以上图的Max Pooling为例，由于取的是最大值7,因此，误差只要传递给7所在的神经元即可。

这里再次强调一下，池化只是对信号的下采样。对于图像来说，这种下采样保留了图像的某些特征，因而是有意义的。但对于另外的任务则未必如此。

比如，AlphaGo采用CNN识别棋局，但对棋局来说，下采样显然是没有什么物理意义的，因此，**AlphaGo的CNN是没有Pooling的**。

## 全局平均池化

Global Average Pooling是另一类池化操作，一般用于替换FullConnection层。

![](/images/article/global_average_pooling.png)

上图是FC和GAP在CNN中的使用方法图。从中可以看出Conv转换成FC，实际上进行了如下操作：

1.对每个通道的feature map进行flatten操作得到一维的tensor。

2.将不同通道的tensor连接成一个大的一维tensor。

![](/images/article/FC.png)

上图展示了FC与Conv、Softmax等层联动时的运算操作。

![](/images/article/GAP.png)

上图是GAP与Conv、Softmax等层联动时的运算操作。可以看出，GAP的实际操作如下：

1.计算每个通道的feature map的均值。

2.将不同通道的均值连接成一个一维tensor。

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

## K-max Pooling

![](/images/article/kmax_pooling.png)

## 参考

http://www.cnblogs.com/tornadomeet/p/3432093.html

Stochastic Pooling简单理解

http://mp.weixin.qq.com/s/XzOri12hwyOCdI1TgGQV3w

新型池化层sort_pool2d实现更快更好的收敛：表现优于最大池化层

http://blog.csdn.net/liuchonge/article/details/67638232

CNN与句子分类之动态池化方法DCNN--模型介绍篇

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


