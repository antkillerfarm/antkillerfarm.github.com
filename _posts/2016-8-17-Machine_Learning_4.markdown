---
layout: post
title:  机器学习（四）——SVM（1）
category: ML 
---

## 文本分类的事件模型

文本分类的事件模型有两类：

1.多值伯努利事件模型（multi-variate Bernoulli event model）。

在这个模型中，我们首先随机选定了邮件的类型（垃圾或者普通邮件，也就是$$p(y)$$），然后翻阅词典，随机决定一个词是否要在邮件中出现，出现则$$x_i$$标示为1，否则标示为0。然后将出现的词，组成一封邮件。一个词是否出现的概率为$$p(x_i\mid y)$$，整封邮件的概率为$$p(y)\prod_{i=1}^np(x_i\mid y)$$。

2.多项事件模型（multinomial event model）。

令$$x_i$$表示邮件的第i个词在字典中的位置，那么$$x_i$$的取值范围为$${1,2,...\lvert V\rvert}$$，$$\lvert V\rvert$$表示字典中单词的数目。这样一封邮件可以表示成$$(x_1,x_2,\dots,x_n)$$，这里n为邮件包含的单词个数，显然每个邮件的n值一般是不同的。

这相当于重复投掷$$\lvert V\rvert$$面的骰子，将观察值记录下来就形成了一封邮件。每个面的概率服从$$p(x_i\mid y)$$，而且每次试验条件独立。这样我们得到的邮件概率是$$p(y)\prod_{i=1}^np(x_i\mid y)$$。

需要注意的是，上面两个事件模型的概率公式虽然一致，但含义却有很大差异，不要弄混了。

# 支持向量机

支持向量机（SVM，Support Vector Machines）是目前最好的监督学习算法。它由Vladimir Naumovich Vapnik与Alexey Ya. Chervonenkis于1963年提出。

>注：Vladimir Naumovich Vapnik，1936年生，乌兹别克国立大学学士，莫斯科控制科学学院博士，后成为该学院计算机科学研究部门负责人。1990年代末移民美国，先后供职于AT&T Bell、NEC、Facebook，伦敦大学和哥伦比亚大学教授。

>Alexey Yakovlevich Chervonenkis，1938～2014，俄罗斯数学家。俄罗斯科学院院士，伦敦大学教授。

我们首先来看一幅图：

![](/images/article/SVM.png)

上图中的x和o分别代表y的两种不同取值（1和0）。前面提到逻辑回归的预测函数为$$h_\theta(x)=g(\theta^Tx)$$，那么图中直线的解析方程就是$$\theta^Tx=0$$。这条直线被称作分割超平面（Separating Hyperplane），也就是预测值的分界线。

图中的三个点A、B、C，虽然都在线的上方，然而从直觉来说，A离分界线最远，我们对它的值为1的信心也最足。反观C点，由于离分界线比较近，我们对其值的预测，实际上并没有多大把握。因为这种情况下，样本集的少许调整，就会导致分界线的移动，从而导致预测值的改变。B点的情况介于A和C之间。

和逻辑回归不同，SVM更关心靠近分界线的点，让他们尽可能地远离分界线，而不是在所有点上都达到最优。

为了便于后续讨论，我们对$$h_\theta(x)$$进行改写：$$h_{w,b}(x)=g(w^Tx+b)$$。其中b为常数项，即$$x_0$$的系数项，w为其余的$$x_i$$的系数项。

同时对$$g(z)$$的定义也调整为：

$$g(z)=\begin{cases}
1, & z\ge 0 \\
-1, & z<0 \\
\end{cases}$$

这个定义的好处在于$$g(z)$$的取值，只和z的符号相关，而和其绝对值的大小无关。

## 函数边距和几何边距

为了定义样本点和边界线的距离，我们引入函数边距（functional margin）和几何边距（geometric margins）的定义。

函数边距的定义如下：

$$\hat\gamma=\min_{i=1,\dots,m}\hat\gamma^{(i)},\hat\gamma^{(i)}=y^{(i)}(w^Tx^{(i)}+b)$$

![](/images/article/SVM_2.png)

几何边距的几何意义如上图所示。w是分界线的法向量，A的坐标是$$x^{(i)}$$，它到分界线的距离是$$\gamma^{(i)}$$，根据解析几何知识可知，A到分界线的垂足B的坐标为$$x^{(i)}-\gamma^{(i)}\frac{w}{\|w\|}$$。将其代入分界线方程$$w^Tx+b=0$$，可得：

$$w^T\left(x^{(i)}-\gamma^{(i)}\frac{w}{\|w\|}\right)+b=0\Rightarrow w^Tx^{(i)}-\gamma^{(i)}\frac{w^Tw}{\|w\|}+b=0$$

$$\Rightarrow w^Tx^{(i)}+b=\gamma^{(i)}\frac{\|w\|^2}{\|w\|}\Rightarrow \gamma^{(i)}=\frac{w^Tx^{(i)}+b}{\|w\|}=\left(\frac{w}{\|w\|}\right)^Tx^{(i)}+\frac{b}{\|w\|}$$

