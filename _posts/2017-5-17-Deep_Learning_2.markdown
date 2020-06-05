---
layout: post
title:  深度学习（二）——神经元激活函数, Dropout, 深度学习常用术语解释
category: DL 
---

# BP算法

## 随机初始化（续）

3.MSRA。该方法基于零均值的Gaussian分布产生随机数。Gaussian分布的标准差为：

$$\sqrt{\frac{2}{n_l}}$$

其中，$$n_l=k_l^2d_{l-1}$$，$$k_l$$表示l层的filter的大小，$$d_{l-1}$$表示l-1层的filter的数量。

这种方法也被称作He initializer，是何恺明发明的。

>何恺明，清华本科+香港中文大学博士（2011）。先后在MS和Facebook担任研究员。   
>个人主页：http://kaiminghe.com/

何恺明在训练ResNet的时候发现Xavier方法对于ReLU激活不是太有效，故而提出了新方法。

参考：

https://pouannes.github.io/blog/initialization/

How to initialize deep neural networks? Xavier and Kaiming initialization

https://mp.weixin.qq.com/s/_wt-zTpbd25OL3os0X6cJg

神经网络中的权重初始化一览：从基础到Kaiming

https://mp.weixin.qq.com/s/Nmi4u8LKrsjYKH3_3vmaVQ

神经网络初始化trick：大神何凯明教你如何训练网络！

## BP算法的缺点

虽然传统的BP算法，理论上可以支持任意深度的神经网络。然而实际使用中，却很少能支持3层以上的神经网络。

![](/images/article/sigmoid.png)

如上图所示，sigmoid函数不是线性的，一个小的输出值的改变，对应了比较大的输入值改变。换句话说，就是输出值的梯度较大，而输入值的梯度较小。而梯度在基于梯度下降的优化问题中，是至关重要的。

随着层数的增多，反向传递的残差梯度会越来越小，这样的现象，被称作**梯度消失**（Vanishing Gradient）。它导致的结果是，虽然靠近输出端的神经网络已经训练好了，但输入端的神经网络仍处于随机状态。也就是说，靠近输入端的神经网络，有和没有都是一样的效果，完全体现不了深度神经网络的优越性。

和梯度消失相反的概念是**梯度爆炸**（Vanishing Explode），也就是神经网络无法收敛。

参考：

https://mp.weixin.qq.com/s/w7EbDI9MQBZF67XM-cV1eQ

一文了解神经网络中的梯度爆炸

## 参考

http://blog.csdn.net/xizero00/article/details/51013088

Different Methods for Weight Initialization in Deep Learning

https://mp.weixin.qq.com/s/xqWli1xnsGkqYDUjgvOnkQ

反向传播神经网络极简入门

https://mp.weixin.qq.com/s/PhxkfWH5bEbykMKGEtDScA

为什么神经网络参数不能够全部初始化为全0？

https://mp.weixin.qq.com/s/s-v7T0k2gy7ZRnrFCUpTYg

通过方差分析详解最流行的Xavier权重初始化方法

https://mp.weixin.qq.com/s/r1OJoLa_t8QwNcL4kfx5uQ

神经网络参数随机初始化已经过时了

https://mp.weixin.qq.com/s/mFA2PeO70o3HR6AQ74Lp3g

深度学习最佳实践之权重初始化

https://mp.weixin.qq.com/s/M1TswiDh-LkH9G7jCQ_UqA

神经网络编程-前向传播和后向传播

https://mp.weixin.qq.com/s/Dygdn0Xzpx40-zUQadiiHg

通过梯度检验帮助实现反向传播

https://mp.weixin.qq.com/s/auNRIPYEwRlROFXug41Ang

简单初始化，训练10000层CNN

https://mp.weixin.qq.com/s/iSJyOe81dnEuaKzOih6WNg

什么是深度学习成功的开始？参数初始化

https://mp.weixin.qq.com/s/s4ew7LYgnC9Z3kVFsjcaXg

不用批归一化也能训练万层ResNet，新型初始化方法Fixup了解一下

https://mp.weixin.qq.com/s/zTB59Fg_JFg9ZZPRlq9txA

