---
layout: post
title:  深度学习（十五）——fine-tuning, Style Transfer（1）
category: DL 
---

# fine-tuning（续）

由于ImageNet数以百万计带标签的训练集数据，使得如CaffeNet之类的预训练的模型具有非常强大的泛化能力，这些预训练的模型的中间层包含非常多一般性的视觉元素，我们只需要对他的后几层进行微调，再应用到我们的数据上，通常就可以得到非常好的结果。最重要的是，**在目标任务上达到很高performance所需要的数据的量相对很少**。

虽然从理论角度尚无法完全解释fine-tuning的原理，但是还是可以给出一些直观的解释。我们知道，CNN越靠近输入端，其抽取的图像特征越原始。比如最初的一层通常只能抽取一些线条之类的元素。越上层，其特征越抽象。

而现实的图像无论多么复杂，总是由简单特征拼凑而成的。因此，无论最终的分类结果差异如何巨大，其底层的图像特征却几乎一致。

![](/images/article/trans_learn.png)

fine-tuning也是图像目标检测、语义分割的基础。

参考：

https://zhuanlan.zhihu.com/p/22624331

fine-tuning:利用已有模型训练其他数据集

http://www.cnblogs.com/louyihang-loves-baiyan/p/5038758.html

Caffe fine-tuning微调网络

http://blog.csdn.net/sinat_26917383/article/details/54999868

caffe中fine-tuning模型三重天（函数详解、框架简述）+微调技巧

http://yongyuan.name/blog/layer-selection-and-finetune-for-cbir.html

图像检索：layer选择与fine-tuning性能提升验证

h1ttps://www.zhihu.com/question/49534423

迁移学习与fine-tuning有什么区别？

https://zhuanlan.zhihu.com/p/37341493

基于Pre-trained模型加速模型学习的6点建议

https://mp.weixin.qq.com/s/54HQU3B4cSdRb1Z4srSfJg

如何为新类别重新训练一个图像分类器

# Style Transfer

![](/images/img2/style_transfer_2.jpg)

上图是Style Transfer问题的效果图：**将图片B的风格迁移到另一张图片A上。**

![](/images/img2/style_transfer.jpg)

上图是图像风格迁移所涉及的科技树。

在继续讨论之前，我们有必要指出Style Transfer和其他传统的有监督学习的CV问题之间的差异。

1.风格这种抽象的概念，该如何定义？艺术领域的很多东西，通常都是很难量化的。如果要采用有监督学习的方法的话，怎么获得学习的标签呢？

2.就算解决了第1个问题，如何生成一批同一风格的图片也是一个大问题。画家可是稀缺资源啊。

因此，Style Transfer问题的关键点就在于：如何利用仅有的1张风格图片，将其风格迁移到其他图片上。

## 早期方法

图像风格迁移这个领域，在2015年之前，连个合适的名字都没有，因为每个风格的算法都是各管各的，互相之间并没有太多的共同之处。

比如油画风格迁移，里面用到了7种不同的步骤来描述和迁移油画的特征。又比如头像风格迁移里用到了三个步骤来把一种头像摄影风格迁移到另一种上。以上十个步骤里没一个重样的。

可以看出这时的图像风格处理的研究，基本都是各自为战，捣鼓出来的算法也没引起什么注意。

![](/images/img2/style_transfer_3.jpg)

上图是一个油画风格迁移的pipe line。

在实践过程中，人们又发现**图像的纹理可以在一定程度上代表图像的风格**。下文如无特指，纹理/风格均为同义词。

这又引入了和风格迁移相关的另一个领域——纹理生成。这个时期，该领域虽然已经有了一些成果，但是通用性也比较差。

早期纹理生成的主要思想：**纹理可以用图像局部特征的统计模型来描述。**然而手工建模毕竟耗时耗力。。。

## CNN的纹理特征

在进行神经风格迁移之前，我们先来从可视化的角度看一下卷积神经网络每一层到底是什么样子？它们各自学习了哪些东西。

遍历所有训练样本，找出让该层激活函数输出最大的9块图像区域；然后再找出该层的其它单元（不同的滤波器通道）激活函数输出最大的9块图像区域；最后共找9次，得到$$9 \times 9$$的图像如下所示，其中每个$$3 \times 3$$区域表示一个运算单元。

![](/images/img2/style_transfer_2.png)

可以看出随着层数的增加，CNN捕捉的区域更大，特征更加复杂，从边缘到纹理再到具体物体。

## Deep Visualization

上述的CNN可视化的方法一般被称作Deep Visualization。

论文：

《Understanding Neural Networks Through Deep Visualization》

这篇论文是Deep Visualization的经典之作。作者是Jason Yosinski。

>Jason Yosinski，Caltech本科+Cornell博士。现为Uber AI Labs的科学家。   
>个人主页：   
>http://yosinski.com/

该文提出了如下公式：

