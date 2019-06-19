---
layout: post
title:  机器学习（三十八）——PageRank算法, 社交网络, 特征工程, 数据清洗
category: ML 
---

# PageRank算法

## 概述

在PageRank提出之前，已经有研究者提出利用网页的入链数量来进行链接分析计算，这种入链方法假设一个网页的入链越多，则该网页越重要。早期的很多搜索引擎也采纳了入链数量作为链接分析方法，对于搜索引擎效果提升也有较明显的效果。PageRank除了考虑到入链数量的影响，还参考了网页质量因素，两者相结合获得了更好的网页重要性评价标准。

对于某个互联网网页A来说，该网页PageRank的计算基于以下两个基本假设：

**数量假设**：在Web图模型中，如果一个页面节点接收到的其他网页指向的入链数量越多，那么这个页面越重要。

**质量假设**：指向页面A的入链质量不同，质量高的页面会通过链接向其他页面传递更多的权重。所以越是质量高的页面指向页面A，则页面A越重要。

利用以上两个假设，PageRank算法刚开始赋予每个网页相同的重要性得分，通过迭代递归计算来更新每个页面节点的PageRank得分，直到得分稳定为止。PageRank计算得出的结果是网页的重要性评价，这和用户输入的查询是没有任何关系的，即算法是主题无关的。

**优点**：

这是一个与查询无关的静态算法，所有网页的PageRank值通过离线计算获得；有效减少在线查询时的计算量，极大降低了查询响应时间。

**缺点**：

1）人们的查询具有主题特征，PageRank忽略了主题相关性，导致结果的相关性和主题性降低。

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

https://mp.weixin.qq.com/s/fGaEYvo3WYKdzA3r8l6O3g

基于TextRank算法的文本摘要

https://mp.weixin.qq.com/s/0ZNsP7sEfagZhjyaLZohSQ

程序员拒绝单曲循环：曲子只有5分钟，也得不重样播放450多天

# 社交网络

## 信息传播模型

### 感染模型

SI、SIR等模型。

https://www.cnblogs.com/scikit-learn/p/6937326.html

基本的传染病模型：SI、SIS、SIR及其Python代码实现

https://blog.csdn.net/robin_Xu_shuai/article/details/73699207

SI疾病传播模型实现

### 影响力模型

IC、LT等模型。

https://blog.csdn.net/asialee_bird/article/details/79673418

社交网络影响力最大化

http://cjc.ict.ac.cn/online/onlinepaper/wzj-201672182158.pdf

基于社交内容的潜在影响力传播模型

## 链接预测

Link Prediction是指如何通过已知的网络节点以及网络结构等信息预测网络中尚未产生连边的两个节点之间产生链接的可能性。这种预测既包含了对未知链接（exist yet unknown links）的预测也包含了对未来链接（future links）的预测。该问题的研究在理论和应用两个方面都具有重要的意义和价值。

http://www.docin.com/p-810499317.html

社交网络中的链接预测研究

## 社区发现

社区发现（Community Detection）算法用来发现网络中的社区结构，也可以视为一种广义的聚类算法。

https://blog.csdn.net/itplus/article/details/9286905

Community Detection算法

https://www.zhihu.com/question/29042018

社区发现(Community detection)的经典方法有哪些？该领域最新的研究进展如何？

http://www.mapequation.org/index.html

这是一个复杂网络方面的网站，提供了很多算法的代码。例如infomap

https://www.cnblogs.com/nolonely/p/6262508.html

社区发现算法总结（一）

https://www.cnblogs.com/nolonely/p/6268150.html

社区发现算法总结（二）

https://sikasjc.github.io/2017/12/20/GN/

GN算法--复杂网络中社区发现与Python实现

https://www.cnblogs.com/LittleHann/p/9078909.html

社区发现算法-Fast Unfolding（Louvian）算法初探

http://blog.sina.com.cn/s/blog_617032070100er0r.html

Infomap算法描述

https://mp.weixin.qq.com/s/S0vUBFCfizjVe_L4SIrGqQ

网络新闻真假难辨？机器学习来助你一臂之力

https://mp.weixin.qq.com/s/2aKc4yM0b52X-SViGQycwA

大规模网络的社区检测和排序问题综述

# 特征工程

https://mp.weixin.qq.com/s/ibiElLIgrT3wYx3tDYMMTw

理解特征工程

https://mp.weixin.qq.com/s/3Ce8uMf_Kyt-hEZUYfdh3g

特征工程之特征选择

https://mp.weixin.qq.com/s/tOcyfK68jW7Tr-PGCvdXMA

特征工程最后一个要点:特征预处理

https://mp.weixin.qq.com/s/c9iHdgtErVd_iitwny7_zw

Kaggle前1%参赛者经验：特征工程为何如此重要？

https://mp.weixin.qq.com/s/xbPJD0uoRB-T1x09AUYdzg

基于Python的自动特征工程——教你如何自动创建机器学习特征

https://mp.weixin.qq.com/s?__biz=MjM5MTQzNzU2NA==&mid=2651664000&idx=1&sn=ae6dda80df6d6278ae33b7bf7fbadcd2

深度特征合成：自动化特征工程的运作机制

https://mp.weixin.qq.com/s/R1MhoCfnd5drvg2CGLVsPw

哪种特征分析法适合你的任务？Ian Goodfellow提出显著性映射的可用性测试

https://mp.weixin.qq.com/s/XSovbUDVTKe59DDaC1Kl8Q

如何进行特征表达，你知道吗？

https://mp.weixin.qq.com/s/vhr5gXoa0S4-QqFcK7uz-w

模型吞噬特征工程

# 数据清洗

https://mp.weixin.qq.com/s/YrCC8CmP6UKuCmSdF2K_3g

数据挖掘中的数据清洗方法大全

https://mp.weixin.qq.com/s/FHdo2DTapoTryA-hOM-y_w

还在为数据清洗抓狂？这里有一个简单实用的清洗代码集

https://mp.weixin.qq.com/s/r7ngZOM9tO-_OSfvs2aDJw

数据清洗&预处理入门完整指南
