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

## 2.GUI设计工具

GTK+的程序可以使用Glade来设计界面，它会生成一个脚本，运行该脚本，并make就行了。

QT的稍微麻烦一些。

1）首先打开QT Designer（新建时，不能使用KDE Designer，但修改建好后的工程可以，不知道是BUG，还是怎么回事）新建一个窗体，然后新建一个main.cpp，并将刚才生成的窗体选为主窗体。

2）打开KDev C/C++，选择导入工程，选择QMake Based导入，然后即可在IDE中编译并运行。

上面写的两个都是官方的设计器，而WxWidgets没有官方的设计器，但有很多第三方的设计器，我使用的是免费且开源的wxFormBuilder。用它可生成XRC文件，而WxWidgets中有使用XRC文件的接口。

## 3.编程所用命令简介

cc：C/C++编译器

as：GNU汇编编译器

ld：链接器

as86、ld86：8086汇编编译器和链接器

linux文件分割用split,合并用cat。最近下了一本采用split分割的书，但是我没有Linux环境，于是在windows的命令行下用type、>、>>合并了文件。

## 4.printf和wprintf混用的问题

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

## 5.关于SIGPIPE导致的程序退出

[http://www.cppblog.com/elva/archive/2008/09/10/61544.html](http://www.cppblog.com/elva/archive/2008/09/10/61544.html)

## 6.使用yum

在RHEL中，可以使用yum从网上下载相应的组件，但需要RHN号，所以我现在换用了CentOS。当你需要使用yum的时候，如果yum找不到相应的组件时，可以在组件名之前加lib，或者在之后加-dev或-devel。

## 7.源码包编译4步曲

1)autogen.sh

2)configure

3)make

4)su -c "make install"

其中一二两步有时只要一个就够了，如果源码包中这两个都有的话，先运行1)

## 8.关于ascii字符集的一些打印控制字符的别名

\b——backspace

\f——pagebreak

\v——vertical tab

## 9.lex&yacc （2014.2）

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