不使用残差连接，ICML新研究靠初始化训练上万层标准CNN

https://mp.weixin.qq.com/s/_WCVvM6sPpsWD8NS8etEKA

京东AI研究院提出ScratchDet：随机初始化训练SSD目标检测器

# 神经元激活函数

## tanh函数

除了阶跃函数和Sigmoid函数之外，常用的神经元激活函数，还有双曲正切函数（tanh函数）：

$$f(z)=\tanh(x)=\frac{\sinh(x)}{\cosh(x)}=\frac{e^x-e^{-x}}{e^x+e^{-x}}$$

其导数为：

$$f'(z)=1-(f(z))^2$$

![](/images/article/sigmoid_vs_tanh.png)

上图是sigmoid函数（蓝）和tanh函数（绿）的曲线图。

![](/images/article/sigmoid_vs_tanh_2.png)

上图是sigmoid函数（蓝）和tanh函数（绿）的梯度曲线图。从中可以看出tanh函数的梯度比sigmoid函数大，因此有利于残差梯度的反向传递，这是tanh函数优于sigmoid函数的地方。但是总的来说，由于两者曲线类似，因此tanh函数仍被归类于sigmoid函数族中。

下图是一些sigmoid函数族的曲线图：

![](/images/article/Gjl-t.svg)

有关sigmoid函数和tanh函数的权威论述，参见Yann LeCun的论文：

http://yann.lecun.com/exdb/publis/pdf/lecun-98b.pdf

## 稀疏性

从数学上来看，非线性的Sigmoid函数对中央区的信号增益较大，对两侧区的信号增益小，在信号的特征空间映射上，有很好的效果。

从神经科学上来看，中央区酷似神经元的兴奋态，两侧区酷似神经元的抑制态，因而在神经网络学习方面，可以将重点特征推向中央区，将非重点特征推向两侧区。

无论是哪种解释，看起来都比早期的线性激活函数$$y=x$$,阶跃激活函数高明了不少。

2001年，Attwell等人基于大脑能量消耗的观察学习上，推测神经元编码工作方式具有稀疏性和分布性。

2003年，Lennie等人估测大脑同时被激活的神经元只有1~4%，进一步表明神经元工作的稀疏性。

从信号方面来看，即神经元同时只对输入信号的少部分选择性响应，大量信号被刻意的屏蔽了，这样可以提高学习的精度，更好更快地提取稀疏特征。

从这个角度来看，在经验规则的初始化W之后，传统的Sigmoid系函数同时近乎有一半的神经元被激活，这不符合神经科学的研究，而且会给深度网络训练带来巨大问题。

参考：

http://www.cnblogs.com/neopenx/p/4453161.html

ReLu(Rectified Linear Units)激活函数

https://en.wikipedia.org/wiki/Activation_function

## ReLU

ReLU(Rectified Linear Units)激活函数的定义如下：

$$f(x) = \max(0, x)$$

其函数曲线如下图中的蓝线所示：

![](/images/article/Rectifier_and_softplus_functions.svg)

从上图可以看出，ReLU相对于Sigmoid，在解决了梯度消失问题的同时，也增加了神经网络的稀疏性，因此ReLU的收敛速度远高于Sigmod，并成为目前最常用的激活函数。

由于ReLU的曲线不是连续可导的，因此有的时候，会用SoftPlus函数（上图中的绿线）替代。其定义为：

$$f(x) = \ln(1 + e^x)$$

除此之外，ReLU函数族还包括Leaky ReLU、PReLU、RReLU、ELU等。

# Dropout

## Dropout训练阶段

Dropout是神经网络中解决过拟合问题的一种常见方法。

它的具体做法是：

![](/images/article/DropOut.png)

1.每次训练时，随机隐藏部分隐层神经元。

2.根据样本值，修改未隐藏的神经元的参数。隐藏的神经元的参数保持不变。

3.下次训练时，重新随机选择需要隐藏的神经元。

由于神经网络的非线性，Dropout的理论证明尚属空白，这里只有一些直观解释。

