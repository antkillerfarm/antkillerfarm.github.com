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
