---
layout: post
title:  深度学习（一）——MP神经元模型, BP算法, Spark
category: theory 
---

# 前言

这部分最主要的参考文献包括：

《机器学习》，周志华著。

《Deep Learning Tutorial》，李宏毅著（台湾大学电机工程学助理教授）。

http://www.useit.com.cn/thread-13132-1-1.html

其他参考文献将在相关部分列出。

Deep Learning圈子的主要人物：

![](/images/article/DL_NB.png)

>注：Yann LeCun，中文名燕乐存，1960年生，法国科学家。Pierre and Marie Curie University博士。Geoffrey Hinton是他博士后时代的导师。CNN的发明人。纽约大学教授，Facebook AI研究所主任。

>Léon Bottou，法国科学家，随机梯度下降算法的发明人。

>Yoshua Bengio，1964年生，法国出生的加拿大科学家。深度学习的另一个宗师。

>这三个法国佬，都是好基友。只不过Yann LeCun和Yoshua Bengio研究神经网络，而Léon Bottou研究SVM，学术上分属不同派系。

>Geoffrey Everest Hinton，1947年生，英国出生的加拿大科学家，爱丁堡大学博士，多伦多大学教授。连接主义的代表人物，多层神经网络的宗师。英国皇家学会会员。

>一般将Geoffrey Hinton、Yann LeCun和Yoshua Bengio并称为深度学习的三大宗师。

# MP神经元模型

MP神经元模型是1943年，由Warren McCulloch和Walter Pitts提出的。

>注：Warren Sturgis McCulloch，1898~1969，美国神经生理学和控制论科学家。哥伦比亚大学博士，先后执教于MIT、Yale、芝加哥大学。

>Walter Harry Pitts, Jr.，1923~1969，美国计算神经学科学家。   
>这个人的经历，实在是非典型。家里贫穷，大约是读不起大学，15岁的时候，到芝加哥大学旁听Bertrand Russell的讲座。Russell很看重这个年轻人，但由于他只是访问学者，于是在回国之前，将Pitts介绍给Rudolf Carnap，后者为Pitts安排了一份在学校打杂的工作。这一打杂就是五六年时间，最后凭借论文，获得芝加哥大学的准学士学位（因为他始终都不是正式学籍的学生），这也是他一生唯一的学位。   
>但是如果看看Pitts的合作者的阵容，就知道Pitts水平之高了。他们是：Warren McCulloch、Jerome Lettvin、Norbert Wiener。

MP神经元模型如下图所示：

![](/images/article/MP_model.jpg)

即：

$$y_j=\sum _{i=1}^nw_{ij}x_i-\theta_j$$

上式其实就是《机器学习（一）》中提到的逻辑回归。

除了阶跃函数和Sigmod函数之外，常用的神经元激活函数，还有双曲正切函数：

$$f(z)=\tanh(x)=\frac{\sinh(x)}{\cosh(x)}=\frac{e^x-e^{-x}}{e^x+e^{-x}}$$

其导数为：

$$f'(z)=1-(f(z))^2$$

生物神经元和MP神经元模型的对应关系如下表：

| 生物神经元 | MP神经元模型 |
|:--:|:--:|
| 神经元 | $$j$$ |
| 输入信号 | $$x_i$$ |
| 权值 | $$w_{ij}$$ |
| 输出信号 | $$y_j$$ |
| 总和 | $$\sum$$ |
| 膜电位 | $$\sum _{i=1}^nw_{ij}x_i$$ |
| 阈值 | $$\theta_j$$ |

从上图亦可看出，如果将阈值看作输入为-1.0的哑节点的连接权重，则权重和阈值可统一为权重。神经网络训练的过程，实际上就是根据样本调整权重和阈值的过程。

参考：

http://blog.csdn.net/u013007900/article/details/50066315

# 单层感知器 vs. 多层感知器

神经网络的层数越多，其表达力越丰富，如下表所示：

![](/images/article/single_layer_vs_multi_layer.png)

# ANN简史

![](/images/article/ANN_history.png)

# BP算法

误差逆传播（error BackPropagation）算法最早由Paul J. Werbos于1974年提出，然而此时正值ANN的低谷，未得到人们的重视。因此到了1986年时，由David Everett Rumelhart重新发明了该算法。

>注：Paul J. Werbos，1947年生，哈佛大学博士。

>David Everett Rumelhart，1942~2011，美国心理学家。斯坦福大学博士，先后执教于UCSD和斯坦福。美国科学院院士。

![](/images/article/BP_network.png)

BP算法的核心思路：

1.利用前向传导公式，计算第n层输出值。

2.计算输出值和实际值的残差。

3.将残差梯度传递回第$$n-1,n-2,\dots,2$$层，并修正各层参数。（即所谓的误差逆传播）

BP算法的推导过程教材已经写的很好了，这里只补充一点：

神经网络的参数的随机初始化的目的是使对称失效。否则的话，所有对称结点的权重都一致，也就无法区分并学习了。

# NLP

自然语言处理（Natural Language Processing）是深度学习的主要应用领域之一。

## 教程

