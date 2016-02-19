---
layout: post
title:  Cocos2d-x v3在Qt 5上的移植, Autotools
category: technology 
---

# 前言

首先对讨论的内容做个解释。

1.本文不是讨论怎么用Qt Creator之类的Qt IDE编译并运行cocos2d-x。

这个问题的答案可以参见：

https://github.com/FlyingFishBird/cocos2d-x_linux_qt_template

2.本文也不是讨论如何利用Qt，使得cocos2d-x能够跨平台运行。虽然Qt有良好的跨平台特性，但是目前cocos2d-x的跨平台特性已经足够好，无需Qt辅助。

3.虽然cocos2d-x本身已经集成了一些常用窗体控件，但其种类毕竟和专门开发窗体应用的Qt有差距。对于像开发cocos2d-x编辑工具之类的复杂窗体应用来说，并不好用。

因此本文的目的就是讨论如何将cocos2d-x渲染的内容嵌入到Qt的窗体控件中。

4.为什么是Qt，而不是其他GUI框架？

现有的cocos2d-x粒子特效编辑器，只有Windows版本，没有Linux版本。目前的实现主要采用MFC+cocos2d-x。如下面的文章：

http://blog.csdn.net/honghaier/article/details/7897077

或者在此基础之上的Qt+cocos2d-x。如下所示：

http://www.cnblogs.com/shadow21/archive/2012/11/09/2763158.html

这个Qt实现的局限在于，由于它用到了Windows平台特有的句柄这个概念，因此虽然披着Qt的皮，但却并不能移植到Linux平台上。

5.为什么是粒子特效编辑器，其他的游戏编辑器怎么做到的跨平台？

粒子特效属于动画效果，普通GUI框架对动画的支持比较差。而其他游戏内容，使用普通GUI框架就足够编辑了。

6.内容编排的侧重点。

honghaier的blog，近乎是手把手操作的流水账，对于关键的理论知识讲的比较少。因此一但cocos2d-x版本升级，相关的示例代码需要修改时，评论区中有不少人都在询问新版本的移植问题。

为了避免重蹈覆辙，本文不粘贴代码占篇幅，而是主要聚焦于技术的要点。所谓授人以鱼，不如授人以渔。当然所有的环节都会提供可下载的代码，以供读者研究。

# 编译环境

可供选择的方案无非两种，或者采用Qt的Qmake，或者采用cocos2d-x的Cmake。

以下是稍早写的一篇心得。

## CMake使用心得

最近研究cocos2d-x如何与Qt一起使用的问题，网上的说法大概有这样几种：

1.使用VS工程管理代码。这个的缺点很明显：不能跨平台。考虑到我的情况，断然放弃之。

2.使用Qt工程管理代码。这个一来我对qmake不熟，二来cocos2d-x那边的代码也需要添加到工程中，这也是个不小的工作量。

3.使用CMake管理代码。这个甚至连Qt官方也认为，如果工程太复杂的话，推荐使用CMake来管理代码。比如最大的Qt工程KDE项目就使用了CMake。

这里主要参考了一下文献：

http://blog.csdn.net/dbzhang800/article/details/6314073

为了防止链接的失效，现将要点摘录如下，并附上实际工程的代码例子，供大家参考。

## 例子一

{% highlight bash %}
.
├── bin
├── build
├── CMakeLists.txt
├── main.cpp
├── mainwindow.cpp
├── mainwindow.h
├── mainwindow.ui
└── qt_cmake.pro
{% endhighlight %}

实现这样效果的代码参见

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/Qt/qt_cmake

这个目录下的build脚本，讲述了如何使用CMake的方法。需要注意的是，在当前路径下运行`cmake .`和在bin文件夹下运行`cmake ..`的效果是不一样的。

## 例子二

{% highlight bash %}
.
├── bin
├── CMakeLists.txt
└── src
    ├── main.cpp
    ├── mainwindow.cpp
    ├── mainwindow.h
    └── mainwindow.ui
{% endhighlight %}

实现这样效果的代码参见

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/Qt/qt_cmake1

# Step 1

有了之前的介绍之后，我们现在就可以使用cocos2d-x的编译环境来编译Qt程序。

代码如下：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/cocos_on_qt_widget/step1

切换proj.linux/main.cpp中的宏，可以实现qt和cocos2d-x窗体之间的切换。这说明两个体系已经可以在同一个编译环境下共存了。

# Step 2

## OpenGL与窗口系统

从代码可知，cocos2d-x的渲染操作是基于OpenGL的。而OpenGL本身只是个图形渲染库，并不提供窗口功能，因此这里有必要对OpenGL如何和窗口系统配合，做个介绍。

一个OpenGL程序的创建一般有如下步骤：

