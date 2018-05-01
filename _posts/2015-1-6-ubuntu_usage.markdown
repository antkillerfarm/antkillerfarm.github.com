---
layout: post
title:  Ubuntu使用技巧（一）
category: linux 
---

# 在Ubuntu上安装VMWare tools

VMWare自带的VMWare tools在新版的Ubuntu上总是安装不上，其实解决方法也很简单。

`sudo apt-get install linux-headers-virtual open-vm-dkms open-vm-tools（图形界面）`

或

`sudo apt-get install --no-install-recommends linux-headers-virtual open-vm-dkms open-vm-tools（命令行）`

# 如何以管理员身份操作Gnome的资源管理器--nautilus

`apt-get install nautilus-gksu`

# 没有声音

没有声音的话，使用alsamixer来配置一下。

# 显示实时网速

最近下载安装了ubuntu 12.04 LTS。由于它使用了Unity桌面，因此之前的一些GNOME桌面工具不再可用。

为了显示实时网速，我找到了indicator-netspeed这个小工具，其安装过程如下：

* install the dependencies:

`sudo apt-get install build-essential libgtop2-dev libgtk-3-dev libappindicator3-dev git-core`

* create a folder for git_project and download the code.

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/indicator-netspeed

原版程序是针对gtk3的早期版本，有些老了：

https://github.com/mgedmin/indicator-netspeed.git

* do make

`make`

* launch the indicator:

`sudo make install`

`indicator-netspeed`

附带的说一下，刚开始的时候，我给这个程序添加了一个桌面快捷方式。但是每次开机还要按一下快捷方式，着实不方便。后来发现在选择“关机”的那个菜单上方还有个叫做“启动应用程序”的东东，之前看名字还以为是Windows下Run的替代品，结果实际上是桌面的开机启动程序。。。

另，修改~/.profile之类的文件是不行的，因为那是在进入桌面之前运行的。由于桌面还没有ready，好多桌面程序都是跑不起来的。

在Ubuntu 14.04中“启动应用程序”找不着了，但是实际的功能实现机制还是没有变——在~/.config/autostart下创建desktop文件。

## 虚拟机和宿主机的文件共享——FTP方式

最近打算在win7的系统上，搭建ubuntu 14.04的虚拟机。由于使用的vmware的版本比较老，只有8.0，其中自带的VM Tool无法在最新的内核下正常工作（有编译错误）。因此在不得以的情况下，只好使用FTP的方式，实现虚拟机和宿主机之间的文件共享。

最初，打算在ubuntu的虚拟机上搭建FTP服务器，而在windows下用Filezilla客户端访问并共享。所用的参考文献如下:

http://blog.chinaunix.net/uid-11187-id-3026834.html

如何搭建虚拟机的FTP服务器

但是最后并没有成功，现将主要的关键点罗列如下：

1）ubuntu使用vsftpd作为FTP服务器。搭建之后，在虚拟机中可以正常访问，但宿主机不行。

2）按照文献中的方法，做好Host和Guest的21端口的映射。这时，FTP登录成功，但LIST列出目录不成功。

3）反复尝试各种设置，包括FTP主动、被动模式，虚拟机端口映射等，但是始终不能正常访问FTP。

最后，比较了一下虚拟机和真实机器在组网上的差异后，我忽然意识到虚拟机FTP不能正常访问的原因，应该是由于虚拟机是在一个虚拟的内网之中。默认情况下，外网机器是无法访问虚拟机的，而虚拟机则可以正常访问外网。因此，反过来，我在win7上用IIS搭建FTP服务，然后在ubuntu虚拟机上用Filezilla访问FTP。这下终于成功了。

# Ubuntu使用小技巧

安装 7zip：

`sudo apt-get install p7zip`

安装 rar:

`sudo apt-get install rar unrar`

rar比较奇怪，压缩和解压是使用不同的包，这点和7zip是不一样的。

`cd - //bash`中回到上一次所在的路径的命令。当需要在两个相隔较远的路径下，相互切换的时候，可以使用该命令。

常按Win键，会弹出Unity所用的键盘快捷键。

