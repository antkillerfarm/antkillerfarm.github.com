---
layout: post
title:  Linux学习心得（三）
category: linux 
---

* toc
{:toc}

# minicom

1.查看串口设备

`ls -l /dev`

2.连接串口设备

`minicom -D /dev/ttyS0`

3.菜单

Ctrl+A Z

4.退出

Ctrl+A X

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

tmux也有一些高端功能，比如分屏等。

常用命令：

`Ctrl+b c`: 创建窗口。

`Ctrl+b "`: 上下分屏。

`Ctrl+b %`: 左右分屏。

`Ctrl+b z`: 最大化/恢复原状。

`Ctrl+b d`: 不销毁进程退出（detach）。

`Ctrl+b x`：关闭当前窗格。

`Ctrl+d`: 销毁进程退出。

`tmux ls`: 查看会话。

`tmux attach -t 0`: 重新接入某个已存在的会话。

复制粘贴有两种方法：

- 方法1：按住Shift，使用系统的复制粘贴。
- 方法2：`ctrl+b [`: 复制。 `ctrl+b ]`: 粘贴。

屏幕滚动：

`ctrl+b [`，就可以用鼠标滑轮滚动了，按q退出。

常用配置：`~/.tmux.conf`：

```bash
set -g mouse on
set -g history-limit 100000
```

如果不生效的话，可以用`tmux source ~/.tmux.conf`使其立即生效。

参考：

https://linuxtoy.org/archives/from-screen-to-tmux.html

从screen切换到tmux

http://mingxinglai.com/cn/2012/09/tmux/

tmux的使用方法和个性化配置

http://chengjin.li/2017/08/09/tmux-using-tutorial/

终端复用工具---tmux的安装及使用

https://www.ruanyifeng.com/blog/2019/10/tmux.html

Tmux使用教程

https://blog.csdn.net/mainking2003/article/details/120504004

用shell脚本运行Tmux

# 设置终端颜色

`printf "\033[1;42mH\033[4;32;49me\033[5ml\033[7ml\033[0;32mo\033[0m\n"`

参考：

https://zhuanlan.zhihu.com/p/81954911

如何改变你的终端颜色

http://easyos.net/articles/bsd/freebsd/output_control_in_freebsd_console

https://en-m.jinzhao.wiki/wiki/ANSI_escape_code

https://mp.weixin.qq.com/s/Q8RkA4cjko-sD5jn7NGzGw

一行命令堆出你的新垣结衣，不爆肝也能创作ASCII Art

# MOTD

motd：message of the day。即系统登陆时，通过终端展示给登陆用户的消息。

静态MOTD: `/etc/motd`

动态MOTD: `/etc/update-motd.d/`

https://www.cnblogs.com/gageshen/p/11565980.html

Linux中创建自己的MOTD

# 设置随机的MAC地址

1.设置MAC地址

`ifconfig eth0 hw ether 477265656e00`

其中eth0是网口的名称，477265656e00是要设置的MAC地址（十六进制）。

2.生成随机数

随机数的生成在Linux中有多种方法，这里使用openssl。因为它和MAC都属于网络编程的范畴，同时使用的概率较大。

`openssl rand -hex 6`

3.SIOCSIFHWADDR: Cannot assign requested address错误

MAC地址的某些位有特定的含义，并不能随意设置。仍以477265656e00为例，第一个字节0x47的最后两位含义如下：

（00）统一管理的单播MAC

（01）统一管理的多播MAC

（10）本地管理的单播MAC

（11）本地管理的多播MAC

由于针对ADSL路由等这样的网络终端，一般使用的都是统一管理的单播MAC。

# 设置网卡eth0的IP地址和子网掩码

`sudo ifconfig eth0 192.168.2.1 netmask 255.255.255.0`

# 无线设置

查看无线网卡状态：

`iwconfig`

`wpa_cli`

扫描周围的wifi信号：

`iwlist scanning`

# 下载工具

wget和curl是最常见的下载工具。这里推荐使用axel，该工具支持多路http下载。

示例：

`wget -c <URL>`

`curl -C -o <file name> <URL>`

`axel <URL>`

`wget -b -i XX.txt`

`-b`：后台，`-i`：批量。

参考：

http://os.51cto.com/art/201605/511423.htm

Linux用户宝典：用于下载的十大命令行工具

# 动态库

## 版本设置

linux动态库使用soname来设定动态库的版本兼容性。

`g++ -fPIC -shared -Wl,-soname,libbar.so.1 -o libbar.so.1.1.0`

这个命令生成的动态库的名字为`libbar.so.1.1.0`，soname为`libbar.so.1`。

soname的意思是：形如`libbar.so.1.x.y`的动态库，都可以兼容。

cmake写法：

`SET_TARGET_PROPERTIES(hello PROPERTIES VERSION 1.2 SOVERSION 1)`

参考：

https://www.jianshu.com/p/931a814083ce

Linux动态库soname的使用

https://tldp.org/HOWTO/Program-Library-HOWTO/shared-libraries.html

Shared Libraries

## PIC

position-independent code

不加`-fPIC`编译的`.so`必须要在加载到用户程序的地址空间时重定向所有表目，所以在它里面不能引用其它地方的代码。

而多个进程引用同一个PIC动态库时，可以共用内存。这个库在不同进程中的虚拟地址不同，操作系统会把它们映射到同一块物理内存上。

cmake写法：

`set(CMAKE_POSITION_INDEPENDENT_CODE ON)`

# popen

popen()函数通过创建一个管道，调用fork产生一个子进程，执行一个shell以运行命令来开启一个进程。也就是说这个函数可以执行shell命令，而且还可以用fread或fgets来获取命令执行后的输出结果。

例子如下：

