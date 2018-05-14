---
layout: post
title:  机器学习（六）——SVM（3）
category: ML 
---

## 序列最小优化方法（续）

同理：

$$\begin{align}
W(\alpha)&=\sum_{i=1}^m\alpha_i-\frac{1}{2}\sum_{i,j=1}^my^{(i)}y^{(j)}\alpha_i\alpha_j\langle x^{(i)},x^{(j)}\rangle=\sum_{i=1}^m\alpha_i-\frac{1}{2}\sum_{i,j=1}^my^{(i)}y^{(j)}\alpha_i\alpha_jK_{ij}
\\&\begin{split}
=\alpha_1+\alpha_2+\sum_{i=3}^m\alpha_i-\frac{1}{2}\left(\sum_{i,j=1}^2y^{(i)}y^{(j)}\alpha_i\alpha_jK_{ij}+2\sum_{j=3}^my^{(1)}y^{(j)}\alpha_1\alpha_jK_{1j}
\\+2\sum_{j=3}^my^{(2)}y^{(j)}\alpha_2\alpha_jK_{2j}+\sum_{i,j=3}^my^{(i)}y^{(j)}\alpha_i\alpha_jK_{ij}\right)
\end{split}
\\&=\alpha_1+\alpha_2+\psi_1-\frac{1}{2}\sum_{i,j=1}^2y^{(i)}y^{(j)}\alpha_i\alpha_jK_{ij}-\sum_{j=3}^my^{(1)}y^{(j)}\alpha_1\alpha_jK_{1j}-\sum_{j=3}^my^{(2)}y^{(j)}\alpha_2\alpha_jK_{2j}-\psi_2
\\&=\alpha_1+\alpha_2-\frac{1}{2}\left((y^{(1)})^2\alpha_1^2K_{11}+(y^{(2)})^2\alpha_2^2K_{22}+2y^{(1)}y^{(2)}\alpha_1\alpha_2K_{12}\right)
\\&\qquad-y^{(1)}\alpha_1\sum_{j=3}^my^{(j)}\alpha_jK_{1j}-y^{(2)}\alpha_2\sum_{j=3}^my^{(j)}\alpha_jK_{2j}+\psi_3
\\&=\alpha_1+\alpha_2-\frac{1}{2}\alpha_1^2K_{11}-\frac{1}{2}\alpha_2^2K_{22}-s\alpha_1\alpha_2K_{12}-y^{(1)}\alpha_1v_1-y^{(2)}\alpha_2v_2+\psi_3 \tag{5}
\end{align}$$

其中，$$\psi_i$$表示常数项，$$s=y^{(1)}y^{(2)}$$，$$v_i=\sum_{j=3}^my^{(j)}\alpha_jK_{ij}$$。

![](/images/article/SVM_8.png)

如上图所示，因为约束2的存在，$$\alpha_1$$和$$\alpha_2$$的值实际上被固定在如图所示的$$[0,C]\times[0,C]$$的方框内。根据约束3可得，$$\alpha_1$$和$$\alpha_2$$落在图中的直线上。从图中还可看出$$L\le \alpha_2\le H$$，其严格定义如下：

$$\begin{align}
L=\max(0,\alpha_2-\alpha_1),H=\min(C,C+\alpha_2-\alpha_1) & & if\ y^{(1)}\neq y^{(2)}\\
L=\max(0,\alpha_2+\alpha_1-C),H=\min(C,\alpha_2+\alpha_1) & & if\ y^{(1)}= y^{(2)}
\end{align}$$

根据公式4可得：

$$\alpha_1=\frac{(\zeta-\alpha_2y^{(2)})}{y^{(1)}}$$

因为$$y^{(1)}$$的取值要么是1，要么是-1，即$$(y^{(1)})^2=1$$，同理，$$s^2=1$$。因此上式又可改写为：

