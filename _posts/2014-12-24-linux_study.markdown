---
layout: post
title:  linux学习心得（一）
category: technology 
---

## GUI设计工具

GTK+的程序可以使用Glade来设计界面，它会生成一个脚本，运行该脚本，并make就行了。

QT的稍微麻烦一些。

1）首先打开QT Designer（新建时，不能使用KDE Designer，但修改建好后的工程可以，不知道是BUG，还是怎么回事）新建一个窗体，然后新建一个main.cpp，并将刚才生成的窗体选为主窗体。

2）打开KDev C/C++，选择导入工程，选择QMake Based导入，然后即可在IDE中编译并运行。

上面写的两个都是官方的设计器，而WxWidgets没有官方的设计器，但有很多第三方的设计器，我使用的是免费且开源的wxFormBuilder。用它可生成XRC文件，而WxWidgets中有使用XRC文件的接口。

## 编程所用命令简介

cc：C/C++编译器

as：GNU汇编编译器

ld：链接器

as86、ld86：8086汇编编译器和链接器

linux文件分割用split,合并用cat。最近下了一本采用split分割的书，但是我没有Linux环境，于是在windows的命令行下用type、>、>>合并了文件。

## printf和wprintf混用的问题

在linux中不可混用printf和wprintf，如果混用的话，则**后使用**的函数没有输出。

例如

{% highlight c %}
printf("a\n");
wprintf(L"b\n");
{% endhighlight %}

输出为：

a

而

{% highlight c %}
wprintf(L"b\n");
printf("a\n");
{% endhighlight %}

输出为：

b

关于这个问题的讨论见

http://bytes.com/groups/c/852681-wprintf-conflicts-printf-glibc-bug

解决方法统一使用一种函数

例如：

{% highlight c %}
wprintf(L"%s","a\n");
wprintf(L"b\n");
{% endhighlight %}

或

{% highlight c %}
printf("a\n");
printf("%ls",L"b\n");
{% endhighlight %}

## 关于SIGPIPE导致的程序退出

http://www.cppblog.com/elva/archive/2008/09/10/61544.html

## 使用yum

在RHEL中，可以使用yum从网上下载相应的组件，但需要RHN号，所以我现在换用了CentOS。当你需要使用yum的时候，如果yum找不到相应的组件时，可以在组件名之前加lib，或者在之后加-dev或-devel。

## 源码包编译4步曲

1)autogen.sh

2)configure

3)make

4)su -c "make install"

其中一二两步有时只要一个就够了，如果源码包中这两个都有的话，先运行1)

## 关于ascii字符集的一些打印控制字符的别名

\b——backspace

\f——pagebreak

\v——vertical tab

## 二进制文件与ASCII、Base64之间的转换

xxd：这个命令可以将二进制文件转换成ASCII码表示文本文件。支持2、8、16等多种进制的ASCII表示形式，还支持输出成C语言格式的数组声明。反过来的转换也同样支持。

uuencode and uudecode：支持二进制文件与Base64之间的转换。

## start-stop-daemon

该命令用于启动和停止系统守护程序。

## popen

popen()函数通过创建一个管道，调用fork 产生一个子进程，执行一个shell以运行命令来开启一个进程。也就是说这个函数可以执行shell命令，而且还可以用fread或fgets来获取命令执行后的输出结果。

例子如下：

{% highlight c %}
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
{% endhighlight %}

## 软件包管理工具

各大linux发行版都有自己的软件包管理工具。比如Debian/Ubuntu使用的apt-get，Red Hat/Fedora使用的yum/dnf，SUSE/openSUSE使用的zypper，Gentoo使用的emerge，Arch Linux使用的pacman。

各大软件包管理工具的功能对比，可参见：

https://wiki.archlinux.org/index.php/Pacman/Rosetta

## IO多路复用

参考文献：

http://www.cnblogs.com/Anker/p/3265058.html

### select函数

{% highlight c %}
int select(int maxfdp1,fd_set *readset,fd_set *writeset,fd_set *exceptset,const struct timeval *timeout)
{% endhighlight %}

