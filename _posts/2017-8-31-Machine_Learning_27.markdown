---
layout: post
title:  机器学习（二十七）——动态规划
category: ML 
---

# Latent Dirichlet Allocation（续）

## 如何确定LDA的topic个数

这个问题上，业界最常用的指标包括Perplexity，MPI-score等。简单的说就是Perplexity越小，且topic个数越少越好。

从模型的角度解决主题个数的话，可以在LDA的基础上融入嵌套中餐馆过程(nested Chinese Restaurant Process)，印度自助餐厅过程(Indian Buffet Process)等。因此就诞生了这样一些主题模型：

1. hierarchical Latent Dirichlet Allocation (hLDA)  (2003_NIPS_Hierarchical topic models and the nested Chinese restaurant process)

2. hierarchical Dirichlet process (HDP)  (2006_NIPS_Hierarchical dirichlet processes)/Nested Hierarchical Dirichlet Processes (nHDP)

3. Indian Buffet Process Compound Dirichlet Process (ICD)  (2010_ICML_The IBP compound Dirichlet process and its application to focused topic modeling)

4. Non-parametric Topic Over Time (npTOT)  (2013_SDM_A nonparametric mixture model for topic modeling over time)

5. collapsed Gibbs Samplingalgorithm for the Dirichlet Multinomial Mixture Model (GSDMM)  (2014_SIGKDD_A Dirichlet Multinomial Mixture Model-based Approach for Short Text Clustering)

这些主题模型都被叫做非参数主题模型(Non-parametric Topic Model)，最初可追溯到David M. Blei于2003年提出hLDA那篇文章(2003_NIPS_Hierarchical topic models and the nested Chinese restaurant process)。非参数主题模型是基于贝叶斯概率的与参数无关的主题模型。这里的参数无关主要是指模型本身可以“**随着观测数据的增长而相应调整**”，即主题模型的主题个数能够随着文档数目的变化而相应调整，无需事先人为指定。

参考：

https://www.zhihu.com/question/32286630

怎么确定LDA的topic个数？

http://blog.csdn.net/luo123n/article/details/48902815

Perplexity详解

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

# 动态规划

Dynamic programming(DP)用于解决那些可分解为**重复子问题（overlapping subproblems）**并具有**最优子结构（optimal substructure）**的问题。这里的programming和编程并无任何关系。

上世纪40年代，Richard Bellman最早使用动态规划这一概念表述通过遍历寻找最优决策解问题的求解过程。1953年，Richard Bellman将动态规划赋予现代意义，该领域被IEEE纳入系统分析和工程中。

除了Bellman之外，苏联的Lev Pontryagin也做出了很大的贡献，他和Bellman被并称为Optimal control之父。

>Lev Semyonovich Pontryagin，1908~1988，苏联数学家。主要研究代数拓扑和微分拓扑。他14岁时，因为煤气爆炸事故成为盲人。苏联科学院院士，国际数学家联盟副主席。

## 最优子结构

最优子结构即可用来寻找整个问题最优解的子问题的最优解。举例来说，寻找图上某顶点到终点的最短路径，可先计算该顶点所有相邻顶点至终点的最短路径，然后以此来选择最佳整体路径，如下图所示：

![](/images/article/Shortest_path_optimal_substructure.png)

一般而言，最优子结构通过如下三个步骤解决问题：

a) 将问题分解成较小的子问题；

b) 通过递归使用这三个步骤求出子问题的最优解；

c) 使用这些最优解构造初始问题的最优解。

子问题的求解是通过不断划分为更小的子问题实现的，直至我们可以在常数时间内求解。

## 重复子问题

![](/images/article/Fibonacci_dynamic_programming.png)

重复子问题是指通过相同的子问题可以解决不同的较大问题。例如，在Fibonacci序列中，F3 = F1 + F2和F4 = F2 + F3都包含计算F2。由于计算F5需要计算F3和F4，一个比较笨的计算F5的方法可能会重复计算F2两次甚至两次以上。

为避免重复计算，可将已经得到的子问题的解保存起来，当我们要解决相同的子问题时，重用即可。该方法即所谓的**缓存（memoization）**。

动态规划通常采用以下两种方式中的一种：

**自顶向下**：将问题划分为若干子问题，求解这些子问题并保存结果以免重复计算。该方法将递归和缓存结合在一起。

**自下而上**：先行求解所有可能用到的子问题，然后用其构造更大问题的解。该方法在节省堆栈空间和减少函数调用数量上略有优势，但有时想找出给定问题的所有子问题并不那么直观。

需要注意的是：动态规划更多的看作是一种解决问题的方法论，而非具体的数值算法，因此，很多不同领域的算法都可看做是动态规划算法的实例。参考文献中，就列出了不少这样的算法。显然，动态规划是一种**迭代（Iteration）算法**。

由前文的描述可知，MDP正好具备overlapping subproblems和optimal substructure的特性，因此也可以通过DP求解。

在继续下文之前，推荐一波资源：

>David Poole，加拿大不列颠哥伦比亚大学教授。加拿大AI协会终身成就奖（2013年）。   
>个人主页：   
>http://www.cs.ubc.ca/~poole/index.html

David Poole的主页上有很多好东西：

http://www.cs.ubc.ca/~poole/demos/

该网页上有一些RL方面的用java applet做的可视化demo。由于年代比较久远，这些demo无法在目前的浏览器上运行。所以，我对其做了一些改造，使之能够使用。相关代码参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/RL

David Poole还写了一本书《Artificial Intelligence: foundations of computational agents》，目前已经是第2版了。

其中的代码资源参见：

http://artint.info/AIPython/

从该书所用编程语言的变迁，亦可感受到Poole教授不断学习的脚步。要知道Poole教授刚进入学术界的时代（1985年前后），就连Java也还没被发明出来呢。

http://uhaweb.hartford.edu/compsci/ccli/samplep.htm

Hartford大学的这个网站也有些不错的资料，偏重RL、机器人、ML for Game等领域。

## RL与DP

在继续讲述之前，我们首先来明确几个概念：

![](/images/img2/RL_3.png)

$$v_{\pi}$$和$$q_{\pi}$$的定义参见《机器学习（二十五）》。

$$v_*(s)=\max_{\pi}v_{\pi}(s)$$

$$q_*(s,a)=\max_{\pi}q_{\pi}(s,a)$$

RL领域的DP算法的主要思想是：利用value function构建搜索Good Policy的方法。这里用$$v_*(s)$$或$$q_*(s, a)$$表示最优的value function。

RL DP主要包括以下算法：（为了抓住问题的本质，这里仅列出各算法最关键的Bellman equation，至于流程参照Q-learning算法即可。）
