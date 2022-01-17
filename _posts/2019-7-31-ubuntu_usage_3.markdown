---
layout: post
title:  Ubuntu使用技巧（三）, diff&patch
category: linux 
---

* toc
{:toc}

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

`sudo apt install unity-tweak-tool`

2.安装Mac OS X主题

```bash
sudo add-apt-repository ppa:noobslab/themes
sudo apt update
sudo apt install mac-ithemes-v3 mac-icons-v3
```

3.Cairo Dock

做完上面两步之后，基本的Mac OS X风格已经有了，但Mac最经典的Dock启动器还没有。这里介绍一下Cairo Dock。

安装方法：

`sudo apt install cairo-dock`

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

# 电子书

电子书的格式很多，除了TXT/PDF/WORD这样的大路货之外，我接触的比较多的主要是CHM和PDG。前者主要在Windows上用，而后者主要是破解版看论文。当然这都是十几年前的事情了（2005年左右）。

最近，下载小说，发现有EPUB格式比较流行。本来以为是类似PDG这样的专有格式，后来转念一想，这类下载站本来就是盗版生意，哪有版权保护的说法。然后才发现EPUB大约是现在最流行的电子书通用格式了。

其他相对主流的电子书还有：亚马逊Kandle专用的AZW3和MOBI等。

电子书阅读制作首推Calibre：

`sudo apt install calibre`

Calibre支持的格式：

Input Formats: AZW, AZW3, AZW4, CBZ, CBR, CB7, CBC, CHM, DJVU, DOCX, EPUB, FB2, FBZ, HTML, HTMLZ, LIT, LRF, MOBI, ODT, PDF, PRC, PDB, PML, RB, RTF, SNB, TCR, TXT, TXTZ

Output Formats: AZW3, EPUB, DOCX, FB2, HTMLZ, OEB, LIT, LRF, MOBI, PDB, PMLZ, RB, PDF, RTF, SNB, TCR, TXT, TXTZ, ZIP

参考：

https://zhuanlan.zhihu.com/p/53298578

7个最佳Linux电子书阅读器

# tftp

tftp可提供不复杂、开销不大的文件传输服务。可以理解为是简化版的FTP。

Ubuntu下面关于TFTP的程序，有三套：

1.tftp和tftpd

2.atftp和atftpd

3.tftp-hpa和tftpd-hpa

目前以tftp-hpa和tftpd-hpa最为流行。

安装命令：

`sudo apt install tftp-hpa tftpd-hpa`

# Ubuntu 20.04使用手记

Ubuntu 20.04是2020.4.24发布的。我第一时间上手体验了一番。

UI方面最大的特点是：菜单栏变成了菜单按钮。这种风格最早来自Chrome的设计，后来部分系统应用也采用了该风格，这次算是收尾阶段了吧。

内核：5.4

LibreOffice：6.4

---

这里必须吐槽一下近期这几个版本的安装过程。不知道从18.04的哪一个版本开始，离线安装OS这样的正常需求，就成了一件不可能的事情。无论你选择什么选项，都要从网上下载一堆文件（170M+）才能安装成功。

众所周知，ubuntu官方的网速，在国内一直不快，即便是安装镜像已经换用`cn.archive.ubuntu.com`，也同样快不了多少。速度飞快的aliyun，不好意思，至少在安装阶段是无法换用的。

碰巧我是尝鲜的，正赶上大家都在尝鲜的时候，那个下载速度实在太感人了。。。囧

但是我也意外发现，3点以后，网速就飞快了（8+M/s）。这点数据也就是1分钟的事情。

虽然有1个月之前安装18.04的经验，然而这次还是遇到了新的麻烦：

离线安装，grub是坏的。好容易在线装，安装成功，但是grub没有Ubuntu的选项。

解决办法：使用boot-repair修理grub。

然而boot-repair既然号称修理，自然是把EFI分区里的`.efi`文件一网打尽，每个文件都是一个启动项。众所周知，一个OS往往不止一个`.efi`，于是那个条目数简直多的没法看。。。

解决办法：修改`/boot/grub/grub.cfg`，去掉多余的条目。

>不要直接修改该文件本身，否则系统一旦更新之后，又要再来一遍。该文件开头的注释中介绍了该文件是如何生成的。

这里主要参考的是以下文章：

https://www.cnblogs.com/schips/p/10141278.html

使用boot-repair对Windows+Ubuntu双系统引导修复

# Ubuntu字体相关

最近gitk中文显示不正常，明明系统的字体是很多的，但可以设置的却甚少。后来发现这里能够设置的并非系统字体，而只是X11字体。

列出字体：

`xlsfonts`

找到系统字体文件夹，生成`fonts.dir`文件：

`sudo mkfontscale -o fonts.dir .`

加载`fonts.dir`文件：

`xset +fp /usr/share/fonts/X11/misc`

文泉驿字体是最知名的中文免费字体：

`sudo apt install ttf-wqy-microhei ttf-wqy-zenhei`

# GnuGo

GnuGo是一个著名的开源围棋软件，但是它只有文字界面。一般使用Quarry作为它的GUI。

`sudo apt install quarry`

# 手机上网

最近公司网络有问题，只好使用手机连接互联网，也就是所谓WLAN热点。

除了Wifi之外，现在的手机还有USB网络共享的功能，该功能基于RNDIS（Remote NDIS）技术，实际上就是TCP/IP over USB，就是在USB设备上跑TCP/IP，让USB设备看上去像一块网卡。

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

参考：

https://mp.weixin.qq.com/s/VlQnDAD4dGQpmHNil7uPuQ

如何在Linux上使用xargs命令

https://mp.weixin.qq.com/s/HchAIWJmqPNTqEEwb3TbAg

Linux下xargs命令
