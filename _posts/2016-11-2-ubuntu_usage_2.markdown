---
layout: post
title:  Ubuntu使用技巧（二）
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

`go-mtpfs /media/MyAndroid`

`fusermount -u /media/MyAndroid`

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

# ASCII表情

╮(╯_╰)╭

(^ω^)

# 远程桌面

Linux下的远程桌面软件主要有RealVNC和rdesktop。前者支持VNC协议，而后者支持MS RDP协议，可连接Windows系统。

## rdesktop

安装方法：

`sudo apt-get install rdesktop`

使用方法：

`rdesktop -u administrator -p ****** -a 16 192.168.1.1`

# sdkman

sdkman(The Software Development Kit Manager), 中文名为:软件开发工具管理器．这个工具的主要用途是用来解决在类unix操作系统(如mac, linux等)中多种版本开发工具的切换, 安装和卸载的工作．对于windows系统的用户可以使用Powershell CLI来体验．

例如: 项目A使用Jdk7中某些特性在后续版本中被移除（尽管这是不好的设计），项目B使用Jdk8,我们在切换开发这两个项目的时候，需要不断的切换系统中的JAVA_PATH,这样很不方便，如果存在很多个类似的版本依赖问题，就会给工作带来很多不必要的麻烦． 
　　 
sdkman这个工具就可以很好的解决这类问题，它的工作原理是自己维护多个版本，当用户需要指定版本时，sdkman会查询自己所管理的多版本软件中对应的版本号，并将它所在的路径设置到系统PATH.

官网：

http://sdkman.io/

教程：

http://blog.csdn.net/heiyouhei123/article/details/51103578

