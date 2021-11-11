---
layout: post
title:  OpenGL研究, GUI框架分析, uboot
category: technology 
---

* toc
{:toc}

# OpenGL研究

## 书籍

我手上其实有几本关于OpenGL的实体书，但是比较了一下之后，发现还是电子版的《OpenGL编程指南》（俗称OpenGL红宝书）写的更好一些。该书目前已经出到第8版，我看的是第7版中文版的电子版。该书的官网是:

http://www.opengl-redbook.com

可以从上面获得书中例子的源代码。

## 教程

书籍偏重理论，而教程偏重实践。这里推荐Nate Robins的教程，该教程在上面的书中也多次提及过。它的网址是：

http://user.xmission.com/~nate/opengl.html

该教程和上面的书籍一样，都是基于GLUT库的，因此代码的移植性相当好。

教程中示例程序的操作非常简单，基本没什么好说的。唯一需要注意的是，点击鼠标右键会弹出菜单。而且右边屏幕的菜单不可点击，否则程序会退出，但可以用菜单上标注的键盘快捷键选中。这些都是看源代码之后，才发现的。

## Tool Kit

![](/images/img4/opengl.png)

GLAD：Multi-Language GL/GLES/EGL/GLX/WGL Loader-Generator

简单说glad是glew的升级版。

## 环境准备

### Ubuntu

主要需要这几个包：

1.freeglut3-dev。最经典的跨平台OpenGL工具包。书籍和教程都用且只用了它。

2.libglew-dev。可以用来运行GLSL的相关例子。这个对于OpenGL新特性的支持要强于glut。

3.libglfw-dev。一个glut的替代品。glut适合做demo，而这个可以用来做产品。

4.libepoxy-dev。GTK项目选择的OpenGL库。

## OpenGL vs Direct X

这两者的争斗已经有近20年的历史了。我最初知道它们，还是在2003年，跟随同学W学习VC的时候。当时我的判断是由于Direct X集成了开发游戏所需的一系列工具，因此它在PC上将超越OpenGL。后来的情况也差不多是这样的。

不过坦率的说，我对Direct X的了解，仅限于DirectDraw和DirectSound，基本上只够开发一些2D应用。

去年底由于想实现一些特殊的Android特效，才接触到OpenGL。按照我的估计，今后随着移动应用越来越重要，OpenGL的应用前景要好于Direct X。而且这个要不了多久，估计也就是这两年的事情。特此备忘。

# GUI框架分析

## 综合型GUI框架

如MFC、Qt、GTK、Minigui、WxWidget等。这些框架不仅提供窗口管理，而且还提供了丰富的控件，因此从分类学的角度又被称为widget toolkits。除此之外，部分框架还提供了一些非GUI的功能模块，方便用户开发APP。

由于提供了控件，因此控件的绘制和消息在控件之间的分发，就成为了实现的难点。

从流派来看，又可分为两类：

1.以WxWidget为代表的框架，使用native控件及窗口系统。

2.以GTK为代表的框架，采用自绘控件，并管理控件消息的方式。我之前在L公司时，公司产品用的就是这种方案，它的特点是APP在所有平台的外观都是一致的。

## 简易型GUI框架

如glut、SDL等。这类框架仅提供窗口系统的功能，不提供控件。

## 特效型

如cocos2d，clutter等。这类框架构建于前两者之上，提供动画之类的特效。一些诸如圆形按钮、浮动按钮之类的非传统UI特效，在这类框架上实现起来要远比传统框架简单。

## GUI框架的图形渲染

自绘型GUI框架需要自己来处理图形渲染。图形渲染主要包括位图贴图和矢量绘图两部分。

早期框架的图形渲染处理主要有以下几个步骤：

1.申请一块内存缓冲区，按照特定格式组成像素缓冲区。

2.对像素缓冲区进行图形渲染。

3.将像素缓冲区绘制到显示设备上。

其中前两步都是平台无关的操作，只有第3步和硬件实现有关。这也是众多框架能跨平台的奥秘所在。

上面这种方法将图形计算完全放到CPU上。而随着硬件的进步，越来越多的设备配备了专门的GPU。因此现代GUI框架的图形处理步骤变成了这样：

1.申请一块显存缓冲区，按照特定格式组成像素缓冲区。

2.对像素缓冲区进行图形渲染。

3.将像素缓冲区绘制到显示设备上。

而完成这些操作的接口就是OpenGL。

这也是SDL从v1.2升级到v2.0所做的最大的改变。

## 理论

GUI框架主要是个实践派的作品，然而也涉及到了少量的设计理论。

例如，Martin Fowler于2006年所写的《GUI Architectures》一文：

https://martinfowler.com/eaaDev/uiArchs.html

---

MVVM最早由微软提出来，它借鉴了桌面应用程序的MVC思想，在前端页面中，把Model用纯JavaScript对象表示，View负责显示，两者做到了最大限度的分离。

把Model和View关联起来的就是ViewModel。ViewModel负责把Model的数据同步到View显示出来，还负责把View的修改同步回Model。

---

参考：

https://segmentfault.com/a/1190000006016817

GUI应用程序架构的十年变迁:MVC,MVP,MVVM,Unidirectional,Clean

## Immediate Mode GUI

2002年的秋天，在一次分享会上，一位叫CASEY MURATORI的开发者，借用了游戏引擎里常用的"Immediate Mode Rendering"的概念，第一次提出了“Immediate Mode GUI“，传统的GUI则统一称为“Retained Mode GUI“。

