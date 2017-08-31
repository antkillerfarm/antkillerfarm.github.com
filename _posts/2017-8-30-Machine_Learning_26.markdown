---
layout: post
title:  机器学习（二十六）——概率图模型, 机器学习的算法体系&相关术语表
category: theory 
---

# 概率图模型

## 资料

probabilistic graphical model（PGM）最早由Judea Pearl发明。

这方面比较重要的文章和书籍有：

http://www.cis.upenn.edu/~mkearns/papers/barbados/jordan-tut.pdf

Michael Irwin Jordan著。

《Probabilistic Graphical Models: Principles and Techniques》，Daphne Koller，Nir Friedman著（2009年）。

>注：Judea Pearl，1936年生，以色列-美国计算机科学家，UCLA教授。2011年获得图灵奖。

>Michael Irwin Jordan，1956年生，美国计算机科学家。UCSD博士，先后执教于MIT和UCB。吴恩达的导师。

>Daphne Koller，女，1968年生，以色列-美国计算机科学家。斯坦福大学博士及教授。和吴恩达共同创立在线教育平台Coursera。

>Nir Friedman，1967年生，以色列计算机科学家。斯坦福大学博士，耶路撒冷希伯来大学教授。

http://www.cs.cmu.edu/~epxing/Class/10708-14/lectures/

CMU的邢波（Eric Xing）所开的概率图模型课程。

## 概述

概率图模型的三要素：Graph：$$\mathcal{G}$$、Model：$$\mathcal{M}$$和Data：$$\mathcal{D}\equiv\{X^{(i)}_1,\dots,X^{(i)}_m\}^N_{i=1}$$。

它要解决的三大问题：

1.**表示**。如何获取或定义真实世界的不确定度？如何对领域知识/假设/约束编码？

2.**推断**。根据模型/数据，推断答案。

$$\text{e.g.}:P(x_i|\mathcal{D})$$

3.**学习**。根据数据确定哪个模型是正确的。

$$\text{e.g.}:\mathcal{M}=\arg\max_{\mathcal{M}\in M}F(\mathcal{D};\mathcal{M})$$

![](/images/article/PGM.png)

上图是PGM的一个示例。其中$$X_i$$表示随机变量，图中共有8个随机变量，假设它们均为二值变量，则整个状态空间共有$$2^8$$种组合。遍历这样大的状态空间无疑是一件极为费力的事情。

如果$$X_i$$是条件独立的话，则由上图可得：

$$P(X_1,\dots,X_8)=P(X_2)P(X_4|X_2)P(X_5|X_2)P(X_1)P(X_3|X_1)\\P(X_6|X_3,X_4)P(X_7|X_6)P(X_8|X_5,X_6)$$

这样，状态空间就缩小为$$2+4+4+2+4+8+4+8=36$$种组合了。

根据边的类型，PGM可分为两类：

1.有向边表示变量间的**因果**关系。这样的PGM，常称为Bayesia Network（BN）或Directed Graphical Model（DGM）。

2.无向边表示变量间的**相关**关系。这样的PGM，常称为Markov Random Field（MRF）或Undirected Graphical Model（UGM）。

>注：因果关系是一种强逻辑关系，需要变量间有深刻的内在联系。而相关关系要弱的多，典型的例子就是《机器学习（十七）》中的尿布和啤酒的故事。尿布和啤酒虽然正相关，然而它们本身却没有多大的联系。

根据模型的不同，PGM又可分为生成模型（Generative Model, GM）和判别模型（Discriminative Model, DM）。两者的区别在《机器学习（二）》中已经简单提到过，这里做一个扩展。

之前提到的机器学习算法，主要是建立特征向量X和标签Y之间的联系。但是实际情况下，X中的状态不一定都能得到，因此可以根据可见性，将X分为可观测变量集合O和其他变量集合R，Y也不一定是一个标签，而可能是一个变量集合。即：

