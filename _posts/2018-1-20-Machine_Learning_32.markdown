---
layout: post
title:  机器学习（三十二）——XGBoost, LightGBM, Parameter Server, LambdaMART, NLP机器翻译常用评价度量
category: ML 
---

## t-SNE

SNE的主要问题在于存在**Crowding问题**：就是说各个簇聚集在一起，无法区分。比如有一种情况，高维度数据在降维到10维下，可以有很好的表达，但是降维到两维后无法得到可信映射，比如降维如10维中有11个点之间两两等距离的，在二维下就无法得到可信的映射结果(最多3个点)。

其中的一种减轻”拥挤问题”的方法：在高维空间下，在高维空间下我们使用高斯分布将距离转换为概率分布，在低维空间下，我们**使用更加偏重长尾分布的方式来将距离转换为概率分布，使得高维度下中低等的距离在映射后能够有一个较大的距离。**

我们对比一下高斯分布和t分布, t分布受异常值影响更小，拟合结果更为合理，较好的捕获了数据的整体特征。

使用了t分布之后的q变化，如下:

$$q_{ij} = \frac{(1 +  \mid  \mid y_i -y_j \mid  \mid ^2)^{-1}}{\sum_{k \neq l} (1 +  \mid  \mid y_i -y_j \mid  \mid ^2)^{-1}}$$

![](/images/img2/sne_norm_t_dist_cost.png)

t-sne的有效性，也可以从上图中看到：横轴表示距离，纵轴表示相似度, 可以看到，对于较大相似度的点，t分布在低维空间中的距离需要稍小一点；而对于低相似度的点，t分布在低维空间中的距离需要更远。这恰好满足了我们的需求，即同一簇内的点(距离较近)聚合的更紧密，不同簇之间的点(距离较远)更加疏远。

总结一下，t-SNE的梯度更新有两大优势：

>对于不相似的点，用一个较小的距离会产生较大的梯度来让这些点排斥开来。

>这种排斥又不会无限大(梯度中分母)，避免不相似的点距离太远。

![](/images/img2/t-SNE.png)

![](/images/img2/Sammon.png)

上图分别是使用t-SNE和Sammon mapping可视化MNIST数据集后的效果图。从中可以看出t-SNE图中，数据更成团状，可视化效果更好。

t-SNE的不足主要有四个:

>主要用于可视化，很难用于其他目的。比如测试集合降维，因为他没有显式的预估部分，不能在测试集合直接降维；比如降维到10维，因为t分布偏重长尾，1个自由度的t分布很难保存好局部特征，可能需要设置成更高的自由度。

>t-SNE倾向于保存局部特征，对于本征维数(intrinsic dimensionality)本身就很高的数据集，是不可能完整的映射到2-3维的空间

>t-SNE没有唯一最优解，且没有预估部分。如果想要做预估，可以考虑降维之后，再构建一个回归方程之类的模型去做。但是要注意，t-sne中距离本身是没有意义，都是概率分布问题。

>训练太慢。有很多基于树的算法在t-sne上做一些改进。

## 参考

https://www.zhihu.com/question/52022955

t-sne数据可视化算法的作用是啥？为了降维还是认识数据？

https://mp.weixin.qq.com/s/Rs9ri6Xs5R-yitrda8pJMg

详解可视化利器t-SNE算法：数无形时少直觉

https://mp.weixin.qq.com/s/_DXMlNZHVKm2jMnLGQFM_Q

还在用PCA降维？快学学大牛最爱的t-SNE算法吧

http://www.datakit.cn/blog/2017/02/05/t_sne_full.html

t-SNE完整笔记

https://yq.aliyun.com/articles/70733

比PCA降维更高级——（R/Python）t-SNE聚类算法实践指南

# XGBoost

XGBoost是陈天奇于2014年提出的一套并行boost算法的工具库。

>注：陈天奇，华盛顿大学计算机系博士生，研究方向为大规模机器学习。上海交通大学本科（2006～2010）和硕士（2010～2013）。   
>http://homes.cs.washington.edu/~tqchen/

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

# Parameter Server

https://www.zhihu.com/question/26998075

最近比较火的parameter server是什么？

http://blog.csdn.net/cyh_24/article/details/50545780

Parameter Server详解

# LambdaMART

https://www.zhihu.com/question/41418093

求解LambdaMART的疑惑？

https://liam0205.me/2016/07/10/a-not-so-simple-introduction-to-lambdamart/

LambdaMART不太简短之介绍

http://blog.csdn.net/huagong_adu/article/details/40710305

Learning To Rank之LambdaMART的前世今生

# NLP机器翻译常用评价度量

机器翻译的评价指标主要有：BLEU、NIST、Rouge、METEOR等。

参考：

http://blog.csdn.net/joshuaxx316/article/details/58696552

BLEU，ROUGE，METEOR，ROUGE-浅述自然语言处理机器翻译常用评价度量

http://blog.csdn.net/guolindonggld/article/details/56966200

机器翻译评价指标之BLEU

http://blog.csdn.net/han_xiaoyang/article/details/10118517

机器翻译评估标准介绍和计算方法

http://blog.csdn.net/lcj369387335/article/details/69845385

自动文档摘要评价方法---Edmundson和ROUGE

https://mp.weixin.qq.com/s/XiZ6Uc5cHZjczn-qoupQnA

对话系统评价方法综述
