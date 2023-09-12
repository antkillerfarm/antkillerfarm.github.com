---
layout: post
title:  Linux学习心得（四）
category: linux 
---

* toc
{:toc}

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

# SSH

## keygen

```bash
cd ~/.ssh
ssh-keygen
cat ~/.ssh/id_rsa.pub
```

>`ssh-keygen`命令会生成两个文件id_rsa和id_rsa.pub，前者是私钥，后者是公钥，不要弄错了。

`scp -rC user@192.168.32.129:/home/work/xxx.txt .`

`rsync -zvaP user@192.168.32.129::/home/work/xxx.txt .`

这两个命令都可以传输远程机器上的文件，后者会忽略已经有的文件。后者还支持两种协议：SSH和rsync，上例展示的是rsync协议的示例，如果使用SSH的话，把上例中的`::`换成`:`即可。

---

`ssh -o ProxyCommand="ssh -Y -q -W %h:%p user@10.10.43.99" user@192.168.32.136`

`scp -rC -o ProxyCommand="ssh -Y -q -W %h:%p user@10.10.43.99" user@192.168.32.136:/home/work/1.txt .`

ssh和scp还支持代理模式。上例中，`10.10.43.99`即为跳板机。还可以用类似的方式，建立多重跳板连接。

还可以修改~/.ssh/config，添加：

```bash
Host bridge
    HostName 10.10.43.99
    User user

Host s136
    HostName 192.168.32.136
    User user
    ProxyCommand ssh -Y -q -W %h:%p bridge
```

---

参考：

https://git-scm.com/book/en/v2/Git-on-the-Server-Generating-Your-SSH-Public-Key

Generating Your SSH Public Key

## 免密登录

1. 本机生成ssh公钥；

2. 复制本机公钥到远程服务器`.ssh/authorized_keys`中，authorized_keys文件不存在则创建；

https://blog.csdn.net/qq_26400953/article/details/105145103

一文读懂authorized_keys和known_hosts

## X Server

假设客户端的ip是1.1.1.1，而ssh服务器的ip是2.2.2.2。

Client:

```bash
xhost +2.2.2.2
ssh -X root@2.2.2.2
```

Server:

```bash
export DISPLAY=1.1.1.1:0.0
xclock
```

参考：

https://www.cnblogs.com/-9-8/p/5365105.html

ssh & display

## 参考

https://mp.weixin.qq.com/s/u3VSyEtdcIgp8dCbwCaavA

SSH只能用于远程Linux主机？那说明你见识太小了！

# 协程

stack-twine：在古老的，DOS如日中天的时代，没有啥线程概念，更没有多个栈的能力。这个时代协程，是通过在栈里规划出一片额外的空间来执行协程。保证压栈退栈不破坏这片栈空间就行。多个协程就会规划出多个空间，然后在这些栈空间里跳转来跳转去的执行代码，就像相互缠绕着一样。

stackfull：栈缠绕毕竟太过于原始粗暴，限制很多，而且危险。所以，在有了多线程能力后，分配一个独立的栈来运行协程再好不过了。除了空间需求增大外，相对于stack-twine几乎没有任何害处。

stackcopy：stackfull被滥用后，或者说被用滥后，其额外空间的消耗就不是一个可以忽视的因素的。动辄上万甚至十万个协程，这内存开销是非常非常大的。在一些时间开销不是那么紧要地方，可以通过把当前栈内容拷贝出去的方式来实现协程切换。因为这种场合下，大部分协程使用的栈空间并不大，故可以显著的节省内存开销。但是呢，在有指针的语言里，这种行为很不安全。限制颇多，故没有流行开来。

stackless：这是一个新兴的实现方案。本质上其实就是一个状态机。但通常由语言提供语法，编译器去实现状态机。拥有内存开销小，切换快的几乎所有优点，是一个“现代”语言都想拥有的功能。目前最成熟的实现是C#的await/async。C++/Java/Rust等语言也在谋划加入（或者已经成功加入）。

参考：

https://www.zhihu.com/question/23955356

协程和纤程的区别？

https://mp.weixin.qq.com/s/mqpsOqc58D9StjlRoBIQug

爱奇艺网络协程编写高并发应用实践

https://mp.weixin.qq.com/s/T5s1IdBgc6yvfqNz5GpbWg

一文讲透 “进程、线程、协程”

https://zhuanlan.zhihu.com/p/94018082

从头到尾理解有栈协程实现原理

https://mp.weixin.qq.com/s/YU6snc2hbK2KvmoMI3nlug

Linux中的各种栈：进程栈 线程栈 内核栈 中断栈

https://zhuanlan.zhihu.com/p/330606651

有栈协程与无栈协程

https://www.zhihu.com/answer/2270734690

为什么觉得协程是趋势？

https://www.zhihu.com/question/52193579

腾讯开源的libco号称千万级协程支持，那个共享栈模式原理是什么?

https://zhuanlan.zhihu.com/p/347445164

浅谈有栈协程与无栈协程

# DPDK

传统的数据包捕获瓶颈往往在于Linux Kernel，数据流需要经过Linux Kernel，就会带来Kernel Spcae和User Space数据拷贝的消耗；系统调用的消耗；中断处理的消耗等。