$$\begin{align}
\alpha_1&=\frac{(\zeta-\alpha_2y^{(2)})}{y^{(1)}}=\frac{(\zeta-\alpha_2y^{(2)})(y^{(1)})^2}{y^{(1)}}=(\zeta-\alpha_2y^{(2)})y^{(1)}
\\&=y^{(1)}\zeta-y^{(1)}y^{(2)}\alpha_2=\omega-s\alpha_2
\end{align}$$

其中$$\omega=y^{(1)}\zeta$$。

将上式代入公式5可得：

$$\begin{align}
W(\alpha)&=\alpha_1+\alpha_2-\frac{1}{2}\alpha_1^2K_{11}-\frac{1}{2}\alpha_2^2K_{22}-s\alpha_1\alpha_2K_{12}-y^{(1)}\alpha_1v_1-y^{(2)}\alpha_2v_2+\psi_3
\\&=(\omega-s\alpha_2)+\alpha_2-\frac{1}{2}(\omega-s\alpha_2)^2K_{11}-\frac{1}{2}\alpha_2^2K_{22}-s(\omega-s\alpha_2)\alpha_2K_{12}
\\&\qquad-y^{(1)}(\omega-s\alpha_2)v_1-y^{(2)}\alpha_2v_2+\psi_3
\\&=(\omega-s\alpha_2)+\alpha_2-\frac{1}{2}(\omega-s\alpha_2)^2K_{11}-\frac{1}{2}\alpha_2^2K_{22}-s\omega\alpha_2K_{12}+s^2\alpha_2^2K_{12}
\\&\qquad-y^{(1)}(\omega-s\alpha_2)v_1-y^{(2)}\alpha_2v_2+\psi_3
\\&=(\omega-s\alpha_2)+\alpha_2-\frac{1}{2}(\omega-s\alpha_2)^2K_{11}-\frac{1}{2}\alpha_2^2K_{22}-s\omega\alpha_2K_{12}+\alpha_2^2K_{12}
\\&\qquad-y^{(1)}(\omega-s\alpha_2)v_1-y^{(2)}\alpha_2v_2+\psi_3
\end{align}$$

对上式求导可得：

$$\begin{align}
\frac{\mathrm{d}W(\alpha)}{\mathrm{d}\alpha_2}&=-s+1+s(\omega-s\alpha_2)K_{11}-\alpha_2K_{22}-s\omega K_{12}+2\alpha_2K_{12}+y^{(1)}sv_1-y^{(2)}v_2
\\&=-s+1+s\omega K_{11}-\alpha_2K_{11}-\alpha_2K_{22}-s\omega K_{12}+2\alpha_2K_{12}+y^{(2)}v_1-y^{(2)}v_2=0
\end{align}$$

所以：

$$\alpha_2(K_{11}+K_{22}-2K_{12})=s\omega(K_{11}-K_{12})+y^{(2)}(v_1-v_2)+1-s \tag{6}$$

定义$$u=w^Tx+b$$，则根据《机器学习（四）》中的公式6，可得$$u_i=\sum_{j=1}^m\alpha_jy^{(j)}K(x^{(i)},x^{(j)})+b$$

因为迭代前后约束条件3不变，所以：

$$\alpha_1y^{(1)}+\alpha_2y^{(2)}=-\sum_{i=3}^m\alpha_iy^{(i)}=\alpha_1^*y^{(1)}+\alpha_2^*y^{(2)}$$

$$\alpha_1+s\alpha_2=\omega=\alpha_1^*+s\alpha_2^* \tag{7}$$

$$v_i=\sum_{j=3}^my^{(j)}\alpha_j^*K_{ij}=u_i-b^*-y^{(1)}\alpha_1^*K_{1i}-y^{(2)}\alpha_2^*K_{2i} \tag{8}$$

$$\begin{align}
v_1-v_2&=(u_1-b^*-y^{(1)}\alpha_1^*K_{11}-y^{(2)}\alpha_2^*K_{21})-(u_2-b^*-y^{(1)}\alpha_1^*K_{12}-y^{(2)}\alpha_2^*K_{22})
\\&=u_1-u_2-y^{(1)}\alpha_1^*K_{11}-y^{(2)}\alpha_2^*K_{21}+y^{(1)}\alpha_1^*K_{12}+y^{(2)}\alpha_2^*K_{22}
\end{align}$$

