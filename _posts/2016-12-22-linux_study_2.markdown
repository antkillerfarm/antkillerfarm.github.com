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

参考：

https://mp.weixin.qq.com/s/8892B95aQZHw4fZhi8iO5A

Linux中load average意义

https://mp.weixin.qq.com/s/FV13ma9LxI1gsgi9ovU4Mw

30个实例详解TOP命令

## free

free命令的内容比较概括，主要包含系统内存的整体使用情况，不深入到进程一级。

## glances

glances算是top的威力加强版了。

`sudo apt install glances`

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

# 性能分析

linux常用的调试工具：

vmstat、iostat、mpstat、netstat、sar、top：查看系统、程序信息等。

gprof、perf、perf top：定位到具体函数、调用等。

strace、ltrace：系统调用、函数调用、库函数调用等。

pstack、ptree、pmap：堆栈信息。

dmesg：系统log信息。

mallinfo：获得内存分配信息。

## 黑盒测试

Linux上主要有perf、gprof和valgrind三个性能分析工具。

参考：

https://cloud.tencent.com/developer/article/1063652

Linux性能分析工具与图形化方法

https://mp.weixin.qq.com/s/HvADkICPYflS2VTuSB16rg

Linux入门必看：如何60秒内分析Linux性能

https://zhuanlan.zhihu.com/p/111556601

valgrind排查内存泄露

## 白盒测试

上面提到的主要是黑盒测试工具。

白盒测试工具主要是Linux Test Project和Gcov。

官网：

http://linux-test-project.github.io/

代码：

https://github.com/linux-test-project/ltp

参考：

https://mp.weixin.qq.com/s/Od17bQhpd3FYE2GQd-kvaQ

iOS代码染色原理及技术实践（gcov，gcno，gcda）

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

https://www.cnblogs.com/zqb-all/p/10666474.html

Linux实用命令之xdg-open

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

# 时间的表示方法

一般遵循ISO 8601标准：

https://www.w3.org/TR/NOTE-datetime

YYYY-MM-DDThh:mm:ss.sTZD (eg 1997-07-16T19:20:30.45+01:00)

其中的TZD表示time zone designator。

# wifi配置

Linux下的wifi配置主要使用iw系列命令，包括iw、iwconfig、iwlist、iwpriv。参见：

http://blog.csdn.net/liangyamin/article/details/7209761

Linux下的iwpriv（iwlist、iwconfig）的简单应用

# Inotify

一种高效、实时的Linux文件系统事件监控框架。参考文档：

http://www.infoq.com/cn/articles/inotify-linux-file-system-event-monitoring

Inotify: 高效、实时的Linux文件系统事件监控框架

# /usr

usr很多人都认为是user缩写，其实不然，这是unix system resource的缩写。

# lsof

lsof(list open files)是一个列出当前系统打开文件的工具。在linux环境下，任何事物都以文件的形式存在，通过文件不仅仅可以访问常规数据，还可以访问网络连接和硬件。所以如传输控制协议 (TCP) 和用户数据报协议 (UDP) 套接字等，系统在后台都为该应用程序分配了一个文件描述符，无论这个文件的本质如何，该文件描述符为应用程序与基础操作系统之间的交互提供了通用接口。因为应用程序打开文件的描述符列表提供了大量关于这个应用程序本身的信息，因此通过lsof工具能够查看这个列表对系统监测以及排错将是很有帮助的。

`lsof xxx.txt`

查看使用`xxx.txt`的进程。

`lsof | grep TCP`

列出打开的文件。

# nc

NetCat，在网络工具中有“瑞士军刀”美誉。它短小精悍（1.84版本也不过25k，旧版本或缩减版甚至更小）、功能实用，被设计为一个简单、可靠的网络工具，可通过TCP或UDP协议传输读写数据。同时，它还是一个网络应用Debug分析器，因为它可以根据需要创建各种不同类型的网络连接。

http://blog.csdn.net/wang7dao/article/details/7684998

Linux nc命令详解

# 常用命令示例

`find . -name *.txt`

查找文件夹（包括子文件夹）中所有的txt文件。

https://mp.weixin.qq.com/s/y8LeZ0-9D56TWsD-ivPaHQ

Linux文件搜索神器find实战详解

---

`sudo dd if=./865_R11_RC2.sdcard of=/dev/sdb status=progress`

烧写系统到U盘。

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

`scp -rC ubuser@192.168.32.129:/home/work/xxx.txt .`

`rsync -zvaP ubuser@192.168.32.129::/home/work/xxx.txt .`

这两个命令都可以传输远程机器上的文件，后者会忽略已经有的文件。后者还支持两种协议：SSH和rsync，上例展示的是rsync协议的示例，如果使用SSH的话，把上例中的`::`换成`:`即可。

`cat /etc/group`

查看用户组

`sudo groupadd docker`

添加用户组

`sudo usermod -aG docker $USER`

添加用户

`rm [0-9]*`

删除`0-9`开头的文件。

linux文件分割用split,合并用cat。最近下了一本采用split分割的书，但是我没有Linux环境，于是在windows的命令行下用`type *.txt >>f:\1.txt`合并了文件。

# DosBox

DosBox是Linux平台玩DOS老游戏的法宝。

安装：

`sudo apt install dosbox`

启动DosBox之后，需要使用如下命令加载本地文件夹：

`mount c ~/dosprom`
