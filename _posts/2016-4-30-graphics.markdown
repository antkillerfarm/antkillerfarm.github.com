---
layout: post
title:  图像处理理论（一）
category: technology 
---

# 对比度和亮度

$$
g(i,j)=a\times f(i,j)+b
$$

上式中$$f(i,j)$$和$$g(i,j)$$表示位于第i行，第j列的像素。上述线性变换中，a表示对比度，b表示亮度。

# 邻域

$$\left[ \begin{array}{ccc} A_0&A_1&A_2\\ A_3&A&A_4 \\ A_5&A_6&A_7\end{array} \right]$$

$$A_0$$~$$A_7$$被称作像素A的1度8-邻域(即$$U(A,1)$$)，相应的上下左右的四个像素$$A_1$$、$$A_3$$、$$A_4$$、$$A_6$$被称作像素A的1度4-邻域。下文如无特别指出，邻域均为8-邻域。

定义$$U^+(A,N)=A\bigcup\limits_{i=1}^N U(A,i)$$。

$$U(A,2)$$的定义如下：

如果$$B\in U(A,1)\land C\in U(B,1)\land C\notin U^+(A,1)$$，那么$$C\in U(A,2)$$。

类似的$$U(A,N)$$的定义为：

如果$$B\in U(A,N-1)\land C\in U(B,1)\land C\notin U^+(A,N-1)$$，那么$$C\in U(A,N)$$。

这里的N被称为度数，也就是两点间的距离，即$$L(A,C)=N$$。

# 相关算子

相关（Correlation)算子

$$g=f\otimes h$$

的定义为：

$$g(i,j)=\sum_{k,l}f(i+k,j+l)h(k,l)$$

其中，h称为相关核(Kernel)，即滤波器的加权系数矩阵，有的书上也称作“模板”。相关核有个叫做锚点（anchor）的属性，也就是被滤波的那个点在核中的位置。以3*3的h矩阵为例，如果锚点在矩阵中央的话，则$$i-1\le k\le i+1,j-1\le l\le j+1$$。如果锚点在左上角的话，则$$i\le k\le i+2,j\le l\le j+2$$。

所有的点$$(k,l)$$组成的集合，叫做核空间。一般也简单记作$$X_{k,l}$$。这里的X可以是累加、求均值、求最大值、求最小值等集合运算符。

此外，h矩阵还有是否归一化的属性。这里将计算矩阵中所有元素之和的操作，记作$$SUM(h)$$.则当$$SUM(h)=1$$时，h为归一化核。$$\frac{h}{SUM(h)}$$，称作核的归一化。

# 卷积算子

卷积（Convolution)算子

$$g=f\ast h$$

的定义为：

$$g(i,j)=\sum_{k,l}f(i-k,j-l)h(k,l)$$

显然

$$f\ast h=f\otimes rot180(h)$$

其中，rotN表示将矩阵元素绕中心逆时针旋转N度，显然这里的N只有为90的倍数，才是有意义的。

# 方框滤波（Box Filter）

$$g=f\otimes h,h=\alpha
\begin{bmatrix}
     1 & 1 & 1 & \cdots & 1 \\
     1 & 1 & 1 & \cdots & 1 \\
     \cdots & \cdots & \cdots & \cdots & \cdots \\
     1 & 1 & 1 & \cdots & 1    
\end{bmatrix}
,\alpha =
\begin{cases}
\frac{1}{SUM(h)},  & \text{normalize=true} \\
1, & \text{normalize=false}  \\
\end{cases}
$$

当normalize=true时的方框滤波，也被称为均值滤波（Mean filter）。

# 高斯滤波（Gauss filter）

高斯平滑滤波器对于抑制服从正态分布的噪声非常有效。

正态分布的概率密度函数为：

$$f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$

其标准化后的概率密度函数为：

$$f(x)=\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}$$

标准二维正态分布的概率密度函数为：

$$f(x,y)=\frac{1}{2\pi}e^{-\frac{x^2+y^2}{2}}=f(x)f(y)$$

这个公式表明标准二维正态分布，可以分解为两个正交方向上的标准一维正态分布。也就是说标准二维正态分布不仅是中心对称，也是轴对称的。

正态分布的性质：

1.两个正态分布密度的乘积、卷积，还是正态分布。

2.正态分布的傅立叶变换、共轭分布，还是正态分布。

3.正态分布和其它具有相同均值、方差的概率分布相比，具有最大熵。

4.二项分布、泊松分布、$$\chi^2$$分布、t分布等在样本增大的情况下，都趋向于正态分布。

正态分布的相关内容可参考：

