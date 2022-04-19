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

# Linux知识图谱

![](/images/article/linux_perf_tools_full.png)

![](/images/img3/linux_kernel_map.png)

原图地址：

http://www.brendangregg.com/linuxperf.html

![](/images/img4/Linux-storage-stack-diagram_v4.10.png)

参考：

https://mp.weixin.qq.com/s/-iCuxQjSghtDGaPMqaDSgQ

Linux思维导图整理

https://mp.weixin.qq.com/s/sLyD6z2xBXRxfBZnImTgtQ

40+张最全Linux/C/C++思维导图，收藏！

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

官网：

https://www.dpdk.org/

参考：

https://zhuanlan.zhihu.com/p/363622877

DPDK解析

https://zhuanlan.zhihu.com/p/347693559

DPDK的基本原理、学习路线总结

# 文件系统

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

## debuger

Ext系列可以使用`debugfs`命令。

XFS可以使用`xfs_db`命令。

## Inotify

一种高效、实时的Linux文件系统事件监控框架。参考文档：

http://www.infoq.com/cn/articles/inotify-linux-file-system-event-monitoring

Inotify: 高效、实时的Linux文件系统事件监控框架

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

# MOTD

motd：message of the day。即系统登陆时，通过终端展示给登陆用户的消息。

静态MOTD: `/etc/motd`

动态MOTD: `/etc/update-motd.d/`

https://www.cnblogs.com/gageshen/p/11565980.html

Linux中创建自己的MOTD

# Linux参考资源

https://www.kernel.org/doc/html/latest/

Linux官方文档

---

https://mp.weixin.qq.com/s/n6D5_6K9TrnuXg3h6AiFNA

华为“鸿蒙”所涉及的微内核到底是什么？一文带你认识微内核

![](/images/img3/Monolithic_vs_Micro.jpg)

![](/images/img3/UNIX.jpg)

---

https://mp.weixin.qq.com/s/I7C7cXFgxO7RO0Wpjjj3xQ

一篇文章带你“重新认识”线程上下文切换怎么玩儿

https://www.cnblogs.com/liqiuhao/p/9450093.html

关于TOCTTOU攻击的简介

https://mp.weixin.qq.com/s/Y_GYtL9m3zmY-5VZMbCfWg

Linux中用户的简介与管理

https://linux.cn/article-8290-1.html

漫画赏析：Linux内核到底长啥样

https://mp.weixin.qq.com/s/IucIsbJPo4eUUopV8xNN9w

图解Linux程序的链接原理

https://mp.weixin.qq.com/s/zBtdhjAOjwcJbGluccwOlA

我和面试官之间关于操作系统的一场对弈！写了很久，希望对你有帮助！

https://mp.weixin.qq.com/s/3Pp7wkDO6Rnxb5aZP0sacw

一文了解操作系统I/O

https://mp.weixin.qq.com/s/xM8uvYbX6VY8MVZrYkvCUg

链接选项rpath的原理和应用

https://www.cnblogs.com/Malphite/p/10405465.html

Makefile中的-rpath/-rpath-link

https://juejin.im/post/5e8844996fb9a03c6675b9d6

我们按下电脑开机键的背后发生了什么？

https://mp.weixin.qq.com/s/DCqkgksOHa81EI-I0oaZvg

Linux下9种优秀的代码比对工具推荐

https://zhuanlan.zhihu.com/p/162366167

Linux下C++so热更新

https://mp.weixin.qq.com/s/rH7WqriomFTA55ecacV8Gw

键盘敲入A字母时，操作系统期间发生了什么...

https://mp.weixin.qq.com/s/vDlWCVK8knxPf5HoqmtZyQ

从创建进程到进入main函数，发生了什么？

https://mp.weixin.qq.com/s/4ZdnacKuqkpWTso6P1Rmjg

如何调试多线程程序

https://mp.weixin.qq.com/s/8DNyicMcycUL3RRAiKAz8g

Linux进程必知必会

https://mp.weixin.qq.com/s/EkScI-WCdjLz1g2ec6nkhQ

理解格式化原理

https://mp.weixin.qq.com/s/Sqpp82FhZEC8HkeVHzk9QA

5万字、97张图总结操作系统核心知识点

https://mp.weixin.qq.com/s/SYlaIkuXBqFrbZ-gDMYqtA

高并发高性能服务器是如何实现的

https://mp.weixin.qq.com/s/73eaj0qvhUFWGbDA4H2MNQ

读取文件时，程序经历了什么？

https://mp.weixin.qq.com/s/Y8YZzkuzVr_ti6skHpd1NA

Linux网络包接收过程的监控与调优

https://mp.weixin.qq.com/s/4tAxQ0auQfv5x7Dh3B-85g

Linux内存管理

https://mp.weixin.qq.com/s/jDPxu6IVo3_VpK5l6_-jdQ

Linux系统内存知识

http://www.ruanyifeng.com/blog/2013/04/processes_and_threads.html

进程与线程的一个简单解释

https://www.cnblogs.com/biyeymyhjob/archive/2012/07/20/2601655.html

Linux写时拷贝技术(copy-on-write)

https://zhuanlan.zhihu.com/p/296207162

Linux I/O原理和Zero-copy技术全面揭秘

https://mp.weixin.qq.com/s/cudK2dhw4jNr7I34luVKVw

终于有人把零拷贝Zero-Copy讲懂了

https://mp.weixin.qq.com/s/1JiXL1f3SSjsBojlJSNOpQ

Linux的启动流程

https://mp.weixin.qq.com/s/ZfprFQjVANuCE2N693gZBQ

用户空间和内核空间

https://mp.weixin.qq.com/s/P14VsWwSh9jiF-jBHSXXOw

申请内存时底层发生了什么？

https://mp.weixin.qq.com/s/OJHQZjxa6u8aA6jZyJnNPg

一文浅析内存管理机制

https://mp.weixin.qq.com/s/lAN0GKjkfkkWCurwRQb6DQ

如何排查句柄泄露问题

https://mp.weixin.qq.com/s/9UmzFxRdE4FFdrqEeBZtOQ

如何实现一个定时器？

https://mp.weixin.qq.com/s/zVi45pZka_kPpKIoNXNVBA

当初我要是这么学习“进程和线程”就好了

https://mp.weixin.qq.com/s/A8TnhOFLQOhEqphE760yvw

15个相见恨晚的Linux神器，你可能一个都没见过

https://mp.weixin.qq.com/s/ejGjsGA1ijPP--j3BLcEFA

Linux并发与同步

https://mp.weixin.qq.com/s/CAPU8bjJWobQs6JHHMasvQ

Linux服务端最大并发数是多少？

http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html

Systemd入门教程：实战篇

https://mp.weixin.qq.com/s/bPqnaMqhi_4p1mwjmvyoIw

多图详解10大高性能开发核心技术
