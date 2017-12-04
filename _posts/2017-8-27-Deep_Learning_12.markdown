---
layout: post
title:  深度学习（十二）——花式池化, Batch Normalization
category: DL 
---

# Winograd（续）

## 中国剩余定理

Chinese remainder theorem算是初等数论中，一个非常重要的定理了。（初等数论意指使用不超过高中程度的初等代数处理的数论问题，其最主要的工具包括整数的整除性与同余。）

CRT最早出自中国四世纪成书的古书《孙子算经》。著名的娱乐圈学霸关晓彤同学所攻克的“鸡兔同笼问题”，就出自该书。 

CRT的内容为：

设$$m_i$$为两两互质（pairwise coprime）的大于1的整数，$$a_i$$为任意整数，则存在x满足：

$$\begin{align} x \equiv a_1 & \pmod{m_1} \\ \quad \vdots \\ x \equiv a_k &\pmod{m_k} \end{align}$$

如果$$0\le x < M,M=\prod_{i=1}^k m_i$$，则该x是唯一的。

CRT的存在性证明略。

这里以如下简单的例子，来讲讲如何求解x。

$$\begin{align}
 x &\equiv 0 \pmod{3} \\
 x &\equiv 3 \pmod{4} \\
 x &\equiv 4 \pmod{5}.
\end{align}$$

这个问题的穷举法需要遍历0到M的所有整数，这显然是个十分低效的算法。因此无论手算还是计算机算，基本都不用穷举法。

再来介绍一下筛法（Sieving）：

1.首先对$$m_i$$按降序排序。

2.选择最大的模（这里为5）和对应的$$a_i$$（这里为4）。

3.

{% highlight text %}
4 mod 4 → 0. Continue
4 + 5 = 9 mod 4 →1. Continue
9 + 5 = 14 mod 4 → 2. Continue
14 + 5 = 19 mod 4 → 3. OK, continue by considering remainders modulo 3 and adding 5×4 = 20 each time
19 mod 3 → 1. Continue
19 + 20 = 39 mod 3 → 0. OK, this is the result.
{% endhighlight %}

筛法对于M较小的情况，是非常高效的，因此手算一般都采用该法。但是，筛法的复杂度是指数级的，对于M较大的情况，并不好用。

CRT虽然只是初等数论的基本定理，但应用范围很广，Lagrange interpolation（一阶多项式插值）、Hermite interpolation（多阶多项式插值）和Dedekind's theorem，都用到了CRT。

## 多项式的Euclidean division和GCD

我们可以仿照整数Euclidean division定义多项式的Euclidean division，如下面的竖式所示：

$$\begin{array}{r}
 x^2 + {\color{White}1}x + 3\\
 x-3\overline{) x^3 - 2x^2 + 0x - 4}\\
 \underline{x^3 - 3x^2 {\color{White} {} + 0x - 4}}\\
 +x^2 + 0x {\color{White} {} - 4}\\
 \underline{+x^2 - 3x {\color{White} {} - 4}}\\
 +3x - 4\\
 \underline{+3x - 9}\\
 +5
\end{array}$$

上式也可写为横式：

$${x^3 - 2x^2 - 4} = (x-3)\,\underbrace{(x^2 + x + 3)}_{q(x)}  +\underbrace{5}_{r(x)}$$

其中的$$r(x)$$即为余数。

同样的可以定义多项式的GCD：

$$x^2 + 7x + 6 = (x + 1)(x + 6)$$

$$x^2 − 5x − 6 = (x + 1)(x − 6)$$

则两多项式的GCD为$$(x + 1)$$。

## 多项式的CRT

CRT亦可改为如下等效形式：

$$c=\left(\sum_{i=0}^kc_iN_iM_i\right)\mod{M}$$

其中$$m_i$$两两互质，$$c_i=R_{m_i}[c],M=\prod_{i=0}^km_i,M_i=M/m_i$$，$$N_i,n_i$$是方程$$N_i M_i + n_i m_i = GCD ( M_i , m_i ) = 1$$的解。

显然这里的$$N_i,n_i$$可以使用Extended Euclidean algorithm求解。

稍加扩展，可得到多项式版本的CRT：

$$c(p)=\left(\sum_{i=0}^kc^{(i)}(p)N^{(i)}(p)M^{(i)}(p)\right)\mod{M(p)}$$

其中$$m^{(i)}(p)$$两两互质，$$c^{(i)}(p)=R_{m^{(i)}}[c(p)],M(p)=\prod_{i=0}^km^{(i)}(p),M^{(i)}(p)=M(p)/m^{(i)}(p)$$，$$N^{(i)}(p)$$是方程$$N^{(i)}(p) M^{(i)}(p) + n^{(i)}(p) m^{(i)}(p) = GCD ( M^{(i)}(p) , m^{(i)}(p)) = 1$$的解。

## Winograd algorithm

下面以一个2x3的卷积为例，介绍一下Winograd algorithm的做法。

2x3卷积的多项式形式为：$$h(p)=h_0+h_1p,x(p)=x_0+x_1p+x_2p^2$$




总的来说，Winograd algorithm是一个很复杂的算法，但是结论却很简单。因此，在具体的IC实现中，一般只针对特定常用尺寸的kernel，实现相应的结论即可。

## 参考

https://colfaxresearch.com/falcon-library/

FALCON Library: Fast Image Convolution in Neural Networks on Intel Architecture

https://www.intelnervana.com/winograd/

"Not so fast, FFT": Winograd

https://www.encyclopediaofmath.org/index.php/Winograd_small_convolution_algorithm

Winograd small convolution algorithm

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


