---
layout: post
title:  makefile, Autotools, premake, Bazel
category: technology 
---

# makefile

简单的规则可以查看《GNU makefile指南》一文，Goerge Foot写的。地址就不贴了，中文版基本到处都是。但是该文只是入门级的，最专业的还是GNU官方的手册：

http://www.gnu.org/software/make/manual/html_node/index.html

应该说make的语法，除了规则依赖之外，大多数与bash相同。但是make也有一些内置函数，它们的用法就不在bash的范畴了，比如call函数。碰到这样的情况，可以查阅以下网页，以快速定位帮助文档的位置：

http://www.gnu.org/software/make/manual/html_node/Quick-Reference.html

# Autotools

## 概述

autotools是GNU提供的一系列自动配置工具的集合，其主要包括autoconf、automake、pkg-config和libtool等工具。

Makefile虽然规则简单，但手工书写makefile却不是一件容易的事。我这里的不容易指的是：编写符合规范的makefile不容易。如果只是那种自己平时demo的话，可能还是直接makefile更简单。毕竟autotools包含那么多工具，每个工具的规则都不尽相同，学习门槛比makefile高多了。如果针对小工程，还不一定有直接makefile来的快。

那么autotools的优点在哪里呢？

1.针对大工程时，手工编写量较小。比如需要添加的源文件和头文件，直接autoscan就出来了，不用一个一个的写。

2.可以检查编译环境。这个手工写，难度太大，我是不会写的。

3.可移植好。由于第2点的存在，autotools生成的makefile的可移植性非常好。这个优点在本地编译的时候，体现的并不十分明显，但如果交叉编译的话，就很突出了。

参考文档：

https://autotools.io/index.html

这个网站基本上每个工具都讲到了，非常值得一看。

## autoconf&automake

这两个工具是整个autotools工具集的核心，使用的流程也比较复杂。教程中最经典的是：

http://www.ibm.com/developerworks/cn/linux/l-makefile/index.html

例解autoconf和automake生成Makefile文件

但是，这个教程比较老了，有的地方的处理并不符合当前的版本，以下针对这些新老版本的差异，做一个说明。

1.首先给出新版本（2016.2）的示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gtk_browser/autotool

2.configure.in在新版本中改叫configure.ac。其中，AM_INIT_AUTOMAKE宏的格式已经和老的不同了。AC_OUTPUT宏虽然还存在，但是部分功能分解给AC_CONFIG_FILES宏。

3.拷贝depcomp和complie文件的过程，现在可以通过命令自动完成，无须手动拷贝。

4.运行automake之前，需要先用autoheader生成config.h文件。

从整个流程来说，autotools相比Cmake无疑复杂的多了。但实际使用中，由于这几个步骤都是流程化的，简化起来也比较容易。因此新版本提供了autoreconf命令，用来一站式生成所需的编译配置文件。其示例如下：

`autoreconf -fi`

## pkg-config

一般来说，如果库的头文件不在/usr/include目录中，那么在编译的时候需要用-I参数指定其路径。由于同一个库在不同系统上可能位于不同的目录下，用户安装库的时候也可以将库安装在不同的目录下，所以即使使用同一个库，由于库的路径的不同，造成了用-I参数指定的头文件的路径和在连接时使用-L参数指定lib库的路径都可能不同，其结果就是造成了编译命令界面的不统一。可能由于编译，连接的不一致，造成同一份程序从一台机器copy到另一台机器时就可能会出现问题。

pkg-config就是用来解决编译连接界面不统一问题的一个工具。

它的基本思想：pkg-config是通过库提供的一个.pc文件获得库的各种必要信息的，包括版本信息、编译和连接需要的参数等。需要的时候可以通过pkg-config提供的参数(–cflags, –libs)，将所需信息提取出来供编译和连接使用。这样，不管库文件安装在哪，通过库对应的.pc文件就可以准确定位,可以使用相同的编译和连接命令，使得编译和连接界面统一。

参见：

http://www.mike.org.cn/articles/description-configure-pkg-config-pkg_config_path-of-the-relations-between/

简述configure、pkg-config、pkg_config_path三者的关系

## autoconf&automake与pkg-config的协同工作

