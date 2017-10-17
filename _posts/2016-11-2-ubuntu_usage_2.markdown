---
layout: post
title:  Ubuntu使用技巧（二）, CentOS
category: technology 
---

# Ubuntu 16.04使用手记

Ubuntu 16.04正式发布（2016.4.21）之后，我第一时间下载了下来。

平心而论，虽然厂商已经很努力，但是Ubuntu的版本升级，仍然存在诸多不兼容的问题。我的电脑最初装的是12.04，后来利用apt升级为14.04。然而，从这次的升级体验来说，不仅升级耗时远比重新安装多，而且有些软件并不能自动升级到新版本，因此，就存在和Ubuntu新版本的兼容问题。而这次16.04的升级更绝，我升级之后，电脑直接不能开机了。因此，必须重新安装Ubuntu。

硬盘安装Ubuntu 16.04的步骤，与之前的版本完全相同，不再赘述。

总的来说，这次的升级没有大的变化，但小的改进还是不少的。

1.内核版本升级到4.4。这个太没存在感了，囧。

2.LibreOffice升级到5.X。外观上更简约了，赞一个。

3.Emacs升级到24.5。提一个细节，以前打开同名文件，文件名的后面按照打开顺序加序号，以示区别，但序号含义很不直观。现在用父文件夹名来区分，好用多了。

下面对使用中遇到的问题，及其解决方法，总结如下：

1.安装Flash插件。

有两个办法——要么安装Chrome，要么将Adobe官方驱动中的libflashplayer.so，安装到~/.mozilla/plugins下。

2.gvfsd-smb-browse进程的CPU占用100%。

这是一个ISP DNS导致的问题。其中一个解决方法：

`sudo apt-get remove gvfs-backends`

# 清理系统

## 清理安装包

`sudo apt-get clean`

## 清理旧内核

1.首先查看旧内核情况

`dpkg --get-selections | grep linux`

2.删除旧内核

`sudo apt-get purge linux-image-4.4.0-21-generic linux-headers-4.4.0-21`

# 查看版本号

## 查看ubuntu版本号

`cat /etc/issue`

## 查看内核版本号

`cat /proc/version`

# Graphviz

Graphviz是一个绘“图”软件。这里的“图”不是指一般意义上的图片或者照片，而是指数据结构或者图论中的抽象意义上的数学概念中的“图”。Graphviz就是一个用来将“图”可视化的工具集。

官网：

http://graphviz.org/

源代码：

https://github.com/ellson/graphviz/

安装方法：

`sudo apt-get install graphviz graphviz-dev`

其中，后者是graphviz的开发工具包，便于其他软件集成graphviz的相关功能。

graphviz包括了以下工具：

## 图形生成类

按照布局方式的不同，包括circo、dot、fdp、neato、osage、sfdp、twopi等命令。例如：

`dot -Tpng hello.gv -o hello.png`

该命令将hello.gv按照dot布局方式输出成png文件。

## 图形查看类

1.xdot

`sudo apt-get install xdot`

这个工具功能简单，只能按照dot布局方式查看文件。

2.kgraphviewer

`sudo apt-get install kgraphviewer-dev`

这个工具可以选择查看的布局方式。

## 编辑类

包括leftty、dotty，界面丑陋，并不好用。

还有个叫smyrna的工具，据称使用3D加速，可处理10000个结点的图。但是ubuntu下没有方便的安装方式。

# Android手机MTP连接Ubuntu

`sudo add-apt-repository ppa:webupd8team/unstable`

`sudo apt-get update`

`sudo apt-get install go-mtpfs`

`sudo chown <user name> /media/mtp`

`go-mtpfs /media/mtp`

`fusermount -u /media/mtp`

# 文件校验和

计算文件校验和，一般采用MD5和SHA算法。在Ubuntu中，这些算法的命令包括：md5sum、sha1sum(160-bit) ,sha224sum(224-bit) ,sha256sum(256-bit),sha384sum(384-bit),sha512sum(512-bit)等。

