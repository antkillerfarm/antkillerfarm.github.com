---
layout: post
title:  机器学习（二十一）——loss function详解, 机器学习分类器性能指标, EMD, LSA
category: ML 
---

# PageRank算法（续）

## 简易推导

![](/images/article/page_rank.jpg)

上图是一个Web图模型的示例。其中的节点表示网页，箭头表示网页链接。因此，从图论的角度来说，这是一个有向图。而从随机过程的角度，这也可以看做是一个Markov链。

上图中，A有两个入链B和C，则：

$$PR(A)=PR(B)+PR(C)$$

然而图中除了C之外，B和D都不止有一条出链，所以上面的计算式并不准确：

$$PR(A) = \frac{PR(B)}{2} + \frac{PR(C)}{1}$$

一般化，即：

$$PR(A)= \frac{PR(B)}{L(B)}+ \frac{PR(C)}{L(C)}$$

其中，L表示外链个数。

更一般化，可得：

$$PR(u) = \sum_{v \in B_u} \frac{PR(v)}{L(v)}$$

这里有两种异常情况需要处理。

1.互联网中不乏一些没有出链的网页，为了满足Markov链的收敛性，设定其对所有的网页（包括它自己）都有出链。

2.互联网中一个网页只有对自己的出链，或者几个网页的出链形成一个循环圈。那么在不断地迭代过程中，这一个或几个网页的PR值将只增不减，显然不合理。

对于这种情况，我们假定有一个确定的概率$$\alpha$$会输入网址直接跳转到一个随机的网页，并且跳转到每个网页的概率是一样的。即：

$$PR(p_{i}) = \alpha \sum_{p_{j} \in M_{p_{i}}} \frac{PR(p_{j})}{L(p_{j})} + \frac{(1 - \alpha)}{N}$$

$$\alpha$$也叫阻尼系数，一般设定为0.85。

由Markov链的收敛性可知，无论每个网页的PR初始值如何设定，都不影响最终的PR值。

在实际计算中，由于网页数量众多，而其中的链接关系相对较少，因此这个计算过程，实际上是一个巨维稀疏矩阵的凸优化问题，此处不再赘述。



## TextRank

TextRank算法是PageRank算法在NLP领域的扩展，被广泛用于自动摘要和提取关键词。

将原文本拆分为句子，在每个句子中过滤掉停用词（可选），并只保留指定词性的单词（可选）。由此可以得到句子的集合和单词的集合。

每个单词作为TextRank中的一个节点。假设一个句子依次由下面的单词组成：$$w_1,\dots,w_n$$。从中取出k个连续的单词序列，组成一个窗口。我们认为窗口中任意两个单词间存在一个无向边，从而构建出一个图模型。

对该图模型应用PageRank算法，可得：

$$WS(V_i)=(1-d)+d\sum_{V_j \in In(V_i)}\frac{w_{ji}}{\sum_{V_k \in Out(V_j)}w_{jk}}WS(V_j)$$

上式的W为权重（也可叫做结点相似度），一般采用以下定义：

$$W(S_i,S_j)=\frac{|\{w_k|w_k\in S_i \& w_k\in S_j\}|}{\log(|S_i|)+\log(|S_j|)}$$

其中，$$\mid S_i\mid$$是句子i的单词数。

上面说的是关键词的计算方法。计算自动摘要的时候，将句子定义为结点，并认为全部句子都是相邻的即可。自动摘要所用的权重函数，一般采用BM25算法。

## 参考

http://www.cnblogs.com/rubinorth/p/5799848.html

PageRank算法--从原理到实现

http://blog.csdn.net/hguisu/article/details/7996185

PageRank算法

http://www.docin.com/p-1231683333.html

有限不可约马尔可夫链的非周期状态

http://www.docin.com/p-630952720.html

马尔科夫链

https://mp.weixin.qq.com/s/J9OmqFzQK-GS95FjgAJkTw

浅析PageRank算法

# loss function详解

## Mean Squared Error(MSE)/Mean Squared Deviation(MSD)

