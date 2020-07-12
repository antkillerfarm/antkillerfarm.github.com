---
layout: post
title:  深度学习（十三）——花式池化, Regularization, 深度信息检索
category: DL 
---

# 花式卷积（续）

https://mp.weixin.qq.com/s/ybI8kJPRn7sH-hJbc5uqnw

CMU研究者探索新卷积方法：在实验中可媲美基准CNN

https://zhuanlan.zhihu.com/p/46633171

深度卷积神经网络中的降采样

https://mp.weixin.qq.com/s/1gBC-bp4Q4dPr0XMYPStXA

万字长文带你看尽深度学习中的各种卷积网络

https://mp.weixin.qq.com/s/qReN6z8s45870HSMCMNatw

微软亚洲研究院：逐层集中Attention的卷积模型

http://blog.csdn.net/shuzfan/article/details/77964370

不规则卷积神经网络

https://mp.weixin.qq.com/s/rXr_XBc2Psh3NSA0pj4ptQ

常建龙：深度卷积网络中的卷积算子研究进展

https://mp.weixin.qq.com/s/i8vOeAVEYX-hRAvPSe6DEA

一文看尽神经网络中不同种类的卷积层

https://mp.weixin.qq.com/s/hZc8MgHoE010hnzLU-trIA

高性能涨点的动态卷积DyNet与CondConv、DynamicConv有什么区别联系？

https://www.yuque.com/yahei/hey-yahei/condconv

CondConv：按需定制的卷积权重

https://mp.weixin.qq.com/s/eRZ3jNuceMYKE3lEj-g1aw

动态卷积：自适应调整卷积参数，显著提升模型表达能力

https://mp.weixin.qq.com/s/_GOXBYyyYnridILemNRDqA

ChannelNets: 省力又讨好的channel-wise卷积，在channel维度进行卷积滑动 

# 花式池化

池化和卷积一样，都是信号采样的一种方式。

## 普通池化

池化的一般步骤是：选择区域P，令$$Y=f(P)$$。这里的f为池化函数。

![](/images/article/max_pooling.png)

上图是Max Pool的示意图，也就是选择池子里最大的那个值。

除了max之外，常用的池化函数还有：

Min Pool:

$$Y=\min(P)$$

Average Pool:

$$Y=\text{mean}(P)$$

L2 Pool:

$$Y=\sqrt{\frac{\sum p^2}{n}}$$

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

Global Average Pool是另一类池化操作，一般用于替换FullConnection层。

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

GAP实际上就是kernel size等于WxH的AP。类似的，还有Global Max Pool。

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

https://mp.weixin.qq.com/s/K1RBux3AfxVFT8_uezYHFA

被Hinton，DeepMind和斯坦福嫌弃的池化，到底是什么？

https://mp.weixin.qq.com/s/J4opJ6NvbTxbHWAWNHEltw

自然语言处理中CNN模型几种常见的Max Pooling操作

# Regularization

DL中的Regularization除了常见的$$l_1$$-norm、$$l_2$$-norm和squared $$l_2$$-norm之外，还有Group Regularization。它的定义如下：

$$loss(W;x;y) = loss_D(W;x;y) + \lambda_R R(W) + \lambda_g \sum_{l=1}^{L} R_g(W_l^{(G)})$$

$$R_g(w^{(g)}) = \sum_{g=1}^{G} \lVert w^{(g)} \rVert_g = \sum_{g=1}^{G} \sum_{i=1}^{|w^{(g)}|} {(w_i^{(g)})}^2$$

Group Regularization也叫做Block Regularization或Structured Regularization。

# 深度信息检索

Information Retrieval是用户进行信息查询和获取的主要方式，是查找信息的方法和手段。狭义的信息检索仅指信息查询（Information Search）。即用户根据需要，采用一定的方法，借助检索工具，从信息集合中找出所需要信息的查找过程。广义的信息检索是信息按一定的方式进行加工、整理、组织并存储起来，再根据信息用户特定的需要将相关信息准确的查找出来的过程。

这方面的DL应用可参见以下的综述文章：

《MatchZoo: A Toolkit for Deep Text Matching》

## ARC-I & ARC-II

《Convolutional neural network architectures for matching natural language sentences》

## DSSM

论文：

《Learning deep structured semantic models for web search using clickthrough data》

### Word Hashing

DSSM已经意识到one-hot是一种低效的词向量表示方式，因此，它转而采用了一种叫做Word Hashing的技术。

