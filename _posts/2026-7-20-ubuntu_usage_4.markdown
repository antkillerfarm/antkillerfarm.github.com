---
layout: post
title:  Ubuntu使用技巧（四）
category: linux 
---

* toc
{:toc}

# 向devhelp添加新书

1）最好的办法是在安装开发环境的包的时候，安装包自动给你把书装好。例如，我最近研究GTK3，在安装相关包的时候，GObject之类的书就已经安装好了。

2）除此之外，有一些项目的源码中也有doc目录，如果在里面找到以devhelp（或者devhelp2）为扩展名的文件的话，那么说明该项目的帮助文件支持devhelp查看。这时可将包含devhelp（或者devhelp2）为扩展名的文件的那个文件夹复制到devhelp专门放书的目录下，并将文件夹的名字改成和devhelp（或者devhelp2）为扩展名的文件的主文件名一致即可。

devhelp每个版本放书的目录都不尽相同，一般如果安装了gtk的话，可以找找gtk-doc文件夹的位置，然后把书放到gtk-doc下。

`sudo apt install libgtk-3-doc`

# 打印机

一般使用sane做为扫描仪后端：

`sudo apt-get install sane sane-utils xsane`

# Virtual MIDI Piano Keyboard

VMPK是一款MIDI生成工具软件，也就是俗称的“虚拟电子琴”软件。但它本身只生成MIDI输出，需要配合使用MIDI后处理软件，才能发声。常见的MIDI后处理软件有Qsynth、TiMidity。

# 电源方案

2023.5

最近，儿子乱动我的笔记本电脑，惹出了事端。

具体现象：

开机风扇狂转，软件一测，转速达到5600转，几乎满速。

最开始怀疑，是否是摇松了CPU，导致风扇接触出了问题。但是软件看温度也就70左右，远没有到警戒线（这个一般在90度左右，100+会导致CPU直接关机保护）

最后一查才知道，系统有个叫做电源方案的东西，无论Win，还是Linux都有。

如果选择性能优先，那CPU自然怎么快，怎么来。

如果选择平衡或者节能的话，CPU会自己降频，避免关机保护，或者如本例中的快速升温——在点击应用瞬间，温度从40+飙升到75+。

一般来说，只要峰值温度控制得当，积热其实并不是啥问题。CPU不可能随时都是满负荷，稍微歇一下，积热就散走了。

# 无线网卡

2023.8

最近，无限网卡突然失灵，上网一查才发现，华硕这几代笔记本由于布局问题，固态硬盘的热量会烧坏附近的网卡芯片。

幸好手里还有一个闲置的USB无线网卡。但是插上网卡，驱动不识别。

经查，该型号网卡的驱动早已集成进内核主线，但是默认并未打开。

解决方法：

`sudo modprobe mt7601u`

当然这个还是不方便，需要每次手动打开。

https://qastack.cn/unix/71064/systemd-automate-modprobe-command-at-boot-time

systemd：在启动时自动执行modprobe命令

# Ubuntu字体相关

最近gitk中文显示不正常，明明系统的字体是很多的，但可以设置的却甚少。后来发现这里能够设置的并非系统字体，而只是X11字体。

列出字体：

`xlsfonts`

找到系统字体文件夹，生成`fonts.dir`文件：

`sudo mkfontscale -o fonts.dir .`

加载`fonts.dir`文件：

`xset +fp /usr/share/fonts/X11/misc`

文泉驿字体是最知名的中文免费字体：

`sudo apt install ttf-wqy-microhei ttf-wqy-zenhei`

# X Desktop Group

设置`XDG_CACHE_HOME`可以将cache文件移动到较快的介质如SSD中，从而加速运行。或者移动到较大的分区，以腾出系统空间。

https://www.cnblogs.com/zqb-all/p/10666474.html

Linux实用命令之xdg-open

https://blog.csdn.net/u014025444/article/details/94029895

linux的XDG基本目录规范

# 便签软件

主要有两类便签软件：

1.支持超链接的便签。典型的有Gnote和Tomboy，这两个软件都有内容检索的功能。

2.桌面随意贴。典型的有Indicator Stickynotes和Knotes。后者有内容检索的功能，而前者没有。

# xournalpp

xournalpp是一个手写笔记软件。

官网：

https://xournalpp.github.io/

安装：

`sudo snap install xournalpp`

# Excalidraw

Excalidraw是一个在线的手写笔记软件。

官网：

https://excalidraw.com/

# Wine

https://www.ubuntukylin.com/news/1682-cn.html

这是优麒麟社区提供的Wine版本，基本涵盖了腾讯系主要的几款通信软件

---

删除软件快捷方式：

1.首先将`~/.local/share/applications/`下和`~/.local/share/applications/wine/Programs/`下相关文件或目录删除掉。

2.然后再删除`~/.config/menus/applications-merged/`里面相关的文件。

---

腾讯会议目前已经有了官方的Linux版本。

检测到窗口系统采用wayland协议 腾讯会议暂不兼容 程序即将退出：

```bash
sudo vim /etc/gdm3/custom.conf
#WaylandEnable=false #uncomment this line
sudo service gdm3 restart
```

# Ubuntu 24.04使用手记

内核：6.8

---

发布的第一时间，我想使用apt更新，然而这种更新通常会安排到当年10月进行。

所以直到2024.8换了新电脑之后，我才有机会体验新版本。

本次不想再捣鼓硬盘安装了，直接U盘搞定安装。

刻录ISO，早些年一般使用Ultra ISO或者Power ISO，本来想着十几年过去了，win应该自带了吧，毕竟隔壁的ubuntu已经这么做了。结果还是没有。。。

好在，现在有了开源软件Rufus：

https://github.com/pbatard/rufus

# Ubuntu 26.04使用手记

内核：7.0

---

本作我没有第一时间尝鲜，因为刚发布的时候，大家都在更新，网速一定很卡。所以直到26.7才开始更新。

总共有三台PC进行了更新，其中一台更新一半出了问题，重启之后，gnome桌面进不去了，幸好命令行和网络还是正常的。

执行以下命令，问题解决：

`sudo apt install --reinstall gnome-shell`
