---
layout: post
title:  深度学习（三十一）——依存分析, MobileNet
category: DL 
---

* toc
{:toc}

# NLP历史

![](/images/img2/text.jpg)

![](/images/img2/NLP_history.png)

第一代技术（1950s）：符号主义，用计算机的符号操作来模拟人的认知过程。

第二代技术（1970s）：语法规则，依赖于专家人工制定的语法规则和本体设计（ontological design）。

第三代技术（1990s）：统计学习，即让计算机阅读大量文章。

第四代技术（2010s）：深度学习，用一个复杂的模型像人脑神经网络一样运作。

![](/images/img2/Han.jpg)

参考：

https://mp.weixin.qq.com/s/1cdg635KcPTV6mFdwPo2OQ

达观数据：文字的起源与文本挖掘的前世今生

https://mp.weixin.qq.com/s/krW1eUFu7iz9YyYFGbFVeQ

香侬科技提出中文字型的深度学习模型Glyce，横扫13项中文NLP记录

https://mp.weixin.qq.com/s/VtEM6paUPH28GFrwVNmz4w

自然语言处理起源：马尔科夫和香农的语言建模实验

https://mp.weixin.qq.com/s/qlKGgWq_FTYonMXEkhRwpw

中国境内语言概览

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

## Subword

对于英文来说，文字的粒度从细到粗依次是character, subword, word。character和word都很好理解，分别是字母和单词。而subword相当于英文中的词根、前缀、后缀等。

之前的Neural Machine Translation基本上都是基于word单词作为基本单位的，但是其缺点是不能很好的解决out-of-vocabulary即单词不在词汇库里的情况，且对于单词的一些词法上的修饰(morphology)处理的也不是很好。一个自然的想法就是能够利用比word更基本的组成来建立模型，以更好的解决这些问题。

参考：

https://plmsmile.github.io/2017/10/19/subword-units/

subword-units

https://zhuanlan.zhihu.com/p/69414965

Subword模型

https://zhuanlan.zhihu.com/p/86965595

深入理解NLP Subword算法：BPE、WordPiece、ULM

https://mp.weixin.qq.com/s/la3nZNFDviRcSFNVso29oQ

NLP三大Subword模型详解：BPE、WordPiece、ULM

https://mp.weixin.qq.com/s/TPRqDyGpkVuJcgomTu774A

子词技巧：The Tricks of Subword

https://mp.weixin.qq.com/s/fe7-wimFCAtp3ohB3TVywg

通俗讲解Subword Models

## BPE

Byte Pair Encoding(BPE)本来是一种数据压缩算法，后来被用于分词。它从命名实体、同根词、外来语、组合词（罕见词有相当大比例是上述几种）的翻译策略中得到启发，认为把这些罕见词拆分为“子词单元”(subword units)的组合，可以有效的缓解NMT的OOV和罕见词翻译的问题。BPE对英文、德文、俄文等表音文字效果较好，但不太适用于中文。

论文：

《Neural Machine Translation of Rare Words with Subword Units》

代码：

https://github.com/rsennrich/subword-nmt

参考：

https://www.cnblogs.com/huangyc/p/10223075.html

一文读懂BERT中的WordPiece

https://mp.weixin.qq.com/s/OGBk_ZptFzbjKdnv2RVZFA

机器如何认识文本？NLP中的Tokenization方法总结

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

# 图像变换++

https://mp.weixin.qq.com/s/JO8n_rEz6LxpZu6IS0Fk0Q

AI把爱豆变胖视频火遍B站，我们找到了背后的技术团队:你是怎么把刘亦菲变胖的？

https://mp.weixin.qq.com/s/9gR062zrQ7im8HpevhY0sw

这个AI“大师级”简笔画水平，惊艳到了网友：竟然不用GAN

https://mp.weixin.qq.com/s/yHy9wCCqXCxki_n7guYE_A

AI算法实现武侠小说中的“绝世武功”——动作残影特效！

https://mp.weixin.qq.com/s/7RqD-rM-UIG-x-YSYzYdxg

Chimera Painter：使用GAN构建大量风格奇幻的卡牌游戏图像

https://mp.weixin.qq.com/s/VaiUuac6VuppjZ2nrshyKA

不满《曼达洛人》用特效给69岁天行者“减龄”，网友用DeepFake重制结局：强过官方

https://mp.weixin.qq.com/s/QwvZSnF1cT5pDcp4aUigYA

川普跳“鸡你太美”？这么专业，一定是AI合成的！

https://mp.weixin.qq.com/s/GpF5ethQpGcDPAJlpPNBYQ

不用GAN，照片也能生成简笔画，效果惊艳
