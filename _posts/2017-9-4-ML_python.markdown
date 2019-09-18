---
layout: post
title:  Machine Learning之Python篇（一）
category: AI 
---

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

## Anaconda

Anaconda是一个科学计算方面的python发行版，下面提到的所有工具都可以通过Anaconda一站式安装。

官网：

https://www.anaconda.com/download/

基本命令：

`conda update --all`

`conda update conda`

`conda update anaconda`

Anaconda同时也支持多个Python版本的并存和切换。它的底层用到了virtualenv。这个功能在Ubuntu下意义不太大，因为后者的apt工具已经维护好了2.x和3.x两个分支，除非你想同时支持两个不同的3.x分支。但在Windows下，由于包管理器的缺位，这个问题是很难解决的。

创建一个virtualenv：

`conda create -n python2 python=2.7`

这条命令会在Anaconda/envs下创建一个python2文件夹。

将命令行环境切换到该版本：

`activate python2`

参考：

http://www.cnblogs.com/zhusleep/p/5616099.html

Anaconda安装更新库

https://segmentfault.com/a/1190000004020387

Python多版本切换工具-Pyenv\virtualenv及Anaconda科学计算环境的配置

http://www.jianshu.com/p/d2e15200ee9b

Anaconda多环境多版本python配置指导

## virtualenv

安装：

`pip3 install virtualenv`

创建env：

`virtualenv --no-site-packages venv`

使用：

`source venv/bin/activate`

退出：

`deactivate`

## NumPy

NumPy是python语言所有数学计算库的基础。它主要提供了矩阵运算的功能。

官网：

http://www.numpy.org/

教程：

https://docs.scipy.org/doc/numpy-dev/user/quickstart.html

API参考：

https://docs.scipy.org/doc/numpy/reference/

quickstart中文版：

https://mp.weixin.qq.com/s/q0jSrxRRQAB7FUUqgD5H6A

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

## SciPy

SciPy提供了一些更高阶的数学运算库，比如：积分、插值、信号处理、傅立叶变换、矩阵特征值、统计计算等。

SciPy提供的功能主要仍局限于数学运算，而并未提升到算法的层面。这也是它和scikit-learn或其他高级库的差别所在。

官网：

http://www.scipy.org/

API参考：

https://docs.scipy.org/doc/scipy/reference/

### Gaussian filter

w = 2*int(truncate*sigma + 0.5) + 1

参考：

https://stackoverflow.com/questions/25216382/gaussian-filter-in-scipy

Gaussian filter in scipy

https://mp.weixin.qq.com/s/vGS4U3g4eaPuQwBWh-lTiA

机器学习核心：优化问题基于Scipy

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

## Matplotlib

Matplotlib是一个高阶的图形库，主要提供生成图表等数据可视化方面的功能。

官网：

http://matplotlib.org/

API参考：

http://matplotlib.org/1.5.3/api/index.html

示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/ml/plot/matplotlib_helloworld.py

参考：

https://mp.weixin.qq.com/s/mpj1QpWpnGm8117p3cEWZw

如何优雅而高效地使用Matplotlib实现数据可视化

https://mp.weixin.qq.com/s/LBrlXEhGYOx1aPFZzQcyTQ

5种快速易用的Python Matplotlib数据可视化方法

https://mp.weixin.qq.com/s/aBi1PTEumRs0frUpb_uYrA

用Python做图像处理（2）

https://mp.weixin.qq.com/s/3VgFKiUOFvtWmqg1BO9xGw

matplotlib--python的数据可视化

https://mp.weixin.qq.com/s/LuXyUBkaJUfg4WteFxStrg

Matplotlib可视化最有价值的50个图表

https://mp.weixin.qq.com/s/hlJFoh0jfduPswr7WrDikQ

当年为什么不好好学“数据可视化”！

https://mp.weixin.qq.com/s/Oh2ma7UjQhklE6YBo0EQBA

快速入门Matplotlib教程

https://mp.weixin.qq.com/s/fmoUGFjqlJf46r_iOrg9fA

高效使用Python可视化工具Matplotlib

https://mp.weixin.qq.com/s/MYqPHzzoWfaV2N7c4ZgfPQ

Python绘图，我只用Matplotlib

https://mp.weixin.qq.com/s/y4W7zK2-nFF-y_hSmB8j2g

超火动态排序图：代码不到40行，手把手教你！

https://zhuanlan.zhihu.com/p/82910169

Matplotlib输出动画实现K-means聚类过程可视化

https://mp.weixin.qq.com/s/6h-1k_D7QIFpknagnA8FZA

Python绘图，我只用Matplotlib

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

https://mp.weixin.qq.com/s/XIQ5EpQcYLxmRBuaTCZFzg

一行代码，Pandas秒变分布式，快速处理TB级数据

https://mp.weixin.qq.com/s/fXI5suCVna6fBxPnVyKevw

浅谈NumPy和Pandas库

https://mp.weixin.qq.com/s/1xHeXs9gM3LzEzCUz0jhXA

简单实用的pandas技巧：如何将内存占用降低90%

http://wiki.jikexueyuan.com/project/start-learning-python/311.html

python科学计算之Pandas使用

https://mp.weixin.qq.com/s/L95slIQ8so5IWpIpy7ZHEQ

23种Pandas核心操作，你需要过一遍吗？

https://mp.weixin.qq.com/s/CAddyz0qQt8SY56VOtcpJA

AI开发最大升级：Pandas与Scikit-Learn合并，新工作流程更简单强大！

https://mp.weixin.qq.com/s/MB7vGecugtemvACK5DRNQg

搞定Python中的“功夫熊猫”，做最高效的数据科学家

https://mp.weixin.qq.com/s/Soy02eshzm2_A7-TzwoGIQ

如何使用Pandas处理Large Data

https://mp.weixin.qq.com/s/2-Ayzmzo8tydDLoKpz1Ezw

如何用一行代码在多CPU环境下高效并行Pandas

https://mp.weixin.qq.com/s/ebRQMJI9sB7wNI_NYdmTAw

高逼格使用Pandas加速代码，向for循环说拜拜！