$$\begin{align}
y^{(2)}(v_1-v_2)+1-s&=y^{(2)}(u_1-u_2)-y^{(2)}y^{(1)}\alpha_1^*K_{11}-(y^{(2)})^2\alpha_2^*K_{21}+y^{(2)}y^{(1)}\alpha_1^*K_{12}
\\&\qquad+(y^{(2)})^2\alpha_2^*K_{22}+(y^{(2)})^2-y^{(2)}y^{(1)}
\\&=y^{(2)}(u_1-u_2)-s\alpha_1^*K_{11}-\alpha_2^*K_{21}+s\alpha_1^*K_{12}
\\&\qquad+\alpha_2^*K_{22}+(y^{(2)})^2-y^{(2)}y^{(1)}
\\&=y^{(2)}(u_1-u_2+y^{(2)}-y^{(1)})-s\alpha_1^*K_{11}-\alpha_2^*K_{21}+s\alpha_1^*K_{12}+\alpha_2^*K_{22}
\end{align}$$

这里的$$\alpha^*$$代表旧的迭代值，虽然它的含义和之前讨论KKT条件时的$$\alpha^*$$有所不同，但内涵是一致的——迭代值的极限是最优值，且迭代过程满足约束条件。其他变量也是类似的。

将$$\omega$$、v代入公式6可得：

$$\alpha_2(K_{11}+K_{22}-2K_{12})=s(\alpha_1^*+s\alpha_2^*)(K_{11}-K_{12})+y^{(2)}(v_1-v_2)+1-s$$

$$\alpha_2(K_{11}+K_{22}-2K_{12})=s\alpha_1^*K_{11}+\alpha_2^*K_{11}-s\alpha_1^*K_{12}-\alpha_2^*K_{12}+y^{(2)}(v_1-v_2)+1-s$$

$$\alpha_2(K_{11}+K_{22}-2K_{12})=\alpha_2^*(K_{11}+K_{22}-2K_{12})+y^{(2)}(u_1-u_2+y^{(2)}-y^{(1)})$$

定义$$\eta=K_{11}+K_{22}-2K_{12},E_i=u_i-y^{(i)}$$。其中，$$\eta$$是W的二阶导数，而$$E_i$$是第i个训练样本的误差，即预测值和实际值的差。

$$\alpha_2\eta=\alpha_2^*\eta+y^{(2)}(E_1-E_2)$$

$$\alpha_2^{new,unclipped}=\alpha_2+\frac{y^{(2)}(E_1-E_2)}{\eta}$$

$$\alpha_2^{new,unclipped}$$是无约束的$$W(\alpha_2)$$问题的最优值。如果考虑约束条件则有：

$$\alpha_2^{new}=\begin{cases}
H, & if \ \alpha_2^{new,unclipped}>H \\
\alpha_2^{new,unclipped}, & if \ L\le \alpha_2^{new,unclipped} \le H \\
L, & if \ \alpha_2^{new,unclipped}<L \\
\end{cases}$$

其中，$$\alpha_2^{new}$$是更新值。

由公式7可得：

$$\alpha_1+s\alpha_2=\alpha_1^{new}+s\alpha_2^{new}$$

$$\alpha_1^{new}=\alpha_1+s(\alpha_2-\alpha_2^{new})$$

在特殊情况下，$$\eta$$可能不为正，如果核函数K不满足Mercer定理，那么目标函数可能变得非正定，$$\eta$$可能出现负值。即使K是有效的核函数，如果训练样本中出现相同的特征x，那么$$\eta$$仍有可能为0。SMO算法在$$\eta$$不为正值的情况下仍有效。

当$$\eta\le 0$$时，W本身没有极值，极值出现在边界处，即$$\alpha_2^{new}=L$$或$$\alpha_2^{new}=H$$。我们需要对边界的W值进行检查。

