---
layout: post
title:  linux学习心得
category: technology 
---

## 1.动态链接

这两天实践了一下怎样在linux下创建动态链接。感觉网上的资料虽然翔实，但仍然有疏漏之处。

1）g++和gcc的区别

本来只想给链接的，以显示这不是我的原创。但是现在的链接失效的也太快了。。。囧

只好拿华丽的分隔符来表示引用的内容。

****
误区一:gcc只能编译c代码,g++只能编译c++代码

两者都可以，但是请注意：

1.后缀为.c的，gcc把它当作是C程序，而g++当作是c++程序；后缀为.cpp的，两者都会认为是c++程序，注意，虽然c++是c的超集，但是两者对语法的要求是有区别的。C++的语法规则更加严谨一些。

2.编译阶段，g++会调用gcc，对于c++代码，两者是等价的，但是因为gcc命令不能自动和C＋＋程序使用的库联接，所以通常用g++来完成链接，为了统一起见，干脆编译/链接统统用g++了，这就给人一种错觉，好像cpp程序只能用g++似的。
 
误区二:gcc不会定义__cplusplus宏，而g++会

实际上，这个宏只是标志着编译器将会把代码按C还是C++语法来解释，如上所述，如果后缀为.c，并且采用gcc编译器，则该宏就是未定义的，否则，就是已定义。
 
误区三:编译只能用gcc，链接只能用g++

严格来说，这句话不算错误，但是它混淆了概念，应该这样说：编译可以用gcc/g++，而链接可以用g++或者gcc -lstdc++。因为gcc命令不能自动和C＋＋程序使用的库联接，所以通常使用g++来完成联接。但在编译阶段，g++会自动调用gcc，二者等价。
****

因此，某些时候编不过去，可以试试换换cc的值。

2）gcc4.1.1下似乎对类型检查严了一些，dlsym返回的void*类型不能转换为相应的函数指针类型，需要强制转换。某些网上的例子在这里编不过去。

3）显式调用时,要注意动态库函数的声明,可能要加`extern "C"`才能正常执行。（显式调用是运行时加载，所以编译能过，执行却不对了。）可以用nm命令看看链接库的符号表，以确定问题所在。

## 2.驱动开发

推荐读物《Beginning Linux Programming》，该书第3版已有中译本。

但第3版中的例子在2.6以后的新内核中不能编译。经研究发现，由于新版内核采用KBuild系统编译内核，所以驱动也必须使用KBuild系统编译。

驱动开发的头文件可以在/usr/src下找到。

## 3.GUI设计工具

GTK+的程序可以使用Glade来设计界面，它会生成一个脚本，运行该脚本，并make就行了。

QT的稍微麻烦一些。

1）首先打开QT Designer（新建时，不能使用KDE Designer，但修改建好后的工程可以，不知道是BUG，还是怎么回事）新建一个窗体，然后新建一个main.cpp，并将刚才生成的窗体选为主窗体。

2）打开KDev C/C++，选择导入工程，选择QMake Based导入，然后即可在IDE中编译并运行。

上面写的两个都是官方的设计器，而WxWidgets没有官方的设计器，但有很多第三方的设计器，我使用的是免费且开源的wxFormBuilder。用它可生成XRC文件，而WxWidgets中有使用XRC文件的接口。

## 4.编程所用命令简介

cc：C/C++编译器

as：GNU汇编编译器

ld：链接器

as86、ld86：8086汇编编译器和链接器

linux文件分割用split,合并用cat。最近下了一本采用split分割的书，但是我没有Linux环境，于是在windows的命令行下用type、>、>>合并了文件。

## 5.printf和wprintf混用的问题

在linux中不可混用printf和wprintf，如果混用的话，则**后使用**的函数没有输出。

例如

{% highlight c %}
printf("a\n");
wprintf(L"b\n");
{% endhighlight %}

输出为：

a

而

{% highlight c %}
wprintf(L"b\n");
printf("a\n");
{% endhighlight %}

输出为：

b

关于这个问题的讨论见

