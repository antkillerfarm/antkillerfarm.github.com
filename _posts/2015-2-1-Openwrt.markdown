---
layout: post
title:  OpenWrt
category: technology 
---

# OpenWrt编译

1.下载代码

`git clone git://git.openwrt.org/openwrt.git`

2.安装必要的包

`sudo apt-get install libtool autoconf automake gcc-multilib bison screen gcc g++ binutils patch bzip2 flex make gettext unzip libc6 git-core git build-essential libncurses5-dev zlib1g-dev gawk quilt asciidoc libz-dev`

包的内容根据OpenWRT和Ubuntu的版本的不同，而略有差异。比如最新的版本可能还需要libssl-dev包。不过这个比较简单，看出错信息就知道还需要什么包了。

3.更新扩展，安装扩展

`./scripts/feeds update -a`

`./scripts/feeds install -a`

4.配置编译选项

`make menuconfig`

这个配置部分和linux内核的配置极为相似，只不过配置的内容相比内核多了很多。

5.编译

`make`

这一步需要的时间很长，除了编译的内容很多之外，还需要下载很多包的源代码。下载的包放在dl文件夹下。

6.编译结果

编译的结果放在bin文件夹下。

wr841nd-v5-squashfs-sysupgrade.bin

wr841nd-v5-squashfs-factory.bin

都生成了，如果从原厂直接升级到openwrt用factory.bin，open升级的话用sysupgrade.bin

7.重新编译固件

`make clean`

这个命令会保留配置文件.config 和dl目录里下载的文件

如果想要全部清除，就用下面的命令：

`make distclean`

# OpenWrt in VirtualBox

## 0.软件准备

需要安装以下软件：

1）VirtualBox

2）Windows平台：Putty、WinSCP

3）Ubuntu平台：Putty、Gigolo

## 1.镜像文件的获得

1）下载源码自己编译。编译的方法见上节。编译的时候，将Target选为X86即可。此外还可以选择生成文件的类型。VirtualBox虚拟硬盘文件的后缀名是vdi。

2）在官网直接下载。

http://downloads.openwrt.org/barrier_breaker/14.07/x86/generic/openwrt-x86-generic-combined-ext4.img.gz

这里解释一下该文件夹下各个文件的区别：

openwrt-x86-generic-combined-ext4.img.gz

rootfs工作区存储格式为ext4

openwrt-x86-generic-combined-jffs2-128k.img

jffs2可以修改，也就是可以自行更换（删除）rootfs的配置文件，而不需要重新刷固件。

openwrt-x86-generic-combined-squashfs.img

squashfs是个只读的文件系统，相当于win的ghost，使用中配置错误，可直接恢复默认。

openwrt-x86-generic-rootfs-ext4.img.gz

rootfs的镜像，不带引导，可自行定义用grub或者syslinux来引导，存储区为ext4。

为了更清楚的说明这个问题，可以参考以下文章：

http://wiki.openwrt.org/doc/techref/header

从这里可以看出一个完整的镜像文件至少要包含三个部分

* Loader

* Kernel

* RootFS

3）如果镜像文件是img格式的，需要转换成vdi方可。方法如下：

`VBoxManage convertfromraw --format VDI openwrt.img openwrt.vdi`

## 2.OpenWrt官网在Firefox下无法打开的问题

只需要在about:config里修改

`intl.accept_languages`
 
的值为

`zh-CN,en-US,zh`

这个是FF的优先语言设置的BUG。在FF中的值是zh-cn en-us 等等，而规范的值应该是zh-CN en-US

## 3.在VirtualBox中运行OpenWrt

可参考以下文章：

http://wiki.openwrt.org/doc/howto/virtualbox

1--基本系统

1）选择OS类型：Linux->2.6/3.X

2）硬盘设置从SATA改为IDE。

3）启用串口，但要关掉连接。

4）启动虚拟机。等屏幕不再输出字符时，按下“Enter”键。会出现OpenWrt的欢迎界面及命令行。这说明OpenWrt已经跑起来了。

5）不是所有的机器都需要2）和3）的设置。如果OpenWrt能跑起来的话，2）和3）也可省略。

2--网络配置

从原理上来说，虚拟机需要设置两个网卡。一个是LAN，用于OpenWrt和PC的交互。另一个是WAN，用于OpenWrt的连接上网。

一般来说LAN和WAN位于不同的网段。而OpenWrt的默认配置只有LAN，没有WAN，而且LAN的IP地址固定为192.168.1.1。这通常和家里面使用的路由器的IP地址相冲突。因此除了修改虚拟机的网络设置之外，还需要修改OpenWrt的网络配置。

1）虚拟机网络配置

