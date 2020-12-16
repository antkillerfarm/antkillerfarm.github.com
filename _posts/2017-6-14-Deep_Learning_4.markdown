---
layout: post
title:  深度学习（四）——AutoEncoder
category: DL 
---

* toc
{:toc}

# CNN

## 卷积（续）

需要注意的是，传统的CV算法中，通常只有单一的卷积运算。而CNN中的卷积层，实际上包括了**卷积+激活**两种运算，即：

$$L_2=\sigma(Conv(L_1,W)+b)$$

因此，相比全连接层而言，卷积层每次只有部分元素参与到最终的激活运算。从宏观角度看，这些元素实际上对应了图片的局部空间二维信息，它和后面的Pooling操作一道，起到了空间降维的作用。

实际上，传统的MLP（MultiLayer Perceptron）网络，就是由于1D全连接的神经元控制了太多参数，而不利于学习到稀疏特征。

CNN网络中，2D全连接的神经元则控制了局部感受野，有利于解离出稀疏特征。

至于激活函数，则是为了保证变换的非线性。这也是CNN被归类为NN的根本原因。

参考：

https://mp.weixin.qq.com/s/dIIWAKv9woLO8M0CyN8lsw

传统计算机视觉技术落伍了吗？不，它们是深度学习的“新动能”

## 池化

Pooling操作（也称Subsampling）使输入表示（特征维度）变得更小，并且网络中的参数和计算的数量更加可控的减小，因此，可以控制过拟合。

它还可使网络对于输入图像中更小的变化、冗余和变换变得不变性。

## Gaussian Connections

LeNet-5最后一步的Gaussian Connections是一个当年的历史遗迹，目前已经被Softmax所取代。它的含义在上面提到的Yann LeCun的原始论文中有描述。

>注意：现代版的LeNet-5最后一步的Softmax层，实际上包含了$$Wx+b$$和Softmax两种计算。相当于用Softmax函数替换Sigmoid/ReLU函数。

### 其他

![](/images/article/CNN_1.jpg)

上图展示了不同分类的图片特征在特征空间中的分布，可以看出在CNN的低层中，这些特征是混杂在一起的；而到了CNN的高层，这些特征就被区分开来了。

![](/images/article/CNN_2.jpg)

上图是若干ML、DL算法按照不同维度划分的情况。

## 多通道卷积

MNIST的例子中，由于图像是单通道（灰度图）的，因此，多数教程都只是展示了单通道卷积的计算步骤：

$$g(i,j)=\sum_{k,l}f(i+k,j+l)h(k,l)$$

而多通道（例如彩色图像的RGB三通道）卷积实际上就是将各通道卷积之后的结果再加在一起：

$$g(i,j)=\sum_{c,k,l}f(i+k,j+l)h(k,l)$$

![](/images/article/conv_1.png)

上图展示了一个4通道图像经卷积之后，得到2通道图像的过程。

从中可以得出以下结论：

1.RGB通道信息在卷积之后，就不复存在了。无论输入图像有多少个通道，输出图像的通道数只和feature的个数相关。

2.即使是LeNet-5的MNIST示例中，实际上也是有多通道卷积的，只不过不在第一个卷积层而已。

3.多通道卷积除了二维空间信息的卷积之外，还包括了**通道间信息**的卷积。这也是CNN中1x1卷积的意义之一。

## CNN的反向传播算法

由于卷积和池化两层，不是一般的神经网络结构。因此CNN的反向传播算法实际上也是很有技巧的。

上面提到卷积操作可以转换为矩阵运算：$$y=Cx$$

其对应的梯度反向传播公式为：

$$\frac{\partial Loss}{\partial x} = \frac{\partial y^T}{\partial x}\cdot \frac{\partial Loss}{\partial y} = C^T \cdot \frac{\partial Loss}{\partial y}$$

因此：

正向：$$Y=X*K$$

反向：$$\Delta X = \Delta Y * rot_{180}(K)$$

卷积的反向传播，有时也被称为反卷积（Deconvolution）。

![](/images/article/dcign.png)

