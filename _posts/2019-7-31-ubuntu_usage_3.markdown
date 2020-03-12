---
layout: post
title:  Ubuntu使用技巧（三）, diff&patch, awk&sed&grep, Mac OS X, WebKit, 辛普森悖论
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

# WebKit

WebKit的代码可以从它的官网www.webkit.org下获得。

在以下网页可以获得webkit向各种GUI移植的相关信息。

http://trac.webkit.org/wiki

由于获得的代码比较新，所以在linux平台下常有一些组件由于过于古老而导致编译失败。所以需要使用yum或者apt-get之类的工具从网上更新相关的组件。这里不推荐使用RHEL或者CentOS之类的服务器版本，因为服务器版本为了追求稳定性，不但组件不是最新的，就连网上的组件源也不是最新的。

可以使用ubuntu 9.04桌面版，不过里面缺少很多开发用的组件，除了

http://trac.webkit.org/wiki/BuildingGtk

列出的之外，还有不少组件需要下载。主要有：

1)autoconf

2)libtool

3)gtk-doc-tools

4)libgail-dev

参考：

https://mp.weixin.qq.com/s/QqpPGWf3IVEDN1t80CZ06Q

深入理解浏览器原理

# Lua

Lua的包管理工具叫做LuaRocks。官网：

https://luarocks.org/

参考：

https://mp.weixin.qq.com/s/nwhSDxz1Pu2JCU_IeMR9ww

Lua程序逆向之Luac文件格式分析

http://lua-users.org/wiki/GraphicalUserInterfaceToolkits

Lua的GUI工具列表

# 辛普森悖论

![](/images/img2/Simpson.png)

如果分专业来看，你就会发现：在各个专业女生的录取率其实都是更高的。之所以会产生“总体录取率女生偏低”这一结果，是因为女生大部分都报考了那些本身就难以录取的学院，而男生则大部分报考了那些录取率本身就偏高的学院。

参考：

https://mp.weixin.qq.com/s/5jZ2dzLInLtUw7rWZF4mtg

张忠元：渣男受女生欢迎？当心统计陷阱

https://mp.weixin.qq.com/s/o1a2YlYritcOrsLN2YuLmA

神奇的霍特林法则：为什么汉堡王总是开在麦当劳旁边？

https://mp.weixin.qq.com/s/eq4MllJta5NmaLARPpvang

公交车总迟到？你大概掉进了“等待时间悖论”

https://zhuanlan.zhihu.com/p/43934918

诡异的布雷斯悖论：为什么越是修新路，城市反而更堵了！

https://mp.weixin.qq.com/s/-0VMucGBq4Trb_9FnsW6KQ

10大反直觉的数学结论

https://mp.weixin.qq.com/s/FqY19sTQd7GPdGSsB5L9eQ

数学大反例合集

https://mp.weixin.qq.com/s/EICefFM3dfv5A6V9kVqGWw

吸烟致癌的迷思是如何破除的

https://mp.weixin.qq.com/s/NlJ4-b5SjIjPGgvLUuSxFw

孩子，有时候并不是生活欺骗了你，而是你可能还不懂概率统计……

# 肺炎版《黄冈密卷》

问题由来：

https://mp.weixin.qq.com/s/dR7fg6PTCVAnezlW6gTY2w

新冠病毒最“强”管控，《黄冈密卷》数学题到底有多难

----

1.设$$\sqrt{3+\sqrt{2}+\sqrt{3}+\sqrt{6}}=\sqrt{x}+\sqrt{y}+\sqrt{z}$$，且x、y、z为有理数，则$$xyz$$=?

解：

$$3+\sqrt{2}+\sqrt{3}+\sqrt{6} = x+y+z+2\sqrt{xy}+2\sqrt{xz}+2\sqrt{yz}$$

由x、y、z为有理数可得：

$$4xy=2, 4xz=3, 4yz=6$$

由于x、y、z在原式中是对称的，所以上式中选择哪个等于2、3、6，都是无所谓的。

三式相乘可得：

$$4^3 \cdot (xyz)^2 = 36$$

$$(xyz)^2 = 9/16$$

$$xyz = 3/4$$

----

2.设二次函数$$f(x)=ax^2+ax+1$$的图像开口向下，且满足$$f(f(1))=f(3)$$，则$$2a=?$$

解：

令$$y=2a$$，则$$f(1)=2a+1=y+1$$

$$f(f(1))=f(y+1)=f(3)$$

$$a(y+1)^2+a(y+1)+1=9a+3a+1$$

$$(y+1)^2+(y+1)=12$$

$$y^2+3y-10=0$$

$$(y+5)(y-2)=0$$

因为图像开口向下，所以$$2a=-5$$。
