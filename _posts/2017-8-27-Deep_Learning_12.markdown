---
layout: post
title:  深度学习（十二）——Winograd（2）
category: DL 
---

# Winograd

## Bézout's identity和Extended Euclidean algorithm（续）

至于如何求解x,y，这就要用到**Extended Euclidean algorithm**了。

首先，我们考虑边界情况，当$$b=0$$时，原方程可化为$$ax=GCD(a,0)$$，则：

$$\begin{cases}
x=1 \\
y=0 \\
\end{cases}\tag{1}$$

接着，设$$a'=b,b'=a \mod{b}$$，则由Euclidean algorithm可得：$$GCD(a,b)=GCD(b,a \mod b)$$，因此：

$$a​′​​x​′​​+b​′​​y​′​​=GCD(a​′​​,b​′​​)=GCD(b,a \mod{b})$$

$$ax+by=a​′​​x​′​​+b​′​​y​′​​=bx​′​​+(a\mod{b})y​′​​=bx​′​​+(a−\lfloor\frac{a}{b}\rfloor b)y​′​​$$

整理得到：

$$ax+by=ay​′​​+b(x​′​​−\lfloor\frac{a}{b}\rfloor y​′​​)$$

对比系数，可得：

$$\begin{cases}
x=y' \\
y=x'−\lfloor\frac{a}{b}\rfloor y​′​​ \\
\end{cases}\tag{2}$$

公式1和2合到一起，就是一种迭代算法，也就是Extended Euclidean algorithm了。

从上面的讨论可知，Extended Euclidean algorithm实际上只在Euclidean algorithm之上前进了很小的一步，它的主要内核还是来源于Euclid。

但Euclid之所以不能更进一步，则主要是受制于负数的概念。虽然现在的小学高年级课本中，已经引入了负数，古代中国、印度、阿拉伯也很早就用到了负数，但是西方差不多要到文艺复兴时期，才逐渐接受了负数的概念。

不过反例也是有的，比如无理数，其它文明貌似根本就没有关注过它和有理数究竟有何区别...

参考：

https://blog.sengxian.com/algorithms/gcd-extgcd

欧几里德算法与扩展欧几里德算法

## 中国剩余定理

Chinese remainder theorem算是初等数论中，一个非常重要的定理了。（初等数论意指使用不超过高中程度的初等代数处理的数论问题，其最主要的工具包括整数的整除性与同余。）

CRT最早出自中国四世纪成书的古书《孙子算经》。著名的娱乐圈学霸关晓彤同学所攻克的“鸡兔同笼问题”，就出自该书。 

CRT的内容为：

设$$m_i$$为两两互质（pairwise coprime）的大于1的整数，$$a_i$$为任意整数，则存在x满足：

$$\begin{align} x \equiv a_1 & \pmod{m_1} \\ \quad \vdots \\ x \equiv a_k &\pmod{m_k} \end{align}$$

如果$$0\le x < M,M=\prod_{i=1}^k m_i$$，则该x是唯一的。

CRT的存在性证明略。

这里以如下简单的例子，来讲讲如何求解x。

$$\begin{align}
 x &\equiv 0 \pmod{3} \\
 x &\equiv 3 \pmod{4} \\
 x &\equiv 4 \pmod{5}.
\end{align}$$

这个问题的穷举法需要遍历0到M的所有整数，这显然是个十分低效的算法。因此无论手算还是计算机算，基本都不用穷举法。

再来介绍一下筛法（Sieving）：

1.首先对$$m_i$$按降序排序。

2.选择最大的模（这里为5）和对应的$$a_i$$（这里为4）。

3.

{% highlight text %}
4 mod 4 → 0. Continue
4 + 5 = 9 mod 4 →1. Continue
9 + 5 = 14 mod 4 → 2. Continue
14 + 5 = 19 mod 4 → 3. OK, continue by considering remainders modulo 3 and adding 5×4 = 20 each time
19 mod 3 → 1. Continue
19 + 20 = 39 mod 3 → 0. OK, this is the result.
{% endhighlight %}

筛法对于M较小的情况，是非常高效的，因此手算一般都采用该法。但是，筛法的复杂度是指数级的，对于M较大的情况，并不好用。

CRT虽然只是初等数论的基本定理，但应用范围很广，Lagrange interpolation（一阶多项式插值）、Hermite interpolation（多阶多项式插值）和Dedekind's theorem，都用到了CRT。

