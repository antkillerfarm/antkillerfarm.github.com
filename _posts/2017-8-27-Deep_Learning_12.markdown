---
layout: post
title:  深度学习（十二）——Winograd（2）
category: DL 
---

# Winograd（续）

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

## Bézout's identity和Extended Euclidean algorithm

Euclidean division还可以表示为如下形式：

如果$$a'=a \mod(b)$$，则$$a=\lfloor\frac{a}{b}\rfloor b+a'$$。

这里的$$\lfloor x \rfloor$$是向下取整的意思。

我们可以把上述定义扩展到负数。例如：

$$-103=-2\times 60+17 \Rightarrow (-103) \mod{60}=17$$

>注意：余数永远$$\ge 0$$。

还可以把GCD的定义扩展为：$$GCD(a,0)=a$$，即任何整数都能整除0。

Euclidean division的定义扩展之后，则有Bézout's identity。

**Bézout's identity**：若a,b是非0整数，$$d=GCD(a,b)$$，则存在整数x,y使得$$ax+by=d$$。证明略。

例如：

$$m_0 = 3 , M_0 = 20 , ( − 1 ) 20 + 7 ( 3 ) = 1$$

>Étienne Bézout，1730~1783，法国数学家。法国科学院院士。

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

这里介绍一下筛法（Sieving）：

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

在中国古代，CRT问题又被称为“韩信点兵”问题。

>韩信带1500名兵士打仗，战死约四五百人，站3人一排，多出2人；站5人一排，多出4人；站7人一排，多出6人。韩信很快说出人数：1049。

## 多项式的Euclidean division和GCD

我们可以仿照整数Euclidean division定义多项式的Euclidean division，如下面的竖式所示：

$$\begin{array}{r}
 x^2 + {\color{White}1}x + 3\\
 x-3\overline{) x^3 - 2x^2 + 0x - 4}\\
 \underline{x^3 - 3x^2 \color{White}{ + 0x - 4}}\\
 +x^2 + 0x \color{White}{ - 4}\\
 \underline{+x^2 - 3x \color{White}{ - 4}}\\
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

例如：

$$m_0 = 3 , M_0 = 20 , ( − 1 ) 20 + 7 ( 3 ) = 1$$

$$m_1 = 4 , M_1 = 15 , ( − 1 ) 15 + ( 4 ) 4 = 1$$

$$m_2 = 5 , M_2 = 12 , ( − 2 ) 12 + ( 5 ) 5 = 1$$

稍加扩展，可得到多项式版本的CRT：

$$c(p)=\left(\sum_{i=0}^kc^{(i)}(p)N^{(i)}(p)M^{(i)}(p)\right)\mod{M(p)}\tag{3}$$

其中$$m^{(i)}(p)$$两两互质，$$c^{(i)}(p)=R_{m^{(i)}}[c(p)],M(p)=\prod_{i=0}^km^{(i)}(p),M^{(i)}(p)=M(p)/m^{(i)}(p)$$，$$N^{(i)}(p)$$是方程$$N^{(i)}(p) M^{(i)}(p) + n^{(i)}(p) m^{(i)}(p) = GCD ( M^{(i)}(p) , m^{(i)}(p)) = 1$$的解。


