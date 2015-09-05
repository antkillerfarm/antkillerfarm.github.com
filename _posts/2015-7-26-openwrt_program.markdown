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

# OpenWrt目录结构粗解

## feeds

在OpenWrt的源代码中，其实程序代码是微乎其微的。整个代码本身主要是维护一个很复杂的编译系统。至于内核和各个应用的软件包，基本都是以网上下载的方式获得代码，进而编译的。使用源代码的原因，是由于OpenWrt通常是运行在各种嵌入式系统中，而二进制包显然不具备可移植性。

feeds文件夹的作用就是指明如何下载并编译这些软件源代码包。feeds的更新由专门的版本库来维护，这些版本库也被称为“软件源”。可修改feeds.conf.default来更换不同的或者是非官方的源。

一个feeds通常会包含一个patch文件夹（也可以没有），这里需要注意的是修改patch文件夹内的patch文件时，需要把生成的临时文件.patch~删除掉，不然会出错。

## dl

存放下载下来的压缩格式的源码包。

## build_dir

解压缩之后的源码包。这里又分为三个文件夹：

host：宿主机上的应用。

toolchain：交叉编译工具链。

target：目标板上的应用。

## staging_dir

编译好的二进制文件及配置文件，也分了host、toolchain、target三个文件夹。

其中，target下的pkginfo用于维护目标板软件的依赖关系，在编译环节显得尤为重要。

## bin

最终的生成结果。

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