上图是Deep convolutional inverse graphics networks的结构图。DCIGN实际上是一个正向CNN连上一个反向CNN，以实现图片合成的目的。其原理可参考《深度学习（四）》中的Autoencoder。

参见：

http://www.cnblogs.com/pinard/p/6494810.html

卷积神经网络(CNN)反向传播算法

http://blog.csdn.net/zy3381/article/details/44409535

CNN误差反传时旋转卷积核的简明分析

https://mp.weixin.qq.com/s/dnElNURJ6xfWHJVf_yeT8w

理解多层CNN中转置卷积的反向传播

https://www.jianshu.com/p/f0674e48894c

Tensorflow反卷积（DeConv）实现原理

## 参考

http://lib.csdn.net/article/deeplearning/58185

BP神经网络与卷积神经网络

http://blog.csdn.net/Fate_fjh/article/details/52882134

卷积神经网络系列blog

https://zhuanlan.zhihu.com/p/47184529

卷积神经网络（CNN）详解

http://mp.weixin.qq.com/s/YRwGwelyA3VOYZ4XGAjUBw

CNN感受野首次可视化：深入解读及计算指南

https://mp.weixin.qq.com/s/EJyG3Y4EHTGMm_Q1mY4RvA

CNN入门手册（上）

https://mp.weixin.qq.com/s/T3tHFdjnQh4asE0V25vTog

CNN入门手册（中）

https://mp.weixin.qq.com/s/chsDjS39qcoHICUNbSdQHQ

长文揭秘图像处理和卷积神经网络架构

https://mp.weixin.qq.com/s/nIbfiDXkqkpdLzQo2Gmc2Q

利用卷积神经网络处理CIFAR图像分类

https://mp.weixin.qq.com/s/5BMU7SRQeuDg68XDcOUBZw

训练集样本不平衡问题对CNN的影响

https://mp.weixin.qq.com/s/p-wZ_6ZQW-zXzDqmRenNow

深度学习入门：几幅手稿讲解CNN

https://mp.weixin.qq.com/s/xXf7hTfH-vx4YbzlZVQucA

CNN入门再介绍

https://mp.weixin.qq.com/s/Q4snAlAi8tPQAyGm0qUy4w

CNN的全面解析

https://mp.weixin.qq.com/s/Do6erhin3W4dK_-RTAyD6A

卷积神经网络(CNN)概念解释

http://www.qingruanit.net/blog/23930/note5837.html

卷积神经网络（CNN）学习算法之----基于LeNet网络的中文验证码识别

https://mp.weixin.qq.com/s/XiaAPd20YxbM0wDiSTAYMg

深度学习之卷积神经网络(CNN)的模型结构

https://mp.weixin.qq.com/s/x-H6h4sRqTrZlOXKStnhPw

卷积神经网络背后的数学原理

https://mp.weixin.qq.com/s/qIdjHqurqvdahEd0dXYIqA

徒手实现CNN：综述论文详解卷积网络的数学本质

https://mp.weixin.qq.com/s/D6ok6dQqyx6cCJKc2M8YpA

从AlexNet剖析-卷积网络CNN的一般结构

https://mp.weixin.qq.com/s/XZeZX8zTtNom0az_ralp4A

ImageNet冠军带你入门计算机视觉：卷积神经网络

https://mp.weixin.qq.com/s/t8jg_bpEcQiJmgIqKarefQ

卷积神经网络CNN学习笔记

https://mp.weixin.qq.com/s/buRuerMHkRMSSVlvmlDdtw

人人都能读懂卷积神经网络：Convolutional Networks for everyone

https://mp.weixin.qq.com/s/3pIybS_GsuN6XobP_4bLrg

深度学习以及卷积基础

https://mp.weixin.qq.com/s/VcjivPhJuCk7NPQ1FNQULw

一文让你入门CNN

https://mp.weixin.qq.com/s/zvPNuP_LT7pWIgoxzfeUWw

综述卷积神经网络：从基础技术到研究前景

# 图像超分辨率进阶+

