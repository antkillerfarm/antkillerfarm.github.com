---
layout: post
title:  Machine Learning之Python篇
category: AI 
---

# Machine Learning之Python篇

## 概述

![](/images/article/python_for_big_data.jpg)

## 教程

https://ljalphabeta.gitbooks.io/python-/content/

《Python机器学习》中文版

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

## NumPy

NumPy是python语言所有数学计算库的基础。它主要提供了矩阵运算的功能。

官网：

http://www.numpy.org/

教程：

https://docs.scipy.org/doc/numpy-dev/user/quickstart.html

API参考：

https://docs.scipy.org/doc/numpy/reference/

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

### 参考

https://mp.weixin.qq.com/s/FVI3zEp4it-fd99-3MU9vA

从数组到矩阵的迹，NumPy常见使用大总结

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

python操作mysql数据库

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

## TuShare

TuShare是一个免费、开源的python财经数据接口包。主要实现对股票等金融数据从数据采集、清洗加工 到 数据存储的过程，能够为金融分析人员提供快速、整洁、和多样的便于分析的数据，为他们在数据获取方面极大地减轻工作量，使他们更加专注于策略和模型的研究与实现上。

官网：

http://tushare.org/

## skimage

skimage相当于python版本的OpenCV。

官网：

http://scikit-image.org/

参考：

https://buptldy.github.io/2016/03/31/2016-03-31-Skimage%20hog/

Compute the HOG descriptor by skimage

## Numba

NumPy的一个高速版本，能完成前者大部分的功能。

官网：

http://numba.pydata.org/

参考：

https://mp.weixin.qq.com/s/5peMiGXNnviAjMP76tV3pw

Python高性能计算库——Numba

## Face Recognition

这是一个人脸识别的软件包。

代码：

https://github.com/ageitgey/face_recognition

参考：

https://mp.weixin.qq.com/s/QcMsSsZY7TcNQ3G57p8eyQ

3行Python代码完成人脸识别

## Luminoth

Luminoth是一个开源的计算机视觉工具包，目前支持目标探测和图像分类，但以后会有更多的扩展。该工具包在TensorFlow和Sonnet上用Python搭建而成。

代码：

https://github.com/tryolabs/luminoth

## OpenFace

OpenFace是一款开源的人脸识别软件。它的原理基于CVPR 2015年的论文：FaceNet。由于采用了深度学习技术，OpenFace对人脸识别的准确率，大大超过了OpenCV。

OpenFace是用Python和Torch编写的。

官网：

https://cmusatyalab.github.io/openface/

## Tangent

Tangent是一个用于自动微分的源到源Python库。官网：

https://github.com/google/tangent

https://mp.weixin.qq.com/s/CExdIdtUxZi4c2B39xdYJA

谷歌开源Tangent

## 参考

https://github.com/neozhaoliang/pywonderland/tree/master/

如何用Python画各种著名数学图案

http://old.sebug.net/paper/books/scipydoc/index.html

用Python做科学计算

https://pan.baidu.com/s/1qYPUHNQ

小抄合集

https://mp.weixin.qq.com/s/8wI49YBAs7ckkYdc4B_WNQ

14张思维导图读懂Python编程核心知识体系

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

https://mp.weixin.qq.com/s/QIfxxY6z7y77RDE_8gDEIg

并行运算Process Pools

https://mp.weixin.qq.com/s/hNOF6YvDjIuX4u36Wr7LCA

随机森林之美

https://mp.weixin.qq.com/s/AHz81u8SGyi9_k40040QVg

Python机器学习入门到进阶

https://mp.weixin.qq.com/s/hFg1TvNocrMi5CpDtvRLcQ

推荐一个计算“造人”成功与否的贝叶斯模型！

http://www.sohu.com/a/168170785_642762

基于机器学习的Python信用卡欺诈检测

https://mp.weixin.qq.com/s/p69x5YyWxMulxx_OwX3nzQ

TuChart：基于Tushare和Echarts的股票数据视觉化应用

https://mp.weixin.qq.com/s/Xl9ZT7_I8P_ZU4O8j6bS5Q

