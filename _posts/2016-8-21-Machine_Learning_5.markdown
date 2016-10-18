---
layout: post
title:  机器学习（五）——SVM（3）
category: technology 
---

## 规则化和不可分情况处理（续）

这时候应该允许一些点游离并在模型中违背限制条件（函数间隔大于1）。我们设计得到新的模型如下（也称软间隔（Soft-margin））：

$$\begin{align}
&\operatorname{min}_{\gamma,w,b}& & \frac{1}{2}\|w\|^2+C\sum_{i=1}^m\xi_i\\
&\operatorname{s.t.}& & y^{(i)}(w^Tx^{(i)}+b)\ge 1-\xi_i,i=1,\dots,m\\
& & & \xi_i\ge 0,i=1,\dots,m
\end{align}$$

这里的C是离群点的权重，C越大表明离群点对目标函数影响越大，也就是越不希望看到离群点。我们看到，目标函数控制了离群点的数目和程度，使大部分样本点仍然遵守限制条件。

模型修改后，拉格朗日公式修改如下：

$$\mathcal{L}(w,b,\xi,\alpha,r)=\frac{1}{2}w^Tw+C\sum_{i=1}^m\xi_i-\sum_{i=1}^m\alpha_i[y^{(i)}(w^Tx^{(i)}+b)-1+\xi_i]-\sum_{i=1}^mr_i\xi_i$$

其对偶优化问题为：

$$\begin{align}
&\operatorname{max}_\alpha & & W(\alpha)=\sum_{i=1}^m\alpha_i-\frac{1}{2}\sum_{i,j=1}^my^{(i)}y^{(j)}\alpha_i\alpha_j\langle x^{(i)},x^{(j)}\rangle\\
&\operatorname{s.t.}& & 0\le\alpha_i\le C,i=1,\dots,m\\
& & & \sum_{i=1}^m\alpha_iy^{(i)}=0
\end{align}$$

此时，我们发现没有了参数$$\xi_i$$，与之前模型唯一不同在于$$\alpha_i$$多了$$\alpha_i\le C$$的限制条件。需要注意的是，b的求值公式发生了改变，这将在SMO算法里面介绍。

## 坐标上升法

$$\begin{align}
&\operatorname{max}_\alpha & & W(\alpha_1,\dots,\alpha_m)
\end{align}$$

对于上面这个优化问题，除了之前介绍的梯度下降法和牛顿法之外，还有坐标上升法(Coordinate ascent)。其过程为：

>Loop until convergence:{   
><span style="white-space: pre">	</span>for i=1 to m, {   
><span style="white-space: pre">	    </span>$$\alpha_i:=\arg\max_{\hat\alpha_i}W(\alpha_1,\dots,\alpha_{i-1},\hat\alpha_i,\alpha_{i+1},\dots,\alpha_m)$$   
><span style="white-space: pre">	</span>}   
>}

最里面语句的意思是固定除$$\alpha_i$$之外的所有$$\alpha_j(j\neq i)$$,这时W可看作只是关于$$\alpha_i$$的函数，那么直接对$$\alpha_i$$求导优化即可。这里我们进行最大化求导的顺序是从1到m,可以通过更改优化顺序来使W能够更快地增加并收敛。如果W在内循环中能够很快地达到最优,那么坐标上升法会是一个很高效的求极值方法。

![](/images/article/SVM_7.png)

图中的直线表示迭代优化的路径，可以看到每一步前进路线都是平行于坐标轴的，因为每一步只优化一个变量。

## 序列最小优化方法

序列最小优化方法（Sequential Minimal Optimization，SMO）算法由Microsoft Research的John C. Platt在1998年提出，并成为最快的二次规划优化算法，特别针对SVM和数据稀疏时性能更优。关于SMO最好的资料就是他本人写的《Sequential Minimal Optimization A Fast Algorithm for Training Support Vector Machines》。

>注：John Carlton Platt，1963年生，14岁进入加州州立大学长滩分校，加州理工学院博士。先后供职于Synaptics和Microsoft Research，现为Google首席科学家。

针对之前列出的SVM对偶优化问题：

$$\begin{align}
&\operatorname{max}_\alpha & & W(\alpha)=\sum_{i=1}^m\alpha_i-\frac{1}{2}\sum_{i,j=1}^my^{(i)}y^{(j)}\alpha_i\alpha_j\langle x^{(i)},x^{(j)}\rangle \tag{1}\\
&\operatorname{s.t.}& & 0\le\alpha_i\le C,i=1,\dots,m \tag{2}\\
& & & \sum_{i=1}^m\alpha_iy^{(i)}=0 \tag{3}
\end{align}$$

我们可以考虑使用坐标上升法，即首先固定除$$\alpha_1$$以外的所有参数,然后在$$\alpha_1$$上求极值。然而由于约束3的存在，把除$$\alpha_1$$以外的所有参数固定，实际上也就把$$\alpha_1$$给固定了。因此，需要一定的技巧来处理，比如:

>Repeat till convergence:{   
><span style="white-space: pre">	</span>1.挑选一对$$\alpha_i$$和$$\alpha_j$$（挑选的规则可以是启发式的，以收敛快为准则）   
><span style="white-space: pre">	</span>2.固定除$$\alpha_i$$和$$\alpha_j$$之外的其余参数，以确定W极值条件下的$$\alpha_i$$和$$\alpha_j$$   
>}

这里假设我们固定$$\alpha_3,\dots,\alpha_m$$，来优化$$\alpha_1$$和$$\alpha_2$$。由约束3可得：

$$\alpha_1y^{(1)}+\alpha_2y^{(2)}=-\sum_{i=3}^m\alpha_iy^{(i)}=\zeta\tag{4}$$

因为$$\alpha_3,\dots,\alpha_m$$的值已经固定，所以$$\zeta$$实际上是个常数。

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

