---
layout: post
title:  深度学习（十一）——花式卷积（2）, 元学习
category: DL 
---

# 花式卷积（续）

## Separable convolution

前面介绍的都是正方形的卷积核，实际上长条形的卷积核也是很常用的。比如可分离卷积。

我们知道卷积的计算量和卷积核的面积成正比。对于k x k的卷积核K来说，计算复杂度就是$$O(k^2)$$。

如果我们能找到1 x k的卷积核H和k x 1的卷积核V，且$$K = V * H$$，则称K是可分离的卷积核。

根据卷积运算满足结合律，可得：

$$f * K = f * (V * H) = f * V * H$$

这样就将一个k x k的卷积运算，转换成1 x k + k x 1的卷积运算，从而大大节省了参数和计算量。

显然，不是所有的卷积核都满足可分离条件。但是不要紧，NN有自动学习并逼近函数的能力。经过训练之后：$$K \approx V * H$$

## 1 x 1卷积

1、升维或降维。

如果卷积的输出输入都只是一个平面，那么1x1卷积核并没有什么意义，它是完全不考虑像素与周边其他像素关系。 但卷积的输出输入是长方体，所以1 x 1卷积实际上是对每个像素点，在不同的channels上进行线性组合（信息整合），且保留了图片的原有平面结构，调控depth，从而完成升维或降维的功能。

![](/images/article/conv_1x1.png)

2、加入非线性。卷积层之后经过激励层，1 x 1的卷积在前一层的学习表示上添加了非线性激励（non-linear activation），提升网络的表达能力；

3.促进不同通道之间的信息交换。

参考：

https://www.zhihu.com/question/56024942

卷积神经网络中用1x1卷积有什么作用或者好处呢？

## depthwise separable convolution

在传统的卷积网络中，卷积层会同时寻找跨空间和跨深度的相关性。

然而Xception指出：跨通道的相关性和空间相关性是完全可分离的，最好不要联合映射它们。

>Xception是Francois Chollet于2016年提出的。

![](/images/article/Xception.png)

上图是Xception中的卷积运算depthwise separable convolution的示意图。

它包含一个深度方面的卷积（一个为每个通道单独执行的空间卷积，depthwise convolution），后面跟着一个逐点的卷积（一个跨通道的1×1卷积，pointwise convolution）。我们可以将其看作是首先求跨一个2D空间的相关性，然后再求跨一个1D空间的相关性。可以看出，这种2D+1D映射学起来比全 3D 映射更加简单。

在ImageNet数据集上，Xception的表现稍稍优于Inception v3，而且在一个有17000类的更大规模的图像分类数据集上的表现更是好得多。而它的模型参数的数量仅和Inception一样多。

上面展示的是1个input channel对应1个output channel的depthwise convolution，实际使用中，也可以1个input channel对应N个output channel，这里的N一般被称作multipler参数。

更一般的，如果是N个input channel对应M个output channel的话，就是之前介绍过的Grouped Convolution了。

论文：

《Xception: Deep Learning with Depthwise Separable Convolutions》

代码：

https://github.com/fchollet/keras/blob/master/keras/applications/xception.py

>Francois Chollet，法国人。现为Google研究员。Keras的作者。

参考：

http://blog.csdn.net/mao_xiao_feng/article/details/78003476

tf.nn.depthwise_conv2d如何实现深度卷积?

http://blog.csdn.net/mao_xiao_feng/article/details/78002811

tf.nn.separable_conv2d如何实现深度可分卷积?

https://blog.csdn.net/tintinetmilou/article/details/81607721

Depthwise卷积与Pointwise卷积

## 感受野

Receptive Field本来是神经科学领域的概念，后来才被推广到DL（尤其是CNN）领域。

Receptive Field的大小实际上就是采样范围的大小，例如一个kernel为9x9，stride为1的普通卷积，其采样范围为13x13。（即kernel size+pad size, 9+4=13）

其他卷积的情况，可以依此类推。

对于多层神经网络的感受野，一般用Layer N上的一个点，在Input中的采样范围表示。所以层数越多，感受野越大。

