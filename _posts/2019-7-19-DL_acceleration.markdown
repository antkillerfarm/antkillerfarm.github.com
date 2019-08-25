---
layout: post
title:  深度加速（一）——概述, Winograd（1）
category: DL acceleration 
---

# 概述

书籍：

《Embedded Deep Learning:  Algorithms, Architectures and Circuits for Always-on Neural Network Processing》

# Winograd

矩阵方面的数值计算，Shmuel Winograd是一个无法绕开的人物。

>Shmuel Winograd, 1936年生，MIT本硕（1959年）+纽约大学博士（1968年）。此后一直在IBM当研究员，直到退休。IEEE Fellow，ACM Fellow，美国科学院院士。

>不要和Terry Winograd搞混了。后者是Google的两位创始人Larry Page和Sergey Brin的导师。MIT博士+斯坦福教授。

>Terry Winograd的导师也很有名，他就是AI界的宗师Seymour Papert。   
>Seymour Aubrey Papert，1928～2016，南非威特沃特斯兰德大学博士+剑桥大学博士。MIT教授。曾和Marvin Minsky合著《Perceptrons》，并一同创建了MIT AI Lab。MIT Media Lab的创立者。启蒙编程语言Logo之父。   
>更多生平参见：   
>https://zhuanlan.zhihu.com/p/21841141   
>Seymour Papert留给我们的思想遗产

>Seymour Papert至少给三个领域带来了革命，分别是儿童发展、人工智能，以及科技与教育之融合。能够在上述任何一个领域做出一点成绩都不容易了，而Papert则横跨三个领域，而且对这三个领域的发展都带来了极为深远的影响。

**Winograd FFT algorithm**：一种FFT算法。FFT算法有很多，最知名的是Cooley–Tukey FFT algorithm。

**Coppersmith–Winograd algorithm（1987年）**：目前最快的矩阵乘法算法。复杂度是$$\mathcal{O}(n^{2.375477})$$。矩阵乘法定义的复杂度是$$\mathcal{O}(n^{3})$$。第一个小于3的算法是Strassen algorithm（1969年）（$$\mathcal{O}(n^{2.807355})$$）。

**Winograd small(short/minimal) convolution algorithm**：一种快速的卷积算法，目前AI芯片领域的基础算法。

论文：

《The Coppersmith-Winograd Matrix Multiplication Algorithm》

## 基本思想

下文的大部分内容摘自Parhi的书《VLSI Digital Signal Processing Systems: Design and Implementation》

>Keshab K. Parhi，Indian Institute of Technology Kharagpur本科（1982年）+宾夕法尼亚大学硕士（1984年）+UCB博士（1988年）。明尼苏达大学教授。

这是相关章节的slide：

http://people.ece.umn.edu/users/parhi/SLIDES/chap8.pdf

Fast Convolution

我们这里以一个简单的例子，介绍一下Fast Convolution的基本思想。

复数运算$$(a+jb)(c+jd)=e+jf$$可以写成如下的矩阵形式：

$$\begin{bmatrix}
e \\ f \\
\end{bmatrix}=
\begin{bmatrix}
c & -d \\
d & c \\
\end{bmatrix}
\begin{bmatrix}
a \\ b \\
\end{bmatrix}$$

上式包含了4个乘法和2个加法。

我们知道，乘法和加法在硬件实现上的时间复杂度一般是不一样的，乘法运算所需的时间通常远大于加法所需的时间。因此，**用廉价运算代替昂贵运算**就成为了加速运算的一种方法。

具体到上面的例子：

$$\begin{cases}
ac − bd = a ( c − d ) + d ( a − b )\\
ad + bc = b ( c + d ) + d ( a − b ) \\
\end{cases}$$

这个公式包含了3个乘法和5个加法，也就是说我们用3个加法替代了1个乘法，显然只要1个乘法所需的时间大于3个加法的时间，这样的变换就是有意义的。

上式可以写成如下的矩阵形式：

$$\begin{bmatrix}
e \\ f \\
\end{bmatrix}=
\begin{bmatrix}
1 & 0 & 1 \\
0 & 1 & 1 \\
\end{bmatrix}\cdot
\begin{bmatrix}
c-d & 0 & 0 \\
0 & c+d & 0 \\
0 & 0 & d \\
\end{bmatrix}\cdot
\begin{bmatrix}
1 & 0 \\
0 & 1 \\
1 & -1\\
\end{bmatrix}\cdot
\begin{bmatrix}
a \\ b \\
\end{bmatrix}=C \cdot H \cdot D \cdot x$$

