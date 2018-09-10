---
layout: post
title:  机器学习（二十一）——loss function详解, 机器学习分类器性能指标
category: ML 
---

# PageRank算法

## 概述（续）

**优点**：

这是一个与查询无关的静态算法，所有网页的PageRank值通过离线计算获得；有效减少在线查询时的计算量，极大降低了查询响应时间。

**缺点**：

1）人们的查询具有主题特征，PageRank忽略了主题相关性，导致结果的相关性和主题性降低

2）旧的页面等级会比新页面高。因为即使是非常好的新页面也不会有很多上游链接，除非它是某个站点的子站点。

## 马尔可夫链

Markov链的基本定义参见《机器学习（十六）》。

这里补充一些定义：

**定义1**：设C为状态空间的一个子集，如果从C内任一状态i不能到C外的任何状态，则称C为**闭集**。除了整个状态空间之外，没有别的闭集的Markov链被称为**不可约的**。

如果用状态转移图表示Markov链的话，上面的定义表征了Markov链的**连通性**。

**定义2**：如果有正整数d，只有当$$n=d,2d,\dots$$时，$$P_{ii}^{(n)}>0$$，或者说当n不能被d整除时，$$P_{ii}^{(n)}=0$$，则称i状态为**周期性状态**。如果除了$$d=1$$之外，使$$P_{ii}^{(n)}>0$$的各n值没有公约数，则称该状态i为**非周期性状态**。

这个定义表征了Markov链各状态的**独立性**。

**定义3**：

$$f_{ij}^{(n)}=P(X_{m+v}\neq j,X_{m+n}=j|X_m=i)$$

其中，$$n>1,1\le v\le n-1$$。

上式表示由i出发，首次到达j的概率，也叫**首中概率**。

相应的还有**最终概率**：

$$f_{ij}=\sum_{n=1}^\infty f_{ij}^{(n)}$$

**定义4**：

如果$$f_{ii}=1$$, 则称状态i为**常返**的，如果$$f_{ii}<1$$, 则称状态i为**非常返**的。

令$$u_i=\sum_{n=1}^\infty nf_{ii}^{(n)}$$，则$$u_i$$表示由i出发i，再返回i的**平均返回时间**。

如果$$u_i=\infty$$，则称i为**零常返**的。

常返态表征Markov链的极限分布。显然如果长期来看，状态i“入不敷出”的话，则其最终的极限概率为0。

根据上面的定义，还可得到Markov链的三个推论：

**推论1**：有限状态的不可约非周期Markov链必存在平稳分布。

**推论2**：若不可约Markov链的所有状态是非常返或零常返的，则不存在平稳分布。

**推论3**：若$$X_j$$是不可约的非周期的Markov链的平稳分布，则$$\lim_{n\to\infty}P_{ij}^{(n)}=X_j$$，即极限分布等于平稳分布。

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

![](/images/img2/loss.png)

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

https://mp.weixin.qq.com/s/qXZMo_RitSenmI7x0xGNsg

中科院自动化所多媒体计算与图形学团队NIPS 2017论文提出平均Top-K损失函数，专注于解决复杂样本

https://mp.weixin.qq.com/s/kI22wSoyNT3QXXI8pVwbjA

腾讯AI Lab提出新型损失函数LMCL：可显著增强人脸识别模型的判别能力

https://mp.weixin.qq.com/s/YOdmv88koSHx5AMMEQZGgg

通俗聊聊损失函数中的均方误差以及平方误差

https://blog.csdn.net/zhangxb35/article/details/72464152

pytorch loss function总结

https://mp.weixin.qq.com/s/Xbi5iOh3xoBIK5kVmqbKYA

机器学习大牛是如何选择回归损失函数的？

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

