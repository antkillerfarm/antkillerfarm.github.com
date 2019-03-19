---
layout: post
title:  深度学习（十一）——Winograd（1）
category: DL 
---

# 花式卷积

## 分组卷积（续）

参考：

https://mp.weixin.qq.com/s/b0dRvkMKSkq6ZPm3liiXxg

旷视科技提出新型卷积网络ShuffleNet，专为移动端设计

https://mp.weixin.qq.com/s/0MvCnm46pgeMGEw-EdNv_w

CNN模型之ShuffleNet

https://mp.weixin.qq.com/s/tceLrEalafgL8R44DZYP9g

旷视科技提出新型轻量架构ShuffleNet V2：从理论复杂度到实用设计准则

https://mp.weixin.qq.com/s/Yhvuog6NZOlVWEZURyqWxA

ShuffleNetV2：轻量级CNN网络中的桂冠

## Separable convolution

前面介绍的都是正方形的卷积核，实际上长条形的卷积核也是很常用的。比如可分离卷积。

我们知道卷积的计算量和卷积核的面积成正比。对于k x k的卷积核K来说，计算复杂度就是$$O(k^2)$$。

如果我们能找到1 x k的卷积核H和k x 1的卷积核V，且$$K = V * H$$，则称K是可分离的卷积核。

根据卷积运算满足结合律，可得：

$$f * K = f * (V * H) = f * V * H$$

这样就将一个k x k的卷积运算，转换成1 x k + k x 1的卷积运算，从而大大节省了参数和计算量。

显然，不是所有的卷积核都满足可分离条件。但是不要紧，NN有自动学习并逼近函数的能力。经过训练之后：$$K \approx V * H$$

## 1 x 1卷积

1、升维或降维。

如果卷积的输出输入都只是一个平面，那么1x1卷积核并没有什么意义，它是完全不考虑像素与周边其他像素关系。 但卷积的输出输入是长方体，所以1 x 1卷积实际上是对每个像素点，在不同的channels上进行线性组合（信息整合），且保留了图片的原有平面结构，调控depth，从而完成升维或降维的功能。

![](/images/article/conv_1x1.png)

2、加入非线性。卷积层之后经过激励层，1 x 1的卷积在前一层的学习表示上添加了非线性激励（non-linear activation），提升网络的表达能力；

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

它包含一个深度方面的卷积（一个为每个通道单独执行的空间卷积，depthwise convolution），后面跟着一个逐点的卷积（一个跨通道的1×1卷积，pointwise convolution）。我们可以将其看作是首先求跨一个2D空间的相关性，然后再求跨一个1D空间的相关性。可以看出，这种2D+1D映射学起来比全 3D 映射更加简单。

在ImageNet数据集上，Xception的表现稍稍优于Inception v3，而且在一个有17000类的更大规模的图像分类数据集上的表现更是好得多。而它的模型参数的数量仅和Inception一样多。

论文：

《Xception: Deep Learning with Depthwise Separable Convolutions》

代码：

https://github.com/fchollet/keras/blob/master/keras/applications/xception.py

>Francois Chollet，法国人。现为Google研究员。Keras的作者。

参考：

http://blog.csdn.net/mao_xiao_feng/article/details/78003476

tf.nn.depthwise_conv2d如何实现深度卷积?

http://blog.csdn.net/mao_xiao_feng/article/details/78002811

tf.nn.separable_conv2d如何实现深度可分卷积?

https://blog.csdn.net/tintinetmilou/article/details/81607721

Depthwise卷积与Pointwise卷积

## 感受野

Receptive Field本来是神经科学领域的概念，后来才被推广到DL（尤其是CNN）领域。

Receptive Field的大小实际上就是采样范围的大小，例如一个kernel为9x9，stride为1的普通卷积，其采样范围为13x13。（即kernel size+pad size, 9+4=13）

其他卷积的情况，可以依此类推。

对于多层神经网络的感受野，一般用Layer N上的一个点，在Input中的采样范围表示。所以层数越多，感受野越大。

需要注意的是，感受野中心的点，由于几乎每层卷积计算都会被采样到，因此它们的采样率是大于边缘点的。换句话说，就是对结果有更大的影响。因此，这又引入了**有效感受野**的概念。从实践角度来看，有效感受野的半径通常为感受野半径的1/3～1/5。

参考：

https://mp.weixin.qq.com/s/R8rEngNI31w0DQwjjeS6kw

关于感受野的总结

https://zhuanlan.zhihu.com/p/44106492

卷积神经网络的感受野

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

http://cs.nyu.edu/~fergus/drafts/utexas2.pdf

Deconvolutional Networks

https://zhuanlan.zhihu.com/p/22245268

CNN-反卷积

http://buptldy.github.io/2016/10/29/2016-10-29-deconv/

Transposed Convolution, Fractionally Strided Convolution or Deconvolution（中文blog）

https://mp.weixin.qq.com/s/ybI8kJPRn7sH-hJbc5uqnw

CMU研究者探索新卷积方法：在实验中可媲美基准CNN

# Winograd

矩阵方面的数值计算，Shmuel Winograd是一个无法绕开的人物。

