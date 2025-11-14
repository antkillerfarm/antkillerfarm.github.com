---
layout: post
title:  硬盘安装Linux（grub2）, Linux学习心得（五）
category: linux 
---

* toc
{:toc}

# 硬盘安装Linux（UEFI）（续）

## RTL8821CE

我的PC使用的无线网卡是RTL8821CE，但是Ubuntu官方的镜像中，并没有集成该网卡的驱动。

查看网卡的命令：

`lspci`

第三方驱动的代码：

https://github.com/tomaspinho/rtl8821ce

安装驱动之前，需要进UEFI，关闭Secure Boot选项。这个选项会拒绝未验证的系统或驱动。Ubuntu官方的镜像经过了MS的认证，可以正常安装。但是UbuntuKylin不行，第三方驱动显然也不行。

## 内核版本回退

最近（2021.1）遇到了高版本内核和驱动不匹配的问题，特将历程记录如下：

1.现象：Wifi找不到了。

首先排查设置和Secure Boot，发现没有问题。然后想起来，上次开机更新了系统内核。重启，进入上个版本的内核，一切正常，看来原因的确和内核版本有关。

2.编译网卡驱动。

首先去github更新驱动的代码。这里必须感谢驱动原作者的持续维护，修正了一些在新内核中出现的问题。

必须说这次的更新，依赖做的很差，连linux-headers都没有，造成了有些驱动无法更新。。。

`sudo apt install linux-headers-5.8.0-36-generic`

安装之后，还是网卡不好使。只好暂时工作在旧内核下了。

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

## tldr

tldr是一个采用示例说明的简化版的man。

官网：

http://tldr.sh/

该项目原生支持node.js，但也提供了其他多种语言的支持。

参考：

https://linuxtoy.org/archives/tldr.html

tldr: 简读Manpage

## Systemd

![](/images/img5/Systemd.png)

内核初始化的最后一步就是启动PID为1的init进程。这个进程是系统的第一个进程。它负责产生其他所有的用户进程。

init系统大体上的演进路线为sysvinit -> upstart -> systemd。

2010年，德国程序员Lennart Poettering（同时也是Avahi和PulseAudio的作者）发明了Systemd。

journalctl = dmesg + tail + grep + syslog的systemd版全家桶

http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html

Systemd入门教程：命令篇

http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-part-two.html

Systemd入门教程：实战篇

## Zero Page

https://www.cnblogs.com/linhaostudy/p/13647189.html

Linux内核虚拟内存管理之匿名映射缺页异常分析

https://zhuanlan.zhihu.com/p/336625055

Zero Fill On Demand

## 参考

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

Ksplice是一个实验性的项目，用于无需重启机器的情况下更新Linux内核。SUSE和Redhat也先后推出了类似的项目。其中，前者为kGraft，后者是kpatch。后来这两者又被Linux内核吸收而成livepatch技术。

https://www.zhihu.com/question/406961061

为什么c语言不支持热更新？

---

http://toaruos.org/

一个日本人个人制作的OS

---

https://zhuanlan.zhihu.com/p/659446170

当代大学生Ubuntu系统个人电脑使用现状(资源分享帖)

---

生产环境在维护的时候重启之后所有数据几乎全部丢失。后来发现在投入生产环境这半年里，这台服务器一直运行在Ubuntu LiveCD环境下。

https://www.v2ex.com/t/974678

亲手造成的运维事故：在Live CD环境下部署并运行了8个月

---

https://www.linuxfromscratch.org/

教你一步一步的构建你自己的Linux发行版

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
