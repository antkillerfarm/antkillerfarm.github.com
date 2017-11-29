---
layout: post
title:  深度学习（二十一）——依存分析, Image Caption Generation, 行人重识别, 图像超分辨率算法
category: DL 
---

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

2.添加Perceptron Layer作为输出层。（Perceptron Layer的含义参见《机器学习（二十三）》中对于Beam Search的解释）

3.全局使用Tri-training算法作为半监督的集成学习算法。（Tri-training算法参见《机器学习（二十二）》）

《Opinion Mining with Deep Recurrent Neural Networks》

![](/images/article/Pointer_Sentinel_Mixture_Models.png)

《Pointer Sentinel Mixture Models》

https://www.zhihu.com/question/46272554

如何评价SyntaxNet？

http://mp.weixin.qq.com/s/IWagHP0MSQFAJ50NeoyOjw

基于神经网络的高性能依存句法分析器

## NLP的女学霸们

http://cs.stanford.edu/people/danqi/

陈丹琦，清华本科（姚班）（2012）+斯坦福博士生。

https://homes.cs.washington.edu/~luheng/

何律恒，上海交大本科（2010）+宾夕法尼亚大学硕士（2012）+华盛顿大学博士生。

## CNN在NLP中的应用

虽然基本上，CV界是CNN的天下，NLP界是RNN的地盘。然而，两者的界限并不是泾渭分明的。比如下图就是一个CNN在NLP中的应用示例：

![](/images/article/Convolutional_Sequence_to_Sequence_Learning.png)

卷积本质上是一个局部运算。对词向量的卷积，实际上等效于n-gram的词袋模型。

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

http://nlp.csai.tsinghua.edu.cn/~zm/

张檬，清华本科（2013）+博士（在读）。

# Image Caption Generation

Image Caption Generation的目标是：给定一张图片，让计算机用一句话来描述这张图片。

参考：

http://geek.csdn.net/news/detail/97193

李理：从Image Caption Generation理解深度学习（part I）

http://geek.csdn.net/news/detail/98776

李理：从Image Caption Generation理解深度学习（part II）

https://zhuanlan.zhihu.com/p/30893160

CVPR2017 Image Caption有关论文总结

# 行人重识别

行人重识别（Person re-identification）也称行人再识别，是利用计算机视觉技术判断图像或者视频序列中是否存在特定行人的技术。广泛被认为是一个图像检索的子问题。给定一个监控行人图像，检索跨设备下的该行人图像。旨在弥补目前固定的摄像头的视觉局限，并可与行人检测/行人跟踪技术相结合 ，可广泛应用于智能视频监控、智能安保等领域。

https://zhuanlan.zhihu.com/p/26168232

行人重识别：从哈利波特地图说起

https://mp.weixin.qq.com/s/_NDw7pFmDB07mliHTA6VYQ

旷视行人再识别（ReID）突破

https://zhuanlan.zhihu.com/p/31181247

从人脸识别到行人重识别，下一个风口

https://mp.weixin.qq.com/s/zRdJktyk1LZWUd2cyTjpiw

基于图像检索的行人重识别

https://zhuanlan.zhihu.com/p/31473785

行人再识别中的迁移学习：图像风格转换

# 图像超分辨率算法

Super Resolution（SR）的目标是：给你一张低分辨率的小图（Low Resolution，LR），你通过算法将这张LR放大成一张高分辨率的大图（High Resolution，HR）。

参考：

https://zhuanlan.zhihu.com/p/25532538

深度学习在图像超分辨率重建中的应用

https://zhuanlan.zhihu.com/p/25201511

深度对抗学习在图像分割和超分辨率中的应用

https://mp.weixin.qq.com/s/uK0L5RV0bB2Jnr5WCZasfw

深度学习在单图像超分辨率上的应用：SRCNN、Perceptual loss、SRResNet

https://mp.weixin.qq.com/s/KxQ-GRnEYEdmS2H-DHIHOg

南京理工大学ICCV 2017论文：图像超分辨率模型MemNet

https://mp.weixin.qq.com/s/xpvGz1HVo9eLNDMv9v7vqg

NTIRE2017夺冠论文：用于单一图像超分辨率的增强型深度残差网络

https://www.zhihu.com/question/25401250

如何通过多帧影像进行超分辨率重构？

https://www.zhihu.com/question/38637977

超分辨率重建还有什么可以研究的吗？

https://zhuanlan.zhihu.com/p/25912465

胎儿MRI高分辨率重建技术：现状与趋势

https://mp.weixin.qq.com/s/i-im1sy6MNWP1Fmi5oWMZg

华为推出新型HiSR：移动端的超分辨率算法


