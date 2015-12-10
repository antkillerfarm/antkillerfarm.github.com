---
layout: post
title:  linux内核研究（一）, GStreamer插件
category: technology 
---

# 驱动开发

推荐入门读物《Beginning Linux Programming》，该书第3版已有中译本。

但第3版中的例子在2.6以后的新内核中不能编译。经研究发现，由于新版内核采用KBuild系统编译内核，所以驱动也必须使用KBuild系统编译。

驱动开发的头文件可以在/usr/src下找到。

进阶读物有：

《LINUX设备驱动程序》

《Linux设备驱动开发详解》

# 驱动开发和内核开发的联系与区别

驱动是内核的一部分，驱动开发工程师所需的技能，和内核开发工程师相差无几。但从工作内容来说，两者还是有较大的差异。

驱动开发偏重于利用内核的现有驱动架构，给内核添加新的硬件支持，而内核开发，则主要是对系统架构进行修改，相当于为驱动开发提供弹药。因此，从这个意义上来说，内核的开发更为困难，国内很少有这方面的人才。

# 永不返回的函数（never return function）

了解C语言的人都知道一个函数的最后一个语句通常是return语句。编译器在处理返回语句时，除了将返回值保存起来之外，最重要的任务就是清理堆栈。具体来说，就是将参数以及局部变量从堆栈中弹出。然后再从堆栈中得到调用函数时的PC寄存器的值，并将其加一个指令的长度，从而得到下一条指令的地址。再将这个地址放入PC寄存器中，完成整个返回流程，接着程序就会继续执行下去了。

对于返回值是void类型，也就是无返回值的函数，保存返回值是没有意义的，但它仍然会执行清理堆栈的操作。

以上提到的这些，基本上适用于99.99%的场合。但凡事无绝对，在一些特殊的地方，例如操作系统内核中的某些函数，就不见得符合上边所说的这些。永不返回的函数就是其中之一。

在Linux源代码中，一个永不返回的函数通常拥有一个类似如下函数的声明：

` NORET_TYPE void do_exit(long code)`

考虑到NORET_TYPE的定义：

`#define NORET_TYPE    /**/`

因此，NORET_TYPE在这里仅仅起到方便阅读代码的作用，而并没有什么其他的特殊作用。

看到do_exit函数，可能熟悉Linux内核的朋友已经猜出永不返回的函数和普通函数有什么区别了。没错，do_exit函数是销毁进程的最后一步。由于进程已经销毁，从进程堆栈中获得下一条指令的地址就显得没有什么意义了。do_exit函数会调用schedule函数进行进程切换，从另一个进程的堆栈中获得相关寄存器的值，并恢复那个进程的执行。因此do_exit函数在正常情况下是不会返回的，一个调用了do_exit函数的函数，其位于do_exit函数之后的语句是不会执行到的。因此那个函数也成为了永不返回的函数。

# Linux链表实现

数据结构课本上教链表的时候，一般是这样定义链表的数据结构的：

{% highlight c %}
typedef struct {
    struct Node *next;
    UserData data;
}Node;
{% endhighlight %}

其中，data字段包含了要保存到链表中的数据内容。使用这样的数据结构实现的链表，通用性不好，需要针对不同的UserData类型定义不同的链表类型，尽管所有这些链表的操作都是类似的。当然这样的定义在C++中不是太大的问题，使用模板就可以实现对不同UserData类型的处理，虽然这样做无法避免代码段的膨胀，但是仅就书写使用来说，并没有太大的不方便。

一种改进的办法是将数据结构改为下面的样子：

{% highlight c %}
typedef struct {
    struct Node *next;
    void* data;
}Node;
{% endhighlight %}

用无类型的指针指向需要保存的数据内容，是一个通用性不错的办法。但是C语言本身没有对元数据的支持，一旦指针退化成无类型的指针，再想恢复成原来的数据类型就比较困难了。（元数据就是所有数据类型的基类，例如Java语言的Object类、MFC的CObject类、GTK的GObject结构。虽然元数据本身并不要求包含数据的类型信息，但在上述这些元数据的实现中，都提供了这个功能。）

Linux的做法是：（为了便于理解，进行了一些改写，以忽略与本话题无关的部分）

{% highlight c %}
typedef struct {
    struct Node *next;
}Node;
typedef struct {
    Node *node;
    UserDataActual data;
}UserData;
{% endhighlight %}

这实际上是一种逆向思维，也就是将链表结点中包含用户数据，改为用户数据中包含链表结点。在链表处理时，将node传给链表处理函数。而在引用用户数据时，通过计算node和data的地址偏差，获得data的实际地址。具体的技巧如下：

`UserDataActual* p_data = (UserDataActual*)(((char*)node) - (int)(&(((UserData*)0)->node)) + (int)(&(((UserData*)0)->data)));`

可以看出，这种实现方式对node在UserData中出现的位置也没有什么额外的要求，有很好的灵活性。

# 同步锁

read-write lock、RCU lock、spin lock

# 内核模块的参数

用户模块可以通过main函数传递命令行参数。而内核模块也有类似的用法：

`insmod module.ko [param1=value param2=value ...]`

为了使用这些参数的值，要在模块中声明变量来保存它们，并在所有函数之外的某个地方使用宏`MODULE_PARM(variable, type)`和`MODULE_PARM_DESC(variable, description)`来接收它们。

# IO操作

