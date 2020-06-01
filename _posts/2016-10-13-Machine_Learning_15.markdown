---
layout: post
title:  机器学习（十五）——机器学习中的矩阵方法（3）病态矩阵
category: ML 
---

## 正定矩阵

positive definite matrix的定义：

一个n阶的实对称矩阵M是正定的的条件是当且仅当对于所有的非零实系数向量z，都有$$z^TMz>0$$。

正定矩阵A的性质：

1.正定矩阵的任一主子矩阵也是正定矩阵。

2.A的特征值和各阶顺序主子式全为正。

3.若A为n阶正定矩阵，则A为n阶可逆矩阵。

类似的还可以定义负定矩阵、半正定矩阵（非负定矩阵）。

参考：

https://zhuanlan.zhihu.com/p/44860862

浅谈“正定矩阵”和“半正定矩阵”

## 向量的范数

范数（norm，也叫模）的定义比较抽象，这里我们使用闵可夫斯基距离，进行一个示意性的介绍。

Minkowski distance的定义：

$$d(x,y)=\sqrt[\lambda]{\sum_{i=1}^{n}\mid x_i-y_i\mid^{\lambda}}$$

>Hermann Minkowski（1864-1909），德国数学家，哥廷根大学数学教授，爱因斯坦的老师。

这里的$$\lambda$$就是范数。

范数可用符号$$\|x\|_\lambda$$表示。常用的有：

$$\|x\|_1=\mid x_1\mid +\dots+\mid x_n\mid $$

$$\|x\|_2=\sqrt{x_1^2+\dots+x_n^2}$$

$$\|x\|_\infty=max(\mid x_1\mid ,\dots,\mid x_n\mid )$$

显然，当$$\lambda=2$$时，该距离为Euclid Distance。

![](/images/img2/Euclid.png)

当$$\lambda=1$$时，也被称为CityBlock Distance或Manhattan Distance（曼哈顿距离，以纽约曼哈顿地区的街道形状得名）。

![](/images/img2/Manhattan.png)

当$$\lambda=\infty$$时，叫做Chebyshev distance。

![](/images/img2/Chebyshev.png)

>Pafnuty Lvovich Chebyshev，1821～1894，俄罗斯数学家，莫斯科大学博士，圣彼得堡大学教授。俄罗斯数学的奠基人，他创建的圣彼得堡学派，是20世纪俄罗斯最主要的数学流派。

这里不做解释的给出如下示意图：

![](/images/article/lp_ball.png)

其中，L0范数表示向量中非0元素的个数。上图中的图形被称为$$l_p$$ ball。表征在同一范数条件下，具有相同距离的点的集合。

范数满足如下不等式：

$$\|A+B\|\le \|A\|+\|B\|(三角不等式)$$

向量范数推广可得到矩阵范数。某些矩阵范数满足如下公式：

$$\|A\cdot B\|\le \|A\|\cdot\|B\|$$

这种范数被称为相容范数。

>注：矩阵范数要比向量范数复杂的多，还包含一些不可以由向量范数来诱导的范数，如Frobenius范数。而且只有极少数矩阵范数，可由简单表达式来表达。这里篇幅有限，不再赘述。

>Ferdinand Georg Frobenius，1849～1917，德国数学家，哥廷根大学博士（1870）,University of Berlin和ETH Zurich教授。他在椭圆函数、微分方程、数论和群论等领域有杰出贡献。矩阵的秩就是他提出来的。

## 病态矩阵

现在有线性系统$$Ax = b$$：

$$\begin{bmatrix} 400 & -201 \\-800 & 401 \end{bmatrix}\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}=\begin{bmatrix} 200 \\ -200 \end{bmatrix}$$

很容易得到解为：$$x_1=-100,x_2=-200$$。如果在样本采集时存在一个微小的误差，比如，将 A矩阵的系数400改变成401：

$$\begin{bmatrix} 401 & -201 \\-800 & 401 \end{bmatrix}\begin{bmatrix} x_1 \\ x_2 \end{bmatrix}=\begin{bmatrix} 200 \\ -200 \end{bmatrix}$$

则得到一个截然不同的解：$$x_1=40000,x_2=79800$$。

当解集x对A和b的系数高度敏感，那么这样的方程组就是病态的 (ill-conditioned/ill-posed)。

从上例的情况来看，矩阵的行向量$$\begin{bmatrix} 400 & -201\end{bmatrix}$$和$$\begin{bmatrix} -800 & 401\end{bmatrix}$$实际上是过于线性相关了，从而导致矩阵已经接近奇异矩阵（near singular matrix）。

