---
layout: post
title:  深度学习（二十）——视频目标分割, Fast Image Processing, 图像超分辨率算法
category: DL 
---

## DenseNet（续）

### DenseNet的优化问题

当前的深度学习框架对DenseNet的密集连接没有很好的支持，我们只能借助于反复的拼接（Concatenation）操作，将之前层的输出与当前层的输出拼接在一起，然后传给下一层。对于大多数框架（如Torch和TensorFlow），每次拼接操作都会开辟新的内存来保存拼接后的特征。这样就导致一个L层的网络，要消耗相当于L(L+1)/2层网络的内存（第l层的输出在内存里被存了(L-l+1)份）。

解决这个问题的思路其实并不难，我们只需要预先分配一块缓存，供网络中所有的拼接层（Concatenation Layer）共享使用，这样DenseNet对内存的消耗便从平方级别降到了线性级别。

### 参考

https://www.leiphone.com/news/201708/0MNOwwfvWiAu43WO.html

CVPR 2017最佳论文作者解读：DenseNet 的“what”、“why”和“how”

https://zhuanlan.zhihu.com/p/28124810

为什么ResNet和DenseNet可以这么深？一文详解残差块为何有助于解决梯度弥散问题

https://mp.weixin.qq.com/s?__biz=MzI3MTA0MTk1MA==&mid=2651988934&idx=2&sn=0e5ffa195ef67a1371f3b5b223519121

ResNets、HighwayNets、DenseNets：用TensorFlow实现超深度神经网络

# 视频目标分割

视频目标分割任务和语义分割有两个基本区别：

1.视频目标分割任务分割的是一般的、非语义的目标；

2.视频目标分割添加了一个时序模块：它的任务是在视频的每一连续帧中寻找感兴趣目标的对应像素。

![](/images/article/Segmentation.png)

上图是Segmentation的细分，其中的每一个叶子都有一个示例数据集。

基于视频任务的特性，我们可以将问题分成两个子类：

无监督（亦称作视频显著性检测）：寻找并分割视频中的主要目标。这意味着算法需要自行决定哪个物体才是“主要的”。

半监督：在输入中（只）给出视频第一帧的正确分割掩膜，然后在之后的每一连续帧中分割标注的目标。

参考：

http://mp.weixin.qq.com/s/pGrzmq5aGoLb2uiJRYAXVw

一文概览视频目标分割

https://www.zhihu.com/question/52185576

视频中的目标检测与图像中的目标检测具体有什么区别？

# Fast Image Processing

![](/images/article/FIP.png)

上图是照片界常用的几种修图方式之一。一般将这些图片风格转换的算法，称为图像处理算子（image processing operators）。如何加速image processing operators的计算，就成为了学界研究的课题之一。

本文提出的模型就是用来加速image processing operators计算的。它是Intel Lab的Qifeng Chen和Jia Xu于2017年提出的。

论文：

《Fast Image Processing with Fully-Convolutional Networks》

代码：

https://github.com/CQFIO/FastImageProcessing

Demo网站：

http://cqf.io/ImageProcessing/

这个课题一般使用MIT-Adobe FiveK Dataset作为基准数据集。网址：

http://groups.csail.mit.edu/graphics/fivek_dataset/

这个数据集包含了5K张原始照片，并雇用了5个专业修图师，对每张图片进行修图。

众所周知，多层神经网络只要有足够的深度和宽度，就可以任意逼近任意连续函数。然而从Fast Image Processing的目的来说，神经网络的深度和宽度注定是有限的，否则肯定快不了。而这也是该课题的研究意义所在。

本文只使用了MIT-Adobe数据集中的原始图片，并使用了10种常用的算子对图片进行处理。因此，该网络训练时的输入是原始图片，而输出是处理后的图片。

![](/images/article/MCA.png)

上图是本文模型的网络结构图。它的设计特点如下：

1.采用Multi-Scale Context Aggregation作为基础网络。MCA的内容参见《深度学习（九）》。

2.传统MCA一般有下采样的过程，但这里由于网络输入和输出的尺寸维度是一样的，因此，所有的feature maps都是等大的。

