---
layout: post
title:  机器学习（三十六）——XGBoost, LightGBM, Parameter Server
category: ML 
---

# XGBoost

XGBoost是陈天奇于2014年提出的一套并行boost算法的工具库。

>注：陈天奇，华盛顿大学计算机系博士（2019），研究方向为大规模机器学习。上海交通大学本科（2006～2010）和硕士（2010～2013）。   
>http://homes.cs.washington.edu/~tqchen/

![](/images/img2/XGBoost.png)

论文：

《XGBoost: A Scalable Tree Boosting System》

参考文献中的部分结论非常精彩，摘录如下。

从算法实现的角度，把握一个机器学习算法的关键点有两个，一个是loss function的理解(包括对特征X/标签Y配对的建模，以及基于X/Y配对建模的loss function的设计，前者应用于inference，后者应用于training，而前者又是后者的组成部分)，另一个是对求解过程的把握。这两个点串接在一起构成了算法实现的主框架。

GBDT的求解算法，具体到每颗树来说，其实就是不断地寻找分割点(split point)，将样本集进行分割，初始情况下，所有样本都处于一个结点（即根结点），随着树的分裂过程的展开，样本会分配到分裂开的子结点上。分割点的选择通过枚举训练样本集上的特征值来完成，分割点的选择依据则是减少Loss。

XGBoost的步骤：

I. 对loss function进行二阶Taylor Expansion，展开以后的形式里，当前待学习的Tree是变量，需要进行优化求解。

II. Tree的优化过程，包括两个环节：

I). 枚举每个叶结点上的特征潜在的分裂点

II). 对每个潜在的分裂点，计算如果以这个分裂点对叶结点进行分割以后，分割前和分割后的loss function的变化情况。

因为Loss Function满足累积性(对MLE取log的好处)，并且每个叶结点对应的weight的求取是独立于其他叶结点的（只跟落在这个叶结点上的样本有关），所以，不同叶结点上的loss function满足单调累加性，只要保证每个叶结点上的样本累积loss function最小化，整体样本集的loss function也就最小化了。

**可见，XGBoost算法之所以能够并行，其要害在于其中枚举分裂点的计算，是能够分布式并行计算的。**

官网：

https://xgboost.readthedocs.io/en/latest/

GitHub：

https://github.com/dmlc/xgboost

中文文档：

http://xgboost.apachecn.org/cn/latest/

编译：

{% highlight java %}
git clone --recursive https://github.com/dmlc/xgboost
cd xgboost; make -j4
{% endhighlight %}

python安装：

`cd python-package; sudo python setup.py install`

XGBoost提供了两种接口：普通接口和sklearn接口。后者的示例如下：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/ml/hello/decision_tree.py

## 参考

https://www.zhihu.com/question/41354392

机器学习算法中GBDT和XGBOOST的区别有哪些？

http://blog.csdn.net/sb19931201/article/details/52577592

xgboost入门与实战

https://mp.weixin.qq.com/s/x06axCC1ZTgezqEYjjNIsw

Xgboost初见面

https://mp.weixin.qq.com/s/f3QVbJiC6gLEptKwFZ-7ZQ

竞赛大杀器XGBoost，你还可以这样玩

http://blog.csdn.net/u013709270/article/details/78156207

Python机器学习实战之手撕XGBoost

https://mp.weixin.qq.com/s/xHVkc1NP2oodU7Hb0Xb_jA

为什么XGBoost在机器学习竞赛中表现如此卓越？

https://mp.weixin.qq.com/s/pn_qn6uRz2-9DmAK4sp35g

史上最详细的XGBoost实战（上）

https://mp.weixin.qq.com/s/xzZvIX0QaCPNSyGfHT3beQ

史上最详细的XGBoost实战（下）

https://mp.weixin.qq.com/s/T3NgIuGZIvmPSMNFyQeeGw

XGBoost原理解析

https://mp.weixin.qq.com/s/JYPnzgzBMSGx09ltBtfwqg

理解XGBoost机器学习模型的决策过程

http://www.cnblogs.com/qcloud1001/p/7542128.html

小巧玲珑：机器学习届快刀XGBoost的介绍和使用

https://mp.weixin.qq.com/s/__ESveAdBS9KJf26R3EMVA

对比TensorFlow提升树与XGBoost：我们该使用怎样的梯度提升方法

https://mp.weixin.qq.com/s/_uscpKaRXZbwfMiWGoY0Uw

多GPU加速学习，这是一份崭新的XGBoost库

https://mp.weixin.qq.com/s/7RU3Nzy5qkTL8-2LOjg9qg

从基础到实现：集成学习综合教程

https://mp.weixin.qq.com/s/Pkc8KyDZ53ZGO5lNLVjoBg

一份完备的集成学习手册！

https://mp.weixin.qq.com/s/pwuOFj_rT5Z_XY9siZosKQ

线性模型已退场，XGBoost时代早已来

https://mp.weixin.qq.com/s/hYuBHHfAGLO3Y0l5t6y94Q

XGBoost缺失值引发的问题及其深度分析

https://mp.weixin.qq.com/s/wgiNutl3FhJSAtfJNd5B5g

Xgboost

https://mp.weixin.qq.com/s/jZ3vcCcZTfTmJNl-9QXelw

XGBoost超详细推导，终于有人讲明白了！

https://mp.weixin.qq.com/s/HcWoDmrp4taRGREvV7x4vA

斯坦福吴恩达团队提出NGBoost：用于概率预测的自然梯度提升

# LightGBM

LightGBM是微软推出的boosting框架。

代码：

https://github.com/Microsoft/LightGBM

文档：

https://lightgbm.readthedocs.io/en/latest/

参考：

