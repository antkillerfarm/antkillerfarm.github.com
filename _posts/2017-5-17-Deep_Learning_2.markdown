---
layout: post
title:  深度学习（二）——神经元激活函数
category: DL 
---

* toc
{:toc}

# BP算法

## 链式法则

Chain Rules本来是微积分中，用于求一个复合函数导数的常用法则。这里用来进行残差梯度的逆传播。

由《机器学习（一）》的公式3可得：

$$\Delta w_{hj}=-\eta\frac{\partial E_k}{\partial w_{hj}}$$

$$w_{hj}$$先影响$$\beta_j$$，再影响$$\hat y_j^k$$，然后影响误差$$E_k$$，因此有：

$$\frac{\partial E_k}{\partial w_{hj}}=\frac{\partial E_k}{\partial \hat y_j^k}\cdot \frac{\partial \hat y_j^k}{\partial \beta_j}\cdot \frac{\partial \beta_j}{\partial w_{hj}}\tag{1}$$

![](/images/article/chain_rule.png)

![](/images/img4/chain_rule.png)

值得注意的是残差梯度实际上包括两部分：$$\Delta x$$和$$\Delta w$$。如下图所示：

![](/images/img2/BP.png)

其中，$$\Delta x$$和$$\Delta w$$分别是$$\Delta$$在x和w的偏导数方向上的分量。$$\Delta x$$用于向上层传递梯度，而$$\Delta w$$用于更新权值w。

通常来说，我们只需要更新权值w，但少数情况下，w和x可能都需要更新，这时只要分别计算w和x的偏导，并更新即可。

![](/images/img4/BP.png)

上图是多层MLP的正反向运算关系图。z表示每层的feature map，w表示weight，g表示gradients。上图上半部分展示了正向运算，而下半部分，左侧展示了gradients的更新，右侧展示了weight的更新。

正向计算：

$$\begin{align}& z_{i+1} = z_{i} \cdot w_{i+1},
\\&z_{i} \in R^{M \times K}, w_{i+1}  \in R^{K \times N}, z_{i+1} \in R^{M \times N}\end{align}$$

Backprop Filter：

$$\begin{align}&\Delta w_{i+1} = z_{i}^T \cdot g_{i+1},
\\& g_{i+1} \text{ is as the same shape as } z_{i+1}\end{align}$$

Backprop Input：

$$g_{i} = g_{i+1} \cdot w_{i+1}^T$$

除了基于梯度下降的BP算法之外，还有基于GA（genetic algorithm）的BP算法，但基本只有学术界还在尝试。

参考：

https://mp.weixin.qq.com/s/A_Sekyi1kxT1zYcQFBOkDA

Quickprop介绍：一个加速梯度下降的学习方法（由于80/90年代的BP算法收敛缓慢，Scott Fahlman发明了一种名为Quickprop的学习算法。）

https://zhuanlan.zhihu.com/p/25202034

道理我都懂，但是神经网络反向传播时的梯度到底怎么求？

https://mp.weixin.qq.com/s/Ub3CMQszkx7pGKoPcB0bYA

BP反向传播矩阵推导图示详解​

https://tech.zealscott.com/deeplearning/11-785/lecture12/

Back propagation through a CNN

https://mp.weixin.qq.com/s/XzyudySeceDueTDM36Q2Wg

学好偏导

https://mp.weixin.qq.com/s/MX8sFKJQZMDMHwkBJ8mCtQ

5种神经网络常见的求导

## 随机初始化

神经网络的参数的**随机初始化**的目的是使对称失效。否则的话，所有对称结点的权重都一致，也就无法区分并学习了。

随机初始化的方法有如下几种：

1.Gaussian。用给定均值和方差的Gaussian分布设定随机值。这也是最常用的方法。

2.Xavier。该方法基于Gaussian分布或均匀分布产生随机数。其中分布W的均值为零，方差公式如下：

$$\text{Var}(W)=\frac{1}{n_{in}}\tag{1}$$

其中，$$n_{in}$$表示需要输入层的神经元的个数。也有如下变种：

$$\text{Var}(W)=\frac{2}{n_{in}+n_{out}}\tag{2}$$

其中，$$n_{out}$$表示需要输出层的神经元的个数。

公式1也被称作LeCun initializer，公式2也被称作Glorot initializer。

3.MSRA。该方法基于零均值的Gaussian分布产生随机数。Gaussian分布的标准差为：

$$\sqrt{\frac{2}{n_l}}$$

其中，$$n_l=k_l^2d_{l-1}$$，$$k_l$$表示l层的filter的大小，$$d_{l-1}$$表示l-1层的filter的数量。

这种方法也被称作He initializer，是何恺明发明的。

>何恺明，清华本科+香港中文大学博士（2011）。先后在MS和Facebook担任研究员。   
>个人主页：http://kaiminghe.com/

何恺明在训练ResNet的时候发现Xavier方法对于ReLU激活不是太有效，故而提出了新方法。

除了随机初始化之外，还有**预训练初始化**。比较早期的方法是使用greedy layerwise auto-encoder做无监督学习的预训练，经典代表为Deep Belief Network；而现在更为常见的是有监督的预训练+模型微调。

参考：

https://pouannes.github.io/blog/initialization/

How to initialize deep neural networks? Xavier and Kaiming initialization

https://mp.weixin.qq.com/s/_wt-zTpbd25OL3os0X6cJg

神经网络中的权重初始化一览：从基础到Kaiming

https://mp.weixin.qq.com/s/Nmi4u8LKrsjYKH3_3vmaVQ

神经网络初始化trick：大神何凯明教你如何训练网络！

https://mp.weixin.qq.com/s/DCYusE1lwvm14qpsnPYMpw

初始化：你真的了解我吗？

https://zhuanlan.zhihu.com/p/305055975

kaiming初始化的推导

## BP算法的缺点

虽然传统的BP算法，理论上可以支持任意深度的神经网络。然而实际使用中，却很少能支持3层以上的神经网络。

![](/images/article/sigmoid.png)

如上图所示，sigmoid函数的特点是左端趋近于0，右端趋近于1，两端都趋于饱和（饱和的定义是导数很接近0）。一个小的输出值的改变，对应了比较大的输入值改变。换句话说，就是输出值的梯度较大，而输入值的梯度较小。而梯度在基于梯度下降的优化问题中，是至关重要的。

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
