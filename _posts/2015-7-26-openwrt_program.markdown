---
layout: post
title:  OpenWrt编程篇
category: technology 
---

# 编译OpenWrt模块--Hello World

1）SDK

编译OpenWrt模块，需要用和img相一致的SDK。

在用源代码生成img的时候，将SDK也选上。这样在生成的img的路径下，就有一个名字中有SDK字样的压缩包，解压即可得到SDK。

2）代码

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/hiOpenWRT

从上面的地址下载代码，然后将hiOpenWRT文件夹，放到package文件夹下。

3）编译

将路径设为SDK的根目录，然后运行以下命令：

`make package/hiOpenWRT/compile V=s`

4）编译结果

在bin/x86_64/packages/base下可以找到hiOpenWRT_1_x86_64.ipk。

4）模块安装

`opkg install xyz.ipk`

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

# OpenWrt常见问题

## 1.`undefined reference to '__stack_chk_fail_local'`

网上查了一下，答案是将链接命令由ld，换成gcc即可。

但这只是针对普通代码包的编译而言，OpenWrt由于使用的是交叉编译工具链，单纯的修改源代码中的链接命令，根本起不到丝毫作用。

那么如何修改交叉编译工具链中使用的链接命令呢？

以luasocket为例，在feeds中的makefile中有如下片段：

{% highlight bash %}
define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/ \
		LIBDIR="$(TARGET_LDFLAGS)" \
		CC="$(TARGET_CC) $(TARGET_CFLAGS) $(TARGET_CPPFLAGS) -std=gnu99" \
		LD="$(TARGET_CROSS)ld -shared" \
		all
endef
{% endhighlight %}

将其中的ld，改为gcc即可。

## 2.`undefined reference to '__sync_fetch_and_add_8'`

这个函数和上面的__stack_chk_fail_local函数一样都是编译器隐含函数。它既不同于自己代码实现的函数，也不同于那些显式调用的库函数，通常是针对某个语法规则或编译选项而链接的函数。因此这类错误，实际上并非代码写的不对，而是编译上的配置不对。

__sync_fetch_and_add_8实际上是和CPU原子操作有关的函数，在PC上目前不支持x86指令集，而只支持x86_64指令集。将目标平台改为x86_64即可。

## 3.链接库版本的问题

最近使用某平台的工具链编译OpenWrt，然后执行as命令时，出现如下错误：

`libz.so.1: cannot open shared object file: No such file or directory`

找了一下，发现libz.so.1在zlib-bin包中，岂料安装zlib-bin之后，问题依旧。

最后才发现这个工具链是32位的程序，相应的libz.so.1实际上在lib32z1-dev包中。因此遇到类似的问题时，可以先注意一下程序的位数是否匹配。

## 4.ip not found

ip命令是linux网络管理方面的命令，它的代码在iproute2包中。

## 5.make menuconfig时提示“error opening terminal”的解决方法

1)首先要确定ncurses库是否已经正确安装。在debian, Ubuntu上，可以用`dpkg -l | grep ncurses`查看ncurses库是否已安装。

2)如果ncurses已经安装了，需要查看TERM, TERMINFO两个环境变量是否已经设置正确。如果没有设置正确，需要设置为正确的值。

{% highlight bash %}
$ echo $TERM
xterm
$ echo $TERMINFO
/lib/terminfo/
{% endhighlight %}

设置环境变量的方法：

1）临时的：使用export命令声明即可，变量在关闭shell时失效。示例如下：

`export PATH=/home/xyz/bin:$PATH;`

2）永久的：需要修改配置文件，变量永久生效。

在/etc/profile文件中添加变量（对所有用户生效）。修改文件后要想马上生效，还要运行`source /etc/profile`，不然只能在下次重进此用户时生效。

在用户目录下的.bash_profile文件中增加变量（对该用户生效）。同样需要source才能马上生效。

# procd

procd是OpenWrt中很重要的一个守护进程。它的作用主要有：

1)初始化系统。相当于普通linux的init。

2)硬件热插拔事件处理、看门狗。相当于普通linux的udev和watchdog。

3)日志系统。相当于普通linux的rsyslog。

## procd的引导过程

init/main.c: start_kernel --Linux的启动入口

init/main.c: rest_init

init/main.c: kenrel_init

执行/etc/preinit脚本

/sbin/init -- 该程序的源代码在procd包中。

