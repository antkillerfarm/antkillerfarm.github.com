---
layout: post
title:  硬盘安装Linux（grub2）
category: linux 
---

* toc
{:toc}

# 硬盘安装Linux（UEFI）

## 内核版本回退（续）

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

# U盘安装Linux

在程序中找到“启动盘创建器”即可。这也是防止变砖的最后一条路，反正BIOS是不可能安装坏的，随便折腾吧。

# Linux参考资源+

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

https://mp.weixin.qq.com/s/djEPqxZSfMp13Uf_h6TSiA

认真分析mmap：是什么 为什么 怎么用

https://mp.weixin.qq.com/s/FMYimnxcAya6bhvdGD5LUw

惊魂48小时，阿里工程师如何紧急定位线上内存泄露？

https://zhuanlan.zhihu.com/p/424240082

编译一个属于自己的最小Linux系统

https://www.zhihu.com/question/66902460

为什么Linux下要把创建进程分为fork()和exec()(一系列函数)两个函数来处理?

https://zhuanlan.zhihu.com/p/464204319

Linux网络子系统中DMA机制的实现

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

https://mp.weixin.qq.com/s/ESLO1RH6Q8udwI13Z2Pz_w

详解linux io flush

https://mp.weixin.qq.com/s/LLlzPB2emr9Hqr7gql0B4Q

为什么Linux需要Swapping

https://mp.weixin.qq.com/s/fzLcAkYwKhj-9hgoVkTzaw

CPU飙高，系统性能问题如何排查？
