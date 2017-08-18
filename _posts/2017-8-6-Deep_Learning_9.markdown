---
layout: post
title:  深度学习（九）——模型压缩, 依存分析, 深度强化学习, CTC
category: theory 
---

# 模型压缩

对于AI应用端而言，由于设备普遍没有模型训练端的性能那么给力，因此如何压缩模型，节省计算的时间和空间就成为一个重要的课题。

此外，对于一些较大的模型（如VGG），即使机器再给力，单位时间内能处理的图像数量，往往也无法达到实际应用的要求。这点在自动驾驶和视频处理领域显得尤为突出。

这里首先提到的是韩松的两篇论文：

《Deep Compression: Compressing Deep Neural Networks with Pruning, Trained Quantization and Huffman Coding》

《Learning both Weights and Connections for Efficient Neural Networks》

>韩松，清华本科（2012）+Stanford博士（2017）。MIT AP（from 2018）。   
>个人主页：   
>https://stanford.edu/~songhan/

韩松也是SqueezeNet的二作。

![](/images/article/nn_compression.png)

韩松论文的中心思想如上图所示。简单来说，就是去掉原有模型的一些不重要的参数、结点和层。

参数的选择，相对比较简单。参数的绝对值越接近零，它对结果的贡献就越小。这一点和稀疏矩阵有些类似。

结点和层的选择，相对麻烦一些，需要通过算法得到不重要的层。

比如可以逐个将每一层50%的参数置零，查看模型性能。对性能影响不大的层就是不重要的。

虽然这些参数、结点和层相对不重要，但是去掉之后，仍然会对准确度有所影响。这时可以对精简之后的模型，用训练样本进行re-train，通过残差对模型进行一定程度的修正，以提高准确度。

其次还可以看看图森科技的论文：

https://www.zhihu.com/question/62068158

如何评价图森科技连发的三篇关于深度模型压缩的文章？

图森的思路比较有意思。其中的方法之一，是利用L1规则化会导致结果的稀疏化的特性，制造出一批接近0的参数。从而达到去除不重要的参数的目的。

除此之外，矩阵量化、Kronecker内积、霍夫曼编码、模型剪枝等也是常见的模型压缩方法。

当然最系统的做法还属Geoffrey Hinton的论文：

《Distilling the Knowledge in a Neural Network》

图森科技的后两篇论文也是在Hinton论文的基础上改进的。

论文：

《Articulatory and Spectrum Features Integration using Generalized Distillation Framework》

参考：

https://zhuanlan.zhihu.com/p/24337627

深度压缩之蒸馏模型

https://zhuanlan.zhihu.com/p/24894102

《Distilling the Knowledge in a Neural Network》阅读笔记

https://luofanghao.github.io/2016/07/20/%E8%AE%BA%E6%96%87%E7%AC%94%E8%AE%B0%20%E3%80%8ADistilling%20the%20Knowledge%20in%20a%20Neural%20Network%E3%80%8B/

论文笔记 《Distilling the Knowledge in a Neural Network》

http://blog.csdn.net/zhongshaoyy/article/details/53582048

蒸馏神经网络

https://www.zhihu.com/question/50519680

如何理解soft target这一做法？

https://mp.weixin.qq.com/s/0KlnQ8UUxpyhBRdeo0EOAA

用于网络压缩的滤波器级别剪枝算法ThiNet

https://mp.weixin.qq.com/s/lO2UM04PfSM5VJYh6vINhw

为模型减减肥：谈谈移动／嵌入式端的深度学习

https://mp.weixin.qq.com/s/cIGuJvYr4lZW01TdINBJnA

深度压缩网络：较大程度减少了网络参数存储问题

https://mp.weixin.qq.com/s/1JwLP0FmV1AGJ65iDgLWQw

神经网络模型压缩技术

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

2.添加Perceptron Layer作为输出层。（Perceptron Layer的含义参见《机器学习（二十二）》中对于Beam Search的解释）

3.全局使用Tri-training算法作为半监督的集成学习算法。（Tri-training算法参见《机器学习（二十二）》）

《Opinion Mining with Deep Recurrent Neural Networks》

![](/images/article/Pointer_Sentinel_Mixture_Models.png)

《Pointer Sentinel Mixture Models》

https://www.zhihu.com/question/46272554

如何评价SyntaxNet？

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

https://mp.weixin.qq.com/s/hGGT61frDfm-PQHjSnI3Tw

自然语言处理中CNN模型几种常见的Max Pooling操作

## TWE

http://nlp.csai.tsinghua.edu.cn/~lzy/publications/aaai2015_twe.pdf

Topical Word Embeddings

TWE的代码：

https://github.com/largelymfs/topical_word_embeddings

![](/images/article/sogou.jpg)

上图可以看出英语的确是世界语言啊。

