---
layout: post
title:  深度学习（三十一）——依存分析, MobileNet
category: DL 
---

* toc
{:toc}

# 依存分析

## 概况

Dependency Parsing是NLP领域的一项重要工作。

![](/images/article/dependency_parsing.png)

依存分析的基本目标是**对一句话构建一个表达词与词之间依赖关系的语法树**，如上图所示。

## 传统方法

这里以2003年提出的Greedy transition-based parsing算法为例，介绍一下依存分析的传统做法。

![](/images/article/tbp.png)

![](/images/article/tbp_2.png)

上图演示了ROOT结点是如何一步步“吃”进词语（即Shift操作），并生成依存分析树的过程。

这里的每一步被称作**transition**。

transition中箭头左边的部分是以ROOT为栈底的**stack**，右边的部分是待处理文本的**buffer**，**A**表示依赖关系树。

stack+buffer+A构成了一个**configuration**。GTBP算法的难点，在于如何根据configuration，确定下一步的transition。这在传统做法中，通常是一堆文法规则，或者特征的分类。

## 依存分析的准确度指标

![](/images/article/dependency_accuracy.png)

依存分析的准确度指标主要有UAS和LAS两种。

上图是某句话的依存分析结果。其中Gold表示正确答案，而Parsed表示算法的计算结果。结果的第二列是依存结点，0表示ROOT；第4列是单词的词性。

Unlabeled attachment score是指依存结点是否正确。以上图中的例子为例，就是4/5=80%。

Labeled	attachment score不仅考虑依存结点是否正确，还考虑词性是否正确。用样以上图为例，则是2/5=40%。

## 深度方法

深度方法的开山之作是陈丹琦2014年的论文：

《A Fast and Accurate Dependency Parser using Neural Networks》

![](/images/article/dependency_parser_nn.png)

上图是该方案的结构图。

我们之前已经指出，在传统方法中，transition是由单词、词性和依赖关系树所确定的。只是这种确定的规则比较复杂，不易提炼出有效特征。

参照我们在CNN中的作为，特征提取这一步骤可以由神经网络来完成。因此，在这里我们将configuration的各个组成部分分别向量化，然后合成为一个长向量，作为Input layer。

这里采用以下的Cube函数作为激活函数，这也是该文的一大创见：

$$h=(W_1^w x^w + W_1^t x^t + W_1^l x^l + b_1)^3$$

Output layer是一个softmax的多分类层，每个分类对应一个transition。

## SyntaxNet

2015年David Weiss在陈丹琦方案的基础上，做了一些改进。

论文：

《Structured Training for Neural Network Transition-Based Parsing》

![](/images/article/dependency_parser_nn_2.png)

上图是Weiss方案的结构图。该方案相比陈丹琦方案的改进如下：

1.由1个隐层改为两个隐层。

2.添加Perceptron Layer作为输出层。（Perceptron Layer的含义参见《机器学习（二十五）》中对于Beam Search的解释）

3.全局使用Tri-training算法作为半监督的集成学习算法。（Tri-training算法参见《机器学习（二十四）》）

《Opinion Mining with Deep Recurrent Neural Networks》

![](/images/article/Pointer_Sentinel_Mixture_Models.png)

《Pointer Sentinel Mixture Models》

https://www.zhihu.com/question/46272554

如何评价SyntaxNet？

http://mp.weixin.qq.com/s/IWagHP0MSQFAJ50NeoyOjw

基于神经网络的高性能依存句法分析器

## 陈丹琦

https://www.cs.princeton.edu/~danqic/

陈丹琦，清华本科（姚班）（2012）+斯坦福博士（2018）。Princeton AP。

http://theory.stanford.edu/~yuhch123/

陈丹琦的老公俞华程也是个学霸。而且从高中就和陈丹琦同校，直到博士毕业。

![](/images/img3/cdq.png)

左二陈丹琦，右一俞华程。两者同为IOI2008金牌得主。

## CNN在NLP中的应用

虽然基本上，CV界是CNN的天下，NLP界是RNN的地盘。然而，两者的界限并不是泾渭分明的。比如下图就是一个CNN在NLP中的应用示例：

![](/images/article/Convolutional_Sequence_to_Sequence_Learning.png)

