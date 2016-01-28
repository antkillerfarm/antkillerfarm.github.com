---
layout: post
title:  linux内核研究（一）
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

# GPIO

GPIO相对来说是最简单的一类驱动，代码在drivers/gpio文件夹下。

从硬件来说，GPIO有两种输出模式：

push-pull模式电平转换速度快，但是功耗相对会大些。

open-drain模式功耗低，且同时具有“线与”的功能。（同时注意GPIO硬件模块内部是否有上拉电阻，如果没有，需要硬件电路上添加额外的上拉电阻）

可在系统的/sys/class/gpio路径下查看当前的GPIO设备。

参考文献：

http://blog.csdn.net/mirkerson/article/details/8464290

http://www.cnblogs.com/lagujw/p/4226424.html

http://www.aichengxu.com/view/18529

http://wenku.baidu.com/view/a6c4b6bfc77da26925c5b001.html?re=view

## 从gpio_set_value到寄存器操作

include/linux/gpio.h: gpio_set_value

include/asm-generic/gpio.h: __gpio_set_value

drivers/gpio/gpiolib.c: gpiod_set_raw_value

这里会调用gpio_chip结构的set函数指针。我们只需要在定义gpio_chip结构的时候，将寄存器操作函数设置到set函数指针中即可。gpio_chip结构可在模块初始化阶段，使用gpiochip_add函数添加到系统中。

## 输入事件处理

1.轮询方式

drivers/input/keyboard/gpio_keys_polled.c中提供了轮询方式的输入事件处理。该实现主要是注册了一个名为gpio-keys-polled的input_polled_dev。而input_polled_dev则利用queue_delayed_work实现任务的调度。

2.中断方式

drivers/input/keyboard/gpio_keys_polled.c中提供了中断方式的输入事件处理。该实现最终调用了request_irq注册中断处理函数。

无论是轮询还是中断，处理的结果最终都会调用input_event函数生成应用层可访问的事件。

代码的编译配置在：

{% highlight text %}
Device Drivers --->
Input device support --->
Keyboards --->
<*> GPIO Buttons
<*> Polled GPIO buttons
{% endhighlight %}

## GPIO的使用方向

除了gpio_direction_input和gpio_direction_output之外，devm_gpio_request_one和gpio_request_one也可用于设置使用方向，只要设置好flag参数即可。内核中的leds-gpio和gpio-keys模块都是使用后面的方法设置使用方向的。

此外，在board级的GPIO实现中，需要注意以下几点：

1.是否有单独的寄存器用于设置使用方向。这个问题与具体的硬件有关，有的硬件可根据赋值语句的方向，自动切换GPIO的使用方向。

2.如果有单独的设置使用方向的寄存器的话，需要在gpio_set_value和gpio_get_value函数的实现中，将使用方向的设置操作添加进去。I2C的algo代码并不会在set或着get操作时，修改GPIO的使用方向。

## active_low

 active_low的设置要根据硬件的连接，如果按下按键为高电平那么active_low =0，如果按下按键为低电平那么active_low =1.如果这个参数搞错了，按键松开后就不断发按键键码，表现为屏幕上乱动作。

也因为active_low的存在，input_event返回的value实际上并不是GPIO的值，1表示按键按下，0表示按键抬起。

# 内核重启

include/reboot.h里总有一个函数可以满足你的需要。