这是A在边界线上方时的情况，扩展到整个坐标系的话，上式可改为：

$$\gamma^{(i)}=y^{(i)}\left(\left(\frac{w}{\|w\|}\right)^Tx^{(i)}+\frac{b}{\|w\|}\right)$$

同理，可得几何边距的定义为：

$$\gamma=\min_{i=1,\dots,m}\gamma^{(i)}$$

从函数边距和几何边距的定义可以看出，如果等比例缩放w和b的话，其几何边距不变，且当$$\|w\|=1$$时，函数边距和几何边距相等。

## 最优边距分类

SVM算法的本质，就是求能使几何边距最大的w和b的取值，用数学语言描述就是求解问题：

$$\begin{align}
&\operatorname{max}_{\gamma,w,b}& & \gamma\\
&\operatorname{s.t.}& & y^{(i)}(w^Tx^{(i)}+b)\ge\gamma,i=1,\dots,m\\
& & & \|w\|=1
\end{align}$$

>注：上式中的$$\operatorname{s.t.}$$是subject to的缩写，表示极值问题的约束条件。

这个问题等价于:

$$\begin{align}
&\operatorname{max}_{\gamma,w,b}& & \frac{\hat\gamma}{\|w\|}\\
&\operatorname{s.t.}& & y^{(i)}(w^Tx^{(i)}+b)\ge\hat\gamma,i=1,\dots,m
\end{align}$$

如果能通过比例变换使$$\hat\gamma=1$$，则问题化解为：

$$\begin{align}
&\operatorname{min}_{\gamma,w,b}& & \frac{1}{2}\|w\|^2\\
&\operatorname{s.t.}& & y^{(i)}(w^Tx^{(i)}+b)\ge 1,i=1,\dots,m
\end{align}$$

这个问题实际上就是数学上的QP（Quadratic Programming）问题，采用这种方案的分类被称为最优边距分类（optimal margin classifier）。

## 拉格朗日对偶

QP问题是有约束条件的优化问题（constrained optimization problem）的一种，下面让我们讨论一下解决这类问题的通用方法。

假设我们求解如下问题：

$$\begin{align}
&\operatorname{min}_w& & f(w)\\
&\operatorname{s.t.}& & g_i(w)\le 0,i=1,\dots,k\\
& & & h_i(w)=0,i=1,\dots,l
\end{align}$$

这里将约束条件分为两类：

1.$$h_i(w)=0$$代表的是约束条件为等式的情况。

2.$$g_i(w)\le 0$$代表的是约束条件为不等式的情况。

上述约束优化问题也被称为原始优化问题（primal optimization problem）。为了求解这个问题，我们定义广义拉格朗日（generalized Lagrangian）函数：

$$\mathcal{L}(w,\alpha,\beta)=f(w)+\sum_{i=1}^k\alpha_ig_i(w)+\sum_{i=1}^l\beta_ih_i(w)$$

利用这个函数可以将约束优化问题转化为无约束优化问题。其中的$$\alpha_i$$、$$\beta_i$$也被称作拉格朗日乘子（Lagrange multiplier）。

$$\theta_\mathcal{P}(w)=\underset{\alpha,\beta:\alpha_i\ge 0}{\operatorname{max}}\mathcal{L}(w,\alpha,\beta)=\underset{\alpha,\beta:\alpha_i\ge 0}{\operatorname{max}}f(w)+\sum_{i=1}^k\alpha_ig_i(w)+\sum_{i=1}^l\beta_ih_i(w)$$

其中，$$\mathcal{P}$$代表原始优化问题，$$\underset{\alpha,\beta:\alpha_i\ge 0}{\operatorname{max}}$$表示在$$\alpha,\beta$$变化，而其他变量不变的情况下，求最大值。

如果w不满足约束，也就是$$g_i(w)>0$$或$$h_i(w)\ne 0$$。这时由于$$\mathcal{L}$$函数是无约束函数，$$\alpha_i$$、$$\beta_i$$可以任意取值，因此$$\sum_{i=1}^k\alpha_ig_i(w)$$或$$\sum_{i=1}^l\beta_ih_i(w)$$并没有极值，也就是说$$\theta_\mathcal{P}(w)=\infty$$。

反之,如果w满足约束，则$$\sum_{i=1}^k\alpha_ig_i(w)$$和$$\sum_{i=1}^l\beta_ih_i(w)$$都为0，因此$$\theta_\mathcal{P}(w)=f(w)$$。

综上：

$$\theta_\mathcal{P}(w)=\begin{cases}
f(w), & w满足约束 \\
\infty, & w不满足约束 \\
\end{cases}$$

我们定义：

$$p^*=\underset{w}{\operatorname{min}}\theta_\mathcal{P}(w)=\underset{w}{\operatorname{min}}\underset{\alpha,\beta:\alpha_i\ge 0}{\operatorname{max}}\mathcal{L}(w,\alpha,\beta)$$