Word Hashing是非常重要的一个trick，以英文单词来说，比如good，它可以写成`#good#`，然后按tri-grams来进行分解为`#go goo ood od#`，再将这个tri-grams灌入到bag-of-word中，这种方式可以非常有效的解决vocabulary太大的问题(因为在真实的web search中vocabulary就是异常的大)，另外也不会出现oov问题，因此英文单词才26个，3个字母的组合都是有限的，很容易枚举光。

那么问题就来了，这样两个不同的单词会不会产出相同的tri-grams，paper里面做了统计，说了这个冲突的概率非常的低，500K个word可以降到30k维，冲突的概率为0.0044%。

但是在中文场景下，这个Word Hashing估计没有这么有效了。

上面讲述了Word Hashing的词向量的构建方法，这种方法也可以扩展到句子：统计一下句子中每个tri-grams出现的次数，然后用次数组成句子向量即可。

### 网络结构

![](/images/img3/DSSM.png)

上图是DSSM的网络结构：句子向量经过若干层的神经网络之后，得到了语义向量（semantic concept vectors）。计算两个语义向量的cos相似度，得到两个句子的匹配程度。

### 参考

https://www.microsoft.com/en-us/research/project/dssm/

微软的DSSM模型

https://www.cnblogs.com/baiting/p/7195998.html

深度语义匹配模型-DSSM及其变种

https://blog.csdn.net/u013074302/article/details/76422551

语义相似度计算——DSSM

https://mp.weixin.qq.com/s/U2r4qDLh4WZFgAIoF_SRPg

金融客服AI新玩法：语言学运用、LSTM+DSSM算法、多模态情感交互

https://mp.weixin.qq.com/s/HMhvcAYRatN0n5EiiNVoXQ

详解深度语义匹配模型DSSM

https://www.cnblogs.com/guoyaohua/p/9229190.html

DSSM：深度语义匹配模型（及其变体CLSM、LSTM-DSSM）

https://zhuanlan.zhihu.com/p/141545370

深度学习语义相似度系列：Ranking Similarity

https://mp.weixin.qq.com/s/2UJQkPuhvFYT2jmuxLJ8VA

实践篇：语义匹配在贝壳找房智能客服中的应用

https://mp.weixin.qq.com/s/6cTG9347dOLxiUs_-tdpnQ

百度开源：语义匹配应用介绍和源代码

https://mp.weixin.qq.com/s/PxyazOPKV3eB-qat8hM9ZQ

神经网络语义匹配技术

https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650749368&idx=3&sn=fc20d9e6682c74227282df3133cea06c

基于IR-transformer、IRGAN模型，解读搜狗语义匹配技术

https://mp.weixin.qq.com/s/Xnea50Eisq9rzhGFa1iTFA

DRr-Net：基于动态重读机制的句子语义匹配方法

## 表示型和交互型

从模型的本质来看可以分为两种类型：表示型和交互型。表示型的模型会在最后一层对待匹配的两个句子进行相似度计算，交互型模型会尽早的让两个句子交互，充分应用交互特征。

https://mp.weixin.qq.com/s/5_QqP3CIBpM4zM5CWIxYuA

深度语义匹配模型原理篇一：表示型

## CDSSM

《Learning semantic representations using convolutional neural networks for web search》

## MV-LSTM

《A deep architecture for semantic matching with multiple positional sentence representations》

## CNTN

《Convolutional Neural Tensor Network Architecture for Community-Based Question Answering》

## DRMM

《A deep relevance matching model for ad-hoc retrieval》

## MatchPyramid

《Text Matching as Image Recognition》

## Match-SRNN

《Match-SRNN: Modeling the Recursive Matching Structure with Spatial RNN》

## K-NRM

《End-to-End Neural Ad-hoc Ranking with Kernel Pooling》

## 代码搜索

https://mp.weixin.qq.com/s/B3Uv-dhB5VYJnu06N4lYBg

深度学习遇见代码搜索，一篇论文概览神经代码搜索

https://mp.weixin.qq.com/s/GFIxA9kEGNJ9rg96mRw0PQ

自然语言语义代码搜索之路

## 参考

https://github.com/harpribot/awesome-information-retrieval

信息检索优质资源汇总

https://mp.weixin.qq.com/s/5ba3EM6e9R-i3UpzUhm49w

神经信息检索导论，微软研究员129页最新书册
