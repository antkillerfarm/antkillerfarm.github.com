---
layout: post
title:  机器学习（二十一）——Loss function详解
category: ML 
---

## 关联规则评价（续）

### 确信度

Conviction的定义如下：

$$\mathrm{conv}(X\Rightarrow Y) =\frac{ 1 - \mathrm{supp}(Y) }{ 1 - \mathrm{conf}(X\Rightarrow Y)}$$

它的值越大，表明X、Y的独立性越小。

### 卡方系数

卡方系数是与卡方分布有关的一个指标。参见：

https://en.wikipedia.org/wiki/Chi-squared_distribution

$$\chi^2 = \sum_{i=1}^n \frac{(O_i - E_i)^2}{E_i}$$

>注：上式最早是Pearson给出的。

公式中的$$O_i$$表示数据的实际值，$$E_i$$表示期望值，不理解没关系，我们看一个例子就明白了。

| 表2 | 买游戏 | 不买游戏 | 行总计 |
|:--:|:--|:--:|:--|
| 买影片 | 4000(4500) | 3500(3000) | 7500 |
| 不买影片 | 2000(1500) | 500(1000) | 2500 |
| 列总计 | 6000 | 4000 | 10000 |

表2的括号中表示的是期望值。以第1行第1列的4500为例，其计算方法为：7500×6000/10000。

经计算可得表2的卡方系数为555.6。基于置信水平和自由度$$(r-1)*(c-1)=(行数-1)*(列数-1)=1$$，查表得到自信度为(1-0.001)的值为6.63。

555.6>6.63，因此拒绝A、B独立的假设，即认为A、B是相关的，而$$E(买影片，买游戏)=4500>4000$$,因此认为A、B呈负相关。

### 全自信度

$$all\_confidence(A,B)=\frac{P(A\cap B)}{max\{P(A),P(B)\}}\\=min\{P(B|A),P(A|B)\}=min\{confidence(A\to B),confidence(B\to A)\}$$

### 最大自信度

$$max\_confidence(A,B)=max\{confidence(A\to B),confidence(B\to A)\}$$

### Kulc

$$kulc(A,B)=\frac{confidence(A\to B)+confidence(B\to A)}{2}$$

### cosine距离

$$cosine(A,B)=\frac{P(A\cap B)}{sqrt(P(A)*P(B))}=sqrt(P(A|B)*P(B|A))\\=sqrt(confidence(A\to B)*confidence(B\to A))$$

### Leverage

$$Leverage(A,B) = P(A\cap B)-P(A)P(B)$$

### 不平衡因子

imbalance ratio的定义：

$$IR(A,B)=\frac{|support(A)-support(B)|}{(support(A)+support(B)-support(A\cap B))}$$

全自信度、最大自信度、Kulc、cosine，Leverage是不受空值影响的，这在处理大数据集是优势更加明显，因为大数据中空记录更多，根据分析我们推荐使用kulc准则和不平衡因子结合的方法。

参考：

http://www.cnblogs.com/fengfenggirl/p/associate_measure.html

关联规则评价

https://mp.weixin.qq.com/s/s1Snb4XnIQk1DcK3nESilw

PrefixSpan算法原理详解

# Loss function详解

![](/images/img2/loss.png)

## Mean Squared Error(MSE)/Mean Squared Deviation(MSD)

$$\operatorname{MSE}=\frac{1}{n}\sum_{i=1}^n(\hat{Y_i} - Y_i)^2$$

## Symmetric Mean Absolute Percentage Error(SMAPE or sMAPE)

MSE定义的误差，实际上是向量空间中的欧氏距离，这也可称为绝对误差。而有些情况下，可能相对误差（即百分比误差）更有意义些：

$$\text{SMAPE} = \frac 1 n \sum_{t=1}^n \frac{\left|F_t-A_t\right|}{(A_t+F_t)/2}$$

上式的问题在于$$A_t+F_t\le 0$$时，该值无意义。为了解决该问题，可用如下变种：

$$\text{SMAPE} = \frac{100\%}{n} \sum_{t=1}^n \frac{|F_t-A_t|}{|A_t|+|F_t|}$$

## Mean Absolute Error(MAE)

$$\mathrm{MAE} = \frac{1}{n}\sum_{i=1}^n \left| f_i-y_i\right| =\frac{1}{n}\sum_{i=1}^n \left| e_i \right|$$

这个可以看作是MSE的1范数版本。

## Mean Percentage Error(MPE)

$$\text{MPE} = \frac{100\%}{n}\sum_{t=1}^n \frac{a_t-f_t}{a_t}$$

