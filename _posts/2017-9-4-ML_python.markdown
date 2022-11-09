---
layout: post
title:  Machine Learning之Python篇（一）
category: AI 
---

* toc
{:toc}

# Machine Learning之Python篇

## 概述

![](/images/article/python_for_big_data.jpg)

![](/images/img3/ML_python.png)

## 教程

https://ljalphabeta.gitbooks.io/python-/content/

《Python机器学习》中文版

https://github.com/lawlite19/MachineLearning_Python

东南大学某研究生的github，包含大量ML算法示例。

https://github.com/lawlite19/DeepLearning_Python

上个哥们的DL示例

http://www.jianshu.com/p/eb9e3be977ed

Python数据分析之武林秘籍。这里包括了大量ML或DL的python工具包。

https://chrisalbon.com/

Chris Albon的个人主页。包含大量ML/DL相关的note。

>Chris Albon, UC Davis博士。在肯尼亚工作多年，援非的IT人。著有《Machine Learning with Python Cookbook: Practical Solutions from Preprocessing to Deep Learning》。

https://store.chrisalbon.com/

Chris Albon写的ML卡片书。

https://github.com/CharlesPikachu/Games

一个python制作游戏的示例库，包括使用AI玩游戏（DQN等）

https://github.com/CharlesPikachu/AIGames

上个作者的另一个python+AI+游戏的代码库

https://github.com/HuberTRoy/leetCode

Python实现各种基础算法

## Conda

Conda是一个开源的软件包管理系统和环境管理系统，用于安装多个版本的软件包及其依赖关系，并在它们之间轻松切换。它是目前最流行的Python环境管理工具。

官网：

https://conda.io/docs/

Conda一般有两个发行版——Anaconda和Miniconda。前者默认安装的包比较多，而后者则是一个最小安装。

参考：

https://mp.weixin.qq.com/s/NX2zWc7y8Y2kK9ZNxlUBVA

conda+jupyter玩转数据科学环境搭建

https://mp.weixin.qq.com/s/SGhKcD9zakXOInkZSmKX9A

Conda太慢？试试这个加速工具！

## Anaconda

Anaconda是一个科学计算方面的python发行版，下面提到的所有工具都可以通过Anaconda一站式安装。

>Python:蟒   
>Anaconda：森蚺

官网：

https://www.anaconda.com/download/

镜像：

https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/

基本命令：

`conda update --all`

`conda update conda`

`conda update anaconda`

Anaconda同时也支持多个Python版本的并存和切换。它的底层用到了virtualenv。这个功能在Ubuntu下意义不太大，因为后者的apt工具已经维护好了2.x和3.x两个分支，除非你想同时支持两个不同的3.x分支。但在Windows下，由于包管理器的缺位，这个问题是很难解决的。

创建一个virtualenv：

`conda create -n python2 python=2.7`

这条命令会在Anaconda/envs下创建一个python2文件夹。

>也可能是`~/.conda/envs`

显示env：

`conda env list`

将命令行环境切换到该版本：

`conda activate python2`

最新的Anaconda提供了`conda init`命令用于将环境添加到shell中。如果不想要的话，可以到`~/.bashrc`删除之。

退出环境：

`conda deactivate`

安装包：

`conda install XXX`

conda环境也支持用pip安装包，而且在`conda list`的时候会给出来源。

在conda环境中，使用pip的话，需要`conda install pip`。

更换源：

`vim ~/.condarc`

```text
channels:
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/main/
  - https://mirrors.ustc.edu.cn/anaconda/pkgs/free/
  - https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/
ssl_verify: true
```

参考：

http://www.cnblogs.com/zhusleep/p/5616099.html

Anaconda安装更新库

https://segmentfault.com/a/1190000004020387

Python多版本切换工具-Pyenv\virtualenv及Anaconda科学计算环境的配置

http://www.jianshu.com/p/d2e15200ee9b

Anaconda多环境多版本python配置指导

https://mp.weixin.qq.com/s/i4oUKZXAUMLGaRpJVmT1ZQ

一行命令装下所有“炼丹”工具及依赖项，就靠这个免费软件源了

## virtualenv

安装：

`sudo apt install python3-pip python3-virtualenv`

