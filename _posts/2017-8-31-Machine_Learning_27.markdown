---
layout: post
title:  机器学习（二十七）——LSA
category: ML 
---

* toc
{:toc}

# Latent Dirichlet Allocation

## LDA（续）

**使用模型**：对于新来的一篇文档$$doc_{new}$$，我们能够计算这篇文档的topic分布$$\overrightarrow{\theta}_{new}$$。

从最终给出的算法可以看出，虽然LDA用到了MCMC和Gibbs Sampling算法，但最终目的并不是生成符合相应分布的随机数，而是求出模型参数$$\overrightarrow{\varphi}$$的值，并用于预测。

这一步实际上是一个**分类**的过程。可见，LDA不仅可用于聚类，也可用于分类，是一种无监督的学习算法。

## 如何确定LDA的topic个数

这个问题上，业界最常用的指标包括Perplexity，MPI-score等。简单的说就是Perplexity越小，且topic个数越少越好。

从模型的角度解决主题个数的话，可以在LDA的基础上融入嵌套中餐馆过程(nested Chinese Restaurant Process)，印度自助餐厅过程(Indian Buffet Process)等。因此就诞生了这样一些主题模型：

1.hierarchical Latent Dirichlet Allocation (hLDA)  (2003_NIPS_Hierarchical topic models and the nested Chinese restaurant process)

2.hierarchical Dirichlet process (HDP)  (2006_NIPS_Hierarchical dirichlet processes)/Nested Hierarchical Dirichlet Processes (nHDP)

3.Indian Buffet Process Compound Dirichlet Process (ICD)  (2010_ICML_The IBP compound Dirichlet process and its application to focused topic modeling)

4.Non-parametric Topic Over Time (npTOT)  (2013_SDM_A nonparametric mixture model for topic modeling over time)

5.collapsed Gibbs Samplingalgorithm for the Dirichlet Multinomial Mixture Model (GSDMM)  (2014_SIGKDD_A Dirichlet Multinomial Mixture Model-based Approach for Short Text Clustering)

这些主题模型都被叫做非参数主题模型(Non-parametric Topic Model)，最初可追溯到David M. Blei于2003年提出hLDA那篇文章(2003_NIPS_Hierarchical topic models and the nested Chinese restaurant process)。非参数主题模型是基于贝叶斯概率的与参数无关的主题模型。这里的参数无关主要是指模型本身可以“**随着观测数据的增长而相应调整**”，即主题模型的主题个数能够随着文档数目的变化而相应调整，无需事先人为指定。

参考：

https://www.zhihu.com/question/32286630

怎么确定LDA的topic个数？

http://blog.sina.com.cn/s/blog_4c9dc2a10102vua9.html

Perplexity（困惑度）详解

https://mp.weixin.qq.com/s/VwSmjv03LyqFFseQHJpgdQ

主题模型的主题数确定和可视化

## LDA漫游指南

除了rickjin的《LDA数学八卦》之外，马晨写的《LDA漫游指南》也是这方面的中文新作。

该书的数学推导部分主要沿用rickjin的内容，但加入了Blei提出的变分贝叶斯方法。此外，还对LDA的代码实现、并行计算和大数据处理进行了深入的讨论。

## 参考

http://www.arbylon.net/publications/text-est.pdf

《Parameter estimation for text analysis》，Gregor Heinrich著

http://www.inference.phy.cam.ac.uk/itprnn/book.pdf

《Information Theory, Inference, and Learning Algorithms》，David J.C. MacKay著

关于MCMC和Gibbs Sampling的更多的内容，可参考《Neural Networks and Learning Machines》，Simon Haykin著。该书有中文版。

>注：Sir David John Cameron MacKay，1967～2016，加州理工学院博士，导师John Hopfield，剑桥大学教授。英国能源与气候变化部首席科学顾问，英国皇家学会会员。在机器学习领域和可持续能源领域有重大贡献。

>Simon Haykin，英国伯明翰大学博士，加拿大McMaster University教授。初为雷达和信号处理专家。自适应信号处理领域的权威。80年代中后期，转而从事神经计算方面的工作。加拿大皇家学会会员。

http://www.cs.cmu.edu/~epxing/Class/10708-14/lectures/lecture17-MCMC.pdf

http://max.book118.com/html/2015/0513/16864294.shtm

