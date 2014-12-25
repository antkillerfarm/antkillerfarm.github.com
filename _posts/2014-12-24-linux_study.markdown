---
layout: post
title:  linux学习心得
category: technology 
---

## 1.动态链接

这两天实践了一下怎样在linux下创建动态链接。感觉网上的资料虽然翔实，但仍然有疏漏之处。

1）g++和gcc的区别

本来只想给链接的，以显示这不是我的原创。但是现在的链接失效的也太快了。。。囧

只好拿华丽的分隔符来表示引用的内容。

****
误区一:gcc只能编译c代码,g++只能编译c++代码

两者都可以，但是请注意：

1.后缀为.c的，gcc把它当作是C程序，而g++当作是c++程序；后缀为.cpp的，两者都会认为是c++程序，注意，虽然c++是c的超集，但是两者对语法的要求是有区别的。C++的语法规则更加严谨一些。

2.编译阶段，g++会调用gcc，对于c++代码，两者是等价的，但是因为gcc命令不能自动和C＋＋程序使用的库联接，所以通常用g++来完成链接，为了统一起见，干脆编译/链接统统用g++了，这就给人一种错觉，好像cpp程序只能用g++似的。
 
误区二:gcc不会定义__cplusplus宏，而g++会

实际上，这个宏只是标志着编译器将会把代码按C还是C++语法来解释，如上所述，如果后缀为.c，并且采用gcc编译器，则该宏就是未定义的，否则，就是已定义。
 
误区三:编译只能用gcc，链接只能用g++

严格来说，这句话不算错误，但是它混淆了概念，应该这样说：编译可以用gcc/g++，而链接可以用g++或者gcc -lstdc++。因为gcc命令不能自动和C＋＋程序使用的库联接，所以通常使用g++来完成联接。但在编译阶段，g++会自动调用gcc，二者等价。
****

因此，某些时候编不过去，可以试试换换cc的值。

2）gcc4.1.1下似乎对类型检查严了一些，dlsym返回的void*类型不能转换为相应的函数指针类型，需要强制转换。某些网上的例子在这里编不过去。

3）显式调用时,要注意动态库函数的声明,可能要加`extern "C"`才能正常执行。（显式调用是运行时加载，所以编译能过，执行却不对了。）可以用nm命令看看链接库的符号表，以确定问题所在。

## 2.驱动开发

推荐读物《Beginning Linux Programming》，该书第3版已有中译本。

但第3版中的例子在2.6以后的新内核中不能编译。经研究发现，由于新版内核采用KBuild系统编译内核，所以驱动也必须使用KBuild系统编译。

驱动开发的头文件可以在/usr/src下找到。

## 3.GUI设计工具

GTK+的程序可以使用Glade来设计界面，它会生成一个脚本，运行该脚本，并make就行了。

QT的稍微麻烦一些。

1）首先打开QT Designer（新建时，不能使用KDE Designer，但修改建好后的工程可以，不知道是BUG，还是怎么回事）新建一个窗体，然后新建一个main.cpp，并将刚才生成的窗体选为主窗体。

2）打开KDev C/C++，选择导入工程，选择QMake Based导入，然后即可在IDE中编译并运行。

上面写的两个都是官方的设计器，而WxWidgets没有官方的设计器，但有很多第三方的设计器，我使用的是免费且开源的wxFormBuilder。用它可生成XRC文件，而WxWidgets中有使用XRC文件的接口。

## 4.编程所用命令简介

cc：C/C++编译器

as：GNU汇编编译器

ld：链接器

as86、ld86：8086汇编编译器和链接器

linux文件分割用split,合并用cat。最近下了一本采用split分割的书，但是我没有Linux环境，于是在windows的命令行下用type、>、>>合并了文件。

## 5.printf和wprintf混用的问题

在linux中不可混用printf和wprintf，如果混用的话，则**后使用**的函数没有输出。

例如

{% highlight c %}
printf("a\n");
wprintf(L"b\n");
{% endhighlight %}

输出为：

a

而

{% highlight c %}
wprintf(L"b\n");
printf("a\n");
{% endhighlight %}

输出为：

b

关于这个问题的讨论见