由于pkg-config是一系列Glib派生项目（如Gstreamer、GTK）所用的配置工具。因此，如何让autoconf&automake与pkg-config协同工作就成为了一个很重要的课题。多数autotools教程由于只是helloworld型的，并没有涉及到这方面的内容。

这里仍然使用上一节的示例代码，进行讲解。

协同工作的关键在于PKG_CHECK_MODULES宏，这个宏的语法为：

`PKG_CHECK_MODULES(prefix, list-of-modules, action-if-found, action-if-not-found)`

该宏被执行后，会生成prefix_CFLAGS和prefix_LIBS两个全局变量。它们在Makefile.am中会用到。

这里的AC_SUBST宏，主要是针对老版本的pkg-config所做的修改，在pkg-config v0.24以后的版本中，加不加都无所谓。

这方面更复杂的例子，可以查看Totem项目的代码，该项目复杂度中等。如果这个例子还不够用的话，那就直接查看GTK项目的代码吧。我相信绝大多数的人，都不可能开发出一个比它更大的项目来。

## CMake和pkg-config的协同工作

Cmake虽然主要用于Qt项目，但用于GTK项目，实际上也没有什么问题。

Cmake使用的pkg_check_modules宏，和上面的PKG_CHECK_MODULES宏，从原理来说，是类似的。这里不再赘述。

参考文档：

http://francesco-cek.com/cmake-and-gtk-3-the-easy-way/

示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gtk_browser/cmake

## Autotools细节汇总

1.`dnl` vs `#`

`dnl`和`#`在autoconf中，都是注释开始的标志。区别在于，`dnl`注释的东西，不会出现在最终的configure脚本中。

2.设置debug版本

CFLAGS的默认设置是`-g -O2`，可用以下命令修改之：

`./configure CFLAGS="-g -O0"`

或者也可以用`./configure --help`查看支持的编译选项。

比如生成静态链接库，可使用以下命令：

`./configure --enable-shared=no --enable-static=yes`

3.设定so文件的导出符号表

`libtool --mode=link gcc -o libfoo.la foo.lo -export-symbols=foo.sym`

# premake

premake是一个轻量级的构建系统。这里所谓的轻量级是和Autotools、CMake相比。

Autotools在Unix平台的功能最为全面。CMake添加了对Windows平台的支持，但在Unix平台上，仍不得不借助部分Autotools工具的功能，比如pkg-config。

这两个重量级工具，都有很多重量级的使用者，一般不必担心功能不够使的情况。它们的缺点是，使用了特殊的DSL（Domain Specific Language），用户需要花费额外的学习时间来学习这些DSL。

premake使用lua语言编写，语法比CMake更简单。它的官网是：

http://premake.github.io/

premake的缺点在于，它基本上是个人作品，全职开发人员太少，导致功能有限，目前尚无特大项目使用该系统，其功能性和可靠性受到质疑。

<a name="Bazel"/>

# Bazel 

安装：

{% highlight bash %}
echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | sudo tee /etc/apt/sources.list.d/bazel.list
curl https://bazel.build/bazel-release.pub.gpg | sudo apt-key add -
sudo apt-get update && sudo apt-get install bazel
{% endhighlight %}

官网：

https://bazel.build/

文档：

https://docs.bazel.build/versions/master/bazel-overview.html

下载：

https://github.com/bazelbuild/bazel/releases

参考：

http://www.cnblogs.com/Jack47/p/bazel-faq.html

Google软件构建工具Bazel FAQ

# Other

## blade

blade是腾讯出品的构建工具。官网：

https://github.com/chen3feng/typhoon-blade

blade的底层用到了SCons。后者是一个python语言写的构建工具，可用于多种编程语言程序的构建。官网：

http://www.scons.org/

参考：

https://www.ibm.com/developerworks/cn/linux/l-cn-scons/

使用SCons轻松建造程序

## MSbuild

MSbuild当然是微软的构建工具了。官网：

https://msdn.microsoft.com/en-us/library/dd393574.aspx

参考：

http://www.cnblogs.com/linianhui/archive/2012/08/30/2662648.html

MSBuild入门

## OkBuck

OkBuck是Uber推出的构建工具。官网：

https://github.com/uber/okbuck