卷积本质上是一个局部运算。对词向量的卷积，实际上等效于n-gram的词袋模型。

论文：

《Convolutional Sequence to Sequence Learning》

参见：

http://www.jeyzhang.com/cnn-apply-on-modelling-sentence.html

卷积神经网络(CNN)在句子建模上的应用

http://blog.csdn.net/malefactor/article/details/51078135

自然语言处理中CNN模型几种常见的Max Pooling操作

http://www.jianshu.com/p/1267072ee8f8

CNN在NLP中的应用

## TWE

http://nlp.csai.tsinghua.edu.cn/~lzy/publications/aaai2015_twe.pdf

Topical Word Embeddings

TWE的代码：

https://github.com/largelymfs/topical_word_embeddings

![](/images/article/sogou.jpg)

上图可以看出英语的确是世界语言啊。

## 无监督的机器翻译

无监督的机器翻译，其要点主要在于比较两种语言语料的词向量空间，以找出词语间的对应关系。

由word2vec的原理可知，由于训练时神经元是随机初始化的，因此即使是同样的语料，两次训练得到的词向量一般也不会相同，更不用说不同语料了。因此直接比较两个词向量空间中的词向量，是行不通的。

参见：

https://zmlarry.github.io

张檬，清华本科（2013）+博士（2018）。

# ESN

Echo State Network

https://blog.csdn.net/zwqhehe/article/details/77025035

回声状态网络(ESN)原理详解

https://mp.weixin.qq.com/s/tjawT2-bhPrit0Fd4knSgA

基于回声状态网络预测股票价格

# MobileNet

论文：

《MobileNets: Efficient Convolutional Neural Networks for Mobile Vision Applications》

代码：

https://github.com/Zehaos/MobileNet

![](/images/article/dwl_pwl.png)

上述结构根据通道数的变化，又可以分为两种：

Bottleneck：

![](/images/img5/BottleNeck.png)

Inverted Bottleneck

![](/images/img5/Inverted_Bottleneck.png)


参考：

https://mp.weixin.qq.com/s/f3bmtbCY5BfA4v3movwLVg

向手机端神经网络进发：MobileNet压缩指南

https://mp.weixin.qq.com/s/mcK8M6pnHiZZRAkYVdaYGQ

MobileNet在手机端上的速度评测：iPhone 8 Plus竟不如iPhone 7 Plus

https://mp.weixin.qq.com/s/2XqBeq3N4mvu05S1Jo2UwA

CNN模型之MobileNet

https://mp.weixin.qq.com/s/fdgaDoYm2sfjqO2esv7jyA

Google论文解读：轻量化卷积神经网络MobileNetV2

https://mp.weixin.qq.com/s/7vFxmvRZuM2DqSYN7C88SA

谷歌发布MobileNetV2：可做语义分割的下一代移动端计算机视觉架构

https://mp.weixin.qq.com/s/lu0GHCpWCmogkmHRKnJ8zQ

浅析两代MobileNet

https://mp.weixin.qq.com/s/T6S1_cFXPEuhRAkJo2m8Ig

轻量级CNN网络之MobileNetv2

https://mp.weixin.qq.com/s/RRu3r_dokORhpSq3eyrPDQ

为什么MobileNet及其变体如此之快？

# 并行 & 框架 & 优化+

https://mp.weixin.qq.com/s/iAHvfgn54zIwfM9K8KFJnw

DLM：微信大规模分布式n-gram语言模型系统

https://mp.weixin.qq.com/s/s7sHzzLANOp8-1LxgXQskA

谷歌开发者大会上，蚂蚁金服开源ElasticDL分布式深度学习系统

https://mp.weixin.qq.com/s/IQMXg6nIJO-9-IG3mJpvRg

ElasticDL：同时提升集群利用率和研发效率的分布式深度学习框架

https://mp.weixin.qq.com/s/uQzwqcGwC9ZveuW64Lzkmg

分布式训练怎么还减速了呢？

https://zhuanlan.zhihu.com/p/294698838

DLPerf—分布式深度学习最佳入门(踩坑)指南

https://zhuanlan.zhihu.com/p/76638962

Pytorch分布式训练

https://zhuanlan.zhihu.com/p/360405558

PyTorch分布式训练

