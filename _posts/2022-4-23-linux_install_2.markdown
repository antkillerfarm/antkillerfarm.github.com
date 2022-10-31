---
layout: post
title:  硬盘安装Linux（grub2）
category: linux 
---

* toc
{:toc}

# 硬盘安装Linux（UEFI）

## 内核版本回退（续）

3.默认启动旧内核

参考：

https://blog.csdn.net/qq_34292087/article/details/109166266

ubuntu16.04内核版本降低

4.安装NVIDIA驱动

旧版本的NVIDIA驱动在更新中，出现了问题。因此需要使用DKMS手动安装之。

dkms的驱动放在`/var/lib/dkms`下，按照`module/version`的方式放置，比如`/var/lib/dkms/nvidia/450.102.04`。

安装：

`sudo dkms install -m nvidia/450.102.04`

安装好之后，可以看到`/var/lib/dkms/nvidia/`下多了一个和内核版本同名的文件夹。这说明不同内核版本的驱动，实际上是可以在DKMS的框架下共存的。

DKMS不仅支持不同内核版本的共存，还支持不同驱动版本的共存。但是这个一般来说是起负作用的，所以还要删掉不用的驱动版本：

`sudo dkms remove nvidia/450.102.04`

## RTL8821CE驱动的最终解决

Linux官方虽然没有集成RTL8821CE驱动，但Ubuntu官方已经有了：

`sudo apt install rtl8821ce-dkms`

从内容来看，还是上面那个github的代码库，所以问题仍然没有解决。

运行`dmesg`，发现：

```bash
[    5.497800] rtl8821ce: Unknown symbol cfg80211_disconnected (err -2)
[    5.497828] rtl8821ce: Unknown symbol cfg80211_michael_mic_failure (err -2)
[    5.497844] rtl8821ce: Unknown symbol cfg80211_ibss_joined (err -2)
[    5.498001] rtl8821ce: Unknown symbol cfg80211_scan_done (err -2)
[    5.498024] rtl8821ce: Unknown symbol cfg80211_roamed (err -2)
[    5.498033] rtl8821ce: Unknown symbol cfg80211_put_bss (err -2)
[    5.498057] rtl8821ce: Unknown symbol cfg80211_connect_done (err -2)
[    5.498087] rtl8821ce: Unknown symbol cfg80211_unlink_bss (err -2)
```

可以看出这里缺少80211的驱动，在官方仓库搜索包含`80211`的文件，发现在`linux-modules-extra-5.8.0-36-generic`中，安装以后，重启，问题解决。

# 硬盘安装Linux（grub2）

2022.4

升级Ubuntu 22.04的时候，遇到了新的问题：Ubuntu 22.04只支持UEFI启动，但是有几台PC是legacy BIOS启动的。。。

经过检查BIOS启动设置，发现PC本身实际上是支持UEFI的。这时又引入了一个新的问题——硬盘分区表。

首先使用`gdisk`将MBR分区转换成GPT分区。这个过程基本是安全的，不会损失数据。当然也不能排除意外，所以数据备份也是重要的。

唯一郁闷的是，改为GPT分区之后，之前在MBR上安装的Windows系统无法使用了。。。

```bash
set root=(hd1,msdos3)

loopback loop /ubuntu.iso
linux (loop)/casper/vmlinuz boot=casper iso-scan/filename=/ubuntu.iso
initrd (loop)/casper/initrd
boot
```

UEFI可以有多个boot loader，所以安装时最好把boot loader放到Ubuntu分区上，而不是硬盘引导头上，这样最多Ubuntu安装失败，其他OS都没有问题。

参见：

https://www.cnblogs.com/f-ck-need-u/archive/2017/06/29/7094693.html

grub2详解

https://www.cnblogs.com/gdme1320/p/9166564.html

grub2引导系统iso镜像

# Linux学习心得

## 时间的表示方法

一般遵循ISO 8601标准：

https://www.w3.org/TR/NOTE-datetime

YYYY-MM-DDThh:mm:ss.sTZD (eg 1997-07-16T19:20:30.45+01:00)

其中的TZD表示time zone designator。

## U盘安装Linux

在程序中找到“启动盘创建器”即可。这也是防止变砖的最后一条路，反正BIOS是不可能安装坏的，随便折腾吧。

## wifi配置

Linux下的wifi配置主要使用iw系列命令，包括iw、iwconfig、iwlist、iwpriv。参见：

http://blog.csdn.net/liangyamin/article/details/7209761

Linux下的iwpriv（iwlist、iwconfig）的简单应用

## eBPF

eBPF是一项革命性技术，它能在内核中运行沙箱程序（sandbox programs），而无需修改内核源码或者加载内核模块。

参考：

https://ebpf.io/zh-cn/

一个eBPF的专栏

https://www.ebpf.top/

一个eBPF的专栏

## Linux参考资源

https://www.kernel.org/doc/html/latest/

Linux官方文档

---

W.Richard Stevens著：

APUE: Advanced Programming in the UNIX Environment

UNP: UNIX Network Programming

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

