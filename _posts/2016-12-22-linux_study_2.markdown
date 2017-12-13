---
layout: post
title:  linux学习心得（二）, Fedora, diff&patch, bash, SWIG
category: linux 
---

# 查看内存使用情况

## top命令

top命令可在进程这一级查看内存、运行时间、CPU等的使用情况。并可根据不同属性对结果排序：

P：按%CPU使用率排序

T：按TIME+排序

M：按%MEM排序

注：运行top命令之后，输入相应字符即可切换排序。

## free命令

free命令的内容比较概括，主要包含系统内存的整体使用情况，不深入到进程一级。

## 查看查看磁盘IO情况

## vmstat

`vmstat`

## iostat

`iostat -d -k 1 10 #查看TPS和吞吐量信息`

`iostat -d -x -k 1 10 #查看设备使用率（%util）、响应时间（await）`

`iostat -c 1 10 #查看cpu状态`

## iotop

`sudo iotop`

## dstat

dstat是后起之秀，号称可以替代vmstat、iostat、ifstat。

`dstat -d --top-io`

## 时间的表示方法

一般遵循ISO 8601标准：

https://www.w3.org/TR/NOTE-datetime

YYYY-MM-DDThh:mm:ss.sTZD (eg 1997-07-16T19:20:30.45+01:00)

其中的TZD表示time zone designator。

# wifi配置

Linux下的wifi配置主要使用iw系列命令，包括iw、iwconfig、iwlist、iwpriv。参见：

http://blog.csdn.net/liangyamin/article/details/7209761

# Inotify

一种高效、实时的Linux文件系统事件监控框架。参考文档：

http://www.infoq.com/cn/articles/inotify-linux-file-system-event-monitoring

# /usr

usr很多人都认为是user缩写，其实不然，这是unix system resource的缩写。

# lsof命令

lsof(list open files)是一个列出当前系统打开文件的工具。在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。所以如传输控制协议 (TCP) 和用户数据报协议 (UDP) 套接字等，系统在后台都为该应用程序分配了一个文件描述符，无论这个文件的本质如何，该文件描述符为应用程序与基础操作系统之间的交互提供了通用接口。因为应用程序打开文件的描述符列表提供了大量关于这个应用程序本身的信息，因此通过lsof工具能够查看这个列表对系统监测以及排错将是很有帮助的。

# nc命令

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

将文件从gbk编码转为utf-8编码。

`mount -t nfs 192.168.0.1:/tmp /mnt/nfs`

挂载NFS。挂载点（即上例中的/mnt/nfs）必须事先创建。

# GnuGo

GnuGo是一个著名的开源围棋软件，但是它只有文字界面。一般使用Quarry作为它的GUI。

`sudo apt-get install quarry`

# DosBox

DosBox是Linux平台玩DOS老游戏的法宝。

安装：

`sudo apt install dosbox`

启动DosBox之后，需要使用如下命令加载本地文件夹：

`mount c ~/dosprom`

# 大文件处理

在“大数据”时代，我们会经常遇到有大文本文件（上 GB 或更大）的情况。传统的文本编辑软件对处理这样的大文件不太有效，当我们试图打开一个大文件时会经常由于内存不足而郁闷的不行。

如果你只需要查看一个文本文件，并不对它做编辑，可以考虑下glogg。

`sudo apt-get install glogg`

如果需要修改的话，可以使用JOE。

`sudo apt-get install joe`

# Linux知识图谱

![](/images/article/linux_perf_tools_full.png)

原图地址：

http://www.brendangregg.com/linuxperf.html

# minicom

1.查看串口设备

`ls -l /dev`

2.连接串口设备

`minicom -D /dev/ttyS0`

3.菜单

Ctrl+A Z

4.退出

Ctrl+A X

# TikZ

安装：

`sudo apt install qtikz texlive-fonts-recommended`

在线编辑：

https://www.sharelatex.com/

代码示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/helloworld/tikz

由于qtikz对于某些包支持的不好，我也不知道该怎样配置，因此采用了原始的命令行方式：

`pdflatex xxx.tex`

`evince xxx.pdf`

参考：

http://www.hahack.com/tools/pgftikz-resources/

PGF/TikZ资源汇总

http://www.texample.net/tikz/examples/

各种tikz示例的网站

http://www.fuzihao.org/blog/2015/08/11/TikZ%E5%85%A5%E9%97%A8%E6%95%99%E7%A8%8B/

TikZ入门教程

http://www.cnblogs.com/wangl393/

一个TikZ的blog

https://sandrocirulli.net/how-to-plot-functions-with-latex/

安装pgfplots

# MobaXterm

MobaXterm是一个远程终端登录软件。

http://mobaxterm.mobatek.net/

# tmux