1.dropout掉不同的隐藏神经元就类似在训练不同的网络，整个dropout过程就相当于对很多个不同的神经网络取平均。而不同的网络产生不同的过拟合，一些互为“反向”的拟合相互抵消就可以达到整体上减少过拟合。这实际上就是bagging的思想。

2.因为dropout程序导致两个神经元不一定每次都在一个dropout网络中出现。这会迫使网络去学习更加鲁棒的特征。换句话说，假如我们的神经网络是在做出某种预测，它不应该对一些特定的线索片段太过敏感，即使丢失特定的线索，它也应该可以从众多其它线索中学习一些共同的模式（鲁棒性）。

Dropout还有若干变种，如Annealed dropout（Dropout rate decreases by epochs）、Standout（Each neural has different dropout rate）。

除了Dropout之外，还有**DropConnect**。两者原理上类似，后者只隐藏神经元之间的连接。DropConnect也被称作Weight dropout。

总的来说，Dropout类似于机器学习中的L1、L2规则化等增加稀疏性的算法，也类似于随机森林、模拟退火之类的增加随机性的算法。

参考：

https://zhuanlan.zhihu.com/p/23178423

Dropout解决过拟合问题

https://mp.weixin.qq.com/s/WvPS9LQ6vfD4OkaRbZ0wBw

如何通过方差偏移理解批归一化与Dropout之间的冲突

https://mp.weixin.qq.com/s/yvwAPmclvgtytfhmETM_mg

Hinton提出的经典防过拟合方法Dropout，只是SDR的特例

https://mp.weixin.qq.com/s/QT7X_ubXS13X5E_5KdbNNQ

谷歌大脑提出DropBlock卷积正则化方法，显著改进CNN精度

https://mp.weixin.qq.com/s/0Go146A5dU0-RESQAaAHzQ

Dropout可能要换了，Hinton等研究者提出神似剪枝的Targeted Dropout

https://mp.weixin.qq.com/s/xmNuUPBuNo6E6XvBA8BR4Q

Dropout的前世与今生

https://mp.weixin.qq.com/s/jFz-2Bdn4dbFAza6MXtroA

Dropout的那些事

https://mp.weixin.qq.com/s/XgiWfRcIMiPUSoadMtAbIg

神经网络Dropout层中为什么dropout后还需要进行rescale？

https://mp.weixin.qq.com/s/L7DwT5LpfWoS474MwWOiLA

华为开源自研算法Disout，多项任务表现更佳

## Dropout预测阶段

经Dropout处理过的模型，在预测阶段不再Dropout，而是打开所有的神经元。这样的效果类似于集成学习，即若干个弱分类器，集成为一个强分类器。

假设p是训练时Dropout的概率。预测阶段由于所有神经元都会参与运算，这会导致$$Wx+b$$是训练阶段的$$\frac{1}{1-p}$$倍，因此需要对W进行相应修正才行，即：

$$W_{ij} \to (1-p)W_{ij}$$

这种修正在预测阶段看来是有些不方便的，因此又出现了Inverted Dropout。它的做法是训练阶段在dropout之后，接上一个除以p的rescale操作，这样的话在预测阶段，就可以忽略dropout操作了。

参考：

https://mp.weixin.qq.com/s/cP8KO3JPIn4lK-n-KdUejA

Dropout有哪些细节问题？

# 深度学习常用术语解释

## 深度学习中epoch、batch size、iterations的区别

one epoch：所有的训练样本完成一次Forword运算和BP运算。

batch size：一次Forword运算以及BP运算中所需要的训练样本数目，其实深度学习每一次参数的更新所需要损失函数并不是由一个{data：label}获得的，而是由一组数据加权得到的，这一组数据的数量就是[batch size]。当然batch size越大，所需的内存就越大，要量力而行。

iterations（迭代）：每一次迭代都是一次权重更新，每一次权重更新需要batch size个数据进行Forward运算得到损失函数，再BP算法更新参数。

最后可以得到一个公式one epoch = numbers of iterations = N = 训练样本的数量/batch size

参考：

https://zhuanlan.zhihu.com/p/83626029

浅析深度学习中Batch Size大小对训练过程的影响
