---
layout: post
title:  硬盘安装Linux（grub2）
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

# Linux学习心得+

https://mp.weixin.qq.com/s/ESLO1RH6Q8udwI13Z2Pz_w

详解linux io flush

https://mp.weixin.qq.com/s/LLlzPB2emr9Hqr7gql0B4Q

为什么Linux需要Swapping

https://mp.weixin.qq.com/s/fzLcAkYwKhj-9hgoVkTzaw

CPU飙高，系统性能问题如何排查？

https://www.zhihu.com/question/456916638

printf是怎么输出到控制台的呢？（app->libc->syscall->console）

https://mp.weixin.qq.com/s/gj6Zw8SvOdSZqRx8KP9wWw

20张图揭开内存管理的迷雾，瞬间豁然开朗

https://mp.weixin.qq.com/s/IQYUNzVgSOFUHB9c1SM0Bw

10张图22段代码，万字长文带你搞懂虚拟内存模型和malloc内部原理

https://zhuanlan.zhihu.com/p/370092684

虚拟内存精粹

https://mp.weixin.qq.com/s/xOqXM5kFi0CzilDg0EXFKg

Linux内核源码规范解析

https://mp.weixin.qq.com/s/QB-IHiCIWEu3bALm2Dp46Q

操作系统课程知识结构

https://www.zhihu.com/answer/460715569

生产力应用大汇总

https://mp.weixin.qq.com/s/QsgoONKwI7ds8Hnx2Wer6A

Linux从程序到进程

https://zhuanlan.zhihu.com/p/2018404395100976478

Linux中的Kernel Bypass技术介绍
