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

# Pytorch+

https://mp.weixin.qq.com/s/Svh27YIG2jYWqXwhhEuoSw

使用Pytorch和BERT进行多标签文本分类

https://mp.weixin.qq.com/s/_3iJCO4gQz7mWcW7G8kimQ

Pytorch实现卷积神经网络训练量化（QAT）

https://mp.weixin.qq.com/s/HQnI8rzPvZN6Q_5c8d1nVQ

唯快不破：基于Apex的混合精度加速

https://mp.weixin.qq.com/s/NupSd4e01cvQ3CRnjy1npw

超原版速度110倍，针对PyTorch的CPU到GPU张量迁移工具开源

https://zhuanlan.zhihu.com/p/87572724

一文看懂align_corners

https://mp.weixin.qq.com/s/BsMwZC3IoXA58OybZ3aYcg

pytorch中如何处理RNN输入变长序列padding

https://mp.weixin.qq.com/s/abd62HNlen9sepH3CrmpGg

PyTorch为何如此高效好用？来探寻深度学习框架的内部架构

https://mp.weixin.qq.com/s/OMjfck4jlMneGZ1NJxbjKQ

PyTorch有哪些坑/bug？

https://mp.weixin.qq.com/s/UykIKAeKB-0eqk6UqvXwXA

对抗自编码器PyTorch手把手实战系列——PyTorch实现对抗自编码器

https://mp.weixin.qq.com/s/E8RMSXVZucSGQJxFdtMgkg

PyTorch实例：用ResNet进行交通标志分类

https://mp.weixin.qq.com/s/_hbuk9nZaLoNSZ5AnDnELg

使用PyTorch从零开始构建Elman循环神经网络

https://mp.weixin.qq.com/s/FQxSYjyR1U3TIinSShnOfg

从头开始了解PyTorch的简单实现

https://mp.weixin.qq.com/s/wz1YdHYom6y-gOB58R86OQ

对抗自编码器PyTorch手把手实战系列——对抗自编码器学习笔迹风格

https://mp.weixin.qq.com/s/3mnV8gz1AsYQ2ElK--Ihrg

从零开始PyTorch项目：YOLO v3目标检测实现

https://mp.weixin.qq.com/s/88klahIOAfBcA5Rdv70JNg

PyTorch的动态计算图深入浅出

https://mp.weixin.qq.com/s/Mcj9SwC6GZEjXxi55vfsaw

minibatching，数据加载和模型构建

https://mp.weixin.qq.com/s/EweQNoP5BlB9e1eVJsCFvg

GANs很难？这篇文章教你50行代码搞定

https://mp.weixin.qq.com/s/1KAbFAWC3jgJTE-zp5Qu6g

如何直观地理解条件随机场，并通过PyTorch简单地实现

https://mp.weixin.qq.com/s/1_fm6F1c_ldQNyb042NQRg

还在自己写训练过程么？你需要一个训练引擎

https://mp.weixin.qq.com/s/mThkHpcq1ThnfONiSKmOKA

如何捕获一只彩色卓别林？黑白照片AI上色教程很友好

https://mp.weixin.qq.com/s/tD4Inrr0l1oBnLhUD81FCA

用PyTorch进行RNN语言建模 - Packed Batching和Tied Weight

https://mp.weixin.qq.com/s/HQJxbnIHXF-GFLIoNTOxGg

手把手教你用torchtext处理文本数据

https://mp.weixin.qq.com/s/KNaFelqk9UO6JYjbPTQohA

显存不足？PyTorch显存使用分析与优化

https://mp.weixin.qq.com/s/7p26q8Mu3IlK39DG7u41ZA

PyTorch之迁移学习实战

https://www.zhihu.com/question/303070254

PyTorch中在反向传播前为什么要手动将梯度清零？

https://www.zhihu.com/question/274635237

Pytorch有什么节省内存（显存）的小技巧？

https://mp.weixin.qq.com/s/S1dRfmqpiLzR3tnsocmfvw

Pytorch中的数据增强方式最全解释

https://mp.weixin.qq.com/s/BTFMvV2ppmRBXYg95YlK4w

PyTorch实现L2和L1正则化的方法

https://zhuanlan.zhihu.com/p/272767300

Pytorch转ONNX-理论篇

https://zhuanlan.zhihu.com/p/273566106

Pytorch转ONNX-实战篇1（tracing机制）

https://zhuanlan.zhihu.com/p/286298001

Pytorch转ONNX-实战篇2（实战踩坑总结）

https://mp.weixin.qq.com/s/EXuFXbPBIbzTyi0fUjvvPw

两行代码统计模型参数量与FLOPs，这个PyTorch小工具值得一试

https://zhuanlan.zhihu.com/p/73711222

巧用torch.backends.cudnn.benchmark减少训练时间

https://zhuanlan.zhihu.com/p/664723980

单机多GPU训练