函数参数介绍如下：

第一个参数maxfdp1指定待测试的描述字个数，它的值是待测试的最大描述字加1（因此把该参数命名为maxfdp1），描述字0、1、2...maxfdp1-1均将被测试，因为文件描述符是从0开始的。

中间的三个参数readset、writeset和exceptset指定我们要让内核测试读、写和异常条件的描述字。如果对某一个的条件不感兴趣，就可以把它设为空指针。struct fd_set可以理解为一个集合，这个集合中存放的是文件描述符，可通过以下四个宏进行设置：

{% highlight c %}
void FD_ZERO(fd_set *fdset);           //清空集合
void FD_SET(int fd, fd_set *fdset);   //将一个给定的文件描述符加入集合之中
void FD_CLR(int fd, fd_set *fdset);   //将一个给定的文件描述符从集合中删除
int FD_ISSET(int fd, fd_set *fdset);   // 检查集合中指定的文件描述符是否可以读写
{% endhighlight %}

（3）timeout告知内核等待所指定描述字中的任何一个就绪可花多少时间。其timeval结构用于指定这段时间的秒数和微秒数。

这个参数有三种可能：

（1）永远等待下去：仅在有一个描述字准备好I/O时才返回。为此，把该参数设置为空指针NULL。

（2）等待一段固定时间：在有一个描述字准备好I/O时返回，但是不超过由该参数所指向的timeval结构中指定的秒数和微秒数。

（3）根本不等待：检查描述字后立即返回，这称为轮询。为此，该参数必须指向一个timeval结构，而且其中的定时器值必须为0。

### poll函数

{% highlight c %}
int poll ( struct pollfd * fds, unsigned int nfds, int timeout);

struct pollfd {
int fd;         /* 文件描述符 */
short events;         /* 等待的事件 */
short revents;       /* 实际发生了的事件 */
} ; 
{% endhighlight %}

事件包括：

{% highlight text %}
POLLIN 　　　　　　　　有数据可读。
POLLRDNORM 　　　　  有普通数据可读。
POLLRDBAND　　　　　 有优先数据可读。
POLLPRI　　　　　　　　 有紧迫数据可读。
POLLOUT　　　　　　      写数据不会导致阻塞。
POLLWRNORM　　　　　  写普通数据不会导致阻塞。
POLLWRBAND　　　　　   写优先数据不会导致阻塞。
POLLMSGSIGPOLL 　　　　消息可用。
POLLER　　   指定的文件描述符发生错误。
POLLHUP　　 指定的文件描述符挂起事件。
POLLNVAL　　指定的文件描述符非法。
{% endhighlight %}

### epoll接口

http://www.cnblogs.com/Anker/archive/2013/08/17/3263780.html

## 启动脚本

Linux启动时，运行一个叫做init的程序，然后由它来启动后面的任务，包括多用户环境，网络等。

那么，到底什么是运行级呢？简单的说，运行级就是操作系统当前正在运行的功能级别。这个级别从1到6，具有不同的功能。这些级别在/etc/inittab 文件里指定。这个文件是init程序寻找的主要文件，最先运行的服务是那些放在/etc/rc.d 目录下的文件。

大多数的Linux发行版本中，启动的是/etc/rc.d/init.d。这些脚本被ln命令来连接到 /etc/rc.d/rcn.d目录。(这里的n就是运行级0-6)

例如/etc/rc.d/rc2.d下面的S10network就是连接到/etc/rc.d/init.d下的network脚本的。

因此，我们可以知道，rc2.d下面的文件就是和运行级2有关的。

文件开头的S代表start就是启动服务的意思，后面的数字10就是启动的顺序。例如，在同一个目录下，你还可以看到 S80postfix这个文件，80就是顺序在10以后，因为没有启动网络的情况下，启动postfix是没有任何作用的。

再看一下/etc/rc.d/rc3.d，可以看到文件S60nfslock，但是这个文件不存在于/etc/rc.d/rc2.d 目录下。NFS要用到这个文件，一般用在多用户环境下，所以放在rc3.d 目录下。

