---
layout: post
title:  深度学习（七）——LSTM, 神经元激活函数进阶
category: DL 
---

* toc
{:toc}

# RNN

## RNN的历史（续）

>Harvard College是Harvard University最古老的本部，目前一般提供本科教育。它和其他许多研究生院以及相关部门，共同组成了Harvard University。类似的还有Yale College和Yale University。

>American Academy of Arts and Sciences建于1780年。当时，美国正在法国等国的协助下与英国作战，所以美国的创立者选择比照包括作家、人文学者、科学家、军事家、政治家在内的法兰西学术院，建立新大陆的学术院。   
>后来，林肯总统比照英国皇家学会，于1863年创建了主要涵盖自然科学的National Academy of Sciences，United States。   
>这两个学院是美国学术界最权威的组织。前者的重要度略高于后者。

>美国的创立者，一般被翻译为Founding Fathers of the United States。此外还有一个更响亮的称号76ers。没错，NBA那支球队的名字就是这么来的。

除了Elman RNN之外，还有Jordan RNN。（没错，这就是吴恩达的导师的作品）

$$\begin{align}
h_t &= \sigma_h(W_{h} x_t + U_{h} y_{t-1} + b_h) \\
y_t &= \sigma_y(W_{y} h_t + b_y)
\end{align}$$

Elman RNN的记忆来自于隐层单元，而Jordan RNN的记忆来自于输出层单元。

## 参考

http://blog.csdn.net/aws3217150/article/details/50768453

递归神经网络(RNN)简介

http://blog.csdn.net/heyongluoyao8/article/details/48636251

循环神经网络(RNN, Recurrent Neural Networks)介绍

http://mp.weixin.qq.com/s?__biz=MzIzODExMDE5MA==&mid=2694182661&idx=1&sn=ddfb3f301f5021571992824b21ddcafe

循环神经网络

http://www.wildml.com/2015/10/recurrent-neural-networks-tutorial-part-3-backpropagation-through-time-and-vanishing-gradients/

Backpropagation Through Time算法

https://baijia.baidu.com/s?old_id=560025

Tomas Mikolov详解RNN与机器智能的实现

https://sanwen8.cn/p/3f8sRTh.html

为什么RNN需要做正交初始化？

http://blog.csdn.net/shenxiaolu1984/article/details/71508892

RNN的梯度消失/爆炸与正交初始化

https://mp.weixin.qq.com/s/vHQ1WbADHAISXCGxOqnP2A

看大牛如何复盘递归神经网络！

https://mp.weixin.qq.com/s/0V9DeG39is_BxAYX0Yomww

为何循环神经网络在众多机器学习方法中脱颖而出？

https://mp.weixin.qq.com/s/-Am9Z4_SsOc-fZA_54Qg3A

深度理解RNN：时间序列数据的首选神经网络！

https://mp.weixin.qq.com/s/ztIrt4_xIPrmCwS1fCn_dA

“魔性”的循环神经网络

https://mp.weixin.qq.com/s/BqVicouktsZu8xLVR-XnFg

完全图解RNN、RNN变体、Seq2Seq、Attention机制

https://mp.weixin.qq.com/s/gGGXKT2fTn2xPPvo7PE8IA

像训练CNN一样快速训练RNN：全新RNN实现，比优化后的LSTM快10倍

https://mp.weixin.qq.com/s/OltT-GFDVxaiukb1HVSY3w

通俗讲解循环神经网络的两种应用

https://mp.weixin.qq.com/s/PZMmjT9eXL7rU2pxkQWTiw

从90年代的SRNN开始，纵览循环神经网络27年的研究进展

https://mp.weixin.qq.com/s/7LcqRGPYX6JXpY_0hbjmbA

循环神经网络(RNN)入门帖：向量到序列，序列到序列，双向RNN，马尔科夫化

# LSTM

本篇笔记主要摘自：

http://www.jianshu.com/p/9dc9f41f0b29

理解LSTM网络

## LSTM结构图