不同的loss函数有不同的用途，比如softmax一般用Cross Entropy作为loss函数。如下图所示：

![](/images/article/cross_vs_mse.png)

## loss function比较

![](/images/article/loss_function.png)

这里m代表了置信度，越靠近右边置信度越高。

其中蓝色的阶跃函数又被称为Gold Standard，黄金标准，因为这是最准确无误的分类器loss function了。分对了loss为0，分错了loss为1，且loss不随到分界面的距离的增加而增加，也就是说这个分类器非常鲁棒。但可惜的是，它不连续，求解这个问题是NP-hard的，所以才有了各种我们熟知的分类器。

其中红色线条就是SVM了，由于它在m=1处有个不可导的转折点，右边都是0，所以分类正确的置信度超过一定的数之后，对分界面的确定就没有一点贡献了。

《机器学习（五）》中提到的SVM软间隔，其所使用的loss function，又被称为Hinge loss函数：

$$l_{hinge}(z)=\max(0,1-z)$$

除此之外，exponential loss函数：

$$l_{exp}(z)=\exp(-z)$$

和logistic loss函数：

$$l_{log}(z)=\log(1+\exp(-z))$$

也是较常用的SVM loss function。

黄色线条是Logistic Regression的损失函数，与SVM不同的是，它非常平滑，但本质跟SVM差别不大。

绿色线条是boost算法使用的损失函数。

黑色线条是ELM（Extreme learning machine）算法的损失函数。它的优点是有解析解，不必使用梯度下降等迭代方法，可直接计算得到最优解。但缺点是随着分类的置信度的增加，loss不降反升，因此，最终准确率有限。此外，解析算法相比迭代算法，对于大数据的适应较差，这也是该方法的局限所在。

参见：

https://www.zhihu.com/question/28810567

Extreme learning machine(ELM)到底怎么样，有没有做的前途？

## Softmax详解

首先给出Softmax function的定义:

$$y_c=\zeta(\textbf{z})_c = \dfrac{e^{z_c}}{\sum_{d=1}^C{e^{z_d}}} \text{  for } c=1, \dots, C$$

从中可以很容易的发现，如果$$z_c$$的值过大，朴素的直接计算会上溢出或下溢出。

解决办法：

$$z_c\leftarrow z_c-a,a=\max\{z_1,\dots,z_C\}$$

证明：

$$\zeta(\textbf{z-a})_c = \dfrac{e^{z_c}\cdot e^{-a}}{\sum_{d=1}^C{e^{z_d}\cdot e^{-a}}} = \dfrac{e^{z_c}}{\sum_{d=1}^C{e^{z_d}}} = \zeta(\textbf{z})_c$$

Softmax的损失函数是cross entropy loss function：

$$\xi(X, Y) = \sum_{i=1}^n \xi(\textbf{t}_i, \textbf{y}_i) = - \sum_{i=1}^n \sum_{i=c}^C t_{ic} \cdot \log(y_{ic})$$

Softmax的反向传播算法：

$$\begin{align}
\dfrac{\partial\xi}{\partial z_i} &= - \sum_{j=1}^C \dfrac{\partial t_j \log(y_j)}{\partial z_i} \\
&= - \sum_{j=1}^C t_j \dfrac{\partial \log(y_j)}{\partial z_i} \\
&= - \sum_{j=1}^C t_j \dfrac{1}{y_j} \dfrac{\partial y_j}{\partial z_i} \\
&= - \dfrac{t_i}{y_i} \dfrac{\partial y_i}{\partial z_i} - \sum_{j \neq i}^C \dfrac{t_j}{y_j} \dfrac{\partial y_j}{\partial z_i} \\
&= - \dfrac{t_i}{y_i} y_i(1-y_i) - \sum_{j \neq i}^{C} \dfrac{t_j}{y_j}(-y_jy_j) \\
&= -t_i + t_iy_i + \sum_{j \neq i}^{C} t_jy_i \\
&= -t_i + \sum_{j=1}^C t_jy_i \\
&= -t_i + y_i \sum_{j=1}^C t_j \\
&= y_i - t_i
\end{align}$$

参考：

https://mp.weixin.qq.com/s/2xYgaeLlmmUfxiHCbCa8dQ

softmax函数计算时候为什么要减去一个最大值？

http://shuokay.com/2016/07/20/softmax-loss/

Softmax输出及其反向传播推导

https://mp.weixin.qq.com/s/HTIgKm8HuZZ_-lIQ3nIFhQ

浅入深出之大话SoftMax

