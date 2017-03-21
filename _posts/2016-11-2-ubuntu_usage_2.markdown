---
layout: post
title:  Ubuntu使用技巧（二）, Fedora, CentOS, Spring, scrapy
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

# apt vs. apt-get

在ubuntu14.04以后，apt逐渐取代apt-get，称为默认的软件升级工具。基本可以认为apt=apt-get+apt-cache。

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

http://flatpak.org/

## Snap

http://snapcraft.io/

# Putty

putty在ubuntu平台的复制粘贴，依赖于鼠标中键。

# Fedora

Fedora作为主要的Linux发行版之一，我虽然用的不多，但实际上这却是我最早接触的Linux发行版。后来换用Ubuntu，很大的原因是因为：这是Google为Android选择的开发平台。

最近因为工作需要重新捡起了Fedora。但公司所用的版本太过古老，还是2009年的Fedora 12。所以想了一下，开始试用最新的Fedora 22。这里是使用过程中的一些操作笔记。

## 安装

https://getfedora.org/

这是官方的下载地址。这里我用的是Workstation版本。

Fedora 22的默认桌面是GNOME 3.16，这一版的外观借鉴了Mac OS X的一些设计，让人眼前一亮。

## 安装软件

Fedora 22使用dnf替代yum。因此安装基本gcc开发环境，可用如下命令：

`dnf install gcc kernel-devel patch bison flex subversion`

如果下载速度较慢的话，可以在/etc/dnf/dnf.conf最后添加：

`fastestmirror=true`

保存后，执行

{% highlight bash %}
$ sudo dnf clean all
$ sudo dnf makecache
{% endhighlight %}

此外，和Ubuntu一样，Fedora也有自己的网站可以查询软件包信息：

https://admin.fedoraproject.org/pkgdb/

## 共享文件夹

我用的是VirtualBox的虚拟环境，因此除了在VirtualBox中，设置共享文件夹之外，还需对Fedora进行如下操作：

1.添加用户到vboxsf中。

`usermod -a -G vboxsf <your user name>`

2.重启。（这一步必不可少，否则上面的配置不会生效。）

这样就可以在Fedora中浏览共享文件夹了。

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

# Spring

Spring是一个Java Web应用框架。官网：

http://spring.io/

## Ubuntu安装Eclipse、Spring

1.安装Eclipse

`sudo apt-get install eclipse`

2.安装Spring

`sudo apt-get install libspring-web-portlet-java`

注意：ubuntu软件仓库中还有一个叫做spring的游戏引擎，不要弄错了。

http://www.mkyong.com/spring/quick-start-maven-spring-example/

http://wiki.jikexueyuan.com/project/spring/

## Restful

http://spring.io/guides/gs/rest-service/

## Spring Boot

https://www.tianmaying.com/tutorial/deploy-spring-boot-application

http://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html

Spring Boot默认的配置文件

## WebService

https://spring.io/guides/gs/producing-web-service/

http://localhost:9999/ws/countries.wsdl

# scrapy

scrapy是一个Python写的网页抓取分析工具。网页抓取分析的学名叫做“Web scraping”，可在wiki上获得更多的相关信息。

官网：

https://scrapy.org/

安装：

`sudo apt install python-scrapy`

`scrapy crawl csdn`

参考：

https://segmentfault.com/a/1190000000583419

一个中文简易教程。

https://github.com/scrapy/dirbot

官方例程。

http://www.cnblogs.com/fengzheng/p/4974509.html

另一个中文简易教程。


