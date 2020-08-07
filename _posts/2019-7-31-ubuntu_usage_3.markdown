---
layout: post
title:  Ubuntu使用技巧（三）
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

# Ubuntu 20.04使用手记

Ubuntu 20.04是2020.4.24发布的。我第一时间上手体验了一番。

UI方面最大的特点是：菜单栏变成了菜单按钮。这种风格最早来自Chrome的设计，后来部分系统应用也采用了该风格，这次算是收尾阶段了吧。

内核：5.4

LibreOffice：6.4

----

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

# Linux参考资源+

https://mp.weixin.qq.com/s/qnZL4ENAbTvVMVcImVTtYw

深入浅出CAS

比较并交换(compare and swap, CAS)，是原子操作的一种，可用于在多线程编程中实现不被打断的数据交换操作，从而避免多线程同时改写某一数据时由于执行顺序不确定性以及中断的不可预知性产生的数据不一致问题。

https://mp.weixin.qq.com/s/T_z2_gsYfs6A-XjVTVV_uQ

说说无锁(Lock-Free)编程那些事（上）

https://mp.weixin.qq.com/s/h75n7sHnrmoLJ4DVAW5AUQ

说说无锁(Lock-Free)编程那些事（下）

https://mp.weixin.qq.com/s/snQ3T86usv4rXz0MMQvFfQ

如何回答性能优化的问题，才能打动阿里面试官？

https://www.cnblogs.com/zhouyu629/p/3734494.html

一次心惊肉跳的服务器误删文件的恢复过程

https://mp.weixin.qq.com/s/A8TnhOFLQOhEqphE760yvw

15个相见恨晚的Linux神器，你可能一个都没见过

https://mp.weixin.qq.com/s/ejGjsGA1ijPP--j3BLcEFA

Linux并发与同步

https://mp.weixin.qq.com/s/CAPU8bjJWobQs6JHHMasvQ

Linux服务端最大并发数是多少？

https://mp.weixin.qq.com/s/iKfWSfzauzNjcAvXPNhq0Q

这些算法都不会还学什么操作系统

# 两弹一星+

2011年，战忽局副局平可夫，在他主编的《汉和防务观察》说，中国虽然有了航母，但是没有拦阻索，飞机照样发挥不了战斗力。

西欧军力最强的法国，也只能从美国进口拦阻索，标价150万美元。

当时只有英国，俄罗斯，美国可以造得出。

但是，就在军方一筹莫展的时候。巨力集团和邢台钢铁锁定军方的这一需求，整合现有技术，仅用4个月就造出了军方满意的拦阻索。

----

野生海带，原产于日本北海道、俄国鄂霍次克海一带的冰冷水域，中国沿海对海带而言太热了。1927年起，日本在中国大连黑石礁（现大连自然博物馆的位置）和烟台进行海带养殖实验，因为中国北部海域水温相对较低，所以取得了一定成果。

抗战胜利后，中共留用了研究海带养殖的日本科学家，并且敏锐的意识到海洋农业对国家战略的重要性。在日本研究成果的基础上，中国科研人员成功突破了海带的自然习性限制，即便是炎热的福建沿海也能养殖海带。通过对以海带为代表的藻类养殖技术攻关，中国在50年代就成为世界主要藻类养殖国家，占当时世界总产量的十分之一还多。

藻类养殖的蓬勃发展，为被西方封锁的新中国提供了碘、琼脂、褐藻胶等关键性工业、医药业原料。而之前的旧中国，连碘酒都无法保证供应，可谓天壤之别。

----

我国早期的核潜艇噪音水平非常糟糕，曾被赤裸裸地嘲讽为“海底拖拉机”，“只要一下海，远在万里之外的英国都能听到。”

美国目前的核潜艇是中压交流系统，依然需要变速箱和主轴带动螺旋，这样的话噪音还是会有一部分通过主轴传到水中。

2001年，马伟明研制出世界上首台交直流双绕组发电机系统，彻底摒弃了传统机械推进轴，将噪音降低10%以上，完成了一次“潜艇静音”的创举。而在这基础上，2010年，他研发出“中压直流输电网”，中国也因此成为了，世界上第一个在舰船上实现中压直流综合电力系统的国家。这一技术水平，反超领先国外十年左右！

----

![](/images/img3/top10.png)

https://mp.weixin.qq.com/s/1NXjgkr3xWDzqm7Y-9x7-Q

从核潜艇到华龙一号：“其他大国有的，我们也要有”

https://mp.weixin.qq.com/s/sRVb8qGtbsuqCvHSrlL9mQ

卡住中国脖子的35项技术

https://mp.weixin.qq.com/s/wsGgWzyaslXCQflaP2ccXA

60年前首登珠峰，他是唯一在世亲历者

https://mp.weixin.qq.com/s/zwlOZzdGlqTFGSg3H9zNxQ

珠峰变“绿”，“隐者”现身，这些年的珠峰发生了什么？

https://mp.weixin.qq.com/s/MCAR8M2DoV30-McdsjTxDw

徒步横穿南极大陆第一人

>秦大河，1947年生，地理学家，中国科学院院士，第三世界科学院院士。曾任中国气象局局长、中国科学技术协会副主席等
