---
layout: post
title:  从版本库看开源项目的发展史, WebKit, CMake, MSYS2
category: technology 
---

* toc
{:toc}

# 前言

自从发现git和github之后，由于git方便的查看版本历史的功能，使我能够对一些著名开源项目的发展史有一定的了解。并以此为契机，一定程度的揭开开源项目的运作之谜。

PS：不要提svn的版本历史，那个在局域网里查看还算方便，对于互联网的查看来说，即使只看最近1000条，也要耗费非常非常多的时间，而且还是每次查看都是这样费劲。

# 从git log看linux的发展史

linux稳定版本的git地址:

git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git

这个版本的提交那叫一个多啊，截止2014年12月25日，已经有482338次提交。这个次数多到什么程度呢？直接让gitk的内存占用超过2.5G，即使这样也才加载到40W次左右，还剩下8W次没有加载呢。。。

所以在万般无奈的情况下，只好使用原始的git log命令，这才统计出刚才的数字。相关命令如下:

`git log --pretty=format:'%h by %an at %cd'`

可能是git时间一长，就太大了的缘故吧，所以还有一个更古老的git地址：

git://git.kernel.org/pub/scm/linux/kernel/git/tglx/history.git

这里面是2002年至2005年期间的代码，估计以当时的网络条件，传这么个165M的庞然大物，也是件浩大的工程了。

（PS：了解BitKeeper和Git的恩恩怨怨的同学，估计看到“2002年至2005年”，应该就能猜出这个git是怎么回事了吧。我好奇的是，在BitKeeper之前，Linus用什么来管理代码。2015.3.20）

这里有一个发现，Linus早先在Transmeta公司干活，这也是他打工时间最长的公司，有兴趣的同学可以自行wiki一下相关的信息。

最后再吐槽一下git，这么大的库，居然不支持断点续传，也不支持object的原子下载。。。相比之下，svn虽然也不能断点续传，但是这次下载一个文件，下次就不用再下载了。考虑到代码文件也不可能太大，这样也就基本够用了。

2017.3

提交数量至662282。

# 从git log看git的发展史

Linux源代码由于历史久远，最初的版本历史已不可考证，至少无法从git log考证。但git本身的历史，则是一笔笔的都记录在git log中。

网上的说法，git是Linus一个周末的产物。但从log来看，项目开始的时间——2005.4.8，那是个周五。所以这个说法不成立。不过看了下代码的长度，也就是2000多行吧，理论上的确是一个周末的工作量。

从初始版本还可以看出，最早的git并不是单个可执行文件，而是一个子命令对应一个可执行文件。而且命令的名字也不是clone、pull之类的东西。

git-pull是在2005.4.19添加的，是个bash脚本。实际上，git所采用的设计思路和通常的教科书里的自顶而下的方法相反。它是自底而上，首先设计基本数据结构和算法，然后再在使用中，根据实际的需求，编写各种脚本，以使之便于使用。这种设计思路在当时看不出有多大的好处。但时至今日，随着各种项目的进展，版本库的规模日益增长，某些项目的提交总数甚至达数十万之巨，而git仍能高效处理。这不得不使人佩服Linus的设计功力。

2005.4.24 添加了网络传输的功能，这样git才算是个完整的分布式版本控制系统。

2005.6.8 添加cvs到git的工具。

2005.7.11 第一个里程碑版本0.99发布。

2005.12.21 v1.0发布。

从上面的历程可以看出，即使参与项目的人都是牛人，git也差不多经过了8个多月，近3000次commit，才达到了基本可用的水平。绝不是什么天才人物一周时间的作品。

# 从git log看svn的发展史

这个标题并没有错。众所周知的，svn查看修改历史是需要远程登录服务器的。而这对于一些历史悠久的版本库来说，简直是是个灾难。因此这里的log实际上是从Apache官方的git服务器上git下来的。

svn的开端实际上和git是有区别的。这个项目从2000.3.1开始。但是最先动手的，并非代码，而是文档。三个作者足足讨论了3个月，才在2000.6.10提交了第一行代码。而git一开始就是代码。

这实际上是有原因的。svn的目的是替代cvs，因此它的主要任务是修补cvs设计中不好的地方。因此它在设计阶段花费的时间较长。

而git在设计之初，有BitKeeper和Monotone作为参考，且从功能角度而言，BitKeeper并无重大问题，因此设计的任务并不是很大。

反倒是开源社区急需一个产品用以摆脱BitKeeper，因此git一上来就是代码，也就不难理解了。

顺便说一句，Mercurial只比git晚了10天诞生。可见那时候开源社区对于分布式版本控制系统的需求之迫切了。

# 从git log看sqlite的发展史

第一版发布于2000.5.29，已经有相当多的代码。

项目的committer只有9人，其中九成以上的代码出自一个人。

这个项目最初使用CVS管理代码，2009.8.12开始该用fossil管理代码。顺便提一句fossil使用sqlite作为对象存储的工具。

# 从git log看emacs的发展史

很遗憾，早期的历史在log中，已经残缺不全了，

按照wiki的说法：

* GNU Emacs最早广泛发布的版本是15.34，出现于1985年。

而现有代码库的第一个可用的版本是从大约1990年开始的，之前的历史版本虽然有，但是根本不可编译使用。