## ape文件的处理

Monkey's Audio，是一种常见的无损音频压缩编码格式，扩展名为.ape。

最近想在Ubuntu下听音乐，但是系统自带的Rhythmbox虽然支持ape文件的播放，却不支持和ape配套的cue文件的播放。在网上查了一圈，最终使用如下方法解决了这个问题：

1）Add the following line to /etc/apt/sources.list:

`deb http://www.deb-multimedia.org squeeze main`

2）Update the package index:

`sudo apt-get update`

3)Install GPG key of the repository:

`sudo apt-get install deb-multimedia-keyring`

4)Install monkeys-audio deb package:

`sudo apt-get install monkeys-audio`

5)安装shntool和flac包

`sudo apt-get install shntool flac`

6)执行以下命令将ape切割成flac文件

`shnsplit -f CDImage.cue -i ape -t '%t' -o flac CDImage.ape`

这里特别关注一下http://pkgs.org这个网站，好多deb包都可以在这里找到。

# 向devhelp添加新书

1）最好的办法是在安装开发环境的包的时候，安装包自动给你把书装好。例如，我最近研究GTK3，在安装相关包的时候，GObject之类的书就已经安装好了。

2）除此之外，有一些项目的源码中也有doc目录，如果在里面找到以devhelp（或者devhelp2）为扩展名的文件的话，那么说明该项目的帮助文件支持devhelp查看。这时可将包含devhelp（或者devhelp2）为扩展名的文件的那个文件夹复制到devhelp专门放书的目录下，并将文件夹的名字改成和devhelp（或者devhelp2）为扩展名的文件的主文件名一致即可。

devhelp每个版本放书的目录都不尽相同，一般如果安装了gtk的话，可以找找gtk-doc文件夹的位置，然后把书放到gtk-doc下。

`sudo apt-get install libgtk-3-doc`

# Unity侧边栏快速启动的研究

Unity侧边栏和Win7的任务栏有些类似，不仅会显示当前正在执行的程序，同时也可以将正在执行的程序的图标锁定在侧边栏上。但是侧边栏的位置有限，当锁定的图标太多时，就会干扰对正在执行的程序的选定。

在Win7/XP上，可以通过快速启动栏的方式解决这个问题。当需要快速启动的图标过多时，快速启动栏上会自动出现一个可以扩展的箭头按钮。但在Unity中就没有类似的简单的办法了。

其实Unity的侧边栏功能还是比较丰富的，除了可以像Win那样提供图标点击和文件拖放的功能之外，右键点击图标也会弹出一个菜单。而这个右键菜单的功能就要超越Win的右键菜单了。

下面谈谈如何修改，才能用单一图标的右键菜单，启动多个应用程序。

1）在任意位置新建一个文件夹，在该文件夹中创建一个名为MyQuickStart.desktop的文件。

2）用任意文本编辑工具编辑该文件，内容如下：

{% highlight c %}
[Desktop Entry]
Version=1.0
Type=Application
Name=MyQuickStart
Exec=/usr/bin/emacs23 %F
Icon=warrior
Terminal=false
X-Ayatana-Desktop-Shortcuts=StarDict;Devhelp;SystemMonitor;SystemSettings;Glade;LibreOffice
[StarDict Shortcut Group]
Name=StarDict
Name[zh_CN]=星际译王
Exec=stardict
TargetEnvironment=Unity
[Devhelp Shortcut Group]
Name=Devhelp
Exec=devhelp
TargetEnvironment=Unity
[SystemMonitor Shortcut Group]
Name=SystemMonitor
Name[zh_CN]=系统监视器
Exec=gnome-system-monitor
TargetEnvironment=Unity
[SystemSettings Shortcut Group]
Name=SystemSettings
Name[zh_CN]=系统设置
Exec=gnome-control-center --overview
TargetEnvironment=Unity
[Glade Shortcut Group]
Name=Glade
Exec=glade
TargetEnvironment=Unity
[LibreOffice Shortcut Group]
Name=LibreOffice
Exec=libreoffice
TargetEnvironment=Unity
{% endhighlight %}