这里首先对$$\alpha_2^{new}=L$$的情况做一下讨论。

$$\alpha_1^{new}=L_1=\alpha_1+s(\alpha_2-L)$$

由公式8可得：

$$\begin{align}
L_1-y^{(1)}L_1v_1&=L_1\left[(y^{(1)})^2-y^{(1)}(u_i-b-y^{(1)}\alpha_1K_{11}-y^{(2)}\alpha_2K_{21})\right]
\\&=L_1\left[y^{(1)}(y^{(1)}-u_i+b)+\alpha_1K_{11}+s\alpha_2K_{12}\right]
\\&=L_1\left[y^{(1)}(b-E_1)+\alpha_1K_{11}+s\alpha_2K_{12}\right]=L_1f_1
\end{align}$$

即

$$f_1=y^{(1)}(b-E_1)+\alpha_1K_{11}+s\alpha_2K_{12}$$

同理：

$$L-y^{(2)}Lv_2=Lf_2$$

$$f_2=y^{(2)}(b-E_2)+s\alpha_1K_{12}+\alpha_2K_{22}$$

由《机器学习（五）》中的公式5可得：

$$W_L=L_1f_1+Lf_2-\frac{1}{2}L_1^2K_{11}-\frac{1}{2}L^2K_{22}-sLL_1K_{12}$$

$$\alpha_2^{new}=H$$的情况，同理可得：

$$\alpha_1^{new}=H_1=\alpha_1+s(\alpha_2-H)$$

$$W_H=H_1f_1+Hf_2-\frac{1}{2}H_1^2K_{11}-\frac{1}{2}H^2K_{22}-sHH_1K_{12}$$

根据$$W_H$$和$$W_L$$哪个是极值，可以反过来确定$$\alpha_2^{new}=L$$，还是$$\alpha_2^{new}=H$$。

至此，迭代关系式除了b的推导式以外，都已经推出。

由公式8可得：

$$u_1^{new}-b^{new}-y^{(1)}\alpha_1^{new}K_{11}-y^{(2)}\alpha_2^{new}K_{12}=u_1-b-y^{(1)}\alpha_1K_{11}-y^{(2)}\alpha_2K_{12}$$

迭代的目的是使预测更为准确，因此设置$$u_1^{new}=y^{(1)}$$是很直观的做法，因此：

$$\begin{align}
-b^{new}&=u_1-u_1^{new}+y^{(1)}(\alpha_1^{new}-\alpha_1)K_{11}+y^{(2)}(\alpha_2^{new}-\alpha_2)K_{12}
\\&=(u_1-y^{(1)})+y^{(1)}(\alpha_1^{new}-\alpha_1)K_{11}+y^{(2)}(\alpha_2^{new}-\alpha_2)K_{12}
\\&=E_1+y^{(1)}(\alpha_1^{new}-\alpha_1)K_{11}+y^{(2)}(\alpha_2^{new}-\alpha_2)K_{12}
\end{align}$$

上式是根据$$u_1$$得到的$$b^{new}$$，我们将之记作$$b_1$$。

同理，可得：

$$-b_2=E_2+y^{(1)}(\alpha_1^{new}-\alpha_1)K_{12}+y^{(2)}(\alpha_2^{new}-\alpha_2)K_{22}$$

如果$$\alpha_1^{new}$$在边界内，则$$b^{new}=b_1$$；如果$$\alpha_2^{new}$$在边界内，则$$b^{new}=b_2$$。

如果$$\alpha_1^{new}$$和$$\alpha_2^{new}$$都在边界内，则$$b^{new}=b_1=b_2$$。

如果$$\alpha_1^{new}$$和$$\alpha_2^{new}$$都在边界上，则$$b_1$$和$$b_2$$之间的任意值都满足KKT条件，一般取$$b^{new}=\frac{b_1+b_2}{2}$$。

## SMO算法在线性SVM时的简化