initd/init.c: main -- /sbin/init的主函数

initd/preinit.c: preinit

这个函数中有以下代码片段：

`char *plug[] = { "/sbin/procd", "-h", "/etc/hotplug-preinit.json", NULL };`

再之后，就开始procd的执行了。

参考文献：

http://m.blog.csdn.net/blog/wwx0715/41725917

http://blog.chinaunix.net/uid-26598889-id-3060545.html

## 开启procd的debug信息

procd本身已经有很多debug信息，只是一般不打印而已。启动时，终端会提示你输入procd的debug level，取值范围0～4。0表示不输出，数字越大，输出的信息越多。

## procd对硬件热插拔事件的处理流程

1)/etc/hotplug.json

这个文件的格式，大致如下：

{% highlight text %}
[
	[ "case", "ACTION", {
		"add": [
			[ "if",
				[ "has", "FIRMWARE" ],
				[
					[ "exec", "/sbin/hotplug-call", "%SUBSYSTEM%" ],
					[ "load-firmware", "/lib/firmware" ],
					[ "return" ]
				]
			],
		],
		"remove" : [
			[ "if",
				[ "and",
					[ "has", "DEVNAME" ],
					[ "has", "MAJOR" ],
					[ "has", "MINOR" ],
				],
				[ "rm", "/dev/%DEVNAME%" ]
			]
		]
	} ],
	[ "if",
		[ "and",
			[ "eq", "SUBSYSTEM", "usb-serial" ],
			[ "regex", "DEVNAME",
				[ "^ttyUSB", "^ttyACM" ]
			],
		],
		[ "exec", "/sbin/hotplug-call", "tty" ]
	],
]
{% endhighlight %}

从代码可以看出，这个文件是个披着json外皮的程序文件，其关键字和C语言类似，而结构风格则类似Lisp语言：在表达式的组合上，广泛使用了逆波兰表达式。

这里的两个ACTION：add和remove，分别对应硬件设备的插消息和拔消息。简单的消息处理命令，可以直接写在这个文件中。复杂的需要交给/sbin/hotplug-call来处理。

以下面的代码片段为例：

`[ "exec", "/sbin/hotplug-call", "tty" ]`

hotplug-call会遍历/etc/hotplug.d/tty文件夹。该文件夹中的脚本，一般以NN_XXX的方式命名。其中的NN为数字。hotplug-call会按照NN的大小，从小到大依次执行文件夹中的所有脚本。

## U盘的热插拔处理

U盘驱动可分为两个层次：

1.USB设备驱动。这个的配置选项在linux内核中。

2.文件系统驱动。这个的配置选项在OpenWrt中，一般选择支持VFAT和NTFS即可。

以下是U盘插入时，生成的事件的procd日志：

