---
layout: post
title:  硬盘安装Linux（legacy/UEFI）
category: linux 
---

* toc
{:toc}

# 硬盘安装Linux（legacy）

2013.9

最近重新开始研究Android和Linux，由于已经快2年没有弄过了。因此之前下载的linux发行版也不打算再继续用了，省得到了Android开发的时候还要更新一大堆的包。

看了一下Google官网上对于Android source的编译条件，再结合2年前编译Android 2.2（Froyo）的经验，在虚拟机上跑Linux的方案首先被排除。开玩笑，16G的RAM/SAWP的开销，哪是VM能搞得定的。就连Froyo在虚拟机上都编不过去，何况是4.3（Jelly Bean）了。

2年前编译Froyo，用的是Wubi安装的Ubuntu 10.04。这次看了一下Jelly Bean的编译条件，100GB+的空间。忽然觉得做软件这么多年，还从来没有在PC上认认真真的安装个双系统用用，实在是职业生涯的一个污点。于是在下载了Ubuntu 12.04之后，老老实实的在网上搜索起如何硬盘安装的办法来。

之所以选择硬盘安装，其实实在是觉得刻张盘太麻烦了。如今就连卖本本的厂商都不提供Recovery CD，而改用Recovery分区来恢复系统。想来硬盘安装Ubuntu也早无技术难度可言了。

硬盘安装的步骤如下：

- EasyBCD和ISO

- `C:\NST\menu.lst`：

```bash
title Install Ubuntu
root (hd0,0)
kernel (hd0,0)/vmlinuz boot=casper iso-scan/filename=/ubuntu-11.10-i386.iso ro quiet splash locale=zh_CN.UTF-8
initrd (hd0,0)/initrd.lz
```

>现在的电脑，一般都有一个Recovery分区，用于在系统崩溃时，恢复系统之用。因此，(hd0,0)需要修改为(hd0,1)，甚至是(hd0,2)。

- 把准备好的iso用压缩软件或者虚拟光驱打开，找到casper文件夹，复制initrd.lz和vmlinuz到C盘,然后在把iso也拷贝到C盘。

- 正式安装之前：

`sudo umount -l /isodevice`

- 安装好之后，没有GRUB开机菜单的话：

```bash
sudo -i
mount /dev/sda7 /mnt
grub-install --root-directory=/mnt /dev/sda
```

- 最近硬盘安装Ubuntu之后，出现了无法进入Windows的问题。原因是由于公司的Dell笔记本的Recovery分区是第一个主分区，因此被错误的认为是Windows分区。

这种情况可用boot-repair修复之。

```bash
sudo add-apt-repository ppa:yannubuntu/boot-repair
sudo apt update
sudo apt install -y boot-repair
sudo boot-repair
```

- 最近Win10升级，把我的grub弄乱了，导致开机出现了grub rescue提示符。

用`grub rescue> ls (hd0,msdos1)/`依次尝试每个分区，直到找到linux分区。

```bash
grub rescue> set root=hd0,msdos1
grub rescue> set prefix=(hd0,msdos1)/root/grub
grub rescue> insmod normal
grub rescue> normal
```

这样就能进入linux系统了。进入之后：

```bash
sudo update-grub
sudo grub-install /dev/sda
```

下面针对最近几天的安装实践，做一个总结性的概括：

1）刚开始的时候，由于不熟悉Ubuntu 12.04的安装步骤，选择了错误的选项，发现之后中断系统安装。重启，结果不但Ubuntu没有安装好，就连Windows也进不去。这时由于之前没有刻录Live CD的原因，只好使用Recovery分区恢复Windows系统。

2）我的本本是ASUS的，在开机时候按住F9，即可启动一键恢复功能。由于这个功能也是第一次使用，因此当选择将Windows恢复到第一个分区，并在恢复过程中出错的时候，我选择了重启。然后选择将Windows恢复到一个硬盘。。。结果除了Recovery分区之外的所有分区都被删除了。保存在电脑中的数据也全部丢失。看来装系统始终是件危险的工作。安装之前一定要将重要数据备份到移动硬盘上。

3）ASUS的一键恢复功能，会重启机器若干次，且每次的画面高度雷同，因此很让人觉得是不是机器陷入了有问题的死循环之中。在反复折腾几次之后，我也泄了气，任由它先无限重启下去。后来大概是重启到第4次之后，我发现桌面的分辨率有改变，然后才意识到这些重启估计是正常现象，并不是死循环。

4）Ubuntu的虚拟内存默认使用的是交换分区的方式，而不是Windows的交换文件。因此要单独划一个分区用于虚拟内存的数据交换。

5）Win7的压缩卷功能对于系统分区不好使，由于有不可移动的数据存在，系统盘最小也要460G（硬盘总共1T），这实在是太大了。可以使用Acronis Disk Director Suite调整系统分区的大小。但是需要注意的是，Windows下的分区工具对分区数量的调整，会影响到GRUB的执行，需要通过grub shell的命令切换到Linux下，然后执行grub update，方可恢复正常。而Linux下的分区工具（如GParted）就没有这个问题。

6）GParted在调整分区大小，主要是分区首地址右移方面，执行效率非常慢。Acronis Disk Director Suite没试过，没准会好些。

