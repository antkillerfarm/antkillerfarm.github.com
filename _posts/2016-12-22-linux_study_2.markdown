---
layout: post
title:  Linux学习心得（二）
category: linux 
---

* toc
{:toc}

# 文件操作

这里列举一些文件操作的命令，不详细讲解，仅供备忘。

mkfifo：创建命名管道。

remove：删除文件（包括命名管道）。

access：可查询文件是否存在及其相关权限。

# 文件锁

Linux系统上的文件锁主要分为协同锁(advisory lock)和强制锁(mandatory lock)。前者是应用层锁，实现方式与信号量相似。后者是内核层锁。

fcntl和flock都可以用于创建文件锁。

文件锁、命名管道和消息队列的示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/pipe/

# 主线程存续的编程技巧

有的多线程程序，其主要功能实现在其他线程中。主线程只是负责创建这些功能线程，一旦创建完成，自己的使命也就结束了。

如果需要让主线程在初始化之后，仍然存在，而不是退出的话，可以使用以下技巧：

```c
sigset_t sigs_to_catch;
sigemptyset(&sigs_to_catch);
sigaddset(&sigs_to_catch, SIGINT);
sigwait(&sigs_to_catch, &sig);
```

这种方法显然比`while (1);`这样的忙等待，有效率的多。

# 查看内存 & CPU

## top

top命令可在进程这一级查看内存、运行时间、CPU等的使用情况。并可根据不同属性对结果排序：

P：按%CPU使用率排序

T：按TIME+排序

M：按%MEM排序

注：运行top命令之后，输入相应字符即可切换排序。

除此之外，还有个升级版本的命令htop，以及htop的升级版——glances。

`sudo apt install glances`

另一个top的升级版：bottom

参考：

https://mp.weixin.qq.com/s/8892B95aQZHw4fZhi8iO5A

Linux中load average意义

https://mp.weixin.qq.com/s/FV13ma9LxI1gsgi9ovU4Mw

30个实例详解TOP命令

## free

free命令的内容比较概括，主要包含系统内存的整体使用情况，不深入到进程一级。

`free -m -s 5`：每5秒输出一次内存使用情况。

## bashtop

bashtop是一个用Bash编写的Linux资源监视器，可以显示处理器、内存、磁盘、网络和进程的使用情况与状态。

代码：

https://github.com/aristocratos/bashtop

它还有一个python的版本：

https://github.com/aristocratos/bpytop

参考：

https://mp.weixin.qq.com/s/3fNLa-yO4fUWO3nO9id1iA

这款Python版终端资源监控器，火了！

## 查看CPU核数

复杂版：

`cat /proc/cpuinfo`

简单版：

`lscpu`

>类似的还有：`lsusb`

极简版：

`nproc`

## 查看CPU温度

`sudo apt install psensor`

代码：

https://github.com/chinf/psensor

参考：

https://www.cnblogs.com/Black-Hawk/articles/14750866.html

如何在linux/unix系统中获取CPU温度

# 硬盘

## 查看磁盘空间大小

`df -hl`

## 查看文件夹大小

`du -sh *`

`ncdu`

`dutree`

https://github.com/nachoparker/dutree

参考：

https://mp.weixin.qq.com/s/ALyVaHfEfgPsT-N1KCGS3w

你知道du和df的统计结果为什么不一样

## 查看硬盘分区情况

`sudo fdisk -l`

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

## 文件管理器

ranger是一个字符式的文件管理器。

https://ranger.github.io/

# 查看网络

查看网络主要使用`ss`和`netstat`命令。

`nethogs`：按进程查看网速。

参考：

https://mp.weixin.qq.com/s/LwWhHvc2Zdvv2OYBG6kCyw

Linux调试三剑客——strace,lsof,tcpdump

https://mp.weixin.qq.com/s/yByM4-526GwIVdJC1ZoE-A

网络状态检测的利器-ss命令

https://mp.weixin.qq.com/s/-Plk7ZrqZFnf4hfP5FcvPQ

Linux网络状态工具ss命令使用详解

https://mp.weixin.qq.com/s/Xn6VTXBCP1t-lVnHmroEGQ

Linux网络分析必备技能：tcpdump实战详解

https://mp.weixin.qq.com/s/DPK00_AkQVMMLf_lByCimQ

Linux网络流量监控利器iftop中文入门指南

# Linux命令/工具

https://mp.weixin.qq.com/s/7ZVpdQOwNkIjW-16Mp8dcA

Linux企业运维人员最常用150个命令汇总

https://mp.weixin.qq.com/s/qspyBQ7MKWBvIQjt0vcBGw

Linux运维必备的40个命令总结

https://mp.weixin.qq.com/s/DG7RjJ_HXSRb6nAZuoTz3g

详解Linux中3个文件查找相关命令

https://www.cnblogs.com/traditional/p/12580638.html

psutil：获取硬件、网络相关信息，以及管理进程

https://www.cnblogs.com/saneri/p/7528283.html

python模块之psutil详解

https://mp.weixin.qq.com/s/EHAvQKsMqHqXW0dFIpSL0Q

Sysstat：开源免费的Linux系统的监控工具

https://mp.weixin.qq.com/s/-m8q2TzlfpvSaPRqGsQPbQ

一个惊人快速的终端录像工具，也能录制VSCode和Chrome窗口（t-rec）

https://mp.weixin.qq.com/s/qii7M6hOIBf9kgbmWpc3XQ

Linux交互式图形可视化磁盘使用软件（Filelight）

https://www.cnblogs.com/renshengdezheli/p/13427865.html

查询OS、CPU、内存、硬盘信息