另外，在/etc/rc.d/rc2.d还可以看到那些K开头的文件，例如

/etc/rc.d/rc2.d/K45named，K代表kill。

标准的Linux运行级为3或者5，如果是3的话，系统就在多用户状态。如果是5的话，则是运行着X Window系统。如果目前正在3或5，而你把运行级降低到2的话，init就会执行K45named脚本。

不同的运行级定义如下：

{% highlight text %}
0 - 停机（千万不要把initdefault 设置为0 ）
1 - 单用户模式
2 - 多用户，但是没有 NFS 
3 - 完全多用户模式
4 - 没有用到
5 - X11 
6 - 重新启动 （千万不要把initdefault 设置为6 ）
{% endhighlight %}

## uint8_t

对于要求严格位宽的场合，应用程序可以使用uint8_t，uint16_t，uint32_t来获得移植时的一致性。它的头文件是inttypes.h。

## Linux Shell

### 空格和TAB的细节

在大多数编程语言中，空格和TAB都不是有意义的字符，有没有或者有多少都无所谓。但Linux Shell不是这样的，尽管它看起来并没有如python那样的对缩进的强制语言规定。

以下是它在使用空格和TAB时的一些细节：

1.makefile文件中，规则定义部分的shell脚本命令需要使用TAB开头，使用空格会出错。

2.if命令的格式：

{% highlight bash %}
if [ "$yn" == "Y" ] || [ "$yn" == "y" ]; then
	echo "OK, continue"
fi
{% endhighlight %}

注意一下上面脚本中的表达式[]之间的部分。其中所使用的空格不可省略，否则会出错。bash在处理[]表达式的时候，实际上调用了`/usr/bin/[`命令。如果把`[`当作特殊名称的普通命令的话，就会发现这里的空格用法实际上并不奇怪。

## 设置随机的MAC地址

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

## awk&sed&grep

这三个工具是文本处理中用的比较多的工具，各有各的特色，且都支持正则表达式。

一般来说，行处理优先考虑使用sed和grep，列处理优先考虑使用awk。通常情况下，组合使用多个命令，其命令编写的难度小于只使用一个命令。比如sed和grep也可以进行列处理，但语法难度远超awk，反之亦然。

这里不打算列出各个命令的选项，而仅列出使用它们的一些示例：

这里假设我们有一文件名为ab。

### awk

{% highlight bash %}
awk '{print $1}' ab #显示第一列
{% endhighlight %}

### sed

{% highlight bash %}
sed '1d' ab #删除第一行 
sed '$d' ab #删除最后一行
sed '1,2d' ab #删除第一行到第二行
sed '2,$d' ab #删除第二行到最后一行

sed -n '1p' ab    #显示第一行 
sed -n '$p' ab    #显示最后一行
sed -n '1,2p' ab  #显示第一行到第二行
sed -n '2,$p' ab  #显示第二行到最后一行

sed -n '/ruby/p' ab #查询包括关键字ruby所在所有行
sed -n '/\$/p' ab #查询包括关键字$所在所有行，使用反斜线\屏蔽特殊含义

sed -n '/ruby/p' ab | sed 's/ruby/bird/g'    #替换ruby为bird
sed -n '/ruby/p' ab | sed 's/ruby//g'        #删除ruby
{% endhighlight %}

### grep

{% highlight bash %}
grep 'ruby' ab #查询包括关键字ruby所在所有行
{% endhighlight %}

### 综合

{% highlight bash %}
ip addr show br-lan | grep 'inet ' | awk  '{print $2}' | sed 's/\/.*//g'
{% endhighlight %}

## diff&patch

diff/patch这对工具在数学上来说，diff是对2个集合求差，patch是求和。

{% highlight bash %}
diff -uNr A B > C #生成A和B的diff文件C,-uNr为最常用的选项
patch A C #给A打上diff文件得到B
patch -R B C #B还原为A
{% endhighlight %}