## 无监督的机器翻译

无监督的机器翻译，其要点主要在于比较两种语言的语料的词向量，并找出词语间的对应关系。

参见：

http://nlp.csai.tsinghua.edu.cn/~zm/

# 深度强化学习

## 教程

http://incompleteideas.net/sutton/book/the-book-2nd.html

《Reinforcement Learning: An Introduction》，Richard S. Sutton和Andrew G. Barto著。

>注：Richard S. Sutton，加拿大计算机科学家，麻省大学阿姆赫斯特分校博士（1984年），阿尔伯塔大学教授。强化学习之父，研究该领域长达三十余年。

>Andrew G. Barto，麻省大学阿姆赫斯特分校教授。Richard S. Sutton的导师。

http://web.stanford.edu/class/cs234/syllabus.html

CS234: Reinforcement Learning

## 概述

![](/images/article/reinforcement_learning.png)

## 参考

https://www.nervanasys.com/demystifying-deep-reinforcement-learning/

深度强化学习揭秘

http://blog.csdn.net/young_gy/article/details/73485518

强化学习之Q-learning简介

https://zhuanlan.zhihu.com/p/24446336

深度强化学习Deep Reinforcement Learning学习整理

https://mp.weixin.qq.com/s/KNXD-MpVHQRXYvJKTqn6WA

完善强化学习安全性：UC Berkeley提出约束型策略优化新算法

http://mp.weixin.qq.com/s/gHM7qh7UTKzatdg34cgfDQ

强化学习全解

http://mp.weixin.qq.com/s/lLPRwInF5qaw7ewYHOpPyw

深度强化学习资料

https://mp.weixin.qq.com/s/f6sq8cSaU1cuzt7jhsK8Ig

强化学习（Reinforcement Learning）基础介绍

https://mp.weixin.qq.com/s/TGN6Zhrea2LPxdkspVTlAw

算法工程师入门——增强学习

https://mp.weixin.qq.com/s/aVWHlwOmNIqOlu3025_RXQ

DeepMind提出多任务强化学习新方法Distral

https://mp.weixin.qq.com/s/laKJ_jfNR5L1uMML9wkS1A

强化学习（Reinforcement Learning）算法基础及分类

https://zhuanlan.zhihu.com/p/27699682

荐译一篇通俗易懂的策略梯度（Policy Gradient）方法讲解

https://mp.weixin.qq.com/s/Cvk_cePK9iQd8JIKKDDrmQ

强化学习的核心基础概念及实现

http://lamda.nju.edu.cn/yangjw/project/drlintro.html

深度强化学习初探

https://zhuanlan.zhihu.com/p/21498750

深度强化学习导引

https://mp.weixin.qq.com/s/RnUWHa6QzgJbE_XqLeAQmg

深度强化学习，决策与控制

# CTC

Connectionist Temporal Classification，是一种改进的RNN模型。它主要解决的是时序模型中，输入数大于输出数，输入输出如何对齐的问题。

参考：

https://www.zhihu.com/question/47642307

语音识别中的CTC方法的基本原理

https://www.zhihu.com/question/20398418

语音识别的技术原理是什么？

https://www.zhihu.com/question/55851184

基于CTC等端到端语音识别方法的出现是否标志着统治数年的HMM方法终结？

https://www.zhihu.com/question/46829056

语音识别领域的最新进展目前是什么样的水准？

https://zhuanlan.zhihu.com/p/27064536

用Wavenet做中文语音识别

https://www.zhihu.com/question/29168274

语音识别中，如何理解HMM是一个生成模型，而DNN是一个判别模型呢？

https://zhuanlan.zhihu.com/p/24979135

从声学模型算法总结2016年语音识别的重大进步

https://mp.weixin.qq.com/s/LsVhMaHrh8JgfpDra6KSPw

横向对比5大开源语音识别工具包

https://mp.weixin.qq.com/s/-NTQG7_-GqGQWrRhiGgAQQ

详述DeepMind wavenet原理及其TensorFlow实现

https://mp.weixin.qq.com/s/bFjXDQlxRbt1ia-DSfYazw

SampleRNN语音合成模型

https://zhuanlan.zhihu.com/p/21344595

端到端的OCR：验证码识别(LSTM+CTC)

https://mp.weixin.qq.com/s/zEqgDh6_fnDgXEI8MC9cmg

端对端的深度卷积神经网络在语音识别中的应用

# WFST

Weighted-Finite-State-Transducer

https://www.microsoft.com/en-us/research/wp-content/uploads/2016/11/ParallelizingWFSTSpeechDecoders.ICASSP2016.pdf

PARALLELIZING WFST SPEECH DECODERS

http://www.cs.nyu.edu/~mohri/pub/csl01.pdf

Weighted Finite-State Transducers in Speech Recognition