`pip3 install virtualenv`

创建env：

`virtualenv --no-site-packages venv`

使用：

`source venv/bin/activate`

退出：

`deactivate`

Conda可以创建当前系统中没有的python版本的env，但是virtualenv则不行。

---

venv多了，有的时候查找lib path就成了问题。

```python
print(tensorflow.__file__)
```

## NumPy

NumPy是python语言所有数学计算库的基础。它主要提供了矩阵运算的功能。

官网：

http://www.numpy.org/

官方中文网：

https://www.numpy.org.cn/

教程：

https://docs.scipy.org/doc/numpy-dev/user/quickstart.html

API参考：

https://docs.scipy.org/doc/numpy/reference/

quickstart中文版：

https://mp.weixin.qq.com/s/q0jSrxRRQAB7FUUqgD5H6A

中文教程：

https://www.runoob.com/numpy/numpy-tutorial.html

### 文件存取

原始二进制文件：tofile()和fromfile()

NumPy专用的格式文件（.npy或.npz，它和原始二进制文件的区别在于：前者包含维度和类型信息，而后者只有数据本身）：save()和load()

文本文件：savetxt()和loadtxt()

参考：

http://www.cnblogs.com/dmir/p/5009075.html

python:numpy（文件存取）

### 数据类型转换

类型转换：`c = b.astype(int)`

把A类型看做B类型，比如将一个float64的数，看做8个单字节的数：`a.dtype = 'int8'`

参考：

http://www.cnblogs.com/hhh5460/p/5129032.html

numpy数据类型dtype转换

### 运算符重载

运算符重载，本来是python语言提供的功能。但通常来说，还是数值计算领域用的比较多，所以就写在这里了。

C++的运算符重载，使用运算符作为函数名。而python则使用内置函数名，作为运算符重载的函数名。例如，`__lt__`函数对应`<`运算符。

>从设计上，我是比较支持python这种方式的。毕竟运算符都有特定含义，突然变成函数名，还是挺别扭的。

numpy的运算符重载还是挺方便的。

示例：

`a[a<0.5]=0`

上面的示例，可将a中小于0.5的值都设置为0。其中，`a<0.5`实际上返回的是一个bool型的tensor。

python的运算符重载不仅于`+-*/><`，还包括索引之类的操作。例如`__getitem__`和`__setitem__`分别对应了`X[key]`的读写操作。

参考：

https://blog.csdn.net/goodlixueyong/article/details/52589979

浅析Python运算符重载

### 特色函数

nan_to_num：将NaN替换为0，将infinity替换为最大值。

histogram：直方图。

percentile：百分位数。用途举例：提取数组中最小的30%的数。

### einsum

在实现一些算法时，数学表达式已经求出来了，需要将之转换为代码实现，简单的一些还好，有时碰到例如矩阵转置、矩阵乘法、求迹、张量乘法、数组求和等等，若是以分别以transopse、sum、trace、tensordot等函数实现的话，不但复杂，还容易出错

现在，这些问题你统统可以一个函数搞定，没错，就是einsum。

einsum全称Einstein summation convention，是爱因斯坦1916年提出的一种标记约定。

示例：

$$c_{ik}=a_{ij}b_{jk}=\sum_ja_{ij}b_{jk}$$

`c = np.einsum('ij,jk->ik', a, b)`

参考：

https://zhuanlan.zhihu.com/p/71639781

一个函数打天下，einsum

https://mp.weixin.qq.com/s/QxowSMirwnsUjIA-MFCj7g

特斯拉AI高管都推荐的张量工具（Einops）

### pad

`arr1D = np.array([1, 1, 2, 2, 3, 4])`

```bash
constant:  [0 0 1 1 2 2 3 4 0 0 0]
edge:  [1 1 1 1 2 2 3 4 4 4 4]
linear_ramp:  [0 0 1 1 2 2 3 4 3 1 0]
maximum:  [4 4 1 1 2 2 3 4 4 4 4]
mean:  [2 2 1 1 2 2 3 4 2 2 2]
median:  [2 2 1 1 2 2 3 4 2 2 2]
minimum:  [1 1 1 1 2 2 3 4 1 1 1]
reflect:  [2 1 1 1 2 2 3 4 3 2 2]
symmetric:  [1 1 1 1 2 2 3 4 4 3 2]
wrap:  [3 4 1 1 2 2 3 4 1 1 2]
```

