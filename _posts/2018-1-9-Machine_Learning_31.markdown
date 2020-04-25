---
layout: post
title:  机器学习（三十一）——CRF
category: ML 
---

# HMM（续）

## Baum–Welch算法

Baum–Welch算法是求解问题3的常用算法，由Baum和Welch于1972年提出。它虽然是EM算法的一个特例，但后者却是1977年才提出的。

>Leonard Esau Baum，1931～2017，美国数学家，哈佛博士（1958）。国防分析研究所研究员，70年代末，加盟对冲基金——文艺复兴科技公司。

>Lloyd Richard Welch，生于1927年，美国数学家，加州理工博士（1958），南加州大学教授。美国工程院院士，Shannon Award获得者（2003）。

Baum–Welch算法也叫前向后向算法。因为它包含了前向和后向两个步骤。

1. **expectation**：计算隐变量的概率分布，并得到可观察变量与隐变量联合概率的log-likelihood在前面求得的隐变量概率分布下的期望。这个步骤就是所谓的前向步骤，算法和求解问题2的forward算法是一致的

2. **maximization**：求得使上述期望最大的新的模型参数。若达到收敛条件则退出，否则回到步骤1。

前向后向算法则主要是解决Expectation这步中求隐变量概率分布的一个算法，它利用dynamic programming大大减少了计算量。

此外，训练HMM模型时，也需要对模型参数进行随机初始化，不然和神经网络一样，由于参数没有差异性，而无法进行训练。

参考：

https://blog.csdn.net/xmu_jupiter/article/details/50965039

HMM的Baum-Welch算法和Viterbi算法公式推导细节

## HMM在NLP领域的应用

具体到分词系统，可以将“标签”当作隐含状态，“字或词”当作可见状态。那么，几个NLP的问题就可以转化为：

词性标注：给定一个词的序列（也就是句子），找出最可能的词性序列（标签是词性）。如ansj分词和ICTCLAS分词等。

