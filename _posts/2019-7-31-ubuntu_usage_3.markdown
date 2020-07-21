---
layout: post
title:  Ubuntu使用技巧（三）
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

# 两弹一星+

我们将不但有一个强大的陆军，而且有一个强大的空军和一个强大的海军。让那些内外反动派在我们面前发抖罢，让他们去说我们这也不行那也不行罢，中国人民的不屈不挠的努力必将稳步地达到自己的目的。

美帝国主义者很傲慢，凡是可以不讲理的地方就一定不讲理，要是讲一点理的话，那是被逼得不得已了。 

这一次，我们摸了一下美国军队的底。对美国军队，如果不接触它，就会怕它。我们跟它打了三十三个月，把它的底摸熟了。美帝国主义并不可怕，就是那么一回事。我们取得了这一条经验，这是一条了不起的经验。

斗争是团结的手段，团结是斗争的目的。以斗争求团结则团结存，以退让求团结则团结亡。

我们的同志在困难的时候，要看到成绩，要看到光明，要提高我们的勇气。

我认为，对我们来说，一个人，一个党，一个军队，或者一个学校，如若不被敌人反对，那就不好了，那一定是与敌人同流合污了。如若被敌人反对，那就好了，那就证明我们同敌人划清界限了。如若敌人起劲地反对我们，把我们说得一塌糊涂，一无是处，那就更好了，那就证明我们不但同敌人划清了界限，而且证明我们的工作是很有成绩的了！

中国人民有志气，有能力，一定要在不远的将来，赶上和超过世界先进水平。

![](/images/img3/korea_war.webp)

http://www.china.com.cn/ch-America/Wenxian/53-9-12.htm

毛泽东:抗美援朝的胜利和意义

----

![](/images/img3/BD.jpg)

为了阻止中国发展全球定位系统，美国1985年的时候就展示了反卫星能力。但是2007年1月11日，美国看到了这样一幕，中国使用机动陆基导弹SC-19实现了反卫星，这让美国明白了意思。美国如果敢摧毁中国卫星，中国将毫不犹豫的摧毁美国的卫星。

2007年1月进行一次反卫星试验，直接摧毁了一枚中国气象卫星。

2009年中国宣布独立搞北斗3系统。

2010年中国北斗1.0系统上线。同年中国透露了DN-1导弹。

2013年中国北斗开始加速部署。同年5月中国DN-2导弹上线。

2015年10月30日，在位于中国西部的库尔勒导弹测试场进行了“动能-3”导弹大气层外拦截试验。

2017年北斗39颗卫星组网，并向全球提供RNSS服务。

https://www.zhihu.com/question/403014869

北斗全球系统在太空有没有可能被别的国家破坏或击落？

----

步履式挖掘机1965年由列支敦士登的“kaiser（凯撒）”公司发明。这种挖掘机由于有四个可以独立移动的支腿，因此可以在山地、沼泽等普通挖掘机不能使用的地区使用。

2001年，徐工集团决定接下这个任务。2004年初整车开始了高原测试，两辆原型车被拉到了工程兵某部位于西藏羊八井附近山区中的工地，当地海拔接近5000米。结果测试完车没下来，直接就被驻军扣下了。后来好说歹说还了一台，剩下的一台我15年去徐工的时候，据说依然活跃在中印边境上，十多年没出过大故障。

中国：building，construction completed，new construction options，unit ready.

印度：Insufficient fund，cannot deploy here，building，low power，unit lost.

https://www.zhihu.com/question/404380682

如何看待徐工机械的全地形挖掘机?

----

1、凡是中国和外国都掌握的，都是没什么难度的技术；

2、凡是中国掌握而外国没掌握的，都是没用的技术；

3、凡是外国掌握而中国没掌握的，都是屌炸天的技术。

有这三句话，喷遍知乎不成问题。
