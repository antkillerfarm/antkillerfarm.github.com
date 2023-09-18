---
layout: post
title:  Android研究（二）, MSYS2, WSL, 线程池
category: technology 
---

* toc
{:toc}

# Android研究

## Flutter

Flutter是Google用以帮助开发者在Ios和Android两个平台开发高质量原生应用的全新移动UI框架。Beta1版本于2018年2月27日在2018世界移动大会上公布。

官网：

https://flutter.dev/

参考：

https://mp.weixin.qq.com/s/pU75twMDry4VUYtTHeV_IQ

一文深入了解Flutter界面开发

https://mp.weixin.qq.com/s/yPvaB7sLuJoGfsjj7x7wcg

深入理解Flutter的编译原理与优化

https://mp.weixin.qq.com/s/SWP7Nu9DEUhvAyvdIG-Ixw

聊一聊Flutter Engine线程管理与Dart Isolate机制

https://mp.weixin.qq.com/s/cJjKZCqc8UuzvEtxK1BJCw

Flutter的原理及美团的实践

https://mp.weixin.qq.com/s/l6xvmnLE6HfRtw6upo6yUA

高效开发与高性能并存的UI框架——携程Flutter实践

https://mp.weixin.qq.com/s/vcbHMtaJEkZhSgiRBST1YA

移动开发这十年

https://mp.weixin.qq.com/s/VlleaiIzsZHZDkDrGgYi1g

如何用Flutter实现混合开发？闲鱼公开源代码实例

https://mp.weixin.qq.com/s/n2avWVS0FAFIEB4w0W0Xsw

学习Dart的10大理由

https://mp.weixin.qq.com/s/CBp1pneBa_t8bkVlh1RGVg

2019年五大跨平台移动应用开发工具

https://mp.weixin.qq.com/s/ygPRdtRMlNW3-nfo0PonAA

揭秘！如何用Flutter设计一个100%准确的埋点框架？

## Litho

Litho是Facebook推出的一套高效构建Android UI的声明式框架，主要目的是提升RecyclerView复杂列表的滑动性能和降低内存占用。

官网：

https://fblitho.com/

参考：

https://mp.weixin.qq.com/s/RS7O7prvkCvKyxkK3YQxtA

Litho的使用及原理剖析

## adb

手机->PC：

`adb pull sdcard/contacts_app.db`

PC->手机：

`adb push aaa/contacts_app.db /sdcard/`

---

https://www.cnblogs.com/caoxinyu/p/10568463.html

Ubuntu adb报错：no permissions (user in plugdev group; are your udev rules wrong?);

## tombstone

当一个动态库（native 程序）开始执行时，系统会注册一些连接到debuggerd的signal handlers，当系统crash的时候，会保存一个tombstone文件到/data/tombstones目录下。

https://www.cnblogs.com/CoderTian/p/5980426.html

Android Tombstone分析

## Logcat

官方文档：

https://developer.android.google.cn/studio/command-line/logcat

参考：

https://www.cnblogs.com/JianXu/p/5468839.html

Android logcat命令详解

## 打包

apk包就是android系统的安装包。

aar是android中特有的归档文件，既包含字节码文件也包含android的资源文件等。

jar是java字节码文件（class文件）的归档文件，其不包含android中的资源文件等信息。

https://blog.csdn.net/it_yangkun/article/details/80119182

Android打包APK,AAR,JAR

## 参考

https://mp.weixin.qq.com/s/twfpUMf9CfXcgwtFFkJ4Ig

Android整体设计及背后意义

https://mp.weixin.qq.com/s/eEuNPtTaPwJ7hSghgeU32g

Android Hook技术防范漫谈

https://mp.weixin.qq.com/s/AQI2S2oK7HFDs9lH-nsx5g

Android性能优化系列：Java内存优化篇

https://www.jianshu.com/p/80013a768a45

Android soong build系统介绍

https://mp.weixin.qq.com/s/xQ6w1qlMjgxlP8QpF34GVA

不吹不擂，一文揭秘鸿蒙操作系统

https://mp.weixin.qq.com/s/QC0QDPwBU5OLjtZNqDzSrg

鸿蒙之迷思

# MSYS2

2015.11

稍早的时候，我在安装pygtk的环境时，发现pygi-aio安装程序提供的GTK版本已经到了3.14。但GTK官方这时仍然没有进展，版本停留在3.6.4。于是我又打算使用pygtk提供的更新来进行编程，但pygtk只提供了.dll文件，而没有.h和.lib文件，实际上并不好用，因此只得放弃之。

最近再上官网，发现其已经提供了更好的解决办法——MSYS2。MSYS2是Martell Malone维护的一个开源项目，旨在提供一个方便易安装的MSYS开发环境，其中也包括为各种开发包提供更新维护，GTK就是其中之一。（当前的版本为Gtk 3.18.3）

现将安装步骤罗列如下：

1.在http://www.msys2.org/，下载安装程序，并按照网页提示，更新pacman。

2.使用pacman下载必要的开发包。

`pacman -S autoconf autogen automake-wrapper pkg-config make gcc gdb`

`pacman -Ss <name>`: 查询相关的软件包是否存在。

`pacman -Syu`: 更新已安装的软件包。

MSYS2提供的环境除了安装友好，便于更新之外，对bash的支持也优于之前的版本。现在已经不需要单独为Windows平台提供特殊的makefile文件了。

还可在如下网址查询packages：

https://github.com/Alexpux/MINGW-packages

https://github.com/Alexpux/MSYS2-packages

