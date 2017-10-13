---
layout: post
title:  深度学习（九）——李飞飞, 花式卷积, 花式池化
category: theory 
---

# 李飞飞

## AI大佬

李飞飞是吴恩达之后的华裔AI新大佬。巧合的是，他们都是斯坦福AP+AI lab的主任，只不过吴是李的前任而已。

**李飞飞（Fei-Fei Li）**，1976年生，成都人，16岁移民美国。普林斯顿大学本科（1995～1999）+加州理工学院博士（2001～2005）。先后执教于UIUC、普林斯顿、斯坦福等学校。

个人主页：

http://vision.stanford.edu/feifeili/

## 大佬的门徒

比如可爱的妹子**Serena Yeung**。这个妹子是斯坦福的本硕博。出身不详，但从姓名的英文拼法来看，应该是美国土生的华裔。Yeung是杨、阳、羊等姓的传统英文拼法，但显然不是大陆推行的拼音拼法。（可以对比的是Fei-Fei Li和Bruce Lee，对于同一个姓的不同拼法。）

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

# 花式卷积

在DL中，卷积实际上是一大类计算的总称。除了原始的卷积、反卷积（Deconvolution）之外，还有各种各样的花式卷积。

## 卷积在CNN和数学领域中的概念差异

首先需要明确一点，CNN中的卷积和反卷积，实际上和数学意义上的卷积、反卷积是有差异的。

数学上的卷积主要用于傅立叶变换，在计算中有个时域上的反向操作，并不是简单的向量内积运算。在信号处理领域，卷积主要用作**信号的采样**。

数学上的反卷积主要作为卷积的逆运算，相当于**信号的重建**，或者解微分方程。因此，它的难度远远大于卷积运算，常见的有Wiener deconvolution、Richardson–Lucy deconvolution等。

CNN的反卷积就简单多了，它只是误差的反向算法而已。因此，也有人用back convolution, transpose convolution这样更精确的说法，来描述CNN的误差反向算法。

## Dilated convolution

![](/images/article/dilation.gif)

上图是Dilated convolution的操作。又叫做多孔卷积(atrous convolution)。

![](/images/article/no_padding_strides_transposed.gif)

上图是transpose convolution的操作。它和Dilated convolution的差别在于，前者是图像上有洞，而后者是kernel上有洞。

和池化相比，Dilated convolution实际上也是一种下采样，只不过采样的位置是固定的，因而能够更好的保持空间结构信息。

## 分组卷积

![](/images/article/AlexNet.png)

分组卷积最早在AlexNet中出现，由于当时的硬件资源有限，训练AlexNet时卷积操作不能全部放在同一个GPU处理，因此作者把feature maps分给2个GPU分别进行处理，最后把2个GPU的结果进行融合。

在AlexNet的Group Convolution当中，特征的通道被平均分到不同组里面，最后再通过两个全连接层来融合特征，这样一来，就只能在最后时刻才融合不同组之间的特征，对模型的泛化性是相当不利的。

为了解决这个问题，ShuffleNet在每一次层叠这种Group conv层前，都进行一次channel shuffle，shuffle过的通道被分配到不同组当中。进行完一次group conv之后，再一次channel shuffle，然后分到下一层组卷积当中，以此循环。

![](/images/article/ShuffleNet.jpg)

论文：

《ShuffleNet: An Extremely Efficient Convolutional Neural Network for Mobile Devices》

无论是在Inception、DenseNet或者ShuffleNet里面，我们对所有通道产生的特征都是不分权重直接结合的，那为什么要认为所有通道的特征对模型的作用就是相等的呢？这是一个好问题，于是，ImageNet2017冠军SEnet就出来了。

论文：

《Squeeze-and-Excitation Networks》

## Separable convolution

前面介绍的都是正方形的卷积核，实际上长条形的卷积核也是很常用的。比如可分离卷积。

我们知道卷积的计算量和卷积核的面积成正比。对于k x k的卷积核K来说，计算复杂度就是$$O(k^2)$$。

如果我们能找到1 x k的卷积核H和k x 1的卷积核V，且$$K = V * H$$，则称K是可分离的卷积核。

根据卷积运算满足结合律，可得：

$$f * K = f * (V * H) = f * V * H$$

这样就将一个k x k的卷积运算，转换成1 x k + k x 1的卷积运算，从而大大节省了参数和计算量。

