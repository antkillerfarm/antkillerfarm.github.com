---
layout: post
title:  机器学习（二十二）——EMD, LSA, HMM
category: ML 
---

# 机器学习分类器性能指标（续）

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

https://mp.weixin.qq.com/s/2HKx36bIBZAqvzdXfcSfqA

我们常听说的置信区间与置信度到底是什么？

# Earth mover's distance

推土机距离（EMD）是两个概率分布之间的距离度量的一种方式。如果将区间D的概率分布比作沙堆P，那么$$P_r$$和$$P_\theta$$之间的EMD距离，就是推土机将$$P_r$$改造为$$P_\theta$$所需要的工作量。

![](/images/article/earth_move.png)

EMD的计算公式为：

$$EMD(P_r,P_\theta) = \frac{\sum_{i=1}^m \sum_{j=1}^n f_{i,j}d_{i,j}}{\sum_{i=1}^m \sum_{j=1}^n f_{i,j}}$$

其中，f表示土方量，d表示运输距离。

EMD可以是多维分布之间的距离。一维的EMD也被称为Match distance。

EMD有时也称作Wasserstein距离。

>Leonid Vaseršteĭn，俄罗斯数学家，Moscow State University硕博，现居美国，Penn State University教授。Wasserstein是他名字的德文拼法，并为英文文献所沿用。他在去美国之前，曾在德国住过一段时间。

在文本处理中，有一个和EMD类似的编辑距离（Edit distance），也叫做Levenshtein distance。它是指两个字串之间，由一个转成另一个所需的最少编辑操作次数。许可的编辑操作包括将一个字符替换成另一个字符，插入一个字符，删除一个字符。一般来说，编辑距离越小，两个串的相似度越大。

>注：严格来说，Edit distance是一系列字符串相似距离的统称。除了Levenshtein distance之外，还包括Hamming distance等。

>Vladimir Levenshtein，1935年生，俄罗斯数学家，毕业于莫斯科州立大学。2006年获得IEEE Richard W. Hamming Medal。

参考：

https://vincentherrmann.github.io/blog/wasserstein/

Wasserstein GAN and the Kantorovich-Rubinstein Duality

http://chaofan.io/archives/earth-movers-distance-%e6%8e%a8%e5%9c%9f%e6%9c%ba%e8%b7%9d%e7%a6%bb

Earth Mover's Distance——推土机距离

https://mp.weixin.qq.com/s/rvPLYa1NFg_LRvb8Y8-aCQ

Wasserstein距离在生成模型中的应用

https://mp.weixin.qq.com/s/2xOrSyyWSbp8rBVbFoNrxQ

Wasserstein is all you need：构建无监督表示的统一框架

https://mp.weixin.qq.com/s/NXDJ4uCpdX-YcWiKAsjJLQ

传说中的推土机距离基础，最优传输理论了解一下

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

## 实现方法

![](/images/article/Topic_model_scheme.jpg)

上图中，行表示单词，列表示文档，单元格的值表示单词在文档中的权重，一般可由TF-IDF生成。

聪明的读者看到这里应该已经反应过来了，这不就是《机器学习（十四）》中提到的协同过滤的商品打分矩阵吗？

没错！LSA的实现方法的确与之类似。多数的blog讲解LSA算法原理时，由于单词-文档矩阵较小，直接采用了矩阵的SVD分解，少数给出了EM算法实现，实际上就是ALS或其变种。

参考：

http://www.cnblogs.com/kemaswill/archive/2013/04/17/3022100.html

Latent Semantic Analysis(LSA/LSI)算法简介

http://blog.csdn.net/u013802188/article/details/40903471

隐含语义索引（Latent Semantic Indexing）

http://www.shareditor.com/blogshow/?blogId=90

比TF-IDF更好的隐含语义索引模型是个什么鬼

http://shiyanjun.cn/archives/548.html

使用libsvm+tfidf实现文本分类

https://mp.weixin.qq.com/s/iZOVUYKWP-fN8BwAuVwAUw

TF-IDF不容小觑

# HMM

![](/images/article/HMM.png)

![](/images/article/HMM_2.png)

![](/images/article/HMM_3.png)

