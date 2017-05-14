---
layout: post
title:  机器学习（二十）——loss function详解, HMM, 概率图模型
category: theory 
---

## 贝叶斯过滤器（续）

上式也可以写作：

**预测**：

$$\overline{\mathbf{Bel(x_t)}}=\int P(x_t\vert u_t,x_{t-1})\mathbf{Bel(x_{t-1})}\mathrm{d}x_{t-1}$$

**修正**：

$$\mathbf{Bel(x_t)}=\eta P(z_t\vert x_t)\overline{\mathbf{Bel(x_t)}}$$

熟悉卡尔曼滤波的同学大概已经看出来了。没错！贝叶斯过滤器是一大类算法的统称。这些算法包括Kalman filters、Particle filters、Hidden Markov models、Dynamic Bayesian networks、Partially Observable Markov Decision Processes (POMDPs)等。

## 递归最小二乘法

http://www.blog.huajh7.com/adaptive-filters-lms-rls-kalman-filter-1/

## 卡尔曼滤波

>注：Rudolf (Rudi) Emil Kálmán，1930～2016，匈牙利出生的美国科学家。哥伦比亚大学博士（1957），先后执教于斯坦福大学和佛罗里达大学。现代控制理论的里程碑人物，美国科学院院士。   
>卡尔曼滤波从纯数学的角度讲，并没有多大意义。因此，主流数学家们在很长一段时间内，并不承认Kálmán是数学家。只是由于卡尔曼滤波在工程界的巨大影响力，才不得不于2012年，授予其美国数学协会院士。

Kalman filters是一种高斯线性滤波器。

参考：

http://www.cs.unc.edu/~welch/media/pdf/kalman_intro.pdf

Gregory Francis Welch写的卡尔曼滤波科普文。

>注：Gregory Francis Welch，北卡罗莱娜大学博士（1997）。中佛罗里达大学教授。

http://www.cs.unc.edu/~welch/kalman/media/misc/kalman_intro_chinese.zip

上文的中文版。

https://zhuanlan.zhihu.com/p/21294526

知乎诸位大神的科普文。

http://www.docin.com/p-976961701.html

动态相对定位中自适应滤波方法的研究

《自适应动态导航定位》，杨元喜著。

>注：杨元喜，1956年生，大地测量学家。中国科学院院士。

# loss function详解

**Mean Squared Error(MSE)/Mean Squared Deviation(MSD)**

$$\operatorname{MSE}=\frac{1}{n}\sum_{i=1}^n(\hat{Y_i} - Y_i)^2$$

**Symmetric Mean Absolute Percentage Error(SMAPE or sMAPE)**

MSE定义的误差，实际上是向量空间中的欧氏距离，这也可称为绝对误差。而有些情况下，可能相对误差（即百分比误差）更有意义些：

$$\text{SMAPE} = \frac 1 n \sum_{t=1}^n \frac{\left|F_t-A_t\right|}{(A_t+F_t)/2}$$

上式的问题在于$$A_t+F_t\le 0$$时，该值无意义。为了解决该问题，可用如下变种：

$$\text{SMAPE} = \frac{100\%}{n} \sum_{t=1}^n \frac{|F_t-A_t|}{|A_t|+|F_t|}$$

**Mean Absolute Error(MAE)**

$$\mathrm{MAE} = \frac{1}{n}\sum_{i=1}^n \left| f_i-y_i\right| =\frac{1}{n}\sum_{i=1}^n \left| e_i \right|$$

这个可以看作是MSE的1范数版本。

**Mean Percentage Error(MPE)**

$$\text{MPE} = \frac{100\%}{n}\sum_{t=1}^n \frac{a_t-f_t}{a_t}$$

# HMM

![](/images/article/HMM.png)

![](/images/article/HMM_2.png)

![](/images/article/HMM_3.png)

和HMM模型相关的算法主要分为三类，分别解决三种问题：

1）**知道骰子有几种（隐含状态数量），每种骰子是什么（转换概率），根据掷骰子掷出的结果（可见状态链），我想知道每次掷出来的都是哪种骰子（隐含状态链）。**

