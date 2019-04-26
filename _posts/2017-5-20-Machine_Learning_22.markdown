---
layout: post
title:  机器学习（二十二）——机器学习分类器性能指标
category: ML 
---

# Loss function详解（续）

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

样本贡献不均：Focal Loss和 Gradient Harmonizing Mechanism

## Other Loss

https://mp.weixin.qq.com/s/7Jg-YvS3nvcPJ-zYhK96EA

分享神经网络中设计loss function的一些技巧

https://mp.weixin.qq.com/s/cYcztl8N9JF-XXp9xLJIxg

一文道尽softmax loss及其变种

https://mp.weixin.qq.com/s/MTeuRYutMiCmthEAObyAIg

从最优化的角度看待Softmax损失函数

https://zhuanlan.zhihu.com/p/23340343

Center Loss及其在人脸识别中的应用

https://zhuanlan.zhihu.com/p/34404607

人脸识别的LOSS（上）

https://zhuanlan.zhihu.com/p/34436551

人脸识别的LOSS（下）

https://mp.weixin.qq.com/s/kI22wSoyNT3QXXI8pVwbjA

腾讯AI Lab提出新型损失函数LMCL：可显著增强人脸识别模型的判别能力

https://mp.weixin.qq.com/s/8KM7wUg_lnFBd0fIoczTHQ

用收缩损失(Shrinkage Loss)进行深度回归跟踪

https://mp.weixin.qq.com/s/VNPX5BADo4EJkIp1yWGoag

斯坦福学者提出GIoU，目标检测任务的新Loss

https://mp.weixin.qq.com/s/piYyhPbA6kAXuSE5yHfQ1g

人脸识别损失函数综述

## 参考

https://mp.weixin.qq.com/s/gw3hoDSaojVQUiD6YsMabA

理解神经网络中的目标函数

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

![](/images/article/ROC.gif)

更多内容参见下图：

![](/images/article/sensitivity_and_specificity.png)

原图地址：

https://en.wikipedia.org/wiki/Sensitivity_and_specificity

从这张图表衍生出一种数据可视化方式——confusion matrix：

![](/images/img2/confusion_matrix.jpg)

除此之外，还有F-measure：

![](/images/article/P_R_F.gif)

如果是做搜索，那就是保证召回的情况下提升准确率；如果做疾病监测、反垃圾，则是保准确率的条件下，提升召回率。所以，在两者都要求高的情况下，可以用F-measure来衡量。

Accuracy和Precision是一对容易混淆的概念。其一般定义如下图所示：

![](/images/article/Accuracy_and_precision.svg)

当然，这个定义和机器学习中的定义无关，主要用于物理和统计领域。

这里再提几个和上述概念类似的ML术语。

**Ground truth**：在有监督学习中，数据是有标注的，以(x, t)的形式出现，其中x是输入数据，t是标注。正确的t标注是ground truth， 错误的标记则不是。（也有人将所有标注数据都叫做ground truth）

由于使用错误的数据，对模型的估计比实际要糟糕，因此使用高质量的数据是很有必要的。

**Golden**：Golden这个术语的使用范围并不局限于ML领域，凡是能够给出“标准答案”的地方，都可以将该答案称为Golden。

比如在DL领域的硬件优化中，通常使用标准算法生成Golden结果，然后用优化之后的运算结果与之比对，以验证优化的正确性。

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

# HMM

![](/images/article/HMM.png)

![](/images/article/HMM_2.png)

![](/images/article/HMM_3.png)

上图中的隐含状态，也称states。可见状态，也称observations。黑线被称为transition probability，而红线则是emission probability或output probability。第一个observations的概率被称为start probability。

因此一个HMM可以表示为：

$$\mu=(A,B,\Pi)$$

其中，A是transition probability，B是emission probability，$$\Pi$$是start probability。

如果可见状态和隐含状态之间，存在一对一的关系，那么HMM就退化成普通的Markov链了。

和HMM（Hidden Markov Model，隐马尔可夫模型）模型相关的算法主要分为三类，分别解决三种问题：

1）**知道骰子有几种（隐含状态数量），每种骰子是什么（转换概率），根据掷骰子掷出的结果（可见状态链），我想知道每次掷出来的都是哪种骰子（隐含状态链）。**

这个问题呢，在语音识别领域呢，叫做解码问题。这个问题其实有两种解法，会给出两个不同的答案。每个答案都对，只不过这些答案的意义不一样。第一种解法求最大似然状态路径，说通俗点呢，就是我求一串骰子序列，这串骰子序列产生观测结果的概率最大。第二种解法呢，就不是求一组骰子序列了，而是求每次掷出的骰子分别是某种骰子的概率。比如说我看到结果后，我可以求得第一次掷骰子是D4的概率是0.5，D6的概率是0.3，D8的概率是0.2。

2）**知道骰子有几种（隐含状态数量），每种骰子是什么（转换概率），根据掷骰子掷出的结果（可见状态链），我想知道掷出这个结果的概率。**

看似这个问题意义不大，因为你掷出来的结果很多时候都对应了一个比较大的概率。问这个问题的目的呢，其实是检测观察到的结果和已知的模型是否吻合。如果很多次结果都对应了比较小的概率，那么就说明我们已知的模型很有可能是错的，有人偷偷把我们的骰子給换了。问题2的更一般的用法是：从若干种模型中选择一个概率最大的模型。

3）**知道骰子有几种（隐含状态数量），不知道每种骰子是什么（转换概率），观测到很多次掷骰子的结果（可见状态链），我想反推出每种骰子是什么（转换概率）。**

这个问题很重要，因为这是最常见的情况。很多时候我们只有可见结果，不知道HMM模型里的参数，我们需要从可见结果估计出这些参数，这是训练建模的一个必要步骤。

参考：

https://www.zhihu.com/question/20962240

如何用简单易懂的例子解释隐马尔可夫模型？

http://www.cnblogs.com/kaituorensheng/archive/2012/11/29/2795499.html

隐马尔可夫模型

https://mp.weixin.qq.com/s/9MmHDVDal57pdotwxAn_uQ

HMM模型详解

https://mp.weixin.qq.com/s/PzPRKZa1C5IlEUASWt71cA

从朴素贝叶斯到维特比算法：详解隐马尔科夫模型

https://mp.weixin.qq.com/s/aIpRx-etMkh7jwL2lFNa4w

理解隐马尔可夫模型

https://mp.weixin.qq.com/s/O0oAzJ-Hk9pQkojgdgT4qg

隐马尔可夫模型
