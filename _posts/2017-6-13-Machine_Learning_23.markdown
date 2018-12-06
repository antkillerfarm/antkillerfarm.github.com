---
layout: post
title:  机器学习（二十三）——AutoML, Optimizer
category: ML 
---

# HMM（续）

## 前向算法

forward算法是求解问题2的常用算法。

仍以上面的掷骰子为例，要算用正常的三个骰子掷出这个结果的概率，其实就是将所有可能情况的概率进行加和计算。同样，简单而暴力的方法就是把穷举所有的骰子序列，还是计算每个骰子序列对应的概率，但是这回，我们不挑最大值了，而是把所有算出来的概率相加，得到的总概率就是我们要求的结果。

穷举法的计算量太大，不适用于计算较长的马尔可夫链。但是我们可以观察一下穷举法的计算步骤。

![](/images/article/forward_algorithm.png)

上图是某骰子序列的穷举计算过程，可以看出第3步计算的概率和公式的某些项，实际上在之前的步骤中已经计算出来了，前向递推的计算量并没有想象中的大。

## Baum–Welch算法

Baum–Welch算法是求解问题3的常用算法，由Baum和Welch于1972年提出。它虽然是EM算法的一个特例，但后者却是1977年才提出的。

>Leonard Esau Baum，1931～2017，美国数学家，哈佛博士（1958）。国防分析研究所研究员，70年代末，加盟对冲基金——文艺复兴科技公司。

>Lloyd Richard Welch，生于1927年，美国数学家，加州理工博士（1958），南加州大学教授。美国工程院院士，Shannon Award获得者（2003）。

Baum–Welch算法也叫前向后向算法。因为它包含了前向和后向两个步骤。

1:expectation，计算隐变量的概率分布，并得到可观察变量与隐变量联合概率的log-likelihood在前面求得的隐变量概率分布下的期望。这个步骤就是所谓的前向步骤，算法和求解问题2的forward算法是一致的

2:maximization求得使上述期望最大的新的模型参数。若达到收敛条件则退出，否则回到步骤1。

前向后向算法则主要是解决Expectation这步中求隐变量概率分布的一个算法，它利用dynamic programming大大减少了计算量。

此外，训练HMM模型时，也需要对模型参数进行随机初始化，不然和神经网络一样，由于参数没有差异性，而无法进行训练。

参考：

https://blog.csdn.net/xmu_jupiter/article/details/50965039

HMM的Baum-Welch算法和Viterbi算法公式推导细节

## HMM在NLP领域的应用

具体到分词系统，可以将“标签”当作隐含状态，“字或词”当作可见状态。那么，几个NLP的问题就可以转化为：

词性标注：给定一个词的序列（也就是句子），找出最可能的词性序列（标签是词性）。如ansj分词和ICTCLAS分词等。