由《机器学习（四）》一节的公式3，可得：

$$w^{new}-y^{(1)}\alpha_1^{new}x^{(1)}-y^{(2)}\alpha_2^{new}x^{(2)}=w-y^{(1)}\alpha_1x^{(1)}-y^{(2)}\alpha_2x^{(2)}$$

$$w^{new}=w+y^{(1)}(\alpha_1^{new}-\alpha_1)x^{(1)}+y^{(2)}(\alpha_2^{new}-\alpha_2)x^{(2)}$$

需要注意的是，虽然我们在之前的讨论中，使用$$K_{ij}$$代替$$\langle x^{(i)},x^{(j)}\rangle$$进行扩展，然而两者只有在线性的情况下，才是等价的，即$$\phi(x)=x$$。

K函数最大的作用不在于计算线性SVM，而在于计算非线性的SVM。

## SMO中拉格朗日乘子的启发式选择方法

所谓的启发式选择方法主要思想是每次选择拉格朗日乘子的时候，优先选择$$0<\alpha_i<C$$的$$\alpha_i$$作优化，因为边界上的$$\alpha_i$$在迭代之后通常不会更改。

给定初始值$$\alpha_i=0$$后，先对所有样例进行循环，循环中碰到违背KKT条件的（不管边界上还是边界内）都进行迭代更新。等这轮过后，如果没有收敛，第二轮就只针对$$0<\alpha_i<C$$的样例进行迭代更新。

软边距SVM问题的KKT条件为：

$$\alpha_i=0 \Leftrightarrow y^{(i)}u_i\ge 1$$

$$0<\alpha_i<C \Leftrightarrow y^{(i)}u_i=1$$

$$\alpha_i=C \Leftrightarrow y^{(i)}u_i\le 1$$

在第一个乘子选择后，第二个乘子也使用启发式方法选择， 第二个乘子的迭代步长大致正比于$$\lvert E_1-E_2\rvert$$，选择能使$$\lvert E_1-E_2\rvert$$最大的$$\alpha_2$$即可。

最后的收敛条件是在界内（$$0<\alpha_i<C$$）的样例都能够遵循KKT条件，且其对应的$$\alpha_i$$只在极小的范围内变动。

## 尾声

除了吴恩达的课程外，以下SVM系列课程也写得不错：

https://mp.weixin.qq.com/s/Ahvp0IAdgK9OVHFXigBk_Q

线性支持向量机（LSVM）

https://mp.weixin.qq.com/s/Q5bFR3vDDXPhtzXlVAE3Rg

对偶支持向量机（DSVM）

https://mp.weixin.qq.com/s/cLovkwwgGJRgSSa1XWZ8eg

核支持向量机（KSVM）

https://mp.weixin.qq.com/s/hfkWgBtSBKW8pT0bi62xzQ

一文详解SVM的Soft-Margin机制

https://mp.weixin.qq.com/s/rU8ijCdbnu4fvM1X2AxQUA

详解烧脑的Support Vector Regression

## 参考

https://mp.weixin.qq.com/s/pXhNRAvJI88tycMsrWhgcQ

机器学习中的算法：支持向量机(SVM)基础

http://www.jianshu.com/p/1aa67a321e33

SVM和Logistic Regression分别在什么时候使用？

https://mp.weixin.qq.com/s/5tUQ9B5juP-Vg8z-gp60rg

详解支持向量机SVM：快速可靠的分类算法

https://mp.weixin.qq.com/s/MfYRipBX4la5jEG-ZMBhEw

详解LinearSVM

https://mp.weixin.qq.com/s/AaTlJTWR3lWdx3_gGurVeQ

从大间隔分类器到核函数：全面理解支持向量机

https://www.zhihu.com/question/41066458

现在还有必要对SVM深入学习吗？

https://mp.weixin.qq.com/s?__biz=MzU0MDQ3MDk3NA==&mid=2247483671&idx=1&sn=0348314f21cb5be0f727054334f58445

libsvm中的svm-toy尝试