https://mp.weixin.qq.com/s/bMWjhWQHTNOStxB4MDDruw

40个Linux资源查看命令

https://mp.weixin.qq.com/s/SQMRdaYChXo4bxEyTJZ6wA

CPU实用监控工具盘点

https://www.zhihu.com/question/452895041

Linux大神都是怎么记住这么多命令的？

http://github.com/ibraheemdev/modern-unix

这里收集了几个非常好用的工具

# /usr

usr很多人都认为是user缩写，其实不然，这是unix system resource的缩写。

# lsof

lsof(list open files)是一个列出当前系统打开文件的工具。在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。所以如传输控制协议 (TCP) 和用户数据报协议 (UDP) 套接字等，系统在后台都为该应用程序分配了一个文件描述符，无论这个文件的本质如何，该文件描述符为应用程序与基础操作系统之间的交互提供了通用接口。因为应用程序打开文件的描述符列表提供了大量关于这个应用程序本身的信息，因此通过lsof工具能够查看这个列表对系统监测以及排错将是很有帮助的。

`lsof xxx.txt`

查看使用`xxx.txt`的进程。

`lsof | grep TCP`

列出打开的文件。

# 常用命令示例

`find . -name *.txt`

查找文件夹（包括子文件夹）中所有的txt文件。

https://mp.weixin.qq.com/s/y8LeZ0-9D56TWsD-ivPaHQ

Linux文件搜索神器find实战详解

---

烧写系统到U盘：

`sudo dd if=./865_R11_RC2.sdcard of=/dev/sdb status=progress`

dd + gzip：

```bash
sudo dd if=/dev/sdb bs=32M status=progress | gzip > ./imx865.iso
gzip -dc ,/imx865.iso | sudo dd of=/dev/sdb status=progress
```

https://blog.csdn.net/weixin_30416497/article/details/97144840

linux下使用dd命令写入镜像文件到u盘

---

`strings /lib/tls/libc.so.6 | grep GLIBC`

strings能输出文件中的可打印字符串（可指定字符串的最小长度），通常用来查看非文本文件（如二进制可执行文件）中的可读内容。上例可查看glibc支持的版本。

`ps -efww`

ps默认只显示当前用户的进程。这里是全显示的示例。

`iconv -f gbk -t utf-8 -c 1.txt -o 2.txt`

将文件从gbk编码转为utf-8编码。

`pgrep -u hchen`

按照条件搜索进程ID。

`pstree`

以树形的方式列出进程。

`echo "2*3.5" | bc`

bash只支持整数四则运算，浮点数和复杂运算就靠`bc`了。

`split -b 50m largefile.tar.gz LF_`

分割大文件。

`cat splog* >newLog.log`

文件合并。

`ldd /usr/bin/java`

查询一个可执行文件所使用的动态链接库。

`sudo adduser tt`

创建新用户、用户的主目录以及密码。

`sudo useradd tt`

只创建新用户。

类似的还有`sudo derlser -r tt`和`sudo userdel tt`。

`chage -d 0 username`

强制密码过期。

`cat /etc/group`

查看用户组

`sudo groupadd docker`

添加用户组

`sudo usermod -aG docker $USER`

添加用户

`rm [0-9]*`

删除`0-9`开头的文件。

linux文件分割用split,合并用cat。最近下了一本采用split分割的书，但是我没有Linux环境，于是在windows的命令行下用`type *.txt >>f:\1.txt`合并了文件。

`find . -type f | xargs -I{} rm -f {}`

删除所有文件，但不删除目录

# 线程安全

按照“线程安全”的安全程度由强到弱来排序，我们可以将各种操作共享的数据分为以下5类：

- 不可变。不可变的对象一定是线程安全的，无论是对象的方法实现还是方法的调用者，都不需要再采取任何的线程安全保障措施。

- 绝对线程安全。一个类要达到“不管运行时环境如何，调用者都不需要任何额外的同步措施”通常需要付出很大的代价。

- 相对线程安全。就是我们通常意义上所讲的一个类是“线程安全”的。它需要保证对这个对象单独的操作是线程安全的，我们在调用的时候不需要做额外的保障措施，但是对于一些特定顺序的连续调用，就可能需要在调用端使用额外的同步手段来保证调用的正确性。

- 线程兼容。对象本身并不是线程安全的，但是可以通过在调用端正确地使用同步手段来保证对象在并发环境下可以安全地使用。

- 线程对立。无论调用端是否采取了同步措施，都无法在多线程环境中并发使用的代码。

# nc

NetCat，在网络工具中有“瑞士军刀”美誉。它短小精悍（1.84版本也不过25k，旧版本或缩减版甚至更小）、功能实用，被设计为一个简单、可靠的网络工具，可通过TCP或UDP协议传输读写数据。同时，它还是一个网络应用Debug分析器，因为它可以根据需要创建各种不同类型的网络连接。

http://blog.csdn.net/wang7dao/article/details/7684998

Linux nc命令详解

# 环境变量

设置环境变量的方法：

1）临时的：使用export命令声明即可，变量在关闭shell时失效。示例如下：

`export PATH=/home/xyz/bin:$PATH;`

2）永久的：需要修改配置文件，变量永久生效。

在/etc/profile文件中添加变量（对所有用户生效）。修改文件后要想马上生效，还要运行`source /etc/profile`，不然只能在下次重进此用户时生效。

在用户目录下的.bash_profile文件中增加变量（对该用户生效）。同样需要source才能马上生效。

重要的环境变量：

PATH：可执行文件路径。

LD_LIBRARY_PATH：动态链接库文件路径。

LD_PRELOAD：优先调用自定义库而非系统库的时候会用到。
