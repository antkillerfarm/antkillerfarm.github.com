---
layout: post
title:  Cocos2d-x v3在Qt 5上的移植, Matlab
category: technology 
---

* toc
{:toc}

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

可供选择的方案无非两种，或者采用Qt的Qmake，或者采用cocos2d-x的CMake。

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

```bash
.
├── bin
├── build
├── CMakeLists.txt
├── main.cpp
├── mainwindow.cpp
├── mainwindow.h
├── mainwindow.ui
└── qt_cmake.pro
```

实现这样效果的代码参见

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/Qt/qt_cmake

这个目录下的build脚本，讲述了如何使用CMake的方法。需要注意的是，在当前路径下运行`cmake .`和在bin文件夹下运行`cmake ..`的效果是不一样的。

## 例子二

```bash
.
├── bin
├── CMakeLists.txt
└── src
    ├── main.cpp
    ├── mainwindow.cpp
    ├── mainwindow.h
    └── mainwindow.ui
```

实现这样效果的代码参见

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/Qt/qt_cmake1

## 例子三

这是一个qmake转cmake的Qt+OpenGL的示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/Qt/MyOpenGL

遇到cmake问题时，查看具体的命令执行是很有帮助的：

`make VERBOSE=1`

## 例子四

这是一个在上例基础上添加OpenGL Shader实现的例子：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/Qt/opengl_shader

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

# Matlab

![](/images/img4/Matlab.jpg)

在MATLAB软件诞生之前，大约1965年，计算机只能生成和输出二维图像，对于L-型膜结构(L-Shaped Membrane)只能生成下图左侧的二维平面效果。直到1990年，MATLAB 3.5发布，3.5版本利用了隐藏线算法(Hidden Line Algorithm)，可以实现黑白网格的三维立体图像的生成。1993年，MATLAB 4进一步实现了彩色化。1995年，MATLAB 4.2利用了网格光滑化算法(Crude Shaping Algorithm)将L型网格进一步光滑处理。1995年，MATLAB 5实现了生成完全光滑和打光效果。

而Mathworks公司也将他们引以为傲的进步作为了MATLAB软件的图标！这个图标反映的正是Mathworks技术的不断迭代升级。

至于为啥是L-Shaped Membrane而不是其他呢？因为这是Mathworks创始人之一Moler的博士课题

https://www.zhihu.com/answer/1294927798

如果中国重新开发像MATLAB、solidworks这样的软件大概需要多久？

## 参考

https://zhuanlan.zhihu.com/p/30905298

XML和MATLAB交互的基本操作

https://mp.weixin.qq.com/s/QkICCbTp53lWOyeZx63-sw

后MATLAB时代的七种开源替代，一种堪称完美！

https://mp.weixin.qq.com/s/vV8kFF7e1uxjMR48_uRcSw

MATLAB动画没有密秘

## GNU Octave

GNU Octave是Matlab的一个开源实现。它拥有和后者兼容的语法，类似的IDE，并实现了大部分的基础库。

官网：

https://gnu.org/software/octave/

安装方法:

`sudo apt-get install octave`

## spyder

spyder是一个Python的IDE，提供了和Matlab类似的数据可视化界面。

安装：

`sudo apt install spyder`

## GeoGebra

GeoGebra是一个结合“几何”、“代数”与“微积分”的动态数学软件，由佛罗里达州亚特兰大学的数学教授Markus Hohenwarter所设计。

官网：

https://www.geogebra.org/

## Maxima

微积分的数值解很多软件都能计算，但解析解就不行了。Maxima就是这样一款Computer Algebra System的软件。

官网：

http://maxima.sourceforge.net/

# OpenCV+

https://mp.weixin.qq.com/s/HmOiQnkaqxcFPOODnOeUkw

使用OpenCV检测坑洼

https://mp.weixin.qq.com/s/BTmozO6Yr-Jsfm4-YXh2Mg

基于OpenCV的图像梯度与边缘检测

https://mp.weixin.qq.com/s/QYnXiAMFC3k_wQIwiaeWQg

三行代码，OpenCV轻松生成19种色彩风格图像

https://mp.weixin.qq.com/s/4LQBY0rMJk0tU8lF3fgHfQ

基于OpenCV的图像阴影去除

https://mp.weixin.qq.com/s/kH6K6L6-BYnYE-g46WhNCg

在OpenCV中使用色彩校正

https://mp.weixin.qq.com/s/K4P8151BuM4DQPSK-KzPFQ

OpenCV图像旋转的原理与技巧

https://mp.weixin.qq.com/s/hGONFisdtwcF5CRSB9IF6g

图像处理基础：颜色空间及其OpenCV实现

https://mp.weixin.qq.com/s/0Z600IIGscgrREgLUqjLuA

手把手教你用Python做一个图像融合demo
