---
layout: post
title:  机器学习（二十）——
category: theory 
---

# loss function详解

**Mean Squared Error(MSE)/Mean Squared Deviation(MSD)**

$$\operatorname{MSE}=\frac{1}{n}\sum_{i=1}^n(\hat{Y_i} - Y_i)^2$$

**Symmetric Mean Absolute Percentage Error(SMAPE or sMAPE)**

MSE定义的误差，实际上是向量空间中的欧氏距离，这也可称为绝对误差。而有些情况下，可能相对误差（即百分比误差）更有意义些：

$$\text{SMAPE} = \frac 1 n \sum_{t=1}^n \frac{\left|F_t-A_t\right|}{(A_t+F_t)/2}$$

上式的问题在于$$A_t+F_t\le 0$$时，该值无意义。为了解决该问题，可用如下变种：

$$\text{SMAPE} = \frac{100\%}{n} \sum_{t=1}^n \frac{|F_t-A_t|}{|A_t|+|F_t|}$$

**Mean Absolute Error(MAE)**

$$\mathrm{MAE} = \frac{1}{n}\sum_{i=1}^n \left| f_i-y_i\right| =\frac{1}{n}\sum_{i=1}^n \left| e_i \right|$$

这个可以看作是MSE的1范数版本。

**Mean Percentage Error(MPE)**

$$\text{MPE} = \frac{100\%}{n}\sum_{t=1}^n \frac{a_t-f_t}{a_t}$$

# word2vec

word2vec是Google于2013年开源推出的一个用于获取word vector的工具包。作者是Tomas Mikolov。

>注：Tomas Mikolov，捷克布尔诺科技大学博士。先后在Google、Facebook担任研究员。

word2vec包中还有一个word2phrase的程序，这个程序可以根据统计信息由单词生成短语。考虑到中文的字和词之间的关系，实际上也可以用它来进行无先验数据的分词。

>注：NLP中的先验数据，最出名的当属分词词典。除此之外，还包括HMM的转移矩阵表等。

其一般方法为：

1.对原始语料按字切分，以空格分隔，相当于认为一个字就是一个词，即**单字成词**。

2.使用word2phrase**组字成词**。

`time ./word2phrase -train 1.txt -output 2.txt -threshold 100 -debug 2`

3.由于word2phrase最多只考虑到2-gram。因此，对于超过3个字以上的词语，需要迭代执行word2phrase。

我以金庸的小说为语料进行测试。从结果来看，这种方法对于人名、地名、武功招式名等专有名词，分词效果较好。但对于具有语法结构的句子，分词效果较差。比如“那人”其实是两个单字词，但却被word2phrase认为是一个双字词。

参考：

http://blog.csdn.net/itplus/article/details/37969519

https://github.com/tmikolov/word2vec

http://wei-li.cnblogs.com/p/word2vec.html

# HMM

![](/images/article/HMM.png)

![](/images/article/HMM_2.png)

![](/images/article/HMM_3.png)

和HMM模型相关的算法主要分为三类，分别解决三种问题：

1）**知道骰子有几种（隐含状态数量），每种骰子是什么（转换概率），根据掷骰子掷出的结果（可见状态链），我想知道每次掷出来的都是哪种骰子（隐含状态链）。**

这个问题呢，在语音识别领域呢，叫做解码问题。这个问题其实有两种解法，会给出两个不同的答案。每个答案都对，只不过这些答案的意义不一样。第一种解法求最大似然状态路径，说通俗点呢，就是我求一串骰子序列，这串骰子序列产生观测结果的概率最大。第二种解法呢，就不是求一组骰子序列了，而是求每次掷出的骰子分别是某种骰子的概率。比如说我看到结果后，我可以求得第一次掷骰子是D4的概率是0.5，D6的概率是0.3，D8的概率是0.2.第一种解法我会在下面说到，但是第二种解法我就不写在这里了，如果大家有兴趣，我们另开一个问题继续写吧。

