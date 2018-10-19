---
layout: post
title:  机器学习（二十一）——Loss function详解, 聚类算法, 特征工程, 模仿学习, 三门问题, 机器学习分类器性能指标
category: ML 
---

## 关联规则评价（续）

### 确信度

Conviction的定义如下：

$$\mathrm{conv}(X\Rightarrow Y) =\frac{ 1 - \mathrm{supp}(Y) }{ 1 - \mathrm{conf}(X\Rightarrow Y)}$$

它的值越大，表明X、Y的独立性越小。

### 卡方系数

卡方系数是与卡方分布有关的一个指标。参见：

https://en.wikipedia.org/wiki/Chi-squared_distribution

$$\chi^2 = \sum_{i=1}^n \frac{(O_i - E_i)^2}{E_i}$$

>注：上式最早是Pearson给出的。

公式中的$$O_i$$表示数据的实际值，$$E_i$$表示期望值，不理解没关系，我们看一个例子就明白了。

| 表2 | 买游戏 | 不买游戏 | 行总计 |
|:--:|:--|:--:|:--|
| 买影片 | 4000(4500) | 3500(3000) | 7500 |
| 不买影片 | 2000(1500) | 500(1000) | 2500 |
| 列总计 | 6000 | 4000 | 10000 |

表2的括号中表示的是期望值。以第1行第1列的4500为例，其计算方法为：7500×6000/10000。

经计算可得表2的卡方系数为555.6。基于置信水平和自由度$$(r-1)*(c-1)=(行数-1)*(列数-1)=1$$，查表得到自信度为(1-0.001)的值为6.63。

555.6>6.63，因此拒绝A、B独立的假设，即认为A、B是相关的，而$$E(买影片，买游戏)=4500>4000$$,因此认为A、B呈负相关。

### 全自信度

$$all\_confidence(A,B)=\frac{P(A\cap B)}{max\{P(A),P(B)\}}\\=min\{P(B|A),P(A|B)\}=min\{confidence(A\to B),confidence(B\to A)\}$$

### 最大自信度

$$max\_confidence(A,B)=max\{confidence(A\to B),confidence(B\to A)\}$$

### Kulc

$$kulc(A,B)=\frac{confidence(A\to B)+confidence(B\to A)}{2}$$

### cosine距离

$$cosine(A,B)=\frac{P(A\cap B)}{sqrt(P(A)*P(B))}=sqrt(P(A|B)*P(B|A))\\=sqrt(confidence(A\to B)*confidence(B\to A))$$

### Leverage

$$Leverage(A,B) = P(A\cap B)-P(A)P(B)$$

### 不平衡因子

imbalance ratio的定义：

$$IR(A,B)=\frac{|support(A)-support(B)|}{(support(A)+support(B)-support(A\cap B))}$$

全自信度、最大自信度、Kulc、cosine，Leverage是不受空值影响的，这在处理大数据集是优势更加明显，因为大数据中空记录更多，根据分析我们推荐使用kulc准则和不平衡因子结合的方法。

参考：

http://www.cnblogs.com/fengfenggirl/p/associate_measure.html

关联规则评价

https://mp.weixin.qq.com/s/s1Snb4XnIQk1DcK3nESilw

PrefixSpan算法原理详解

# Loss function详解

![](/images/img2/loss.png)

## Mean Squared Error(MSE)/Mean Squared Deviation(MSD)

$$\operatorname{MSE}=\frac{1}{n}\sum_{i=1}^n(\hat{Y_i} - Y_i)^2$$

## Symmetric Mean Absolute Percentage Error(SMAPE or sMAPE)

MSE定义的误差，实际上是向量空间中的欧氏距离，这也可称为绝对误差。而有些情况下，可能相对误差（即百分比误差）更有意义些：

$$\text{SMAPE} = \frac 1 n \sum_{t=1}^n \frac{\left|F_t-A_t\right|}{(A_t+F_t)/2}$$

上式的问题在于$$A_t+F_t\le 0$$时，该值无意义。为了解决该问题，可用如下变种：

$$\text{SMAPE} = \frac{100\%}{n} \sum_{t=1}^n \frac{|F_t-A_t|}{|A_t|+|F_t|}$$

## Mean Absolute Error(MAE)

