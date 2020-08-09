---
layout: post
title:  深度学习（十三）——花式池化, Regularization
category: DL 
---

* toc
{:toc}

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

# 深度推荐系统+

https://zhuanlan.zhihu.com/p/100019681

推荐系统技术演进趋势：从召回到排序再到重排

https://zhuanlan.zhihu.com/p/102918124

微信看一看Embedding

https://zhuanlan.zhihu.com/p/115781834

深度学习在花椒直播中的应用——推荐系统冷启动算法

https://mp.weixin.qq.com/s/ec88cMR4K6pWyHhJs7FEFQ

智能推荐算法在花椒直播中的应用

https://zhuanlan.zhihu.com/p/68897114

如何刻画用户的多样兴趣——MIND network阅读笔记

https://mp.weixin.qq.com/s/sbdnEMez_BKPzXOl1Z4AzQ

DeepMatch：用于推荐&广告的深度召回匹配算法库

https://zhuanlan.zhihu.com/p/127030405

对话推荐系统综述论文

https://mp.weixin.qq.com/s/kPdYAzVYelE9LxvGvi4f8w

多值类别特征加入CTR预估模型的方法汇总

https://zhuanlan.zhihu.com/p/101298495

稠密特征加入CTR预估模型有哪些方法？

https://zhuanlan.zhihu.com/p/101136699

推荐系统中的深度匹配模型

https://mp.weixin.qq.com/s/POEnU7bNG44Mz9844WuzIw

腾讯视频是如何给你高效精准推送的

https://zhuanlan.zhihu.com/p/99953120

YouTube推荐系统算法梳理

https://mp.weixin.qq.com/s/zeF7C7YrLqOjIWPen04K2Q

搜索模型核心技术公开，淘宝如何做用户建模？

https://www.zhihu.com/question/362190044

推荐系统领域有啥巧妙的idea？

https://zhuanlan.zhihu.com/p/96796043

推荐系统中如何做多目标优化

https://zhuanlan.zhihu.com/p/52876883

深度CTR预估模型中的特征自动组合机制演化简史

https://mp.weixin.qq.com/s/Ni42SEukRBGDUHu_bh3Lig

基于LSTM模型的广告库存预估算法

https://mp.weixin.qq.com/s/FYghBvkye8J7BqPje4JhFw

汽车之家推荐系统排序算法迭代之路

https://mp.weixin.qq.com/s/2whZpeQPXggHRDSEUnQZ-w

加州大学-Liwei Wu博士论文：协同过滤与排序进展，150页pdf

https://mp.weixin.qq.com/s/jIjdYIbdls5lIbF7TVfbdA

阿里又出排序新模型，还被国际顶会认可了

https://mp.weixin.qq.com/s/fUdKIqygxqlkuv0P4wiIRg

智能推荐算法在直播场景中的应用

https://mp.weixin.qq.com/s/HE0ckOe71dROafTZt-2mAA

最新边信息推荐系统综述

https://mp.weixin.qq.com/s/GazjnVwQKItrph7_n7SGTw

飞猪的“猜你喜欢”如何排序？

https://mp.weixin.qq.com/s/x4Q5di8oVAAZBJBTlY8usw

深入理解推荐系统：梳理YouTube推荐算法

https://zhuanlan.zhihu.com/p/128988454

谷歌最新双塔DNN召回模型——应用于YouTube大规模视频推荐场景

https://mp.weixin.qq.com/s/Bpw-q3wISbCAKHsuYQo0QQ

阿里DMR:融合Match中协同过滤思想的深度排序模型

https://mp.weixin.qq.com/s/D57jP5EwIx4Y1n4mteGOjQ

深度学习推荐系统中各类流行的Embedding方法（上）

https://mp.weixin.qq.com/s/N76XuNJ7yGzdP6NHk2Rs-w

深度学习推荐系统中各类流行的Embedding方法（下）

https://mp.weixin.qq.com/s/VHRV1Z6F8-3o6b-3v-5_BA

深度时空网络、记忆网络与特征表达学习在CTR预估中的应用

https://mp.weixin.qq.com/s/j34nJGomvR23ZJiqIFMoAQ

推荐系统中稀疏特征Embedding的优化表示方法

https://mp.weixin.qq.com/s/1xVPRIVwQQJfEen0RiNYvg

谈谈推荐系统中的用户行为序列建模最新进展

https://mp.weixin.qq.com/s/n5MYTk6V77Hazodw4pxnSQ

基于多任务学习和负反馈的深度召回模型

https://mp.weixin.qq.com/s/CuTEW0y7juWSBGlhltB7qw

FGCNN：使用CNN进行特征生成的CTR预测模型

https://zhuanlan.zhihu.com/p/143161957

点积 vs. MLP：推荐模型到底用哪个更好？

https://mp.weixin.qq.com/s/MSzojHCW8WPFNyF3aM1l2w

腾讯&微博 GateNet: 使用门机制提升点击率预估效果

https://zhuanlan.zhihu.com/p/162980539

2018年kdd阿里召回系统tdm读后感

# 行人重识别+

https://mp.weixin.qq.com/s/Vi_1Sg8OKG-EG4aC4QTCWA

半监督学习的新助力：无监督数据扩增法

https://mp.weixin.qq.com/s/omUtD3GFOpP1dvfWZgLDww

计算机视觉模型效果不佳，你可能是被相机的Exif信息坑了

https://mp.weixin.qq.com/s/pV7C2sSJwP3rBO6OYeF-nw

基于马尔可夫链的数据增强

https://mp.weixin.qq.com/s/6yfHwsk-fTEtQhrciMEBug

重识别（re-ID）特征适合直接用于跟踪（tracking）问题么？

https://mp.weixin.qq.com/s/Q34wjziJBBOrb1VPhQJK8g

行人重识别简介

https://mp.weixin.qq.com/s/OALGxuvUdQMbsK1k4g2V7Q

遮挡也能识别？地平线提出用时序信息提升行人检测准确度

https://mp.weixin.qq.com/s/2v-Y_-_si6_dxq3cICy-lg

疫情蔓延让这项CV技术突然火了，盘点开源代码

https://mp.weixin.qq.com/s/Z6l9R5uzWsAn_DTgTsGstg

京东发布FastReID：目前最强悍的目标重识别开源库！
