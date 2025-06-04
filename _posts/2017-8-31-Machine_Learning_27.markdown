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

>Sir David John Cameron MacKay，1967～2016，加州理工学院博士，导师John Hopfield，剑桥大学教授。英国能源与气候变化部首席科学顾问，英国皇家学会会员。在机器学习领域和可持续能源领域有重大贡献。

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

# 清=

南洋劝业会是中国历史上首次以官方名义主办的国际性博览会。由时任两江总督端方于1910年（清宣统二年）6月5日在南京举办，历时达半年。

---

清代的师爷中，以钱粮和刑名两项最为重要，这两个师爷叫做“大席”，大席每年的聘金少则七八百两，多则可达数千两；除了两名大席外，小席每年的聘金也要二百两上下。

---

萨赖占在新疆很有名望，算是贵族，他也知道斌静名声在外。他反抗的方式很极端，他亲手砍下女儿的头，扔入斌静的衙署。

你不是要看吗，脸到了就行，我家闺女干净身子我得留下。

---

晚清中兴四大名臣张之洞有个口头禅“我大清”。说话做事，动辄“我大清”怎么怎么，如何如何。

有一次在朝上议政，张之洞口头禅又来了，“我大清”的时候。素来与张之洞不和的醇亲王载沣一听便来气，只见他一本正经地问他：“张大人，你什么时候入的旗啊？这么重要的事，我怎么不知道！”

张之洞闻之，不免诧异，说道：“老夫没有入旗！”

载沣得意地说：“大清是我爱新觉罗家的。你等是我大清养的狗，连奴才都不算，那就不要张口我大清、闭口我大清，省得让人误会了，还以为大清是你们家的呢！”

---

从日本方面的记录来看，当时联合舰队司令长官伊东祐亨，下令舰队拆成各小部分，分别巡航，寻找北洋舰队主力。但是这个命令遭到日本海军内部很多人反对，觉得小舰队遇到镇远定远就是必死。当时整个联合舰队在巡航期间都是心惊肉跳，生怕北洋舰队出港。甚至后来复盘的时候，秋山真之直接拿这个当反面教材，美国的马汉、俄国的马卡洛夫等世界海军名将，都批评伊东祐亨这是臭棋。

但TM谁能想到北洋舰队没有维修能力……

---

咸丰年间，有个叫王库儿的捡了个腰牌，愣是在宫里卖了好几年馒头，期间不仅没人盘问他，甚至大家还都觉得他的馒头很好吃。不仅上朝的大臣来吃，就连宫女太监也来惠顾。卖到最后，就连咸丰帝的慈安皇后都成了他的客户。有一天，竟然还守着皇帝夸王库儿的馒头好吃。

皇上：“谁是王库儿？”

皇后：“不是您安排的啊，在宫里卖了三年馒头了。”

一查，闹了半天是混进宫里来的小贩。

皇帝勃然大怒，不怒是不可能的，王库儿下点手脚，大家就要被毒死了。

于是下令把王库儿捉来打一百大板。结果，慈安竟然还偷偷地让太监知会行刑的，可别打重了，人家也不容易。

---

一鸦时为大清国事自杀的大小官员三千多，二鸦就没几个了，到了辛亥革命，为国尽忠对于官员们来说就是个笑话。

---

个人武艺方面，雍正帝是个弱鸡，故宫保留了不少清代皇帝的御用弓，顺治用的弓是七力，七力在清代算是硬弓的门槛，康熙年轻时用的弓是九力的，到晚年换成了七力的弓，此外康熙还有一把十三力的硬弓，据康熙帝自述，他最多能开15力的硬弓，这在武举考试里都算是顶配了。

雍正之后的乾隆，乾隆用的是七力的弓，晚年体力下降了，换了一把六力二斤的弓，后世被称为鸦片鬼的咸丰，他都用五力的弓，而雍正皇帝的弓是四力半的，这个水平在小孩那桌都算差的，雍正一辈子都不参加射猎活动，秋狩基本上都是十三爷代劳的。

---

前几年听了个报告，说培养人切不可培养自己的掘墓人。然后开始介绍反面教材：清朝。说清政府秉持孝子必是忠臣的传统观念，不加甄别地吸收了很多孝顺父母但是痛恨清朝的革命人士进入军队、官派留学队伍；白花花的银子给留学生当海外补贴，留学生转头就拿着钱捐给国内革命人士。

报告人在ppt里贴出了秋瑾、徐锡麟、陈天华等人的照片。我之前是万万没想到这几位哥们儿姐们儿居然会作为反面教材的佐证材料出现，也算是人生一大见闻。