MSYS2提供了两套API：mingw32和mingw64。可用以下方法（以gcc为例）查询需要安装的package的名字：

`pacman -Sl | grep gcc`

与MSYS2类似的还有Cygwin、MinGW等。

参见：

https://www.zhihu.com/question/22137175

Cygwin和MinGW的区别与联系是怎样的？

# WSL

Cygwin：提供了兼容POSIX接口的应用层接口，性能不佳。

MinGW：直接接Win32 API，只移植了GNU工具链，功能不全。

MSYS2：MinGW+Cygwin部分工具包+pacman。

WSL（Windows Subsystem for Linux），刚出来的时候叫“Bash on Ubuntu on Windows”：POSIX接口直接接到NT内核，性能超过Cygwin。

WSL2：虚拟机，有独立的Linux内核。和VirtualBox之类类似。

https://www.zhihu.com/question/50144689

win10 linux子系统和cygwin有什么不同？

https://mp.weixin.qq.com/s/ZCkboBBFYFm57pLEGftpCw

双系统的日子结束了：Windows和Linux将合二为一

https://zhuanlan.zhihu.com/p/57774611

盘点与Cygwin相似和相反的项目

# 线程池

假设一个服务器完成一项任务所需时间为：T1创建线程时间，T2在线程中执行任务的时间，T3销毁线程时间。如果：T1+T3远大于T2，则可以采用线程池，以提高服务器性能。

一个线程池包括以下四个基本组成部分：

1.线程池管理器（ThreadPool）：用于创建并管理线程池，包括 创建线程池，销毁线程池，添加新任务；

2.工作线程（PoolWorker）：线程池中线程，在没有任务时处于等待状态，可以循环的执行任务；

3.任务接口（Task）：每个任务必须实现的接口，以供工作线程调度任务的执行，它主要规定了任务的入口，任务执行完后的收尾工作，任务的执行状态等；

4.任务队列（taskQueue）：用于存放没有处理的任务。提供一种缓冲机制。

在libupnp项目中，提供了一个简易的线程池实现。

其他的线程池实现还有：

https://github.com/vit-vit/ctpl

参考：

https://mp.weixin.qq.com/s/KigkxkUDYP8r1SEmDUvCCw

从串行线程封闭到对象池、线程池

https://www.jianshu.com/p/a24aa3c63d50

从TCP角度出发，看看连接池的优点

https://mp.weixin.qq.com/s/baYuX8aCwQ9PP6k7TDl2Ww

Java线程池实现原理及其在美团业务中的实践

https://mp.weixin.qq.com/s/8tFPVJfGJ0G8FUu9m5TCug

高并发中的线程与线程池

https://mp.weixin.qq.com/s/QbBr7ZQhD8DwzSzf4RRvgQ

聊一聊携程的Apollo

# Git+

## Monorepo

上面提到的多项目管理，一般称为MultiRepo，即每个项目对应一个单独的仓库。与之相对的则是Monorepo，即把多个项目放在一个仓库里面。

![](/images/img4/Monorepo.png)

参考：

https://juejin.cn/post/6944877410827370504

现代前端工程为什么越来越离不开Monorepo?

https://zhuanlan.zhihu.com/p/77577415

Monorepo是什么，为什么大家都在用？

## Git多项目管理

项目越来越大，一些通用的模块我们希望将他抽离出来作为单独的项目，以便其他项目也可以使用，或者使用一些第三方库，可能我们并不想将代码直接拷贝进我们的项目里面，而仅仅只是单纯的引用。

基于Git有多种方式来解决这个问题：

- Git Submodule。

- Git Subtree。这两个已经集成到git中。

- GitSlave。一个git插件，需要额外安装。

- Google Repo。基于git的python脚本。Android项目的官方工具。

`git submodule update --init --remote --rebase`

单独更新子module AAA：

`git submodule update AAA`

参考：

https://www.jianshu.com/p/284ded3d191b

Git多项目管理

## OpenGrok

OpenGrok是一个阅读源码的Web系统。

官网：

http://oracle.github.io/opengrok/

代码：

https://github.com/oracle/opengrok

参考：

http://mazhuang.org/2016/12/14/rtfsc-with-opengrok/

搭建大型源码阅读环境——使用OpenGrok

## 参考

https://mp.weixin.qq.com/s/z_zFveiiLu9vLvWuBcsaIg

Git从入门到进阶

https://mp.weixin.qq.com/s/0Cv968LpSSYKJpAbW1MlMA

手把手教你git全操作

https://mp.weixin.qq.com/s/DbvRbaH7BJKeTCT4LVXUoA

Git的4个阶段的撤销更改

https://mp.weixin.qq.com/s/nUqvSnnPjYqk2O8u0tQEtQ

Git内部原理揭秘

https://mp.weixin.qq.com/s/nmi1HYkKD8QX0phbvOko2Q

Git居然还有这么高级用法，你一定需要

https://mp.weixin.qq.com/s/PUUL913fig6cFfqy4OKcGA

工作流一目了然，看小姐姐用动图展示10大Git命令

https://blog.csdn.net/mocoe/article/details/84344411

修改git commit的author信息

https://mp.weixin.qq.com/s/9Ey04P5Xv4W95N2FEioZ1g

如何选择Git分支模式？

https://mp.weixin.qq.com/s/GZkGfwrfMTzrMfki8LKbIw

腾讯广告3000+万行大代码库主干开发实战

https://mp.weixin.qq.com/s/KEu6qCl-En6HiKGo2pgEQg

TensorBay：一款易用的像Git一样数据版本管理工具！
