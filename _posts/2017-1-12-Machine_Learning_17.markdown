---
layout: post
title:  机器学习（十七）——决策树, 推荐系统进阶
category: theory 
---

# 隐式狄利克雷划分（续）

## PLSA

Probabilistic Latent Semantic Analysis是Thomas Hofmann于1999年在UCB读博期间提出的算法。

原文：

https://dslpitt.org/uai/papers/99/p289-hofmann.pdf

示意图：

![](/images/article/plsa-doc-topic-word.jpg)

PLSA将生成文档的过程，分为两个步骤：

1.生成文档的主题。（doc->topic）

2.根据主题生成相关的词.（topic->word）

第m篇文档$$d_m$$中的每个词的生成概率为：

$$p(w|d_m) = \sum_{z=1}^K p(w|z)p(z|d_m) = \sum_{z=1}^K \varphi_{zw} \theta_{mz}$$

## LDA

利用贝叶斯学派的观点改造PLSA，可得：

![](/images/article/lda-dice.jpg)

LDA生成模型包含两个过程：

1.生成第m篇文档中的所有词对应的topics。

$$\overrightarrow{\alpha}\xrightarrow[Dirichlet]{} \overrightarrow{\theta}_m \xrightarrow[Multinomial]{} \overrightarrow{z}_{m}$$

2.K个由topics生成words的独立过程。

$$\overrightarrow{\beta} \xrightarrow[Dirichlet]{} \overrightarrow{\varphi}_k \xrightarrow[Multinomial]{} \overrightarrow{w}_{(k)}$$

因此，总共就是$$M+K$$个Dirichlet-Multinomial共轭结构。

LDA的Gibbs Sampling图示：

![](/images/article/gibbs-path-search.jpg)

LDA模型的目标有两个：

**训练模型**：估计模型中的参数：$$\overrightarrow{\varphi}_1, \cdots, \overrightarrow{\varphi}_K$$和$$\overrightarrow{\theta}_1, \cdots, \overrightarrow{\theta}_M$$。

由于参数$$\overrightarrow{\theta}_m$$是和训练语料中的每篇文档相关的，对于我们理解新的文档并无用处，所以工程上最终存储LDA模型时，一般没有必要保留。

**使用模型**：对于新来的一篇文档$$doc_{new}$$，我们能够计算这篇文档的topic分布$$\overrightarrow{\theta}_{new}$$。

从最终给出的算法可以看出，虽然LDA用到了MCMC和Gibbs Sampling算法，但最终目的并不是生成符合相应分布的随机数，而是求出模型参数$$\overrightarrow{\varphi}$$的值，并用于预测。

## 参考

http://www.arbylon.net/publications/text-est.pdf

《Parameter estimation for text analysis》，Gregor Heinrich著

http://www.inference.phy.cam.ac.uk/itprnn/book.pdf

《Information Theory, Inference, and Learning Algorithms》，David J.C. MacKay著

关于MCMC和Gibbs Sampling的更多的内容，可参考《Neural Networks and Learning Machines》，Simon Haykin著。该书有中文版。

>注：Sir David John Cameron MacKay，1967～2016，加州理工学院博士，导师John Hopfield，剑桥大学教授。英国能源与气候变化部首席科学顾问，英国皇家学会会员。在机器学习领域和可持续能源领域有重大贡献。

>Simon Haykin，英国伯明翰大学博士，加拿大McMaster University教授。初为雷达和信号处理专家，80年代中后期，转而从事神经计算方面的工作。加拿大皇家学会会员。

http://www.cs.cmu.edu/~epxing/Class/10708-14/lectures/lecture17-MCMC.pdf

# 决策树

Decision Tree讲的最好的，首推周志华的《机器学习》。这里只对要点进行备忘。

当前样本集合D中，第k类样本所占的比例为$$p_k(k=1,2,\dots,\vert y\vert)$$，则D的信息熵（information entropy）定义为：

$$Ent(D)=-\sum_{k=1}^{|y|}p_k\log_2p_k$$

