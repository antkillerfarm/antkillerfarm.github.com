---
layout: post
title:  linux学习心得（二）, 知名数据集, Python（二）
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

## Linux知识图谱

![](/images/article/linux_perf_tools_full.png)

原图地址：

http://www.brendangregg.com/linuxperf.html

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

## CIFAR-10

CIFAR-10是由Hinton的两个大弟子Alex Krizhevsky、Ilya Sutskever收集的一个用于普适物体识别的数据集。Cifar是加拿大政府牵头投资的一个先进科学项目研究所。

说白了，就是看你穷的没钱搞研究，就施舍给你。Hinton、Bengio和他的学生在2004年拿到了Cifar投资的少量资金，建立了神经计算和自适应感知项目。

这个项目结集了不少计算机科学家、生物学家、电气工程师、神经科学家、物理学家、心理学家，加速推动了DL的进程。从这个阵容来看，DL已经和ML系的数据挖掘分的很远了。

DL强调的是自适应感知和人工智能，是计算机与神经科学交叉。DM强调的是高速、大数据、统计数学分析，是计算机和数学的交叉。

CIFAR-10由60000张32*32的RGB彩色图片构成，共10个分类。50000张训练，10000张测试（交叉验证）。这个数据集最大的特点在于将识别迁移到了普适物体，而且应用于多分类（姊妹数据集CIFAR-10达到100类，ILSVRC比赛则是1000类）。

官网：

https://www.cs.toronto.edu/~kriz/cifar.html

参考：

http://www.cnblogs.com/neopenx/p/4480701.html

## ImageNet

ImageNet是由李飞飞等创建的一个计算机视觉系统识别项目，是目前世界上图像识别最大的数据库。

官网：

http://www.image-net.org/

需要注意的是，由于ImageNet的数据过于庞大，因此主页下载的数据文件，仅仅只是图片的URL而已。

## PASCAL VOC

PASCAL VOC是一个标有物体类别和位置的图片库。

官网：

http://host.robots.ox.ac.uk/pascal/VOC/

2005～2012年期间，围绕着该数据集展开了Pascal VOC挑战赛。

## UCI数据集

UCI大学有个专门提供数据集的网站：

http://archive.ics.uci.edu/ml/datasets

其中包含360+的数据集，实在是个宝库啊。

## 其他

http://www.csdn.net/article/2014-06-06/2820111-100-Interesting-Data-Sets-for-Statistics/1

100+诡异的数据集

http://www.sogou.com/labs/

搜狗实验室的网站可以下载很多NLP和图片识别方面的数据

http://www.zanmel.com/2015/12/14/10-great-datasets-on-movies/

最知名的电影数据集。

http://www.speech.cs.cmu.edu/databases/an4/

CMU的音频数据库，可用于语音识别

http://blog.csdn.net/wangkr111/article/details/44514097

肤色检测&人脸检测数据集等链接大集合

https://mp.weixin.qq.com/s/tewjGzfAVCKcG1dlURxyeg

MIT发布的10大自然语言处理数据集和语料库

https://github.com/candlewill/Dialog_Corpus/blob/master/README.md

用于对话系统的中英文语料

http://www.cvpapers.com/datasets.html

常见的cvpr研究的数据库下载链接

http://universaldependencies.org/

一个依存语法方面的标注数据集。这类数据也叫做treebank数据，原因是依存语法生成的结果是一棵语法树。

https://zhuanlan.zhihu.com/p/25138563

各领域公开数据集下载

# Python

## python调用GTK函数的一般规则

众所周知，GTK库是用C语言编写的，其相关的文档提供的也是C函数的接口，python接口的文档相对较少，获得也不太容易。因此有必要掌握一下这方面基本的命名规则。（这里只讲方法，对深层次的调用原理不做探究。）

1）类初始化函数

C:`GtkWidget * gtk_button_new (void);`

python:`button = Gtk.Button()`

这里除了把C函数式的写法，替换成python的类的写法之外，并无其他差异。

2）普通类成员函数

C:`void gtk_container_add (GtkContainer *container, GtkWidget *widget);`

python:`button.add(image)`

可以看出，C函数的第一个表示self类指针的参数被省略掉了。

3）回调函数

这里以按钮的click事件回调为例：

C:`void user_function (GtkButton *button, gpointer user_data)`

python:

`button.connect("clicked", self.playToggled)`

`def playToggled(self, w):`

这里的情况要复杂的多。首先在事件回调注册的时候，由于我们并没有给user_data赋值，因此照理说playToggled函数，只有button这一个参数。但实际传递给playToggled函数的有两个参数，self是一个用于占位的参数，w对应button，没有东西对应user_data。

4）用于输出的参数

C:`void gst_message_parse_error (GstMessage *message, GError **gerror, gchar **debug);`

python:`err, debug = message.parse_error()`

这个例子中的gerror和debug都是用于输出的参数。如果只想得到其一，还可用以下方法:

`debug = message.parse_error()[2]`

5）枚举类型

C:

{% highlight c %}
enum GstSeekFlags
{
  GST_SEEK_FLAG_FLUSH,
  ...
}
{% endhighlight %}

python:`Gst.SeekFlags.FLUSH`

6）宏常量

C:`#define GST_CLOCK_TIME_NONE ((GstClockTime) -1)`

python:`Gst.CLOCK_TIME_NONE`

7）类型转换

大多数情况下，类型转换自动完成，并无什么问题。这里只讨论特殊的情况。

C:

{% highlight c %}
gst_element_seek (pipeline, 1.0, GST_FORMAT_TIME, GST_SEEK_FLAG_FLUSH,
                         GST_SEEK_TYPE_SET, time_nanoseconds,
                         GST_SEEK_TYPE_NONE, GST_CLOCK_TIME_NONE))
{% endhighlight %}

python:

{% highlight python %}
self.player.seek(1.0, Gst.Format.TIME, Gst.SeekFlags.FLUSH, Gst.SeekType.SET,\
                 time_nanoseconds, Gst.SeekType.NONE, Gst.CLOCK_TIME_NONE)
{% endhighlight %}

这里首先按照一般的方法，将C代码转化成python代码。但是运行之后，产生如下错误：

`OverflowError: long too big to convert`

究其原因，gst_element_seek的最后一个参数是uint64类型的。但python原生并不支持该类型，而是将之转换成int类型（实际上是int64类型），这样的话，GST_CLOCK_TIME_NONE的值显然超出了int64所能表示的范围。

解决的办法是使用ctypes库，将

`Gst.CLOCK_TIME_NONE`

改为

`c_long(Gst.CLOCK_TIME_NONE).value`