为了解决原始RNN只有短时记忆的问题，人们又提出了一个RNN的变种——LSTM（Long Short-Term Memory）。其结构图如下所示：

![](/images/article/LSTM.png)

和RNN的时序展开图类似，这里的每个方框表示**某个时刻从输入层到隐层的映射**。

我们首先回顾一下之前的模型在这里的处理。

MLP的该映射关系为：

$$h=\sigma (W\cdot x+b)$$

RNN在上式基础上添加了历史状态$$h_{t-1}$$：

$$h_t=\sigma (W\cdot [h_{t-1},x_t]+b)$$

LSTM不仅添加了历史状态$$h_{t-1}$$，还添加了所谓的**细胞状态**$$C_{t-1}$$，即上图中图像上部的水平横线。

## 步骤详解

神经网络的设计方式和其他算法不同，我们不需要指定具体的参数，而只需要给出一个功能的实现机制，然后借助误差的反向传播算法，训练得到相应的参数。这一点在LSTM上体现的尤为明显。

LSTM主要包括以下4个步骤（也可称为4个功能或门）：

- 决定丢弃信息

![](/images/article/LSTM_1.png)

这一部分也被称为**忘记门**。

- 确定更新的信息

![](/images/article/LSTM_2.png)

这一部分也被称为**输入门**。

- 更新细胞状态

![](/images/article/LSTM_3.png)

- 输出信息

![](/images/article/LSTM_4.png)

显然，在这里不同的参数会对上述4个功能进行任意组合，从而最终达到长时记忆的目的。

>上面的公式中，$$\cdot$$表示Dot product，而$$*$$表示eltwise product。

### 其他细节

在一般的神经网络中，激活函数可以随意选择，无论是传统的sigmoid，还是新的tanh、ReLU，都不影响模型的大致效果。（差异主要体现在训练的收敛速度上，最终结果也可能会有细微影响。）

**但是，上述标准LSTM模型中，tanh函数可以随意替换，而sigmoid函数却不能被替换，切记。**

sigmoid用在了各种gate上，产生0~1之间的值，这个一般只有sigmoid最直接了。

tanh用在了状态和输出上，是对数据的处理，这个用其他激活函数也可以。

forget bias的初始值可以设为以1为均值，这对于训练很有好处，这就是tensorflow中forget_bias参数的来历。参见论文：

《An Empirical Exploration of Recurrent Network Architectures》

## LSTM的变体

![](/images/article/LSTM_5.png)

上图中的LSTM变体被称为**peephole connection**。其实就是将细胞状态加入各门的输入中。可以全部添加，也可以部分添加。

![](/images/article/LSTM_6.png)

上图中的LSTM变体被称为**Coupled Input and Forget Gate（CIFG）**。它将忘记和输入门连在了一起。

![](/images/article/LSTM_7.png)

上图是一个改动较大的变体**Gated Recurrent Unit（GRU）**。它将忘记门和输入门合成了一个单一的更新门。同样还混合了细胞状态和隐藏状态，和其他一些改动。最终的模型比标准的 LSTM 模型要简单，也是非常流行的变体。

## 参考

http://www.csdn.net/article/2015-06-05/2824880

深入浅出LSTM神经网络

https://mp.weixin.qq.com/s/y2kV4ye2zr1HYvZd3APeWA

难以置信！LSTM和GRU的解析从未如此清晰

https://blog.csdn.net/zhangxb35/article/details/70060295

RNN, LSTM, GRU公式总结

https://zhuanlan.zhihu.com/p/25821063

循环神经网络——scan实现LSTM

http://blog.csdn.net/a635661820/article/details/45390671

LSTM简介以及数学推导(FULL BPTT)

http://blog.csdn.net/dark_scope/article/details/47056361

RNN以及LSTM的介绍和公式梳理

https://mp.weixin.qq.com/s/x3y9WTuVFYQb60eJvw02HQ

如何解决LSTM循环神经网络中的超长序列问题

https://mp.weixin.qq.com/s/IhCfoabRrtjvQBIQMaPpNQ

从任务到可视化，如何理解LSTM网络中的神经元

