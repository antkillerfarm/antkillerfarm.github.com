---
layout: post
title:  linux内核研究
category: technology 
---

# 驱动开发

推荐读物《Beginning Linux Programming》，该书第3版已有中译本。

但第3版中的例子在2.6以后的新内核中不能编译。经研究发现，由于新版内核采用KBuild系统编译内核，所以驱动也必须使用KBuild系统编译。

驱动开发的头文件可以在/usr/src下找到。

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

# Linux源代码编译

1)按照一般的linux教程上的说法，编译的第一步，是配置内核的编译选项。这时有几种方式可以选择:从命令行方式的make config，到基于ncurse库的make menuconfig，再到基于qt的make xconfig和基于GTK+的make gconfig。这让我不得不感叹，即使是内核这样超底层的东西，居然也会用到GUI。Linus也不总是命令行的拥趸。

不过方式虽多，除了make config很不好用之外，其他几个基本上没有什么大的区别。唯一需要注意的是，无论何种方式这一步的目标都是生成.config文件（注意是.config文件，而不是XXX.config文件）。

正是由于有了这一步的存在，定制内核或者说裁剪内核，实际上并没有想象中那么高不可攀。不过编译选项实在是多，好些东西我也不能明白它的真正含义，只能说裁剪过内核，而不敢加上“熟悉”二字。。。

2)接着就是make了，根据机器的给力的程度，这个过程会在十分钟到40分钟之间。最后生成了vmlinux这个内核镜像。

3）最后一步，是安装镜像，这一步由于比较有风险，我还没有实际操作。

# 内核开发心得

* 从最简单的内核模块做起

最近开始研究linux驱动。应该说在驱动领域，我已经有5年以上的工作经验，不算是新手，但是之前的开发要么是在嵌入式内核上，要么就直接是裸机程序，并没有做过真正的linux驱动程序。所以对于这个特定的领域来说，我就是一个新手。

闲话休提，先从最简单的可动态加载的模块说起。

[www.ibm.com/developerworks/cn/linux/l-proc.html](http://www.ibm.com/developerworks/cn/linux/l-proc.html)

这篇文章是我学习的主要参考资料。资料中已经提到的，在此不再赘述。这里仅作补遗之用。

1）makefile文件的名字必须为Makefile，不然会有编译错误。

2）示例代码虽然能够编译通过，但是insmod之后，会报如下的错误：

simple_lkm: module verification failed: signature and/or  required key missing - tainting kernel

解决的办法是在最开头加上

`#include <linux/init.h>`

3）自动加载LKM

参考文献: [http://edoceo.com/howto/kernel-modules](http://edoceo.com/howto/kernel-modules)

以下为节选:

****
Module Configuration Files

The kernel modules can use two different methods of automatic loading. The first method (modules.conf) is my preferred method, but you can do as you please.

modules.conf - This method load the modules before the rest of the services, I think before your computer chooses which runlevel to use

rc.local - Using this method loads the modules after all other services are started
****

从这里可以看出，LKM的加载是要看时机的，如果需要在服务启动之前加载的话，就修改/etc/modules，否则的话,就修改/etc/rc.local。

PS：/etc/modules由/etc/init/module-init-tools.conf 或 /etc/init/kmod.conf负责执行。

例子见[这里](https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/linux_driver/simple-lkm)。

* proc文件系统

继续按照上节的参考资料实践，但是发现create_proc_entry函数老是无法编译通过。于是找到现在版本的内核代码进行研究，发现该函数虽然还在用，但已经被定义为内部函数，且仅有一处用到。

这个过程同时也打开了我的思路——还有什么比内核代码更丰富的例子库呢？不管是proc文件系统，还是普通的设备驱动，在内核代码里例子比比皆是。

因此，有了下面的[例子](https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/linux_driver/simple-vfs)。

这里需要注意的是:

1.proc_simple_vfs_write的返回值不能是0。否则的话，一旦用类似`echo "a">/proc/simple-vfs`这样的方式，向文件写入数据的时候，proc_simple_vfs_write会一直被反复调用。

2.如果想要用类似`cat /proc/simple-vfs`的方式读取文件的话，就需要使用seq_open、seq_open、seq_release、seq_printf等一系列以seq开头的函数。具体的实现可以参照内核中dma.c的代码。

# Uart驱动分析

## Write

1.与应用层的接口

这一层的操作是基于文件的。众所周知，UART属于TTY设备。因此实际执行的函数是tty_write@tty_io.c。

2.tty_ldisc.ops->write

3.n_tty_write@n_tty.c

4.tty_struct.ops->write => tty_operations->write@serial_core.c

5.uart_write@serial_core.c

6.__uart_start@serial_core.c

7.uart_port.ops->start_tx=>uart_ops->write@uartlite.c

这一层以下，就和具体的设备有关了。这里以Xlinux的uartlite为例。

8.ulite_start_tx

9.ulite_transmit

这里已经是具体的寄存器操作了。

## Read

Read的过程要复杂一些，可分为上层调用部分和底层驱动部分。

上层调用部分：

1.tty_read@tty_io.c

2.tty_ldisc.ops->read

3.n_tty_read@n_tty.c

上层调用，到这里为止。这个函数执行到add_wait_queue时，会等待底层驱动返回接收的数据。底层驱动可以是中断式的，也可以是轮询式的。函数会调用copy_from_read_buf，将内核态的数据搬到用户态。

底层驱动部分

1.ulite_startup=>ulite_isr@uartlite.c

这里仍以Xlinux的uartlite为例。初始化阶段注册ulite_isr中断服务程序。

2.ulite_receive@uartlite.c

具体的寄存器操作。

3.tty_flip_buffer_push@tty_buffer.c

4.tty_schedule_flip@tty_buffer.c

调用schedule_work唤醒上层应用。

## select代码分析

1.select@select.c

2.core_sys_select@select.c

3.do_select@select.c

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