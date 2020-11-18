---
layout: post
title:  深度学习（十四）——Normalization进阶
category: DL 
---

* toc
{:toc}

# Normalization进阶

## Batch Normalization

在[《深度学习（三）》](/dl/2017/05/27/Deep_Learning_3.html#BN)中，我们已经简单的介绍了Batch Normalization的基本概念。这里主要讲述一下它的实现细节。

我们知道在神经网络训练开始前，都要对输入数据做一个归一化处理，那么具体为什么需要归一化呢？归一化后有什么好处呢？

原因在于神经网络学习过程本质就是为了学习数据分布，一旦训练数据与测试数据的分布不同，那么网络的泛化能力也大大降低；另外一方面，一旦每批训练数据的分布各不相同(batch梯度下降)，那么网络就要在每次迭代都去学习适应不同的分布，这样将会大大降低网络的训练速度，这也正是为什么我们需要对数据都要做一个归一化预处理的原因。

对输入数据归一化，早就是一种基本操作了。然而这样只对神经网络的输入层有效。更好的办法是对每一层都进行归一化。

然而简单的归一化，会破坏神经网络的特征。（归一化是线性操作，但神经网络本身是非线性的，不具备线性不变性。）因此，如何归一化，实际上是个很有技巧的事情。

首先，我们回顾一下归一化的一般做法：

$$\hat x^{(k)} = \frac{x^{(k)} - E[x^{(k)}]}{\sqrt{Var[x^{(k)}]}}\tag{1}$$

其中，$$x = (x^{(0)},x^{(1)},…x^{(d)})$$表示d维的输入向量。

接着，定义归一化变换函数：

$$y^{(k)}=\gamma^{(k)}\hat x^{(k)}+\beta^{(k)}\tag{2}$$

这里的$$\gamma^{(k)},\beta^{(k)}$$是待学习的参数。

BN的主要思想是用同一batch的样本分布来近似整体的样本分布。显然，batch size越大，这种近似也就越准确。

用$$\mathcal{B}=\{x_{1,\dots,m}\}$$表示batch，则BN的计算过程如下：

**Step 1**.计算mini-batch mean。

$$\mu_\mathcal{B}\leftarrow \frac{1}{m}\sum_{i=1}^mx_i\tag{3}$$

**Step 2**.计算mini-batch variance。

$$\sigma_\mathcal{B}^2\leftarrow \frac{1}{m}\sum_{i=1}^m(x_i-\mu_\mathcal{B})^2\tag{4}$$

**Step 3**.normalize。

$$\hat x_i\leftarrow \frac{x_i-\mu_\mathcal{B}}{\sqrt{\sigma_\mathcal{B}^2+\epsilon}}\tag{5}$$

这里的$$\epsilon$$是为了数值的稳定性而添加的常数。

**Step 4**.scale and shift。

$$y_i=\gamma\hat x_i+\beta\equiv BN_{\gamma,\beta}(x_i)\tag{6}$$

在实际使用中，BN计算和卷积计算一样，都被当作神经网络的其中一层。即：

$$z=g(Wx+b)\rightarrow z=g(BN(Wx+b))=g(BN(Wx))\tag{7}$$

从另一个角度来看，BN的均值、方差操作，相当于去除一阶和二阶信息，而只保留网络的高阶信息，即非线性部分。因此，上式最后一步中b被忽略，也就不难理解了。

BN的误差反向算法相对复杂，这里不再赘述。

在inference阶段，BN网络忽略Step 1和Step 2，只计算后两步。

- $$\beta,\gamma$$由之前的训练得到。

- $$\mu,\sigma$$原则上要求使用全体样本的均值和方差，但样本量过大的情况下，也可使用训练时的若干个mini batch的均值和方差的FIR滤波值。因此，**这两个参数也是训练得到的**。

由公式7可以看出，BN不是针对x（输入的），而是针对Wx+b的。而W每个channel都不同。因此，对于Layer: `batch*channel*height*width`来说，对`batch*height*width`个像素点统计得到一个均值和一个标准差，这样总共会得到channel组参数。

前面章节曾指出：

- relu得用Kaiming初始化。

- tanh得用Xavier初始化。

而在网络层的激活函数前加上BN的话，采用什么初始化都无所谓。

BN的缺点参见：

https://mp.weixin.qq.com/s/l2N_cQAzy_mubCeok1qfnA

Batch Normalization的诅咒

BN推理加速参见：

https://mp.weixin.qq.com/s/fAiIGJnciAi3pf8MQ7UqKg

使用Batch Normalization折叠来加速模型推理

## Instance Normalization

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

## 再看Batch Normalization

![](/images/img2/BN.jpg)

从上图可以看出，**BN是对input tensor的每个通道进行mini-batch级别的Normalization。而LN则是对所有通道的input tensor进行Normalization**。

BN的特点：

对于batch size比较小的时候，效果非常不好，而batch size越大，那么效果则越好，因为其本质上是要通过mini-batch得到对整个数据集的无偏估计；

在训练阶段和推理阶段的计算过程是不一样的；

在CNN上表现较好，而不适用于RNN甚至LSTM。

## Layer Normalization

Layer Normalization的计算比较简单，直接使用公式1即可。由于它是针对x的，因此对每个样本都要统计它的均值和方差，这对于inference来说，计算量是偏大的。

LN的特点：

不依赖于batch size的大小，即使对于batch size为1的在线学习，也可以完美适应；

训练阶段和推理阶段的计算过程完全一样。

适用于RNN或LSTM，而在CNN上表现一般。

和BN一样，LN通常也忽略公式7中的b。

从实践来看，LN在NLP任务上的表现比BN好。

参考：

https://mp.weixin.qq.com/s/-QzsVs6V2pwvFQlRVYVx2Q

LayerNorm是Transformer的最优解吗？

## Weight Normalization

WN的公式如下：

$$w=\frac{g}{\|v\|}v$$

**WN将权重分为模和方向两个分量，并分别进行训练。**

论文：

《Weight Normalization: A Simple Reparameterization to Accelerate Training of Deep Neural Networks》

WN的特点：

计算简单，易于理解。

相比于其他两种方法，其训练起来不太稳定，非常依赖于输入数据的分布。

参考：

https://mp.weixin.qq.com/s/merU6hjBVuNN2C84Rp2RkA

Weight Standarization：携手GN，超越BN

http://mlexplained.com/2018/01/13/weight-normalization-and-layer-normalization-explained-normalization-in-deep-learning-part-2/

Weight Normalization and Layer Normalization Explained

## Cosine Normalization

Normalization还能怎么做？

我们再来看看神经元的经典变换$$f_w(x)=w\cdot x$$。

对输入数据x的变换已经做过了，横着来是LN，纵着来是BN。

对模型参数w的变换也已经做过了，就是WN。

好像没啥可做的了。然而天才的研究员们盯上了中间的那个点，对，就是$$\cdot$$。

$$f_w(x)=\cos \theta=\frac{w\cdot x}{\|w\|\cdot\|x\|}$$

参考：

https://mp.weixin.qq.com/s/EBRYlCoj9rwf0NQY0B4nhQ

Layer Normalization原理及其TensorFlow实现

http://mlexplained.com/2018/01/10/an-intuitive-explanation-of-why-batch-normalization-really-works-normalization-in-deep-learning-part-1/

An Intuitive Explanation of Why Batch Normalization Really Works

https://mp.weixin.qq.com/s/KnmQTKneSimuOGqGSPy58w

详解深度学习中的Normalization，不只是BN（1）

https://mp.weixin.qq.com/s/nSQvjBRMaBeoOjdHbyrbuw

详解深度学习中的Normalization，不只是BN（2）

https://mp.weixin.qq.com/s/Z119_EpLKDz1TiLXGbygJQ

MIT新研究参透批归一化原理

https://mp.weixin.qq.com/s/Lp2pq95woQ5-E3RemdRnyw

动态层归一化（Dynamic Layer Normalization）

https://zhuanlan.zhihu.com/p/43200897

深度学习中的Normalization模型

## IBN-Net

IBN-Net是汤晓鸥小组的新作（2018.7）。

![](/images/img2/IBN-Net.png)

与BN相比，IN有两个主要的特点：第一，它不是用训练批次来将图像特征标准化，而是用单个样本的统计信息；第二，IN能将同样的标准化步骤既用于训练，又用于推断。

潘新钢等发现，IN和BN的核心区别在于，IN学习到的是不随着颜色、风格、虚拟性/现实性等外观变化而改变的特征，而要保留与内容相关的信息，就要用到BN。

论文：

《Two at Once: Enhancing Learning and Generalization Capacities via IBN-Net》

参考：

https://mp.weixin.qq.com/s/LVL90n4--WPgFLMQ-Gnf6g

汤晓鸥为CNN搓了一颗大力丸

https://mp.weixin.qq.com/s/6hNpgffEnUTkNAfrPgKHkA

IBN-Net：打开Domain Generalization的新方式

https://mp.weixin.qq.com/s/lCasw_-Bl3_J6cGBipNsSA

从IBN-Net到Switchable Whitening：在不变性与判别力之间权衡

## Group Normalization

论文：

《Group Normalization》

![](/images/img2/Group_Normalization.png)

参考：

https://mp.weixin.qq.com/s/H2GmqloNumttFlaSArjgUg

FAIR何恺明等人提出组归一化：替代批归一化，不受批量大小限制

https://mp.weixin.qq.com/s/44RvXEYYc5lebsHs_ooswg

全面解读Group Normalization

https://mp.weixin.qq.com/s/J8i0Qsl0q9sYS2iAc-M44w

BN，LN，IN，GN都是什么？不同归一化方法的比较

## L2 Normalization

L2 Normalization本身并不复杂，然而多数资料都只提到1维的L2 Normalization的计算公式：

$$
\begin{array}\\
x=[x_1,x_2,\dots,x_d]\\
y=[y_1,y_2,\dots,y_d]\\
y=\frac{x}{\sqrt{\sum_{i=1}^dx_i^2}}=\frac{x}{\sqrt{x^Tx}}
\end{array}
$$

对于多维L2 Normalization几乎未曾提及，这里以3维tensor：A[width, height, channel]为例介绍一下多维L2 Normalization的计算方法。

多维L2 Normalization有一个叫axis(有时也叫dim)的参数，如果axis=0的话，实际上就是将整个tensor flatten之后，再L2 Normalization。这个是比较简单的。

这里说说axis=3的情况。axis=3意味着对channel进行Normalization，也就是：

$$B_{xy}=\sum_{z=0}^Z \sqrt{A_{xyz}^2}\\
C_{xyz}=\frac{A_{xyz}}{B_{xy}}\\
D_{xyz}=C_{xyz} \cdot S_{z}
$$

一般来说，求出C的运算被称作L2 Normalization，而求出D的运算被称作L2 Scale Normalization，S被称为Scale。

## Local Response Normalization

![](/images/img3/LRN.png)

LRN最早出自Alexnet，虽然后来由于效果不佳，已经很少使用了，但它的思路还是可以借鉴的。

LRN分为Inter-Channel LRN和Intra-Channel LRN两种，如果不加说明的话，一般是指前者。