https://mp.weixin.qq.com/s/XBK7T1P7z3rm3o-3BDNeOA

三分钟带你对Softmax划重点

https://mp.weixin.qq.com/s/vhvXsSsEHPVjJGqtCOOwLw

Softmax和交叉熵的深度解析和Python实现

https://mp.weixin.qq.com/s/rw-7-4_07TJ48Mq__HnYEg

用Mixtape代替softmax，CMU提出新方法兼顾表达性和高效性

## Softmax loss

通常我们使用的Softmax loss，实际上是由softmax和交叉熵(cross-entropy loss)loss组合而成，所以全称是softmax with cross-entropy loss。

$$l(y,z)=-\sum_{k=0}^C y_k\log (f(z_k))$$

$$f(z_k)=e^{z_k}/(\sum_j e^{z_j})$$

原始的softmax loss非常优雅，简洁，被广泛用于分类问题。它的特点就是优化类间的距离非常棒，但是优化类内距离时比较弱。

## Weighted softmax loss

假如有一个二分类问题，两类的样本数目差距非常之大。比如图像任务中的边缘检测问题，它可以看作是一个逐像素的分类问题。此时两类的样本数目差距非常之大，明显边缘像素的重要性是比非边缘像素大的，此时可以针对性的对样本进行加权。

$$l(y,z)=-\sum_{k=0}^C w_ky_k\log (f(z_k))$$

## Triplet Loss

Triplet loss通常是在个体级别的细粒度识别上使用，传统的分类是花鸟狗的大类别的识别，但是有些需求是要精确到个体级别，比如精确到哪个人的人脸识别，所以triplet loss的最主要应用也就是face identification，person re-identification，vehicle re-identification的各种identification识别问题上。

当然你可以把每个人当做一个类别来进行分类训练，但是往往最后会造成softmax的维数远大于feature的维数。

论文：

《Deep feature learning with relative distance comparison for person re-identification》

![](/images/img2/Triplet_Loss.png)

如上图所示，triplet是一个三元组，这个三元组是这样构成的：从训练数据集中随机选一个样本，该样本称为Anchor，然后再随机选取一个和Anchor(记为$$x^a$$)属于同一类的样本和不同类的样本,这两个样本对应的称为Positive(记为$$x^p$$)和Negative(记为$$x^n$$)，由此构成一个（Anchor，Positive，Negative）三元组。

针对每个样本$$x_i$$，训练一个参数共享或者不共享的网络，得到三个元素的特征表达，分别记为：$$f(x_i^a), f(x_i^p), f(x_i^n)$$。

**triplet loss的目的就是通过学习（即上图中的Learning），让$$x^a$$和$$x^p$$特征表达之间的距离尽可能小，而$$x^a$$和$$x^n$$的特征表达之间的距离尽可能大。**

它的公式化的表示就是：

$$\|f(x_i^a)-f(x_i^p)\|_2^2 + \alpha < \|f(x_i^a)-f(x_i^n)\|_2^2$$

其中，$$\alpha$$表示两个距离之间的间隔。因此，对应的目标函数也就很清楚了：

$$\sum_i^N\left[\|f(x_i^a)-f(x_i^p)\|_2^2 - \|f(x_i^a)-f(x_i^n)\|_2^2 + \alpha \right]_+$$

这里距离用欧式距离度量，+表示[]内的值大于零的时候，取该值为损失，小于零的时候，损失为零。

>需要注意的是Triplet Loss以及后面介绍的各种改进版softmax，其收敛速度不如softmax，因此，先用softmax训练几轮，再改用这些loss，也是常用的调参技巧。

参考：

https://blog.csdn.net/u010167269/article/details/52027378

Triplet Loss、Coupled Cluster Loss探究

https://blog.csdn.net/tangwei2014/article/details/46788025

triplet loss原理以及梯度推导

https://www.zhihu.com/question/62486208

triplet loss在深度学习中主要应用在什么地方？有什么明显的优势？

https://mp.weixin.qq.com/s/XB9VsW3NRwHua6AdRL3n8w

Lossless Triplet Loss:一种高效的Siamese网络损失函数

https://gehaocool.github.io/2018/03/20/Angular-Margin-%E5%9C%A8%E4%BA%BA%E8%84%B8%E8%AF%86%E5%88%AB%E4%B8%AD%E7%9A%84%E5%BA%94%E7%94%A8/

Angular Margin在人脸识别中的应用

https://mp.weixin.qq.com/s/SqaR_7gwJpUNPM7g4IHaYw

深度人脸识别中不同损失函数的性能对比
