---
layout: post
title:  Cocos2d-x v3在Qt 5上的移植, lex&yacc, ANTLR
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


# lex&yacc （2014.2）

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

代码示例参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/mylex

# ANTLR

ANTLR—Another Tool for Language Recognition，其前身是PCCTS，它为包括Java，C++，C#在内的语言提供了一个通过语法描述来自动构造自定义语言的识别器（recognizer），编译器（parser）和解释器（translator）的框架。

官网：

http://www.antlr.org/

参考：

http://yuzhouwan.com/posts/55501/

Antlr

https://www.ibm.com/developerworks/cn/java/j-lo-antlr/index.html

使用Antlr开发领域语言

