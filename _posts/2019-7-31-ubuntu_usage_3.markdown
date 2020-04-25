---
layout: post
title:  Ubuntu使用技巧（三）, 硬盘安装Linux（UEFI）
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

## DKMS

我们都知道，如果要使用没有集成到内核之中的Linux驱动程序需要手动编译。而Linux模块和内核是有依赖关系的，如果遇到因为发行版更新造成的内核版本的变动，之前编译的模块是无法继续使用的，我们只能手动再编译一遍。

DKMS（Dynamic Kernel Module Support）可以帮我们维护内核外的这些驱动程序，在内核版本变动之后，可以自动重新生成新的模块。

参考：

https://blog.csdn.net/fouweng/article/details/53435602

DKMS简介

# Ubuntu 20.04使用手记

Ubuntu 20.04是2020.4.24发布的。我第一时间上手体验了一番。

UI方面最大的特点是：菜单栏变成了菜单按钮。这种风格最早来自Chrome的设计，后来部分系统应用也采用了该风格，这次算是收尾阶段了吧。

内核：5.4

LibreOffice：6.4

----

这里必须吐槽一下近期这几个版本的安装过程。不知道从18.04的哪一个版本开始，离线安装OS这样的正常需求，就成了一件不可能的事情。无论你选择什么选项，都要从网上下载一堆文件（170M+）才能安装成功。

众所周知，ubuntu官方的网速，在国内一直不快，即便是安装镜像已经换用`cn.archive.ubuntu.com`，也同样快不了多少。速度飞快的aliyun，不好意思，至少在安装阶段是无法换用的。

碰巧我是尝鲜的，正赶上大家都在尝鲜的时候，那个下载速度实在太感人了。。。囧

但是我也意外发现，3点以后，网速就飞快了（8+M/s）。这点数据也就1分钟的事情。

虽然有1个月之前安装18.04的经验，然而这次还是遇到了新的麻烦：

离线安装，grub是坏的。好容易在线装，安装成功，但是grub没有Ubuntu的选项。

解决办法：使用boot-repair修理grub。

然而boot-repair既然号称修理，自然是把EFI分区里的`.efi`文件一网打尽，每个文件都是一个启动项。众所周知，一个OS往往不止一个`.efi`，于是那个条目数简直多的没法看。。。

解决办法：修改`/boot/grub/grub.cfg`，去掉多余的条目。

这里主要参考的是以下文章：

https://www.cnblogs.com/schips/p/10141278.html

使用boot-repair对Windows+Ubuntu双系统引导修复

# betty

betty是Jeff Pickhardt开发的人工智能助手，可以将英文转换成Linux命令。

安装方法如下：

```bash
sudo apt-get install git curl ruby
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
