---
layout: post
title:  Transifex与GTK文档翻译, Linux镜像文件
category: technology 
---

# Transifex与GTK文档翻译

## 参与GTK+开发的一段小经历（2013.10）

最近忽然对GTK+产生了浓厚的兴趣，打算研究一下。学习一个新东西，最好的方法就是先阅读一下它的文档。应该说GTK的文档虽然比不了MSDN，但也颇为详尽。主要的问题在于文档都是英文的，阅读起来比较吃力。

考虑到GTK已经有15年的历史，所以试着在网上搜了一下参考手册的中文版，结果找到了这个网址：http://code.google.com/p/gtk-doc-cn/

这是一个叫yetist的人发起的翻译项目。

http://forum.linuxfans.org/viewthread.php?tid=164933

https://wiki.ubuntu.org.cn/viewtopic.php?f=163&t=232005&start=15

这两个网页讲述了他发起这个项目的经历。

至于GTK官方的语言翻译地址，查了半天也没找到，所以也就没有办法贡献自己的力量了。

在学习兼翻译的过程中，发现了一个我搞不懂的地方，于是给GTK官网上的Bugzilla发了一个bug，结果很快就收到了回复，一段有趣而愉快的经历。看来以后可以多参加一些开源社区的活动，积攒自己的人品，^_^。

## Transifex（2015.6）

上面提到的yetist同学发起的翻译项目，使用Transifex作为翻译协作工具。其网址为：

https://www.transifex.com/projects/p/gobject-reference-manual/

yetist同学还同时主导了一系列的GTK文档翻译项目。有兴趣的同学，可以根据自己的情况，选择性的进行翻译。

2013年的时候，我曾经翻译了一部分GObject文档，并提交给yetist。但他近来已不活跃，未能及时处理我的提交请求。直到最近我才被接纳为GObject项目的翻译人员。于是就有了下面的对transifex的初体验。

另外说一下，我翻译的部分主要参考了TualatriX的成果，他的网址是：

http://imtx.me/

他也是ubuntu-tweak的作者。我经常用这个软件清理磁盘。

## GTK文档翻译流程

由于Google Code即将关闭，翻译项目的网址搬迁到：

https://github.com/yetist/gtk-doc-cn

或者也可以在我的github下找到这个项目的fork：

https://github.com/antkillerfarm/gtk-doc-cn

欢迎大家向yetist或者我的github提交翻译成果。

从项目的log可以看出，yetist同学不仅自己翻译了很多内容，而且还编写了整个翻译自动处理的框架。

该框架的大概流程如下：

1.下载代码，并使用gtk-doc工具，抽取代码中的注释，生成帮助文档的po文件。

2.将po文件发布到Transifex平台，由网上的翻译志愿者，翻译po文件。

3.将翻译好的po文件，合并到代码中，并生成最终的html格式的文档。其中合并和生成，都可以使用框架提供的命令来完成。

前两步yetist同学已经做好了，我们需要做的主要是第3步。

框架的基本用法见项目的说明文档，这里仅列举文档中未提到的细节：

1.环境准备

以Ubuntu为例，可用以下命令安装依赖软件包：

`sudo apt-get install python python-pip fam gtk-doc-tools gnome-doc-utils`

框架需要初始化：

`make init`

2.Transifex客户端

Transifex客户端用于同步Transifex平台的翻译更新，其使用方法和git颇为类似。它的官方地址如下：

http://docs.transifex.com/client/

以下仅列出必要的操作，完整的操作参见官网。

1）安装

`pip install transifex-client`

2）初始化

`tx init`

3）配置

`tx set --auto-remote <url>`

4）下载

`tx pull -a`

3.合并po文件

1）修改项目目录下的AUTHORS文件，添加你要翻译的po文件。

2）将翻译后的po文件，放到po或者xmlpo文件夹中，并改为上一步时你起的名字。

3）合并。

`make merge`

4）生成最终文档。

`make docs`

这里不知是否存在bug，有的时候这个命令需要运行两次，第1次失败，第2次就可以看到最终的文档了。

# Linux镜像文件

## vmlinux

这是源代码直接生成的镜像文件。以x86平台为例：

arch\x86\kernel\vmlinux.lds.S--这是链接脚本的源代码，经过C语言的宏预处理之后会生成vmlinux.lds，使用这个脚本，链接即可得到vmlinux，其过程与普通应用程序并无太大区别，也就是个elf文件罢了。

## image

vmlinux使用objcopy处理之后，生成的不包含符号表的镜像文件。这是linux默认生成的结果。

## zImage

zImage = 使用gzip压缩后的image + GZip自解压代码。使用`make zImage`或者`make bzImage`创建。两者的区别是zImage只适用于大小在640KB以内的内核镜像。

## uImage

uImage = uImage header + zImage。使用uboot提供的mkimage工具创建。

以上的这些镜像文件的关系可参见：

http://www.cnblogs.com/armlinux/archive/2011/11/06/2396786.html

http://www.linuxidc.com/Linux/2011-02/32096.htm

## Flash镜像

一般来说，一个完整的linux系统，不仅包括内核，还包括bootloader和若干分区。这些镜像文件散布，不利于批量生产的进行。这时就需要将之打包，并生成一个可直接用于生产烧写的Flash镜像。

可使用mtd-utils库中的ubinize工具生成Flash镜像。

mtd-utils的官网是：

http://www.linux-mtd.infradead.org/

安装方法：

`sudo apt-get install mtd-utils`

参考：

http://blog.csdn.net/andy205214/article/details/7390287

从代码来查看板子的MTD分区方案，主要是搜索mtd_partition类型的使用定义。比如mini2440板子的分区方案可在mini2440_default_nand_part数组中查到。