https://zhuanlan.zhihu.com/p/67613641

基于多级神经纹理迁移的图像超分辨方法(Adobe Research)

https://mp.weixin.qq.com/s/IStOD22WgZ6EBFCvIHkNSg

图像超分辨率网络：RCAN

https://mp.weixin.qq.com/s/4LIq3kZaXgaoEEyb8TQvwg

图像超分辨率重建--AI研究

https://mp.weixin.qq.com/s/ETGPXVDGRHOq0W-ef_q2_A

RankSRGAN:排序学习+GAN用于超分辨率

https://zhuanlan.zhihu.com/p/140507840

图像超分：USRNet

https://mp.weixin.qq.com/s/sju4SYFxzDkJevk3mi68Rw

图像超分辨率网络：EDSR

https://mp.weixin.qq.com/s/uZQK0oQbV7wm0WVz_QxVQg

DRN：用于单图像超分辨率的对偶回归网络

# AutoEncoder

## Basic AE

Bengio在2003年的《A neural probabilistic language model》中指出，维度过高，会导致每次学习，都会强制改变大部分参数。

由此发生蝴蝶效应，本来很好的参数，可能就因为一个小小传播误差，就改的乱七八糟。

因此，数据降维是数据预处理中，非常重要的一环。常用的降维算法，除了线性的PCA算法之外，还有非线性的Autoencoder。

![](/images/article/Autoencoder.png)

Autoencoder的结构如上图所示。它的特殊之处在于：

1.输入样本就是输出样本。

2.隐藏层的神经元数量小于样本的维度。

粗看起来，这类恒等变换没有太大意义。然而这类恒等变换之所以能够成立，最根本的地方在于，隐藏层的神经元具有表达输出样本的能力，也就是用低维表达高维的能力。反过来，我们就可以利用这一点，实现数据的降维操作。

但是，不是所有的数据都能够降维，而这种情况通常会导致Autoencoder的训练失败。

类似的，如果隐藏层的神经元数量大于样本的维度，则该AE可用于升维。这样的AE又叫做Sparse autoencoders。

![](/images/img3/sae.png)

总体来看，AE是个Encoder/Decoder结构。我们上面提到的降维/升维，主要是利用了Encoder部分。而Decoder部分也是很有意义的，它表明我们能够从tensor生成样本，这实际上就是一种**生成模型**。

和Autoencoder类似的神经网络还有：Denoising Autoencoder（DAE）。

![](/images/img3/dae.png)

黄色的三角表明输入数据中被加入了噪声。当然了DAE的输出要和无噪声样本做比较，这样才能体现去噪的效果。

## Stacked AutoEncoders

AE不仅可以单独使用，还可以堆叠式的使用。

![](/images/img2/Stacked_SparseAE_Features1.png)

上图是个普通的AE，其中的hidden层可以看作是input的Features，不妨称作Features I。

![](/images/img2/Stacked_SparseAE_Features2.png)

将Features I作为input，送进另一个AE，得到Features II。依此类推，就可以形成一个深度网络，这种方法叫做Stacked Auto-encoder Networks（SANs）。

这实际上，就是Relu发明之前，预训练DNN的标准做法。经过SANs预训练的网络，每层的参数都被归一化，即使使用sigmoid激活函数，也没有严重的梯度消失现象，从而使DNN的训练成为了可能。

参考：

http://ufldl.stanford.edu/wiki/index.php/Stacked_Autoencoders

Stacked Autoencoders

## Deep AE

一层的AE有时可能不能很好的进行数据降维，这个时候就可以使用如下所示的Deep AE：

![](/images/img2/Deep_AE.jpg)

Deep AE可用于异常检测：根据正常数据训练出来的Autoencoder，能够将正常样本重建还原，但是却无法将异于正常分布的数据点较好地还原，导致还原误差较大。

参考：

http://sofasofa.io/tutorials/anomaly_detection/

利用Autoencoder进行无监督异常检测

https://zhuanlan.zhihu.com/p/51053142

基于自编码器的时间序列异常检测算法