从本质上来说，这其实就是个桌面启动文件。有兴趣的同学可以用“Desktop Entry”为关键字搜索一下.desktop文件的写法。

此外，还可以在/usr/share/applications文件夹下找到系统目前已安装的桌面应用的.desktop文件，用文本编辑工具打开即可看到其内容。这也是自己写.desktop文件的一个很好的参考。

3）将MyQuickStart.desktop的文件权限改为可执行，并将其拖放到侧边栏，就可以看效果了。

# 软件包管理

## 修改软件源

Ubuntu更新软件时的软件源配置文件是/etc/apt/sources.list。

ubuntu的官方软件源分为4类：

main：这个是官方维护的基本库。

restricted：官方维护的其他自由软件。

universe：自由软件，但是官方不维护。

multiverse：非自由软件，官方不维护。

## apt

apt是一套完整的软件包管理方案。除了最常用apt-get之外，还包括了一系列的客户端和服务器软件。例如：

`sudo apt-cache search gstreamer`

搜索名字中包含gstreamer的软件包。

`sudo add-apt-repository ppa:tualatrix/ppa`

添加新的软件源。

## apt vs. apt-get

在ubuntu14.04以后，apt逐渐取代apt-get，称为默认的软件升级工具。基本可以认为apt=apt-get+apt-cache。

## 软件包的网上查询

使用apt-get获取软件虽然方便，但是从ubuntu的源获得的软件包和直接使用源码编译安装的包相比，包中的各个文件被分散在好多个文件夹中，查找起来很不方便。

这时可以到这个网址：

http://packages.ubuntu.com/

去查找软件包里的文件清单，以弄清楚XX软件官网上所说的YY文件在ubuntu中到底放在哪里。

该网站还可以下载源码包。以libupnp为例，从网上可以下载以下文件：

libupnp_1.6.19+git20160116-1.dsc

libupnp_1.6.19+git20160116.orig.tar.bz2

libupnp_1.6.19+git20160116-1.debian.tar.xz

其中，第二个文件是项目原始的源代码包，而第三个文件是debian项目的patch包。最终使用的代码，实际上是在第二个包上打上patch以后的结果。

## apt离线安装

1.下载deb包。

只下载该软件的deb：

`sudo apt download <package name>`

下载软件deb+未安装的依赖：

`sudo apt-get --print-uris --yes -d --reinstall install <package name> | grep "http://" |  awk '{print$1}' | xargs -I'{}' echo {} | tee files.list`

`wget --input-file files.list`

下载软件deb+所有的依赖：

`PACKAGES="<package name>"`

`apt-get download ${PACKAGES} && apt-cache depends -i ${PACKAGES} | awk '/Depends:/ {print $2}' | xargs  apt-get download`

2.安装deb包。

只安装该软件的deb：

`sudo apt install xx.deb`

安装软件deb+依赖：

把deb放到apt的cache路径下。

apt的cache路径为：/var/cache/apt/archives/

参考：

Ubuntu使用apt-get安装本地deb包

## apt降级（downgrade）安装

有的时候新的版本不好使的情况下，也可采用如下方式降级：

`sudo apt-get install <pkg_name>=<version>`

# 系统清理工具

1.ubuntu tweak

一个国内小伙写的工具。(2016.5停止更新，不支持Ubuntu 16.04以上的版本。)官网：

http://ubuntu-tweak.com/

代码：

https://github.com/tualatrix/ubuntu-tweak

安装依赖：

`sudo apt-get install python-pip python-aptdaemon.gtk3widgets python-gi python-lxml libwebkitgtk-3.0-dev libgconf2-dev python-compizconfig libdbus-glib-1-dev python-dbus python-xdg python-cairo`

2.BleachBit

支持平台广泛，大多数Linux发行版都有对应的软件包。

# tftp

Ubuntu下面关于TFTP的程序，有三套：

1.tftp和tftpd

2.atftp和atftpd

3.tftp-hpa和tftpd-hpa

目前以tftp-hpa和tftpd-hpa最为流行。

安装命令：

`sudo apt-get install tftp-hpa tftpd-hpa`


