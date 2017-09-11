---
layout: post
title:  Machine Learning之Python篇
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

## TuShare

TuShare是一个免费、开源的python财经数据接口包。主要实现对股票等金融数据从数据采集、清洗加工 到 数据存储的过程，能够为金融分析人员提供快速、整洁、和多样的便于分析的数据，为他们在数据获取方面极大地减轻工作量，使他们更加专注于策略和模型的研究与实现上。

官网：

http://tushare.org/

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