基于LDA分析的词聚类算法

http://www.doc88.com/p-9159009103987.html

基于LDA的博客分类算法

http://blog.csdn.net/sinat_26917383/article/details/52095013

基于LDA的Topic Model变形+一些NLP开源项目

https://mp.weixin.qq.com/s/74lXwDg9H_dyOubfXVn2Bw

一文详解LDA主题模型

https://mp.weixin.qq.com/s/_bAiiJqPjPPMzJ0hiuCkOg

通过Python实现马尔科夫链蒙特卡罗方法的入门级应用

https://mp.weixin.qq.com/s/BJaRUnpcPe8iybSz14gabw

一文读懂如何用LSA、PSLA、LDA和lda2vec进行主题建模

https://zhuanlan.zhihu.com/p/57996219

短文本主题模型

https://mp.weixin.qq.com/s/FalGcZwnL5_Jgt650JtUag

NLP关键字提取技术之LDA算法原理与实践

https://www.zhihu.com/question/298517764

目前有比Topic Model更先进的聚类方式么？比如针对短文本的、加入情感分析的？

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

# 俄乌战争=

去年有个俄军讲述他的经历，他本来是个义务兵修车的，差3个月退役，被忽悠说签合同去乌克兰每个月给20万卢布，合同3个月一签，工作还是在后方修车不用上战场。他一寻思反正差3个月退役，签3个月合同白捡60万卢布啊，就签了。结果就被送上前线填战壕了，他拼命解释自己是修车的都没有用，好不容易熬了3个月想走人的时候被告知合同3个月自动续签，想解除合同必须本人亲自去国防部某部门办手续，但是前线的兵没有军令不得离开前线。他这时候才知道被骗了，而且说好的20万卢布一个月的工资，一分钱都没见到。

---

中非话事团队会把一些名义上属于他们，但是实际上属于反对派的矿许诺给瓦格纳，事发金矿属于CPC的控制范围，我相信金矿肯定是给CPC交了保护费的。那个矿开了有一阵子了，并不是才开张。还有最重要一点，瓦格纳在中非实力雄厚，抢我们东西也不是第一次了，21年就连抢带噶过了。

之前法国人在中非，后来法国人被瓦格纳赶跑了，西部归了瓦格纳，东部三不管，我们之前在东部是由法国人罩着的，现在没人罩了。

瓦格纳连偷拍恩达西马的鹅记者都噶了三个还会管别的吗？

https://www.zhihu.com/question/590674335

对于中非共和国遇袭，导致九名中国同胞死亡，你是如何看待此事的?

https://new.qq.com/rain/a/20210514A00O7Q00

中非激战告一段落！中国金矿遭瓦格纳雇佣兵抢占，彻底丧失采矿权

https://zhuanlan.zhihu.com/p/600369287

普京让普里戈任控制中非共和国金矿：瓦格纳雇佣军的利润估计为每年10亿美元

https://zhuanlan.zhihu.com/p/615867597

瓦格纳在中非的那些事

---

苏联红军老兵说：我们的领袖斯大林很了不起，他每天要去前线视察一次。

他在俄军服役的孙子面无表情地说：我们的总统普京更了不起，他根本不需要去前线，可前线每天都在向他靠近。

---

一名俄罗斯女兵泪诉在军中担任医疗工作，却根本沦为高级军官的“战地妻子”，即便正常工作时，也随时会觉得“好像有人坐在我身上”。

---

11月，私人军事公司、混蛋和一名伞兵打着收集伤亡人员的幌子，试图袭击乌克兰人。从那时起，就不可能收集到300和200了。

前线交换俘虏这类做法，在哈尔科夫反击攻势后就大量减少，现在已经几乎看不到了（主要原因是出现过几次俄军部队假扮换俘人员偷袭）。

在极为罕见的情况下，双方的逃兵会联合行动，进行抢劫活动或是试图脱逃，前线报告过几例类似的行为。

---

https://www.zhihu.com/question/560300904

如何评价一架满载弹药的俄军SU-34战斗机坠毁在俄国叶伊斯克市的居民楼上？

https://www.zhihu.com/question/561705498

如何评价一架俄军SU-30战斗机坠毁在俄国伊库尔茨克市的居民楼上，并造成两名飞行员死亡？
