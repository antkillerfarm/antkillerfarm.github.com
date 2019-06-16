---
layout: post
title:  机器学习（二十四）——单分类SVM&多分类SVM, Stacking, 三门问题, 用户画像
category: ML 
---

# Optimizer（续）

## Adam

Adaptive Moment Estimation借用了卡尔曼滤波的思想，对$$g_t,g_t^2$$进行滤波：

$$m_t = \beta_1 m_{t-1} + (1 - \beta_1) g_t$$

$$v_t = \beta_2 v_{t-1} + (1 - \beta_2) g_t^2$$

估计：

$$\hat{m}_t = \dfrac{m_t}{1 - \beta^t_1}$$

$$\hat{v}_t = \dfrac{v_t}{1 - \beta^t_2}$$

更新：

$$\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t$$

## AdamW

Adam自动调整学习率，大幅提高了训练速度，也很少需要调整学习率，但是有相当多的资料报告Adam优化的最终精度略低于SGD。问题出在哪呢，其实Adam本身没有问题，问题在于目前大多数DL框架的L2 loss实现用的是weight decay的方式，而weight decay在与Adam共同使用的时候有互相耦合。

为了解决这个问题，人们又提出了AdamW。

参考：

https://mp.weixin.qq.com/s/TVJIC7IEUeWypN8Z1NGKaA

都9102年了，别再用Adam + L2 Loss了

## Nadam

http://cs229.stanford.edu/proj2015/054_report.pdf

ncorporating Nesterov Momentum into Adam

## 参考

http://sebastianruder.com/optimizing-gradient-descent/

An overview of gradient descent optimization algorithms

https://mp.weixin.qq.com/s/k_d02G2V4yd6HdGfw2mf1Q

从修正Adam到理解泛化：概览2017年深度学习优化算法的最新研究进展

https://mp.weixin.qq.com/s/cOCCapYrmrS_DyPkj_XRlg

常见的几种最优化方法

https://morvanzhou.github.io/tutorials/machine-learning/ML-intro/3-06-speed-up-learning/

加速神经网络训练

http://www.cnblogs.com/neopenx/p/4768388.html

自适应学习率调整：AdaDelta

https://mp.weixin.qq.com/s/VoBK-l_ieSg2UupC2ix2pA

听说你了解深度学习最常用的学习算法：Adam优化算法？

https://mp.weixin.qq.com/s/YRyqvlNe24mlFZ7GB9vDnw

一文看懂常用的梯度下降算法

https://mp.weixin.qq.com/s/qncTSBCvjMzAual5Sz9R3A

解析深度学习优化：Momentum、RMSProp 和 Adam

https://mp.weixin.qq.com/s/q7BI-YyhtmNzUfBMTKVdqQ

Hitting time analysis of SGLD！

https://mp.weixin.qq.com/s/vt7BEHbwJrAzlL2Pc-6QFg

掌握机器学习数学基础之优化（上）

https://mp.weixin.qq.com/s/6NBLLLa-S625iaehR8zDfQ

掌握机器学习数学基础之优化（下）

https://mp.weixin.qq.com/s/o10Fp2VCwoLqgzirbGL9LQ

如何估算深度神经网络的最优学习率

https://mp.weixin.qq.com/s/T4f4W0V6YNBbjWqWBF19mA

目标函数的经典优化算法介绍

https://mp.weixin.qq.com/s/R_0_E5Ieaj9KiWgg1prxeg

为什么梯度的方向与等高线切线方向垂直？

https://mp.weixin.qq.com/s/0gdGNv98DytB8KxwVu_M0A

通俗易懂讲解Deep Learning最优化方法之AdaGrad

https://mp.weixin.qq.com/s/VVHe2msyeUTGiC7f_f0FFA

一文概览深度学习中的五大正则化方法和七大优化策略

https://mp.weixin.qq.com/s/qp5tJynA2uZIgv-IzJ_lrA

从基础知识到实际应用，一文了解“机器学习非凸优化技术”

