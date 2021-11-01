---
layout: post
title:  机器学习（三十六）——XGBoost, LightGBM, CatBoost
category: ML 
---

* toc
{:toc}

# Adaboost（续）

https://mp.weixin.qq.com/s/Joz2FpGgBY0tC8lpoFz8Mw

AdaBoost元算法如何提高分类性能——机器学习实战

https://mp.weixin.qq.com/s/MLEVUKse5usmKIWJF-yfOQ

通俗易懂讲解自适应提升算法AdaBoost

https://mp.weixin.qq.com/s/VuDAdeVsoZsTokh3n_wWFw

一文详解机器学习中最好用的提升方法：Boosting与AdaBoost

https://mp.weixin.qq.com/s/Jnh7yIOmzbTvWk77zh2-lA

周志华：Boosting学习理论的探索——一个跨越30年的故事

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

```bash
git clone --recursive https://github.com/dmlc/xgboost
cd xgboost; make -j4
```

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

https://mp.weixin.qq.com/s/pwuOFj_rT5Z_XY9siZosKQ

线性模型已退场，XGBoost时代早已来

https://mp.weixin.qq.com/s/hYuBHHfAGLO3Y0l5t6y94Q

XGBoost缺失值引发的问题及其深度分析

https://mp.weixin.qq.com/s/Il_S6y4UkN5nim91mwVcVw

缺失值处理

https://mp.weixin.qq.com/s/wgiNutl3FhJSAtfJNd5B5g

Xgboost

https://mp.weixin.qq.com/s/jZ3vcCcZTfTmJNl-9QXelw

XGBoost超详细推导，终于有人讲明白了！

https://mp.weixin.qq.com/s/HcWoDmrp4taRGREvV7x4vA

斯坦福吴恩达团队提出NGBoost：用于概率预测的自然梯度提升

https://mp.weixin.qq.com/s/X4K6UFZPxL05v2uolId7Lw

XGBoost在携程搜索排序中的应用

https://mp.weixin.qq.com/s/5zSLod4oyL4m6LADI6KC0Q

深入理解XGBoost，优缺点分析，原理推导及工程实现

https://mp.weixin.qq.com/s/uvUN4JiqSb-bS4HAVCDTIQ

集成模型Xgboost！机器学习最热研究方向入门，附学习路线图

https://mp.weixin.qq.com/s/-aRRORqBnTMmBDXuW749_w

在没有技术术语的情况下介绍Adaptive、GBDT、XGboosting等提升算法的原理简介

https://mp.weixin.qq.com/s/umGzRySCnJo25e5_0nO7cw

如何用XGBoost做时间序列预测？

https://mp.weixin.qq.com/s/QYMPBWMVTQf3LACGGWGX7A

样本不平衡处理：xgboost几种权重设置方法比较

https://mp.weixin.qq.com/s/d6sVDAKnaU97mAeDs_Gedg

从零解读Xgboost(原理+代码)

https://mp.weixin.qq.com/s/YunDfYPLywc0tMJF72YIAQ

数据分析利器：XGBoost算法最佳解析

https://mp.weixin.qq.com/s/QAu0nM72E2gVhbRXmtCMcw

XGBoost和时间序列

https://www.zhihu.com/question/359567100

XGBoost为什么若模型决策树的叶子节点值越大，越容易过拟合呢？

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

https://mp.weixin.qq.com/s/l6Fp5WTNH0b_cl2y7Az76Q

深入理解LightGBM

https://mp.weixin.qq.com/s/H9zkyO9oZAysWMyigd1tNw

LightGBM

https://mp.weixin.qq.com/s/RaWeiQwlQjCi1zz5S3tOmA

LightGBM的参数详解以及如何调优

https://mp.weixin.qq.com/s/9gEfkiZyZkoIgwRCYISQgQ

你应该知道的LightGBM各种操作！

https://mp.weixin.qq.com/s/YSDB6SSrU7xlzZ73PrjIAw

比赛杀器LightGBM常用操作总结！

https://mp.weixin.qq.com/s/_y16bW-afo9gOI5ObMBRYQ

Kaggle神器LightGBM最全解读！

https://mp.weixin.qq.com/s/A3b2L_0-atm5jJslhr50Bg

最新LightGBM进展介绍报告，39页ppt

https://mp.weixin.qq.com/s/M0W6-bLZcsdMsKBUAeKDnw

树模型奠基性论文解读——GBM: Gradient Boosting Machine

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

https://mp.weixin.qq.com/s/E3pSPsG18053F5GG1Z8jNQ

一文详尽系列之CatBoost

https://mp.weixin.qq.com/s/eCZHpFvtDYnpI6jm2nEtnQ

深入理解CatBoost

https://mp.weixin.qq.com/s/pQ9_0d8sl5Sr5O360Risnw

使用CatBoost进行不确定度估算：模型为何不确定以及如何估计不确定性水平

https://mp.weixin.qq.com/s/o129MwpGzx8tzjUCkz1npw

使用CatBoost和NODE建模表格数据对比测试
