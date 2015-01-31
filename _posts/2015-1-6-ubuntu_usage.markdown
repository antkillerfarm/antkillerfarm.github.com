---
layout: post
title:  Ubuntu使用技巧
category: technology 
---

## 1.在Ubuntu上安装VMWare tools

VMWare自带的VMWare tools在新版的Ubuntu上总是安装不上，其实解决方法也很简单。

`apt-get install linux-headers-virtual open-vm-dkms open-vm-tools（图形界面）`

或

`apt-get install --no-install-recommends linux-headers-virtual open-vm-dkms open-vm-tools（命令行）`

## 2.如何以管理员身份操作Gnome的资源管理器--nautilus

`apt-get install nautilus-gksu`

## 3.

没有声音的话，使用alsamixer来配置一下。

## 4.

最近下载安装了ubuntu 12.04 LTS。由于它使用了Unity桌面，因此之前的一些GNOME桌面工具不再可用。

为了显示实时网速，我找到了indicator-netspeed这个小工具，其安装过程如下：

* install the dependencies:

`sudo apt-get install build-essential libgtop2-dev libgtk-3-dev libappindicator3-dev git-core`

 * create a folder for git_project and download the code.

`mkdir git_project && cd git_project`
`git clone git://github.com/mgedmin/indicator-netspeed.git`

* do make

`make`

* launch the indicator:

`./indicator-netspeed`

附带的说一下，刚开始的时候，我给这个程序添加了一个桌面快捷方式。但是每次开机还要按一下快捷方式，着实不方便。后来发现在选择“关机”的那个菜单上方还有个叫做“启动应用程序”的东东，之前看名字还以为是Windows下Run的替代品，结果实际上是桌面的开机启动程序。。。

另，修改~/.profile之类的文件是不行的，因为那是在进入桌面之前运行的。由于桌面还没有ready，好多桌面程序都是跑不起来的。

在Ubuntu 14.04中“启动应用程序”找不着了，但是实际的功能实现机制还是没有变——在~/.config/autostart下创建desktop文件。

## 5.小杂谈

最近重新开始研究Android和Linux，由于已经快2年没有弄过了。因此之前下载的linux发行版也不打算再继续用了，省得到了Android开发的时候还要更新一大堆的包。

看了一下Google官网上对于Android source的编译条件，再结合2年前编译Android 2.2（Froyo）的经验，在虚拟机上跑Linux的方案首先被排除。开玩笑，16G的RAM/SAWP的开销，哪是VM能搞得定的。就连Froyo在虚拟机上都编不过去，何况是4.3（Jelly Bean）了。

2年前编译Froyo，用的是Wubi安装的Ubuntu 10.04。这次看了一下Jelly Bean的编译条件，100GB+的空间。忽然觉得做软件这么多年，还从来没有在PC上认认真真的安装个双系统用用，实在是职业生涯的一个污点。于是在下载了Ubuntu 12.04之后，老老实实的在网上搜索起如何硬盘安装的办法来。

之所以选择硬盘安装，其实实在是觉得刻张盘太麻烦了。如今就连卖本本的厂商都不提供Recovery CD，而改用Recovery分区来恢复系统。想来硬盘安装Ubuntu也早无技术难度可言了。

下面针对最近几天的安装实践，做一个总结性的概括：

1）刚开始的时候，由于不熟悉Ubuntu 12.04的安装步骤，选择了错误的选项，发现之后中断系统安装。重启，结果不但Ubuntu没有安装好，就连Windows也进不去。这时由于之前没有刻录Live CD的原因，只好使用Recovery分区恢复Windows系统。

2）我的本本是ASUS的，在开机时候按住F9，即可启动一键恢复功能。由于这个功能也是第一次使用，因此当选择将Windows恢复到第一个分区，并在恢复过程中出错的时候，我选择了重启。然后选择将Windows恢复到一个硬盘。。。结果除了Recovery分区之外的所有分区都被删除了。保存在电脑中的数据也全部丢失。看来装系统始终是件危险的工作。安装之前一定要将重要数据备份到移动硬盘上。

3）ASUS的一键恢复功能，会重启机器若干次，且每次的画面高度雷同，因此很让人觉得是不是机器陷入了有问题的死循环之中。在反复折腾几次之后，我也泄了气，任由它先无限重启下去。后来大概是重启到第4次之后，我发现桌面的分辨率有改变，然后才意识到这些重启估计是正常现象，并不是死循环。

4）Ubuntu的虚拟内存使用的是交换分区的方式，而不是Windows的交换文件。因此要单独划一个分区用于虚拟内存的数据交换。

5）Win7的压缩卷功能对于系统分区不好使，由于有不可移动的数据存在，系统盘最小也要460G（硬盘总共1T），这实在是太大了。可以使用Acronis Disk Director Suite调整系统分区的大小。但是需要注意的是，Windows下的分区工具对分区数量的调整，会影响到GRUB的执行，需要通过grub shell的命令切换到Linux下，然后执行grub update，方可恢复正常。而Linux下的分区工具（如GParted）就没有这个问题。

6）GParted在调整分区大小，主要是分区首地址右移方面，执行效率非常慢。Acronis Disk Director Suite没试过，没准会好些。