假定离散属性a有V个可能的取值，若使用a对D进行划分，则第v个分支结点包含了D中所有在a上取值$$a^v$$的样本，记为$$D^v$$。则信息增益（information gain）为：

$$Gain(D,a)=Ent(D)-\sum_{v=1}^V\frac{|D^v|}{|D|}Ent(D^v)$$

增益率（gain ratio）：

$$Gain\_ratio(D,a)=\frac{Gain(D,a)}{IV(a)}$$

其中

$$IV(a)=-\sum_{v=1}^V\frac{|D^v|}{|D|}\log_2 \frac{|D^v|}{|D|}$$

基尼值：

$$Gini(D)=1-\sum_{k=1}^{|y|}p_k^2$$

基尼指数：

$$Gini\_index(D,a)=\sum_{v=1}^V\frac{|D^v|}{|D|}Gini(D^v)$$

各种决策树和它的划分依据如下表所示：

| 名称 | 划分依据 |
|:--:|:--:|
| ID3 | Gain |
| C4.5 | Gain_ratio |
| CART | Gini_index |

决策树是一种可以将训练误差变为0的算法，只要每个样本对应一个叶子结点即可，然而这样做会导致过拟合。为了限制树的生长，我们可以加入阈值，当增益大于阈值时才让节点分裂。

# GBDT

GBDT这个算法有很多名字，但都是同一个算法：

GBRT (Gradient Boost Regression Tree)渐进梯度回归树

GBDT (Gradient Boost Decision Tree)渐进梯度决策树

MART (Multiple Additive Regression Tree)多决策回归树

Tree Net决策树网络

GBDT属于集成学习（Ensemble Learning）的范畴。集成学习的思路是在对新的实例进行分类的时候，把若干个单个分类器集成起来，通过对多个分类器的分类结果进行某种组合来决定最终的分类，以取得比单个分类器更好的性能。

集成学习的算法主要分为两大类：

**并行算法**：若干个不同的分类器同时分类，选择票数多的分类结果。这类算法包括bagging和随机森林等。

**串行算法**：使用同种或不同的分类器，不断迭代。每次迭代的目标是缩小残差或者提高预测错误项的权重。这类算法包括Adaboost和GBDT等。

GBDT写的比较好的，有以下blog：

http://blog.csdn.net/w28971023/article/details/8240756

摘录要点如下：

决策树分为两大类：

**回归树**：用于预测实数值，如明天的温度、用户的年龄、网页的相关程度。其结果加减是有意义的，如10岁+5岁-3岁=12岁。

**分类树**：用于分类标签值，如晴天/阴天/雾/雨、用户性别、网页是否是垃圾页面。其结果加减无意义，如男+男+女=到底是男是女？

GBDT的核心在于**累加所有树的结果作为最终结果**。例如根到某叶子结点的路径上的决策值为10岁、5岁、-3岁，则该叶子的最终结果为10岁+5岁-3岁=12岁。

所以**GBDT中的树都是回归树，不是分类树**。

上面举的例子中，越靠近叶子，其决策值的绝对值越小。这不是偶然的。决策树的基本思路就是“分而治之”，自然越靠近根结点，其划分的粒度越粗。每划分一次，预测误差（即残差）越小，这样也就变相提高了下一步划分中预测错误项的权重，这就是算法名称中Gradient的由来。

为了防止过拟合，GBDT还采用了Shrinkage（缩减）的思想，每次只走一小步来逐渐逼近结果，这样各个树的残差就是渐变的，而不是陡变的。

参考：

http://blog.csdn.net/u010691898/article/details/38292937

http://www.cs.cmu.edu/~tom/

# XGBoost

XGBoost是陈天奇于2014年提出的一套并行boost算法的工具库。

>注：陈天奇，华盛顿大学计算机系博士生，研究方向为大规模机器学习。上海交通大学本科（2006～2010）和硕士（2010～2013）。   
>http://homes.cs.washington.edu/~tqchen/

原始论文：

https://arxiv.org/pdf/1603.02754v3.pdf