[http://bytes.com/groups/c/852681-wprintf-conflicts-printf-glibc-bug](http://bytes.com/groups/c/852681-wprintf-conflicts-printf-glibc-bug)

解决方法统一使用一种函数

例如：

{% highlight c %}
wprintf(L"%s","a\n");
wprintf(L"b\n");
{% endhighlight %}

或

{% highlight c %}
printf("a\n");
printf("%ls",L"b\n");
{% endhighlight %}

## 6.关于SIGPIPE导致的程序退出

[http://www.cppblog.com/elva/archive/2008/09/10/61544.html](http://www.cppblog.com/elva/archive/2008/09/10/61544.html)

## 7.使用yum

在RHEL中，可以使用yum从网上下载相应的组件，但需要RHN号，所以我现在换用了CentOS。当你需要使用yum的时候，如果yum找不到相应的组件时，可以在组件名之前加lib，或者在之后加-dev或-devel。

## 8.源码包编译4步曲

1)autogen.sh

2)configure

3)make

4)su -c "make install"

其中一二两步有时只要一个就够了，如果源码包中这两个都有的话，先运行1)

## 9.永不返回的函数（never return function）

了解C语言的人都知道一个函数的最后一个语句通常是return语句。编译器在处理返回语句时，除了将返回值保存起来之外，最重要的任务就是清理堆栈。具体来说，就是将参数以及局部变量从堆栈中弹出。然后再从堆栈中得到调用函数时的PC寄存器的值，并将其加一个指令的长度，从而得到下一条指令的地址。再将这个地址放入PC寄存器中，完成整个返回流程，接着程序就会继续执行下去了。

对于返回值是void类型，也就是无返回值的函数，保存返回值是没有意义的，但它仍然会执行清理堆栈的操作。

以上提到的这些，基本上适用于99.99%的场合。但凡事无绝对，在一些特殊的地方，例如操作系统内核中的某些函数，就不见得符合上边所说的这些。永不返回的函数就是其中之一。

在Linux源代码中，一个永不返回的函数通常拥有一个类似如下函数的声明：

` NORET_TYPE void do_exit(long code)`

考虑到NORET_TYPE的定义：

`#define NORET_TYPE    /**/`

因此，NORET_TYPE在这里仅仅起到方便阅读代码的作用，而并没有什么其他的特殊作用。

看到do_exit函数，可能熟悉Linux内核的朋友已经猜出永不返回的函数和普通函数有什么区别了。没错，do_exit函数是销毁进程的最后一步。由于进程已经销毁，从进程堆栈中获得下一条指令的地址就显得没有什么意义了。do_exit函数会调用schedule函数进行进程切换，从另一个进程的堆栈中获得相关寄存器的值，并恢复那个进程的执行。因此do_exit函数在正常情况下是不会返回的，一个调用了do_exit函数的函数，其位于do_exit函数之后的语句是不会执行到的。因此那个函数也成为了永不返回的函数。

## 10.Linux链表实现

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

## 11.linux源代码编译

1)按照一般的linux教程上的说法，编译的第一步，是配置内核的编译选项。这时有几种方式可以选择:从命令行方式的make config，到基于ncurse库的make menuconfig，再到基于qt的make xconfig和基于GTK+的make gconfig。这让我不得不感叹，即使是内核这样超底层的东西，居然也会用到GUI。Linus也不总是命令行的拥趸。

不过方式虽多，除了make config很不好用之外，其他几个基本上没有什么大的区别。唯一需要注意的是，无论何种方式这一步的目标都是生成.config文件（注意是.config文件，而不是XXX.config文件）。

正是由于有了这一步的存在，定制内核或者说裁剪内核，实际上并没有想象中那么高不可攀。不过编译选项实在是多，好些东西我也不能明白它的真正含义，只能说裁剪过内核，而不敢加上“熟悉”二字。。。

2)接着就是make了，根据机器的给力的程度，这个过程会在十分钟到40分钟之间。最后生成了vmlinux这个内核镜像。

3）最后一步，是安装镜像，这一步由于比较有风险，我还没有实际操作。

## 12.内核开发心得

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

## 13.关于ascii字符集的一些打印控制字符的别名

\b——backspace

\f——pagebreak

\v——vertical tab

## 14.lex&yacc

* 前言