需要注意的是，感受野中心的点，由于几乎每层卷积计算都会被采样到，因此它们的采样率是大于边缘点的。换句话说，就是对结果有更大的影响。因此，这又引入了**有效感受野**的概念。从实践角度来看，有效感受野的半径通常为感受野半径的1/3～1/5。

参考：

https://mp.weixin.qq.com/s/R8rEngNI31w0DQwjjeS6kw

关于感受野的总结

https://zhuanlan.zhihu.com/p/44106492

卷积神经网络的感受野

## 参考

https://github.com/vdumoulin/conv_arithmetic

Convolution arithmetic

http://deeplearning.net/software/theano_versions/dev/tutorial/conv_arithmetic.html

Convolution arithmetic

https://mp.weixin.qq.com/s/dR2nhGqpz7OdmxKPYSaaxw

如何理解空洞卷积（dilated convolution）？

https://mp.weixin.qq.com/s/CLFbhWMcat4rN8YS_7q25g

这12张图生动的告诉你，深度学习中的卷积网络是怎么一回事？

https://mp.weixin.qq.com/s/kJEeKzC9pC375EjIJpTuzg

一文全解深度学习中的卷积

http://cs.nyu.edu/~fergus/drafts/utexas2.pdf

Deconvolutional Networks

https://zhuanlan.zhihu.com/p/22245268

CNN-反卷积

http://buptldy.github.io/2016/10/29/2016-10-29-deconv/

Transposed Convolution, Fractionally Strided Convolution or Deconvolution（中文blog）

https://mp.weixin.qq.com/s/ybI8kJPRn7sH-hJbc5uqnw

CMU研究者探索新卷积方法：在实验中可媲美基准CNN

https://zhuanlan.zhihu.com/p/46633171

深度卷积神经网络中的降采样

# ESN

Echo State Network

https://blog.csdn.net/zwqhehe/article/details/77025035

回声状态网络(ESN)原理详解

https://mp.weixin.qq.com/s/tjawT2-bhPrit0Fd4knSgA

基于回声状态网络预测股票价格

# 元学习

人工智能的历史显示了明确的进展方向：

第一代：**良好的老式人工智能**

>手工预测   
>什么都不学

第二代：**浅学习**

>手工功能   
>学习预测

第三代：**深度学习**

>手工算法（优化器，目标，架构......）   
>端到端地学习功能和预测

第四代：**元学习**

>无手工   
>端到端学习算法和功能以及预测

## 参考

https://github.com/gopala-kr/meta-learning

元学习（meta-learning）相关文献资源大列表

https://github.com/sudharsan13296/Awesome-Meta-Learning

元学习相关资源汇总

https://mp.weixin.qq.com/s/KKK3VEpwL90g6Aro8qtXxQ

学习如何学习的算法：简述元学习研究方向现状

https://mp.weixin.qq.com/s/qoKQwEvOnP384i5Z-_jO1A

CVPR2019最新元学习教程：基于元学习的计算机视觉应用

https://mp.weixin.qq.com/s/KtO3OTZ-bZ6m0ZSI6jTyjw

OpenAI提出Reptile：可扩展的元学习算法

https://mp.weixin.qq.com/s/T4GiL9vW7ALOzWloE_QQBA

OpenAI开发可拓展元学习算法Reptile，能快速学习

https://mp.weixin.qq.com/s/MWcoGsQJg1GBbSqzyPD9uQ

基于梯度的元学习算法，可高效适应非平稳环境

https://zhuanlan.zhihu.com/p/35695477

基于Meta Learning在动态竞争环境中实现策略自适应

https://mp.weixin.qq.com/s/AhadWUjtgsFmb8uTylTvqg

OpenAI提出新型元学习方法EPG，调整损失函数实现新任务上的快速训练

https://mp.weixin.qq.com/s/dmRdp2oMn0vGukclJSVZDg

Uber AI论文：利用反向传播训练可塑神经网络，生物启发的元学习范式

https://mp.weixin.qq.com/s/Cc4EHc6ei-PtZWhewM10xw

学习如何学习的算法：简述元学习研究方向现状

https://mp.weixin.qq.com/s/4f6-gXovdrYk7240TrUwJg

