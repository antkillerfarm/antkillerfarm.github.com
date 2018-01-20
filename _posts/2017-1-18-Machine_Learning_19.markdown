---
layout: post
title:  机器学习（十九）——关联规则挖掘
category: ML 
---

# 决策树（续）

## 参考

https://mp.weixin.qq.com/s/XnMXXFEBPXnEUk3jdMMoXA

从决策树到随机森林：树型算法的原理与实现

https://mp.weixin.qq.com/s/NcBGYtgiWa0uY48wnFOoVg

机器学习之决策树算法

https://zhuanlan.zhihu.com/p/22852262

经典决策树，条件推断树，随机森林，SVM的R实现

https://mp.weixin.qq.com/s/DTDH2m21Gz1UQ2tW64kPZg

如何解读决策树和随机森林的内部工作机制？

https://mp.weixin.qq.com/s/LC41Mk7Sjm30qr1KXsZd8Q

机器学习利器——决策树和随机森林！

https://mp.weixin.qq.com/s/PZ-1fkNvdJmv_8zLbvoW1g

Adaboost算法原理小结

https://mp.weixin.qq.com/s/KoOUgwXLOfJfOjWhbFX52Q

如果Boosting你懂，那Adaboost你懂么？

https://mp.weixin.qq.com/s/3yVosp2Kgp8cUyYWw_ULvw

如何解读决策树和随机森林的内部工作机制？

https://mp.weixin.qq.com/s/hY5cEug3xEpkkPE1X0Ykvg

GBDT详解

https://mp.weixin.qq.com/s/-AwJvJ_YQ7p_yo5TNHTOfw

决策树的python实现

https://mp.weixin.qq.com/s/PkUPGnsfCjiGPJpOmjACkA

Bagging与随机森林

http://blog.csdn.net/xwd18280820053/article/details/68927422

关于树的几个ensemble模型的比较（GBDT、xgBoost、lightGBM、RF）

https://mp.weixin.qq.com/s/XbrnhlxUbmK0JzQ4I_X2wQ

决策树之随机森林

https://mp.weixin.qq.com/s/NY2E2c808WSacyj68TFInw

决策树模型组合理解

https://mp.weixin.qq.com/s/K2uh0J-BLj-eSriI1_mEjA

决策树分类和预测算法原理

https://mp.weixin.qq.com/s/I5AXiHrN02zpyhF85Ze-jg

从零开始学习Gradient Boosting算法

https://mp.weixin.qq.com/s/Joz2FpGgBY0tC8lpoFz8Mw

AdaBoost元算法如何提高分类性能——机器学习实战

https://mp.weixin.qq.com/s/6X27b97X_7OOOSijqAau9g

随机森林（Random Forest）

https://mp.weixin.qq.com/s/hGoprRIeyoXPt5OnzgV-bg

集成学习算法(Ensemble Method)浅析

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

**定义一**：设$$I=\{i_1,i_2,\dots,i_m\}$$，是m个不同的项目的集合，每个$$i_k$$称为一个**项目**。项目的集合I称为**项集**。其元素的个数称为项集的长度，长度为k的项集称为k-项集。引例中每个商品就是一个项目，项集为$$I=\{bread, beer, cake,cream, milk, tea\}$$，I的长度为6。

**定义二**：每笔**交易T**是项集I的一个子集。对应每一个交易有一个唯一标识交易号，记作TID。交易全体构成了**交易数据库D**，$$\vert D\vert$$等于D中交易的个数。引例中包含10笔交易，因此$$\vert D\vert=10$$。

**定义三**：对于项集X，设定$$count(X\subseteq T)$$为交易集D中包含X的交易的数量，则项集X的**支持度**为：

$$support(X)=\frac{count(X\subseteq T)}{|D|}$$

引例中$$X=\{bread, milk\}$$出现在T1，T2，T5，T9和T10中，所以支持度为0.5。

**定义四**：**最小支持度**是项集的最小支持阀值，记为$$SUP_{min}$$，代表了用户关心的关联规则的最低重要性。支持度不小于$$SUP_{min}$$的项集称为频繁集，长度为k的频繁集称为k-频繁集。如果设定$$SUP_{min}$$为0.3，引例中$$\{bread, milk\}$$的支持度是0.5，所以是2-频繁集。

**定义五**：**关联规则**是一个蕴含式：

$$R：X\Rightarrow Y$$