3.借鉴FCN的思想，去掉了池化层和全连接层。

4.L1~L3主要用于图片的特征提取和升维，而L4~L5则用于特征的聚合和降维，并最终和输出数据的尺寸维度相匹配。

在normalization方面，作者发现有的operators经过normalization之后，精度会上升，而有的精度反而会下降，因此为了统一模型，定义如下的normalization运算：

$$\Psi^s(x)=\lambda_sx+\mu_sBN(x)$$

Loss函数为：

$$\mathcal{l(K,B)}=\sum_i\frac{1}{N_i}\|\hat f (I_i;\mathcal{K,B})-f(I_i)\|^2$$

这实际上就是RGB颜色空间的MSE误差。

为了检验模型的泛化能力，本文还使用RAISE数据集作为交叉验证的数据集。该数据集的网址：

http://mmlab.science.unitn.it/RAISE/

RAISE数据集包含了8156张高分辨率原始照片，由3台不同的相机拍摄，并给出了相机的型号和参数。

# 图像超分辨率算法

![](/images/img2/Super_Resolution.png)

如上图所示，一张低分辨率的小图（Low Resolution，LR）如果采用简单的插值算法进行图片放大的话，图像中物体的边缘会比较模糊。如何用算法将这种LR的图片放大成HR的图片，这就是Super Resolution（SR）的目标了。

SR目前主要有两个用途：

1.提升设备的测量精度。这个在天文和医疗图像方面用的比较多，比如Google和NASA利用AI探测太阳系外的行星，还有癌症的早期诊断。

2.Image Signal Processor。上面的两个应用比较高端，SR最主要的用途恐怕还是相机的ISP领域。ISP的基本概念参见《图像处理理论（五）》。

这里主要讨论DL在SR领域的应用。

## 前DL时代的SR

最简单的当然是《图像处理理论（二）》中提到的梯度锐化和拉普拉斯锐化，这种简单算法当然不要指望有什么好效果，聊胜于无而已。这是1995年以前的主流做法。

稍微复杂的方法，如同CV的其它领域经历了“信号处理->ML->DL”的变迁一样，SR也进入了ML阶段。

![](/images/img2/SR.png)

上图是两种典型的SR算法。

左图算法的中心思想是从图片中找出相似的大尺度区域，然后利用这个大区域的边缘信息进行SR。但这个方法对于那些只出现一次的边缘信息是没什么用的。

于是就有了右图的算法。对各种边缘信息建立一个数据库，使用时从数据库中挑一个最类似的边缘信息进行SR。这个方法比上一个方法好一些，但不够鲁棒，图片稍作改动，就有可能无法检索到匹配的边缘信息了。

ML时代的代表算法还有：

《Image Super-Resolution via Sparse Representation》

这篇论文是黄煦涛和马毅小组的Jianchao Yang的作品。

>黄煦涛（Thomas Huang），1936年生。生于上海，国立台湾大学本科（1956）+MIT硕博（1960,1963）。UIUC教授。美国工程院院士，中国科学院+中国工程院外籍院士。

>马毅，清华本科（1995）+UCB硕博（1997,2000）。UCB教授。IEEE fellow。   
>个人主页：   
>http://yima.csl.illinois.edu/

这篇论文提出的算法，在形式上和后文这些DL算法已经非常类似了，也是基于HR和LR配对的有监督训练。区别只在于这篇论文使用矩阵的稀疏表示来拟合SR函数，而DL算法使用神经网络拟合SR函数。前者是线性变换，而后者是非线性变换。

## SRCNN

SRCNN是汤晓鸥小组的Chao Dong的作品。

>汤晓鸥，中国科学技术大学本科（1990）+罗切斯特大学硕士（1991）+麻省理工学院博士（1996）。香港中文大学教授，商汤科技联合创始人。

论文：

《Learning a Deep Convolutional Network for Image Super-Resolution》

![](/images/article/SRCNN.jpg)

