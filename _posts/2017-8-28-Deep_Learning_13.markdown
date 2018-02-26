---
layout: post
title:  深度学习（十三）——花式池化, Batch Normalization
category: DL 
---

# Winograd（续）

## Winograd algorithm

下面以一个2x3的卷积为例，介绍一下Winograd algorithm的做法。

2x3卷积的多项式形式为：

$$h(p)=h_0+h_1p,x(p)=x_0+x_1p+x_2p^2,s(p)=h(p)x(p)$$

这里引入**多项式（polynomial）的度（degree）**的概念：多项式中包含的最高次项的次数，被称为多项式的度。

例如，上面的$$h(p)$$的degree为1，而$$x(p)$$的degree为2，而$$s(p)$$的degree为3。

和Cook-Toom algorithm一样，Winograd algorithm也是一个构造式的算法。

**Step 1**：首先要构造一个degree大于等于3的多项式：

$$m(p)=m^{(0)}(p)m^{(0)}(p)\cdots m^{(k)}(p)$$

其中的$$m^{(i)}(p)$$两两互质。

这里为了简单起见，不妨令$$m(p)=p(p-1)(p+1)$$，并使用Extended Euclidean algorithm构建如下计算表格：

| i | $$m^{(i)}(p)$$ | $$M^{(i)}(p)$$ | $$n^{(i)}(p)$$ | $$N^{(i)}(p)$$ |
|:--:|:--:|:--:|:--:|:--:|
| 0 | $$p$$ | $$p^2-1$$ | $$p$$ | $$-1$$ |
| 1 | $$p-1$$ | $$p^2+p$$ | $$-\frac{1}{2}(p+2)$$ | $$\frac{1}{2}$$ |
| 2 | $$p+1$$ | $$p^2-p$$ | $$-\frac{1}{2}(p-2)$$ | $$\frac{1}{2}$$ |

**Step 2**：使用如下公式计算$$h^{(i)}(p),x^{(i)}(p)$$：

$$h^{(i)}(p)=h(p)\mod{m^{(i)}(p)}$$

$$x^{(i)}(p)=x(p)\mod{m^{(i)}(p)}$$

计算过程如下：

$$h^{(0)}(p)=h_0,x^{(0)}(p)=x_0$$

$$h^{(1)}(p)=h_0+h_1,x^{(1)}(p)=x_0+x_1+x_2$$

$$h^{(2)}(p)=h_0-h_1,x^{(2)}(p)=x_0-x_1+x_2$$

**Step 3**：使用如下公式计算$$s'^{(i)}(p)$$：

$$s'^{(i)}(p)=h^{(i)}(p)x^{(i)}(p)\mod{m^{(i)}(p)}$$

计算过程如下：

$$s'^{(0)}(p)=h_0x_0$$

$$s'^{(1)}(p)=(h_0+h_1)(x_0+x_1+x_2)$$

$$s'^{(2)}(p)=(h_0-h_1)(x_0-x_1+x_2)$$

**Step 4**：根据公式3计算余数$$s'(p)$$，并利用如下公式计算被除数$$s(p)$$：

$$s(p)=s'(p)+h_{N-1}x_{L-1}m(p)$$

计算过程如下：

$$\begin{align}
s(p)&=s'(p)+h_1x_2m(p) \\
&=s'(0)(-p^2+1)+\frac{s'(1)}{2}(p^2+p)+\frac{s'(2)}{2}(p^2-p)+h_1x_2m(p^3-p)\\
&=s'(0)+p(\frac{s'(1)}{2}-\frac{s'(2)}{2}-h_1x_2)+p^2(-s'(0)+\frac{s'(1)}{2}+\frac{s'(2)}{2})+p^3(h_1x_2)
\end{align}$$

这里用4个乘法和7个加法，替代了6个乘法和2个加法。

总的来说，Winograd algorithm是一个很复杂的算法，但是结论却很简单。因此，在具体的IC实现中，一般只针对特定常用尺寸的kernel，应用相应的结论即可。

>Winograd这个知识点的复杂，其实主要还不在于算法本身，而是在于其前置了很多数论方面的知识。而我恰恰不具备这些知识，因此进展极度缓慢，前后用了近20天才看完了相关的内容。。。不过，收获很大^_^

## Winograd for CNN

CNN中的Winograd算法一般使用如下论文的结论：

《Fast Algorithms for Convolutional Neural Networks》

该文引论部分提到了Winograd算法的结论，该结论和本文上述的算法步骤略有不同，最初是Winograd针对FIR提出的Minimal FIR Filtering算法。但是算法的本质是相同的，仍然是构建多项式和CRT。

https://github.com/andravin/wincnn

这个项目可以很方便的计算不同大小的核的Winograd的结果。这个项目中还有一个pdf文件作为上述论文的补充材料，详细的给出了各矩阵的计算方法。

## FFT与卷积

FFT是加速卷积运算的一种常用方法。但由于其原理，当卷积核小的时候，是没什么加速的，当核是3或者5时，速度有时更慢或者相当，而在CNN中卷积的核大多数比较小，FFT起到的加速作用很小，所以基本没人用。

此外，FFT是复数运算，如果没有特殊硬件，而用实数计算的话，还是比较费劲的。

参见：

http://www.cnblogs.com/jianyingzhou/p/4303578.html

convolution,fft, 加速

http://www.cnblogs.com/luoqingyu/p/5930181.html

数字信号处理--FFT与蝶形算法

http://blog.csdn.net/xxinliu/article/details/7438429

Cooley-Tukey算法 （蝶形算法）

## 参考

https://colfaxresearch.com/falcon-library/

FALCON Library: Fast Image Convolution in Neural Networks on Intel Architecture

https://www.intelnervana.com/winograd/

"Not so fast, FFT": Winograd

https://www.encyclopediaofmath.org/index.php/Winograd_small_convolution_algorithm

Winograd small convolution algorithm

https://mp.weixin.qq.com/s/VcGwT0rKYQr13MQ_PRVmyg

每个AI程序员都应该知道的基础数论

https://mp.weixin.qq.com/s/jCPHXVI_utwnxaHqvW5nyA

商汤联合提出基于FPGA的快速Winograd算法：实现FPGA之上最优的CNN表现与能耗

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