$$\mathrm{MAE} = \frac{1}{n}\sum_{i=1}^n \left| f_i-y_i\right| =\frac{1}{n}\sum_{i=1}^n \left| e_i \right|$$

这个可以看作是MSE的1范数版本。

## Mean Percentage Error(MPE)

$$\text{MPE} = \frac{100\%}{n}\sum_{t=1}^n \frac{a_t-f_t}{a_t}$$

不同的loss函数有不同的用途，比如softmax一般用Cross Entropy作为loss函数。如下图所示：

![](/images/article/cross_vs_mse.png)

## loss function比较

![](/images/article/loss_function.png)

这里m代表了置信度，越靠近右边置信度越高。

其中蓝色的阶跃函数又被称为Gold Standard，黄金标准，因为这是最准确无误的分类器loss function了。分对了loss为0，分错了loss为1，且loss不随到分界面的距离的增加而增加，也就是说这个分类器非常鲁棒。但可惜的是，它不连续，求解这个问题是NP-hard的，所以才有了各种我们熟知的分类器。

其中红色线条就是SVM了，由于它在m=1处有个不可导的转折点，右边都是0，所以分类正确的置信度超过一定的数之后，对分界面的确定就没有一点贡献了。

《机器学习（五）》中提到的SVM软间隔，其所使用的loss function，又被称为Hinge loss函数：

$$l_{hinge}(z)=\max(0,1-z)$$

除此之外，exponential loss函数：

$$l_{exp}(z)=\exp(-z)$$

和logistic loss函数：

$$l_{log}(z)=\log(1+\exp(-z))$$

也是较常用的SVM loss function。

黄色线条是Logistic Regression的损失函数，与SVM不同的是，它非常平滑，但本质跟SVM差别不大。

绿色线条是boost算法使用的损失函数。

黑色线条是ELM（Extreme learning machine）算法的损失函数。它的优点是有解析解，不必使用梯度下降等迭代方法，可直接计算得到最优解。但缺点是随着分类的置信度的增加，loss不降反升，因此，最终准确率有限。此外，解析算法相比迭代算法，对于大数据的适应较差，这也是该方法的局限所在。

参见：

https://www.zhihu.com/question/28810567

Extreme learning machine(ELM)到底怎么样，有没有做的前途？

## 参考

https://mp.weixin.qq.com/s/gw3hoDSaojVQUiD6YsMabA

理解神经网络中的目标函数

https://mp.weixin.qq.com/s/h-QbwEbaivvHjdhDhE4V1A

如何为单变量模型选择最佳的回归函数

https://mp.weixin.qq.com/s/qXZMo_RitSenmI7x0xGNsg

中科院自动化所多媒体计算与图形学团队NIPS 2017论文提出平均Top-K损失函数，专注于解决复杂样本

https://mp.weixin.qq.com/s/kI22wSoyNT3QXXI8pVwbjA

腾讯AI Lab提出新型损失函数LMCL：可显著增强人脸识别模型的判别能力

https://mp.weixin.qq.com/s/YOdmv88koSHx5AMMEQZGgg

通俗聊聊损失函数中的均方误差以及平方误差

https://blog.csdn.net/zhangxb35/article/details/72464152

pytorch loss function总结

https://mp.weixin.qq.com/s/Xbi5iOh3xoBIK5kVmqbKYA

机器学习大牛是如何选择回归损失函数的？

https://mp.weixin.qq.com/s/f29WSb_xZxY1S_MP3Yp0dg

机器学习中常用的损失函数你知多少？

https://mp.weixin.qq.com/s/AdpO4xxTi0G7YiTfjEz_ig

机器学习必备的分类损失函数速查手册

https://mp.weixin.qq.com/s/NY1y0N6XedmMEvfsEFcTjQ

机器学习中的目标函数总结

https://mp.weixin.qq.com/s/8KM7wUg_lnFBd0fIoczTHQ

用收缩损失(Shrinkage Loss)进行深度回归跟踪

# 聚类算法

https://mp.weixin.qq.com/s/xGPiaXTnQad3RcMwIELP4w

流式聚类算法

https://wenku.baidu.com/view/4169e77f27284b73f2425047.html

层次聚类

https://mp.weixin.qq.com/s/uSHLJKB0knVcCY759Ul25w

什么是聚类和降维？

https://mp.weixin.qq.com/s/ORLOOhufrInyPdS6fbywOw

深入浅出——基于密度的聚类方法