显然，不是所有的卷积核都满足可分离条件。但是不要紧，NN有自动学习并逼近函数的能力。经过训练之后：$$K \approx V * H$$

## 可变形卷积核

MSRA于2017年提出了可变形卷积核的概念。

论文：

《Deformable Convolutional Networks》

![](/images/article/Deformable_convolution.png)

## 1 x 1卷积

1、升维或降维。

如果卷积的输出输入都只是一个平面，那么1x1卷积核并没有什么意义，它是完全不考虑像素与周边其他像素关系。 但卷积的输出输入是长方体，所以1 x 1卷积实际上是对每个像素点，在不同的channels上进行线性组合（信息整合），且保留了图片的原有平面结构，调控depth，从而完成升维或降维的功能。

![](/images/article/conv_1x1.png)

2、加入非线性。卷积层之后经过激励层，1 x 1的卷积在前一层的学习表示上添加了非线性激励（ non-linear activation ），提升网络的表达能力；

3.促进不同通道之间的信息交换。

参考：

https://www.zhihu.com/question/56024942

卷积神经网络中用1x1卷积有什么作用或者好处呢？

## depthwise separable convolution

在传统的卷积网络中，卷积层会同时寻找跨空间和跨深度的相关性。

然而Xception指出：跨通道的相关性和空间相关性是完全可分离的，最好不要联合映射它们。

>Xception是Francois Chollet于2016年提出的。

![](/images/article/Xception.png)

上图是Xception中的卷积运算depthwise separable convolution的示意图。

它包含一个深度方面的卷积（一个为每个通道单独执行的空间卷积），后面跟着一个逐点的卷积（一个跨通道的 1×1 卷积）。我们可以将其看作是首先求跨一个 2D 空间的相关性，然后再求跨一个 1D 空间的相关性。可以看出，这种 2D+1D 映射学起来比全 3D 映射更加简单。

在ImageNet数据集上，Xception的表现稍稍优于Inception v3，而且在一个有17000类的更大规模的图像分类数据集上的表现更是好得多。而它的模型参数的数量仅和Inception一样多。

论文：

《Xception: Deep Learning with Depthwise Separable Convolutions》

代码：

https://github.com/fchollet/keras/blob/master/keras/applications/xception.py

>Francois Chollet，法国人。现为Google研究员。Keras的作者。

## 参考

https://github.com/vdumoulin/conv_arithmetic

Convolution arithmetic

http://deeplearning.net/software/theano_versions/dev/tutorial/conv_arithmetic.html

Convolution arithmetic

https://mp.weixin.qq.com/s/dR2nhGqpz7OdmxKPYSaaxw

如何理解空洞卷积（dilated convolution）？

https://mp.weixin.qq.com/s/CLFbhWMcat4rN8YS_7q25g

这12张图生动的告诉你，深度学习中的卷积网络是怎么一回事？

https://mp.weixin.qq.com/s/kJEeKzC9pC375EjIJpTuzg

一文全解深度学习中的卷积

http://mp.weixin.qq.com/s/dvuX3Ih_DZrv0kgqFn8-lg

卷积神经网络结构变化——可变形卷积网络deformable convolutional networks

http://cs.nyu.edu/~fergus/drafts/utexas2.pdf

Deconvolutional Networks

https://zhuanlan.zhihu.com/p/22245268

CNN-反卷积

http://buptldy.github.io/2016/10/29/2016-10-29-deconv/

Transposed Convolution, Fractionally Strided Convolution or Deconvolution（中文blog）

https://buptldy.github.io/2016/10/01/2016-10-01-im2col/

Implementing convolution as a matrix multiplication（中文blog）

https://mp.weixin.qq.com/s/iN2LDAQ2ee-rQnlD3N1yaw

变形卷积核、可分离卷积？CNN中十大拍案叫绝的操作！

http://www.msra.cn/zh-cn/news/features/deformable-convolutional-networks-20170609

可变形卷积网络：计算机新“视”界

https://mp.weixin.qq.com/s/ybI8kJPRn7sH-hJbc5uqnw

CMU研究者探索新卷积方法：在实验中可媲美基准CNN

https://mp.weixin.qq.com/s/qReN6z8s45870HSMCMNatw

微软亚洲研究院：逐层集中Attention的卷积模型

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

