---
layout: post
title:  机器学习（二十三）——Optimizer
category: ML 
---

# KNN（续）

## KNN在时间序列分析上的应用

KNN虽然主要是个分类算法，但通过构建特殊的模型，亦可应用于其他领域。其中，KNN在时间序列分析上的应用，就是一个很有技巧性的事情。

假设已知时间序列$$X:\{x_1,\dots,x_n\}$$，来预测$$x_{n+1}$$。

首先，我们选取$$x_{n+1}$$之前的最近m个序列值，作为预测值的特征向量$$X_{m\{n+1\}}$$。这里的m一般根据时间序列的周期来选择，比如商场客流的周期一般为一周。

$$X_{m\{n+1\}}$$和预测值$$x_{n+1}$$组成了扩展向量$$[X_{m\{n+1\}},x_{n+1}]$$。为了表明$$x_{n+1}$$是预测值的事实，上述向量又写作$$[X_{m\{n+1\}},y_{n+1}]$$。

依此类推，对于X中的任意$$x_i$$，我们都可以构建扩展向量$$[X_{m\{i\}},y_{i}]$$。即我们假定，$$x_i$$的值由它之前的m个序列值唯一确定。显然，由于是已经发生了的事件，这里的$$y_{i}$$都是已知的。

在X中，这样的m维特征向量共有$$n-m$$个。使用KNN算法，获得与$$X_{m\{n+1\}}$$最邻近的k个特征向量$$X_{m\{i\}}$$。然后根据这k个特征向量的时间和相似度，对k个$$y_{i}$$值进行加权平均，以获得最终的预测值$$y_{n+1}$$。

参考：

http://www.doc88.com/p-1416660147532.html

KNN算法在股票预测中的应用

https://zhuanlan.zhihu.com/p/29838009

K近邻算法

https://mp.weixin.qq.com/s?__biz=MzI4ODU5NjQ3OQ==&mid=2247483791&idx=1&sn=0fafce03d0c20a14020e193b9b5b64e6

机器学习分类算法之k-近邻算法

# Optimizer

在《机器学习（一）》中，我们已经指出梯度下降是解决凸优化问题的一般方法。而如何更有效率的梯度下降，就是本节中Optimizer的责任了。

## 原始版本

早期的梯度下降法一般用以下公式确定学习率：

$$\eta_t=\frac{\eta_0}{\sqrt{t+1}}$$

## Momentum

Momentum是梯度下降法中一种常用的加速技术。其公式为：

$$v_t = \gamma v_{t-1} + \eta \nabla_\theta J( \theta)$$

$$\theta = \theta - v_t$$

从上式可以看出，参数的更新值$$v_t$$，不仅取决于当前梯度$$\nabla_\theta J( \theta)$$，还取决于上一刻的速度$$v_{t-1}$$。

## Nesterov accelerated gradient

该方法是Momentum的一个变种。其公式为：

$$v_t = \gamma v_{t-1} + \eta \nabla_\theta J( \theta - \gamma v_{t-1})$$

$$\theta = \theta - v_t$$

>注：Yurii Nesterov，莫斯科大学应用数学系本科（1977年），凸优化理论专家。法国鲁汶天主教大学教授。2009年获John von Neumann Theory Prize。

参考：

https://zhuanlan.zhihu.com/p/22810533

比Momentum更快：揭开Nesterov Accelerated Gradient的真面目

https://zhuanlan.zhihu.com/p/27435669

从Nesterov的角度看：我们为什么要研究凸优化？

https://www.cs.cmu.edu/~ggordon/10725-F12/slides/09-acceleration.pdf

Accelerated first-order methods

## Adagrad

Momentum算法中所有的参数$$\theta$$都使用同一个学习率，而Adagrad采用了另一种方法进行优化：为每个参数确定不同的学习率。

Adagrad的基本思想：给经常更新的参数一个较小的学习率，而给很少更新的参数一个较大的学习率。