下面我们定义对偶函数：

$$\theta_\mathcal{D}(w)=\underset{w}{\operatorname{min}}\mathcal{L}(w,\alpha,\beta)$$

这里的$$\mathcal{D}$$代表原始优化问题的对偶优化问题。仿照原始优化问题定义如下：

$$d^*=\underset{\alpha,\beta:\alpha_i\ge 0}{\operatorname{max}}\theta_\mathcal{D}(w)=\underset{\alpha,\beta:\alpha_i\ge 0}{\operatorname{max}}\underset{w}{\operatorname{min}}\mathcal{L}(w,\alpha,\beta)$$

这里我们不加证明的给出如下公式：

$$d^*=\underset{\alpha,\beta:\alpha_i\ge 0}{\operatorname{max}}\underset{w}{\operatorname{min}}\mathcal{L}(w,\alpha,\beta)\le\underset{w}{\operatorname{min}}\underset{\alpha,\beta:\alpha_i\ge 0}{\operatorname{max}}\mathcal{L}(w,\alpha,\beta)=p^*$$

这样的对偶问题被称作拉格朗日对偶（Lagrange duality）。

## KKT条件

拉格朗日对偶公式中使$$p^*=d^*$$成立的条件，被称为KKT条件（Karush-Kuhn-Tucker conditions）：

$$\begin{align}
\frac{\partial}{\partial w_i}\mathcal{L}(w^*,\alpha^*,\beta^*) & =0,i=1,\dots,n & \\
\frac{\partial}{\partial \beta_i}\mathcal{L}(w^*,\alpha^*,\beta^*) & =0,i=1,\dots,l & \\
\alpha_i^*g_i(w^*)& =0,i=1,\dots,k & \tag{1}\\
g_i(w^*)& \le 0,i=1,\dots,k & \\
\alpha_i^* & \ge 0,i=1,\dots,k & \\
\end{align}$$

其中的$$w^*,\alpha^*,\beta^*$$表示满足KKT条件的相应变量的取值。条件1也被称为KKT对偶互补条件（KKT dual complementarity condition）。显然这些$$w^*,\alpha^*,\beta^*$$既是原始问题的解，也是对偶问题的解。

严格的说，KKT条件是非线性约束优化问题存在最优解的必要条件。这个问题的充分条件比较复杂，这里不做讨论。

>注：Harold William Kuhn，1925～2014，美国数学家，普林斯顿大学教授。

>Albert William Tucker，1905～1995，加拿大数学家，普林斯顿大学教授。

>William Karush，1917～1997，美国数学家，加州州立大学北岭分校教授。（注意，California State University和University of California是不同的学校）

参考：

https://mp.weixin.qq.com/s/AKTGyHlbCj0P949K-LAv6A

拉格朗日乘数法

https://www.zhihu.com/question/64834116

拉格朗日乘子法漏解的情况？

https://mp.weixin.qq.com/s/UyNpJZ7k_q1qVdcNhrzDcQ

直观详解：拉格朗日乘法和KKT条件

https://mp.weixin.qq.com/s/PELnlB5vMV0gGJbL6BzoIA

原始-对偶算法的设计原理

## 支持向量

针对最优边距分类问题，我们定义：

$$g_i(w)=-y^{(i)}(w^Tx^{(i)}+b)+1\le 0$$

由KKT对偶互补条件可知，如果$$\alpha_i>0$$，则$$g_i(w)=0$$。

![](/images/article/SVM_3.png)

上图中的实线表示最大边距的分割超平面。由之前对于边距的几何意义的讨论可知，只有离该分界线最近的几个点（即图中的所示的两个x点和一个o点）才会取得约束条件的极值，即$$g_i(w)=0$$。也只有这几个点的$$\alpha_i>0$$，其余点的$$\alpha_i=0$$。这样的点被称作支持向量（support vectors）。显然支持向量的数量是远远小于样本集的数量的。

为我们的问题构建拉格朗日函数如下：

$$\mathcal{L}(w,b,\alpha)=\frac{1}{2}\|w\|^2-\sum_{i=1}^m\alpha_i[y^{(i)}(w^Tx^{(i)}+b)-1] \tag{2}$$

为了求解

$$\theta_\mathcal{D}(\alpha)=\underset{w,b}{\operatorname{min}}\mathcal{L}(w,b,\alpha)$$

可得：

$$\nabla_w\mathcal{L}(w,b,\alpha)=w-\sum_{i=1}^m\alpha_iy^{(i)}x^{(i)}=0$$

即

$$w=\sum_{i=1}^m\alpha_iy^{(i)}x^{(i)} \tag{3}$$

对b求导可得：

$$\frac{\partial}{\partial b}\mathcal{L}(w,b,\alpha)=\sum_{i=1}^m\alpha_iy^{(i)}=0 \tag{4}$$

