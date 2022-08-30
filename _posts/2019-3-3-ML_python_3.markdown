---
layout: post
title:  Machine Learning之Python篇（三）
category: AI 
---

* toc
{:toc}

# Machine Learning之Python篇

## iPython

ipython是一个python的交互式 shell，比默认的python shell 好用得多，支持变量自动补全，自动缩进，支持 bash shell 命令，内置了许多很有用的功能和函数。

在较新的ipython版本中，添加了ipython notebook的功能，弥补了ipython shell下代码不易保存等缺点，并且在使用--pylab inline选项后，可以在代码执行后立即显示运行结果（包括图片，数据表格等），因此在数据分析中运用十分广泛。

`sudo apt install ipython ipython-notebook`

参考：

https://mp.weixin.qq.com/s/e7WBxoL_00EWi5G8Mz1L-w

50个关于IPython的使用技巧，get起来！

## Jupyter

Jupyter是iPython的后继项目，它不仅支持python语言，还支持其他50多种交互式语言。成为目前最流行的交互式shell和数据文本交换格式。

官网：

https://jupyter.org/

安装：

`pip install jupyter`

运行：

`jupyter notebook`

参见：

https://mp.weixin.qq.com/s/UXlPhX3Vb2yqocpUH_3W5w

Jupyter项目的前世今生

https://mp.weixin.qq.com/s/aJRVh7BWOMq4KCoBMtLGGw

快速学习Jupyter Notebook

https://blog.csdn.net/u011493486/article/details/55001477

Jupyter中显示matplotlib的图片

https://www.cnblogs.com/nxld/p/6566380.html

Jupyter Notebook快速入门

https://mp.weixin.qq.com/s/u-e66SgesPjmpKEihHHr8g

Jupyter Notebook超实用的5个插件，值得一试！

https://mp.weixin.qq.com/s/nlskrbG5x6rG7Mu6qDeRxw

青出于蓝而胜于蓝，这是一款脱胎于Jupyter Notebook的新型编程环境

https://mp.weixin.qq.com/s/nVRFgtYsR72nJrJvy31s5A

Jupyter官方神器：可视化Debug工具!

https://mp.weixin.qq.com/s/fkqnnQ3hqakPATRETahLPw

9个jupyter实用技巧

https://mp.weixin.qq.com/s/OHO3ePD3Ih3AtKv3qn2IEw

9个可以提高Jupyter Notebook开发效率的魔术命令

## Autograd

一个基于numpy的自动求导库。它是由Harvard Intelligent Probabilistic Systems Group开发的。

官网：

https://github.com/HIPS/autograd

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

## PIL

PIL：Python Imaging Library，已经是Python平台事实上的图像处理标准库了。

官网：

http://pythonware.com/products/pil/

安装：

`sudo apt install python-imaging`

或

`pip install pillow`

文档：

http://www.effbot.org/imagingbook/pil-index.htm

参考：

https://mp.weixin.qq.com/s/lJYzxehwo3K2APB2l_WeTA

用Python做图像处理（1）

https://mp.weixin.qq.com/s/yc_-PyR1qM-3_oKcH9PztQ

Python PIL图像处理开发极简教程

https://mp.weixin.qq.com/s/rQnPh_ny0APpylwoBY1Pkw

Python图像处理，366页pdf，Image Operators Image Processing in Python

https://mp.weixin.qq.com/s/haCH8uv3oIv9gl9BZCwwAA

Python处理图像五个有趣场景，很实用！

## Tangent

Tangent是一个用于自动微分的源到源Python库。

官网：

https://github.com/google/tangent

参考：

https://mp.weixin.qq.com/s/CExdIdtUxZi4c2B39xdYJA

谷歌开源Tangent

## VisPy

VisPy可以算的上是Matplotlib的威力加强版，它添加了对GPU、3D和大数据的支持。

官网：

http://vispy.org/

参考：

https://mp.weixin.qq.com/s/yduK_XKQ-qhiYTNl-YZMFg