```c
int8_t strcmd[256];
memset(strcmd, 0 , sizeof(strcmd));
sprintf(strcmd, "cat /etc/resolv.conf | awk '{printf $2}'");
pfile = popen(strcmd, "r");
if (pfile != NULL){
	int8_t str[64];
	bzero(str, sizeof(str));
	fgets(str, sizeof(str), pfile);
	pclose(pfile);
}
```

# Linux知识图谱

![](/images/article/linux_perf_tools_full.png)

![](/images/img3/linux_kernel_map.png)

原图地址：

http://www.brendangregg.com/linuxperf.html

![](/images/img4/Linux-storage-stack-diagram_v4.10.png)

BSP: Board Support Package

参考：

https://mp.weixin.qq.com/s/-iCuxQjSghtDGaPMqaDSgQ

Linux思维导图整理

https://mp.weixin.qq.com/s/sLyD6z2xBXRxfBZnImTgtQ

40+张最全Linux/C/C++思维导图，收藏！

# 时间的表示方法

一般遵循ISO 8601标准：

https://www.w3.org/TR/NOTE-datetime

YYYY-MM-DDThh:mm:ss.sTZD (eg 1997-07-16T19:20:30.45+01:00)

其中的TZD表示time zone designator。

# 启动脚本

Linux启动时，运行一个叫做init的程序，然后由它来启动后面的任务，包括多用户环境，网络等。

那么，到底什么是运行级呢？简单的说，运行级就是操作系统当前正在运行的功能级别。这个级别从1到6，具有不同的功能。这些级别在/etc/inittab文件里指定。这个文件是init程序寻找的主要文件，最先运行的服务是那些放在/etc/rc.d目录下的文件。

大多数的Linux发行版本中，启动的是/etc/rc.d/init.d。这些脚本被ln命令来连接到/etc/rc.d/rcn.d目录。(这里的n就是运行级0-6)

例如/etc/rc.d/rc2.d下面的S10network就是连接到/etc/rc.d/init.d下的network脚本的。

因此，我们可以知道，rc2.d下面的文件就是和运行级2有关的。

文件开头的S代表start就是启动服务的意思，后面的数字10就是启动的顺序。例如，在同一个目录下，你还可以看到 S80postfix这个文件，80就是顺序在10以后，因为没有启动网络的情况下，启动postfix是没有任何作用的。

再看一下/etc/rc.d/rc3.d，可以看到文件S60nfslock，但是这个文件不存在于/etc/rc.d/rc2.d 目录下。NFS要用到这个文件，一般用在多用户环境下，所以放在rc3.d目录下。

另外，在/etc/rc.d/rc2.d还可以看到那些K开头的文件，例如

/etc/rc.d/rc2.d/K45named，K代表kill。

标准的Linux运行级为3或者5，如果是3的话，系统就在多用户状态。如果是5的话，则是运行着X Window系统。如果目前正在3或5，而你把运行级降低到2的话，init就会执行K45named脚本。

不同的运行级定义如下：

```text
0 - 停机（千万不要把initdefault设置为0）
1 - 单用户模式
2 - 多用户，但是没有NFS
3 - 完全多用户模式
4 - 没有用到
5 - X11
6 - 重新启动（千万不要把initdefault设置为6）
```

# U盘安装Linux

在程序中找到“启动盘创建器”即可。这也是防止变砖的最后一条路，反正BIOS是不可能安装坏的，随便折腾吧。

# wifi配置

Linux下的wifi配置主要使用iw系列命令，包括iw、iwconfig、iwlist、iwpriv。参见：

http://blog.csdn.net/liangyamin/article/details/7209761

Linux下的iwpriv（iwlist、iwconfig）的简单应用

# DTrace

https://zhuanlan.zhihu.com/p/24124082

动态追踪技术：Linux喜迎DTrace

# 大文件处理

在“大数据”时代，我们会经常遇到有大文本文件（上 GB 或更大）的情况。传统的文本编辑软件对处理这样的大文件不太有效，当我们试图打开一个大文件时会经常由于内存不足而郁闷的不行。

如果你只需要查看一个文本文件，并不对它做编辑，可以考虑下glogg。

`sudo apt install glogg`

如果需要修改的话，可以使用JOE。

`sudo apt install joe`

# 调整交换文件大小

```bash
fallocate -l 16G /swapfile
chmod 600 /swapfile
ls -lh /swapfile
mkswap /swapfile
swapon /swapfile
swapon --show
# Add this line to /etc/fstab to mount swap at boot
/swapfile swap swap defaults 0 0
swapoff /swapfile
```

# 剪贴板

Linux下的GUI程序的剪贴板功能一般是由X11提供的。

X11支持复数个剪贴板，每个剪贴板都有一个名字。一般常见/常用的：

PRIMARY剪贴板（选中的文本内容会被送到这个剪贴板，一般按鼠标中键可以粘贴）

CLIPBOARD剪贴板（更加接近于Windows下的剪贴板的存在，Ctrl-C/V功能一般作用于这个剪贴板）。

此外，X11的剪贴板里的内容似乎是由内容来源的程序来保存的，内容来源程序终止的话剪贴板就会被清空。这时一般需要一个常驻后台的剪贴板管理器（Clipboard Manager）来接管剪贴板的内容。

常用的Clipboard Manager的列表如下：

https://wiki.archlinux.org/title/Clipboard

相关的标准文件：

https://tronche.com/gui/x/icccm/

参考：

https://www.uninformativ.de/blog/postings/2017-04-02/0/POSTING-en.html

X11: How does “the” clipboard work?

https://wiki.archlinux.org/title/Clipboard

https://www.zhihu.com/question/66284095

电脑复制粘贴背后发生了什么？

https://linux.cn/article-7329-1.html

面向Linux的10款最佳剪贴板管理器
