---
layout: post
title:  OpenWRT
category: technology 
---

# OpenWRT编译

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