这里的H是对角矩阵。

Fast Convolution算法主要有**基于Lagrange插值的Cook-Toom算法**和**基于中国剩余定理（Chinese remainder theorem）的Winograd算法**。

## 向量卷积与多项式乘法

在继续介绍之前，我们首先看一下向量卷积与多项式乘法之间的关系。

我们首先来看一个简单的多项式乘法的例子：

$$(x^2+2x+2)(3x+2)=3x^3+\color{red}{(2\times 3+1\times 2)}x^2+10x+4$$

上式中红色的部分是有意保留下来没有合并的同类项，是不是感觉上和卷积运算十分类似呢？

我们知道矩阵的定义，除了最原始的线性方程组定义之外，还有线性向量空间定义。而多项式可以看做是以$${1,x,x^2,\dots}$$为基的空间向量，因此用多项式表示矩阵或者卷积运算，是一种很自然的方式。

这是有限离散域的Convolution定义：

$$(f * g)[n]=\sum_{m=-M}^M f[n-m]g[m]$$

>注：这里的Convolution和DL领域的Convolution的定义略有不同，后者实际上是数学上的Cross-correlation运算，但稍加变化，就可以变为前者。

由多项式乘法的规则和Convolution定义可得：（这里以2x2的Convolution为例）

令$$h(p)=h_0+h_1p,x(p)=x_0+x_1p,s(p)=h(p)x(p)=s_0+s_1p+s_2p^2$$，则$$s(p)$$的系数$$s_i$$正好为i点的卷积值$$Conv_i(h,x)$$。

上述事实的矩阵表示为：

$$\begin{bmatrix}
s_0 \\ s_1 \\ s_2 \\
\end{bmatrix}=
\begin{bmatrix}
h_0 & 0 \\
h_1 & h_0 \\
0 & h_1 \\
\end{bmatrix}\cdot
\begin{bmatrix}
x_0 \\ x_1 \\
\end{bmatrix}$$

向量卷积与多项式乘法的这个性质，在数值计算领域应用的比较广泛。Matlab中多项式的乘法计算，实际上就是计算一个卷积。

如果令p=10，则多项式乘法就变成了大整数乘法，因此卷积运算在大整数乘法中，也有很重要的地位。

参见：

http://www.cnblogs.com/larch18/p/4569495.html

向量卷积与多项式乘法

http://blog.sina.com.cn/s/blog_455c7a6001010t3h.html

卷积和与多项式乘法的关系

## Cook-Toom algorithm

>Andrei Leonovich Toom，俄国数学家。

>Stephen Cook，1939年生，密歇根大学本科（1961年）+哈佛硕博（1962年、1966年）。多伦多大学教授，图灵奖获得者（1982年）。

>Cook的生平虽然让我感兴趣，然而我更感兴趣的却是他的导师王浩。   
>王浩，1921年~1995年，数理逻辑学家，山东人。西南联大本科（1943年）+清华硕士（1945年）+哈佛博士（1948年）。先后执教于哈佛大学、牛津大学和洛克菲勒大学。英国皇家学会会员。IJCAI第一届“数学定理机械证明里程碑奖”获得者。   
>鉴于这是一个大路货的名字，这里特给出百度百科的网址：   
>https://baike.baidu.com/item/王浩/22564

这里仍以上述2x2的Convolution为例，讲述一下Cook-Toom算法的步骤。

$$\beta_0=0, h(\beta_0)=h_0, x(\beta_0)=x_0$$

$$\beta_1=1, h(\beta_1)=h_0+h_1, x(\beta_1)=x_0+x_1$$

$$\beta_2=-1, h(\beta_2)=h_0-h_1, x(\beta_2)=x_0-x_1$$

接着计算$$s(\beta_i)$$；

$$s(\beta_0)=h(\beta_0)x(\beta_0),s(\beta_1)=h(\beta_1)x(\beta_1),s(\beta_2)=h(\beta_2)x(\beta_2)$$

有了3个已知点，就可以应用Lagrange插值了（Lagrange插值的内容可参见《机器学习（一）》）：

$$\begin{align}s(p)&=s(\beta_0)\frac{(p-\beta_1)(p-\beta_2)}{(\beta_0-\beta_1)(\beta_0-\beta_2)}+s(\beta_1)\frac{(p-\beta_0)(p-\beta_2)}{(\beta_1-\beta_0)(\beta_1-\beta_2)}+s(\beta_2)\frac{(p-\beta_0)(p-\beta_1)}{(\beta_2-\beta_0)(\beta_2-\beta_1)}
\\&=s(\beta_0)+p\left(\frac{s(\beta_1)-s(\beta_2)}{2}\right)+p^2\left(-s(\beta_0)+\frac{s(\beta_1)+s(\beta_2)}{2}\right)
\\&=s_0+ps_1+p^2s_2
\end{align}$$