$$\operatorname{MSE}=\frac{1}{n}\sum_{i=1}^n(\hat{Y_i} - Y_i)^2$$

## Symmetric Mean Absolute Percentage Error(SMAPE or sMAPE)

MSE定义的误差，实际上是向量空间中的欧氏距离，这也可称为绝对误差。而有些情况下，可能相对误差（即百分比误差）更有意义些：

$$\text{SMAPE} = \frac 1 n \sum_{t=1}^n \frac{\left|F_t-A_t\right|}{(A_t+F_t)/2}$$

上式的问题在于$$A_t+F_t\le 0$$时，该值无意义。为了解决该问题，可用如下变种：

$$\text{SMAPE} = \frac{100\%}{n} \sum_{t=1}^n \frac{|F_t-A_t|}{|A_t|+|F_t|}$$

## Mean Absolute Error(MAE)

$$\mathrm{MAE} = \frac{1}{n}\sum_{i=1}^n \left| f_i-y_i\right| =\frac{1}{n}\sum_{i=1}^n \left| e_i \right|$$

这个可以看作是MSE的1范数版本。

## Mean Percentage Error(MPE)

$$\text{MPE} = \frac{100\%}{n}\sum_{t=1}^n \frac{a_t-f_t}{a_t}$$

不同的loss函数有不同的用途，比如softmax一般用Cross Entropy作为loss函数。如下图所示：

![](/images/article/cross_vs_mse.png)

## 参考

https://mp.weixin.qq.com/s/gw3hoDSaojVQUiD6YsMabA

理解神经网络中的目标函数

https://mp.weixin.qq.com/s/h-QbwEbaivvHjdhDhE4V1A

如何为单变量模型选择最佳的回归函数

# 机器学习分类器性能指标

很多学习器是为测试样本产生一个实值或概率预测，然后将这个预测值与一个分类阈值（threshold）进行比较，若大于阈值则分为正类，否则为反类。这个实值或概率预测结果的好坏，直接决定了学习器的泛化能力。实际上，根据这个实值或概率预测结果，我们可将测试样本进行**排序**，“最可能”是正例的排在最前面，“最不可能”是正例的排在最后面。这样，分类过程就相当于在这个排序中以某个“截断点”（cut point）将样本分为两部分，前一部分判作正例，后一部分则判作反例。

在不同的应用任务中，我们可根据任务需求来采用不同的截断点，例如若我们更重视 “查准率”（precision），则可选择排序中靠前的位置进行截断，若更重视“查全率”（recall，也称召回率），则可选择靠后的位置进行截断。

对于二分类问题，可将样例根据其真实类别与学习器预测类别的组合划分为真正例（true positive）、假正例（false positive）、真反例（true negative）和假反例（false negative）。

查准率P和查全率R的定义如下：

$$P=\frac{TP}{TP+FP},R=\frac{TP}{TP+FN}$$

以P和R为坐标轴，所形成的曲线就是P-R曲线。曲线下方的面积一般称为AP（Average Precision）。

![](/images/article/AP.png)

>注意：   
>1.测试样本的**排序**过程非常重要。不然P-R曲线的峰值可能出现在图形的中部。   
>2.虽然P-R曲线总体上是个下降曲线，但不是严格的单调下降曲线。在局部，会由于TP样本的增多，使P值升高。

ROC（Receiver operating characteristic）曲线的纵轴是真正例率（True Positive Rate，TPR），横轴是假正例率（False Positive Rate，FPR）。其定义如下：

$$TPR=\frac{TP}{TP+FN},FPR=\frac{FP}{TN+FP}$$

ROC曲线下方的面积被称为AUC（Area Under ROC Curve）。

![](/images/article/ROC.gif)

更多内容参见下图：

![](/images/article/sensitivity_and_specificity.png)

原图地址：

https://en.wikipedia.org/wiki/Sensitivity_and_specificity

除此之外，还有F-measure：

![](/images/article/P_R_F.gif)

