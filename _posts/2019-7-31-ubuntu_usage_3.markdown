---
layout: post
title:  Ubuntu使用技巧（三）, 硬盘安装Linux（UEFI）, diff&patch, awk&sed&grep, Mac OS X
category: linux 
---

# Ubuntu 18.04使用手记

又是两年过去了，这次是Ubuntu 18.04（2018.4.26发布）。

这次的变化还是有点大，Ubuntu舍弃了自己开发的Unity，转回Gnome，连带着好多软件的界面都出现了一定的调整。这个适应过程，要长于之前的几次升级。

>前些年由于Unity界面乏善可陈，Ubuntu的版本升级被吐槽为换壁纸。这次算是换主题吧。

由于这个改变是2017.4做出的，有了1年的过渡期，因此拿到手的Ubuntu 18.04的成品度还是蛮高的。

输入法比原来好，但有些软件存在兼容问题。

内核：4.15

LibreOffice：6.0

Emacs：25.2

# VNC

## vino & remmina

ubuntu不同于一般的发行版，它对桌面做了很大的改动，因此通常的VNC手段对其并不好使。

但其实它已经自带了相关的应用：

- 服务端：vino

设置->共享->屏幕共享，设置密码并打开。

`ss -lnt`查看5900端口是否开启。

设置防火墙规则：

`sudo ufw allow from any to any port 5900 proto tcp`

- 客户端：remmina

该方法可将物理桌面共享给VNC，但是无法创建新的桌面。

参考：

https://linuxconfig.org/ubuntu-remote-desktop-18-04-bionic-beaver-linux

Ubuntu Remote Desktop - 18.04 Bionic Beaver Linux

## xfce4

如果非要使用传统的vncserver的话，只能选择其他桌面，例如xfce4。

`sudo apt install xfce4 xfce4-goodies vnc4server`

修改`~/.vnc/xstartup`：

```bash
#!/bin/sh
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
startxfce4 &
```

启动服务：


```bash
vnc4server -kill :2
vnc4server -geometry 1920x1080 :2
```

参考：

https://www.jianshu.com/p/f58fe5cdeb5f

Ubuntu 18.04搭建VNC服务器

https://linuxconfig.org/ubuntu-remote-desktop-18-04-bionic-beaver-linux

VNC server on Ubuntu 18.04 Bionic Beaver Linux

# 桌面主题

用腻了系统自带的桌面主题之后，我打算换个新鲜一些的桌面主题，比如Mac OS X风格的。

1.安装主题修改工具

`sudo apt-get install unity-tweak-tool`

2.安装Mac OS X主题

```bash
sudo add-apt-repository ppa:noobslab/themes
sudo apt-get update
sudo apt-get install mac-ithemes-v3 mac-icons-v3
```

3.Cairo Dock

做完上面两步之后，基本的Mac OS X风格已经有了，但Mac最经典的Dock启动器还没有。这里介绍一下Cairo Dock。

安装方法：

`sudo apt-get install cairo-dock`

Cairo Dock不仅具有类似Mac OS X的风格，还有其他的风格可供选择下载。比如我使用的是Chrome风格。

4.其他主题

http://www.ubuntuthemes.org/

这个网站收集了很多桌面主题，但是需要注册，因为有些主题是收费的。

# 发行版乱战

Linux以发行版众多闻名于世。最近发现了以下网站，或可对各个发行版进行一个简单的比较。

http://distrowatch.com/

下面对几个主要的参数，进行一下点评：

## Office

主要是3个流派：

1.StarOffice->OpenOffice.org->LibreOffice。最初由Sun主导，后来改为Google主导。

2.KOffice->Calligra Office。KDE项目的成果。

3.GOffice。Gnome项目的成果，和前两个相比，GOffice的组件比较独立，没有什么协同能力。

# 便签软件

主要有两类便签软件：

1.支持超链接的便签。典型的有Gnote和Tomboy，这两个软件都有内容检索的功能。

2.桌面随意贴。典型的有Indicator Stickynotes和Knotes。后者有内容检索的功能，而前者没有。

# tftp

tftp可提供不复杂、开销不大的文件传输服务。可以理解为是简化版的FTP。

Ubuntu下面关于TFTP的程序，有三套：

1.tftp和tftpd

2.atftp和atftpd

3.tftp-hpa和tftpd-hpa

目前以tftp-hpa和tftpd-hpa最为流行。

安装命令：

`sudo apt-get install tftp-hpa tftpd-hpa`

# ASCII表情

╮(╯_╰)╭

(^ω^)

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

参考：

https://zhuanlan.zhihu.com/p/81960137

UEFI引导与传统BIOS引导在原理上有什么区别？芯片公司在其中扮演什么角色？

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

而且我们还可以看到，系统的第一个分区，并不是Windows分区，而是EFI分区。这也是UEFI启动的特殊之处。这个分区对于一般应用是不可见的，也就没有了文件或分区被误删的问题。安装新OS的风险也大大减少了。

- 安装必须要联网，否则会失败。（搞不懂这个镜像有何意义。。。囧）

