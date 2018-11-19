---
layout: post
title:  深度学习（十四）——Instance Normalization, IBN-Net, Softmax详解, Style Transfer
category: DL 
---

# Batch Normalization（续）

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

# IBN-Net

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

https://mp.weixin.qq.com/s/HTIgKm8HuZZ_-lIQ3nIFhQ

浅入深出之大话SoftMax

https://mp.weixin.qq.com/s/XBK7T1P7z3rm3o-3BDNeOA

三分钟带你对Softmax划重点

https://mp.weixin.qq.com/s/vhvXsSsEHPVjJGqtCOOwLw

Softmax和交叉熵的深度解析和Python实现

# Style Transfer

![](/images/img2/style_transfer_2.jpg)

上图是Style Transfer问题的效果图：**将图片B的风格迁移到另一张图片A上。**

![](/images/img2/style_transfer.jpg)

上图是图像风格迁移的科技树。

## 早期方法

图像风格迁移这个领域，在2015年之前，连个合适的名字都没有，因为每个风格的算法都是各管各的，互相之间并没有太多的共同之处。

比如油画风格迁移，里面用到了7种不同的步骤来描述和迁移油画的特征。又比如头像风格迁移里用到了三个步骤来把一种头像摄影风格迁移到另一种上。以上十个步骤里没一个重样的。

可以看出这时的图像风格处理的研究，基本都是各自为战，捣鼓出来的算法也没引起什么注意。

![](/images/img2/style_transfer_3.jpg)

上图是一个油画风格迁移的pipe line。

和风格迁移相关的另一个领域——纹理生成，这时虽然已经有了一些成果，但是通用性也比较差。

早期纹理生成的主要思想：**纹理可以用图像局部特征的统计模型来描述。**然而手工建模毕竟耗时耗力。。。

## CNN的纹理特征

在进行神经风格迁移之前，我们先来从可视化的角度看一下卷积神经网络每一层到底是什么样子？它们各自学习了哪些东西。

遍历所有训练样本，找出让该层激活函数输出最大的9块图像区域；然后再找出该层的其它单元（不同的滤波器通道）激活函数输出最大的9块图像区域；最后共找9次，得到$$9 \times 9$$的图像如下所示，其中每个$$3 \times 3$$区域表示一个运算单元。

![](/images/img2/style_transfer_2.png)

可以看出随着层数的增加，CNN捕捉的区域更大，特征更加复杂，从边缘到纹理再到具体物体。

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