如果是做搜索，那就是保证召回的情况下提升准确率；如果做疾病监测、反垃圾，则是保准确率的条件下，提升召回率。所以，在两者都要求高的情况下，可以用F-measure来衡量。

Accuracy和Precision是一对容易混淆的概念。其一般定义如下图所示：

![](/images/article/Accuracy_and_precision.svg)

当然，这个定义和机器学习中的定义无关，主要用于物理和统计领域。

这里再提几个和上述概念类似的ML术语。

**Ground truth**：在有监督学习中，数据是有标注的，以(x, t)的形式出现，其中x是输入数据，t是标注。正确的t标注是ground truth， 错误的标记则不是。（也有人将所有标注数据都叫做ground truth）

由于使用错误的数据，对模型的估计比实际要糟糕，因此使用高质量的数据是很有必要的。

**Golden**：Golden这个术语的使用范围并不局限于ML领域，凡是能够给出“标准答案”的地方，都可以将该答案称为Golden。

比如在DL领域的硬件优化中，通常使用标准算法生成Golden结果，然后用优化之后的运算结果与之比对，以验证优化的正确性。

参考：

https://mp.weixin.qq.com/s/5nnHBKEToepi3dhXLfQBtw

机器学习分类器性能指标详解

https://mp.weixin.qq.com/s/6eESoUvMObXSb2jy_KPRyg

如何评价我们分类模型的性能？

https://mp.weixin.qq.com/s/mOYUCc3xKMfVw81B6zSeNw

7种最常用的机器学习算法衡量指标

# Earth mover's distance

推土机距离（EMD）是两个概率分布之间的距离度量的一种方式。如果将区间D的概率分布比作沙堆P，那么$$P_r$$和$$P_\theta$$之间的EMD距离，就是推土机将$$P_r$$改造为$$P_\theta$$所需要的工作量。

![](/images/article/earth_move.png)

EMD的计算公式为：

$$EMD(P_r,P_\theta) = \frac{\sum_{i=1}^m \sum_{j=1}^n f_{i,j}d_{i,j}}{\sum_{i=1}^m \sum_{j=1}^n f_{i,j}}$$

其中，f表示土方量，d表示运输距离。

EMD可以是多维分布之间的距离。一维的EMD也被称为Match distance。

EMD有时也称作Wasserstein距离。

在文本处理中，有一个和EMD类似的编辑距离（Edit distance），也叫做Levenshtein distance。它是指两个字串之间，由一个转成另一个所需的最少编辑操作次数。许可的编辑操作包括将一个字符替换成另一个字符，插入一个字符，删除一个字符。一般来说，编辑距离越小，两个串的相似度越大。

>注：严格来说，Edit distance是一系列字符串相似距离的统称。除了Levenshtein distance之外，还包括Hamming distance等。

>Vladimir Levenshtein，1935年生，俄罗斯数学家，毕业于莫斯科州立大学。2006年获得IEEE Richard W. Hamming Medal。

参考：

https://vincentherrmann.github.io/blog/wasserstein/

http://chaofan.io/archives/earth-movers-distance-%e6%8e%a8%e5%9c%9f%e6%9c%ba%e8%b7%9d%e7%a6%bb

# LSA

## 基本原理

Latent Semantic Analysis（隐式语义分析），也叫Latent Semantic Indexing。它是PCA算法在NLP领域的一个应用。

在TF-IDF模型中，所有词构成一个高维的语义空间，每个文档在这个空间中被映射为一个点，这种方法维数一般比较高而且每个词作为一维割裂了词与词之间的关系。

为了解决这个问题，我们要把词和文档同等对待，构造一个维数不高的语义空间，每个词和每个文档都是被映射到这个空间中的一个点。

LSA的思想就是说，我们考察的概率既包括文档的概率，也包括词的概率，以及他们的联合概率。

为了加入语义方面的信息，我们设计一个假想的隐含类包括在文档和词之间，具体思路是这样的：

1.选择一个文档的概率是$$p(d)$$

2.找到一个隐含类的概率是$$p(z\mid d)$$

3.生成一个词w的概率为$$p(w\mid z)$$