$$V(F_i^l)=\mathop{\arg\max}_{X}A_i^l(X), X \leftarrow X + \eta\frac{\partial A_i^l(X)}{\partial X}$$

X初始化为一张噪声图片，然后按照上述公式，优化得到激活函数输出最大的X。

Deep Visualization除了用于提取纹理之外，还可用于模型压缩。

论文：

《Demystifying Neural Network Filter Pruning》

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

https://mp.weixin.qq.com/s/dflEAOELK0f19y4KuVd_dQ

40行Python代码，实现卷积特征可视化

https://mp.weixin.qq.com/s/fpVFpN5beJFyLW19WnBzvw

揭开黑盒一角！谷歌联合OpenAI发布“神经元显微镜”，可视化神经网络运行机制

https://mp.weixin.qq.com/s/o-MFPt6LR_5XDpacqw754Q

马里兰大学论文：可视化神经网络的损失函数

## DL方法

受到上述事实的启发，2015年德国University of Tuebingen的Leon A. Gatys写了如下两篇论文：

《Texture Synthesis Using Convolutional Neural Networks》

《A Neural Algorithm of Artistic Style》

代码：

https://github.com/jcjohnson/neural-style

torch版本

https://github.com/anishathalye/neural-style

tensorflow版本

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

神经风格迁移的基本算法流程是：首先令G为随机像素点，然后使用梯度下降算法，不断修正G的所有像素点，使得J(G)不断减小，从而使G逐渐有C的内容和S的风格，如下图所示：

![](/images/img2/style_transfer_3.png)

换句话来说就是：**每次迭代只更新图片G，而不更新网络的参数。**

我们先来看J(G)的第一部分$$J_{content}(C,G)$$，它表示内容图片C与生成图片G之间的相似度。

使用的CNN网络是之前预训练好的模型，例如Alex-Net。**C，S，G共用相同模型和参数。**

首先，需要选择合适的层数l来计算$$J_{content}(C,G)$$。

如前所述，CNN的每个隐藏层分别提取原始图片的不同深度特征，由简单到复杂。如果l太小，则G与C在像素上会非常接近，没有迁移效果；如果l太深，则G上某个区域将直接会出现C中的物体。因此，l既不能太浅也不能太深，一般选择网络中间层。

若C和G在l层的激活函数输出$$a^{[l](C)}$$与$$a^{[l](G)}$$，则相应的$$J_{content}(C,G)$$的表达式为：

$$J_{content}(C,G)=\frac12||a^{[l](C)}-a^{[l](G)}||^2$$

接下来，我们定义图片的风格矩阵（style matrix）为：

$$G_{kk'}^{[l]}=\sum_{i=1}^{n_H^{[l]}}\sum_{j=1}^{n_W^{[l]}}a_{ijk}^{[l]}a_{ijk'}^{[l]}$$

风格矩阵$$G_{kk'}^{[l]}$$计算第l层隐藏层不同通道对应的所有激活函数输出和。若两个通道之间相似性高，则对应的$$G_{kk'}^{[l]}$$较大。从数学的角度来说，这里的风格矩阵实际上就是两个tensor的**互相关矩阵**，也就是上面提到的Gram矩阵。

Gram矩阵描述的是全局特征的自相关，如果输出图与风格图的这种自相关相近，那么差不多是我们所理解的”风格”。当然，其实也可以用很多其他的统计信息进行描绘风格。比如有用直方图的, 甚至还可以直接简化成”均值+方差”的。

风格矩阵$$G_{kk'}^{[l][S]}$$表征了风格图片S第l层隐藏层的“风格”。相应地，生成图片G也有$$G_{kk'}^{[l][G]}$$。那么，$$G_{kk'}^{[l][S]}$$与$$G_{kk'}^{[l][G]}$$越相近，则表示G的风格越接近S。这样，我们就可以定义出$$J^{[l]}_{style}(S,G)$$的表达式：

$$J^{[l]}_{style}(S,G)=\frac{1}{(2n_H^{[l]}n_W^{[l]}n_C^{[l]})}\sum_{k=1}^{n_C^{[l]}}\sum_{k'=1}^{n_C^{[l]}}||G_{kk'}^{[l][S]}-G_{kk'}^{[l][G]}||^2$$

为了提取的“风格”更多，也可以使用多层隐藏层，然后相加，表达式为：

$$J_{style}(S,G)=\sum_l\lambda^{[l]}\cdot J^{[l]}_{style}(S,G)$$

## 实现细节

![](/images/img2/style_transfer.png)

这是原始论文的插图，其符号表示和本文有所差异。其中的A、F、P各层的output，都是使用预训练好的Alex-Net生成的。

可以看出A和P，在整个迭代过程中，只需要进行一次Alex-Net的前向计算，因此可以事先计算好。

为了在迭代过程中，不修改Alex-Net的权重，而只修改F，我们可以使用`tf.constant`来创建Alex-Net的各个参数，进而建立Alex-Net。这样在backward的时候，梯度就只会修正到`tf.Variable`，也就是F上。