病态矩阵实际上就是奇异矩阵和近奇异矩阵的另一个说法。

参见：

http://www.cnblogs.com/daniel-D/p/3219802.html

病态矩阵与条件数

## 矩阵的条件数

我们首先假设向量b受到扰动，导致解集x产生偏差，即：

$$A(x+\Delta x)=b+\Delta b$$

也就是：

$$A\Delta x=\Delta b$$

因此，由矩阵相容性可得：

$$\|\Delta x\|\le \|A^{-1}\|\cdot\|\Delta b\|$$

同时，由于：

$$\|A\|\cdot\|x\|\ge\|b\|$$

所以：

$$\frac{\|\Delta x\|}{\|A\|\cdot\|x\|}\le \frac{\|A^{-1}\|\cdot\|\Delta b\|}{\|b\|}$$

即：

$$\frac{\|\Delta x\|}{\|x\|}\le \frac{\|A\|\cdot\|A^{-1}\|\cdot\|\Delta b\|}{\|b\|}$$

我们定义矩阵的条件数$$K(A)=\|A\|\cdot\|A^{-1}\|$$，则上式可写为：

$$\frac{\|\Delta x\|}{\|x\|}\le K(A)\frac{\|\Delta b\|}{\|b\|}$$

同样的，我们针对A的扰动，所导致的x的偏差，也可得到类似的结论：

$$\frac{\|\Delta x\|}{\|x+\Delta x\|}\le K(A)\frac{\|\Delta A\|}{\|A\|}$$

可见，矩阵的条件数是描述输入扰动对输出结果影响的量度。显然，条件数越大，矩阵越病态。

然而这个定义，在病态矩阵的条件下，并不能直接用于数值计算。因为浮点数所引入的微小的量化误差，也会导致求逆结果的很大误差。所以通常情况下，一般使用矩阵的特征值或奇异值来计算条件数。

假设A是2阶方阵，它有两个单位特征向量$$x_1,x_2$$和相应的特征值$$\lambda_1,\lambda_2$$。

由之前的讨论可知，$$x_1,x_2$$是相互正交的。因此，向量b能够被$$x_1,x_2$$的线性组合所表示，即：

$$b=mx_1+nx_2=\frac{m}{\lambda_1}\lambda_1x_1+\frac{n}{\lambda_2}\lambda_2x_2=A(\frac{m}{\lambda_1}x_1+\frac{n}{\lambda_2}x_2)$$

从这里可以看出，b在$$x_1,x_2$$上的扰动，所带来的影响，和特征值$$\lambda_1,\lambda_2$$有很密切的关系。奇异值实际上也有类似的特点。

因此，一般情况下，条件数也可以由最大奇异值与最小奇异值之间的比值，或者最大特征值和最小特征值之间的比值来表示。这里的最大和最小，都是针对绝对值而言的。

参见：

https://en.wikipedia.org/wiki/Condition_number

## 矩阵规则化

病态矩阵处理方法有很多，这里只介绍矩阵规则化（regularization）方法。

机器学习领域，经常用到各种损失函数（loss function）。这里我们用：

$$\min_f \sum_{i=1}^nV(f(\hat x_i),\hat y_i)$$

表示损失函数。

当样本数远小于特征向量维数时，损失函数所表示的矩阵是一个稀疏矩阵，而且往往还是一个病态矩阵。这时，就需要引入规则化因子用以改善损失函数的稳定性：

$$\min_f \sum_{i=1}^nV(f(\hat x_i),\hat y_i)+\lambda R(f)$$

其中的$$\lambda$$表示规则化因子的权重。

>注：稀疏矩阵并不一定是病态矩阵，比如单位阵就不是病态的。但是从系统论的角度，高维空间中样本量的稀疏，的确会带来很大的不确定性。可类比下围棋，棋子过于稀疏的地方，只能称作势力范围，而不能称作实地。

函数V（又叫做Fit measure）和R（又叫做Entropy measure），在不同的算法中，有不同的取值。

比如，在Ridge regression问题中：

$$\text{Fit measure}:\|Y-X\beta\|_2,\text{Entropy measure}:\|\beta\|_2$$

Ridge regression问题中规则化方法，又被称为$$L_2$$ regularization，或Tikhonov regularization。