利用Python实现卷积神经网络的可视化

## Seaborn

Seaborn是另一个非常棒的Matplotlib的威力加强版，专注于统计绘图，并可无缝对接Pandas库。

官网：

https://seaborn.pydata.org

参考：

https://mp.weixin.qq.com/s/hPbTZHR2iDH4t1eQghNvUQ

数据可视化详解+代码演练

https://mp.weixin.qq.com/s/HpOx6BWM2lbgDpJcF6XncA

可视化神器Seaborn的超全介绍

https://mp.weixin.qq.com/s/94nwkxn20ZixmcsX8oUJIw

Seaborn绘图

https://mp.weixin.qq.com/s/GKrqAD0tud6WOFjNwQVVZw

使用Seaborn绘制常用图表

https://mp.weixin.qq.com/s/Lrag8Wilf4B6abVG2UcMZw

详解seaborn可视化中的kdeplot、rugplot、distplot与jointplot

https://mp.weixin.qq.com/s/-TwkUUjGdOiik835xNemfg

14个Seaborn数据可视化图

https://mp.weixin.qq.com/s/AWU-ul7TQ6zWgvX8j2Ut1g

这3个Seaborn函数可以搞定90%的可视化任务

## Bokeh

Bokeh是一个数据可视化的库。它不仅提供了和Matplotlib类似的静态图功能，还提供了生成交互动态图的功能。

官网：

http://bokeh.pydata.org/en/latest/

参考：

https://mp.weixin.qq.com/s/R6NclZO1MqjScRlLJ6Vefw

Python地图可视化三大秘密武器（bokeh、basemap、geopandas）

https://mp.weixin.qq.com/s/XKjo5Dj7bpIlBtxkbozekA

掌握这几点，轻松玩转Bokeh可视化

## Chartify

Chartify在Bokeh的基础上又封装了一层，提供了更友好的API。

官网：

https://github.com/spotify/chartify

## Plotly

Plotly也是Matplotlib的威力加强版，主打交互式绘图。

官网：

https://plot.ly/python/

它还有一个高级封装叫做Plotly Express。

官网：

https://www.plotly.express/

参考：

https://www.jianshu.com/p/57bad75139ca

python plotly使用教程

https://mp.weixin.qq.com/s/RkuLhwj_to_B01RJDQsGcA

强烈推荐一款Python可视化神器！

https://mp.weixin.qq.com/s/9mwLGsXQkTaxIohPwgDyKw

使用Plotly创建带有回归趋势线的时间序列可视化图表

https://mp.weixin.qq.com/s/R_uiw-XCDL40RVcQzfhISA

Python绘图库Plotly，手把手教你用

## ImageAI

ImageAI是一个CV方面的库，集成了大量的DL模型，其目标是使用数十行代码完成一个CV任务。

代码：

https://github.com/OlafenwaMoses/ImageAI

## Pyecharts

py是一个用于生成Echarts图表的类库。Echarts是百度开源的一个数据可视化JS库。

官网：

http://pyecharts.org/

参考：

https://mp.weixin.qq.com/s/3T594c4DzuRmPW5A4zu8Dg

Pyecharts：极其强大的Python数据可视化模块

https://mp.weixin.qq.com/s/20cxuC_tAVmofA7lvquEMQ

Python可视化神器——pyecharts的超详细使用指南！

https://mp.weixin.qq.com/s/5GJKIt5OkMh7C3xqoTJsTA

PyEcharts

https://mp.weixin.qq.com/s/6rusBFuLGw4gpDt6tSQTIw

PyEcharts TreeMap

https://mp.weixin.qq.com/s/6LhoraTo2cLtLJaXbKpt-A

教你用pyecharts制作交互式桑基图，赶快学起来吧！

https://mp.weixin.qq.com/s/3NYEM2MsfGigRPscP-RVZQ

使用pyecharts绘制词云图-淘宝商品评论展示