其中$$X\subset I$$，$$Y\subset I$$，并且$$X\cap Y=\varnothing$$。表示项集X在某一交易中出现，则导致Y以某一概率也会出现。用户关心的关联规则，可以用两个标准来衡量：支持度和可信度。

**定义六**：关联规则R的**支持度**是交易集同时包含X和Y的交易数与$$\vert D\vert$$之比。即：

$$support(X\Rightarrow Y)=\frac{count(X\cap Y)}{|D|}$$

支持度反映了X、Y同时出现的概率。关联规则的支持度等于频繁集的支持度。

**定义七**：对于关联规则R，**可信度**是指包含X和Y的交易数与包含X的交易数之比。即：

$$confidence(X\Rightarrow Y)=\frac{support(X\Rightarrow Y)}{support(X)}$$

可信度反映了如果交易中包含X，则交易包含Y的概率。一般来说，只有支持度和可信度较高的关联规则才是用户感兴趣的。

**定义八**：设定关联规则的最小支持度和最小可信度为$$SUP_{min}$$和$$CONF_{min}$$。规则R的支持度和可信度均不小于$$SUP_{min}$$和$$CONF_{min}$$，则称为**强关联规则**。关联规则挖掘的目的就是找出强关联规则，从而指导商家的决策。

这八个定义包含了关联规则相关的几个重要基本概念，关联规则挖掘主要有两个问题：

1.找出交易数据库中所有大于或等于用户指定的最小支持度的频繁项集。

2.利用频繁项集生成所需要的关联规则，根据用户设定的最小可信度筛选出强关联规则。

其中，步骤1是关联规则挖掘算法的难点，下文介绍的Apriori算法和FP-growth算法，都是解决步骤1问题的算法。

参考：

http://blog.csdn.net/OpenNaive/article/details/7047823

关联规则挖掘（一）：基本概念

## Apriori算法

Apriori算法的思路如下：

1.第一次扫描交易数据库D时，产生1-频繁集。在此基础上经过连接、修剪产生2-频繁集。以此类推，直到无法产生更高阶的频繁集为止。

2.在第k次循环中，也就是产生k-频繁集的时候，首先产生k-候选集，k-候选集中每一个项集都是对两个只有一个项不同的属于k-1频繁集的项集连接产生的。

3.k-候选集经过筛选后产生k-频繁集。

从频繁集的定义，我们可以很容易的推导出如下结论：

**如果项目集X是频繁集，那么它的非空子集都是频繁集。**

如果k-候选集中的项集Y，包含有某个k-1阶子集不属于k-1频繁集，那么Y就不可能是频繁集，应该从候选集中裁剪掉。Apriori算法就是利用了频繁集的这个性质。

参考：

http://zhan.renren.com/dmeryuyang?gid=3602888498023976650

小白学数据分析----->关联分析学习算法篇Apriori

http://blog.csdn.net/lizhengnanhua/article/details/9061755

Apriori算法详解之：一、相关概念和核心步骤

https://mp.weixin.qq.com/s/W1Bu_I3p2DO_sT2Nl0582w

Apriori算法原理总结

## FP-growth算法

Aprori算法利用频繁集的两个特性，过滤了很多无关的集合，效率提高不少，但是我们发现Apriori算法是一个候选消除算法，每一次消除都需要扫描一次所有数据记录，造成整个算法在面临大数据集时显得无能为力。

FP-Growth算法是韩家炜等人在2000年提出的关联分析算法。它通过构造一个树结构来压缩数据记录，使得挖掘频繁项集只需要扫描两次数据记录，而且该算法不需要生成候选集合，所以效率会比较高。

>注：韩家炜，中国科学技术大学本科（1979）+中科院硕士+威斯康辛大学博士（1985）。美国伊利诺伊大学香槟分校计算机系教授，IEEE和ACM院士。

FpGrowth算法的平均效率远高于Apriori算法，但是它并不能保证高效率，它的效率依赖于数据集，当数据集中的频繁项集的没有公共项时，所有的项集都挂在根结点上，不能实现压缩存储，而且Fptree还需要其他的开销，需要存储空间更大，使用FpGrowth算法前，对数据分析一下，看是否适合用FpGrowth算法。

参考：

http://www.cnblogs.com/fengfenggirl/p/associate_fpgowth.html

数据挖掘系列（2）--关联规则FpGrowth算法

https://mp.weixin.qq.com/s/zD5hwBmMmSxzTj-3YzZKdg

频繁集挖掘FP Tree详解