https://mp.weixin.qq.com/s/GGpaFZ0crP_NQ564d79hFw

LSTM、GRU与神经图灵机：详解深度学习最热门的循环神经网络

https://mp.weixin.qq.com/s/0bBTVjkfAK2EzQiaFcUjBA

LSTM入门必读：从基础知识到工作方式详解

https://mp.weixin.qq.com/s/jcS4IX7LKCt1E2FVzLWzDw

LSTM入门详解

https://mp.weixin.qq.com/s/MQR7c57NL4b5i4MRA2JgWA

用Python实现CNN长短期记忆网络！

http://mp.weixin.qq.com/s/V2-grLPdZ66FOiC2duc-EA

如何判断LSTM模型中的过拟合与欠拟合

http://blog.csdn.net/malefactor/article/details/51183989

深度学习计算模型中“门函数（Gating Function）”的作用

https://mp.weixin.qq.com/s/ORLpqqV8pOv-pIagi8yS1A

在调用API之前，你需要理解的LSTM工作原理

https://mp.weixin.qq.com/s/BzlFbweHEJ3z7dSIGmd-QA

深度学习基础之LSTM

https://mp.weixin.qq.com/s/lbHTDdzPbYn2Ln4aihGujQ

人人都能看懂的LSTM

https://mp.weixin.qq.com/s/LI6TsPjzIaa8DxDu3UaV1A

门控循环单元（GRU）的基本概念与原理

https://mp.weixin.qq.com/s/LcdmXgAFpiIoHMIIXECC9g

人人都能看懂的GRU

https://mp.weixin.qq.com/s/m_cOjUHwvW496Gv9_aYgpA

LSTM和循环神经网络基础教程

https://blog.csdn.net/taoqick/article/details/79475350

学会区分RNN的output和state

# 神经元激活函数进阶

在《深度学习（二）》中，我们探讨了ReLU相对于sigmoid函数的改进，以及一些保证深度神经网络能够训练的措施。然而即便如此，深度神经网络的训练仍然是一件非常困难的事情，还需要更多的技巧和方法。

![](/images/img4/activation.png)

## 激活函数的作用

神经网络中激活函数的主要作用是提供网络的**非线性建模能力**，如不特别说明，激活函数一般而言是非线性函数。

假设一个神经网络中仅包含线性卷积和全连接运算，那么该网络仅能够表达线性映射，即便增加网络的深度也依旧还是线性映射，难以有效建模实际环境中非线性分布的数据。

加入非线性激活函数之后，深度神经网络才具备了分层的非线性映射学习能力。因此，激活函数是深度神经网络中不可或缺的部分。

>注意：其实也有采用线性激活函数的神经网络，亦被称为linear neurons。但是这些神经网络，基本只有学术价值而无实际意义。

理论上来说，只要是非线性函数，都有做激活函数的可能性。然而不同的激活函数其训练成本是不同的。

虽然OpenAI的探索表明，连浮点误差都可以做激活函数，但是由于这个操作的不可微分性，因此他们使用了“进化策略”来训练模型，所谓“进化策略”，是诸如遗传算法之类的耗时耗力的算法。

参考：

https://mp.weixin.qq.com/s/d9XmDCahK6UBlYWhI0D5jQ

深度线性神经网络也能做非线性计算，OpenAI使用进化策略新发现

https://mp.weixin.qq.com/s/PNe2aKVMYjV_Nd7qZwGuOw

理解激活函数作用，看这篇文章就够了！

## ReLU的缺点

深度神经网络的训练问题，最早是2006年Hinton使用**分层无监督预训练**的方法解决的，然而该方法使用起来很不方便。

而深度网络的**直接监督式训练**的最终突破，最主要的原因是采用了新型激活函数ReLU。

但是ReLU并不完美。它在x<0时硬饱和，而当x>0时，导数为1。所以，ReLU能够在x>0时保持梯度不衰减，从而缓解梯度消失问题。但随着训练的推进，部分输入会落入硬饱和区，导致对应权重无法更新。这种现象被称为**神经元死亡**。
