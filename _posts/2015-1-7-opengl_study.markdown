---
layout: post
title:  OpenGL研究, GUI框架分析, 游戏开发
category: technology 
---

* toc
{:toc}

# OpenGL研究

https://www.khronos.org/opengl/wiki/History_of_OpenGL

OpenGL的历史

RISC/UNIX图形工作站，运行X Window System，提供Motif库，运行CDE桌面环境，通过PEX接口（有OpenGL，就有GLX，同理有PHIGS就有PEX。），调用PHIGS库，实现三维CAD应用。

Motif有一个竞争对手叫OPEN LOOK。两者之间的竞争史堪比今天的gtk与Qt。

后来SGI在自家的图形工作站上，开发出了IrisGL。IrisGL标准化后，称为OpenGL。

## 书籍

我手上其实有几本关于OpenGL的实体书，但是比较了一下之后，发现还是电子版的《OpenGL编程指南》（俗称OpenGL红宝书）写的更好一些。该书目前已经出到第8版，我看的是第7版中文版的电子版。该书的官网是:

http://www.opengl-redbook.com

可以从上面获得书中例子的源代码。

## 教程

书籍偏重理论，而教程偏重实践。这里推荐Nate Robins的教程，该教程在上面的书中也多次提及过。它的网址是：

http://user.xmission.com/~nate/opengl.html

该教程和上面的书籍一样，都是基于GLUT库的，因此代码的移植性相当好。

教程中示例程序的操作非常简单，基本没什么好说的。唯一需要注意的是，点击鼠标右键会弹出菜单。而且右边屏幕的菜单不可点击，否则程序会退出，但可以用菜单上标注的键盘快捷键选中。这些都是看源代码之后，才发现的。

---

https://learnopengl.com/

https://learnopengl-cn.github.io/

---

https://zhuanlan.zhihu.com/p/447324756

OpenGL笔记

## Tool Kit

![](/images/img4/opengl.png)

GLAD：Multi-Language GL/GLES/EGL/GLX/WGL Loader-Generator

简单说glad是glew的升级版。

---

OpenTK提供了OpenGL, OpenGL ES, OpenAL, OpenCL等库的C#语言的绑定。

官网：

https://opentk.net/

## 环境准备

### Ubuntu

主要需要这几个包：

1.freeglut3-dev。最经典的跨平台OpenGL工具包。书籍和教程都用且只用了它。

2.libglew-dev。可以用来运行GLSL的相关例子。这个对于OpenGL新特性的支持要强于glut。

3.libglfw-dev。一个glut的替代品。glut适合做demo，而这个可以用来做产品。

4.libepoxy-dev。GTK项目选择的OpenGL库。

## OpenGL vs Direct X

2015.2

这两者的争斗已经有近20年的历史了。我最初知道它们，还是在2003年，跟随同学W学习VC的时候。当时我的判断是由于Direct X集成了开发游戏所需的一系列工具，因此它在PC上将超越OpenGL。后来的情况也差不多是这样的。

不过坦率的说，我对Direct X的了解，仅限于DirectDraw和DirectSound，基本上只够开发一些2D应用。

去年底由于想实现一些特殊的Android特效，才接触到OpenGL。按照我的估计，今后随着移动应用越来越重要，OpenGL的应用前景要好于Direct X。而且这个要不了多久，估计也就是这两年的事情。特此备忘。

---

2022.6

以dx9为代表的端游末期时代。

以gles2为代表的漫长的手游时代。

近几年metal与vulkan崛起的AAA手游时代。

---

通常显卡的功能可以在不同的API之间映射，比如OpenGL 2.x对应DX9，OpenGL 3.x对应DX10，OpenGL 4.x对应DX11，Vulkan对应DX12。

## 概述

![](/images/img4/RenderingPipeline.png)

MVP：Model View project matrices

VBO即vertex buffer object，顶点缓存对象负责实际数据的存储；而VAO即 vertex array object，记录数据的存储和如何使用的细节信息。

## 参考

https://blog.csdn.net/wangdingqiaoit/article/details/51318793

OpenGL学习脚印: 绘制一个三角形

https://zhuanlan.zhihu.com/p/97457249

OpenGL + Qt: 三角形绘制

https://mp.weixin.qq.com/s/APVu9agDJN4VJT8u18N1rQ

移动应用中使用OpenGL生成转场特效

https://keyou.github.io/blog/2020/06/10/commandbuffer/

Command Buffer原理解析

https://zhuanlan.zhihu.com/p/677941516

Vulkan/DirectX/OpenGL的坐标系差异以及实际工程中的处理方案

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

Model-View-Controller：

![](/images/img6/MVC.webp)

除了Model不需要了解Controller与View的细节，相对独立之外，V和C都需要知道对方以及M的细节，耦合较大。

MVVM最早由微软提出来，它借鉴了桌面应用程序的MVC思想，在前端页面中，把Model用纯JavaScript对象表示，View负责显示，两者做到了最大限度的分离。

把Model和View关联起来的就是ViewModel。ViewModel负责把Model的数据同步到View显示出来，还负责把View的修改同步回Model。

![](/images/img6/MVVM.webp)

View不需要了解ViewModel的实现细节，但是会声明自己所需要的数据类型。ViewModel和Model紧耦合。

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

## 其他

Single Document Interface（SDI）

Multiple Document Interface（MDI）

Tab Document Interface（TDI）

https://www.zhihu.com/question/21143701

在macOS中关闭应用窗口，为什么默认设定不是完全退出？

# 游戏开发

https://www.cnblogs.com/AMzz/p/12597808.html

Havok物理引擎不完全指南--从入门到放弃

https://www.zhihu.com/question/43616312

为什么很少有游戏支持场景破坏？是因为技术问题吗？

---

securom是一个过时的保护软件，可以被欺骗注册、脱壳、虚拟机等方法轻易绕过，早已经被放弃开发。

https://zhuanlan.zhihu.com/p/20519697

让“三大妈”头疼的Denuvo是一种怎样的技术？

---

所谓的Voxel Space引擎，说白了就是高度图+Raycast的伪3D方案，适用于描绘起伏不定的山野开阔地形。野外大场景的一个特点是超级扁平化（水平尺度能大到几公里，垂直高度差却只有几十米、百十米）这种情况，BSP很不擅长去负担，树会超级不平衡，效力底下，遮挡关系很少，基于BSP的PVS会很可怜。Voxel在当时可以说是效果惊人，处理野外场景的能力，得到的表现力可以说甩开多边形3D引擎几条街。

杯具的是后面行业的发展是GPU和相关的图形硬件突飞猛进。DF即使是野外场景也毫无优势，能支持的表现力和性能大幅度落后当时主流走多边形路线的FPS。

参考：

https://s-macke.github.io/VoxelSpace/

Voxel Space

https://www.zhihu.com/question/51667350

曾经赫赫有名的FPS射击游戏《Delta Force 三角洲特种部队》为什么没落了？

https://www.zhihu.com/answer/2154627154

上古游戏引擎

---

https://zhuanlan.zhihu.com/p/32462890

学习编程的好方法——控制台游戏

https://zhuanlan.zhihu.com/p/529694839

来，我们用Unity做一个大炮

https://zhuanlan.zhihu.com/p/562570278

像《红警》里的大兵那样找路（上）——全局向量场寻路

https://www.zhihu.com/question/576484859

远距离时游戏模型会退化为简单模型，FPS游戏的远距离狙击是如何判断命中的？