该方法对于一个低分辨率图像，先使用双三次（bicubic）插值将其放大到目标大小，再通过三层卷积网络做非线性映射，得到的结果作为高分辨率图像输出。作者将三层卷积的结构解释成与传统SR方法对应的三个步骤：图像块的提取和特征表示，特征非线性映射和最终的重建。

三个卷积层使用的卷积核的大小分为为9x9, 1x1和5x5，前两个的输出特征个数分别为64和32。

以下是论文的效果表格：

>吐槽一下，这种表格属于论文必须有，但是却没什么营养的部分，且不乏造假的例子。原因很简单，一个idea，如果没有好效果，paper连发都发不了。但是，没有好效果的idea，未必没有价值，不说是否能启发人们的思维，至少能让后来者，不用再掉到同一个坑里。   
>比如化学领域，失败的实验远远多于成功的实验。在计算能力不发达的时代，人们主要关注成功的案例，但现在大家逐渐意识到：失败的案例才是更大的财富。

![](/images/img2/SRCNN.jpg)

这里对其中的指标做一个简介。

**PSNR（Peak Signal to Noise Ratio，峰值信噪比）**

$$MSE=\frac{1}{H\times W}\sum_{i=1}^H\sum_{j=1}^W(X(i,j)-Y(i,j))^2$$

$$PSNR=10\log_{10}\left(\frac{(2^n-1)^2}{MSE}\right)$$

其中，MSE表示当前图像X和参考图像Y的均方误差（Mean Square Error），H、W分别为图像的高度和宽度；n为每像素的比特数，一般取8，即像素灰阶数为256. PSNR的单位是dB，数值越大表示失真越小。

虽然PSNR和人眼的视觉特性并不完全一致，但是一般认为PSNR在38以上的时候，人眼就无法区分两幅图片了。

**SSIM（structural similarity， 结构相似性）**，也是一种全参考的图像质量评价指标，它分别从亮度、对比度、结构三方面度量图像相似性。

$$\mu_X=\frac{1}{H\times W}\sum_{i=1}^H\sum_{j=1}^WX(i,j),\, \sigma_X^2=\frac{1}{H\times W}\sum_{i=1}^H\sum_{j=1}^W(X(i,j)-\mu_X)^2$$

$$\sigma_{XY}=\frac{1}{H\times W}\sum_{i=1}^H\sum_{j=1}^W((X(i,j)-\mu_X)(Y(i,j)-\mu_Y))$$

$$l(X,Y)=\frac{2\mu_X\mu_Y+C_1}{\mu_X^2+\mu_Y^2+C_1},\, c(X,Y)=\frac{2\sigma_X\sigma_Y+C_2}{\sigma_X^2+\sigma_Y^2+C_2},\, s(X,Y)=\frac{\sigma_{XY}+C_3}{\sigma_X\sigma_Y+C_3}$$

$$SSIM(X,Y)=l(X,Y)\cdot c(X,Y)\cdot s(X,Y)$$

$$C_1,C_2,C_3$$为常数，为了避免分母为0的情况，通常取$$C_1=(K_1\cdot L)^2, C_2=(K_2\cdot L)^2, C_3=C_2/2$$，一般地$$K1=0.01, K2=0.03, L=255$$。

SSIM取值范围[0,1]，值越大，表示图像失真越小。

在实际应用中，可以利用滑动窗将图像分块，令分块总数为N，考虑到窗口形状对分块的影响，采用高斯加权计算每一窗口的均值、方差以及协方差，然后计算对应块的结构相似度SSIM，最后将平均值作为两图像的结构相似性度量，即平均结构相似性MSSIM：

$$MSSIM(X,Y)=\frac{1}{N}\sum_{k=1}^NSSIM(x_k,y_k)$$

参考：

http://www.cnblogs.com/vincent2012/archive/2012/10/13/2723152.html

PSNR和SSIM

## DRCN

![](/images/img2/DRCN.jpg)

论文：

《Deeply-Recursive Convolutional Network for Image Super-Resolution》

## VDSR

论文：

《Accurate Image Super-Resolution Using Very Deep Convolutional Networks》

![](/images/img2/VDSR.png)



