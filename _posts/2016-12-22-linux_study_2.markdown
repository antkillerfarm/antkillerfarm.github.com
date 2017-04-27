---
layout: post
title:  linux学习心得（二）, 知名数据集, Python（二）, 运维工具集, Spring
category: technology 
---

## 查看内存使用情况

### top命令

top命令可在进程这一级查看内存、运行时间、CPU等的使用情况。并可根据不同属性对结果排序：

P：按%CPU使用率排序

T：按TIME+排序

M：按%MEM排序

注：运行top命令之后，输入相应字符即可切换排序。

### free命令

free命令的内容比较概括，主要包含系统内存的整体使用情况，不深入到进程一级。

## 查看查看磁盘IO情况

### vmstat

`vmstat`

### iostat

`iostat -d -k 1 10 #查看TPS和吞吐量信息`

`iostat -d -x -k 1 10 #查看设备使用率（%util）、响应时间（await）`

`iostat -c 1 10 #查看cpu状态`

### iotop

`sudo iotop`

### dstat

dstat是后起之秀，号称可以替代vmstat、iostat、ifstat。

`dstat -d --top-io`

## 时间的表示方法

一般遵循ISO 8601标准：

https://www.w3.org/TR/NOTE-datetime

YYYY-MM-DDThh:mm:ss.sTZD (eg 1997-07-16T19:20:30.45+01:00)

其中的TZD表示time zone designator。

## wifi配置

Linux下的wifi配置主要使用iw系列命令，包括iw、iwconfig、iwlist、iwpriv。参见：

http://blog.csdn.net/liangyamin/article/details/7209761

## Inotify

一种高效、实时的Linux文件系统事件监控框架。参考文档：

http://www.infoq.com/cn/articles/inotify-linux-file-system-event-monitoring

## /usr

usr很多人都认为是user缩写，其实不然，这是unix system resource的缩写。

## lsof命令

lsof(list open files)是一个列出当前系统打开文件的工具。在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。所以如传输控制协议 (TCP) 和用户数据报协议 (UDP) 套接字等，系统在后台都为该应用程序分配了一个文件描述符，无论这个文件的本质如何，该文件描述符为应用程序与基础操作系统之间的交互提供了通用接口。因为应用程序打开文件的描述符列表提供了大量关于这个应用程序本身的信息，因此通过lsof工具能够查看这个列表对系统监测以及排错将是很有帮助的。

## nc命令

NetCat，在网络工具中有“瑞士军刀”美誉。它短小精悍（1.84版本也不过25k，旧版本或缩减版甚至更小）、功能实用，被设计为一个简单、可靠的网络工具，可通过TCP或UDP协议传输读写数据。同时，它还是一个网络应用Debug分析器，因为它可以根据需要创建各种不同类型的网络连接。

http://blog.csdn.net/wang7dao/article/details/7684998

## 常用命令示例

`find . -name *.txt`

查找文件夹（包括子文件夹）中所有的txt文件。

`strings /lib/tls/libc.so.6 | grep GLIBC`

strings能输出文件中的可打印字符串（可指定字符串的最小长度），通常用来查看非文本文件（如二进制可执行文件）中的可读内容。上例可查看glibc支持的版本。

`ps -efww`

ps默认只显示当前用户的进程。这里是全显示的示例。

`iconv -f gbk -t utf-8 -c 1.txt -o 2.txt`

## GnuGo

GnuGo是一个著名的开源围棋软件，但是它只有文字界面。一般使用Quarry作为它的GUI。

`sudo apt-get install quarry`

## OCR

linux下可以使用tesseract作为OCR工具。安装方法：

`sudo apt install tesseract-ocr libtesseract-dev`

使用方法：

`tesseract ./111.png 1 -l chi_sim+eng`

## DosBox

DosBox是Linux平台玩DOS老游戏的法宝。

安装：

`sudo apt install dosbox`

启动DosBox之后，需要使用如下命令加载本地文件夹：

`mount c ~/dosprom`

## 大文件处理

在“大数据”时代，我们会经常遇到有大文本文件（上 GB 或更大）的情况。传统的文本编辑软件对处理这样的大文件不太有效，当我们试图打开一个大文件时会经常由于内存不足而郁闷的不行。

如果你只需要查看一个文本文件，并不对它做编辑，可以考虑下glogg。

`sudo apt-get install glogg`

如果需要修改的话，可以使用JOE。

`sudo apt-get install joe`

# 知名数据集

## MNIST

MNIST是一个手写字符集，也是学习深度学习和SVM的入门必备数据集。目前由Yann LeCun维护。网址：

http://yann.lecun.com/exdb/mnist/

MNIST是NIST的一个子集，包含了6万个训练样本和1万个测试样本。为了避免碎小文件的问题，所有的手写字符图片都被放到一个文件中。整个数据集包含4个这样的文件。它们的格式说明，实际上在官网就有，只是比较靠后面，容易被忽视。

## Iris flower Data Set

Iris是一种叫做鸢尾的植物。Iris flower Data Set是Ronald Fisher在1936年的论文中给出的数据集。该数据集包含了三种鸢尾花的4个特征的样本集。Fisher基于该数据集，提出了linear discriminant analysis算法。