https://mp.weixin.qq.com/s/0aSBHvscloEnPMRLyNjQsg

PyTorch分布式训练简明教程

https://blog.csdn.net/orangerfun/article/details/123887725

torch分布式训练

https://mp.weixin.qq.com/s/r7kt1k7D1wurWs_uxdLCtg

PyTorch源码解读之分布式训练

https://mp.weixin.qq.com/s/_85oWK2plv2QOX5Qfg_-ZA

大规模机器学习优化，195页ppt与视频

https://mp.weixin.qq.com/s/soruo90Dbtzi6d1kA63Akg

阿里提出智能算力引擎DCAF，节省20%GPU算力

https://mp.weixin.qq.com/s/oDak7peTT5ynNYrH7LSWTg

分布式层次GPU参数服务器架构

https://zhuanlan.zhihu.com/p/28226956

浮点峰值那些事儿

https://zhuanlan.zhihu.com/p/285994980

针对深度学习的GPU共享

https://mp.weixin.qq.com/s/Np4w7RC2JFlB7ZGIduu71w

爱奇艺机器学习平台的建设实践

https://mp.weixin.qq.com/s/DwjvEn04lGzKU8mDu-5q4g

大幅提升训练性能，字节跳动与清华提出新型分布式DNN训练架构

https://mp.weixin.qq.com/s/dJa5zOXgJJQOM5uWog3JZA

Local Parallesim：一种新并行训练方法

https://zhuanlan.zhihu.com/p/335116835

推荐系统Serving架构分析

https://mp.weixin.qq.com/s/DdsJ-ZB_cX9UhbQNK6dCag

分布式深度学习训练网络综述

https://mp.weixin.qq.com/s/qpwBGlTtTLEAhYAUpPyXTQ

CMU：分布式机器学习原理与策略 AAAI2021教程，附221页ppt

https://mp.weixin.qq.com/s/nK-9ck5S6noIETOb8b2dJw

vivo AI计算平台弹性分布式训练的探索和实践

https://mp.weixin.qq.com/s/IzLtn1SR-aFuxXM3GNZbFw

蘑菇街自研服务框架如何提升在线推理效率？

https://mp.weixin.qq.com/s/GheEA0Ag0vbhZeyzqpTl0A

分布式优化：在大数据时代应运而生

https://mp.weixin.qq.com/s/3uu50NWFJqA_MTb8wSxIKA

如何优雅地训练大型模型？

https://mp.weixin.qq.com/s/RMDEvy-3-L-Rag1OrZLYhg

深度学习模型的训练时内存次线性优化

https://mp.weixin.qq.com/s/8PUIJykzoNe-fYht5ozrcQ

新一代CTR预测服务的GPU优化实践

https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650771181&idx=1&sn=30b2a5abc7261b4f2ea122e8e96fdabf

世界第一超算跑深度学习模型，2.76万块V100 GPU将分布式训练扩展到极致

https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650771231&idx=2&sn=6907d6d7a98eab353a076ed48352aadc

15分钟完成Kinetics视频识别训练，除了超级计算机你还需要TSM

https://www.zhihu.com/question/404721763

如何评价Google的GShard论文？

https://mp.weixin.qq.com/s/eTwSo3GnxSnK-BwwZeWmKA

Jeff Dean等提出自动化分层模型，优化CPU、GPU等异构环境，性能提升超60%

https://mp.weixin.qq.com/s/q0VENBNgolpeWmDapF5q_g

在有池化层、1步幅的CNN上减少冗余计算，一种广泛适用的架构转换方法

https://mp.weixin.qq.com/s/YusIuUtvTwoskNRV_OV7iw

100万帧数据仅1秒！AI大牛颜水成团队强化学习新作，代码已开源

https://www.zhihu.com/answer/2259890109

资源受限的人工智能

https://mp.weixin.qq.com/s/ai_XI8ddP5I2m3ChCqnQsA

高效大规模机器学习训练，198页PDF带你概览领域前沿进展

https://mp.weixin.qq.com/s/RAjusu-Jyqb8K19N8KZ_3w

一份552页《大规模数据系统：Large-scale Data Systems》硬核课程PPT

https://mp.weixin.qq.com/s/AeCQK2hFy60pq6y1tRcs_A

20页pdf，A Survey on Large-scale Machine
