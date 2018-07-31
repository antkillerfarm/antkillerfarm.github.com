---
layout: post
title:  机器学习（三十三）——机器学习的算法体系&相关术语表, simhash, 模仿学习
category: ML 
---

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
| LSTM | Long Short-Term Memory | 长短时记忆 |  |
| GRU | Gated Recurrent Unit |  |  |
| DRN | Deep Residual Network | 深度残差网络 |  |
| SVDD | Support Vector Domain Description | 支持向量域描述 |  |
| NMT | Neural Machine Translation | 神经机器翻译 |  |
| BLEU | Bilingual Evaluation Understudy | 双语评估替代 |  |
| SMT | Statistical Machine Translation | 统计机器翻译 |  |
| DMN | Dynamic Memory Networks | 动态记忆网络 |  |
| GAN | Generative Adversarial Networks | 生成对抗式网络 |  |
| SGNS | Skip-Gram Negative Sampling |  |  |
|  | Semi-supervised Learning | 半监督学习 |  |
|  | Transfer Learning | 迁移学习 |  |
|  | Text Classification | 文本分类 |  |
| MDP | Markov Decision Process | 马尔可夫决策过程 |  |
| GEMM | General Matrix Multiply | 通用矩阵乘法 |  |
| VIF | Variance Inflation Factor | 方差膨胀因子 |  |
|  |  |  |  |

参考：

https://mp.weixin.qq.com/s/6KRNawRE1i8de3ctgv6aVg

机器学习词汇表：纵览机器学习基本词汇与概念

https://mp.weixin.qq.com/s/AtImcIBaRIJVXpOP2hruBQ

Google发布机器学习术语表 (包括简体中文)

# simhash

simhash是一种能计算文档相似度的hash算法。

论文：

《Detecting Near-Duplicates for Web Crawling》

代码：

https://github.com/yanyiwu/simhash

类似的相似度Hash算法，还有用于图像搜索的aHash、pHash、dHash。

参考：

https://www.biaodianfu.com/simhash.html

文本内容相似度计算方法：simhash

http://www.cnblogs.com/chenwenbiao/archive/2011/09/12/2174139.html

simhash算法的原理

http://www.cnblogs.com/chenwenbiao/archive/2011/09/12/2174137.html

simhash与Google的网页去重

http://blog.jobbole.com/21928/

Simhash算法原理和网页查重应用

https://blog.csdn.net/luoweifu/article/details/8220992

看起来像它——图像搜索其实也不难

https://www.jianshu.com/p/193f0089b7a2

相似图片检测：感知哈希算法之dHash的Python实现

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

# ACBM算法

ACBM算法是在AC（Aho-Corasick）自动机（UNIX上的fgrep命令使用的就是AC算法）的基础之上，引入了BM（Boyer-Moore）算法的多模扩展，实现的高效的多模匹配。和AC自动机不同的是，ACBM算法不需要扫描目标文本串中的每一个字符，可以利用本次匹配不成功的信息，跳过尽可能多的字符，实现高效匹配。

>注： Alfred Vaino Aho，1941年生，加拿大计算机科学家。普林斯顿大学博士，长期供职于贝尔实验室，后为哥伦比亚大学教授。egrep和fgrep的发明人，AWK语言的联合发明人。著有《Principles of Compiler Design Compilers: Principles, Techniques, and Tools》。该书由于封面上有龙图案，又被称为“龙书”，是编译原理方面的权威书籍。2003年获IEEE John von Neumann Medal。

>Margaret John Corasick，贝尔实验室研究员。

>Robert Stephen Boyer，德克萨斯大学教授。

>J Strother Moore，德克萨斯大学教授。Boyer的好朋友，两人的绝大多数成就都是合作完成的。

参见：

http://blog.csdn.net/sealyao/article/details/6817944

ACBM算法

https://mp.weixin.qq.com/s/yVOgAuD9hEYAMdLyvae2VA

最长公共子序列与最长公共子串

https://mp.weixin.qq.com/s/8rP3fF9iVnplhkFmxuj6fw

一文读懂KMP算法

https://mp.weixin.qq.com/s/if7P0yq59DbBEjW15vfaQA

推荐一个高效算法wumanber：每秒680万匹配！

https://mp.weixin.qq.com/s/4m1O5ZHsZTRc-JuBF97_3w

字符串匹配的Boyer-Moore算法

# 高斯过程回归

从大的分类来说，机器学习的算法可分为两类：

1.定义一个模型，用训练数据训练模型的参数，然后用训练好的模型进行预测。这种方法的缺点在于，预测效果和模型与样本的匹配程度有关。比如对非线性样本采用线性模型，其预测效果通常不会太好。但是增加模型的复杂度，又会导致过拟合。

2.定义一个函数分布，赋予每一种可能的函数一个先验概率，可能性越大的函数，其先验概率越大。但是可能的函数往往为一个不可数集，即有无限个可能的函数，随之引入一个新的问题：如何在有限的时间内对这些无限的函数进行选择？一种有效解决方法就是高斯过程回归(Gaussian process regression，GPR)。

>注：Radford M. Neal，1956年生，加拿大科学家。多伦多大学博士（1995）和教授。贝叶斯神经网络的发明人。导师为Geoffrey Hinton。   
>个人主页：http://www.cs.toronto.edu/~radford/

>Danie G. Krige，1919～2013，南非矿业工程师和统计学家，威特沃特斯兰德大学教授。地理统计学早期的代表人物之一。

http://www.cnblogs.com/hxsyl/p/5229746.html

浅谈高斯过程回归

https://mqshen.gitbooks.io/prml/content/Chapter6/gaussian/gaussian_processes_regression.html

Gaussian Processes Regression

http://www.gaussianprocess.org/gpml/chapters/RW.pdf

Gaussian Processes for Machine Learning

http://people.cs.umass.edu/~wallach/talks/gp_intro.pdf

Introduction to Gaussian Process Regression

http://wenku.baidu.com/view/72f80113915f804d2b16c173.html

高斯过程回归方法综述

https://mp.weixin.qq.com/s/ZE_Chzlgy_7wl9E9q1ckOA

AlphaGo Zero用它来调参？“高斯过程”到底有何过人之处？

https://mp.weixin.qq.com/s/VzN02XW3yN2peqTD6q-4Cg

从数学到实现，全面回顾高斯过程中的函数最优化

