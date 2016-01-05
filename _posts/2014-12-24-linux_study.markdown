---
layout: post
title:  linux学习心得
category: technology 
---

## 动态链接

这两天实践了一下怎样在linux下创建动态链接。感觉网上的资料虽然翔实，但仍然有疏漏之处。

1）g++和gcc的区别

本来只想给链接的，以显示这不是我的原创。但是现在的链接失效的也太快了。。。囧

只好拿华丽的分隔符来表示引用的内容。

****
误区一:gcc只能编译c代码,g++只能编译c++代码

两者都可以，但是请注意：

1.后缀为.c的，gcc把它当作是C程序，而g++当作是c++程序；后缀为.cpp的，两者都会认为是c++程序，注意，虽然c++是c的超集，但是两者对语法的要求是有区别的。C++的语法规则更加严谨一些。

2.编译阶段，g++会调用gcc，对于c++代码，两者是等价的，但是因为gcc命令不能自动和C＋＋程序使用的库联接，所以通常用g++来完成链接，为了统一起见，干脆编译/链接统统用g++了，这就给人一种错觉，好像cpp程序只能用g++似的。
 
误区二:gcc不会定义__cplusplus宏，而g++会

实际上，这个宏只是标志着编译器将会把代码按C还是C++语法来解释，如上所述，如果后缀为.c，并且采用gcc编译器，则该宏就是未定义的，否则，就是已定义。
 
误区三:编译只能用gcc，链接只能用g++

严格来说，这句话不算错误，但是它混淆了概念，应该这样说：编译可以用gcc/g++，而链接可以用g++或者gcc -lstdc++。因为gcc命令不能自动和C＋＋程序使用的库联接，所以通常使用g++来完成联接。但在编译阶段，g++会自动调用gcc，二者等价。
****

因此，某些时候编不过去，可以试试换换cc的值。

2）gcc4.1.1下似乎对类型检查严了一些，dlsym返回的void*类型不能转换为相应的函数指针类型，需要强制转换。某些网上的例子在这里编不过去。

3）显式调用时,要注意动态库函数的声明,可能要加`extern "C"`才能正常执行。（显式调用是运行时加载，所以编译能过，执行却不对了。）可以用nm命令看看链接库的符号表，以确定问题所在。

4）链接的时候需要注意文件的顺序。

比如下面的例子：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/helloworld/linux_so

`gcc -o main_link main_link.c -L. -lhello`

这条命令中的main_link.c如果放到`-lhello`之后就会出问题。也考虑使用`--start-group`和`--end-group`之类的链接选项解决链接顺序问题。

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

## pkg-config

一般来说，如果库的头文件不在/usr/include目录中，那么在编译的时候需要用-I参数指定其路径。由于同一个库在不同系统上可能位于不同的目录下，用户安装库的时候也可以将库安装在不同的目录下，所以即使使用同一个库，由于库的路径的不同，造成了用-I参数指定的头文件的路径和在连接时使用-L参数指定lib库的路径都可能不同，其结果就是造成了编译命令界面的不统一。可能由于编译，连接的不一致，造成同一份程序从一台机器copy到另一台机器时就可能会出现问题。

pkg-config 就是用来解决编译连接界面不统一问题的一个工具。

它的基本思想：pkg-config是通过库提供的一个.pc文件获得库的各种必要信息的，包括版本信息、编译和连接需要的参数等。需要的时候可以通过pkg-config提供的参数(–cflags, –libs)，将所需信息提取出来供编译和连接使用。这样，不管库文件安装在哪，通过库对应的.pc文件就可以准确定位,可以使用相同的编译和连接命令，使得编译和连接界面统一。

参见：

http://www.mike.org.cn/articles/description-configure-pkg-config-pkg_config_path-of-the-relations-between/

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