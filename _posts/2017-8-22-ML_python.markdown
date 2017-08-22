---
layout: post
title:  Machine Learning之Python篇, OpenVX, NVIDIA
category: technology 
---

# Machine Learning之Python篇

## 概述

![](/images/article/python_for_big_data.jpg)

## Anaconda

Anaconda是一个科学计算方面的python发行版，下面提到的所有工具都可以通过Anaconda一站式安装。

官网：

https://www.continuum.io/downloads

## NumPy

NumPy是python语言所有数学计算库的基础。它主要提供了矩阵运算的功能。

官网：

http://www.numpy.org/

教程：

https://docs.scipy.org/doc/numpy-dev/user/quickstart.html

API参考：

https://docs.scipy.org/doc/numpy/reference/

## SciPy

SciPy提供了一些更高阶的数学运算库，比如：积分、插值、信号处理、傅立叶变换、矩阵特征值、统计计算等。

SciPy提供的功能主要仍局限于数学运算，而并未提升到算法的层面。这也是它和scikit-learn或其他高级库的差别所在。

官网：

http://www.scipy.org/

API参考：

https://docs.scipy.org/doc/scipy/reference/

## Scikit-learn

Scikit-learn提供了常见的机器学习算法的实现。

官网：

http://scikit-learn.org/stable/index.html

教程：

http://scikit-learn.org/stable/user_guide.html

API参考：

http://scikit-learn.org/stable/tutorial/index.html

http://scikit-learn.org/stable/modules/classes.html

## Matplotlib

Matplotlib是一个高阶的图形库，主要提供生成图表等数据可视化方面的功能。

官网：

http://matplotlib.org/

API参考：

http://matplotlib.org/1.5.3/api/index.html

## Pandas

Pandas是一个数据分析方面的工具库。它提供的Series(1-dimensional)和DataFrame(2-dimensional)数据结构，可以提供类似sql的数据操作和查询的功能。

官网：

http://pandas.pydata.org/

文档：

http://pandas.pydata.org/pandas-docs/stable/

API参考：

http://pandas.pydata.org/pandas-docs/stable/api.html

参考：

http://www.cnblogs.com/chaosimple/p/4153083.html

十分钟搞定pandas

http://pandas.pydata.org/pandas-docs/stable/comparison_with_sql.html

Pandas和SQL的比较

## mysql

http://www.runoob.com/python/python-mysql.html

## chainer

chainer是一个日本人写的基于python的深度学习框架。

官网：

https://chainer.org/

代码：

https://github.com/chainer/chainer

## iPython

ipython是一个python的交互式 shell，比默认的python shell 好用得多，支持变量自动补全，自动缩进，支持 bash shell 命令，内置了许多很有用的功能和函数。

在较新的ipython版本中，添加了ipython notebook的功能，弥补了ipython shell下代码不易保存等缺点，并且在使用--pylab inline选项后，可以在代码执行后立即显示运行结果（包括图片，数据表格等），因此在数据分析中运用十分广泛。

`sudo apt-get install ipython ipython-notebook`

## Jupyter

Jupyter是iPython的后继项目，它不仅支持python语言，还支持其他50多种交互式语言。成为目前最流行的交互式shell和数据文本交换格式。

官网：

https://jupyter.org/

`pip install jupyter`

参见：

https://mp.weixin.qq.com/s/UXlPhX3Vb2yqocpUH_3W5w

Jupyter项目的前世今生

## 参考

https://github.com/neozhaoliang/pywonderland/tree/master/

如何用Python画各种著名数学图案

http://old.sebug.net/paper/books/scipydoc/index.html

用Python做科学计算

https://pan.baidu.com/s/1qYPUHNQ

小抄合集

https://mp.weixin.qq.com/s/HKHLiCqEAtoCAqf4CHv86w

python机器学习算法代码实现

https://mp.weixin.qq.com/s/1--i1OhxRNPvNnmP9bFo3g

全机器学习和Python的27个速查表

https://mp.weixin.qq.com/s/PtZ3YvFrUgbK5y3pcAeW9A

使用OpenCV和Python实现的机器学习

https://mp.weixin.qq.com/s/wbcAIrI0eNALfKhgkZQYTQ

用Python也能进军金融领域？这有一份股票交易策略开发指南

https://mp.weixin.qq.com/s/e--IeRTRZMqhs_DSJKpgyQ

特征工程之Scikit-learn

https://mp.weixin.qq.com/s/z3N5v4H_W2sxh2XPpFbAcA

除了Python，这些语言写的机器学习项目也很牛

https://mp.weixin.qq.com/s/U2cDVRxZPVsqoL-zHn__CA

Python做机器学习之路