环境一:Windows平台+路由器（WAN和LAN不在一个网段）

LAN: eth0 Host-only

WAN: eth1 NAT

环境二:Linux平台+拨号无线路由（WAN和LAN在一个网段）

LAN: eth0 Internal Network

WAN: eth1 Bridge（Host有两个网卡：eth0和wlan0。这一步的时候,界面名称要选择wlan0的网卡，也就是和Internet相连的那个网卡）

2）关于网卡芯片类型

官方img中，PCnet-FAST和Intel Pro 1000的驱动都有，因此VirtualBox选择任何一个芯片类型都可以。

但是从源代码编译得到的img，就要看编译时的选项了。当前默认的是Intel Pro 1000。

网上的文章提到要修改成PCnet-FAST，我怀疑是由于早先默认的是PCnet-FAST的缘故。

3）OpenWrt网络配置

修改文件/etc/config/network

{% highlight bash %}
config interface 'lan'
        option ifname 'eth0'
        option proto 'dhcp'

config interface 'wan'
        option ifname 'eth1'
        option proto 'dhcp'
{% endhighlight %}

4）验证联网是否成功

WAN：使用ping命令，例如`ping www.baidu.com`

LAN：见下一节的登录过程。

如果不成功的话，可用ifconfig查看网络状态。看看eth0和eth1的IP地址是否是LAN和WAN所在的网段。

4--登录

0）登录的IP地址

登录的IP地址，使用LAN的IP地址。可用ifconfig查看获得。

1）首次登录

使用telnet登录成功后，使用`passwd`命令设置密码。

2）命令行登录

使用支持ssh的客户端（如putty）登录。

3）图形界面登录

使用浏览器，输入`http://192.168.1.1`即可。（实际使用中需用LAN的IP地址替换之）

#LuCI

LuCI是OpenWrt管理配置界面使用的接口。

从文件上，它由两个部分组成：

1）/www

index.html--网站主页。

2）/usr/lib/lua

lua脚本的根目录。

主流程：

1./www/index.html

2./www/cgi-bin/luci

3.luci.sgi.cgi.run（即/usr/lib/lua/luci/sgi/cgi.lua里的run函数，以下只写简称。）

4.luci.dispatcher.httpdispatch

可参考一下文献:

http://www.cnblogs.com/zmkeil/archive/2013/05/14/3078774.html

这篇文章对LuCI的流程有详细的描述。

http://wiki.openwrt.org/doc/uci

这个是官方的文档。

参考文献

http://www.cnblogs.com/zmkeil/archive/2013/04/17/3027385.html

这篇文章的内容和我写的差不多，可惜没有早看到。。。多走了弯路。

# 编译OpenWrt模块--Hello World

1）SDK

编译OpenWrt模块，需要用和img相一致的SDK。

在用源代码生成img的时候，将SDK也选上。这样在生成的img的路径下，就有一个名字中有SDK字样的压缩包，解压即可得到SDK。

2）代码

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/hiOpenWRT

从上面的地址下载代码，然后将hiOpenWRT文件夹，放到package文件夹下。

3）编译

将路径设为SDK的根目录，然后运行以下命令：

make package/hiOpenWRT/compile V=s

4）编译结果

在bin/x86_64/packages/base下可以找到hiOpenWRT_1_x86_64.ipk。

参考文献：

http://blog.chinaunix.net/uid-29418452-id-4071751.html

# 编译OpenWrt模块--进阶篇

1）参考文献

http://www.cnblogs.com/sammei/p/3968916.html

这篇文章描述了OpenWrt makefile的框架

http://www.ccs.neu.edu/home/noubir/Courses/CS6710/S12/material/OpenWrt_Dev_Tutorial.pdf

这是美国东北大学（不是中国东北大学）的学生写的开发入门。

2）编译内核模块

编译内核模块的大体步骤与编译应用模块并无太大差异。其主要不同在于内核模块需要使用linux内核的那一套makefile机制，而应用模块的编译就简单的多了，简单的配一下gcc的参数就行了。

这里我偷了个懒，直接将OpenWrt源代码中的package/kernel/gpio-button-hotplug复制到SDK的package文件夹下，岂料编译之后什么也没有生成。

网上查了一下也没有找到什么办法，有的网页甚至说SDK不能用于编译内核模块。这一点让我很疑惑，因为普通的linux内核模块只需要linux-header就可以编译了，并不需要linux源代码。

最后我尝试运行了一下`make menuconfig`，终于找到了问题的症结。不同于编译源代码时候的选项，这里的选项很少，碰巧找到了gpio-button-hotplug，然后选中该项，再次编译。这一下终于在bin/x86_64/packages/base下找到了需要的ipk文件。