http://cs224d.stanford.edu/

CS224d: Deep Learning for Natural Language Processing

http://web.stanford.edu/class/cs224n/syllabus.html

cs224d课程的课件

http://demo.clab.cs.cmu.edu/NLP/

CMU的NLP教程。该网页下方还有美国其他高校的NLP课程的链接。

http://ccl.pku.edu.cn/alcourse/nlp/

北京大学的NLP教程，特色：中文处理。

## 书籍

http://ccl.pku.edu.cn/alcourse/nlp/LectureNotes/Natural%20Language%20Processing%20with%20Python.pdf

《Natural Language Processing with Python》，Steven Bird、Ewan Klein、Edward Loper著。

>注：Steven Bird，爱丁堡大学博士，墨尔本大学副教授。   
>http://www.stevenbird.net/about.html

>Ewan Klein，苏格兰人，哥伦比亚大学博士（1978年），爱丁堡大学教授。

>Edward Loper，宾夕法尼亚大学博士。

## 网站

http://www.52nlp.cn/

一个自然语言处理爱好者的群体博客。包括52nlp、rickjin、liwei等国内外华人大牛。

http://www.shareditor.com/bloglistbytag/?tagname=%E8%87%AA%E5%B7%B1%E5%8A%A8%E6%89%8B%E5%81%9A%E8%81%8A%E5%A4%A9%E6%9C%BA%E5%99%A8%E4%BA%BA

实战课程：自己动手做聊天机器人

# Spark

官网：

http://spark.apache.org/

## RDD

Resilient Distributed Datasets是一个只读的，可分区的分布式数据集，这个数据集的全部或部分可以缓存在内存中，在多次计算间重用。RDD克服了传统的MapReduce所进行大量的磁盘IO操作。

我的理解：RDD将计算包括中间结果，全部放到内存中，以节省磁盘IO操作。

http://www.infoq.com/cn/articles/spark-core-rdd

http://f.dataguru.cn/thread-475874-1-1.html

## DataFrame和DataSet

http://www.jianshu.com/p/c0181667daa0

http://www.csdn.net/article/2015-02-17/2823997

https://www.iteblog.com/pdf/1675

## transformation & action

transformation是得到一个新的RDD，方式很多，比如从数据源生成一个新的RDD，从RDD生成一个新的RDD。

action是得到一个值，或者一个结果（直接将RDDcache到内存中）。

所有的transformation都是采用的懒策略，就是如果只是将transformation提交是不会执行计算的，计算只有在action被提交的时候才被触发。

## Spark部署

Spark没有服务程序，因此无须部署，只需要在集群中的某台机器上安装spark，进行任务提交即可。Spark的并行执行主要依赖Hadoop YARN。

参见：

http://blog.csdn.net/book_mmicky/article/details/25714287

Spark虽然对Hadoop的版本有一定的要求，但是并不是太严重的问题。比如，目前最新的Spark 2.0.1（2016.10）仍然支持Hadoop 2.3，而后者是2014年2月出的版本。

## shuffle

![](/images/article/mapreduce-process.png)

上图描述了MapReduce算法的整个流程，其中shuffle phase就是介于Map phase和Reduce phase之间的那一堆连线。很显然，shuffle虽然是MapReduce算法提出的概念，但在各类分布式计算框架中普遍存在，也是影响计算效率的关键点和框架设计的难点之一。

参考：

http://jerryshao.me/architecture/2014/01/04/spark-shuffle-detail-investigation/

## 控制日志输出等级

有的时候为了防止控制台的日志输出过多，淹没了程序的正常输出，可以采用如下方法：

{% highlight java %}
SparkContext sc = SparkContext.getOrCreate(conf);
sc.setLogLevel("WARN");
{% endhighlight %}

## 基本统计操作

1.统计列中不相同值的个数。

方法一：

比如在观众观影的表格中，找出观众的数量或电影的数量。

`Dataset<Row> df = session.sql("SELECT userId FROM Movie group by userId");//df.count() is the count of different values`

方法二：

Spark中有个org.apache.spark.sql.functions类，专门针对数据集进行各种运算操作。其中的countDistinct方法可实现该功能。片段示例如下：

`Dataset<Row> df2 = df.agg(functions.countDistinct("userId"));df2.show();`

## 参考

http://www.cnblogs.com/zlslch/p/5723857.html

Spark的各种算子

http://mp.weixin.qq.com/s?__biz=MjM5NzAyNTE0Ng==&mid=2649517135&idx=2&sn=7fc02a006b7c5015f3492354d0e298a4&scene=0#rd

Spark性能优化指南：高级篇

https://www.zhihu.com/question/23079001

内存有限的情况下，Spark如何处理T级别的数据

https://www.zhihu.com/question/26568496

与Hadoop对比，如何看待Spark技术？

http://www.csdn.net/article/1970-01-01/2825748

如何利用“图计算”实现大规模实时预测分析

http://www.cnblogs.com/bluejoe/p/5115846.html

学习GraphX

http://www.cnblogs.com/bluejoe/p/5115845.html

Hive体系结构介绍