https://mp.weixin.qq.com/s/zFGQzC_uQdAwlr9BzA-CYg

深度学习需要了解的四种神经网络优化算法

https://mp.weixin.qq.com/s/rUqIfKWmEBVjajlAn2HXfg

理解深度学习中的学习率及多种选择策略

https://mp.weixin.qq.com/s/UfplwSgyWnLNiCdIrconhA

SGD的那些变种，真的比SGD强吗

# 单分类SVM&多分类SVM

原始的SVM主要用于二分类，然而稍加变化，也可用于单分类和多分类。

## 单分类SVM

单分类任务是一类特殊的分类任务。在该任务中，大多数样本只有positive一类标签，而其他样本则笼统的划为另一类。

单分类SVM（也叫Support Vector Domain Description(SVDD)）是一种单分类算法。和普通SVM相比，它不再使用maximum margin了，因为这里并没有两类的data。

单分类SVM的目标，实际上是确定positive样本的boundary。boundary之外的数据，会被分为另一类。这实际上就是一种异常检测的算法了。它主要适用于negative样本的特征不容易确定的场景。

![](/images/article/one_class_svm.png)

这里可以假设最好的boundary要远离feature space中的原点。左边是在original space中的boundary，可以看到有很多的boundary都符合要求，但是比较靠谱的是找一个比较紧（closeness）的boundary（红色的）。这个目标转换到feature space就是找一个离原点比较远的boundary，同样是红色的直线。

当然这些约束条件都是人为加上去的，你可以按照你自己的需要采取相应的约束条件。比如让data的中心离原点最远。

下面我们讨论一下SVDD的算法实现。

首先定义需要最小化的目标函数：

$$\begin{align}
&\operatorname{min}& & F(R,a,\xi_i) = R^2 + C \sum_{i=1}^N \xi_i\\
&\operatorname{s.t.}& & (x_i - a)^T (x_i - a) \leq R^2 + \xi_i\text{,} \qquad \xi_i \geq 0
\end{align}$$

这里a表示形状的中心，R表示半径，C和$$\xi$$的含义与普通SVM相同。

Lagrangian算子：

$$L(R,a,\alpha_i,\xi_i) = R^2 + C \sum_{i=1}^N \xi_i - \sum_{i=1}^N \gamma_i \xi_i - \sum_{i=1}^N \alpha_i \left(  R^2 + \xi_i - (x_i - c)^T (x_i - c) \right)$$

对偶问题：

$$L = \sum_{i=1}^N \alpha_i (x_i^T \cdot x_i) - \sum_{i,j=1}^N \alpha_i \alpha_j (x_i^T \cdot x_i)$$

使用核函数：

$$L = \sum_{i=1}^N \alpha_i K(x_i,x_i) - \sum_{i,j=1}^N \alpha_i \alpha_j K(x_i,x_j)$$

预测函数：

$$y(x) = \sum_{i=1}^N \alpha_i K(x,x_n) + b$$

根据计算结果的符号，来判定是正常样本，还是异常样本。

参考：

https://www.projectrhea.org/rhea/index.php/One_class_svm

One-Class Support Vector Machines for Anomaly Detection

https://www.zhihu.com/question/22365729

什么是一类支持向量机（one class SVM）

## 多分类SVM

多分类任务除了使用多分类算法之外，也可以通过对两分类算法的组合来实施多分类。常用的方法有两种：one-against-rest和DAG SVM。

### one-against-rest

比如我们有5个类别，第一次就把类别1的样本定为正样本，其余2，3，4，5的样本合起来定为负样本，这样得到一个两类分类器，它能够指出一篇文章是还是不是第1类的；第二次我们把类别2的样本定为正样本，把1，3，4，5的样本合起来定为负样本，得到一个分类器，如此下去，我们可以得到5个这样的两类分类器（总是和类别的数目一致）。

但有时也会出现两种很尴尬的情况，例如拿一篇文章问了一圈，每一个分类器都说它是属于它那一类的，或者每一个分类器都说它不是它那一类的，前者叫分类重叠现象，后者叫不可分类现象。