基于Python的人脸识别：识别准确率高达99.38%！

https://mp.weixin.qq.com/s/tN7KVBA9GqdjpAjW-_WF8Q

Python学习者最易上手的机器学习漫游指南

https://mp.weixin.qq.com/s/MmuxNA8Xm6uRiVNGhDnyFw

Scikit-learn实战之数据预处理

https://mp.weixin.qq.com/s/V1EVdhX6-c1VNGNBgscFeA

用Python统计新浪微博各种表情使用频率

https://mp.weixin.qq.com/s/UOJsoB_YwBunj0YNShF11w

深度学习 + OpenCV，Python实现实时视频目标检测

https://mp.weixin.qq.com/s/YCNnfR9XAP0i-Qs-DIpOCg

6段Python代码刻画深度学习历史：从最小二乘法到深度神经网络

https://mp.weixin.qq.com/s/a7TLJD1-k3AlJ_ENPoJpwA

用Python分析《红楼梦》，后四十回是曹雪芹所写吗？

https://mp.weixin.qq.com/s/MUMQuZLDE5iyogLnIIBaMw

7步让你从零开始掌握Python机器学习！

http://mp.weixin.qq.com/s/5vI1Ii6bd_IzrXKHlvULSQ

是学习Java还是Python？一张图告诉你！

http://mp.weixin.qq.com/s/Ergq5KKM5vWLWirTf9loEw

7步让你从零开始掌握Python机器学习！

https://mp.weixin.qq.com/s/Ty46-s0kvr2MpbUhQPXJfQ

朴素贝叶斯实战篇之新浪新闻分类

https://mp.weixin.qq.com/s/CMxo4FNxWYyoG-gxAgtzyA

从零开始：手把手教你安装深度学习操作系统、驱动和各种python库！

https://github.com/leetmaa/KMCLib

KMCLib - a general framework for lattice kinetic Monte Carlo (KMC) simulations

https://mp.weixin.qq.com/s/T7dswxqhie0riPlnjdf9_Q

RNN入门与实践

https://mp.weixin.qq.com/s/ZPCfnj2kLxXLpBNbxs2ggw

Logistic回归实战篇之预测病马死亡率（一）

https://mp.weixin.qq.com/s/XpX9TPgv4a_uKAKiy8W4fg

Logistic回归实战篇之预测病马死亡率（二）

https://mp.weixin.qq.com/s/0i1jKgaRu2pLPAXAvoH6qA

Logistic回归实战篇之预测病马死亡率（三）

https://mp.weixin.qq.com/s/U46HOaBP5ijghpc2myJCvA

机器学习中，使用Scikit-Learn简单处理文本数据？

https://mp.weixin.qq.com/s/HoMr52X9O4fMs2YjtH9oFg

scikit-learn Adaboost类库的实战分析

https://mp.weixin.qq.com/s/61oY3gLEOjQjCJgGkEcABw

支持向量机分类实战

https://mp.weixin.qq.com/s?__biz=MzIxNjA2ODUzNg==&mid=2651435914&idx=1&sn=b0b6f1cda08798dc35bf0930dc9d9096

盘膝而坐--跟你聊聊APP市场的数据分析

https://mp.weixin.qq.com/s/AVSFbfikczjm7flsQQPC1g

2017年最受数据科学欢迎的Top15个Python库

https://mp.weixin.qq.com/s/jBULfMR0_7Ms9nc5CLLIFQ

Python的开源人脸识别库：离线识别率高达99.38%

https://zhuanlan.zhihu.com/p/30630608

Python和MATLAB交互的基本操作

https://mp.weixin.qq.com/s/drpd1O502TcL54cHSEdl_g

如何为时间序列数据优化K-均值聚类速度？

https://mp.weixin.qq.com/s/Di4xVdD1jOGGCiIB-vdkkg

Python可以被用来做哪些神奇好玩的事情

https://mp.weixin.qq.com/s/-SHD9rXUGQ-yp21RtFhJpg

手把手教你 Python挖掘用户评论典型意见并自动生产报告