[http://bytes.com/groups/c/852681-wprintf-conflicts-printf-glibc-bug](http://bytes.com/groups/c/852681-wprintf-conflicts-printf-glibc-bug)

解决方法统一使用一种函数

例如：

{% highlight c %}
wprintf(L"%s","a\n");
wprintf(L"b\n");
{% endhighlight %}

或

{% highlight c %}
printf("a\n");
printf("%ls",L"b\n");
{% endhighlight %}

## 5.关于SIGPIPE导致的程序退出
[http://www.cppblog.com/elva/archive/2008/09/10/61544.html](http://www.cppblog.com/elva/archive/2008/09/10/61544.html)

## 6.使用yum

在RHEL中，可以使用yum从网上下载相应的组件，但需要RHN号，所以我现在换用了CentOS。当你需要使用yum的时候，如果yum找不到相应的组件时，可以在组件名之前加lib，或者在之后加-dev或-devel。

##7.源码包编译4步曲

1)autogen.sh

2)configure

3)make

4)su -c "make install"

其中一二两步有时只要一个就够了，如果源码包中这两个都有的话，先运行1)

##8.永不返回的函数（never return function）

了解C语言的人都知道一个函数的最后一个语句通常是return语句。编译器在处理返回语句时，除了将返回值保存起来之外，最重要的任务就是清理堆栈。具体来说，就是将参数以及局部变量从堆栈中弹出。然后再从堆栈中得到调用函数时的PC寄存器的值，并将其加一个指令的长度，从而得到下一条指令的地址。再将这个地址放入PC寄存器中，完成整个返回流程，接着程序就会继续执行下去了。

对于返回值是void类型，也就是无返回值的函数，保存返回值是没有意义的，但它仍然会执行清理堆栈的操作。

以上提到的这些，基本上适用于99.99%的场合。但凡事无绝对，在一些特殊的地方，例如操作系统内核中的某些函数，就不见得符合上边所说的这些。永不返回的函数就是其中之一。

在Linux源代码中，一个永不返回的函数通常拥有一个类似如下函数的声明：

` NORET_TYPE void do_exit(long code)`

考虑到NORET_TYPE的定义：

`#define NORET_TYPE    /**/`

因此，NORET_TYPE在这里仅仅起到方便阅读代码的作用，而并没有什么其他的特殊作用。

看到do_exit函数，可能熟悉Linux内核的朋友已经猜出永不返回的函数和普通函数有什么区别了。没错，do_exit函数是销毁进程的最后一步。由于进程已经销毁，从进程堆栈中获得下一条指令的地址就显得没有什么意义了。do_exit函数会调用schedule函数进行进程切换，从另一个进程的堆栈中获得相关寄存器的值，并恢复那个进程的执行。因此do_exit函数在正常情况下是不会返回的，一个调用了do_exit函数的函数，其位于do_exit函数之后的语句是不会执行到的。因此那个函数也成为了永不返回的函数。

## 9.在Ubuntu上安装VMWare tools
VMWare自带的VMWare tools在新版的Ubuntu上总是安装不上，其实解决方法也很简单。

`apt-get install linux-headers-virtual open-vm-dkms open-vm-tools（图形界面）`

或

`apt-get install --no-install-recommends linux-headers-virtual open-vm-dkms open-vm-tools（命令行）`

## 10.如何以管理员身份操作Gnome的资源管理器--nautilus

`apt-get install nautilus-gksu`

## 11.没有声音的话，使用alsamixer来配置一下。

## 12.最近下载安装了ubuntu 12.04 LTS。由于它使用了Unity桌面，因此之前的一些GNOME桌面工具不再可用。

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

## 13.小杂谈

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

## 14.虚拟机和宿主机的文件共享——FTP方式

最近打算在win7的系统上，搭建ubuntu 14.04的虚拟机。由于使用的vmware的版本比较老，只有8.0，其中自带的VM Tool无法在最新的内核下正常工作（有编译错误）。因此在不得以的情况下，只好使用FTP的方式，实现虚拟机和宿主机之间的文件共享。

最初，打算在ubuntu的虚拟机上搭建FTP服务器，而在windows下用Filezilla客户端访问并共享。所用的参考文献如下:

[http://blog.chinaunix.net/uid-11187-id-3026834.html](http://blog.chinaunix.net/uid-11187-id-3026834.html)

但是最后并没有成功，现将主要的关键点罗列如下：

1）ubuntu使用vsftpd作为FTP服务器。搭建之后，在虚拟机中可以正常访问，但宿主机不行。

2）按照文献中的方法，做好Host和Guest的21端口的映射。这时，FTP登录成功，但LIST列出目录不成功。

3）反复尝试各种设置，包括FTP主动、被动模式，虚拟机端口映射等，但是始终不能正常访问FTP。

最后，比较了一下虚拟机和真实机器在组网上的差异后，我忽然意识到虚拟机FTP不能正常访问的原因，应该是由于虚拟机是在一个虚拟的内网之中。默认情况下，外网机器是无法访问虚拟机的，而虚拟机则可以正常访问外网。因此，反过来，我在win7上用IIS搭建FTP服务，然后在ubuntu虚拟机上用Filezilla访问FTP。这下终于成功了。

## 15.使用cscope+emacs在Ubuntu 12.04下的注意事项

1）除了安装cscope还需要安装cscope-el，不然找不到xcscope.el

`sudo apt-get install cscope-el cscope`

2）xcscope.el的默认路径：/usr/share/emacs/site-lisp

3）进入源代码的根目录，然后

`cscope-indexer -r`

即可建立索引文件。

4)添加cscope查询的历史功能

