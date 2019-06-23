---
layout: post
title:  深度学习（三十一）——依存分析, Image Caption Generation, Recursive NN
category: DL 
---

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

## NLP的女学霸们

http://cs.stanford.edu/people/danqi/

陈丹琦，清华本科（姚班）（2012）+斯坦福博士生。

http://theory.stanford.edu/~yuhch123/

陈丹琦的老公俞华程也是个学霸。而且从高中就和陈丹琦同校，直到博士毕业。

https://homes.cs.washington.edu/~luheng/

何律恒，上海交大本科（2010）+宾夕法尼亚大学硕士（2012）+华盛顿大学博士生。

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

http://nlp.csai.tsinghua.edu.cn/~zm/

张檬，清华本科（2013）+博士（在读）。

# Image Caption Generation

Image Caption Generation的目标是：给定一张图片，让计算机用一句话来描述这张图片。如果将这里的Image换成Video，那就是Video Caption Generation了。

当然，它也有反操作：Generating Image/Videos from Captions。

参考：

http://geek.csdn.net/news/detail/97193

李理：从Image Caption Generation理解深度学习（part I）

http://geek.csdn.net/news/detail/98776

李理：从Image Caption Generation理解深度学习（part II）

https://mp.weixin.qq.com/s/i4zL-bPaPXUlwpKZzlRtcg

Image Captioning36页最新综述

https://zhuanlan.zhihu.com/p/30893160

CVPR2017 Image Caption有关论文总结

https://mp.weixin.qq.com/s/3l4mYVSVfjFS_06j3OvX8g

阿里提出新图像描述框架，解决梯度消失难题

https://mp.weixin.qq.com/s/O1LqJftEezBuuA8JHeqMzw

基于对比学习的Image Captioning

https://mp.weixin.qq.com/s/-K3WIo64_wJb9p6C49Z5fA

基于属性学习和额外知识库的图像描述生成和视觉问答

https://mp.weixin.qq.com/s/CBqDR7JEPfhIwTc8BDdeoQ

“诗画合一”的跨媒体理解与检索

https://mp.weixin.qq.com/s/vRHA3Hf1ivsgKBB2XuECNg

李飞飞论文：神经网络是怎样给一幅图增加文字描述，实现“看图说话”的？

https://mp.weixin.qq.com/s/DXRiGSI0p8u7yA9uW1hHxA

最新四篇CVPR2018 视频描述生成相关论文—双向注意力、Transformer、重构网络、层次强化学习

https://mp.weixin.qq.com/s/bDLVD_8LscgWSNp0ZNc0Pg

图像和文本的融合表示学习——Text2Image和Image2Text

https://mp.weixin.qq.com/s/oV8gcKqmp43EBYzQhr5S6w

逆视觉问答任务：一种根据回答与图像想问题的模型

https://mp.weixin.qq.com/s/YgYod-gcFZruEGtE4wF87w

牛津大学提出全新生成式模型“SQAIR”，用于移动目标的视频理解

https://mp.weixin.qq.com/s/eBNoTDOMLlVymiU3LUqSgQ

用Attention模型自动生成图像字幕

https://mp.weixin.qq.com/s/4wuYNYGDesNgsJ13y65XQA

AI都可以将文字轻松转成图像

https://mp.weixin.qq.com/s/TtU0R8P-YIq8JOJrgHP07w

Facobook开源视觉问答VQA框架：Pythia

https://mp.weixin.qq.com/s/sc3ROchBFK9pAb-FCKNwLg

你说我导！微软玩转标题描述生成视频

https://zhuanlan.zhihu.com/p/35305264

一文看懂深度学习中的VQA(视觉问答)技术

https://mp.weixin.qq.com/s/shBHKh2emSIL5uWV7po8cw

梅涛：“看图说话”

https://mp.weixin.qq.com/s/BQy3qpmQqeQquhf92hwcvQ

2017 VQA Challenge第一名技术报告

https://mp.weixin.qq.com/s/0yb-YRGe-q4-vpKpuE4D_w

多种注意力机制互补完成VQA（视觉问答）

https://mp.weixin.qq.com/s/YBaELQlBHOZTGEqc2WI9NQ

MIT等提出NS-VQA：结合深度学习与符号推理的视觉问答

https://mp.weixin.qq.com/s/gaNSQ_8JGqX4a3lGuf-9fA

如何让电脑成为看图说话的高手？

https://mp.weixin.qq.com/s/-b8FuEQlpEb5G0L0QAxVEA

如何使用深度学习为照片自动生成文本描述

https://zhuanlan.zhihu.com/p/50784504

《Reconstruction Network for Video Captioning》阅读笔记

https://mp.weixin.qq.com/s/tEzZ770T1NpYLAxUK-MwIg

综述：Image Caption 任务之语句多样性

https://zhuanlan.zhihu.com/p/53220566

《Adaptive Co-Attention Network for NER in Tweet》阅读笔记

https://mp.weixin.qq.com/s/sQoqt-7EqZmy5gvIWTtMKQ

更有智慧的眼睛：图像描述（Image Caption）&视觉问答（VQA）综述（上）

https://mp.weixin.qq.com/s/a0Xt_hkcW2OVlEzhQFWugg

由浅及深，细致解读图像问答 VQA 2018 Challenge冠军模型Pythia

https://mp.weixin.qq.com/s/3Y5f7JsxsmmuCjAX625h1Q

MirrorGAN出世！浙大等提出文本-图像新框架

https://mp.weixin.qq.com/s/1rtONnhwEc3Osf9gP5fBCQ

李飞飞CVPR最新论文“文本转图”效果优化可多一步：物体关系描述

https://mp.weixin.qq.com/s/9XiAJzC2_vmZturE9ELVLA

多级语言与视觉集成用于文本-剪辑检索

https://mp.weixin.qq.com/s/UehKCDiBRnjZWLEcx_iU-g

微软最新提出ObjGAN，输入一句话秒生成图片

# Recursive NN

http://blog.csdn.net/qq_26609915/article/details/52119512

递归神经网络（recursive NN）结合自编码（Autoencode）实现句子建模

http://blog.csdn.net/mengmengz07/article/details/51348554

recursive neural network梳理

https://mp.weixin.qq.com/s/DwaSF76uvqFKkybaTUpl3w

空间序列递归神经网络用于高光谱图像分类
