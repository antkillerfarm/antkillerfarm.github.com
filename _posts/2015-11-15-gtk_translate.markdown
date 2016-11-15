---
layout: post
title:  Transifex与GTK文档翻译, Linux镜像文件, 外设接口杂谈
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

# GLib的Socket操作

示例如下：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/helloworld/glib/network

需要注意的是，此例中Server端采用的是阻塞式操作，因此会将main loop阻塞住。如果main loop需要处理其他事件的话，这里可使用GThreadedSocketService启动单独的线程，处理之。

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

mtd-utils还可用于烧写分区。例如如下命令：

`mtd write xyz.uimage linux`

其中`xyz.uimage`是镜像文件名，`linux`是分区名称。

参考：

http://blog.csdn.net/andy205214/article/details/7390287

从代码来查看板子的MTD分区方案，主要是搜索mtd_partition类型的使用定义。比如mini2440板子的分区方案可在mini2440_default_nand_part数组中查到。

# 外设接口杂谈

## USB_IN和USB_OUT

粗看这个也觉得奇怪，USB总线是双向传输的，怎么还有IN和OUT的区别。后来才知道这是相对于USB HUB而言的。靠近根节点的那边是IN，靠近设备的那边是OUT。这里之所以用“靠近”这个词，是因为USB是树状结构的，USB HUB相当于是这个树中，层与层之间的连接器。

USB HUB的典型实例是用的比较广的1路转2路或者1路转4路的USB扩展器。

## UART

UART的硬件形态分为TTL、RS232和RS485等，其中最常用的是RS232，也就是有的PC主板上提供的那种9针的接口。

从原理来讲，RS485和USB类似都采用了双绞线和差分编码。

## 各种外设接口物理层对比

这里不打算罗列各种外设接口的规格参数，也不打算给出一个表格对比各个方面。我的目的是在对比各种外设接口的过程中，总结设计外设接口所需要注意的地方。

### 1.数据线

外设接口的目的是传输数据，因此数据线是必不可少的。由于一根数据线无法同时处于两种状态，因此，必须有两根独立的数据线，才能实现真正的全双工。否则，至多是半双工。

### 2.时钟线

时钟用于划分数据位，起到标尺的作用，否则通信双方是无法理解一段高电平表示的是多少个“1”。（这里假设高电平表示“1”）

时钟有三种形式：

1.约定式。通信双方约定一个时钟频率进行通信。最典型的是UART接口。这种方式的好处是无需时钟线。但由于两侧时钟是异步时钟，时间长了之后就会出现同步错误，因此这种方式只适合慢速接口。一个改进的方法是通过在数据中加入停止位，来纠正累积的时钟同步误差。

2.时钟线。这种方式通常设计为主从式，即主设备提供时钟信号，而从设备利用该信号同步自己的时钟。比较典型的是I2C、I2S、SPI。

3.时钟线和数据线混合式。典型如USB所采用的差分编码，虽然是两根数据线D+和D-，但从信息量来说，相当于其他接口的数据线+时钟线。也可以换句话来说，由于时钟和数据是信息中相互独立的分量，因此，无论采用何种编码方式，都不能以少于2根线的方式提供完整的数据线+时钟线的功能。

正因为如此，USB 2.0虽然有两根数据线，但它实际上只是个半双工接口。而USB 3.0在USB 2.0的基础上，又添加了两根数据线，才成为了真正的全双工设备。

### 3.数据有效

分为电平式和边沿式两种。前者如I2C，只允许数据在时钟信号为低电平时改变，后者如SPI，规定数据在时钟信号的边沿有效。

## I2C

![](/images/article/i2c.png)

I2C的读写时序一般如上图所示。从中可以看出I2C的数据由从机地址、读写标志位、寄存器地址和普通数据位组成。其中后面的三部分在其他的外设接口中也能见到，意义大致相同，这里就不赘述了。这里重点谈谈从机地址。

I2C相比于UART和SPI，其优点在于一个接口可以外接多个设备（多个从设备的情况较多）。从机地址就是用于区分这些设备的。以7位从机地址为例，高4位一般由设备厂家分配设定，低3位由用户设定。因此一个I2C总线上可以挂接多个同类设备，只要用户设定好它们的地址就可以了。与SPI的片选不同，I2C的用户设定位采用连接上下拉电阻的方式设定，而不用连接到主设备上。

在Linux内核中，使用I2C_BOARD_INFO宏设置从机地址。

## SMBus

SMBus(System Management Bus,系统管理总线)是1995年由Intel提出的，应用于移动PC和桌面PC系统中的低速率通讯总线。由于它大部分基于I2C总线规范，因此在Linux内核中，被归类为I2C总线。