### 参考

https://mp.weixin.qq.com/s/FVI3zEp4it-fd99-3MU9vA

从数组到矩阵的迹，NumPy常见使用大总结

https://www.machinelearningplus.com/101-numpy-exercises-python/

101 NumPy Exercises for Data Analysis。这里包含了101个和numpy有关的问题，并附有答案。

https://mp.weixin.qq.com/s/DiFTV29gvMlosoy2HaPQjw

用Python做图像处理（3）

https://mp.weixin.qq.com/s/qMiNGHHZMnPeFqXFz9c6rQ

numpy ndarray之内功心法，理解高维操作！

https://mp.weixin.qq.com/s/YxRPhTVu3zjVTzWn1290HQ

第二代NumPy？阿里开源超大规模矩阵计算框架Mars

https://mp.weixin.qq.com/s/0OSweIx9ibpp2bc5N61I4Q

C++程序员也能用上NumPy了

https://github.com/ddbourgin/numpy-ml

惊为天人，NumPy手写全部主流机器学习模型，代码超3万行！

https://mp.weixin.qq.com/s/MxvA-f6ocpmGLW5JQ8qsSg

玩数据必备Python库：Numpy使用详解

https://mp.weixin.qq.com/s/udKMuAfMVD4axulCR6Wgvw

这100道练习，带你玩转Numpy

https://mp.weixin.qq.com/s/2_q89PIfw43zivwHmTmY-A

在向量化NumPy数组上进行移动窗口操作

https://mp.weixin.qq.com/s/m-j1_2la-tg38NwNvDDSjw

Numpy中常用的10个矩阵操作示例

## SciPy

SciPy提供了一些更高阶的数学运算库，比如：积分、插值、信号处理、傅立叶变换、矩阵特征值、统计计算等。

SciPy提供的功能主要仍局限于数学运算，而并未提升到算法的层面。这也是它和scikit-learn或其他高级库的差别所在。

官网：

http://www.scipy.org/

API参考：

https://docs.scipy.org/doc/scipy/reference/

- Gaussian filter

`w = 2*int(truncate*sigma + 0.5) + 1`

参考：

https://stackoverflow.com/questions/25216382/gaussian-filter-in-scipy

Gaussian filter in scipy

https://mp.weixin.qq.com/s/vGS4U3g4eaPuQwBWh-lTiA

机器学习核心：优化问题基于Scipy

https://mp.weixin.qq.com/s/xN0mX9XWvVUpi0p5Tc6vLA

利用SciPy求解非线性规划问题

https://mp.weixin.qq.com/s/44RjMdARJMSM1_seImaSbg

SciPy稀疏矩阵

https://mp.weixin.qq.com/s/5g1WgWEOkA-38MXbkKUtWg

Scipy使用大全

## sklearn

Scikit-learn提供了常见的机器学习算法的实现。

官网：

http://scikit-learn.org/stable/index.html

教程：

http://scikit-learn.org/stable/user_guide.html

API参考：

http://scikit-learn.org/stable/tutorial/index.html

http://scikit-learn.org/stable/modules/classes.html

中文文档：

http://sklearn.apachecn.org

参考：

https://mp.weixin.qq.com/s/OHfQtJWq0wkF6BLkRWOAyw

sklearn与分类算法

https://mp.weixin.qq.com/s/OxNj9fWaEMh8SuQiK52HWg

sklearn中PCA库讲解与实战

https://mp.weixin.qq.com/s/JjJcyAccRc84U8C_qllf_Q

如何用sklearn创建机器学习分类器？

https://mp.weixin.qq.com/s/JkyJMKyDAaiX2M58PnWXaA

如何使用sklearn优雅地进行数据挖掘？

https://mp.weixin.qq.com/s/G5P0M-WGV_rMmHJE8lz6WQ

Scikit-Learn决策树算法类库使用小结

https://mp.weixin.qq.com/s/O1wPvi_aKK73yJcUpDf6EQ

开源sk-dist，超参数调优仅需3.4秒，sk-learn训练速度提升100倍