7）分区的话题说完了，继续谈谈对Linux启动过程的心得。会提到一些名词，但是不会展开来说。首先是BIOS和UEFI。它们决定从哪个存储介质启动。

8）MBR的结构。MBR决定了一个介质最多只有3个主分区和1个扩展分区。一个扩展分区可包含若干个逻辑分区。

9）GRUB和LILO。

10）initrd和vmlinuz。initrd包括image-initrd和cpio-initrd。zImage和bzImage的区别和作用。

---

既然有安装，当然也有卸载教程了。

参见：

https://blog.csdn.net/aisikaov5/article/details/49847005

win10和Linux双系统怎么在win10下用EasyBcd卸载Linux系统

注意：正如安装有legacy和UEFI的区别，卸载也是一样的，需要找对应的工具才行。

# 硬盘安装Linux（UEFI）

2020.3

最近换了一台PC，由于它是UEFI启动的，因此之前的那篇《硬盘安装Linux》宣告作废。

## UEFI

Unified Extensible Firmware Interface是为了替代传统BIOS而诞生。

它的历史已有20年左右，甚至我那台被淘汰的PC，其实也是支持UEFI的。但它之前一直用的不太广，直到Win8的时代。

尽管Win8+仍然可以在传统的BIOS上运行，但MS决定预装Win8+的PC必须是UEFI启动的。因此，近几年的PC不用问，肯定是UEFI启动的。

UEFI官网：

https://uefi.org/

UEFI要求硬盘分区必须是GPT方式的，因此也被称作UEFI+GPT，与之相对的传统方案叫做legacy+MBR。

UEFI的竞争对手主要有：MinPlatform、CoreBoot和LinuxBoot。

参考：

https://zhuanlan.zhihu.com/p/81960137

UEFI引导与传统BIOS引导在原理上有什么区别？芯片公司在其中扮演什么角色？

https://zhuanlan.zhihu.com/p/354914114

下一代BIOS标准探讨引子：之各种Bootloader大比拼

https://zhuanlan.zhihu.com/p/83039006

X86生态圈为什么在物联网玩不转？什么是Intel FSP？它能解决什么问题？

## 安装步骤

主要参考以下文章：

https://www.cnblogs.com/iamnewsea/p/7701464.html

用EasyUEFI在Win8/10中硬盘安装Ubuntu

要点摘录及补充如下：

- 镜像所在分区的格式必须是FAT32，镜像解压到该分区即可。如果是SSD+硬盘的话，则该分区必须在SSD上，因为系统只能有一个引导硬盘。

- EasyBCD不可以用了，这和专不专业版，系统是不是Win10没有关系，根本原因是EasyBCD只是一个MBR工具而已。

- EasyUEFI个人版现在无法添加新的启动项目了，必须用破解版。

- 有的PC，启动文件必须选择`/EFI/BOOT/BOOTx64.EFI`，其实随便选哪个都一样。

- 进入Ubuntu安装界面之后，还是一样要umount镜像文件。

`sudo umount -l /dev/sda5`

可以用`sudo fdisk -l`查看分区名称，例如SSD分区一般叫做`/dev/nvme0n1p4`。

或者

`sudo umount -l /cdrom`

而且我们还可以看到，系统的第一个分区，并不是Windows分区，而是EFI分区。这也是UEFI启动的特殊之处。这个分区对于一般应用是不可见的，也就没有了文件或分区被误删的问题。安装新OS的风险也大大减少了。

- 安装必须要联网，否则会失败。（搞不懂这个镜像有何意义。。。囧）

- 安装失败后重启，可能会出现找不到`/EFI/BOOT/mmx64.efi`的提示。

这时，需要进入UEFI设置界面。我的PC的进入方法是：按住F2，并重启。

选择windows启动优先，保存设置并重启。

不得不说，UEFI的界面比BIOS还是好看多了。由于UEFI的优先级比OS高，即使引导记录被破坏（例如系统安装失败），也照样能进UEFI，再也不用和grub死磕了，后者的门槛还是太高了。

进入windows之后，从`/EFI/BOOT/`下，随便找个efi文件，将其改名为`/EFI/BOOT/mmx64.efi`。

重启，进UEFI，设置Ubuntu启动优先，然后就可以再次安装了。

- Ubuntu分区有个小技巧，数据分区的挂载点最好不要设为默认的`/home`。

因为，这个路径下的很多隐藏文件是和系统相关的。如果今后要升级，比如Ubuntu 18.04升为Ubuntu 20.04，这些文件在Ubuntu 20.04下常有兼容问题，还不如完全重装系统。

挂载到其他地方就可以避免这个问题，比如挂载到`/home/data`。

除了硬盘安装之外，现在的PC还可以网络安装：

https://mp.weixin.qq.com/s/6vFXphhkPpyYEDSgibl_vA

从无盘启动看Linux启动原理

## Ubuntu Mirror

Ubuntu官网很慢，可以选择国内的Mirror替换之：

更改/etc/apt/sources.list文件中Ubuntu默认的源地址`http://archive.ubuntu.com/`为`http://mirrors.aliyun.com/ubuntu/`即可。

其他mirror还有：

http://mirrors.163.com/ubuntu/

https://mirrors.ustc.edu.cn/ubuntu/

https://mirrors.tuna.tsinghua.edu.cn/ubuntu/

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
