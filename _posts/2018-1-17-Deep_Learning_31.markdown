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

# 欧洲+

女王的另外两个儿媳约克公爵夫人莎拉和威塞克斯伯爵夫人苏菲也是平民出身，没有娘家王冠。但是为了表示王室的诚意，对于她们二人女王是各赠送了一顶婚礼王冠，但是谁能想到后来莎拉和安德鲁离婚带走了价值不菲的王冠，再之后的王室成员结婚，女王就不再赠送王冠了。

别说外国王室，就是本国的贵族们也不愿意放弃自由而奢华的生活去追求规矩繁多的王室生活。所以其实查尔斯那一代结婚的时候选择其实就挺少的，到了威廉这一代那就更少了。不是女王愿意让王室接纳平民成员，而是根本选不到合适的贵族。

---

土耳其人海战并不强，很多近在咫尺的岛屿都拿不下。

小亚细亚附近的罗德岛直到1522年才被攻破。

希腊南部的克里特岛更是在君堡沦陷两百年后的1669年才沦陷。

而希腊北部的爱奥尼亚群岛则一直被威尼斯统治，和土耳其人从来就没有关系。

---

当你发现一只蟑螂，可能全屋都是。。。

苏纳克是英国首相，英国内政大臣布雷弗曼是印度裔（此人是苏纳克铁杆），气候变化事务高级官员和北爱尔兰事务官都是印度裔，前内政大臣帕特尔也是印度裔。

https://www.zhihu.com/question/563621763

为什么苏纳克仅仅从政七年就能成为英国首相?

---

市长选举大部分由穆斯林取胜。

伦敦市长：穆斯林。

​伯明翰市长：穆斯林。

​利兹市长：穆斯林。

​布莱克本市长：穆斯林。

​谢菲尔德市市长：穆斯林。

​牛津市长：穆斯林。

---

维京人的社会体系包括三个阶层：

Jarl：王侯，多为大领主(lord)，世袭的贵族；

Karl：自由人，军队的主力，武士阶层；部落首领实行统治的基本工具，是依靠个人财力豢养的心腹亲兵，称huscarl。

Thralls：最底层的奴隶。

https://www.zhihu.com/question/467481944

为什么维京人以战死为荣耀，而以老死为屈辱，他们怎么想的，如果为了显得能打，难道不是活最久的最能打？

---

拿破仑当时已经有了个名叫宰纳卜的埃及女人，不过显然他还是喜欢法国风味，又得知了约瑟芬出轨。于是随即计上心来，把富雷斯中尉派往马耳他，执行危险的跨海任务——不过有一说一，拿破仑还是给富雷斯发了三千法郎的活动经费，也算是变相补偿吧。

富雷斯走后，拿破仑随后便和波利娜双宿双飞，整个东方军团都知道波利娜成了拿破仑的“克娄巴特拉”。

拿破仑1799年回国后，波利娜又成为其继任者让-巴蒂斯特·克莱贝尔将军的情人，可见这个绰号还真的有那么一点准。
