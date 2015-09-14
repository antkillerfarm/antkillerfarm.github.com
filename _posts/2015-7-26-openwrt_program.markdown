---
layout: post
title:  OpenWrt编程篇, uboot, Linux镜像文件
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

# 扩展包

大多数情况下，我们并不需要亲手编写OpenWrt模块，因为日常使用中的绝大部分功能，OpenWrt项目都提供了相应的扩展包。因此，如何寻找、安装扩展包，就成为一个经常性的问题。

官方扩展包可以在代码根目录下的feeds.conf.default文件中找到。

从这里可以看出，OpenWrt项目早期的代码托管在openwrt.org下，而最近的开发已经迁移至github。因此，如果有的包在源代码中没有看到的话，可以到openwrt.org下碰碰运气。

一个软件包可以生成独立的ipk安装文件（Modularizes），也可以直接打包进img中（Built-in）。这个生成选项在`make menuconfig`的选项菜单中，选择Y就是Built-in，选择M就是Modularizes。

# OpenWrt常见编译问题

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

# uboot

## 从uboot到Linux

这里以uboot 2014年11月的主线代码为例分析从uboot到linux的全过程。之所以写这篇文章，是由于网上的资料多数都很陈旧，诸如start_armboot之类的函数在新的代码里根本找不到了。由于uboot支持的CPU以及Board非常的多，所以本文仅以Samsung exynos为例来介绍这个过程。

从上电到uboot启动:

1./arch/arm/cpu/armv7/start.S: reset——uboot的汇编入口

2./arch/arm/lib/crt0.S: _main

3./arch/arm/lib/board.c: board_init_f——初始化第一阶段

4./arch/arm/lib/board.c: board_init_r——初始化第二阶段

5./common/main.c: main_loop——uboot主循环

uboot启动Linux

1.uboot中有个bootd的命令选项,执行该命令会进入/common/cmd_bootm.c: do_bootd

2.common/cli.c: run_command，传入bootcmd命令作为参数。

3.common/cmd_bootm.c: do_bootm

4.arch/arm/lib/bootm.c: do_bootm_linux

5.arch/arm/lib/bootm.c: do_jump_linux——跳转到Linux内核的入口地址

uImage格式是专为uboot开发的格式，主要解决了uboot和linux在嵌入式设备的存储上共存的问题。

## uboot命令处理流程

从main_loop到命令处理：

1./common/main.c: main_loop

2./common/cli.c: cli_loop

3./common/cli_simple.c: cli_simple_loop

4./common/cli.c: run_command_repeatable

5./common/cli_simple.c: cli_simple_run_command

6./common/cli_simple.c: cmd_process

7./common/command.c: cmd_call

上面的流程仅是主循环如何调用命令回调函数的过程。下面介绍一下命令是如何声明、存储和查询的。

首先查看链接脚本，uboot使用的链接脚本文件名为u-boot.lds。根据cpu和board的不同，u-boot.lds也有所差异。例如Samsung exynos所用的u-boot.lds在arch\arm\cpu下。

其中有个`.u_boot_list`段就是用来存储命令数据的。它的表述如下所示：

{% highlight bash %}
.u_boot_list : {
		KEEP(*(SORT(.u_boot_list*)));
	}
{% endhighlight %}

命令的声明，通常使用U_BOOT_CMD宏。这个宏最终展开为：

{% highlight bash %}
_type _u_boot_list_2_##_list##_2_##_name __aligned(4)		\
		__attribute__((unused,				\
		section(".u_boot_list_2_"#_list"_2_"#_name)))
{% endhighlight %}

这也就是`.u_boot_list*`的来历了。

可以使用/common/command.c: find_cmd函数在命令列表中，根据名称查找命令数据。

## setenv

/common/cmd_nvedit.c: setenv--这个函数用于设置环境变量的值。它的原理是：

1.首先在环境变量数组default_environment中，更改相应内容的值。

2.然后调用saveenv，保存default_environment的值，到具体的硬件中。例如NAND设备的代码在/common/env_nand.c中。

# Linux镜像文件

## vmlinux

这是源代码直接生成的镜像文件。以x86平台为例：

arch\x86\kernel\vmlinux.lds.S--这是链接脚本的源代码，经过C语言的宏预处理之后会生成vmlinux.lds，使用这个脚本，链接即可得到vmlinux，其过程与普通应用程序并无太大区别，也就是个elf文件罢了。

## image

vmlinux使用objcopy处理之后，生成的不包含符号表的镜像文件。这是linux默认生成的结果。

## zImage

zImage = 使用gzip压缩后的image + GZip自解压代码。使用`make zImage`或者`make bzImage`创建。两者的区别是zImage只适用于大小在640KB以内的内核镜像。

## uImage

uImage = uImage header + zImage。使用uboot提供的mkimage工具创建。

以上的这些镜像文件的关系可参见：

http://www.cnblogs.com/armlinux/archive/2011/11/06/2396786.html

http://www.linuxidc.com/Linux/2011-02/32096.htm

## Flash镜像

一般来说，一个完整的linux系统，不仅包括内核，还包括bootloader和若干分区。这些镜像文件散布，不利于批量生产的进行。这时就需要将之打包，并生成一个可直接用于生产烧写的Flash镜像。

可使用mtd-utils库中的ubinize工具生成Flash镜像。

mtd-utils的官网是：

http://www.linux-mtd.infradead.org/

安装方法：

`sudo apt-get install mtd-utils`

参考：

http://blog.csdn.net/andy205214/article/details/7390287





