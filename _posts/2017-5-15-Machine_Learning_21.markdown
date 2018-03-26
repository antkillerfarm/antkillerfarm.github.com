---
layout: post
title:  机器学习（二十一）——loss function详解, 机器学习分类器性能指标
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

## loss function比较

![](/images/article/loss_function.png)

这里m代表了置信度，越靠近右边置信度越高。

其中蓝色的阶跃函数又被称为Gold Standard，黄金标准，因为这是最准确无误的分类器loss function了。分对了loss为0，分错了loss为1，且loss不随到分界面的距离的增加而增加，也就是说这个分类器非常鲁棒。但可惜的是，它不连续，求解这个问题是NP-hard的，所以才有了各种我们熟知的分类器。

其中红色线条就是SVM了，由于它在m=1处有个不可导的转折点，右边都是0，所以分类正确的置信度超过一定的数之后，对分界面的确定就没有一点贡献了。

《机器学习（五）》中提到的SVM软间隔，其所使用的loss function，又被称为Hinge loss函数：

$$l_{hinge}(z)=\max(0,1-z)$$

除此之外，exponential loss函数：

$$l_{exp}(z)=\exp(-z)$$

和logistic loss函数：

$$l_{log}(z)=\log(1+\exp(-z))$$

也是较常用的SVM loss function。

黄色线条是Logistic Regression的损失函数，与SVM不同的是，它非常平滑，但本质跟SVM差别不大。

绿色线条是boost算法使用的损失函数。

黑色线条是ELM（Extreme learning machine）算法的损失函数。它的优点是有解析解，不必使用梯度下降等迭代方法，可直接计算得到最优解。但缺点是随着分类的置信度的增加，loss不降反升，因此，最终准确率有限。此外，解析算法相比迭代算法，对于大数据的适应较差，这也是该方法的局限所在。

参见：

https://www.zhihu.com/question/28810567

Extreme learning machine(ELM)到底怎么样，有没有做的前途？

## 参考

https://mp.weixin.qq.com/s/gw3hoDSaojVQUiD6YsMabA

理解神经网络中的目标函数

https://mp.weixin.qq.com/s/h-QbwEbaivvHjdhDhE4V1A

如何为单变量模型选择最佳的回归函数

https://mp.weixin.qq.com/s/XB9VsW3NRwHua6AdRL3n8w

Lossless Triplet Loss:一种高效的Siamese网络损失函数

https://mp.weixin.qq.com/s/qXZMo_RitSenmI7x0xGNsg

中科院自动化所多媒体计算与图形学团队NIPS 2017论文提出平均Top-K损失函数，专注于解决复杂样本

https://mp.weixin.qq.com/s/kI22wSoyNT3QXXI8pVwbjA

腾讯AI Lab提出新型损失函数LMCL：可显著增强人脸识别模型的判别能力

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

https://mp.weixin.qq.com/s/zvxB6VqrSOosgGSViCmjEQ

不止准确率：为分类任务选择正确的机器学习度量指标