>注：Andrey Nikolayevich Tikhonov，1906~1993，苏联数学家和地球物理学家。大地电磁学的发明人之一。苏联科学院院士。著有《Solutions of Ill-posed problems》一书。

更多的V和R取值参见：

https://en.wikipedia.org/wiki/Regularization_(mathematics)

从形式上来看，对比之前提到的拉格朗日函数，我们可以发现规则化因子，实际上就是给损失函数增加了一个约束条件。它的好处是增加了解向量的稳定度，缺点是增加了数值解和真实解之间的误差。

为了更便于理解规则化，这里以二维向量空间为例，给出了规则化因子对损失函数的约束效应。

![](/images/article/L1_vs_L2.png)

上图中的圆圈是损失函数的等高线，坐标原点是规则化因子的约束中心，左图的方形和右图的圆形是$$l_p$$ ball。图中的黑点是等高线和$$l_p$$ ball的交点，实际上也就是这个带约束的优化问题的解。

可以看出$$L_1$$ regularization的解一般出现在坐标轴上，因而其他坐标上的值就是0，因此，$$L_1$$ regularization会导致矩阵的稀疏。

$$L_1$$ regularization又被称为Lasso（least absolute shrinkage and selection operator） regression。

从贝叶斯统计的角度来看，规则化相当于增加了一个先验分布，比如$$L_2$$ regularization对应的贝叶斯先验分布是Gaussian distribution，而$$L_1$$ regularization对应的贝叶斯先验分布是Laplace distribution。

除此之外，还有混合使用规则化的情况，比如：

$$\min_f \sum_{i=1}^nV(f(\hat x_i),\hat y_i)+\lambda \|\beta\|_1 + \eta  \|\beta\|_2$$

这种方法也被称为弹性网络回归（ElasticNet Regression）。

规则化同时也提供了一种衡量特征重要度的方法：**loss函数的值，如果显著小于规则项，则说明该特征不太重要。**

参考：

https://en.wikipedia.org/wiki/Tikhonov_regularization

http://www.mit.edu/~cuongng/Site/Publication_files/Tikhonov06.pdf

http://blog.csdn.net/zouxy09/article/details/24971995

机器学习中的范数规则化之（一）L0、L1与L2范数

https://mp.weixin.qq.com/s/pZko9gM5sbFhMl8P8TCFww

机器学习损失函数、L1-L2正则化的前世今生

https://mp.weixin.qq.com/s/pNG8u8V7zp6fLFF9TomEug

史上最全面的正则化技术总结与分析

https://mp.weixin.qq.com/s/PMisvVy4EwEF-5xEY5LrwA

从损失函数的角度详解常见机器学习算法

https://mp.weixin.qq.com/s/MRabAUZrfgD2t2GhnLI43Q

开发者必读：计算机科学中的线性代数

https://mp.weixin.qq.com/s/ctLe1UbvWqBJ8jh-ppU3rA

机器学习中的五种回归模型及其优缺点

https://mp.weixin.qq.com/s/zzCRiSSyfTAFYSGMnr8zfg

机器学习中L1和L2正则化的直观解释

https://mp.weixin.qq.com/s/xwYldlEjJ9Co9uo8o0mlKQ

深度学习之DNN的多种正则化方式

https://mp.weixin.qq.com/s/-axtm6ZBm8yYneiA3mvQrw

SIGIR 2018大会最佳短论文：利用对抗学习的跨域正则化

https://mp.weixin.qq.com/s/FtWA1rff13e7_FM0lJKCVg

Petuum提出新型正则化方法：非重叠促进型变量选择

https://zhuanlan.zhihu.com/p/50142573

L1正则化引起稀疏解的多种解释

https://blog.csdn.net/daunxx/article/details/51596877

Lasso Regression

https://mp.weixin.qq.com/s/kFEwvgBC4-Y05vL3lmRbXQ

正则的一些intuition，一分钟发明新正则

https://mp.weixin.qq.com/s/oZZGBZLRoJVsY7M-CP5MRg

矩阵L2,1/L2,p范数扫盲

# Krylov subspace

Krylov subspace是一类针对大矩阵的近似计算的方法，由Aleksey Nikolaevich Krylov于1931年提出。

>Aleksey Nikolaevich Krylov，1863～1945，俄罗斯海军工程师、数学家、作家。

参考：

https://blog.csdn.net/lizhengjiang/article/details/18794275

krylov子空间迭代法

https://www.zhihu.com/question/23309010

如何使用Krylov方法求解矩阵的运算尤其是逆？