当时这个项目使用RCS管理代码，这也是发展至今的诸多开源软件中很少见的一例。因为同时期大部分的软件，都已经成为了历史。而像emacs这样，至今仍然相当活跃的项目实在是凤毛麟角。

# 从git log看SDL的发展史

SDL尽管使用广泛，但从代码来看基本上是Sam Lantinga的个人作品。

Sam Lantinga早年创建了Loki Software，专门将Windows游戏移植到Linux平台上，SDL正是这个时期的产物。著名的《英雄无敌3》Linux版就是Loki Software制作的。

后来他先后供职于Blizzard Entertainment和Valve Software，是暴雪的主力程序员之一。

# WebKit

WebKit的代码可以从它的官网www.webkit.org下获得。

在以下网页可以获得webkit向各种GUI移植的相关信息。

http://trac.webkit.org/wiki

由于获得的代码比较新，所以在linux平台下常有一些组件由于过于古老而导致编译失败。所以需要使用yum或者apt-get之类的工具从网上更新相关的组件。这里不推荐使用RHEL或者CentOS之类的服务器版本，因为服务器版本为了追求稳定性，不但组件不是最新的，就连网上的组件源也不是最新的。

可以使用ubuntu 9.04桌面版，不过里面缺少很多开发用的组件，除了

http://trac.webkit.org/wiki/BuildingGtk

列出的之外，还有不少组件需要下载。主要有：

1)autoconf

2)libtool

3)gtk-doc-tools

4)libgail-dev

参考：

https://mp.weixin.qq.com/s/QqpPGWf3IVEDN1t80CZ06Q

深入理解浏览器原理

https://www.zhihu.com/answer/1200063036

据报道称“浏览器内核有上千万行代码”，浏览器内核真的很复杂吗？

https://zhuanlan.zhihu.com/p/96986818

万字详文：深入理解浏览器原理

# CMake

1. 添加头文件目录

`include_directories(../../../thirdparty/comm/include)`

2. 添加需要链接的库文件目录

`link_directories("/home/server/third/lib")`

3. 查找库所在目录

`find_library(RUNTIME_LIB rt /usr/lib  /usr/local/lib NO_DEFAULT_PATH)`

4. 添加需要链接的库文件路径

`link_libraries(“/home/server/third/lib/libcommon.a”)`

5. 设置要链接的库文件的名称

`target_link_libraries(myProject libcomm.so)`

6. 为工程生成目标文件

`add_executable(demo main.cpp)`

参考：

https://www.cnblogs.com/binbinjx/p/5626916.html

cmake添加头文件目录，链接动态、静态库

## cross compile

需要用`-DCMAKE_TOOLCHAIN_FILE=XXXX`来指定toolchain file。后者的示例如下：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/other/toolchain-arm-imx8qm.cmake

# MSYS2

2015.11

稍早的时候，我在安装pygtk的环境时，发现pygi-aio安装程序提供的GTK版本已经到了3.14。但GTK官方这时仍然没有进展，版本停留在3.6.4。于是我又打算使用pygtk提供的更新来进行编程，但pygtk只提供了.dll文件，而没有.h和.lib文件，实际上并不好用，因此只得放弃之。

最近再上官网，发现其已经提供了更好的解决办法——MSYS2。MSYS2是Martell Malone维护的一个开源项目，旨在提供一个方便易安装的MSYS开发环境，其中也包括为各种开发包提供更新维护，GTK就是其中之一。（当前的版本为Gtk 3.18.3）

现将安装步骤罗列如下：

1.在http://www.msys2.org/，下载安装程序，并按照网页提示，更新pacman。

2.使用pacman下载必要的开发包。

`pacman -S autoconf autogen automake-wrapper pkg-config make gcc gdb`

`pacman -Ss <name>`: 查询相关的软件包是否存在。

`pacman -Syu`: 更新已安装的软件包。

MSYS2提供的环境除了安装友好，便于更新之外，对bash的支持也优于之前的版本。现在已经不需要单独为Windows平台提供特殊的makefile文件了。

还可在如下网址查询packages：

https://github.com/Alexpux/MINGW-packages

https://github.com/Alexpux/MSYS2-packages

MSYS2提供了两套API：mingw32和mingw64。可用以下方法（以gcc为例）查询需要安装的package的名字：

`pacman -Sl | grep gcc`

与MSYS2类似的还有Cygwin、MinGW等。

参见：

https://www.zhihu.com/question/22137175

Cygwin和MinGW的区别与联系是怎样的？

# WSL

Cygwin：提供了兼容POSIX接口的应用层接口，性能不佳。

MinGW：直接接Win32 API，只移植了GNU工具链，功能不全。

MSYS2：MinGW+Cygwin部分工具包+pacman。

WSL（Windows Subsystem for Linux），刚出来的时候叫“Bash on Ubuntu on Windows”：POSIX接口直接接到NT内核，性能超过Cygwin。

WSL2：虚拟机，有独立的Linux内核。和VirtualBox之类类似。

https://www.zhihu.com/question/50144689

win10 linux子系统和cygwin有什么不同？

https://mp.weixin.qq.com/s/ZCkboBBFYFm57pLEGftpCw

双系统的日子结束了：Windows和Linux将合二为一

https://zhuanlan.zhihu.com/p/57774611

盘点与Cygwin相似和相反的项目