>Shmuel Winograd, 1936年生，MIT本硕（1959年）+纽约大学博士（1968年）。此后一直在IBM当研究员，直到退休。IEEE Fellow，ACM Fellow，美国科学院院士。

>不要和Terry Winograd搞混了。后者是Google的两位创始人Larry Page和Sergey Brin的导师。MIT博士+斯坦福教授。

>Terry Winograd的导师也很有名，他就是AI界的宗师Seymour Papert。   
>Seymour Aubrey Papert，1928～2016，南非威特沃特斯兰德大学博士+剑桥大学博士。MIT教授。曾和Marvin Minsky合著《Perceptrons》，并一同创建了MIT AI Lab。MIT Media Lab的创立者。启蒙编程语言Logo之父。   
>更多生平参见：   
>https://zhuanlan.zhihu.com/p/21841141   
>Seymour Papert留给我们的思想遗产

>Seymour Papert至少给三个领域带来了革命，分别是儿童发展、人工智能，以及科技与教育之融合。能够在上述任何一个领域做出一点成绩都不容易了，而Papert则横跨三个领域，而且对这三个领域的发展都带来了极为深远的影响。

**Winograd FFT algorithm**：一种FFT算法。FFT算法有很多，最知名的是Cooley–Tukey FFT algorithm。

**Coppersmith–Winograd algorithm（1987年）**：目前最快的矩阵乘法算法。复杂度是$$\mathcal{O}(n^{2.375477})$$。矩阵乘法定义的复杂度是$$\mathcal{O}(n^{3})$$。第一个小于3的算法是Strassen algorithm（1969年）（$$\mathcal{O}(n^{2.807355})$$）。

**Winograd small(short/minimal) convolution algorithm**：一种快速的卷积算法，目前AI芯片领域的基础算法。

论文：

《The Coppersmith-Winograd Matrix Multiplication Algorithm》

## 基本思想

下文的大部分内容摘自Parhi的书《VLSI Digital Signal Processing Systems: Design and Implementation》

>Keshab K. Parhi，Indian Institute of Technology Kharagpur本科（1982年）+宾夕法尼亚大学硕士（1984年）+UCB博士（1988年）。明尼苏达大学教授。

这是相关章节的slide：

http://people.ece.umn.edu/users/parhi/SLIDES/chap8.pdf

Fast Convolution

我们这里以一个简单的例子，介绍一下Fast Convolution的基本思想。

复数运算$$(a+jb)(c+jd)=e+jf$$可以写成如下的矩阵形式：

$$\begin{bmatrix}
e \\ f \\
\end{bmatrix}=
\begin{bmatrix}
c & -d \\
d & c \\
\end{bmatrix}
\begin{bmatrix}
a \\ b \\
\end{bmatrix}$$

上式包含了4个乘法和2个加法。

我们知道，乘法和加法在硬件实现上的时间复杂度一般是不一样的，乘法运算所需的时间通常远大于加法所需的时间。因此，**用廉价运算代替昂贵运算**就成为了加速运算的一种方法。

具体到上面的例子：

$$\begin{cases}
ac − bd = a ( c − d ) + d ( a − b )\\
ad + bc = b ( c + d ) + d ( a − b ) \\
\end{cases}$$

这个公式包含了3个乘法和5个加法，也就是说我们用3个加法替代了1个乘法，显然只要1个乘法所需的时间大于3个加法的时间，这样的变换就是有意义的。

上式可以写成如下的矩阵形式：

$$\begin{bmatrix}
e \\ f \\
\end{bmatrix}=
\begin{bmatrix}
1 & 0 & 1 \\
0 & 1 & 1 \\
\end{bmatrix}\cdot
\begin{bmatrix}
c-d & 0 & 0 \\
0 & c+d & 0 \\
0 & 0 & d \\
\end{bmatrix}\cdot
\begin{bmatrix}
1 & 0 \\
0 & 1 \\
1 & -1\\
\end{bmatrix}\cdot
\begin{bmatrix}
a \\ b \\
\end{bmatrix}=C \cdot H \cdot D \cdot x$$

这里的H是对角矩阵。

Fast Convolution算法主要有**基于Lagrange插值的Cook-Toom算法**和**基于中国剩余定理（Chinese remainder theorem）的Winograd算法**。

## 向量卷积与多项式乘法

在继续介绍之前，我们首先看一下向量卷积与多项式乘法之间的关系。

我们首先来看一个简单的多项式乘法的例子：

$$(x^2+2x+2)(3x+2)=3x^3+\color{red}{(2\times 3+1\times 2)}x^2+10x+4$$

上式中红色的部分是有意保留下来没有合并的同类项，是不是感觉上和卷积运算十分类似呢？

我们知道矩阵的定义，除了最原始的线性方程组定义之外，还有线性向量空间定义。而多项式可以看做是以$${1,x,x^2,\dots}$$为基的空间向量，因此用多项式表示矩阵或者卷积运算，是一种很自然的方式。

这是有限离散域的Convolution定义：

$$(f * g)[n]=\sum_{m=-M}^M f[n-m]g[m]$$

>注：这里的Convolution和DL领域的Convolution的定义略有不同，后者实际上是数学上的Cross-correlation运算，但稍加变化，就可以变为前者。
