---
layout: post
title:  机器学习（四十）——Adaboost
category: ML 
---

* toc
{:toc}

# Adaboost

Adaboost是Yoav Freund和Robert Schapire于1997年提出的算法。两人后来因为该算法被授予Gödel Prize（2003）。

>Yoav Freund，UCSC博士，UCSD教授。

>Robert Elias Schapire，MIT博士。先后供职于Princeton University、AT&T Labs和Microsoft Research。

>Gödel Prize，由欧洲计算机学会（EATCS）与美国计算机学会基础理论专业组织（ACM SIGACT）于1993年共同设立，颁给理论计算机领域最杰出的学术论文。其名称取自Kurt Gödel。

>Kurt Friedrich Gödel，1906～1978，奥地利逻辑学家，数学家，哲学家，后加入美国藉。维也纳大学博士（1930）。在逻辑学方面，他是继Aristotle、Gottlob Frege之后最伟大的逻辑学家。在数学方面，他以哥德尔不完备定理著称，和Bertrand Russell、 David Hilbert、Georg Cantor齐名。

Adaboost既可用于分类问题，也可用于回归问题。这里仅针对二分类问题进行讨论。

假设我们有数据集$$\{(x_1, y_1), \ldots, (x_N, y_N)\}$$，其中$$y_i \in \{-1, 1\}$$，还有一系列弱分类器$$\{k_1, \ldots, k_L\}$$。

由于Boost算法是个串行算法，每次迭代就会加入一个弱分类器。这样m-1次迭代之后的分类器如下所示：

$$C_{(m-1)}(x_i) = \alpha_1k_1(x_i) + \cdots + \alpha_{m-1}k_{m-1}(x_i)$$

而m次迭代之后的分类器则为：

$$C_{m}(x_i) = C_{(m-1)}(x_i) + \alpha_m k_m(x_i)$$

如何选择新加入的弱分类器$$k_m$$和对应的权重$$\alpha_m$$呢？我们可以定义误差E如下所示：

$$E = \sum_{i=1}^N e^{-y_i C_m(x_i)}$$

令$$w_i^{(1)} = 1,w_i^{(m)} = e^{-y_i C_{m-1}(x_i)}$$，则：

$$E = \sum_{i=1}^N w_i^{(m)}e^{-y_i\alpha_m k_m(x_i)}$$

因为$$k_m$$分类正确时，$$y_i k_m(x_i) = 1$$，分类错误时，$$y_i k_m(x_i) = -1$$。所以：

$$E = \sum_{y_i = k_m(x_i)} w_i^{(m)}e^{-\alpha_m} + \sum_{y_i \neq k_m(x_i)} w_i^{(m)}e^{\alpha_m}\\= \sum_{i=1}^N w_i^{(m)}e^{-\alpha_m} + \sum_{y_i \neq k_m(x_i)} w_i^{(m)}(e^{\alpha_m}-e^{-\alpha_m})$$

可以看出和$$k_m$$相关的实际上只有上式的右半部分。显然，使得$$\sum_{y_i \neq k_m(x_i)} w_i^{(m)}$$最小的$$k_m$$，也会令E最小，这也就是我们选择加入的$$k_m$$。

对E求导，得：

$$\frac{d E}{d \alpha_m} = \frac{d (\sum_{y_i = k_m(x_i)} w_i^{(m)}e^{-\alpha_m} + \sum_{y_i \neq k_m(x_i)} w_i^{(m)}e^{\alpha_m}) }{d \alpha_m}$$

令导数为0，可得：

$$\alpha_m = \frac{1}{2}\ln\left(\frac{\sum_{y_i = k_m(x_i)} w_i^{(m)}}{\sum_{y_i \neq k_m(x_i)} w_i^{(m)}}\right)$$

令$$\epsilon_m = \sum_{y_i \neq k_m(x_i)} w_i^{(m)} / \sum_{i=1}^N w_i^{(m)}$$，则：

$$\alpha_m = \frac{1}{2}\ln\left( \frac{1 - \epsilon_m}{\epsilon_m}\right)$$

参考：

https://mp.weixin.qq.com/s/G06VDc6iTwmNGsH4IfSeJQ

Adaboost从原理到实现

https://mp.weixin.qq.com/s/PZ-1fkNvdJmv_8zLbvoW1g

Adaboost算法原理小结

https://mp.weixin.qq.com/s/KoOUgwXLOfJfOjWhbFX52Q

如果Boosting你懂，那Adaboost你懂么？