{% highlight c %}
{"ACTION":"add","DEVPATH":"/devices/platform/rtl819x-ehci/usb1/1-1/1-1.1","SUBSYSTEM":"usb","MAJOR":"189","MINOR":"3","DEVNAME":"bus/usb/001/004","DEVTYPE":"usb_device","PRODUCT":"c76/5/100","TYPE":"0/0/0","BUSNUM":"001","DEVNUM":"004","SEQNUM":"466"}
{"ACTION":"add","DEVPATH":"/devices/platform/rtl819x-ehci/usb1/1-1/1-1.1/1-1.1:1.0","SUBSYSTEM":"usb","DEVTYPE":"usb_interface","PRODUCT":"c76/5/100","TYPE":"0/0/0","INTERFACE":"8/6/80","MODALIAS":"usb:v0C76p0005d0100dc00dsc00dp00ic08isc06ip50in00","SEQNUM":"467"}
{"ACTION":"add","DEVPATH":"/devices/platform/rtl819x-ehci/usb1/1-1/1-1.1/1-1.1:1.0/host0","SUBSYSTEM":"scsi","DEVTYPE":"scsi_host","SEQNUM":"468"}
{"ACTION":"add","DEVPATH":"/devices/platform/rtl819x-ehci/usb1/1-1/1-1.1/1-1.1:1.0/host0/scsi_host/host0","SUBSYSTEM":"scsi_host","SEQNUM":"469"}
{"ACTION":"add","DEVPATH":"/devices/platform/rtl819x-ehci/usb1/1-1/1-1.1/1-1.1:1.0/host0/target0:0:0","SUBSYSTEM":"scsi","DEVTYPE":"scsi_target","SEQNUM":
"470"}
{"ACTION":"add","DEVPATH":"/devices/platform/rtl819x-ehci/usb1/1-1/1-1.1/1-1.1:1.0/host0/target0:0:0/0:0:0:0","SUBSYSTEM":"scsi","DEVTYPE":"scsi_device","MODALIAS":"scsi:t-0x00","SEQNUM":"471"}
{"ACTION":"add","DEVPATH":"/devices/platform/rtl819x-ehci/usb1/1-1/1-1.1/1-1.1:1.0/host0/target0:0:0/0:0:0:0/scsi_disk/0:0:0:0","SUBSYSTEM":"scsi_disk","SEQNUM":"472"}
{"ACTION":"add","DEVPATH":"/devices/platform/rtl819x-ehci/usb1/1-1/1-1.1/1-1.1:1.0/host0/target0:0:0/0:0:0:0/scsi_device/0:0:0:0","SUBSYSTEM":"scsi_device","SEQNUM":"473"}
{"ACTION":"add","DEVPATH":"/devices/virtual/bdi/8:0","SUBSYSTEM":"bdi","SEQNUM":"474"}
{"ACTION":"add","DEVPATH":"/devices/platform/rtl819x-ehci/usb1/1-1/1-1.1/1-1.1:1.0/host0/target0:0:0/0:0:0:0/block/sda","SUBSYSTEM":"block","MAJOR":"8","MINOR":"0","DEVNAME":"sda","DEVTYPE":"disk","SEQNUM":"475"}
{"ACTION":"add","DEVPATH":"/devices/platform/rtl819x-ehci/usb1/1-1/1-1.1/1-1.1:1.0/host0/target0:0:0/0:0:0:0/block/sda/sda1","SUBSYSTEM":"block","MAJOR":"8","MINOR":"1","DEVNAME":"sda1","DEVTYPE":"partition","SEQNUM":"476"}
{% endhighlight %}

从上面的内容可以看出：

1.驱动的加载是有层次的。例如U盘驱动至少包括USB->SCSI->disk->partition这几个大的阶段。

2.上面提到的脚本，可以接收诸如ACTION、DEVPATH之类的环境变量。

## 其他事件

IP地址被改变事件示例：

{% highlight c %}
[ "$INTERFACE" = "lan" ] && [ "$ACTION" = "ifup" ] && {
	/etc/init.d/gmediarender restart
}
{% endhighlight %}

# Openwrt 3G拨号上网

参见：

http://blog.csdn.net/yicao821/article/details/45370669

# hotplug-button

kmod-gpio-button-hotplug     Simple GPIO Button Hotplug driver

kmod-button-hotplug          Button Hotplug driver

这两个内核模块不在内核主线中，需要在`make menuconfig`中单独勾选。

# 组建N2N VPN网络实现内网设备之间的相互访问

参见：

http://www.shuyz.com/n2n-vpn-network-introduction-and-config.html

# Openwrt对autotools、Cmake的支持

autotools和Cmake是目前应用最广的两套编译配置系统。Openwrt对它们支持的代码在/include/autotools.mk和/include/cmake.mk中。

# libubox

libubox是Openwrt平台的一个工具库。详见：

http://www.w2bc.com/article/91056

# 使用GDB调试

由于完整的GDB尺寸太大（~1.5MB），因此通常使用GDBServer进行调试。两者的代码都在gdb软件包中。

参考文档：

http://wiki.openwrt.org/doc/devel/gdb

http://h4x3rotab.github.io/blog/2014/02/27/openwrtxia-de-gdbyuan-cheng-diao-shi/

除了上面列出的内容之外，我还遇到了一个问题：我所用平台的SDK将`-Os`作为全局的编译选项。这在平时自然没什么，但调试的时候就有问题了。如何将`-Os`换成`-O0`呢？可参见以下示例：

{% highlight c %}
TARGET0_CFLAGS:=$(filter-out -Os,$(TARGET_CFLAGS))
TARGET_CFLAGS:= -O0 $(TARGET0_CFLAGS) -ggdb3
{% endhighlight %}

这里解释一下：

1.filter-out是make提供的过滤函数，可去除字符串A中包含的特定字符串B。

2.定义TARGET0_CFLAGS的原因在于：make不支持变量的递归定义，需要中间变量暂存之。

