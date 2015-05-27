---
layout: post
title:  FreeType, CMake, Qt, FFmpeg, SDL
category: technology 
---

# FreeType使用指南

FreeType是一套跨平台的字体文件编程开发包。它的官网是www.freetype.org，你可以到这个网站获得最新的源代码。

网上的关于FreeType的文献很多。写得较好的有以下两篇：

http://www.cppblog.com/wlwlxj/archive/2006/11/08/14843.aspx   （文献A）

这篇文章比较概括，且有demo，适合入门之用。

http://hi.baidu.com/guopeng717/blog/item/f371088287532a95f603a6db.html   （文献B）

这篇文章深入一些，但代码片段不可运行，需要对FreeType有根底的才看的明白。

本着原创的精神，上面文献提到过的，我不再复述，仅结合自己近来的研究，对一些关键点做一下说明。

## 1.编译FreeType库

由于我只用VC编过，所以这里只谈VC编库心得。这是我见到过的最友好的跨平台库，VS的工程文件都很齐全，不像其他的软件库往往只有make文件。而且FreeType库中，不但有PC的工程，还有Wince的工程。这里唯一要注意的是，在VS2005或更高版本中，有时需要调整工程的参数，以满足VS不同版本之间的移植需要。

## 2.字符对齐

文献A中的代码，显示单个字是没有问题的，但显示一排字尤其是数字符号时，就有问题了。虽然我们已经使用了FT_Set_Char_Size或FT_Set_Pixel_Sizes设定了字模的大小，但返回的字模并不都是一样大的。空白字符返回的字模，大小为0，逗号、句号返回的字模只有普通字的几分之一。这时就需要用glyph->bitmap_left和glyph->bitmap_top来指定起始位置。（详见文献B）

## 3.文献A勘误

文献A的代码中

`printf( " %d " , bitmap.buffer[i * bitmap.width + j] ? 1 : 0 );`

是不准确的，应该改为

`printf( " %d " , bitmap.buffer[i * bitmap.pitch + j] ? 1 : 0 );`

pitch指字模一行所占的字节数，在ft_render_mode_normal模式（即256灰度模式）下，pitch的值和width的值相等。但在ft_render_mode_mono模式（即黑白模式）下，这两个值一般就不等了。

黑白模式下，这句应为

`printf( " %d " , (bitmap.buffer[i * bitmap.pitch + (j>>3)]<<(j & 0x7 )) & 0x80 );`

## 4.小尺寸字体

并不是所有的矢量字库都包含小字体的，例如微软的宋体就不支持小于20*20的字模，所以，使用小尺寸字体时，必须仔细选择字库。

# Qt

## Qt主循环

这篇文章有个大概的比较和说明：

http://blog.chinaunix.net/uid-20754930-id-1877623.html

以下是我针对Qt5代码（2015.4）的一个研究。

QApplication::exec

QGuiApplication::exec

QCoreApplication::exec

QEventLoop::exec

QEventLoop::processEvents

QEventDispatcher::processEvents

再以下就和具体的平台相关了。

Windows平台：

QEventDispatcherWin32::processEvents

它的实现主要是GetMessage+WaitForMultipleObjects，如上文中对MiniGUI的主循环所述。

Unix平台：

QEventDispatcherUNIX::processEvents

QEventDispatcherUNIXPrivate::doSelect

这个函数主要使用select系统调用来监听事件源。

Android平台：

QAndroidEventDispatcher::processEvents

QUnixEventDispatcherQPA::processEvents

QEventDispatcherUNIX::processEvents

以下和Unix平台相同。

# FFmpeg

# 官网 & 安装

http://www.ffmpeg.org/

这是FFmpeg的官网。

值得注意的是，在官网的下载页面默认下载的是源代码。对于不需要源代码的使用者来说，需要在左下角的“Get the Package”处，根据所用平台选择合适的安装包。

安装包分为三类：

1.Static。只有一个可以自解压的exe文件。

2.Shared。相当于要用解压工具来解压的压缩文件。

3.Dev。相关链接库的的头文件和链接文件。

一般没有二次开发需要的话，下载Shared包是最佳的选择。

# 工具的使用

Fabrice Bellard是我崇拜的一位高人。他除了发明ffmpeg之外，还是Qemu和TinyCC的作者和圆周率计算记录的获得者（2009）。

之前的想法，ffmpeg主要是一套编解码框架，其本身的功能有限，需要进行二次开发方可使用。没想到其实它自带的程序，功能已经相当强大了。

安装包中包含三个命令行程序：

1.ffmpeg。编解码工具。功能十分强大，使用它可以很轻松的改变视频文件格式或者压缩视频文件。不过也因为视频格式及编辑选项实在太多，以至于想给ffmpeg做一个好用且功能齐全的GUI外壳都不是件容易的事情。

2.ffplay。播放工具。

3.ffprobe。多媒体流分析工具。

# 教程

官方教程《FFmpeg Basics》，在CSDN可以下载到。但该书偏重于如何使用ffmpeg的命令行工具以及编解码的基本流程。对于ffmpeg源代码，以及如何使用ffmpeg做二次开发讲的很少。

http://dranger.com/ffmpeg/

这篇文章是使用ffmpeg做二次开发的入门手册，写的不错。特将要点翻译摘录如下：

# SDL

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/HelloSDL

目前网上查到的中文教程，多是针对SDL v1.2的。至于SDL v2.0的例子，Github上已经有不少了，可惜多是英文，查找起来还是不太方便。因此这里我也提供一个自己写的SDL v2.0的Hello World代码。可以用这个代码确认SDL v2.0的环境搭建是否正确。