你是不是经常需要SSH或者telent远程登录到Linux服务器？你是不是经常为一些长时间运行的任务而头疼，比如系统备份、ftp传输等等。

通常情况下我们都是为每一个这样的任务开一个远程终端窗口，因为他们执行的时间太长了。必须等待它执行完毕，在此期间可不能关掉窗口或者断开连接，否则这个任务就会被杀掉，一切半途而废了。

这个问题的解决办法是安装一个会话管理工具。原先主要使用screen：

https://www.gnu.org/software/screen/

这是一个有30年历史（1987年）的软件。

现在的话，一般推荐使用tmux：

https://github.com/tmux/tmux/

安装：

`sudo apt install tmux`

参考：

https://linuxtoy.org/archives/from-screen-to-tmux.html

从screen切换到tmux

http://mingxinglai.com/cn/2012/09/tmux/

tmux的使用方法和个性化配置

http://chengjin.li/2017/08/09/tmux-using-tutorial/

终端复用工具---tmux的安装及使用

# 环境变量

设置环境变量的方法：

1）临时的：使用export命令声明即可，变量在关闭shell时失效。示例如下：

`export PATH=/home/xyz/bin:$PATH;`

2）永久的：需要修改配置文件，变量永久生效。

在/etc/profile文件中添加变量（对所有用户生效）。修改文件后要想马上生效，还要运行`source /etc/profile`，不然只能在下次重进此用户时生效。

在用户目录下的.bash_profile文件中增加变量（对该用户生效）。同样需要source才能马上生效。

重要的环境变量：

PATH：可执行文件路径。

LD_LIBRARY_PATH：动态链接库文件路径

# 设置网卡eth0的IP地址和子网掩码

`sudo ifconfig eth0 192.168.2.1 netmask 255.255.255.0`

# 无线设置

查看无线网卡状态：

`iwconfig`

`wpa_cli`

扫描周围的wifi信号：

`iwlist scanning`

# Fedora

Fedora作为主要的Linux发行版之一，我虽然用的不多，但实际上这却是我最早接触的Linux发行版。后来换用Ubuntu，很大的原因是因为：这是Google为Android选择的开发平台。

最近因为工作需要重新捡起了Fedora。但公司所用的版本太过古老，还是2009年的Fedora 12。所以想了一下，开始试用最新的Fedora 22。这里是使用过程中的一些操作笔记。

## 安装

https://getfedora.org/

这是官方的下载地址。这里我用的是Workstation版本。

Fedora 22的默认桌面是GNOME 3.16，这一版的外观借鉴了Mac OS X的一些设计，让人眼前一亮。

## 安装软件

Fedora 22使用dnf替代yum。因此安装基本gcc开发环境，可用如下命令：

`dnf install gcc kernel-devel patch bison flex subversion`

如果下载速度较慢的话，可以在/etc/dnf/dnf.conf最后添加：

`fastestmirror=true`

保存后，执行

{% highlight bash %}
$ sudo dnf clean all
$ sudo dnf makecache
{% endhighlight %}

此外，和Ubuntu一样，Fedora也有自己的网站可以查询软件包信息：

https://admin.fedoraproject.org/pkgdb/

## 共享文件夹

我用的是VirtualBox的虚拟环境，因此除了在VirtualBox中，设置共享文件夹之外，还需对Fedora进行如下操作：

1.添加用户到vboxsf中。

`usermod -a -G vboxsf <your user name>`

2.重启。（这一步必不可少，否则上面的配置不会生效。）

这样就可以在Fedora中浏览共享文件夹了。

# diff&patch

diff/patch这对工具在数学上来说，diff是对2个集合求差，patch是求和。

{% highlight bash %}
diff -uNr A B > C #生成A和B的diff文件C,-uNr为最常用的选项
patch A C #给A打上diff文件得到B
patch -R B C #B还原为A
{% endhighlight %}

# 批量patch

## 给目录应用patch。

`patch -p1 <1.patch`

这种情况适合1.patch中包含对多个文件的修改时。

## 批量应用patch

有的时候，patch不是一个patch文件，而是一个目录中的若干个patch文件。这时可用如下办法：

`find . -name "*.patch">1.txt`

`sort 1.txt | xargs cat >2.patch`

`patch -p1 <2.patch`

# bash

## 查看当前使用的shell

实时查看：

`ps |  grep $$  |  awk '{print $4}'`

非实时查看：

`echo $SHELL`

## return和exit的区别

return用于函数的返回，它只能用在函数中。

exit用于整个shell脚本的退出。

## 预定义变量

`$?`: 上条命令的返回值

`$$`: 当前shell的PID。

# SWIG

Simplified Wrapper and Interface Generator可以为C/C++的库生成其他高级脚本语言的Wrapper。

官网：

http://swig.org/