$$GM:P(Y,R,O)\to P(Y|O)$$

$$DM:P(Y,R|O)\to P(Y|O)$$

注意，在贝叶斯学派的观点中，模型的参数也是随机变量，因此，R在某些情况下，不仅包含不可观测的变量，也包含模型参数。

## 贝叶斯网络

贝叶斯网络是最简单的有向图模型。

首先给出几个术语的定义：

**有向无环图(Directed Acyclic Graph, DAG)**：这个术语的字面意思很清楚，不解释。

**马尔可夫毯(Markov Blanket, MB)**：有向图——结点A的父结点+A的子结点+A的子结点的其他父结点。如下图所示：

![](/images/article/Markov_blanket.png)

无向图——结点A的邻近结点。

下图是图模型的部分变种之间的关系图。

![](/images/article/Generative_Models.png)

# 机器学习的算法体系&相关术语表

## 算法体系

![](/images/article/ML_2.png)

原文：

https://mp.weixin.qq.com/s/HapJwwmN3-dbQvzp2jzt1w

一文看懂机器学习的算法体系

## 相关术语表

| 缩写 | 全称 | 中文名 | 备注 |
|:--:|:--:|:--:|:--:|
| LR | Linear Regression | 线性回归 |  |
| LS | Least Squares | 最小二乘法 |  |
| LWR | Locally Weighted linear Regression | 局部加权线性回归 |  |
| LR | Logistic Regression | 逻辑回归 |  |
| MLE | Maximum Likelihood Estimator | 最大似然估计 |  |
| GLM | Generalized Linear Model | 广义线性模型 |  |
| IID | Independent and Identically Distributed | 独立同分布 |  |
| DLA | Discriminative Learning Algorithm | 判别学习算法 |  |
| GLA | Generative Learning Algorithms | 生成学习算法 |  |
| GDA | Gaussian Discriminant Analysis | 高斯判别分析 |  |
| NB | Naive Bayes | 朴素贝叶斯 |  |
| SVM | Support Vector Machines | 支持向量机 |  |
| SMO | Sequential Minimal Optimization | 序列最小优化方法 |  |
| PAC | Probably Approximately Correct | 可能近似正确 |  |
| ERM | Empirical Risk Minimization | 经验风险最小化 |  |
| EM | Expectation-Maximization | 期望最大化 |  |
| GMM | Mixture of Gaussians Model | 高斯混合模型 |  |
| SVD | Singular Value Decomposition | 奇异值分解 |  |
| PPMCC/PCC | Pearson Product-Moment Correlation Coefficient | 皮尔逊相关系数 |  |
| PMF | Probabilistic Matrix Factorization | 概率矩阵分解算法 |  |
| RMSE | Root-Mean-Square Error | 均方根误差 |  |
| ALS | Alternating Least Squares | 交替最小二乘算法 |  |
| PCA | Principal Components Analysis | 主成分分析 |  |
| ELM | Extreme Learning Machine | 极限学习机 |  |
| ICA | Independent Components Analysis | 独立成分分析 |  |
| CDF | Cumulative Distribution Function | 累积分布函数 |  |
| PDF | Probability Density Function | 概率密度函数 |  |
| ECDF | Empirical Distribution Function | 实证分配函数 |  |
| LDA | Latent Dirichlet Allocation | 隐式狄利克雷划分 |  |
| LDA | Linear Discriminant Analysis | 线性判别分析 |  |
| MCMC | Markov Chain Monte Carlo | 马尔可夫链蒙特卡罗 |  |
| MRF | Markov Random Field | 马尔可夫随机场 |  |
| PLSA | Probabilistic Latent Semantic Analysis | 概率隐含语义分析 |  |
| GBDT | Gradient Boost Decision Tree | 渐进梯度决策树 |  |
| FM | Factorization Machines | 因子分解机 |  |
| PITF | Pairwise Interaction Tensor Factorization | 配对互动张量分解 |  |
| ARM | Association Rule Mining | 关联规则挖掘 |  |
| ROC | Receiver Operating Characteristic | 受试者工作特征 |  |
| AUC | Area Under ROC Curve | ROC曲线下面积 |  |
| KNN | k-Nearest Neighbor | K最近邻 |  |
| GPR | Gaussian Process Regression | 高斯过程回归 |  |
| PGM | Probabilistic Graphical Model | 概率图模型 |  |
| PID | Proportional-Integral-Differential | 比例积分微分 |  |
| HMM | Hidden Markov Model | 隐马尔可夫模型 |  |
| PLSDA | Partial Least Squares Discriminant Analysis | 偏最小二乘判别分析 |  |
| ARIMA | Autoregressive Integrated Moving Average Model | 差分自回归移动平均模型 |  |
| IIR | Infinite Impulse Response | 无限脉冲响应 |  |
| FIR | Finite Impulse Response | 有限脉冲响应 |  |
| HNM | Hard Negative Mining | 硬负挖掘 |  |
| ACBM | Aho-Corasick Boyer-Moore |  | 一种字符串匹配算法 |
| LASSO | Least Absolute Shrinkage and Selection Operator | 最小绝对收缩和选择算子 |  |
| CRF | Conditional Random Field | 条件随机场 |  |
| MSE/MSD | Mean Squared Error/Mean Squared Deviation | 均方误差 |  |
| SMAPE | Symmetric Mean Absolute Percentage Error | 对称平均绝对百分比误差 |  |
| MAE | Mean Absolute Error | 平均绝对值误差 |  |
| MPE | Mean Percentage Error | 平均百分比误差 |  |
| DSSM | Deep Structured Semantic Model / Deep Semantic Similarity Model | 深度结构语义模型 |  |
| OOV | out-of-vocabulary |  | 无论多大的单词表都会遇到生词。 |
| DRL | Deep Reinforcement Learning | 深度强化学习 |  |
| DQN | Deep Q-Learning Network |  |  |
| EMD | Earth Mover's Distance | 推土机距离 |  |
| ED | Edit Distance | 编辑距离 |  |
| MLP | MultiLayer Perceptron | 多层感知器 |  |
| CTC | Connectionist Temporal Classification |  | 新词无常用译名 |
| RNN | Recurrent Neural Network | 循环神经网络 |  |
| RNN | Recursive Neural Network | 递归神经网络 | 已没落 |
| CNN | Convolutional Neural Network | 卷积神经网络 |  |
| NLP | Natural Language Processing | 自然语言处理 |  |
| HDP | Hierarchical Dirichlet Processes | 分层狄利克雷进程 |  |
| LSA/LSI | Latent Semantic Analysis/Latent Semantic Indexing | 隐式语义分析/隐式语义索引 |  |
| NER | Named Entity Recognition | 命名实体识别 |  |
| LSTM | Long Short-Term Memory | 长时记忆 |  |
| GRU | Gated Recurrent Unit |  |  |
| DRN | Deep Residual Network | 深度残差网络 |  |
| SVDD | Support Vector Domain Description | 支持向量域描述 |  |
| NMT | Neural Machine Translation | 神经机器翻译 |  |
| BLEU | Bilingual Evaluation Understudy | 双语评估替代 |  |
| NMT | Neural Machine Translation | 神经机器翻译 |  |
| SMT | Statistical Machine Translation | 统计机器翻译 |  |
| DMN | Dynamic Memory Networks | 动态记忆网络 |  |
| GAN | Generative Adversarial Networks | 生成对抗式网络 |  |
| SGNS | Skip-Gram Negative Sampling |  |  |
|  | Semi-supervised Learning | 半监督学习 |  |
|  | Transfer Learning | 迁移学习 |  |
|  | Text Classification | 文本分类 |  |
|  |  |  |  |
|  |  |  |  |

