---
layout: post
title:  机器学习（二十四）——机器学习的算法体系&相关术语表, 模型压缩
category: theory 
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