https://mp.weixin.qq.com/s/7YVuouHAq2OfrowhoHVmnQ

Android对so体积优化的探索与实践

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

https://blog.csdn.net/orangeboyye/article/details/125998574

深入理解Linux内存管理

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

https://mp.weixin.qq.com/s/-U7L8aXoaPXSwZshSpjQ2g

进程间通信

https://mp.weixin.qq.com/s/oKtu3AA9D3y--xMDQ8EARw

携程一次Dubbo连接超时问题的排查

https://mp.weixin.qq.com/s/4o_cSzWeJdLJMObJBhaZlw

计算机系统中的内存

https://www.jianshu.com/p/fad3339e3448

浅析Linux中的零拷贝技术

https://mp.weixin.qq.com/s/Q9BOA88Q6OBaDch1AiS9QA

原来8张图，就可以搞懂零拷贝了

https://mp.weixin.qq.com/s/6R8UcLLjm9gdWud-eNHztw

中断及其初始化

https://mp.weixin.qq.com/s/qwouMWc4CFtqG_jra4xbIg

IDT及中断处理的实现

https://mp.weixin.qq.com/s/pRsXWAv7wgYcN_jlzcA2YA

内存都没了，还能运行程序？

https://mp.weixin.qq.com/s/snQ3T86usv4rXz0MMQvFfQ

如何回答性能优化的问题，才能打动阿里面试官？

https://www.cnblogs.com/zhouyu629/p/3734494.html

一次心惊肉跳的服务器误删文件的恢复过程

https://mp.weixin.qq.com/s/IcEP-JGQbWA7s7yPdIC9vA

80%时间屏蔽了中断，实时性还有救么？

https://mp.weixin.qq.com/s/iKfWSfzauzNjcAvXPNhq0Q

这些算法都不会还学什么操作系统

https://mp.weixin.qq.com/s/gj6Zw8SvOdSZqRx8KP9wWw

20张图揭开内存管理的迷雾，瞬间豁然开朗

https://mp.weixin.qq.com/s/IQYUNzVgSOFUHB9c1SM0Bw

10张图22段代码，万字长文带你搞懂虚拟内存模型和malloc内部原理

https://zhuanlan.zhihu.com/p/370092684

虚拟内存精粹

http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html

Systemd入门教程：命令篇

https://mp.weixin.qq.com/s/xOqXM5kFi0CzilDg0EXFKg

Linux内核源码规范解析

https://mp.weixin.qq.com/s/QB-IHiCIWEu3bALm2Dp46Q

操作系统课程知识结构

https://www.zhihu.com/answer/460715569

生产力应用大汇总

https://mp.weixin.qq.com/s/QsgoONKwI7ds8Hnx2Wer6A

Linux从程序到进程

https://mp.weixin.qq.com/s/v9XlJjIQkuVpSudhQIS70A

神秘！申请内存时底层发生了什么？

https://mp.weixin.qq.com/s/V-XT6QuDG522P0bP2e3ULg

咋办，死锁了

https://mp.weixin.qq.com/s/RHAoM8zhFvQl9R8V0ePxNQ

看腾讯这道多线程面试题

https://mp.weixin.qq.com/s/QshDG-nbmBcF1OBZbBFwjg

操作系统与存储：解析Linux内核全新异步IO引擎io_uring设计与实现

https://mp.weixin.qq.com/s/qWXcL90ZAkc7rrhsbuB_Bw

只有170字节，最小的64位Hello World程序这样写成

https://mp.weixin.qq.com/s/5iyWeSeDzuA2cY7YBMhk7w

MMU那些事儿

https://mp.weixin.qq.com/s/0OeeYUgBBVVMtxscvzgJHw

i++是线程安全的吗？

https://mp.weixin.qq.com/s/U0qr1oZYXBBmZnC5vsKYLQ

浅谈linux IO

https://mp.weixin.qq.com/s/3kgwoyYI90XHm1QPqFJAiQ

内存分页不就够了？为什么还要分段？

https://mp.weixin.qq.com/s/VSbzTh3xEbVdB4IgGJzQ3A

25张图，一万字，拆解Linux网络包发送过程

https://mp.weixin.qq.com/s/2dbr4-dxRCJ_SLCQnrt8ag

Linux内核调度器源码分析

https://yanqiyu.info/2021/06/21/huawei-v-qwr/

某不知名网友怒斥华为，究竟发生了什么

https://mp.weixin.qq.com/s/-8L5MFZrgmyatGgYaR1AEA

波兰极客用一张软盘运行Linux系统，用的还是最新内核！

https://mp.weixin.qq.com/s/-hfI4GLkChRJQDqcLcvbGg

嵌入式C编程实现上下文的快速切换（cpost）

https://zhuanlan.zhihu.com/p/400200921

x86 Linux下实现10us误差的高精度延时

https://www.zhihu.com/question/496656138

为什么Windows文件设计成占用无法删除？

https://mp.weixin.qq.com/s/h4LwSRAsDgRqOq3mLt_SCw

浅谈mmap