分词：给定一个字的序列，找出最可能的标签序列（断句符号：[词尾]或[非词尾]构成的序列）。结巴分词目前就是利用BMES标签来分词的，B（开头）,M（中间),E(结尾),S(独立成词）

命名实体识别：给定一个词的序列，找出最可能的标签序列（内外符号：[内]表示词属于命名实体，[外]表示不属于）。如ICTCLAS实现的人名识别、翻译人名识别、地名识别都是用同一个Tagger实现的。

综上，**在监督学习中，一般把训练数据当作HMM的可见状态，而把标签当作隐含状态。**当然这里的标签一般是生成最终训练标签的一个概率模型的参数。

如果标签不是概率模型的，而是已知确定的话，那么就退化为普通的逻辑回归，直接用极大似然估计即可。

正由于标签是概率模型的，存在一个建模的过程，因此HMM实际上是一种生成模型算法。

HMM只遵循了一阶马尔科夫假设，即1-gram。如果数据的依赖超过1-gram，则不能使用HMM。

参考：

http://blog.sina.com.cn/s/blog_8267db980102wq4l.html

HMM识别新词

https://mp.weixin.qq.com/s/eGx0PHvZXwmykUon-9M-mQ

HMM模型在贝壳对话系统中的应用

# MRF

前文已经指出无向的概率图模型也叫做Markov Random Field。

![](/images/img3/MRF.png)

上图是一个简单的MRF的示意图。无向图的边没有箭头，表示变量之间是相互依赖的。

对于图中结点的一个子集，若其中任意两结点间都有边连接，则称该结点子集为一个**团（clique）**。

若在一个团中，加入另外任何一个结点都不能形成团，则称该团为**极大团（maximal clique）**。

上图中，$$\{x_5,x_6\}$$是团，$$\{x_2,x_5,x_6\}$$是极大团，而$$\{x_1,x_2,x_3\}$$不是团，因为$$x_2,x_3$$之间没有连接。

从团的定义可以看出：

- 无法组团的两个结点之间由于没有依赖关系，因此是相互独立的。

- 每个结点至少出现在一个极大团中。孤立结点的极大团就是它本身。

- 若Q不是一个极大团，则必有极大团Q*包含Q。

基于以上性质，我们效仿整数的因子分解，提出了概率图的**极大团分解**：

$$P(x)=\frac{1}{Z^*}\prod_{Q\in C^*}\psi_Q(x_Q)$$

其中，$$\psi_Q$$为Q对应的势函数。

$$Z^*=\sum_x \prod_{Q\in C^*} \psi_Q(x_Q)$$

$$Z^*$$通常被称为规范化因子，用于使输出符合概率的定义（部分概率$$\le 1$$，全概率$$=1$$）。

![](/images/img3/MRF_2.png)

如上图所示，结点集A中的结点到B中的结点，必须经过C中的结点，则称A和B被C分离，C称为**分离集（separating set）**。

显然，A和B由于没有直接关系，因此是相对于C条件独立的。这就是所谓的**global Markov property**。即：

$$P(x_A,x_B|x_C)=P(x_A|x_C)P(x_B|x_C)$$

从结点间的连通性上，还可得以下术语：

**马尔可夫毯(Markov Blanket, MB)**：有向图——结点A的父结点+A的子结点+A的子结点的其他父结点。如下图所示：

![](/images/article/Markov_blanket.png)

无向图——结点A的邻接结点。

和global Markov property类似，还有**local Markov property**：给定某变量的邻接变量，则该变量条件独立于其他变量。

# MEMM

Maximum Entropy Markov Model是一种判别模型。

http://www.cnblogs.com/en-heng/p/6201893.html

中文分词：最大熵马尔可夫模型MEMM

http://www.cnblogs.com/549294286/archive/2013/06/06/3121761.html

统计模型之间的比较，HMM，最大熵模型，CRF条件随机场

http://blog.csdn.net/caohao2008/article/details/4242308

HMM,MEMM,CRF模型的比较

http://blog.csdn.net/zhoubl668/article/details/7787690

标注偏置问题(Label Bias Problem)和HMM、MEMM、CRF模型比较

http://tripleday.cn/2016/07/14/hmm-memm-crf/

HMM、MEMM和CRF的学习总结

https://zhuanlan.zhihu.com/p/33397147

概率图模型体系：HMM、MEMM、CRF

# CRF

条件随机场(Conditional Random Field)由Lafferty等人于2001年提出，结合了最大熵模型和隐马尔可夫模型的特点，是一种无向图模型，近年来在分词、词性标注和命名实体识别等序列标注任务中取得了很好的效果。

https://zhuanlan.zhihu.com/p/35969159

如何轻松愉快的理解条件随机场（CRF）？

http://www.chokkan.org/software/crfsuite/

CRFsuite: A fast implementation of Conditional Random Fields (CRFs)

https://github.com/scrapinghub/python-crfsuite

A python binding for crfsuite

http://taku910.github.io/crfpp/

CRF++: Yet Another CRF toolkit

https://mp.weixin.qq.com/s/1rx_R1BGRVAIDqKixNLMQA

终极入门 马尔可夫网络 (Markov Networks)——概率图模型

https://mp.weixin.qq.com/s/GXbFxlExDtjtQe-OPwfokA

一文轻松搞懂-条件随机场CRF

https://mp.weixin.qq.com/s/0FIns5Xt2G1seqFbpGvzTQ

长文详解基于并行计算的条件随机场

https://blog.csdn.net/liuyuemaicha/article/details/73147548

从PGM到HMM再到CRF

https://mp.weixin.qq.com/s/4r4k6JIj4xvHHmt3QqmbuA

以RNN形式做CRF后处理—CRFasRNN

https://mp.weixin.qq.com/s/JsqhwwJ7wnNcgOuAR6ekxw

理解条件随机场

https://mp.weixin.qq.com/s/79M6ehrQTiUc0l_sO9fUqA

用于序列标注问题的条件随机场（Conditional Random Field, CRF）

https://zhuanlan.zhihu.com/p/78006020

NCRF++学习笔记

https://zhuanlan.zhihu.com/p/91031332

用腻了CRF，试试LAN吧？

https://zhuanlan.zhihu.com/p/100576406

条件随机场及Mininum Risk Training

https://www.jianshu.com/p/55755fc649b1

如何轻松愉快地理解条件随机场（CRF）？

https://zhuanlan.zhihu.com/p/34261803

白话条件随机场（conditional random field）

## BiLSTM+CRF

![](/images/img2/BiLSTM_CRF.jpg)

https://mp.weixin.qq.com/s/vbBNYzKq6AnsDTy8lFsKAw

TensorFlow RNN深度学习BiLSTM+CRF实现sequence labeling序列标注

https://www.jianshu.com/p/97cb3b6db573

BiLSTM模型中CRF层的运行原理-1

https://www.jianshu.com/p/7c83478eeb56

BiLSTM模型中CRF层的运行原理-2

https://www.zhihu.com/question/62399257

如何理解LSTM后接CRF？

https://mp.weixin.qq.com/s/1FCWMRapGMXjxTLoA2fYCg

CRF和LSTM模型在序列标注上的优劣？

https://zhuanlan.zhihu.com/p/97676647

手撕BiLSTM-CRF

https://zhuanlan.zhihu.com/p/44042528

最通俗易懂的BiLSTM-CRF模型中的CRF层介绍

https://mp.weixin.qq.com/s/0WVqQkvzb6TYFA9gEh73ZQ

BiLSTM上的CRF，用命名实体识别任务来解释CRF（1）

https://mp.weixin.qq.com/s/VG5C9NFMejetrj60KIbWug

BiLSTM上的CRF，用命名实体识别任务来解释CRF（2）损失函数

https://mp.weixin.qq.com/s/PaunoXYUz13s0lbgzcqE9A

BiLSTM上的CRF，用命名实体识别任务来解释CRF（3）推理

https://mp.weixin.qq.com/s/xJ7MpUkVfLQKxRYyJs29NQ

BiLSTM上的CRF，用命名实体识别任务来解释CRF（4）

# t-SNE

## 概述

t-SNE(t-distributed stochastic neighbor embedding)是用于降维的一种机器学习算法，是由Laurens van der Maaten和Geoffrey Hinton在08年提出来。此外，t-SNE 是一种非线性降维算法，非常适用于高维数据降维到2维或者3维，进行可视化。

论文：

《Visualizing Data using t-SNE》

以下是几种常见的降维算法：

1.主成分分析（线性）

2.t-SNE（非参数/非线性）

3.萨蒙映射（非线性）

4.等距映射（非线性）

5.局部线性嵌入（非线性）

6.规范相关分析（非线性）

7.SNE（非线性）

8.最小方差无偏估计（非线性）

9.拉普拉斯特征图（非线性）

PCA的相关内容参见《机器学习（十六）》。

## SNE

在介绍t-SNE之前，我们首先介绍一下SNE（Stochastic Neighbor Embedding）的原理。

假设我们有数据集X，它共有N个数据点。每一个数据点$$x_i$$的维度为D，我们希望降低为d维。在一般用于可视化的条件下，d的取值为 2，即在平面上表示出所有数据。

SNE将数据点间的欧几里德距离转化为条件概率来表征相似性：

$$p_{j\mid i}=\frac{\exp(-\|x_i-x_j\|^2/2\sigma^2)}{\sum_{k\neq i}\exp(-\|x_i-x_k\|^2/2\sigma^2)}$$