# 产品设计工具

| 类别 | 收费 | 免费 |
|:--:|:--:|:--:|
| Office | MS Office | LibreOffice |
| 流程图 | MS Visio | DIA、Kivio |
| 思维导图 | Mindmanager | FreeMind |
| 快速原型 | Axure RP | pencil |

# 发行版乱战

Linux以发行版众多闻名于世。最近发现了以下网站，或可对各个发行版进行一个简单的比较。

http://distrowatch.com/

下面对几个主要的参数，进行一下点评：

## Office

主要是3个流派：

1.StarOffice->OpenOffice.org->LibreOffice。最初由Sun主导，后来改为Google主导。

2.KOffice->Calligra Office。KDE项目的成果。

3.GOffice。Gnome项目的成果，和前两个相比，GOffice的组件比较独立，没有什么协同能力。

# Firefox插件

http://mozilla.com.cn/addon/76-pagesaver/

这个插件可以将网页保存为图片。

# Virtual MIDI Piano Keyboard

VMPK是一款MIDI生成工具软件，也就是俗称的“虚拟电子琴”软件。但它本身只生成MIDI输出，需要配合使用MIDI后处理软件，才能发声。常见的MIDI后处理软件有Qsynth、TiMidity。

# 便签软件

主要有两类便签软件：

1.支持超链接的便签。典型的有Gnote和Tomboy，这两个软件都有内容检索的功能。

2.桌面随意贴。典型的有Indicator Stickynotes和Knotes。后者有内容检索的功能，而前者没有。

# ASCII表情

╮(╯_╰)╭

(^ω^)

# 常用英语缩写

FYI：for your information

# 远程桌面

Linux下的远程桌面软件主要有RealVNC和rdesktop。前者支持VNC协议，而后者支持MS RDP协议，可连接Windows系统。

## rdesktop

安装方法：

`sudo apt-get install rdesktop`

使用方法：

`rdesktop -u administrator -p ****** -a 16 192.168.1.1`

# 安装工具

目前研发用的主流操作系统越来越多，相比于10年前的Windows几乎一统天下，目前Linux、Mac OS X在开发群体中，也有一定的流行度。更不用说Linux本身还有众多的发行版，软件部署工作在这么复杂的环境中，实非易事。

以下介绍的软件，都能不同程度的改善软件部署的工作。

## sdkman

sdkman(The Software Development Kit Manager), 中文名为:软件开发工具管理器．这个工具的主要用途是用来解决在类unix操作系统(如mac, linux等)中多种版本开发工具的切换, 安装和卸载的工作．对于windows系统的用户可以使用Powershell CLI来体验．

例如: 项目A使用Jdk7中某些特性在后续版本中被移除（尽管这是不好的设计），项目B使用Jdk8,我们在切换开发这两个项目的时候，需要不断的切换系统中的JAVA_PATH,这样很不方便，如果存在很多个类似的版本依赖问题，就会给工作带来很多不必要的麻烦． 
　　 
sdkman这个工具就可以很好的解决这类问题，它的工作原理是自己维护多个版本，当用户需要指定版本时，sdkman会查询自己所管理的多版本软件中对应的版本号，并将它所在的路径设置到系统PATH.

官网：

http://sdkman.io/

教程：

http://blog.csdn.net/heiyouhei123/article/details/51103578

## Flatpak

Linux社区出现了两种新的应用打包格式，其一是Ubuntu力推的snap格式，另一种是Red Hat主导开发的Flatpak格式，两种包格式都利用了沙盒隔离应用，增强安全性。

支持snap包的开源软件包括了Firefox、LibreOffice、Krita和Mycroft等，而提供了Flatpak包的应用有LibreOffice、GIMP、InkScape、MyPaint和Darktable。

http://flatpak.org/

## Snap

http://snapcraft.io/

## Conda

Conda 是一个开源的软件包管理系统和环境管理系统，用于安装多个版本的软件包及其依赖关系，并在它们之间轻松切换。它是目前最流行的Python环境管理工具。

