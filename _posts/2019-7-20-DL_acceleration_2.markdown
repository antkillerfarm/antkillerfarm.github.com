---
layout: post
title:  深度加速（二）——Winograd（2）
category: DL acceleration 
---

# Winograd（续）

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

## Winograd algorithm

下面以一个2x3的卷积为例，介绍一下Winograd algorithm的做法。

2x3卷积的多项式形式为：

$$h(p)=h_0+h_1p,x(p)=x_0+x_1p+x_2p^2,s(p)=h(p)x(p)$$

这里引入**多项式（polynomial）的度（degree）**的概念：多项式中包含的最高次项的次数，被称为多项式的度。

例如，上面的$$h(p)$$的degree为1，而$$x(p)$$的degree为2，而$$s(p)$$的degree为3。

和Cook-Toom algorithm一样，Winograd algorithm也是一个构造式的算法。

**Step 1**：首先要构造一个degree大于等于3的多项式：

$$m(p)=m^{(0)}(p)m^{(0)}(p)\cdots m^{(k)}(p)$$

其中的$$m^{(i)}(p)$$两两互质。

这里为了简单起见，不妨令$$m(p)=p(p-1)(p+1)$$，并使用Extended Euclidean algorithm构建如下计算表格：

| i | $$m^{(i)}(p)$$ | $$M^{(i)}(p)$$ | $$n^{(i)}(p)$$ | $$N^{(i)}(p)$$ |
|:--:|:--:|:--:|:--:|:--:|
| 0 | $$p$$ | $$p^2-1$$ | $$p$$ | $$-1$$ |
| 1 | $$p-1$$ | $$p^2+p$$ | $$-\frac{1}{2}(p+2)$$ | $$\frac{1}{2}$$ |
| 2 | $$p+1$$ | $$p^2-p$$ | $$-\frac{1}{2}(p-2)$$ | $$\frac{1}{2}$$ |

**Step 2**：使用如下公式计算$$h^{(i)}(p),x^{(i)}(p)$$：

$$h^{(i)}(p)=h(p)\mod{m^{(i)}(p)}$$

$$x^{(i)}(p)=x(p)\mod{m^{(i)}(p)}$$

计算过程如下：

$$h^{(0)}(p)=h_0,x^{(0)}(p)=x_0$$

$$h^{(1)}(p)=h_0+h_1,x^{(1)}(p)=x_0+x_1+x_2$$

$$h^{(2)}(p)=h_0-h_1,x^{(2)}(p)=x_0-x_1+x_2$$

**Step 3**：使用如下公式计算$$s'^{(i)}(p)$$：

$$s'^{(i)}(p)=h^{(i)}(p)x^{(i)}(p)\mod{m^{(i)}(p)}$$

计算过程如下：

$$s'^{(0)}(p)=h_0x_0$$

$$s'^{(1)}(p)=(h_0+h_1)(x_0+x_1+x_2)$$

$$s'^{(2)}(p)=(h_0-h_1)(x_0-x_1+x_2)$$

**Step 4**：根据公式3计算余数$$s'(p)$$，并利用如下公式计算被除数$$s(p)$$：

$$s(p)=s'(p)+h_{N-1}x_{L-1}m(p)$$

计算过程如下：

$$\begin{align}
s(p)&=s'(p)+h_1x_2m(p) \\
&=s'(0)(-p^2+1)+\frac{s'(1)}{2}(p^2+p)+\frac{s'(2)}{2}(p^2-p)+h_1x_2m(p^3-p)\\
&=s'(0)+p(\frac{s'(1)}{2}-\frac{s'(2)}{2}-h_1x_2)+p^2(-s'(0)+\frac{s'(1)}{2}+\frac{s'(2)}{2})+p^3(h_1x_2)
\end{align}$$

这里用4个乘法和7个加法，替代了6个乘法和2个加法。

总的来说，Winograd algorithm是一个很复杂的算法，但是结论却很简单。因此，**在具体的IC实现中，一般只针对特定常用尺寸的kernel，应用相应的结论即可。**

>Winograd这个知识点的复杂，其实主要还不在于算法本身，而是在于其前置了很多数论方面的知识。而我恰恰不具备这些知识，因此进展极度缓慢，前后用了近20天才看完了相关的内容。。。不过，收获很大^_^

## Winograd for CNN

CNN中的Winograd算法一般使用如下论文的结论：

《Fast Algorithms for Convolutional Neural Networks》

该文引论部分提到了Winograd算法的结论，该结论和本文上述的算法步骤略有不同，最初是Winograd针对FIR提出的Minimal FIR Filtering算法。但是算法的本质是相同的，仍然是构建多项式和CRT。

https://github.com/andravin/wincnn

这个项目可以很方便的计算不同大小的核的Winograd的结果。这个项目中还有一个pdf文件作为上述论文的补充材料，详细的给出了各矩阵的计算方法。

论文：

《Efficient Sparse-Winograd Convolutional Neural Networks》

![](/images/img3/Winograd.png)

这篇论文（2018.2）讨论了如何在稀疏矩阵中应用Winograd算法。

论文：

《Sparse Winograd Convolutional neural networks on small-scale systolic arrays》

![](/images/img3/Winograd_3.png)

这篇论文（2018.10）讨论了如何在脉动阵列上实现Winograd算法，还讨论了3D卷积的计算方法。

![](/images/img3/Winograd_2.png)

参考：

http://shuokay.com/2018/02/21/winograd/

Winograd方法快速计算卷积