参考文献中的部分结论非常精彩，摘录如下。

从算法实现的角度，把握一个机器学习算法的关键点有两个，一个是loss function的理解(包括对特征X/标签Y配对的建模，以及基于X/Y配对建模的loss function的设计，前者应用于inference，后者应用于training，而前者又是后者的组成部分)，另一个是对求解过程的把握。这两个点串接在一起构成了算法实现的主框架。

GBDT的求解算法，具体到每颗树来说，其实就是不断地寻找分割点(split point)，将样本集进行分割，初始情况下，所有样本都处于一个结点（即根结点），随着树的分裂过程的展开，样本会分配到分裂开的子结点上。分割点的选择通过枚举训练样本集上的特征值来完成，分割点的选择依据则是减少Loss。

XGBoost的步骤：

I. 对loss function进行二阶Taylor Expansion，展开以后的形式里，当前待学习的Tree是变量，需要进行优化求解。

II. Tree的优化过程，包括两个环节：

I). 枚举每个叶结点上的特征潜在的分裂点

II). 对每个潜在的分裂点，计算如果以这个分裂点对叶结点进行分割以后，分割前和分割后的loss function的变化情况。

因为Loss Function满足累积性(对MLE取log的好处)，并且每个叶结点对应的weight的求取是独立于其他叶结点的（只跟落在这个叶结点上的样本有关），所以，不同叶结点上的loss function满足单调累加性，只要保证每个叶结点上的样本累积loss function最小化，整体样本集的loss function也就最小化了。

可见，XGBoost算法之所以能够并行，其要害在于其中枚举分裂点的计算，是能够分布式并行计算的。

参考：

https://www.zhihu.com/question/41354392

# 推荐系统进阶

除了《机器学习（十三～十五）》提及的ALS和PCA之外，相关的算法还包括：

# FM：Factorization Machines

Factorization Machines是Steffen Rendle于2010年提出的算法。

>注：Steffen Rendle，弗赖堡大学博士，现为Google研究员。libFM的作者，被誉为推荐系统的新星。

FM算法实际上是一大类与矩阵分解有关的算法的广义模型。

参考文献1是Rendle本人的论文，其中有章节证明了SVD++、PITF、FPMC等算法，都是FM算法的特例。《机器学习（十四）》中提到的ALS算法，也是FM的特例。

参考文献2是国人写的中文说明，相对浅显一些。

参考：

1.https://www.ismll.uni-hildesheim.de/pub/pdfs/Rendle2010FM.pdf

2.http://blog.csdn.net/itplus/article/details/40534885

# PITF

配对互动张量分解（Pairwise Interaction Tensor Factorization）算法，也是最早由Rendle引入推荐系统领域的。

论文：

http://www.wsdm-conference.org/2010/proceedings/docs/p81.pdf

# 关联规则挖掘

## 基本概念

关联规则挖掘（Association rule mining）是机器学习的一个子领域。它最早的案例就是以下的**尿布和啤酒**的故事：

>沃尔玛曾今对数据仓库中一年多的原始交易数据进行了详细的分析，发现与尿布一起被购买最多的商品竟然是啤酒。   
>借助数据仓库和关联规则，发现了这个隐藏在背后的事实：**美国妇女经常会嘱咐丈夫下班后为孩子买尿布，而30%~40%的丈夫在买完尿布之后又要顺便购买自己爱喝的啤酒。**   
>根据这个发现，沃尔玛调整了货架的位置，把尿布和啤酒放在一起销售，大大增加了销量。

这里借用一个引例来介绍关联规则挖掘的基本概念。

| 交易号TID | 顾客购买的商品 | 交易号TID | 顾客购买的商品 |
|:--:|:--|:--:|:--|
| T1 | bread, cream, milk, tea | T6 | bread, tea |
| T2 | bread, cream, milk | T7 | beer, milk, tea |
| T3 | cake, milk | T8 | bread, tea |
| T4 | milk, tea | T9 | bread, cream, milk, tea |
| T5 | bread, cake, milk | T10 | bread, milk, tea |