分词：给定一个字的序列，找出最可能的标签序列（断句符号：[词尾]或[非词尾]构成的序列）。结巴分词目前就是利用BMES标签来分词的，B（开头）,M（中间),E(结尾),S(独立成词）

命名实体识别：给定一个词的序列，找出最可能的标签序列（内外符号：[内]表示词属于命名实体，[外]表示不属于）。如ICTCLAS实现的人名识别、翻译人名识别、地名识别都是用同一个Tagger实现的。

综上，**在监督学习中，一般把训练数据当作HMM的可见状态，而把标签当作隐含状态。**当然这里的标签可能是生成最终训练标签的一个概率模型的参数。

参考：

http://blog.sina.com.cn/s/blog_8267db980102wq4l.html

HMM识别新词

# AutoML

## 概述

尽管现在已经有许多成熟的ML算法，然而大多数ML任务仍依赖于专业人员的手工编程实现。

然而但凡做过若干同类项目的人都明白，在算法选择和参数调优的过程中，有大量的套路可以遵循。

比如有人就总结出参加kaggle比赛的套路：

http://www.jianshu.com/p/63ef4b87e197

一个框架解决几乎所有机器学习问题

https://mlwave.com/kaggle-ensembling-guide/

Kaggle Ensembling Guide

既然是套路，那么就有将之自动化的可能，比如下面网页中，就有好几个AutoML的框架：

https://mp.weixin.qq.com/s/QIR_l8OqvCQzXXXVY2WA1w

十大你不可忽视的机器学习项目

下面给几个套路图：

![](/images/article/ML.png)

![](/images/article/AutoML.jpg)

![](/images/img2/AutoNLP.png)

## 超参数

所谓hyper-parameters，就是机器学习模型里面的框架参数，比如聚类方法里面类的个数，或者话题模型里面话题的个数等等，都称为超参数。它们跟训练过程中学习的参数（权重）是不一样的，通常是手工设定，不断试错调整，或者对一系列穷举出来的参数组合一通枚举（叫做网格搜索）。

AutoML很大程度上就是自动化寻找合适的hyper-parameters的方案或方法。

参见：

http://blog.csdn.net/xiewenbo/article/details/51585054

什么是超参数

http://www.cnblogs.com/fhsy9373/p/6993675.html

如何选取一个神经网络中的超参数hyper-parameters

https://mp.weixin.qq.com/s/Q7Xqb-GZXktFIM5yW8moPg

机器学习中的超参数的选择与交叉验证

## 参考

https://mp.weixin.qq.com/s/-0--sZXjMvFKxxo87w4udg

自动机器学习工具全景图：精选22种框架，解放炼丹师

http://blog.csdn.net/aliceyangxi1987/article/details/71079448

一个框架解决几乎所有机器学习问题

https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-algorithm-cheat-sheet

MS提供的ML算法选择指南

https://mp.weixin.qq.com/s/53AcAZcCKBZI-i1CORl0bQ

分分钟带你杀入Kaggle Top 1%

https://mp.weixin.qq.com/s/NwVGkAcoDmyXKrYFUaK2Bw

如何在机器学习竞赛中更胜一筹？

https://mp.weixin.qq.com/s/5v80Qz2nEfoAig0ft_HzaA

Kaggle求生

https://mp.weixin.qq.com/s/K3EVwRFBJufXK5QKSQsPbQ

这是一份为数据科学初学者准备的Kaggle竞赛指南

https://mp.weixin.qq.com/s/hf4IOAayS29i6GB9m4GHcA

全自动机器学习：ML工程师屠龙利器

https://mp.weixin.qq.com/s/h2QQhoBfnEhU12RgatT3EA

机器学习都能自动化了？

https://mp.weixin.qq.com/s/-n-5Cp_hgkvdmsHGWEIpWw

自动化机器学习第一步：使用Hyperopt自动选择超参数

https://mp.weixin.qq.com/s/Nbwii7Di_h5Ewy5p5xzBdQ

解决机器学习问题有通法

http://automl.info/

某牛的blog

https://mp.weixin.qq.com/s/gXkD2PPNRhZGcXDxDXRAiQ

由0到1走入Kaggle-入门指导

https://mp.weixin.qq.com/s/2ZwhNN7kqigwRLqRnUOENw

谷歌做了45万次不同类型的文本分类后，总结出一个通用的“模型选择算法”

https://mp.weixin.qq.com/s/WQ-8OvF9-fRpRf5lgr5_iw

一种简单有效的网络结构搜索

https://mp.weixin.qq.com/s/g8U2C9bi75mE5OqWLPkgQw

自动化学习框架（AutoML）的性能比较

https://mp.weixin.qq.com/s/DkZGkI-CnEHfhXTDyp2nHQ

超参数搜索不够高效？这几大策略了解一下

https://zhuanlan.zhihu.com/p/48642938

分享一篇比较全面的AutoML综述

https://mp.weixin.qq.com/s/zE8N5snKK2EoM9WgAhI-_g

NeurIPS 2018 AutoML Phase1 冠军队伍 DeepSmart 团队解决方案分享

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