春节期间，空闲时间较多，于是研究了一下lex和yacc的用法。知道lex和yacc，那还是大四学习编译原理那门课时候的事情了。转眼之间，那已经是十年前的事情了。

编译原理在整个大学期间的专业课中，属于难度比较高的课程。而且如果不是计算机专业的话，基本没有可能学到这门课。当时的课程作业是完成一个支持脚本绘图的软件。其难度即使以我现在的眼光来看，也颇不容易。当时只有少数人能够做出来，但基本上是参考教这门课的老师出的一本教辅书来写的。

这个课程作业之所以复杂，主要在于老师要求词法和语法的分析器都必须要自己编码。如果退一步，可以使用lex和yacc的话，就没有那么困难了。当然这也与大学里以传授理论为主的思想有关，我还是相当认同这一点的。

再顺便说一句，lex的作者之一是google的前CEO Eric Schmidt，这是他20岁时，在贝尔实验室的作品。当然，不全是他的功劳。实际上lex和yacc都是贝尔实验室的作品，这从lex效仿yacc的书写风格就能略见一斑。相比而言，yacc的地位和复杂度更为重要些。

* 前置条件

要想研究lex和yacc，除了需要有C语言的基础之外。还需要对正则式和BNF（Backus-Naur Form）有所了解。顺便提一下，John Warner Backus，FORTRAN、ALGOL语言之父，1977年ACM图灵奖得主。他在中学时代居然是个勉强毕业的差生，在大学里换了两次专业，还是一事无成。。。

* 教材

LEX & YACC TUTORIAL by Tom Niemann——这本书比较简练，且附有代码，入门级的极品

Aho, Alfred V., Ravi Sethi and Jeffrey D. Ullman [2006]. Compilers, Prinicples, Techniques and Tools——这本书是编译原理方面的权威作品，堪称编译原理界的TAOCP，不过篇幅太长了。。。

* 心得

lex生成的代码中，最重要的是yylex函数，该函数每匹配一个词，就返回一次。yacc生成的代码中，最重要的是yyparse函数，这个函数调用yylex函数以获得所需要的语法词汇。

lex的词法分析，依据用法的不同，可分为三类：

1）需要匹配识别的词汇。

2）需要过滤的词汇。一般是空白、TAB之类的分隔符。

3）直译的词汇。就是那些lex不处理，也不吃掉，而是直接交给yacc分析的词汇。

这三类词汇必须仔细规划，因为被解析的文本中，一旦出现不在上述三类的任何一类中的词汇时，程序就会报错。

yacc的BNF中一般都要包括类似下面的语句：

{% highlight c %}
stmt_list:
          stmt                  { }
        | stmt_list stmt        { }
        ;
{% endhighlight %}

其中stmt表示单个语句的语法目标，而stmt_list则是一系列语句的集合。

为什么要添加这一句呢？因为yacc在处理被解析的文本时，如果文本不能最终归结为一个单一的语法目标的时候，程序也会报错。

## 15.从uboot到Linux

这里以uboot 2014年11月的主线代码为例分析从uboot到linux的全过程。之所以写这篇文章，是由于网上的资料多数都很陈旧，诸如start_armboot之类的函数在新的代码里根本找不到了。由于uboot支持的CPU以及Board非常的多，所以本文仅以Samsung exynos为例来介绍这个过程。

从上电到uboot启动:

1./arch/arm/cpu/armv7/start.S:reset——uboot的汇编入口

2./arch/arm/lib/crt0.S:_main

3./arch/arm/lib/board.c:board_init_f——初始化第一阶段

4./arch/arm/lib/board.c:board_init_r——初始化第二阶段

5./common/main.c: main_loop——uboot主循环

uboot启动Linux

1.uboot中有个bootd的命令选项,执行该命令会进入/common/cmd_bootm.c:do_bootd

2.common/cli.c:run_command，传入bootcmd命令作为参数。

3.common/cmd_bootm.c:do_bootm

4.arch/arm/lib/bootm.c:do_bootm_linux

5.arch/arm/lib/bootm.c:do_jump_linux——跳转到Linux内核的入口地址

uImage格式是专为uboot开发的格式，主要解决了uboot和linux在嵌入式设备的存储上共存的问题。