下图是该数据集的LDA图示。

![](/images/article/Iris_dataset_scatterplot.svg)

参考：

https://en.wikipedia.org/wiki/Iris_flower_data_set

## UCI数据集

UCI大学有个专门提供数据集的网站：

http://archive.ics.uci.edu/ml/datasets

其中包含360+的数据集，实在是个宝库啊。

# Machine Learning之Python篇

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

## TensorFlow

TensorFlow是Google主导的开源深度学习库。官网：

https://www.tensorflow.org/

代码：

https://github.com/tensorflow/tensorflow

TensorFlow提供了一个可视化的神经网络展示：

http://playground.tensorflow.org/

TensorFlow的教程：

http://tensorflowtutorial.net/tensorflow-tutorial

教程中文版：

http://wiki.jikexueyuan.com/project/tensorflow-zh/

安装：

`pip install tensorflow`

由于我的PC显卡不合要求，因此直接安装的是CPU版本，这也是最通用的版本。

# Python

## 如何通过需认证的代理获取HTTP网页

Python内置的urllib和urllib2模块都可用于获取HTTP网页，但使用范围是不同的。urllib只支持HTTP 0.9和HTTP 1.0，所以如果只是使用代理可以使用urllib.FancyURLopener类，如果该代理还需要认证的话urllib就不行了。因为代理认证是HTTP 1.1中引入的。

这时可以考虑使用urllib2模块。代码如下：

{% highlight python %}
import urllib2
l_proxy_info = {
'user' : 'user',
'pass' : 'pass',
'host' : 'host',
'port' : 3128
}

l_proxy_support = urllib2.ProxyHandler({"http" : "http://%(user)s:%(pass)s@%(host)s:%(port)d" % l_proxy_info})
l_opener = urllib2.build_opener(l_proxy_support, urllib2.HTTPHandler)

urllib2.install_opener(l_opener)
usock = urllib2.urlopen('http://www.sohu.com')
{% endhighlight %}

## 时间、日期数据的格式处理。

时间数据上的格式处理，主要指strftime和strptime函数。其中前者可将时间数据转换成指定格式的字符串，而后者则将字符串转换成时间数据。

{% highlight python %}
import time
a = "2013-10-10 23:40:00"
timeArray = time.strptime(a, "%Y-%m-%d %H:%M:%S")
otherStyleTime = time.strftime("%Y/%m/%d %H:%M:%S", timeArray)
{% endhighlight %}

日期方面也是类似的做法。

## 星期处理

`datetime(2002, 12, 4).isoweekday() == 3`

`datetime(2001, 12, 31).isocalendar() == (2002, 1, 1)#(year, week number, weekday)`

从上面的例子还可以看出，isocalendar可以完美的处理跨年问题。

## 参数传递

基本数据类型：数值型，字符串，布尔，是值传递；其他的是引用传递，包括类类型，列表，字典。

## list操作

{% highlight python %}
L.append(var)   #追加元素
L.extend(list)  #追加list，即合并list到L上
{% endhighlight %}

# 运维工具集

## Zabbix

zabbix是一个基于WEB界面的提供分布式系统监视以及网络监视功能的企业级的开源解决方案。

http://www.zabbix.com/

## Cacti

Cacti是一套基于PHP,MySQL,SNMP及RRDTool开发的网络流量监测图形分析工具。

http://cacti.net/

## Nagios

Nagios是一款开源的免费网络监视工具，能有效监控Windows、Linux和Unix的主机状态，交换机路由器等网络设备，打印机等。

https://www.nagios.org/

# Ganglia

Ganglia是伯克利开发的一个集群监控软件。可以监视和显示集群中的节点的各种状态信息，比如如：cpu 、mem、硬盘利用率， I/O负载、网络流量情况等，同时可以将历史数据以曲线方式通过php页面呈现。

官网：

http://ganglia.sourceforge.net/

## Jenkins

Jenkins是一个开源软件项目，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

https://jenkins.io/index.html

## Walle

Walle一个web部署系统工具，配置简单、功能完善、界面流畅、开箱即用！支持git、svn版本管理，支持各种web代码发布，PHP，Python，JAVA等代码的发布、回滚，可以通过web来一键完成。

https://walle-web.io/

# Spring

Spring是一个Java Web应用框架。官网：

http://spring.io/

## Ubuntu安装Eclipse、Spring

1.安装Eclipse

`sudo apt-get install eclipse`

2.安装Spring

`sudo apt-get install libspring-web-portlet-java`

注意：ubuntu软件仓库中还有一个叫做spring的游戏引擎，不要弄错了。

http://www.mkyong.com/spring/quick-start-maven-spring-example/

http://wiki.jikexueyuan.com/project/spring/

## Restful

http://spring.io/guides/gs/rest-service/

## Spring Boot

https://www.tianmaying.com/tutorial/deploy-spring-boot-application

http://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html

Spring Boot默认的配置文件

## WebService

https://spring.io/guides/gs/producing-web-service/

http://localhost:9999/ws/countries.wsdl