其公式为：

$$g_{t, i} = \nabla_\theta J( \theta_i )$$

$$\theta_{t+1, i} = \theta_{t, i} - \dfrac{\eta}{\sqrt{G_{t, ii} + \epsilon}} \cdot g_{t, i}$$

其中，$$G_{t, ii}$$表示参数$$\theta_i$$梯度平方和的历史累积值，$$\epsilon$$是为了防止分母为0，而加入的平滑项，数量级一般为$$10^{-8}$$。

有趣的是，如果去掉上式中的根号，则其效果会变糟。

Adagrad的优点在于：它是一个自适应算法，初值选择显得不太重要了。

Adagrad的缺点在于：训练越往后，G越大，从而学习率越小。如果在训练完成之前，学习率变为0，就会导致提前结束训练。

## Adadelta

为了克服Adagrad的缺点，Matthew D. Zeiler于2012年提出了Adadelta算法。

>注：Matthew D. Zeiler，多伦多大学本科（2009）+纽约大学博士（2013）。Clarifai创始人和CEO。读书期间，他还创立了一家给大学生卖习题册的公司。   
>个人主页：   
>http://www.matthewzeiler.com/

该算法不再使用历史累积值，而是只取最近的w个状态，这样就不会让梯度被惩罚至0。

为了避免保存前w个状态的梯度平方和，可做如下变换：

$$E[g^2]_t = \gamma E[g^2]_{t-1} + (1 - \gamma) g^2_t$$

$$\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{E[g^2]_t + \epsilon}} g_{t}$$

上边的公式，就是Hinton在同一年提出的**RMSprop算法**。其中的$$\gamma E[g^2]_{t-1}$$即可看作是前w个状态的滤波值，也可看作是Momentum算法中动量值。

Adadelta在RMSprop的基础上更进一步：

$$RMS[g]_{t}=\sqrt{E[g^{2}]_{t}+\epsilon }$$

$$\Delta \theta_t = - \dfrac{RMS[\Delta \theta]_{t-1}}{RMS[g]_{t}} g_{t}$$

也就是说，Adadelta不仅考虑了梯度的平方和，也考虑了更新量的平方和。

## Adam

Adaptive Moment Estimation借用了卡尔曼滤波的思想，对$$g_t,g_t^2$$进行滤波：

$$m_t = \beta_1 m_{t-1} + (1 - \beta_1) g_t$$

$$v_t = \beta_2 v_{t-1} + (1 - \beta_2) g_t^2$$

估计：

$$\hat{m}_t = \dfrac{m_t}{1 - \beta^t_1}$$

$$\hat{v}_t = \dfrac{v_t}{1 - \beta^t_2}$$

更新：

$$\theta_{t+1} = \theta_{t} - \dfrac{\eta}{\sqrt{\hat{v}_t} + \epsilon} \hat{m}_t$$

## Nadam

http://cs229.stanford.edu/proj2015/054_report.pdf

ncorporating Nesterov Momentum into Adam

## AdaSecant

《ADASECANT: Robust Adaptive Secant Method for Stochastic Gradient》

## 二阶Optimizer

虽然二阶Optimizer的收敛效果优于一阶Optimizer，但由于计算量较大，通常用的较少。

常用的算法有BGFS和L-BFGS。

http://www.cnblogs.com/kemaswill/p/3352898.html

优化算法-BFGS

http://blog.csdn.net/acdreamers/article/details/44728041

L-BFGS算法

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

https://mp.weixin.qq.com/s/T-v9OTcJa5OQ71QmYrFtbg

斯坦福大学提出SGD动量自调节器YellowFin

https://mp.weixin.qq.com/s/4XOI8Dq6fqe8rhtJjeyxeA

超级收敛：使用超大学习率超快速训练残差网络

http://mp.weixin.qq.com/s/Q5kBCNZs3a6oiznC9-2bVg