readb 从 I/O 读取 8 位数据 ( 1 字节 )

readw 从 I/O 读取 16 位数据 ( 2 字节 )

readl 从 I/O 读取 32 位数据 ( 4 字节 )

writeb(), writew(), writel()也是类似的。

IO操作之所以用宏实现，是由于这是和具体机器相关的操作，有的甚至要用到汇编来实现。从计算机体系结构来说，IO空间可以和内存空间属于同一个地址空间，这样就无需特殊的指令，直接使用C语言的赋值语句即可达到效果。IO空间也可以和内存空间采用不同的地址空间（比如x86就是这样的），这时就需要特殊处理了。

# 内核模块的条件编译

内核代码除了可以采用C语言的预处理命令，进行条件编译之外。还可以在.o文件一级，实现条件编译。

例如，在Kbuild系统的Makefile中：

`obj-y += foo.o`

该例子告诉Kbuild在这目录里，有一个名为foo.o的目标文件。foo.o将从foo.c或foo.S文件编译得到。obj-y表示编译进内核，obj-m表示编译成内核模块。

将上面的例子稍微改一下：

`obj-$(CONFIG_FOO) += foo.o`

这里的`$(CONFIG_FOO)`可以为y(编译进内核) 或m(编译成模块)。如果CONFIG_FOO不是y 和m,那么该文件就不会被编译联接了。通过控制`$(CONFIG_FOO)`的值，即可实现.o文件一级的条件编译。

# GStreamer插件

## 概况

GStreamer本质上只是一个多媒体应用框架，具体的多媒体播放功能由插件来完成。

http://gstreamer.freedesktop.org/documentation/plugins.html

这个网页就是gstreamer的插件列表。表中列出的插件，分属4个不同的插件集：

* gst-plugins-base。这类插件格式规范，维护的也很好。

* gst-plugins-good。这类插件有高质量的代码（但格式未必规范），而且许可证也符合要求（LGPL或与LGPL兼容的许可证）。

* gst-plugins-ugly。这类插件有高质量的代码，但许可证方面有问题。

* gst-plugins-bad。这类插件尚不成熟，需要更多文档、测试和应用。

插件安装方法，以gst-plugins-base为例。

`sudo apt-get install gstreamer1.0-plugins-base`

除了上面列出的插件之外，目前的做法，更倾向于使用ffmpeg作为后端编解码库，尤其是编解码更复杂的视频文件。因此在0.10.x时代，提供了gstreamer0.10-ffmpeg插件，而1.x时代，则有gstreamer1.0-libav提供对avcodec、avformat等ffmpeg库的支持。

## 万能插件

GStreamer除了那些完成具体功能的插件以外，还有一些抽象的高级插件，如playbin插件。该插件使用了GStreamer的自动加载（Auto plugging）机制，可以自动根据媒体类型，选择不同的管道播放，相当于是个万能播放插件。对于GStreamer应用开发人员来说，是个相当好用的东西。

playbin插件负责媒体播放的全过程，还有其他一些只负责某个步骤的全能插件：

decodebin：解码插件。

autoaudiosink：音频播放插件

autovideosink：视频播放插件

## 播放视频

播放视频也可以使用playbin插件。这里主要存在以下几个问题：

1）不支持0.10.x系列。由于视频解码主要由ffmpeg来实现，而在Ubuntu14.04以后，官方已经移除了gstreamer0.10-ffmpeg插件，因此很多视频流已经无法处理。

2）xvimagesink错误问题。playbin插件默认使用autovideosink作为视频播放插件，而autovideosink优先使用xvimagesink插件。这个插件的优点是使用了硬件加速的功能，缺点是需要显卡驱动的支持。因此，无论在真实机器还是虚拟机上，都有显卡驱动不匹配，从而导致错误的问题。

这时可以换个思路，自己构建播放视频的管道，其核心是使用ximagesink替代xvimagesink。ximagesink是一个兼容性较好的videosink，缺点是速度没有xvimagesink快。

以下是一个播放视频文件的示例：

`gst-launch-1.0 filesrc location=1.avi ! decodebin name=dmux dmux. ! queue ! audioconvert ! autoaudiosink dmux. ! queue ! videoconvert ! ximagesink`

示例中的`dmux`是个用于定位的标记，两个`dmux.`实际上都指向同一个位置，这样就可以在一个线性的字符串中，表示若干个流水管道。这种表示法多用于多媒体流的分路和合路处理。标记的名称是无所谓的，将`dmux`改为其他字符串，并不影响管道的实际含义。

## 核心插件

核心插件又称gst-plugins-core，目前已经集成到gstreamer代码中，没有独立的库。

和上面提到的四类插件不同。它不提供具体的多媒体编解码功能，而是配合框架，搭建完整的多媒体流水线。核心插件相关的资料参见：

http://gstreamer.freedesktop.org/data/doc/gstreamer/head/gstreamer-plugins/html/

这里仅对其中一部分插件的功能描述如下：

fakesink：一个数据只进不出的“黑洞”。例如，一个视频文件一般包括视频流和音频流。如果设备只能播放音频（例如音箱），那么视频流对于设备来说，就是没有意义的东西。这时可以用fakesink插件将之吃掉。否则，由于GStreamer会在视频流和音频流之间进行同步，如果视频流没有被消耗，音频流也无法向前进。

tee：1路分成N路的分路器。与之相对应的是funnel：N路合成1路的合路器。