http://www.msra.cn/zh-cn/news/features/lightgbm-20170105

微软亚洲研究院：LightGBM介绍

https://zhuanlan.zhihu.com/p/25308051

比XGBOOST更快--LightGBM介绍

https://www.zhihu.com/question/51644470

如何看待微软新开源的LightGBM?

http://www.cnblogs.com/rocketfan/p/6005353.html

LightGBM中GBDT的实现

https://zhuanlan.zhihu.com/p/28768447

一个例子读懂LightGBM的模型文件

https://zhuanlan.zhihu.com/p/27916208

LightGBM调参指南(带贝叶斯优化代码)

https://mp.weixin.qq.com/s/zopaNDXABhPVqqUur9FFYg

lightgbm算法优化-不平衡二分类问题

https://mp.weixin.qq.com/s/JQasgzl-EpqBey7W6jKCTw

LightGBM大战XGBoost，谁将夺得桂冠？

http://blog.csdn.net/xwd18280820053/article/details/68927422

关于树的几个ensemble模型的比较（GBDT、xgBoost、lightGBM、RF）

https://mp.weixin.qq.com/s/TD3RbdDidCrcL45oWpxNmw

从结构到性能，一文概述XGBoost、Light GBM和CatBoost的同与不同

https://mp.weixin.qq.com/s/8IFKL0thCMBh40wUolbTVw

从XGB到LGB：美团外卖树模型的迭代之路

https://mp.weixin.qq.com/s/LoX987dypDg8jbeTJMpEPQ

终于有人把XGBoost和LightGBM讲明白了，项目中最主流的集成算法！

# CatBoost

这是Yandex推出的Boost工具包。

官网：

https://catboost.yandex/

论文：

《Fighting biases with dynamic boosting》

代码：

https://github.com/catboost/catboost

官方还提供了一个可视化工具：

https://github.com/catboost/catboost-viewer

参考：

https://mp.weixin.qq.com/s/TAminQXid3qq5b8qkeN1rA

ClickHouse如何结合自家的GNDT算法库CatBoost来做机器学习

https://mp.weixin.qq.com/s/4qQxB4AthVAYKggEV3BHFw

ThunderGBM：快成一道闪电的梯度提升决策树

https://mp.weixin.qq.com/s/LxPkd6PUHWoHQCw-xyE9SQ

大战三回合：XGBoost、LightGBM和Catboost一决高低

# Parameter Server

在深度学习概念提出之前，算法工程师手头能用的工具其实并不多，就LR、SVM、感知机等寥寥可数、相对固定的若干个模型和算法；那时候要解决一个实际的问题，算法工程师更多的工作主要是在特征工程方面。而特征工程本身并没有很系统化的指导理论（至少目前没有看到系统介绍特征工程的书籍），所以很多时候特征的构造技法显得光怪陆离，是否有用也取决于问题本身、数据样本、模型以及运气。如果给这种方式起一个名字的话，大概是**简单模型+复杂特征**；

深度学习代表的**简单特征+复杂模型**是解决实际问题的另一种方式。

两种模式孰优孰劣还难有定论，以点击率预测为例，在计算广告领域往往以海量特征+LR为主流，根据VC维理论，LR的表达能力和特征个数成正比，因此海量的feature也完全可以使LR拥有足够的描述能力。

Parameter Server就是处理海量特征计算的一种方法。

>我最初也被某系统号称上亿的特征给吓到了，毕竟自己设计的推荐系统搜肠刮肚也不过500左右的特征。后来才了解到，国内几家大公司在特征构造方面的成功率在后期一般不会超过20%。也就是80%的新构造特征往往并没什么正向提升效果。一个特征随便弄一下（窗口滑动、离散化、归一化、开方、平方、笛卡尔积、多重笛卡尔积）就弄出一堆特征。。。   
>一些所谓从事数据挖掘工作多年的专家，其实从方法论上也没有多高明。特征工程严重依赖领域知识，理论知识嘛，LR又不是多高深的东西。。。

>2017.5，我曾去某电商面试推荐系统职位。言谈之中发现他们对于DL几乎一无所知，当时就觉得有些古怪。直到接触Parameter Server才明白了他们的玩法。。。非常庆幸他们鄙视了我。后来到了2017.12的时候，他们主动找我，想再次面试，被我婉拒。

这类问题的另一个特征是：特征虽多，但单独的一个样本具有的有效特征相对有限，一般不过数百个。使用样本更新参数时，只考虑这几百个特征即可，这也为相关的分布式运算提供了有利条件。

参考：

https://www.zhihu.com/question/26998075

最近比较火的parameter server是什么？

http://blog.csdn.net/cyh_24/article/details/50545780

Parameter Server详解

https://mp.weixin.qq.com/s/yuHavuGTYMH5JDC_1fnjcg

阿里妈妈基于TensorFlow做了哪些深度优化？TensorFlowRS架构解析

https://zhuanlan.zhihu.com/p/29968773

大规模机器学习框架的四重境界

https://mp.weixin.qq.com/s/2RCH2Or_ITUTGrlfYLB8mg

腾讯千亿级参数分布式ML系统无量背后的秘密

https://mp.weixin.qq.com/s/Na2SJkfC9LzgfbTfSCclOw

如何基于Ray使用15行代码实现参数服务器

https://zhuanlan.zhihu.com/p/82116922

一文读懂“Parameter Server”的分布式机器学习训练原理

# Bloom Filter

https://blog.csdn.net/zhaodedong/article/details/78186450

Bloom Filter的原理和实现

https://blog.csdn.net/zhaodedong/article/details/78445910

Bloom Filter的数学背景

https://mp.weixin.qq.com/s/aPepcwG_VMioqGQ_Fp3deg

海量数据处理利器之布隆过滤器