Michael Jordan新研究官方解读：如何有效地避开鞍点

https://mp.weixin.qq.com/s/YRyqvlNe24mlFZ7GB9vDnw

一文看懂常用的梯度下降算法

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

https://mp.weixin.qq.com/s/6u5W7Lm81Wtczdzp5WCJWw

DeepMind提出新型超参数最优化方法：性能超越手动调参和贝叶斯优化

https://mp.weixin.qq.com/s/0V8B-u5_bRM5Fu9oOAYjqw

清华大学：通过在单纯形上软门限投影的加速随机贪心坐标下降

https://mp.weixin.qq.com/s/fXlbB7KmiX0iIv6xwSxNIA

梯度下降法的三种形式BGD、SGD以及MBGD

https://mp.weixin.qq.com/s/R_0_E5Ieaj9KiWgg1prxeg

为什么梯度的方向与等高线切线方向垂直？

https://mp.weixin.qq.com/s/LuuvvL9yZ3ucXxRq0pZfsg

优化策略：Label Smoothing Regularization_LSR原理分析

https://zhuanlan.zhihu.com/p/23866364

从梯度下降到Hessian-Free优化

https://mp.weixin.qq.com/s/0gdGNv98DytB8KxwVu_M0A

通俗易懂讲解Deep Learning最优化方法之AdaGrad

https://mp.weixin.qq.com/s/HPrjEdszBSvVoVS66W-Fjw

2017年深度学习优化算法研究亮点最新综述

https://mp.weixin.qq.com/s/W06YcuGWalDbyUaZa_kZnQ

2017年深度学习优化算法最新综述

https://mp.weixin.qq.com/s/VVHe2msyeUTGiC7f_f0FFA

一文概览深度学习中的五大正则化方法和七大优化策略

https://mp.weixin.qq.com/s/qp5tJynA2uZIgv-IzJ_lrA

从基础知识到实际应用，一文了解“机器学习非凸优化技术”

https://mp.weixin.qq.com/s/7E8o1TnvmAvZgB7_AWCunQ

2018值得尝试的无参数全局优化新算法

https://mp.weixin.qq.com/s/hK6BJGAPyg_RqqilCcf2NA

全局自动优化：C++机器学习库dlib引入自动调参算法

https://mp.weixin.qq.com/s/zFGQzC_uQdAwlr9BzA-CYg

深度学习需要了解的四种神经网络优化算法

https://mp.weixin.qq.com/s/rUqIfKWmEBVjajlAn2HXfg

理解深度学习中的学习率及多种选择策略

https://mp.weixin.qq.com/s/jVjemfcLzIWOdWdxMgoxsA

超越Adam，从适应性学习率家族出发解读ICLR 2018高分论文

# 单分类SVM&多分类SVM

原始的SVM主要用于二分类，然而稍加变化，也可用于单分类和多分类。

## 单分类SVM

单分类任务是一类特殊的分类任务。在该任务中，大多数样本只有positive一类标签，而其他样本则笼统的划为另一类。

单分类SVM（也叫Support Vector Domain Description(SVDD)）是一种单分类算法。和普通SVM相比，它不再使用maximum margin了，因为这里并没有两类的data。

单分类SVM的目标，实际上是确定positive样本的boundary。boundary之外的数据，会被分为另一类。这实际上就是一种异常检测的算法了。它主要适用于negative样本的特征不容易确定的场景。

![](/images/article/one_class_svm.png)

这里可以假设最好的boundary要远离feature space中的原点。左边是在original space中的boundary，可以看到有很多的boundary都符合要求，但是比较靠谱的是找一个比较紧（closeness）的boundary（红色的）。这个目标转换到feature space就是找一个离原点比较远的boundary，同样是红色的直线。

当然这些约束条件都是人为加上去的，你可以按照你自己的需要采取相应的约束条件。比如让data的中心离原点最远。