- 安装失败后重启，可能会出现找不到`/EFI/BOOT/mmx64.efi`的提示。

这时，需要进入UEFI设置界面。我的PC的进入方法是：按住F2，并重启。

选择windows启动优先，保存设置并重启。

不得不说，UEFI的界面比BIOS还是好看多了。由于UEFI的优先级比OS高，即使引导记录被破坏（例如系统安装失败），也照样能进UEFI，再也不用和grub死磕了，后者的门槛还是太高了。

进入windows之后，从`/EFI/BOOT/`下，随便找个efi文件，将其改名为`/EFI/BOOT/mmx64.efi`。

重启，进UEFI，设置Ubuntu启动优先，然后就可以再次安装了。

- Ubuntu分区有个小技巧，数据分区的挂载点最好不要设为默认的`/home`。

因为，这个路径下的很多隐藏文件是和系统相关的。如果今后要升级，比如Ubuntu 18.04升为Ubuntu 20.04，这些文件在Ubuntu 20.04下常用兼容问题，还不如完全重装系统。

挂载到其他地方就可以避免这个问题，比如挂载到`/home/data`。

## Ubuntu Mirror

Ubuntu官网很慢，可以选择国内的Mirror替换之：

更改 /etc/apt/sources.list 文件中 Ubuntu 默认的源地址`http://archive.ubuntu.com/ `为`http://mirrors.aliyun.com/ubuntu/`即可。 

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

# diff&patch

diff/patch这对工具在数学上来说，diff是对2个集合求差，patch是求和。

```bash
diff -uNr A B > C #生成A和B的diff文件C,-uNr为最常用的选项
patch A C #给A打上diff文件得到B
patch -R B C #B还原为A
```

## 给目录应用patch。

`patch -p1 <1.patch`

这种情况适合1.patch中包含对多个文件的修改时。

## 批量应用patch

有的时候，patch不是一个patch文件，而是一个目录中的若干个patch文件。这时可用如下办法：

`find . -name "*.patch">1.txt`

`sort 1.txt | xargs cat >2.patch`

`patch -p1 <2.patch`

# awk&sed&grep

这三个工具是文本处理中用的比较多的工具，各有各的特色，且都支持正则表达式。

一般来说，行处理优先考虑使用sed和grep，列处理优先考虑使用awk。通常情况下，组合使用多个命令，其命令编写的难度小于只使用一个命令。比如sed和grep也可以进行列处理，但语法难度远超awk，反之亦然。

这里不打算列出各个命令的选项，而仅列出使用它们的一些示例：

这里假设我们有一文件名为ab。

## awk

```bash
awk '{print $1}' ab #显示第一列
```

参考：

https://mp.weixin.qq.com/s/-mWYn195TBSB1lDlncuYeA

常用命令之awk常用实例

## sed

```bash
sed '1d' ab #删除第一行 
sed '$d' ab #删除最后一行
sed '1,2d' ab #删除第一行到第二行
sed '2,$d' ab #删除第二行到最后一行

sed -n '1p' ab    #显示第一行 
sed -n '$p' ab    #显示最后一行
sed -n '1,2p' ab  #显示第一行到第二行
sed -n '2,$p' ab  #显示第二行到最后一行

sed -n '/ruby/p' ab #查询包括关键字ruby所在所有行
sed -n '/\$/p' ab #查询包括关键字$所在所有行，使用反斜线\屏蔽特殊含义

sed -n '/ruby/p' ab | sed 's/ruby/bird/g'    #替换ruby为bird
sed -n '/ruby/p' ab | sed 's/ruby//g'        #删除ruby

sed -e 's/.$//' mydos.txt > myunix.txt #dos->unix
```

## grep

```bash
grep 'ruby' ab #查询包括关键字ruby所在所有行
grep -nri 'ruby' #n 显示行号，r 子目录搜索，i 忽略大小写
```

## 综合

```bash
ip addr show br-lan | grep 'inet ' | awk  '{print $2}' | sed 's/\/.*//g'
```

参考：

https://mp.weixin.qq.com/s/o1vuL3RrWz9tyUPguZeSWA

简单快捷的数据处理，数据科学需要注意的命令行

# Mac OS X

最近对iOS开发产生了兴趣，于是准备在PC上搭建一个iOS的开发环境。

首先，我搜了一下在Linux上搭建相关环境的方法，搜到了一些结果。但历史比较老，基本都是3、4年前的东西，就算搭好，也不见得有什么用。

于是，目标改为在PC上使用Virtual Box搭建Mac OS X虚拟机。目标版本为Mac OS X 10.10。

1.下载镜像文件。

镜像文件主要有dmg和iso两种。前者必须在Mac OS X中才能执行，而后者和其他OS镜像差别不大。

2.boot

原版镜像由于Apple的硬件检测机制，并不能在PC上运行。这时就需要破解，这一步一般是在boot中做的。

可用的boot工具，早期有empireEFI、HackBoot。较新的有chameleon、Niresh。