https://mp.weixin.qq.com/s/LgVS5N80UlCeEfrPtyUF4Q

深度学习矩阵运算的概念和代码实现

https://mp.weixin.qq.com/s/0XteuIk71qSpxrZPGVnMbg

Python3实现K-近邻算法

https://mp.weixin.qq.com/s/bGudwxu8c0LIxv1h64qZtQ

Python3实现决策树算法

https://mp.weixin.qq.com/s?__biz=MzA3MTM3NTA5Ng==&mid=2651056396&idx=1&sn=dabbda2c433e54a2bad506fc13bfd743

<<战狼Ⅱ>>豆瓣十二万影评浅析

https://mp.weixin.qq.com/s/fXI5suCVna6fBxPnVyKevw

浅谈NumPy和Pandas库

# OpenVX

## Khronos Group

Khronos Group是一个行业组织，创建开放标准以实现并行计算、图形、视觉、传感处理和动态媒体在各种平台和设备上的编写和加速。Khronos标准包括 Vulkan, OpenGL, OpenGL ES, WebGL, OpenCL, SPIR, SYCL, WebCL, OpenVX, EGL, OpenMAX, OpenVG, OpenSL ES, StreamInput, COLLADA 和 glTF。

简单来说，Khronos的任务就是创建一个统一的硬件和软件之间的API，这样无论软件厂商，还是硬件厂商，都能各行其道，互不干扰了。

## 概述

官网：

https://www.khronos.org/openvx/

![](/images/article/openvx.png)

上图给出了OpenVX的主要用途，以及它和Khronos其他兄弟项目之间的关系。

OpenVX本身也是一个系列标准。它包括：

**OpenVX**：一个传统的CV接口。提供包括直方图、Harris、Canny等特征算子的API。

**OpenVX SC（Safety Critical）**：安全版的OpenVX。

**OpenVX NN Extension**：专门用于提供NN加速方面的API。目前主要集中于CNN的加速，即卷积、池化等操作，对其他NN支持有限。此外，这些API主要用于预测，而非训练。

Khronos官方提供了一个OpenVX的软件参考实现，用于软硬件厂商的测试工作。

相关API文档和参考实现（sample code）参见：

https://www.khronos.org/registry/OpenVX/

## NNEF

Neural Network Exchange Format是Khronos制定的用于交换NN模型数据的数据格式标准。

官网：

https://www.khronos.org/nnef

## OpenCL

官网：

https://www.khronos.org/opencl/

按照Michael J. Flynn的分类方法，计算机的体系结构可分为如下四类：

**Single instruction stream single data stream (SISD)**

**Single instruction stream, multiple data streams (SIMD)**

**Multiple instruction streams, single data stream (MISD)**

**Multiple instruction streams, multiple data streams (MIMD)**

![](/images/article/simd_mimd.png)

原图地址：

https://en.wikipedia.org/wiki/Flynn%27s_taxonomy

CPU通常是SISD和SIMD的，而GPU则是MISD的，超级计算机则是MIMD的。

OpenCL是一个硬件中立标准，原则上和计算机的体系结构无关。当然现实中，我们主要使用GPU进行运算加速。

和OpenGL、OpenVX的专用性不同，OpenCL主要定位于通用数学运算。OpenGL年代久远也就罢了。对于像OpenVX这样的新标准，有的时候其内部实现也有可能依赖于OpenCL。毕竟无论哪个领域的专用计算，最终都可以分解为基本的数学运算。

参考：

http://blog.csdn.net/leonwei/article/details/8880012

从零开始学习OpenCL开发（一）架构

## MKL

Intel Math Kernel Library是一套经过高度优化和广泛线程化的数学例程，专为需要极致性能的科学、工程及金融等领域的应用而设计。

官网：

https://software.intel.com/zh-cn/mkl

# NVIDIA

NVIDIA作为行业龙头，其影响力甚至在Khronos Group之上，它提出的标准很多成为了行业的事实标准。

## CUDA

CUDA是NVIDIA最早推出的通用数学运算库。除了基本的数学运算之外，还提供了一些工具包：

cuBLAS：线性计算库。

NVBLAS：多GPU版的cuBLAS。

cuFFT：FFT计算库。

nvGRAPH：图计算库。（这里的图是数学图论中的图，和DL框架中的计算图是两回事。）

cuRAND：随机数生成库。

cuSPARSE；稀疏矩阵计算库。

cuSOLVER：解线性方程的计算库。包括解稠密方程的cuSolverDN、解稀疏方程的cuSolverSP和矩阵分解的cuSolverRF。

## Deep Learning SDK

cuDNN：DL计算库。

NCCL：多结点、多GPU的通信库。

TensorRT：嵌入式设备上专用于DL inference的计算库。