IMGUI跟RMGUI最大的区别在于，不存储“额外的“，“重复的”状态。比如TextView的Text属性就是一个“额外的”、“重复的”状态。IMGUI不存储Text信息，而是在需要绘制Text的时候，从原始对象中直接获取。这么做的好处是，TextView显示的文本跟原始对象的Text属性永远是一致的。在RMGUI，原始对象的Text变更之后，需要调用TextView::SetText更新TextView的Text属性，否则就产生了不一致。

缺点：不利于布局/动画/Style配置。造成这些问题的原因是：因为imgui是没有状态的，通常布局算法都需要两遍遍历所有UI组件（第一遍计算大小第二遍计算布局)，但是imgui只会运行一遍这样就会对布局造成困难。同样实现动画和Style都需要额外的状态维护，需要在imgui上添加额外的状态层才能实现动画/Style。

参考：

https://www.zhihu.com/answer/1463984603

玄铁匠的回答

https://www.zhihu.com/question/267602287

如何评价imgui？

http://iki.fi/sol/imgui/

Sol on Immediate Mode GUIs (IMGUI)

https://zhuanlan.zhihu.com/p/36588396

关于Korok的GUI系统

https://blog.codingnow.com/2020/07/game_ui.html

游戏UI模块的选择

# uboot

## 从uboot到Linux

这里以uboot 2014年11月的主线代码为例分析从uboot到linux的全过程。之所以写这篇文章，是由于网上的资料多数都很陈旧，诸如start_armboot之类的函数在新的代码里根本找不到了。由于uboot支持的CPU以及Board非常的多，所以本文仅以Samsung exynos为例来介绍这个过程。

从上电到uboot启动:

1./arch/arm/cpu/armv7/start.S: reset——uboot的汇编入口

2./arch/arm/lib/crt0.S: _main

3./arch/arm/lib/board.c: board_init_f——初始化第一阶段

4./arch/arm/lib/board.c: board_init_r——初始化第二阶段

5./common/main.c: main_loop——uboot主循环

uboot启动Linux

1.uboot中有个bootd的命令选项,执行该命令会进入/common/cmd_bootm.c: do_bootd

2.common/cli.c: run_command，传入bootcmd命令作为参数。

3.common/cmd_bootm.c: do_bootm

4.arch/arm/lib/bootm.c: do_bootm_linux

5.arch/arm/lib/bootm.c: do_jump_linux——跳转到Linux内核的入口地址

uImage格式是专为uboot开发的格式，主要解决了uboot和linux在嵌入式设备的存储上共存的问题。

## uboot命令处理流程

从main_loop到命令处理：

1./common/main.c: main_loop

2./common/cli.c: cli_loop

3./common/cli_simple.c: cli_simple_loop

4./common/cli.c: run_command_repeatable

5./common/cli_simple.c: cli_simple_run_command

6./common/cli_simple.c: cmd_process

7./common/command.c: cmd_call

上面的流程仅是主循环如何调用命令回调函数的过程。下面介绍一下命令是如何声明、存储和查询的。

首先查看链接脚本，uboot使用的链接脚本文件名为u-boot.lds。根据cpu和board的不同，u-boot.lds也有所差异。例如Samsung exynos所用的u-boot.lds在arch\arm\cpu下。

其中有个`.u_boot_list`段就是用来存储命令数据的。它的表述如下所示：

```bash
.u_boot_list : {
		KEEP(*(SORT(.u_boot_list*)));
	}
```

命令的声明，通常使用U_BOOT_CMD宏。这个宏最终展开为：

```bash
_type _u_boot_list_2_##_list##_2_##_name __aligned(4)		\
		__attribute__((unused,				\
		section(".u_boot_list_2_"#_list"_2_"#_name)))
```

这也就是`.u_boot_list*`的来历了。

可以使用/common/command.c: find_cmd函数在命令列表中，根据名称查找命令数据。

## 环境变量

/common/cmd_nvedit.c: setenv--这个函数用于设置环境变量的值。它的原理是：

1.首先在环境变量数组default_environment中，更改相应内容的值。

2.然后调用saveenv，保存default_environment的值，到具体的硬件中。例如NAND设备的代码在/common/env_nand.c中。

在linux内核层面也可以修改uboot的环境变量。通常的做法步骤如下：

1.uboot代码中有个tools/env文件夹。编译改代码可以得到fw_printenv文件。编译的命令是：

`make env`

2.将fw_printenv放到linux系统的/usr/sbin路径下，并创建符号链接fw_setenv。此处的符号链接并不是可有可无的，这里有个编程小技巧：如何用同一个可执行文件执行不同的功能呢？

除了最常用的使用参数区分的方法之外，还可以采用如下方法：

`int main(int argc, char *argv[])`

这是main函数的声明，其中argv是参数数组，而argv[0]是输入的命令本身，因此可以使用这个作为判断依据，来区分不同的用途。这时候符号链接也就派上用场了。

## tftpsrv

有些uboot提供了tftpsrv的功能，用于从网口传输文件（主要是烧写用的镜像文件）。

该tftpsrv默认监听的ip地址保存在uboot的ip环境变量中。如果需要的话，可进行必要的修改并重启。

客户端传输镜像文件时，需要采用二进制模式。命令如下：

`tftp 10.3.9.161 -m binary -c put <file name>`