## Yellowbrick

Yellowbrick是和Scikit-Learn配套的ML可视化库。

官网：

https://www.scikit-yb.org/en/latest/

## mlpy

mlpy是一个开源的ML库。只是它最近的一次更新，已经是2012年的事情了。

官网：

http://mlpy.sourceforge.net

## PyFlux

PyFlux是Python中为处理时间序列问题而创建的开源库。该库有一系列极好的时间序列模型，包括但不限于ARIMA、GARCH和VAR模型。

官网：

https://pyflux.readthedocs.io/en/latest/index.html

## ImagePy

ImagePy是国人写的一个图像处理工具。

官网：

https://github.com/Image-Py/imagepy

## imgaug

imgaug是一个图像数据增强方面的库，可用于扩充机器学习训练时所用的图片数据集。

官网：

https://imgaug.readthedocs.io/en/latest/

参考：

https://www.cnblogs.com/vincentcheng/p/9186540.html

Augmentor和imgaug——python图像数据增强库

## Netron

Netron是一个DL模型的可视化工具，支持几乎所有类型的模型。

代码：

https://github.com/lutzroeder/netron

安装：

`snap install netron`

## SymPy

SymPy是一个符号计算的Python库。

官网：

https://www.sympy.org/en/index.html

参考：

https://zhuanlan.zhihu.com/p/96738286

用Python来研究数学---SymPy符号工具包介绍

https://mp.weixin.qq.com/s/pexY5bG0NhQL2uOJF4UHPQ

符号运算他做得比Matlab更漂亮

## 可视化

GCJ-02是由中国国家测绘局（G表示Guojia国家，C表示Cehui测绘，J表示Ju局）制订的地理信息系统的坐标系统。又名“火星坐标系”。

国内出版的各种地图系统（包括电子形式），必须至少采用GCJ-02对地理位置进行首次加密。

CGCS2000坐标系是目前在测绘和地理信息以及工程建设领域主推的坐标系。

粗略转换：GCJ(wgs) ≈ wgs，由此 GCJ(GCJ(wgs)) - GCJ(wgs) ≈ GCJ(wgs) - wgs。因此使用 GCJ(wgs) - (GCJ(GCJ(wgs)) - GCJ(wgs)) 去逼近 wgs。

精确转换：因为GCJ(wgs1) - GCJ(wgs2) ≈ wgs1 - wgs2，所以使用 wgsGuess <- wgsGuess - (GCJ(wgsGuess) - GCJ(wgsReal)) 来迭代逼近。

https://zhuanlan.zhihu.com/p/100993681

利用Python进行地理坐标系统的转换

https://www.zhihu.com/question/29806566

如何看待“地形图非线性保密处理技术”？

---

https://github.com/neozhaoliang/pywonderland/tree/master/

如何用Python画各种著名数学图案

https://mp.weixin.qq.com/s/p69x5YyWxMulxx_OwX3nzQ

TuChart：基于Tushare和Echarts的股票数据视觉化应用

https://mp.weixin.qq.com/s/D_e8lxO3cx-dshEQ_zhfzQ

如何利用散点图矩阵进行数据可视化

https://mp.weixin.qq.com/s/dARWbR1ghho0yX02lNA-gw

Python绘制histogram直方图

https://github.com/minimaxir/stylecloud

一款牛逼的词云工具，开箱即用

https://mp.weixin.qq.com/s/tHNd6y2v7GXN2mSIv0qXlg

手把手：一张图看清编程语言发展史，你也能用Python画出来！

https://mp.weixin.qq.com/s/7M_xbmDQIlSEB1TKN3fbQw

4种更快更简单实现Python数据可视化的方法

https://mp.weixin.qq.com/s/wsa-HxJRCzhBF8E6yrLCQg

如何用Python画一个中国地图？

https://mp.weixin.qq.com/s/c7Sry6FaGv3ZgkKLekxc2g

用Python画中国地图（下）