[http://sourceforge.net/p/cscope/patches/65/](http://sourceforge.net/p/cscope/patches/65/)

## 16.GIMP

在linux下奋斗已经快半个月了，今天处理公务，需要处理一张图片，于是想到了GIMP。大名鼎鼎的linux两大GUI库之一的GTK就是在GIMP开发的过程中做出来的。GIMP功能很强大，不过它的使用方法和windows自带的画笔还是有差异的。

1）画直线：

先在直线的起点单击， 按住shift键然后点击直线的 终点位置，这样，你就可以获得一条很直的线了。

2）截屏

文件->创建->屏幕截图

## 17.Ubuntu使用小技巧

安装 7zip：

`sudo apt-get install p7zip`

安装 rar:

`sudo apt-get install rar unrar`

rar比较奇怪，压缩和解压是使用不同的包，这点和7zip是不一样的。

`cd - //bash`中回到上一次所在的路径的命令。当需要在两个相隔较远的路径下，相互切换的时候，可以使用该命令。

常按Win键，会弹出Unity所用的键盘快捷键。

## 18.
使用apt-get获取软件虽然方便，但是从ubuntu的源获得的软件包和直接使用源码编译安装的包相比，包中的各个文件被分散在好多个文件夹中，查找起来很不方便。这时可以到http://packages.ubuntu.com/，去查找软件包里的文件清单，以弄清楚XX软件官网上所说的YY文件在ubuntu中到底放在哪里。

## 19.使用github

自从最近google code日益难以访问以来，我就一直在思考着替代的方案。然后在大徐的blog的指引之下，找到了github。

应该说使用git和svn相比，在现在的网络条件下，还是有不少优势的。我托管代码的目的，只是给自己的blog提供一个代码链接的地方而已，基本无意使用这个和他人协作。由于git的版本库是在本地的，即使github由于某种原因倒掉了，我也可以很方便的换一个替代品。

## 20.svn命令行使用的注意事项

一直以来都是在Windows下使用TortoiseSVN客户端来操作svn。现在到了linux环境下，由于找了一圈貌似也没有什么很给力的GUI客户端。所以只有对svn的命令行使用做一些研究了。

 好在之前一直使用英文版的TortoiseSVN，因此在使用svn help后，基本操作方面倒是没有遇到什么大的问题。只有commit的时候，除了commit之外，还要update一下，然后当前目录的svn状态才会切换到提交了新版本之后的状态。

## 21.ape文件的处理

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

## 22.参与GTK+开发的一段小经历

## 23.

这几天研究Android源代码，发现了一个牛人——Robert Love。最初注意他是因为他的姓氏挺有意思的。没想到过了几十分钟，就在另一个嵌入式操作系统方面的课件中看到了他的名字。他是Preempt Linux的作者，也是Linux Kernel Development一书的作者，同时还是Android的日志系统的作者。

最关键的是，他是1981年出生的人，比我也就大一点儿。而Preempt Linux是他2001年的作品。那个时候我貌似连C语言都没怎么弄利索。。。

至于国内的牛人，有个叫李云的诺西工程师，写了本嵌入式方面的书感觉还不错，不是随便复制粘贴的东西，可以看看。不过一者，李云比我大好几岁，二者他水平虽然比我高，但还没到仰视的地步。所以终究比不了那些老外的牛人啊。。。

## 24.向devhelp添加新书

1）最好的办法是在安装开发环境的包的时候，安装包自动给你把书装好。例如，我最近研究GTK3，在安装相关包的时候，GObject之类的书就已经安装好了。

2）除此之外，有一些项目的源码中也有doc目录，如果在里面找到以devhelp（或者devhelp2）为扩展名的文件的话，那么说明该项目的帮助文件支持devhelp查看。这时可将包含devhelp（或者devhelp2）为扩展名的文件的那个文件夹复制到devhelp专门放书的目录下，并将文件夹的名字改成和devhelp（或者devhelp2）为扩展名的文件的主文件名一致即可。

devhelp每个版本放书的目录都不尽相同，一般如果安装了gtk的话，可以找找gtk-doc文件夹的位置，然后把书放到gtk-doc下。

## 25.Logo语言的安装

Logo语言是我学习计算机时的启蒙语言。那是初一、初二的时候，当时的电脑还是运行Dos的386系统，我们在原始的命令行下，津津有味的编着代码。那时候电脑可是稀罕物，为了节约上机时间，程序都是事先写到小本本上的，我至今还保留着当时的作业本。

话归正传，说说Linux下Logo语言的安装。

1）到Berkeley的官网下载Logo语言的源代码

http://www.cs.berkeley.edu/~bh/logo.html

2）按照源代码包里面readme的提示，编译之。提示缺少libbsd和libtermcap。

3）`sudo apt-get install libbsd-dev libncurses5-dev`

重新编译即可。

再一次看到熟悉的命令行界面和海龟了:)