7）分区的话题说完了，继续谈谈对Linux启动过程的心得。会提到一些名词，但是不会展开来说。首先是BIOS和UEFI。它们决定从哪个存储介质启动。

8）MBR的结构。MBR决定了一个介质最多只有3个主分区和1个扩展分区。一个扩展分区可包含若干个逻辑分区。

9）GRUB和LILO。

10）initrd和vmlinuz。initrd包括image-initrd和cpio-initrd。zImage和bzImage的区别和作用。

11）在逛Ubuntu软件中心的时候，发现了Batttle of Wesnoth这个开源的回合制战旗游戏。试着玩了一下，感觉蛮不错的。正好这个项目在Source Forge上使用Git管理源代码。考虑到Android Source也是用Git管理的，于是就用Git下载了Batttle of Wesnoth的source来熟悉一下Git的用法。

12）在linux下有个叫做gitk的Git GUI工具。

## 6.虚拟机和宿主机的文件共享——FTP方式

最近打算在win7的系统上，搭建ubuntu 14.04的虚拟机。由于使用的vmware的版本比较老，只有8.0，其中自带的VM Tool无法在最新的内核下正常工作（有编译错误）。因此在不得以的情况下，只好使用FTP的方式，实现虚拟机和宿主机之间的文件共享。

最初，打算在ubuntu的虚拟机上搭建FTP服务器，而在windows下用Filezilla客户端访问并共享。所用的参考文献如下:

[http://blog.chinaunix.net/uid-11187-id-3026834.html](http://blog.chinaunix.net/uid-11187-id-3026834.html)

但是最后并没有成功，现将主要的关键点罗列如下：

1）ubuntu使用vsftpd作为FTP服务器。搭建之后，在虚拟机中可以正常访问，但宿主机不行。

2）按照文献中的方法，做好Host和Guest的21端口的映射。这时，FTP登录成功，但LIST列出目录不成功。

3）反复尝试各种设置，包括FTP主动、被动模式，虚拟机端口映射等，但是始终不能正常访问FTP。

最后，比较了一下虚拟机和真实机器在组网上的差异后，我忽然意识到虚拟机FTP不能正常访问的原因，应该是由于虚拟机是在一个虚拟的内网之中。默认情况下，外网机器是无法访问虚拟机的，而虚拟机则可以正常访问外网。因此，反过来，我在win7上用IIS搭建FTP服务，然后在ubuntu虚拟机上用Filezilla访问FTP。这下终于成功了。

## 7.GIMP

在linux下奋斗已经快半个月了，今天处理公务，需要处理一张图片，于是想到了GIMP。大名鼎鼎的linux两大GUI库之一的GTK就是在GIMP开发的过程中做出来的。GIMP功能很强大，不过它的使用方法和windows自带的画笔还是有差异的。

1）画直线：

先在直线的起点单击， 按住shift键然后点击直线的 终点位置，这样，你就可以获得一条很直的线了。

2）截屏

文件->创建->屏幕截图

## 8.Ubuntu使用小技巧

安装 7zip：

`sudo apt-get install p7zip`

安装 rar:

`sudo apt-get install rar unrar`

rar比较奇怪，压缩和解压是使用不同的包，这点和7zip是不一样的。

`cd - //bash`中回到上一次所在的路径的命令。当需要在两个相隔较远的路径下，相互切换的时候，可以使用该命令。

常按Win键，会弹出Unity所用的键盘快捷键。

## 10.ape文件的处理

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

## 11.向devhelp添加新书

1）最好的办法是在安装开发环境的包的时候，安装包自动给你把书装好。例如，我最近研究GTK3，在安装相关包的时候，GObject之类的书就已经安装好了。

2）除此之外，有一些项目的源码中也有doc目录，如果在里面找到以devhelp（或者devhelp2）为扩展名的文件的话，那么说明该项目的帮助文件支持devhelp查看。这时可将包含devhelp（或者devhelp2）为扩展名的文件的那个文件夹复制到devhelp专门放书的目录下，并将文件夹的名字改成和devhelp（或者devhelp2）为扩展名的文件的主文件名一致即可。

devhelp每个版本放书的目录都不尽相同，一般如果安装了gtk的话，可以找找gtk-doc文件夹的位置，然后把书放到gtk-doc下。

## 12.Logo语言的安装

Logo语言是我学习计算机时的启蒙语言。那是初一、初二的时候，当时的电脑还是运行Dos的386系统，我们在原始的命令行下，津津有味的编着代码。那时候电脑可是稀罕物，为了节约上机时间，程序都是事先写到小本本上的，我至今还保留着当时的作业本。

话归正传，说说Linux下Logo语言的安装。

1）到Berkeley的官网下载Logo语言的源代码

http://www.cs.berkeley.edu/~bh/logo.html

2）按照源代码包里面readme的提示，编译之。提示缺少libbsd和libtermcap。

3）`sudo apt-get install libbsd-dev libncurses5-dev`

重新编译即可。

再一次看到熟悉的命令行界面和海龟了:)

## 13.Unity侧边栏快速启动的研究

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

