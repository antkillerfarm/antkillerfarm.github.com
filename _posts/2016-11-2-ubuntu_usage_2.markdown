---
layout: post
title:  Ubuntu使用技巧（二）
category: linux 
---

* toc
{:toc}

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

`sudo apt remove gvfs-backends`

# Battle of Wesnoth

在逛Ubuntu软件中心的时候，发现了Battle of Wesnoth这个开源的回合制战旗游戏。试着玩了一下，感觉蛮不错的。正好这个项目在Source Forge上使用Git管理源代码。考虑到Android Source也是用Git管理的，于是就用Git下载了Battle of Wesnoth的source来熟悉一下Git的用法。

# 系统清理工具

1.ubuntu tweak

一个国内小伙写的工具。(2016.5停止更新，不支持Ubuntu 16.04以上的版本。)官网：

http://ubuntu-tweak.com/

代码：

https://github.com/tualatrix/ubuntu-tweak

安装依赖：

`sudo apt install python-pip python-aptdaemon.gtk3widgets python-gi python-lxml libwebkitgtk-3.0-dev libgconf2-dev python-compizconfig libdbus-glib-1-dev python-dbus python-xdg python-cairo`

2.BleachBit

支持平台广泛，大多数Linux发行版都有对应的软件包。

3.gnome-tweaks

这是Gnome官方的优化工具，当然了这个主要关注gnome桌面的设置，并没有系统设置的内容。

4.gconf-editor

这个相当于是Windows的注册表编辑器在Gnome桌面的等价物。

---

如何以管理员身份操作Gnome的资源管理器--nautilus

`apt install nautilus-gksu`

地址栏不能用输入，使用快捷键Ctrl+L

# 清理系统

## 清理安装包

`sudo apt clean`

## 清理旧内核

1.首先查看旧内核情况

`dpkg --get-selections | grep linux`

2.删除旧内核

`sudo apt purge linux-image-4.4.0-21-generic linux-headers-4.4.0-21`

智能版：

`sudo dpkg --get-selections | grep deinstall | sed 's/deinstall/\lpurge/' | sudo dpkg --set-selections; sudo dpkg -Pa`

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

`sudo apt install graphviz libgraphviz-dev`

其中，后者是graphviz的开发工具包，便于其他软件集成graphviz的相关功能。

graphviz包括了以下工具：

## 图形生成类

按照布局方式的不同，包括circo、dot、fdp、neato、osage、sfdp、twopi等命令。例如：

`dot -Tpng hello.gv -o hello.png`

该命令将hello.gv按照dot布局方式输出成png文件。

## 图形查看类

1.xdot

`sudo apt install xdot`

这个工具功能简单，只能按照dot布局方式查看文件。

2.kgraphviewer

`sudo apt install kgraphviewer-dev`

这个工具可以选择查看的布局方式。

## 编辑类

包括leftty、dotty，界面丑陋，并不好用。

还有个叫smyrna的工具，据称使用3D加速，可处理10000个结点的图。但是ubuntu下没有方便的安装方式。

# Android手机MTP连接Ubuntu

`sudo add-apt-repository ppa:webupd8team/unstable`

`sudo apt update`

`sudo apt install go-mtpfs`

`sudo chown <user name> /media/mtp`

`go-mtpfs /media/mtp`

`fusermount -u /media/mtp`

>2018.7 Ubuntu 18.04最近的更新终于可以默认支持MTP连接了。偶尔遇到问题的话，可以尝试同时重启PC和手机，再重新连接的方法。

在没有MTP之前，有些手机的USB传输文件功能采用了如下方式：

1.手机连接并选择传输文件。

2.手机卸载SD卡驱动。

3.PC挂载SD卡，将之视为一个移动硬盘。

这个方式的问题在于：USB连线通常来说，不算很稳定，经常会出现传输了一半，突然被拔线的使用场景。设备的反复卸载/加载对文件系统的数据完整性压力很大。经常有SD卡需要格式化，才能继续加载使用的情况。

MTP类似于HTTP之类的应用层通信协议传输，由于不涉及驱动的变化，随时插拔都无所谓。

# 文件校验和

计算文件校验和，一般采用MD5和SHA算法。在Ubuntu中，这些算法的命令包括：md5sum、sha1sum(160-bit)、sha224sum(224-bit)、sha256sum(256-bit)、sha384sum(384-bit)、sha512sum(512-bit)等。

`sha256sum a.data>a.sha256.txt`

参考：

https://mp.weixin.qq.com/s/FPf2EMwHPsNetxx_sCBzNA

MD5算法和SHA-1算法

# 产品设计工具

| 类别 | 收费 | 免费 |
|:--:|:--:|:--:|
| Office | MS Office | LibreOffice |
| 流程图 | MS Visio | DIA、Kivio |
| 思维导图 | Mindmanager | FreeMind |
| 快速原型 | Axure RP | pencil |

## UMLet

UMLet是最近流行的一个绘制UML图的工具。它生成后缀为`.uxf`的UML图文件。

官网：

https://www.umlet.com/

