---
layout: post
title:  机器学习（二十二）——机器学习分类器性能指标
category: ML 
---

* toc
{:toc}

# Loss function详解

## Triplet Loss（续）

https://gehaocool.github.io/2018/03/20/Angular-Margin-%E5%9C%A8%E4%BA%BA%E8%84%B8%E8%AF%86%E5%88%AB%E4%B8%AD%E7%9A%84%E5%BA%94%E7%94%A8/

Angular Margin在人脸识别中的应用

https://mp.weixin.qq.com/s/SqaR_7gwJpUNPM7g4IHaYw

深度人脸识别中不同损失函数的性能对比

https://zhuanlan.zhihu.com/p/138850070

从Triplet loss看推荐系统中文章Embedding

https://mp.weixin.qq.com/s/tpI5k5sg15WHGv3YoMp-rA

​Triplet-loss原理与应用

https://zhuanlan.zhihu.com/p/295512971

完全解析triplet loss

## Coupled Cluster Loss

论文：

《Deep Relative Distance Learning: Tell the Difference Between Similar Vehicles》

参考：

https://blog.csdn.net/u010167269/article/details/51783446

论文中文笔记

## Focal Loss

https://zhuanlan.zhihu.com/p/28442066

何恺明团队提出Focal Loss，目标检测精度高达39.1AP，打破现有记录

https://www.zhihu.com/question/63581984

如何评价Kaiming的Focal Loss for Dense Object Detection？

https://mp.weixin.qq.com/s/Uf1lWtxOpKYCDLmCDlnVAQ

把Cross Entropy梯度分布拉‘平’，就能轻松超越Focal Loss

https://mp.weixin.qq.com/s/aKRUJt-_1QSQFcRVtuyJ4w

被忽略的Focal Loss变种

https://zhuanlan.zhihu.com/p/55036597

样本贡献不均：Focal Loss和Gradient Harmonizing Mechanism

https://mp.weixin.qq.com/s/LfCuOEndS4Y5dPqXTsE_hA

剖析Focal Loss损失函数： 消除类别不平衡+挖掘难分样本

https://mp.weixin.qq.com/s/Ryxu1qsmL_Roi5zDb1KSRQ

AP-Loss：提高单阶段目标检测性能的分类损失，超越Focal loss

https://mp.weixin.qq.com/s/lzb-R0CQ3YRY4a8WBbdOhw

Focal Loss详解以及为什么能够提高处理不平衡数据分类的表现

https://zhuanlan.zhihu.com/p/147691786

大白话Generalized Focal Loss

https://zhuanlan.zhihu.com/p/313684358

大白话Generalized Focal Loss V2

https://mp.weixin.qq.com/s/zLDUAnxghsBYiUMQQdbBGQ

Generalized Focal Loss：Focal loss魔改以及预测框概率分布

https://mp.weixin.qq.com/s/is8veFCOPpJDXA2okn7pNA

10分钟理解Focal loss数学原理与Pytorch代码

https://zhuanlan.zhihu.com/p/324613604

用概率分布评估检测框质量的Generalized Focal Loss V2

## 参考

https://mp.weixin.qq.com/s/SsHF3Zpqv61G68nim8ocnQ

深度学习激活函数全面综述论文

https://mp.weixin.qq.com/s/gw3hoDSaojVQUiD6YsMabA

理解神经网络中的目标函数

https://mp.weixin.qq.com/s/l0FRj78M73vQGqppNSyBwQ

深度学习常用损失函数总览：基本形式、原理、特点

https://mp.weixin.qq.com/s/h-QbwEbaivvHjdhDhE4V1A

如何为单变量模型选择最佳的回归函数

https://mp.weixin.qq.com/s/qXZMo_RitSenmI7x0xGNsg

中科院自动化所多媒体计算与图形学团队NIPS 2017论文提出平均Top-K损失函数，专注于解决复杂样本

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

https://mp.weixin.qq.com/s/ixYhM29-famb8lbzNYnHAg

深度学习中常用的损失函数有哪些？

https://mp.weixin.qq.com/s/AmXF0xA_T-ZjjnOt4XRgRw

谷歌提出新分类损失函数：将噪声对训练结果影响降到最低

https://www.zhihu.com/question/268105631

神经网络中，设计loss function有哪些技巧?

https://mp.weixin.qq.com/s/7cr6ptZucXzsZauItcZehw

使用一个特别设计的损失来处理类别不均衡的数据集

https://www.zhihu.com/question/264892967

深度学习中loss和accuracy的关系?

https://zhuanlan.zhihu.com/p/82199561

深度度量学习中的损失函数

https://mp.weixin.qq.com/s/CbORYhJQn27J0G4G6XpODw

用于弱监督图像语义分割的新型损失函数

https://mp.weixin.qq.com/s/Yo68YnMMvy5FXkCjBLCJuw

常见的损失函数

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
>3.有的评测为了使P-R曲线成为单调下降曲线，对原始定义进行了细微修改（如下图所示）：$$P(r_0)=\max (P(r\mid r \ge r_0))$$