https://conda.io/docs/

# Putty

putty在ubuntu平台的复制粘贴，依赖于鼠标中键。

# XMind

XMind是一款开源的思维导图工具，比FreeMind更友好。官网：

https://www.xmind.net/

# Chrome

`sudo wget http://www.linuxidc.com/files/repo/google-chrome.list -P /etc/apt/sources.list.d/`

`wget -q -O - https://dl.google.com/linux/linux_signing_key.pub  | sudo apt-key add -`

`sudo apt update`

`sudo apt-get install google-chrome-stable`

`google-chrome-stable`

安装flash：

1、首先在adobe官网下载tar.gz格式的linux安装包，之后将其解压。

2.`sudo gedit /usr/share/applications/google-chrome.desktop`

3.将`Exec=/usr/bin/google-chrome-stable %U`后，添加`--ppapi-flash-path=path/libpepflashplayer.so --ppapi-flash-version=<version>`

# 常用快捷键

Ctrl+Alt+T：启动Terminal

Ctrl+Super+D：最小化所有窗口

# 桌面主题

用腻了系统自带的桌面主题之后，我打算换个新鲜一些的桌面主题，比如Mac OS X风格的。

1.安装主题修改工具

`sudo apt-get install unity-tweak-tool`

2.安装Mac OS X主题

{% highlight bash %}
sudo add-apt-repository ppa:noobslab/themes
sudo apt-get update
sudo apt-get install mac-ithemes-v3 mac-icons-v3
{% endhighlight %}

3.Cairo Dock

做完上面两步之后，基本的Mac OS X风格已经有了，但Mac最经典的Dock启动器还没有。这里介绍一下Cairo Dock。

安装方法：

`sudo apt-get install cairo-dock`

Cairo Dock不仅具有类似Mac OS X的风格，还有其他的风格可供选择下载。比如我使用的是Chrome风格。

4.其他主题

http://www.ubuntuthemes.org/

这个网站收集了很多桌面主题，但是需要注册，因为有些主题是收费的。

# CentOS

## 关于repo设置

最近在数台PC上部署软件，系统都是CentOS 6。结果发现其中有一台机器无法使用yum安装软件。

解决办法：

1.进入/etc/yum.repos.d中删除CentOS6-Base.repo之外的所有文件。

2.`yum clean all`

第1步很重要，从事后情况来看，故障是由于某些之前的repo现在已经无法连接所致导致的。

## start-stop-daemon

start-stop-daemon是Ubuntu中用的比较多的工具，但是CentOS中并没有。由于start-stop-daemon在ubuntu的dpkg包中，和apt关系比较近，因此直接下载源码，也不是个好办法。

https://packagecloud.io/willgarcia/start-stop-daemon/install

上面的网页提供了一种办法。但是由于网络不好，中间步骤的文件有时需要手动下载才行。

## 关于epel

1.安装epel源

`yum install epel-release`

2.修改/etc/yum.repos.d/epel.repo

将mirrorlist中的https修改为http。否则，会报如下错误：

`Error: Cannot retrieve metalink for repository: epel`

## DNF

`yum install dnf`

这个似乎需要Cent OS 7以上，Cent OS 6反正是不行的。

## 关于VNC

1.安装

`yum install tightvnc-server`

这里虽然包名叫做tightvnc-server，但实际上用的是tigervnc-server，因此以后者为包名来安装也是可以的。

2.初次启动，设置密码

`vncpasswd`

3.配置分辨率、端口

修改/etc/sysconfig/vncservers：

{% highlight bash %}
## Single User ##
VNCSERVERS="1:<user name>"
VNCSERVERARGS[1]="-geometry 1280x1024"
{% endhighlight %}

默认端口一般是5900~5904。这里的数组下标表明它使用的端口是5901。

4.启动服务

`/etc/init.d/vncserver start`

参考：

http://www.tecmint.com/install-tightvnc-remote-desktop/