除了独立工具之外，还提供了VSCode和Eclipse的插件。

## draw.io

draw.io是一个在线的流程图工具。

官网：

https://app.diagrams.net/

此外，它也有离线版和VSCode插件。

## ProcessOn

ProcessOn也是一个在线的流程图工具。

官网：

https://www.processon.com

## mxGraph

mxGraph是一个JS的流程图工具库。上面提到draw.io、ProcessOn都使用了该库。

官网：

https://jgraph.github.io/mxgraph/

参考：

https://juejin.cn/post/6844903811115401224

mxgraph的艰难入门

https://yejinzhan.gitee.io/2019/04/27/mxGraph%20%E5%85%A5%E9%97%A8%E5%AE%9E%E4%BE%8B%E6%95%99%E7%A8%8B/

mxGraph入门实例教程

# Firefox插件

http://mozilla.com.cn/addon/76-pagesaver/

这个插件可以将网页保存为图片。

# Virtual MIDI Piano Keyboard

VMPK是一款MIDI生成工具软件，也就是俗称的“虚拟电子琴”软件。但它本身只生成MIDI输出，需要配合使用MIDI后处理软件，才能发声。常见的MIDI后处理软件有Qsynth、TiMidity。

# Xming

除了远程桌面之外，X Server也是远程执行GUI程序的一种方法。Xming就是Windows平台上用的较多的X Server。

官网：

https://sourceforge.net/projects/xming/

Xming安装运行之后，还需要对putty进行设置，Connection->SSH->X11->Enable X11 forwarding，X display location: localhost:0。

# 安装工具

目前研发用的主流操作系统越来越多，相比于10年前的Windows几乎一统天下，目前Linux、Mac OS X在开发群体中，也有一定的流行度。更不用说Linux本身还有众多的发行版，软件部署工作在这么复杂的环境中，实非易事。

以下介绍的软件，都能不同程度的改善软件部署的工作。

## sdkman

sdkman(The Software Development Kit Manager), 中文名为:软件开发工具管理器．这个工具的主要用途是用来解决在类unix操作系统(如mac, linux等)中多种版本开发工具的切换, 安装和卸载的工作．对于windows系统的用户可以使用Powershell CLI来体验．

例如: 项目A使用Jdk7中某些特性在后续版本中被移除（尽管这是不好的设计），项目B使用Jdk8,我们在切换开发这两个项目的时候，需要不断的切换系统中的JAVA_PATH,这样很不方便，如果存在很多个类似的版本依赖问题，就会给工作带来很多不必要的麻烦。

sdkman这个工具就可以很好的解决这类问题，它的工作原理是自己维护多个版本，当用户需要指定版本时，sdkman会查询自己所管理的多版本软件中对应的版本号，并将它所在的路径设置到系统PATH.

官网：

http://sdkman.io/

安装：

`curl -s "https://get.sdkman.io" | bash`

教程：

http://blog.csdn.net/heiyouhei123/article/details/51103578

## Flatpak

Linux社区出现了两种新的应用打包格式，其一是Ubuntu力推的snap格式，另一种是Red Hat主导开发的Flatpak格式，两种包格式都利用了沙盒隔离应用，增强安全性。

支持snap包的开源软件包括了Firefox、LibreOffice、Krita和Mycroft等，而提供了Flatpak包的应用有LibreOffice、GIMP、InkScape、MyPaint和Darktable。

官网：

http://flatpak.org/

## Snap

官网：

http://snapcraft.io/

使用方法：

安装：`sudo snap install <snap name>`

更新：`sudo snap refresh <snap name>`

更新所有：`sudo snap refresh`

删除：`sudo snap remove <snap name>`

有的时候，snap官方的下载比较慢，这时可以到如下网站直接下载snap包：

https://uappexplorer.com/snaps

安装离线包：

`sudo snap install xxx.snap --dangerous`

参考：

https://www.ubuntu.com/desktop/snappy

A ‘snap’ is a universal Linux package

https://blog.csdn.net/dyxcome/article/details/86105724

解决ubuntu安装软件has install-snap change in progress错误

## Ubuntu Make

Ubuntu Make前身是Ubuntu Developer Tools Center。可在Ubuntu平台上快速安装各种语言的开发环境。

`sudo add-apt-repository ppa:ubuntu-desktop/ubuntu-make`

`sudo apt update`

`sudo apt install ubuntu-make`

`umake ide eclipse`

# Putty

putty在ubuntu平台的复制粘贴，依赖于鼠标中键。

# 思维导图

## XMind

官网：

https://www.xmind.net/

## Freemind

官网：

http://freemind.sourceforge.net/wiki/index.php/Main_Page

安装：

`sudo snap install freemind`

## Freeplane

官网：

https://www.freeplane.org/wiki/index.php/Home

安装：

`sudo apt install freeplane`

功能上来说，Freeplane不算太强，但是它原生支持Latex。。。

## Other

https://mp.weixin.qq.com/s/yD12Ih29_9CwwOe4z5BtgQ

被收费绘图工具PUA了怎么办？来看看这个老实工具吧