这个问题呢，在语音识别领域呢，叫做解码问题。这个问题其实有两种解法，会给出两个不同的答案。每个答案都对，只不过这些答案的意义不一样。第一种解法求最大似然状态路径，说通俗点呢，就是我求一串骰子序列，这串骰子序列产生观测结果的概率最大。第二种解法呢，就不是求一组骰子序列了，而是求每次掷出的骰子分别是某种骰子的概率。比如说我看到结果后，我可以求得第一次掷骰子是D4的概率是0.5，D6的概率是0.3，D8的概率是0.2。

2）**还是知道骰子有几种（隐含状态数量），每种骰子是什么（转换概率），根据掷骰子掷出的结果（可见状态链），我想知道掷出这个结果的概率。**

看似这个问题意义不大，因为你掷出来的结果很多时候都对应了一个比较大的概率。问这个问题的目的呢，其实是检测观察到的结果和已知的模型是否吻合。如果很多次结果都对应了比较小的概率，那么就说明我们已知的模型很有可能是错的，有人偷偷把我们的骰子給换了。问题2的更一般的用法是：从若干种模型中选择一个概率最大的模型。

3）**知道骰子有几种（隐含状态数量），不知道每种骰子是什么（转换概率），观测到很多次掷骰子的结果（可见状态链），我想反推出每种骰子是什么（转换概率）。**

这个问题很重要，因为这是最常见的情况。很多时候我们只有可见结果，不知道HMM模型里的参数，我们需要从可见结果估计出这些参数，这是建模的一个必要步骤。

参考：

https://www.zhihu.com/question/20962240

如何用简单易懂的例子解释隐马尔可夫模型？

http://www.cnblogs.com/kaituorensheng/archive/2012/11/29/2795499.html

隐马尔可夫模型

## Viterbi算法

Viterbi算法是求解最大似然状态路径的常用算法，被广泛应用于通信和NLP领域。

>注：Andrew James Viterbi，1935年生，意大利裔美国工程师、企业家，高通公司联合创始人。MIT本硕+南加州大学博士。viterbi算法和CDMA标准的主要发明人。

![](/images/article/HMM_4.png)

上图是一个HMM模型的概率图表示，其中{'Healthy','Fever'}是隐含状态，而{'normal','cold','dizzy'}是可见状态，边是各状态的转移概率。

![](/images/article/Viterbi_animated_demo.gif)

上图是Viterbi算法的动画图。简单来说就是，从开始状态之后的每一步，都选择最大似然状态的路径。由于每一步都是最优方案，因此整个路径也是最优路径。

## 前向算法

forward算法是求解问题2的常用算法。

仍以上面的掷骰子为例，要算用正常的三个骰子掷出这个结果的概率，其实就是将所有可能情况的概率进行加和计算。同样，简单而暴力的方法就是把穷举所有的骰子序列，还是计算每个骰子序列对应的概率，但是这回，我们不挑最大值了，而是把所有算出来的概率相加，得到的总概率就是我们要求的结果。

穷举法的计算量太大，不适用于计算较长的马尔可夫链。但是我们可以观察一下穷举法的计算步骤。

![](/images/article/forward_algorithm.png)

上图是某骰子序列的穷举计算过程，可以看出第3步计算的概率和公式的某些项，实际上在之前的步骤中已经计算出来了，前向递推的计算量并没有想象中的大。

# 概率图模型

probabilistic graphical model（PGM）最早由Judea Pearl发明。

这方面比较重要的文章和书籍有：

http://www.cis.upenn.edu/~mkearns/papers/barbados/jordan-tut.pdf

Michael Irwin Jordan著。

《Probabilistic Graphical Models: Principles and Techniques》，Daphne Koller，Nir Friedman著（2009年）。

>注：Judea Pearl，1936年生，以色列-美国计算机科学家，UCLA教授。2011年获得图灵奖。

>Michael Irwin Jordan，1956年生，美国计算机科学家。UCSD博士，先后执教于MIT和UCB。吴恩达的导师。

>Daphne Koller，女，1968年生，以色列-美国计算机科学家。斯坦福大学博士及教授。和吴恩达共同创立在线教育平台Coursera。