上式用矩阵形式可表示为：

$$\begin{align}
\begin{bmatrix}
s_0 \\ s_1 \\ s_2 \\
\end{bmatrix}
&=\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & -1 \\
-1 & 1 & 1 \\
\end{bmatrix}\cdot
\begin{bmatrix}
s(\beta_0) \\ s(\beta_1)/2 \\ s(\beta_2)/2 \\
\end{bmatrix}
\\&=\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & -1 \\
-1 & 1 & 1 \\
\end{bmatrix}\cdot
\begin{bmatrix}
h(\beta_0) & 0 & 0 \\
0 & h(\beta_1)/2 & 0 \\
0 & 0 & h(\beta_2)/2 \\
\end{bmatrix}\cdot
\begin{bmatrix}
x(\beta_0) \\ x(\beta_1) \\ x(\beta_2) \\
\end{bmatrix}
\\&=\begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & -1 \\
-1 & 1 & 1 \\
\end{bmatrix}\cdot
\begin{bmatrix}
h_0 & 0 & 0 \\
0 & (h_0+h_1)/2 & 0 \\
0 & 0 & (h_0-h_1)/2 \\
\end{bmatrix}\cdot
\begin{bmatrix}
1 & 0 \\
1 & 1 \\
1 & -1 \\
\end{bmatrix}\cdot
\begin{bmatrix}
x_0 \\ x_1 \\
\end{bmatrix}
\end{align}$$

表面看起来这里多了2个除法，似乎计算量更大了，但是：

1.除以2，对于2进制的计算机来说是个简单运算。

2.如果大量卷积运算的其中一方不变的话，例如CNN中的kernel，这些运算都可以在预处理阶段计算。

Cook-Toom algorithm的缺点在于：当卷积核较大时，增加的加法数量以远超核大小的速度增长，最终会导致增加的加法所耗费的时间甚至超过节省下来的乘法所耗费的时间。

## 最大公约数和Euclidean algorithm

在介绍Winograd算法之前，我们首先介绍一下求最大公约数的Euclidean algorithm。

小学课本中介绍了最大公约数（Greatest common divisor）的定义，当时给出的算法是如下图所示的短除法（Short division）：

![](/images/article/Short_division.jpg)

但这个算法实际上是个非常低效的方法。实际中最常用的是Euclid在他的巨著《几何原本》中，给出的**Euclidean algorithm**，中文叫做**辗转相除法**。

顺便提一句，求余数的整数除法，也被称作**Euclidean division**。（普通整数除法以小数，而非余数（remainder），代替无法整除的部分）宗师就是这么牛！

这里引入如下数学符号：

$$m(m>0)$$整除n记作$$m \mid n$$，其定义为存在一个整数k使得$$km=n$$。

余数一般用$$c=R_m[n]$$或$$c=n \mod{m}$$来表示。

再引入**同余（congruences）**的概念和符号：

$$x \equiv a \pmod{m}$$

上式表示x除以m的余数和a除以m的余数相等，这样的关系被称为“x和a同余（模为m）”

Euclidean algorithm的步骤如下图所示：

![](/images/article/Euclidean_algorithm.png)

1.假设$$a>b$$，则令$$c:=a \mod{b}$$。

2.如果$$c=0$$，则$$GCD(a,b)=b$$。

3.否则令$$a:=b,b:=c$$，并返回到第1步。

这个算法应该是Euclid记述的前人成果，因为更早的Eudoxus of Cnidus曾提到过这个算法。

>Eudoxus of Cnidus，公元前390年～公元前337年，古希腊几何学家、天文学家和地理学家。柏拉图同时代最杰出的数学家。《几何原本》卷Ⅴ和卷Ⅻ主要来自欧多克索斯的工作。

然而，小学课本不使用Euclidean algorithm是有原因的，除了Euclidean algorithm本身相对复杂之外，短除法能同时搞定最大公约数和最小公倍数（Least common multiple），这也是它的教学优势所在。

Euclidean algorithm作为最古老的算法之一，被收录进Knuth的巨著TAOCP。这里的算法，指的是那些根据一定的规则来一步步执行的运算。