[正态分布的前世今生](http://www.52nlp.cn/%E6%AD%A3%E6%80%81%E5%88%86%E5%B8%83%E7%9A%84%E5%89%8D%E4%B8%96%E4%BB%8A%E7%94%9F%E4%B8%80)

标准正态分布的最佳逼近符合杨辉三角，比如一个具有5个点的一维标准正态分布的最佳逼近为：

$$\left[ \begin{array}{ccccc} 1&4&6&4&1\end{array} \right]$$

同理，最常用的3*3高斯滤波h矩阵为：

$$\left[\begin{array}{c} 1\\2\\1\end{array} \right]\times \left[\begin{array}{ccc} 1&2&1\end{array} \right]=\left[\begin{array}{ccc} 1&2&1\\ 2&4&2 \\ 1&2&1\end{array} \right]$$

其归一化形式为：

$$\left[\begin{array}{ccc} 0.0625&0.125&0.0625\\ 0.125&0.25&0.125 \\ 0.0625&0.125&0.0625\end{array}\right]$$

从效果来说，高斯滤波可产生类似毛玻璃的效果。

## 中值滤波（Median filter）

中值滤波是一种典型的非线性滤波技术，对于斑点噪声（speckle noise）和椒盐噪声（salt-and-pepper noise）来说尤其有用，对滤除脉冲干扰及图像扫描噪声非常有效，也常用于保护边缘信息。

以3*3的滤波窗口为例，计算以点$$[i,j]$$为中心的像素中值步骤如下：

1）对$$U^+(A,1)$$的9个像素点$$a_0\sim a_8$$，按强度值大小排列像素点，得到有序数组$$A_0\sim A_8$$

2）选择排序像素集的中间值$$A_4$$作为点$$[i,j]$$的新值。

从效果来说，中值滤波可产生类似油彩画的效果。

上面这些滤波的效果如图所示：

![](/images/article/img_filter.png)

## 双边滤波（Bilateral filter）

双边滤波（Bilateral filter）是一种可以保边去噪的滤波器。其输出像素的值依赖于邻域像素的值的加权组合，即：

$$g(i,j)=\frac{\sum_{k,l}f(k,l)w(i,j,k,l)}{\sum_{k,l}w(i,j,k,l)}$$

也就是：

$$h=\frac{w(i,j,k,l)}{\sum_{k,l}w(i,j,k,l)}$$

其中，

$$w(i,j,k,l)=d(i,j,k,l)\cdot r(i,j,k,l)=exp\left(-\frac{(i-k)^2+(j-l)^2}{2\sigma_d^2}\right)\cdot exp\left(-\frac{\|f(i,j)-f(k,l)\|^2}{2\sigma_r^2}\right)$$

这里的$$d(i,j,k,l)$$由于只与定义域有关，通常叫做“定义域核”。实际上，这就是一个高斯滤波核。而$$r(i,j,k,l)$$由于和像素值的差有关（像素差越大，权重越小），也被叫做“值域核”。

从效果来说，双边滤波可产生类似美肤的效果。皮肤上的皱纹和斑，与正常皮肤的差异，远小于黑白眼珠之间的差异，因此前者被平滑，而后者被保留。

为了体现效果，这里来张大叔的照片。

![](/images/article/bilateral_filter.png)

## Steerable滤波

高斯滤波是一种各向同性滤波，如果想要对特定方向进行滤波的话，可使用Steerable滤波。

对最简二维高斯函数$$G(x,y)=e^{-(x^2+y^2)}$$求1阶偏导可得：

$$G_1^0=\frac{\partial G(x,y)}{\partial x}=-2xe^{-(x^2+y^2)},G_1^{\frac{\pi}{2}}=\frac{\partial G(x,y)}{\partial y}=-2ye^{-(x^2+y^2)}$$

这就是两个轴向上的1阶Steerable滤波函数。

任意角度的1阶Steerable滤波函数为：

$$G_1^{\theta}=cos\theta G_1^0+sin\theta G_1^{\frac{\pi}{2}}$$

如果对高斯函数求2阶偏导,还可得到2阶Steerable滤波函数。进一步的讨论详见参考论文。

参考：

1991年IEEE论文：The Design and Use of Steerable Filters

作者：William T. Freeman，斯坦福大学本科+斯坦福/康奈尔大学双料硕士+麻省理工学院博士，麻省理工学院教授。1987年，曾做为访问学者在太原理工大学待了一学年。不知道爱不爱吃刀削面(^ω^)

Edward H. Adelson，密歇根大学博士，麻省理工学院教授。

## Gabor滤波

一般的函数可以展开为幂级数或者Fourier级数。这些级数中的幂函数或者正弦函数，被称作“基(basis)函数”。

基的属性主要涉及“线性无关”和“正交”这两个名词。

**线性无关**的几何含义：在$$R^3$$(3维空间)中，如果三个向量不共面，则它们相互线性无关。

基如果线性无关，则其函数的级数展开式是唯一的。由于线性相关基使用的比较少，以下如无特指，基均为线性无关基。

**正交**的几何含义：两个向量正交，则它们是相互垂直的。

正交基一定线性无关，反之则不成立。一般采用施密特正交化方法，将线性无关基，转换为正交基。

幂级数是线性无关基，而Fourier级数是正交基。

除了以上两种常用的基函数外，其他函数也可以作为基函数。其中使用最多的基函数是小波(wavelet)函数，其变换也被称作小波变换。

需要指出的是，小波函数不是一个函数，而是一类函数。Gabor函数就是小波函数的其中一种，其定义如下：

$$g_{t,n}(x)=g(x-al)e^{2\pi ibnx},-\infty<l,n<+\infty$$

这里的$$a,b$$为常数，$$g$$为$$L^2(R)$$(立方可积函数)，且$$\parallel g\parallel=1$$。

注：Dennis Gabor（1900~1979），全息学创始人，1971年获诺贝尔物理学奖，著有《Theory of Communication》（1946）。

当$$g$$为高斯函数时，可得到Gabor wavelet：

$$f(x)=e^{-(x-x_0)^2/a^2}e^{-ik_0(x-x_0)}$$

Gabor wavelet的性质：

1.Gabor wavelet不是正交基。

2.Gabor wavelet的Fourier变换还是Gabor wavelet：

$$F(k)=e^{-(k-k_0)^2a^2}e^{-ix_0(k-k_0)}$$

3.从物理上来说，Gabor wavelet等效于在一个正弦载波上，调制一个高斯函数。这也是Dennis Gabor最早提出它的时候的用途。

4.Fourier变换是信号在整个时域内的积分，因此反映的是信号频率的统计特性，没有局部化分析信号的功能。而Gabor变换是一种短时Fourier变换。

将Gabor wavelet扩展到2维，可得到Gabor filter：

$$g(x,y;\lambda,\theta,\psi,\sigma,\gamma)=exp\left(-\frac{x'^2+\gamma^2y'^2}{2\sigma^2}\right)exp\left(i\left(2\pi\frac{x'}{\lambda}+\psi\right)\right)$$

其中，

$$x'=xcos\theta+ysin\theta,y'=-xsin\theta+ycos\theta$$

可以看出Gabor filter是一个复函数，其实部为：

$$g(x,y;\lambda,\theta,\psi,\sigma,\gamma)=exp\left(-\frac{x'^2+\gamma^2y'^2}{2\sigma^2}\right)cos\left(2\pi\frac{x'}{\lambda}+\psi\right)$$

其虚部为：

$$g(x,y;\lambda,\theta,\psi,\sigma,\gamma)=exp\left(-\frac{x'^2+\gamma^2y'^2}{2\sigma^2}\right)sin\left(2\pi\frac{x'}{\lambda}+\psi\right)$$

1987年，J.P. Jones和L.A. Palmer发现Gabor滤波器可以很好地近似单细胞的感受野函数（光强刺激下的传递函数）。

此外，还有对数Gabor函数：

$$G(f)=exp\left(\frac{-(log(f/f_0))^2}{2(log(\sigma/f_0))^2}\right)$$

参考：

1996年IEEE论文：Image Representation Using 2D Gabor Wavelets

作者：Tai Sing Lee，哈佛大学博士，卡内基梅隆大学教授。

# 膨胀与腐蚀(Dilation & Erosion)

腐蚀和膨胀是对白色部分（高亮部分）而言的，不是黑色部分。膨胀就是图像中的高亮部分进行膨胀，“领域扩张”，效果图拥有比原图更大的高亮区域。腐蚀就是原图中的高亮部分被腐蚀，“领域被蚕食”，效果图拥有比原图更小的高亮区域。

具体方法如下：

1.膨胀。

$$g(i,j)=max_{k,l}f(i,j)$$

2.腐蚀。

$$g(i,j)=min_{k,l}f(i,j)$$

这里仿照C语言的记法，将膨胀操作记为$$dilate(src)$$，其中src表示源图像。同理，将腐蚀操作记为$$erode(src)$$。

效果如下：

![](/images/article/dilate_erode.png)

# 高级形态学操作

1.开运算（Opening Operation）

$$open(src)=dilate(erode(src))$$

开运算可以用来消除小物体、在纤细点处分离物体、平滑较大物体的边界的同时并不明显改变其面积。

2.闭运算(Closing Operation)

$$close(src)=erode(dilate(src))$$

闭运算能够排除小型黑洞(黑色区域)。

3.形态学梯度（Morphological Gradient）

$$morphgrad(src)=dilate(src)-erode(src)$$

对二值图像进行这一操作可以将团块（blob）的边缘突出出来。我们可以用形态学梯度来保留物体的边缘轮廓。

4.顶帽（Top Hat）

$$tophat(src)=src-open(src)$$

顶帽运算往往用来分离比邻近点亮一些的斑块。当一幅图像具有大幅的背景的时候，而微小物品比较有规律的情况下，可以使用顶帽运算进行背景提取。

5.黑帽（Black Hat）

$$blackhat(src)=close(src)-src$$

黑帽运算后的效果图突出了比原图轮廓周围的区域更暗的区域。

效果如下：

![](/images/article/morphology.png)