>Nir Friedman，1967年生，以色列计算机科学家。斯坦福大学博士，耶路撒冷希伯来大学教授。

http://www.cs.cmu.edu/~epxing/Class/10708-14/lectures/

CMU的邢波（Eric Xing）所开的概率图模型课程。

## 概述

概率图模型的三要素：Graph：$$\mathcal{G}$$、Model：$$\mathcal{M}$$和Data：$$\mathcal{D}\equiv\{X^{(i)}_1,\dots,X^{(i)}_m\}^N_{i=1}$$。

它要解决的三大问题：

1.**表示**。如何获取或定义真实世界的不确定度？如何对领域知识/假设/约束编码？

2.**推断**。根据模型/数据，推断答案。

$$\text{e.g.}:P(x_i|\mathcal{D})$$

3.**学习**。根据数据确定哪个模型是正确的。

$$\text{e.g.}:\mathcal{M}=\arg\max_{\mathcal{M}\in M}F(\mathcal{D};\mathcal{M})$$

![](/images/article/PGM.png)

上图是PGM的一个示例。其中$$X_i$$表示随机变量，图中共有8个随机变量，假设它们均为二值变量，则整个状态空间共有$$2^8$$种组合。遍历这样大的状态空间无疑是一件极为费力的事情。

如果$$X_i$$是条件独立的话，则由上图可得：

$$P(X_1,\dots,X_8)=P(X_2)P(X_4|X_2)P(X_5|X_2)P(X_1)P(X_3|X_1)\\P(X_6|X_3,X_4)P(X_7|X_6)P(X_8|X_5,X_6)$$

这样，状态空间就缩小为$$2+4+4+2+4+8+4+8=36$$种组合了。

根据边的类型，PGM可分为两类：

1.有向边表示变量间的**因果**关系。这样的PGM，常称为Bayesia Network（BN）或Directed Graphical Model（DGM）。

2.无向边表示变量间的**相关**关系。这样的PGM，常称为Markov Random Field（MRF）或Undirected Graphical model（UGM）。

>注：因果关系是一种强逻辑关系，需要变量间有深刻的内在联系。而相关关系要弱的多，典型的例子就是《机器学习（十七）》中的尿布和啤酒的故事。尿布和啤酒虽然正相关，然而它们本身却没有多大的联系。

根据模型的不同，PGM又可分为生成模型（Generative Model, GM）和判别模型（Discriminative Model, DM）。两者的区别在《机器学习（二）》中已经简单提到过，这里做一个扩展。

之前提到的机器学习算法，主要是建立特征向量X和标签Y之间的联系。但是实际情况下，X中的状态不一定都能得到，因此可以根据可见性，将X分为可观测变量集合O和其他变量集合R，Y也不一定是一个标签，而可能是一个变量集合。即：

$$GM:P(Y,R,O)\to P(Y|O)$$

$$DM:P(Y,R|O)\to P(Y|O)$$

注意，在贝叶斯学派的观点中，模型的参数也是随机变量，因此，R在某些情况下，不仅包含不可观测的变量，也包含模型参数。

## 贝叶斯网络

贝叶斯网络是最简单的有向图模型。

首先给出几个术语的定义：

**有向无环图(Directed Acyclic Graph, DAG)**：这个术语的字面意思很清楚，不解释。

**马尔可夫毯(Markov Blanket, MB)**：有向图——结点A的父结点+A的子结点+A的子结点的其他父结点。如下图所示：

![](/images/article/Markov_blanket.png)

无向图——结点A的邻近结点。

下图是图模型的部分变种之间的关系图。

![](/images/article/Generative_Models.png)

# CRF

条件随机场(Conditional Random Field)由Lafferty等人于2001年提出，结合了最大熵模型和隐马尔可夫模型的特点，是一种无向图模型，近年来在分词、词性标注和命名实体识别等序列标注任务中取得了很好的效果。

# 异常点检测

http://chuansong.me/n/377440751130

http://jiangshuxia.9.blog.163.com/blog/static/3487586020083662621887/

http://www.cnblogs.com/fengfenggirl/p/iForest.html