https://mp.weixin.qq.com/s/Vi4Yb8TOJydj9yL078iNHw

BIRCH层次聚类详解

https://mp.weixin.qq.com/s/dIsq3RKAU_K6vS0-MPKbzA

BIRCH聚类算法原理

https://mp.weixin.qq.com/s/_5A3DuVyN6aE9n5OEc19kA

数据科学家必须了解的六大聚类算法：带你发现数据之美

https://mp.weixin.qq.com/s/pDiZt4ydWw-4cYeE4gpjiw

数据科学家需要了解的5大聚类算法

https://www.zhihu.com/question/34554321

用于数据挖掘的聚类算法有哪些，各有何优势？

https://mp.weixin.qq.com/s/7zV370J7nv5wSWxdYa5Plg

DBSCAN聚类算法原理介绍，以及代码实现

https://mp.weixin.qq.com/s/8dB2OQCoZ7_kSxExm32WSA

于剑：聚类理论与算法选讲

https://mp.weixin.qq.com/s/1SOQZ3fsiYtT4emt4jvMxQ

深入浅出聚类算法

# 特征工程

https://mp.weixin.qq.com/s/ibiElLIgrT3wYx3tDYMMTw

理解特征工程

https://mp.weixin.qq.com/s/3Ce8uMf_Kyt-hEZUYfdh3g

特征工程之特征选择

https://mp.weixin.qq.com/s/tOcyfK68jW7Tr-PGCvdXMA

特征工程最后一个要点:特征预处理

https://mp.weixin.qq.com/s/c9iHdgtErVd_iitwny7_zw

Kaggle前1%参赛者经验：特征工程为何如此重要？

https://mp.weixin.qq.com/s/xbPJD0uoRB-T1x09AUYdzg

基于Python的自动特征工程——教你如何自动创建机器学习特征

# 模仿学习

https://zhuanlan.zhihu.com/p/27935902

机器人学习Robot Learning之模仿学习Imitation Learning的发展

https://zhuanlan.zhihu.com/p/25688750

模仿学习（Imitation Learning）完全介绍

https://mp.weixin.qq.com/s/naq73D27vsCOUBperKto8A

从监督式到DAgger，综述论文描绘模仿学习全貌

https://mp.weixin.qq.com/s/LNNqp2KsEAljG26hY43mUw

ICML2018 模仿学习教程

# 三门问题

https://www.zhihu.com/question/26709273/

蒙提霍尔问题（又称三门问题、山羊汽车问题）的正解是什么？

https://zhuanlan.zhihu.com/p/21461266

数学杂谈——“三门问题”：Monty Hall Problem

https://zhuanlan.zhihu.com/p/23338174

蒙提霍尔问题/三门问题（Monty Hall problem）

# 机器学习分类器性能指标

很多学习器是为测试样本产生一个实值或概率预测，然后将这个预测值与一个分类阈值（threshold）进行比较，若大于阈值则分为正类，否则为反类。这个实值或概率预测结果的好坏，直接决定了学习器的泛化能力。实际上，根据这个实值或概率预测结果，我们可将测试样本进行**排序**，“最可能”是正例的排在最前面，“最不可能”是正例的排在最后面。这样，分类过程就相当于在这个排序中以某个“截断点”（cut point）将样本分为两部分，前一部分判作正例，后一部分则判作反例。

在不同的应用任务中，我们可根据任务需求来采用不同的截断点，例如若我们更重视 “查准率”（precision），则可选择排序中靠前的位置进行截断，若更重视“查全率”（recall，也称召回率），则可选择靠后的位置进行截断。

对于二分类问题，可将样例根据其真实类别与学习器预测类别的组合划分为真正例（true positive）、假正例（false positive）、真反例（true negative）和假反例（false negative）。

查准率P和查全率R的定义如下：

$$P=\frac{TP}{TP+FP},R=\frac{TP}{TP+FN}$$

以P和R为坐标轴，所形成的曲线就是P-R曲线。曲线下方的面积一般称为AP（Average Precision）。

![](/images/article/AP.png)

>注意：   
>1.测试样本的**排序**过程非常重要。不然P-R曲线的峰值可能出现在图形的中部。   
>2.虽然P-R曲线总体上是个下降曲线，但不是严格的单调下降曲线。在局部，会由于TP样本的增多，使P值升高。