上图中的隐含状态，也称states。可见状态，也称observations。黑线被称为transition probability，而红线则是emission probability或output probability。第一个observations的概率被称为start probability。

因此一个HMM可以表示为：

$$\mu=(A,B,\Pi)$$

其中，A是transition probability，B是emission probability，$$\Pi$$是start probability。

如果可见状态和隐含状态之间，存在一对一的关系，那么HMM就退化成普通的Markov链了。

和HMM（Hidden Markov Model，隐马尔可夫模型）模型相关的算法主要分为三类，分别解决三种问题：

1）**知道骰子有几种（隐含状态数量），每种骰子是什么（转换概率），根据掷骰子掷出的结果（可见状态链），我想知道每次掷出来的都是哪种骰子（隐含状态链）。**

这个问题呢，在语音识别领域呢，叫做解码问题。这个问题其实有两种解法，会给出两个不同的答案。每个答案都对，只不过这些答案的意义不一样。第一种解法求最大似然状态路径，说通俗点呢，就是我求一串骰子序列，这串骰子序列产生观测结果的概率最大。第二种解法呢，就不是求一组骰子序列了，而是求每次掷出的骰子分别是某种骰子的概率。比如说我看到结果后，我可以求得第一次掷骰子是D4的概率是0.5，D6的概率是0.3，D8的概率是0.2。

2）**还是知道骰子有几种（隐含状态数量），每种骰子是什么（转换概率），根据掷骰子掷出的结果（可见状态链），我想知道掷出这个结果的概率。**

看似这个问题意义不大，因为你掷出来的结果很多时候都对应了一个比较大的概率。问这个问题的目的呢，其实是检测观察到的结果和已知的模型是否吻合。如果很多次结果都对应了比较小的概率，那么就说明我们已知的模型很有可能是错的，有人偷偷把我们的骰子給换了。问题2的更一般的用法是：从若干种模型中选择一个概率最大的模型。

3）**知道骰子有几种（隐含状态数量），不知道每种骰子是什么（转换概率），观测到很多次掷骰子的结果（可见状态链），我想反推出每种骰子是什么（转换概率）。**

这个问题很重要，因为这是最常见的情况。很多时候我们只有可见结果，不知道HMM模型里的参数，我们需要从可见结果估计出这些参数，这是训练建模的一个必要步骤。

参考：

https://www.zhihu.com/question/20962240

如何用简单易懂的例子解释隐马尔可夫模型？

http://www.cnblogs.com/kaituorensheng/archive/2012/11/29/2795499.html

隐马尔可夫模型

https://mp.weixin.qq.com/s/9MmHDVDal57pdotwxAn_uQ

HMM模型详解

https://mp.weixin.qq.com/s/PzPRKZa1C5IlEUASWt71cA

从朴素贝叶斯到维特比算法：详解隐马尔科夫模型

## Viterbi算法

Viterbi算法是求解最大似然状态路径的常用算法，被广泛应用于通信（CDMA技术的理论基础之一）和NLP领域。

>注：Andrew James Viterbi，1935年生，意大利裔美国工程师、企业家，高通公司联合创始人。MIT本硕+南加州大学博士。viterbi算法和CDMA标准的主要发明人。

![](/images/article/HMM_4.png)

上图是一个HMM模型的概率图表示，其中{'Healthy','Fever'}是隐含状态，而{'normal','cold','dizzy'}是可见状态，边是各状态的转移概率。

![](/images/article/Viterbi_animated_demo.gif)

上图是Viterbi算法的动画图。简单来说就是：从开始状态之后每走一步，就记录下到达该状态的所有路径的概率最大值，然后以此最大值为基准继续向后推进。显然，如果这个最大值都不能使该状态成为最大似然状态路径上的结点的话，那些小于它的概率值（以及对应的路径）就更没有可能了。

Viterbi算法只能求出最佳路径，对于N-best问题就需要进行扩展方可。

参考：

https://mp.weixin.qq.com/s/FQ520ojMmbFhNMoNCVTKug

通俗理解维特比算法

https://www.zhihu.com/question/20136144

谁能通俗的讲解下viterbi算法？

https://mp.weixin.qq.com/s/xyWY3Z5PiHkCFzCP0noBvA

一文读懂HMM模型和Viterbi算法