谷歌大脑：基于元学习的无监督学习更新规则

https://mp.weixin.qq.com/s/cAbMB-DB9vu2ua8t5J28ww

从零开始，了解元学习

https://mp.weixin.qq.com/s/Q36vpS1HF2IfeCsFLh656A

基于元强化学习的神经科学新理论

https://mp.weixin.qq.com/s/XtzvHOk7CdXRBy02kUmgsg

近期爆火的Meta Learning，遗传算法与深度学习的火花，再不了解你就out了

https://mp.weixin.qq.com/s/KvgYyuyICueNQPo_S27fEA

BAIR展示新型模仿学习，学会像人那样执行任务

https://zhuanlan.zhihu.com/p/41223529

最前沿:Meta RL论文解读

https://zhuanlan.zhihu.com/p/40600485

最前沿：Meta Learning前沿进展扫描

https://zhuanlan.zhihu.com/p/28639662

百家争鸣的Meta Learning/Learning to learn

https://zhuanlan.zhihu.com/p/45845001

最前沿：用模仿学习来学习增强学习

https://zhuanlan.zhihu.com/p/46059552

Meta Learning单排小教学

https://zhuanlan.zhihu.com/p/46131981

最前沿：Meta Learning在少样本文本翻译上的应用

https://zhuanlan.zhihu.com/p/46339823

谈谈无监督Meta Learning的研究

https://zhuanlan.zhihu.com/p/46340382

ICLR19最新论文解读之Meta Domain Adaptation

https://mp.weixin.qq.com/s/RBMGI20AI92ZcWSlYczqAA

伯克利、OpenAI等提出基于模型的元策略优化强化学习

https://mp.weixin.qq.com/s/p0dcov84pZqsU7XP30bexQ

Meta-Learning元学习：学会快速学习

https://mp.weixin.qq.com/s/wl8j7dLu3OxPV7MNaO2-7Q

《基于梯度的元学习》199页伯克利博士论文带你回顾元学习最新发展脉络

https://mp.weixin.qq.com/s/ftiGPBhAx5iqlW_Ltg1yhg

《元监督视觉学习》132页伯克利博士论文带你回顾元监督视觉应用最新发展脉络

https://mp.weixin.qq.com/s/K7sLM-LMcF6-gQrV1ddrDw

让智能体主动交互，DeepMind提出用元强化学习实现因果推理

https://mp.weixin.qq.com/s/8sBXlnXiZNsPRwFsgJVRQQ

谷歌提出元奖励学习，两大基准测试刷新最优结果

https://mp.weixin.qq.com/s/x7uk7jBNvnM7Tgk9lFKy3Q

元学习(Meta-Learning)综述及五篇顶会论文推荐

https://mp.weixin.qq.com/s/GF_NLkSw64_6msmFep81fw

Google Brain ICLR Talk：元学习的前沿与挑战

https://mp.weixin.qq.com/s/sQmDZsVGIADwO97yEFATkw

ICML2019《元学习》教程与必读论文列表

https://zhuanlan.zhihu.com/p/70782949

最前沿：General Meta Learning

https://mp.weixin.qq.com/s/rZdd-vWlicynthaSasX3kQ

Meta Learning入门：MAML 和 Reptile

https://mp.weixin.qq.com/s/MsIAkJAcYHWkkMjzd7qXKA

元学习与强化学习的概率视角，47页ppt，DeepMind牛津Yee Whye Teh

https://mp.weixin.qq.com/s/IdUhvWJYviKtPs9jCbtybA

元知识图谱推理

https://www.zhihu.com/question/291656490

求问meta-learning和few-shot learning的关系是什么？

https://mp.weixin.qq.com/s/LZbprcnben6vPqsoC1DgDA

DeepMind提出元梯度强化学习算法，显著提高大规模深度强化学习应用的性能

https://mp.weixin.qq.com/s/AH35EGTH1YDSx4WzUwY15g

三四行代码打造元学习核心，PyTorch元学习库L2L现已开源

https://github.com/tristandeleu/pytorch-meta

PyTorch上方便好用的元学习工具包