26.Unity侧边栏快速启动的研究

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

## 27.Linux链表实现

数据结构课本上教链表的时候，一般是这样定义链表的数据结构的：

{% highlight c %}
typedef struct {
    struct Node *next;
    UserData data;
}Node;
{% endhighlight %}

其中，data字段包含了要保存到链表中的数据内容。使用这样的数据结构实现的链表，通用性不好，需要针对不同的UserData类型定义不同的链表类型，尽管所有这些链表的操作都是类似的。当然这样的定义在C++中不是太大的问题，使用模板就可以实现对不同UserData类型的处理，虽然这样做无法避免代码段的膨胀，但是仅就书写使用来说，并没有太大的不方便。

一种改进的办法是将数据结构改为下面的样子：

{% highlight c %}
typedef struct {
    struct Node *next;
    void* data;
}Node;
{% endhighlight %}

用无类型的指针指向需要保存的数据内容，是一个通用性不错的办法。但是C语言本身没有对元数据的支持，一旦指针退化成无类型的指针，再想恢复成原来的数据类型就比较困难了。（元数据就是所有数据类型的基类，例如Java语言的Object类、MFC的CObject类、GTK的GObject结构。虽然元数据本身并不要求包含数据的类型信息，但在上述这些元数据的实现中，都提供了这个功能。）

Linux的做法是：（为了便于理解，进行了一些改写，以忽略与本话题无关的部分）

{% highlight c %}
typedef struct {
    struct Node *next;
}Node;
typedef struct {
    Node *node;
    UserDataActual data;
}UserData;
{% endhighlight %}

这实际上是一种逆向思维，也就是将链表结点中包含用户数据，改为用户数据中包含链表结点。在链表处理时，将node传给链表处理函数。而在引用用户数据时，通过计算node和data的地址偏差，获得data的实际地址。具体的技巧如下：

`UserDataActual* p_data = (UserDataActual*)(((char*)node) - (int)(&(((UserData*)0)->node)) + (int)(&(((UserData*)0)->data)));`

可以看出，这种实现方式对node在UserData中出现的位置也没有什么额外的要求，有很好的灵活性。

## 28.linux源代码编译

1)按照一般的linux教程上的说法，编译的第一步，是配置内核的编译选项。这时有几种方式可以选择:从命令行方式的make config，到基于ncurse库的make menuconfig，再到基于qt的make xconfig和基于GTK+的make gconfig。这让我不得不感叹，即使是内核这样超底层的东西，居然也会用到GUI。Linus也不总是命令行的拥趸。

不过方式虽多，除了make config很不好用之外，其他几个基本上没有什么大的区别。唯一需要注意的是，无论何种方式这一步的目标都是生成.config文件（注意是.config文件，而不是XXX.config文件）。

正是由于有了这一步的存在，定制内核或者说裁剪内核，实际上并没有想象中那么高不可攀。不过编译选项实在是多，好些东西我也不能明白它的真正含义，只能说裁剪过内核，而不敢加上“熟悉”二字。。。

2)接着就是make了，根据机器的给力的程度，这个过程会在十分钟到40分钟之间。最后生成了vmlinux这个内核镜像。

3）最后一步，是安装镜像，这一步由于比较有风险，我还没有实际操作。

## 29. 内核开发心得

* 从最简单的内核模块做起
最近开始研究linux驱动。应该说在驱动领域，我已经有5年以上的工作经验，不算是新手，但是之前的开发要么是在嵌入式内核上，要么就直接是裸机程序，并没有做过真正的linux驱动程序。所以对于这个特定的领域来说，我就是一个新手。
闲话休提，先从最简单的可动态加载的模块说起。

[www.ibm.com/developerworks/cn/linux/l-proc.html](www.ibm.com/developerworks/cn/linux/l-proc.html)