DPDK(Data Plane Development Kit)由Intel开发，针对Linux Kernel传统的数据包捕获模式的问题，进行了一定程度的优化。

内核网络栈的根本瓶颈是内核是通用软件，要考虑复杂多样的使用场景和历史路径，因此不能完全为性能而生，所以实现方式上有很多束缚和历史遗留问题；而且DPDK只专注于高IO场景，可以以性能为最优先去重新实现网络栈。DPDK可以看作是优先性能的专用内核软件。

官网：

https://www.dpdk.org/

参考：

https://zhuanlan.zhihu.com/p/363622877

DPDK解析

https://zhuanlan.zhihu.com/p/347693559

DPDK的基本原理、学习路线总结

https://www.zhihu.com/question/27413080

Intel推出DPDK开发包的意义是什么？

# 文件系统

`mount`命令本质上就是挂载文件系统。比如插上一块移动硬盘，哪怕什么都不做，也会产生诸如`/dev/sda1`这样的设备文件，但仅有设备文件，你是无法访问硬盘里的数据文件的。这些必须在挂载了文件系统之后才能访问。

---

https://mp.weixin.qq.com/s/2QAm6F109LV2koC64xIpqA

简直不要太硬了！一文带你彻底理解文件系统

https://mp.weixin.qq.com/s/0jR4Y3sT8RRW7FYEOmBsXg

一口气搞懂文件系统，就靠这25张图了

https://mp.weixin.qq.com/s/tguC-WOleKRWLfTmGdJlQg

Linux文件系统的实现

https://mp.weixin.qq.com/s/MNzSES3dlMuiwkffLWJGIg

文件系统：隐匿在Linux背后的机制

https://mp.weixin.qq.com/s/wmzwvnOToqCkKJz5yw-USQ

低调的Linux文件系统家族

https://mp.weixin.qq.com/s/9x1JOl4m_mj0WpsVgHu4rg

Linux文件系统与持久性内存介绍

https://zhuanlan.zhihu.com/p/61407714

Linux文件系统的未来btrfs

https://linux.cn/article-7083-1.html

如何选择文件系统：EXT4、Btrfs和XFS

https://zhuanlan.zhihu.com/p/579011810

300行代码带你实现一个Linux文件系统

## Inotify

一种高效、实时的Linux文件系统事件监控框架。参考文档：

http://www.infoq.com/cn/articles/inotify-linux-file-system-event-monitoring

Inotify: 高效、实时的Linux文件系统事件监控框架

## debuger

Ext系列可以使用`debugfs`命令。

XFS可以使用`xfs_db`命令。

# 二进制文件与ASCII、Base64之间的转换

ASCII中共有95个可打印字符。常见的Binary-to-text编码主要有：Base32/Base36/Base64/Base85。其中尤以Base64使用最为广泛

xxd：这个命令可以将二进制文件转换成ASCII码表示文本文件。支持2、8、16等多种进制的ASCII表示形式，还支持输出成C语言格式的数组声明。反过来的转换也同样支持。

uuencode and uudecode：支持二进制文件与Base64之间的转换。

参考：

https://zhuyie.github.io/posts/binary-to-text-encoding/

从Base16到Base85, 谈谈Binary-to-text编码

# start-stop-daemon

该命令用于启动和停止系统守护程序。

# 软件包管理工具

各大linux发行版都有自己的软件包管理工具。例如：

| Debian/Ubuntu | apt |
| Red Hat/Fedora | yum/dnf |
| SUSE/openSUSE | zypper |
| Gentoo | emerge |
| Arch Linux | pacman |

各大软件包管理工具的功能对比，可参见：

https://wiki.archlinux.org/index.php/Pacman/Rosetta

类似的概念也被一些编程语言所使用。例如：

| Ruby | RubyGems(gem) |
| Python | PyPI(pip) |
| Java | Maven(mvn) |
| Perl | PPM |
| Node.js | NPM |
| PHP | pear |

# betty

betty是Jeff Pickhardt开发的人工智能助手，可以将英文转换成Linux命令。

安装方法如下：

```bash
sudo apt install git curl ruby
cd ~
git clone https://github.com/pickhardt/betty
sudo nano ~/.bashrc
```

在.bashrc末尾添加以下内容：

`alias betty="/home/sk/betty/main.rb"`

重启终端即可。

使用方法：

`betty compress test/ test.tar.gz`

# Hubot

Hubot是个和betty类似的开源聊天机器人，可以用来做一些自动化任务，如部署网站，翻译语言等等。

官网：

https://hubot.github.com/

参考：

https://segmentfault.com/a/1190000004855149

Hubot的简单用法

# 格式化硬盘

查看块设备：`lsblk`

分区：`sudo fdisk /dev/sda`

`g create a new empty GPT partition table`

`n add a new partition`

格式化文件系统：`sudo mkfs.ext4 /dev/sda1`

开机自动挂载，修改`/etc/fstab`：

`/dev/sda1 /home/pi/data ext4 defaults 0 1`

参考：

https://blog.csdn.net/weixin_38472804/article/details/109361682

linux系统格式化硬盘

# tldr

tldr是一个采用示例说明的简化版的man。

官网：

http://tldr.sh/

该项目原生支持node.js，但也提供了其他多种语言的支持。

参考：

https://linuxtoy.org/archives/tldr.html

tldr: 简读Manpage