1.创建窗体。

2.初始化OpenGL库。

3.CreateContext。这个函数相当于创建一个特定大小和颜色格式的画布。

4.MakeCurrent。这个函数将第3步创建的Context和第1步创建的窗体关联起来。

以上步骤的顺序并不严格，只要求第2步必须在第3步之前，第4步必须在最后即可。

此外，很多OpenGL相关的窗体工具库，将第1步和第3步合为1步。在创建窗口的同时，亦创建Context。从而导致从用户接口的层面来看，缺失了第3步的假象。

渲染步骤一般为：

1.OpenGL渲染操作。OpenGL的设计基于状态机，因此操作都是相对于当前状态机而言的。OpenGL状态机是一系列属性的集合，其中也包括了Context的状态。所以这些操作实际上都是针对当前Context执行的。

2.swapBuffers。在使用双缓冲绘制的窗体时，将渲染结果交换到当前的显示缓冲区。单缓冲绘制无此步骤。

可以参考以下文章，加深对上面概念的理解：

http://blog.csdn.net/hong19860320/article/details/7287763

这篇文章介绍了OpenGL和窗口系统交互的过程。

http://antkillerfarm.github.io/technology/2014/12/26/gtk_study.html

这篇文章是本人写的在GTK3上添加OpenGL的心得。如果没有这次实践，就没有现在的移植成果。

## 为cocos2d-x添加新平台支持的要点

话题回到cocos2d-x本身。cocos2d-x的平台相关部分都放在cocos/platform下。平台相关部分又可分为窗口和非窗口两部分。这里我们主要讨论窗口部分的实现。

对于PC平台来说，由于其所使用的glfw库是跨平台的，因此无论是Windows，还是Linux，其窗口部分都被统一化，放在cocos/platform/desktop下。这个文件夹下主要是CCGLViewImpl类的实现。

CCGLViewImpl类为上层提供了统一的调用接口。它的关键是维护_mainWindow指针，在glfw实现中，这个指针是GLFWwindow类型的。

综上所述，我们将Cocos2d-x移植到Qt上（或者更专业的说法：为Cocos2d-x添加Qt后端）的关键，就是使用Qt的API替换glfw的API。

## 成果

Step 2的代码可以在这里下载：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/cocos_on_qt_widget/step2

这个例子程序将Cocos2d-x的渲染区域嵌入到一个QGLWidget控件中。

# Step 3

Step 2虽然成果很大，但也有如下缺陷：

1.Cocos2d-x不能处理鼠标、键盘事件。

2.Cocos2d-x游戏主循环未建立，因此只有单帧渲染效果，而无动画渲染效果。

# 参考代码

https://github.com/ascetic85/quick-cocos2d-x-20130509

这个代码有些老，是基于cocos2d-x v2的，但是基本的思路是一样的。

# Autotools

## 概述

autotools是GNU提供的一系列自动配置工具的集合，其主要包括autoconf、automake、pkg-config和libtool等工具。

Makefile虽然规则简单，但手工书写makefile却不是一件容易的事。我这里的不容易指的是：编写好的makefile不容易。如果只是那种自己平时demo的话，可能还是直接makefile更简单。毕竟autotools包含那么多工具，每个工具的规则都不尽相同，学习门槛比makefile高多了。如果针对小工程，还不一定有直接makefile来的快。

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

## pkg-config

一般来说，如果库的头文件不在/usr/include目录中，那么在编译的时候需要用-I参数指定其路径。由于同一个库在不同系统上可能位于不同的目录下，用户安装库的时候也可以将库安装在不同的目录下，所以即使使用同一个库，由于库的路径的不同，造成了用-I参数指定的头文件的路径和在连接时使用-L参数指定lib库的路径都可能不同，其结果就是造成了编译命令界面的不统一。可能由于编译，连接的不一致，造成同一份程序从一台机器copy到另一台机器时就可能会出现问题。

pkg-config就是用来解决编译连接界面不统一问题的一个工具。

它的基本思想：pkg-config是通过库提供的一个.pc文件获得库的各种必要信息的，包括版本信息、编译和连接需要的参数等。需要的时候可以通过pkg-config提供的参数(–cflags, –libs)，将所需信息提取出来供编译和连接使用。这样，不管库文件安装在哪，通过库对应的.pc文件就可以准确定位,可以使用相同的编译和连接命令，使得编译和连接界面统一。

参见：

http://www.mike.org.cn/articles/description-configure-pkg-config-pkg_config_path-of-the-relations-between/

## autoconf&automake与pkg-config的协同工作



## CMake和pkg-config的协同工作

参考文档：

http://francesco-cek.com/cmake-and-gtk-3-the-easy-way/

示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gtk_browser/cmake