这篇文章是我学习的主要参考资料。资料中已经提到的，在此不再赘述。这里仅作补遗之用。

1）makefile文件的名字必须为Makefile，不然会有编译错误。

2）示例代码虽然能够编译通过，但是insmod之后，会报如下的错误：

simple_lkm: module verification failed: signature and/or  required key missing - tainting kernel

解决的办法是在最开头加上

`#include <linux/init.h>`

3）自动加载LKM

参考文献: [http://edoceo.com/howto/kernel-modules](http://edoceo.com/howto/kernel-modules)

以下为节选:

****
Module Configuration Files

The kernel modules can use two different methods of automatic loading. The first method (modules.conf) is my preferred method, but you can do as you please.

modules.conf - This method load the modules before the rest of the services, I think before your computer chooses which runlevel to use

rc.local - Using this method loads the modules after all other services are started
****

从这里可以看出，LKM的加载是要看时机的，如果需要在服务启动之前加载的话，就修改/etc/modules，否则的话,就修改/etc/rc.local。

PS：/etc/modules由/etc/init/module-init-tools.conf 或 /etc/init/kmod.conf负责执行。

## 30.关于google code

一直以来，在sohu写blog都面临一个很大的问题。代码全贴上的话，太占篇幅，而以附件形式提供代码，又不为blog系统所支持。

之前的解决办法是使用网盘，但是网盘时间一长之后，就不再可用，而我显然也不可能经常去刷新网盘，使得其随时可用。再者，网盘也不是专业的代码托管方式。

好在现在有了google code。

## 31.关于ascii字符集的一些打印控制字符的别名

\b——backspace

\f——pagebreak

\v——vertical tab

## 32.lex&yacc

* 前言

春节期间，空闲时间较多，于是研究了一下lex和yacc的用法。知道lex和yacc，那还是大四学习编译原理那门课时候的事情了。转眼之间，那已经是十年前的事情了。

编译原理在整个大学期间的专业课中，属于难度比较高的课程。而且如果不是计算机专业的话，基本没有可能学到这门课。当时的课程作业是完成一个支持脚本绘图的软件。其难度即使以我现在的眼光来看，也颇不容易。当时只有少数人能够做出来，但基本上是参考教这门课的老师出的一本教辅书来写的。

这个课程作业之所以复杂，主要在于老师要求词法和语法的分析器都必须要自己编码。如果退一步，可以使用lex和yacc的话，就没有那么困难了。当然这也与大学里以传授理论为主的思想有关，我还是相当认同这一点的。

再顺便说一句，lex的作者之一是google的前CEO Eric Schmidt，这是他20岁时，在贝尔实验室的作品。当然，不全是他的功劳。实际上lex和yacc都是贝尔实验室的作品，这从lex效仿yacc的书写风格就能略见一斑。相比而言，yacc的地位和复杂度更为重要些。

* 前置条件

要想研究lex和yacc，除了需要有C语言的基础之外。还需要对正则式和BNF（Backus-Naur Form）有所了解。顺便提一下，John Warner Backus，FORTRAN、ALGOL语言之父，1977年ACM图灵奖得主。他在中学时代居然是个勉强毕业的差生，在大学里换了两次专业，还是一事无成。。。

* 教材

LEX & YACC TUTORIAL by Tom Niemann——这本书比较简练，且附有代码，入门级的极品

Aho, Alfred V., Ravi Sethi and Jeffrey D. Ullman [2006]. Compilers, Prinicples, Techniques and Tools——这本书是编译原理方面的权威作品，堪称编译原理界的TAOCP，不过篇幅太长了。。。

* 心得

lex生成的代码中，最重要的是yylex函数，该函数每匹配一个词，就返回一次。yacc生成的代码中，最重要的是yyparse函数，这个函数调用yylex函数以获得所需要的语法词汇。

lex的词法分析，依据用法的不同，可分为三类：

1）需要匹配识别的词汇。

2）需要过滤的词汇。一般是空白、TAB之类的分隔符。

3）直译的词汇。就是那些lex不处理，也不吃掉，而是直接交给yacc分析的词汇。

这三类词汇必须仔细规划，因为被解析的文本中，一旦出现不在上述三类的任何一类中的词汇时，程序就会报错。

yacc的BNF中一般都要包括类似下面的语句：

{% highlight c %}
stmt_list:
          stmt                  { }
        | stmt_list stmt        { }
        ;
{% endhighlight %}

其中stmt表示单个语句的语法目标，而stmt_list则是一系列语句的集合。

为什么要添加这一句呢？因为yacc在处理被解析的文本时，如果文本不能最终归结为一个单一的语法目标的时候，程序也会报错。