## 多项式的Euclidean division和GCD

我们可以仿照整数Euclidean division定义多项式的Euclidean division，如下面的竖式所示：

$$\begin{array}{r}
 x^2 + {\color{White}1}x + 3\\
 x-3\overline{) x^3 - 2x^2 + 0x - 4}\\
 \underline{x^3 - 3x^2 {\color{White} {} + 0x - 4}}\\
 +x^2 + 0x {\color{White} {} - 4}\\
 \underline{+x^2 - 3x {\color{White} {} - 4}}\\
 +3x - 4\\
 \underline{+3x - 9}\\
 +5
\end{array}$$

上式也可写为横式：

$${x^3 - 2x^2 - 4} = (x-3)\,\underbrace{(x^2 + x + 3)}_{q(x)}  +\underbrace{5}_{r(x)}$$

其中的$$r(x)$$即为余数。

同样的可以定义多项式的GCD：

$$x^2 + 7x + 6 = (x + 1)(x + 6)$$

$$x^2 − 5x − 6 = (x + 1)(x − 6)$$

则两多项式的GCD为$$(x + 1)$$。

## 多项式的CRT

CRT亦可改为如下等效形式：

$$c=\left(\sum_{i=0}^kc_iN_iM_i\right)\mod{M}$$

其中$$m_i$$两两互质，$$c_i=R_{m_i}[c],M=\prod_{i=0}^km_i,M_i=M/m_i$$，$$N_i,n_i$$是方程$$N_i M_i + n_i m_i = GCD ( M_i , m_i ) = 1$$的解。

显然这里的$$N_i,n_i$$可以使用Extended Euclidean algorithm求解。

稍加扩展，可得到多项式版本的CRT：

$$c(p)=\left(\sum_{i=0}^kc^{(i)}(p)N^{(i)}(p)M^{(i)}(p)\right)\mod{M(p)}$$

其中$$m^{(i)}(p)$$两两互质，$$c^{(i)}(p)=R_{m^{(i)}}[c(p)],M(p)=\prod_{i=0}^km^{(i)}(p),M^{(i)}(p)=M(p)/m^{(i)}(p)$$，$$N^{(i)}(p)$$是方程$$N^{(i)}(p) M^{(i)}(p) + n^{(i)}(p) m^{(i)}(p) = GCD ( M^{(i)}(p) , m^{(i)}(p)) = 1$$的解。

## Winograd algorithm

下面以一个2x3的卷积为例，介绍一下Winograd algorithm的做法。

2x3卷积的多项式形式为：

$$h(p)=h_0+h_1p,x(p)=x_0+x_1p+x_2p^2,s(p)=h(p)x(p)$$

这里引入**多项式（polynomial）的度（degree）**的概念：多项式中包含的最高次项的次数，被称为多项式的度。

例如，上面的$$h(p)$$的degree为1，而$$x(p)$$的degree为2，而$$s(p)$$的degree为3。

和Cook-Toom algorithm一样，Winograd algorithm也是一个构造式的算法。我们首先要构造一个degree大于等于3的多项式：

$$m(p)=m^{(0)}(p)m^{(0)}(p)\cdots m^{(k)}(p)$$

其中的$$m^{(i)}(p)$$两两互质。

这里为了简单起见，不妨令$$m(p)=p(p-1)(p+1)$$，构建如下计算表格：

| i | $$m^{(i)}(p)$$ | $$M^{(i)}(p)$$ | $$n^{(i)}(p)$$ | $$N^{(i)}(p)$$ |
|:--:|:--:|:--:|:--:|:--:|
| 0 | $$p$$ | $$p$$ | $$p$$ | $$p$$ |
| 1 | $$p$$ | $$p$$ | $$p$$ | $$p$$ |
| 2 | $$p$$ | $$p$$ | $$p$$ | $$p$$ |

总的来说，Winograd algorithm是一个很复杂的算法，但是结论却很简单。因此，在具体的IC实现中，一般只针对特定常用尺寸的kernel，应用相应的结论即可。

## 参考

https://colfaxresearch.com/falcon-library/

FALCON Library: Fast Image Convolution in Neural Networks on Intel Architecture

https://www.intelnervana.com/winograd/

"Not so fast, FFT": Winograd

https://www.encyclopediaofmath.org/index.php/Winograd_small_convolution_algorithm

Winograd small convolution algorithm