![](/images/img2/AP.jpg)

ROC（Receiver operating characteristic）曲线的纵轴是真正例率（True Positive Rate，TPR），横轴是假正例率（False Positive Rate，FPR）。其定义如下：

$$TPR=\frac{TP}{TP+FN},FPR=\frac{FP}{TN+FP}$$

ROC曲线下方的面积被称为AUC（Area Under ROC Curve）。

![](/images/article/ROC.png)

更多内容参见下图：

![](/images/article/sensitivity_and_specificity.png)

原图地址：

https://en.wikipedia.org/wiki/Sensitivity_and_specificity

从这张图表衍生出一种数据可视化方式——confusion matrix：

![](/images/img2/confusion_matrix.jpg)

除此之外，还有F-measure：

![](/images/article/P_R_F.png)

如果是做搜索，那就是保证召回的情况下提升准确率；如果做疾病监测、反垃圾，则是保准确率的条件下，提升召回率。所以，在两者都要求高的情况下，可以用F-measure来衡量。

虽然precision、recall和F1为我们提供了一个单值度量，但它们不考虑返回的搜索结果的顺序。为了解决这一局限性，人们设计了以下排序感知的度量标准：

Mean Reciprocal Rank(MRR)

Average Precision(AP)

Mean Average Precision(MAP)

---

Accuracy和Precision是一对容易混淆的概念。其一般定义如下图所示：

![](/images/article/Accuracy_and_precision.svg)

当然，这个定义和机器学习中的定义无关，主要用于物理和统计领域。

这里再提几个和上述概念类似的ML术语。

**Ground truth**：在有监督学习中，数据是有标注的，以(x, t)的形式出现，其中x是输入数据，t是标注。正确的t标注是ground truth， 错误的标记则不是。（也有人将所有标注数据都叫做ground truth）

由于使用错误的数据，对模型的估计比实际要糟糕，因此使用高质量的数据是很有必要的。

**Golden**：Golden这个术语的使用范围并不局限于ML领域，凡是能够给出“标准答案”的地方，都可以将该答案称为Golden。

比如在DL领域的硬件优化中，通常使用标准算法生成Golden结果，然后用优化之后的运算结果与之比对，以验证优化的正确性。

---

上述内容中，预测结果和标签之间只有真和假两种关系，因此又被称为**二元相关性**问题。

类似的还有**分等级的相关性**问题。例如，我们有一个排序模型，它会为一个特定的查询返回5个最相关的结果。这时预测结果和ground-truth之间就不是简单的真和假的关系了。

比如，预测结果的第一名虽然不是ground-truth的第一名，但却是ground-truth的第二名。这样的话，显然就不能直接否定预测结果了。

评估这类问题，可以采用如下度量：

Cumulative Gain

Discounted Cumulative Gain

Normalized Discounted Cumulative Gain

---

![](/images/img4/AUC.jpg)

如图，modelA和modelB的ROC曲线下面积AUC是相等的，但是两个模型在不同区域的预测能力是不相同的，所以我们不能单纯根据AUC的大小来判断模型的好坏。

---

参考：

https://mp.weixin.qq.com/s/5nnHBKEToepi3dhXLfQBtw

机器学习分类器性能指标详解

https://mp.weixin.qq.com/s/6eESoUvMObXSb2jy_KPRyg

如何评价我们分类模型的性能？

https://mp.weixin.qq.com/s/mOYUCc3xKMfVw81B6zSeNw

7种最常用的机器学习算法衡量指标

https://mp.weixin.qq.com/s/zvxB6VqrSOosgGSViCmjEQ

不止准确率：为分类任务选择正确的机器学习度量指标

https://mp.weixin.qq.com/s/2HKx36bIBZAqvzdXfcSfqA

我们常听说的置信区间与置信度到底是什么？

https://mp.weixin.qq.com/s/Uowbo19wNkjT-9eAIjS8jQ

过来，我这里有个“混淆矩阵”跟你谈一谈

https://mp.weixin.qq.com/s/u-QTHFSA-8rwvL0KBVYXjQ

深度学习模型评估，从图像分类到生成模型

https://mp.weixin.qq.com/s/sKiAwLoxdP9yyZX0-R4UrA

一文读懂AUC-ROC

https://mp.weixin.qq.com/s/k-Jm24aMlPgWMtFYaKNlog

信息检索中的度量指标全解析

https://mp.weixin.qq.com/s/Q_lN83h6nA242_Tv9hqtPQ

关于个性化排序任务评价指标的大讨论

https://zhuanlan.zhihu.com/p/161703182

深挖一下F1 score

https://zhuanlan.zhihu.com/p/69101372

分类问题的评估指标一览

https://zhuanlan.zhihu.com/p/92792702

AUC的缺陷

https://zhuanlan.zhihu.com/p/33025359

ROC和CMC曲线的理解（FAR, FRR的理解）

https://mp.weixin.qq.com/s/5Kl311pNRSwjo0yDGsrySA

小孩都看得懂的ROC
