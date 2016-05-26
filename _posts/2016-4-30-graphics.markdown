---
layout: post
title:  图像处理理论, UPNP（二）, linux学习心得（二）
category: technology 
---

# 图像处理理论

## 对比度和亮度

$$
g(i,j)=a\times f(i,j)+b
$$

上式中$$f(i,j)$$和$$g(i,j)$$表示位于第i行，第j列的像素。上述线性变换中，a表示对比度，b表示亮度。

## 邻域

$$\left[ \begin{array}{ccc} A_0&A_1&A_2\\ A_3&A&A_4 \\ A_5&A_6&A_7\end{array} \right]$$

$$A_0$$~$$A_7$$被称作像素A的1度8-邻域(即$$U(A,1)$$)，相应的上下左右的四个像素$$A_1$$、$$A_3$$、$$A_4$$、$$A_6$$被称作像素A的1度4-邻域。下文如无特别指出，邻域均为8-邻域。

定义$$U^+(A,N)=A\bigcup\limits_{i=1}^N U(A,i)$$。

$$U(A,2)$$的定义如下：

如果$$B\in U(A,1)\land C\in U(B,1)\land C\notin U^+(A,1)$$，那么$$C\in U(A,2)$$。

类似的$$U(A,N)$$的定义为：

如果$$B\in U(A,N-1)\land C\in U(B,1)\land C\notin U^+(A,N-1)$$，那么$$C\in U(A,N)$$。

这里的N被称为度数，也就是两点间的距离，即$$L(A,C)=N$$。

## 相关算子

相关（Correlation)算子

$$g=f\bigotimes h$$

的定义为：

$$g(i,j)=\sum_{k,l}f(i+k,j+l)h(k,l)$$

其中，h称为相关核(Kernel)，即滤波器的加权系数矩阵。相关核有个叫做锚点（anchor）的属性，也就是被滤波的那个点在核中的位置。以3*3的h矩阵为例，如果锚点在矩阵中央的话，则$$i-1\le k\le i+1,j-1\le l\le j+1$$。如果锚点在左上角的话，则$$i\le k\le i+2,j\le l\le j+2$$。

所有的点$$(k,l)$$组成的集合，叫做核空间。一般也简单记作$$X_{k,l}$$。这里的X可以是累加、求均值、求最大值、求最小值等集合运算符。

此外，h矩阵还有是否归一化的属性。这里将计算矩阵中所有元素之和的操作，记作$$SUM(h)$$.则当$$SUM(h)=1$$时，h为归一化核。$$\frac{h}{SUM(h)}$$，称作核的归一化。

## 卷积算子

卷积（Convolution)算子

$$g=f\ast h$$

的定义为：

$$g(i,j)=\sum_{k,l}f(i-k,j-l)h(k,l)$$

显然

$$f\ast h=f\bigotimes rot180(h)$$

其中，rotN表示将矩阵元素绕中心逆时针旋转N度，显然这里的N只有为90的倍数，才是有意义的。

## 方框滤波（Box Filter）

$$h=\alpha
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

## 高斯滤波（Gauss filter）

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

标准正态分布的最佳逼近符合杨辉三角，比如一个具有5个点的一维正态分布的最佳逼近为：

$$\left[ \begin{array}{ccccc} 1&4&6&4&1\end{array} \right]$$

同理，最常用的3*3高斯滤波h矩阵为：

$$\left[ \begin{array}{ccc} 1&1&1\\ 1&2&1 \\ 1&1&1\end{array} \right]$$

其归一化形式为：

$$\left[ \begin{array}{ccc} 0.1&0.1&0.1\\ 0.1&0.2&0.1 \\ 0.1&0.1&0.1\end{array} \right]$$

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

## 膨胀与腐蚀(Dilation & Erosion)

腐蚀和膨胀是对白色部分（高亮部分）而言的，不是黑色部分。膨胀就是图像中的高亮部分进行膨胀，“领域扩张”，效果图拥有比原图更大的高亮区域。腐蚀就是原图中的高亮部分被腐蚀，“领域被蚕食”，效果图拥有比原图更小的高亮区域。

具体方法如下：

1.膨胀。

$$g(i,j)=max_{k,l}f(i,j)$$

2.腐蚀。

$$g(i,j)=min_{k,l}f(i,j)$$

这里仿照C语言的记法，将膨胀操作记为$$dilate(src)$$，其中src表示源图像。同理，将腐蚀操作记为$$erode(src)$$。

效果如下：

![](/images/article/dilate_erode.png)

## 高级形态学操作

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

# UPNP（二）

# 自制的Control Point示例（续）

## Step 1

这次，我打算从头开始搭建Control Point示例。也就是从main函数出发，逐步完善相关功能。这一步的代码在：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/helloworld/upnp/step1

该程序主要实现：

1.基本框架。包括初始化和注册Control Point。

2.通过SSDP的搜索功能，搜索网络设备。

## Step 2

这一步的代码在：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/helloworld/upnp/step2

# linux学习心得（二）

## 查看内存使用情况

### top命令

top命令可在进程这一级查看内存、运行时间、CPU等的使用情况。并可根据不同属性对结果排序：

P：按%CPU使用率排序

T：按TIME+排序

M：按%MEM排序

注：运行top命令之后，输入相应字符即可切换排序。

### free命令

free命令的内容比较概括，主要包含系统内存的整体使用情况，不深入到进程一级。

## 时间的表示方法

一般遵循ISO 8601标准：

https://www.w3.org/TR/NOTE-datetime

YYYY-MM-DDThh:mm:ss.sTZD (eg 1997-07-16T19:20:30.45+01:00)

其中的TZD表示time zone designator。