分类重叠倒还好办，随便选一个结果都不至于太离谱，或者看看这篇文章到各个超平面的距离，哪个远就判给哪个。不可分类现象就着实难办了，只能把它分给第6个类别了……

更要命的是，本来各个类别的样本数目是差不多的，但“其余”的那一类样本数总是要数倍于正类（因为它是除正类以外其他类别的样本之和嘛），这就人为的造成了“数据集偏斜”问题。

### DAG SVM

![](/images/article/dag_svm.png)

DAG SVM（也称one-against-one）的分类思路如上图所示。

粗看起来DAG SVM的分类次数远超one-against-rest，然而由于每次分类都只使用了部分数据，因此，DAG SVM的计算量反而更小。

其次，DAG SVM的误差上限有理论保障，而one-against-rest则不然（准确率可能降为0）。

显然，上面提到的两种方法，不仅可用于SVM，也适用于其他二分类算法。

参考：

http://www.blogjava.net/zhenandaci/archive/2009/03/26/262113.html

将SVM用于多类分类

## Hinge Loss

在之前的SVM的推导中，我们主要是从解析几何的角度，给出了SVM的计算公式。但SVM实际上也是有loss function的：

$$\ell(y) = \max(0, 1-t \cdot y)$$

其中，$$t=\pm 1$$表示分类标签，$$y=w \cdot x+b$$表示分类超平面计算的score。可以看出当t和y有相同的符号时（意味着 y 预测出正确的分类），loss为0。反之，则会根据y线性增加one-sided error。

![](/images/article/hinge_loss.png)

由于其函数形状像个合叶，因此又名Hinge Loss函数。

多分类SVM的Hinge Loss公式：

$$\ell(y) = \sum_{t \ne y} \max(0, 1 + \mathbf{w}_t \mathbf{x} - \mathbf{w}_y \mathbf{x})$$

参见：

http://www.jianshu.com/p/4a40f90f0d98

Hinge loss

http://mp.weixin.qq.com/s/96_u9QM63SISwTbimmH6wA

支持向量回归机

https://mp.weixin.qq.com/s/0yvu3oG7YXUdeQz4QwEJ-A

线性分类原来是这么一回事

https://mp.weixin.qq.com/s/1ZNCbj5kMFENzP_SapQYgg

Softmax分类及与SVM比较

## 数据不平衡问题

SVM中超参数C决定了错误分类的惩罚值，为了处理不平衡类别的问题，我们可以给C按类加权重：

$$C_k=C*W_k$$

其中，权值和类别K出现的频率成反比。

## Platt scaling

https://blog.csdn.net/giskun/article/details/49329095

SVM的概率输出（Platt scaling）

# Stacking

模型融合的方法除了Bagging和Boosting，还有Stacking。

参考：

https://mp.weixin.qq.com/s/lYj-GVNSDp26czRXbf0iNw

如果你会模型融合！那么，我要和你做朋友！！

# 三门问题

https://www.zhihu.com/question/26709273/

蒙提霍尔问题（又称三门问题、山羊汽车问题）的正解是什么？

https://zhuanlan.zhihu.com/p/21461266

数学杂谈——“三门问题”：Monty Hall Problem

https://zhuanlan.zhihu.com/p/23338174

蒙提霍尔问题/三门问题（Monty Hall problem）

# 用户画像

https://mp.weixin.qq.com/s/TydTE50NzxMbGAigqv6BCw

如何破解“千人千面”，深度解读用户画像

https://mp.weixin.qq.com/s/bdAp_FExIK5IJH8WnD53Wg

你真的懂用户画像吗？

https://mp.weixin.qq.com/s/LN6ib-8b_SZHR9u_-OMwNg

推荐系统眼中的你：内容画像与用户画像

https://mp.weixin.qq.com/s/95Zklj8ovheQV3Gnc-2h-Q

小米大数据总监司马云瑞详解小米用户画像的演进及应用解读
