---
layout: post
title:  机器学习（十九）——PageRank算法, loss function详解
category: theory 
---

## 关联规则评价（续）

这条规则的支持度和自信度都满足要求，因此我们很兴奋，我们找到了一条强规则，于是我们建议超市把影片光碟和游戏光碟放在一起，可以提高销量。

可是我们想想，一个喜欢的玩游戏的人会有时间看影片么，这个规则是不是有问题，事实上这条规则误导了我们。在整个数据集中买影片光碟的概率p(买影片)=7500/10000=75%，而买游戏的人也买影片的概率只有66%，66%<75%恰恰说明了买游戏光碟抑制了影片光碟的购买，也就是说买了游戏光碟的人更倾向于不买影片光碟，这才是符合现实的。

从上面的例子我们看到，支持度和自信度并不总能成功滤掉那些我们不感兴趣的规则，因此我们需要一些新的评价标准，下面介绍几种评价标准：

### 相关性系数

相关性系数的英文名是Lift，这就是一个单词，而不是缩写。

$$\mathrm{lift}(X\Rightarrow Y) = \frac{ \mathrm{supp}(X \cup Y)}{ \mathrm{supp}(X) \times \mathrm{supp}(Y) }$$

$$\mathrm{lift}(X\Rightarrow Y)\begin{cases}
>1, & 正相关 \\
=1, & 独立 \\
<1, & 负相关 \\
\end{cases}$$

实际运用中，正相关和负相关都是我们需要关注的，而独立往往是我们不需要的。显然：

$$\mathrm{lift}(X\Rightarrow Y)=\mathrm{lift}(Y\Rightarrow X)$$

### 确信度

Conviction的定义如下：

$$\mathrm{conv}(X\Rightarrow Y) =\frac{ 1 - \mathrm{supp}(Y) }{ 1 - \mathrm{conf}(X\Rightarrow Y)}$$

它的值越大，表明X、Y的独立性越小。

### 卡方系数

卡方系数是与卡方分布有关的一个指标。参见：

https://en.wikipedia.org/wiki/Chi-squared_distribution

$$\chi^2 = \sum_{i=1}^n \frac{(O_i - E_i)^2}{E_i}$$

>注：上式最早是Pearson给出的。

公式中的$$O_i$$表示数据的实际值，$$E_i$$表示期望值，不理解没关系，我们看一个例子就明白了。

| 表2 | 买游戏 | 不买游戏 | 行总计 |
|:--:|:--|:--:|:--|
| 买影片 | 4000(4500) | 3500(3000) | 7500 |
| 不买影片 | 2000(1500) | 500(1000) | 2500 |
| 列总计 | 6000 | 4000 | 10000 |

表2的括号中表示的是期望值。以第1行第1列的4500为例，其计算方法为：7500×6000/10000。

经计算可得表2的卡方系数为555.6。基于置信水平和自由度$$(r-1)*(c-1)=(行数-1)*(列数-1)=1$$，查表得到自信度为(1-0.001)的值为6.63。

555.6>6.63，因此拒绝A、B独立的假设，即认为A、B是相关的，而$$E(买影片，买游戏)=4500>4000$$,因此认为A、B呈负相关。

### 全自信度

$$all\_confidence(A,B)=\frac{P(A\cap B)}{max\{P(A),P(B)\}}\\=min\{P(B|A),P(A|B)\}=min\{confidence(A\to B),confidence(B\to A)\}$$

### 最大自信度

$$max\_confidence(A,B)=max\{confidence(A\to B),confidence(B\to A)\}$$

### Kulc

$$kulc(A,B)=\frac{confidence(A\to B)+confidence(B\to A)}{2}$$

### cosine距离

$$cosine(A,B)=\frac{P(A\cap B)}{sqrt(P(A)*P(B))}=sqrt(P(A|B)*P(B|A))\\=sqrt(confidence(A\to B)*confidence(B\to A))$$

### Leverage

$$Leverage(A,B) = P(A\cap B)-P(A)P(B)$$

### 不平衡因子

imbalance ratio的定义：

$$IR(A,B)=\frac{|support(A)-support(B)|}{(support(A)+support(B)-support(A\cap B))}$$

全自信度、最大自信度、Kulc、cosine，Leverage是不受空值影响的，这在处理大数据集是优势更加明显，因为大数据中空记录更多，根据分析我们推荐使用kulc准则和不平衡因子结合的方法。

参考：

http://www.cnblogs.com/fengfenggirl/p/associate_measure.html

# PageRank算法

## 概述

在PageRank提出之前，已经有研究者提出利用网页的入链数量来进行链接分析计算，这种入链方法假设一个网页的入链越多，则该网页越重要。早期的很多搜索引擎也采纳了入链数量作为链接分析方法，对于搜索引擎效果提升也有较明显的效果。 PageRank除了考虑到入链数量的影响，还参考了网页质量因素，两者相结合获得了更好的网页重要性评价标准。

对于某个互联网网页A来说，该网页PageRank的计算基于以下两个基本假设： 

**数量假设**：在Web图模型中，如果一个页面节点接收到的其他网页指向的入链数量越多，那么这个页面越重要。

**质量假设**：指向页面A的入链质量不同，质量高的页面会通过链接向其他页面传递更多的权重。所以越是质量高的页面指向页面A，则页面A越重要。

利用以上两个假设，PageRank算法刚开始赋予每个网页相同的重要性得分，通过迭代递归计算来更新每个页面节点的PageRank得分，直到得分稳定为止。 PageRank计算得出的结果是网页的重要性评价，这和用户输入的查询是没有任何关系的，即算法是主题无关的。

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

其中，$$\vert S_i\vert$$是句子i的单词数。

上面说的是关键词的计算方法。计算自动摘要的时候，将句子定义为结点，并认为全部句子都是相邻的即可。自动摘要所用的权重函数，一般采用BM25算法。

## 参考

http://www.cnblogs.com/rubinorth/p/5799848.html

http://blog.csdn.net/hguisu/article/details/7996185

http://www.docin.com/p-1231683333.html

http://www.docin.com/p-630952720.html

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