2）**还是知道骰子有几种（隐含状态数量），每种骰子是什么（转换概率），根据掷骰子掷出的结果（可见状态链），我想知道掷出这个结果的概率。**

看似这个问题意义不大，因为你掷出来的结果很多时候都对应了一个比较大的概率。问这个问题的目的呢，其实是检测观察到的结果和已知的模型是否吻合。如果很多次结果都对应了比较小的概率，那么就说明我们已知的模型很有可能是错的，有人偷偷把我们的骰子給换了。

3）**知道骰子有几种（隐含状态数量），不知道每种骰子是什么（转换概率），观测到很多次掷骰子的结果（可见状态链），我想反推出每种骰子是什么（转换概率）。**

这个问题很重要，因为这是最常见的情况。很多时候我们只有可见结果，不知道HMM模型里的参数，我们需要从可见结果估计出这些参数，这是建模的一个必要步骤。

参考：

https://www.zhihu.com/question/20962240

如何用简单易懂的例子解释隐马尔可夫模型？

http://www.cnblogs.com/kaituorensheng/archive/2012/11/29/2795499.html

隐马尔可夫模型

https://www.zhihu.com/question/20136144

谁能通俗的讲解下viterbi算法吗？

# CRF

条件随机场(Conditional Random Field)由Lafferty等人于2001年提出，结合了最大熵模型和隐马尔可夫模型的特点，是一种无向图模型，近年来在分词、词性标注和命名实体识别等序列标注任务中取得了很好的效果。

# 概率图模型

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

## 贝叶斯网络

概率图模型的三要素：Graph：$$\mathcal{G}$$、Model：$$\mathcal{M}$$和Data：$$\mathcal{D}\equiv\{X^{(i)}_1,\dots,X^{(i)}_m\}^N_{i=1}$$。

它要解决的三大问题：

1.**表示**。如何获取或定义真实世界的不确定度？如何对领域知识/假设/约束编码？

2.**推断**。根据模型/数据，推断答案。

$$\text{e.g.}:P(x_i|\mathcal{D})$$

3.**学习**。根据数据确定哪个模型是正确的。

$$\text{e.g.}:\mathcal{M}=\arg\max_{\mathcal{M}\in M}F(\mathcal{D};\mathcal{M})$$

![](/images/article/PGM.png)

上图是PGM的一个示例。

# 机器学习语录

这里收录一些网上的只言片语式的心得，以区别于一般的教程。

>首先要考虑你的数据维度是线性相关的还是非线性相关的，数据是稀疏的还是稠密的，正例反例比例是多少，数据量是否充足。数据是否具有可分类性。是否需要降维。是否有噪音，是否有异常点等，然后去选择分类策略。通常包括数据采集，预处理，分类训练，预测，后处理等过程。

# 角度值的特征化

角度值是数据分析中常见的值，然而它不是线性的，比如0度和359度之间只相差1度，然而数值上却差了359度，因此无法将角度值直接代入线性回归等模型。因为后者的loss函数是用线性的欧氏距离定义的，角度显然不满足要求。

既然角度在一维上不是线性的，那么二维呢？没错，可以采用复数坐标(x,y)来表示角度，这样角度就是线性的了。

# 机器学习相关术语表

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
| ACBM | Aho-Corasick Boyer-Moore |  |  |
| LASSO | Least Absolute Shrinkage and Selection Operator | 最小绝对收缩和选择算子 |  |
| CRF | Conditional Random Field | 条件随机场 |  |
| MSE/MSD | Mean Squared Error/Mean Squared Deviation | 均方误差 |  |
| SMAPE | Symmetric Mean Absolute Percentage Error | 对称平均绝对百分比误差 |  |
| MAE | Mean Absolute Error | 平均绝对值误差 |  |
| MPE | Mean Percentage Error | 平均百分比误差 |  |

