---
layout: post
title:  OpenGL研究
category: technology 
---

# 书籍

我手上其实有几本关于OpenGL的实体书，但是比较了一下之后，发现还是电子版的《OpenGL编程指南》（俗称OpenGL红宝书）写的更好一些。该书目前已经出到第8版，我看的是第7版中文版的电子版。该书的官网是[http://www.opengl-redbook.com](http://www.opengl-redbook.com)，可以从上面获得书中例子的源代码。

# 教程

书籍偏重理论，而教程偏重实践。这里推荐Nate Robins的教程，该教程在上面的书中也多次提及过。它的网址是：[http://user.xmission.com/~nate/opengl.html](http://user.xmission.com/~nate/opengl.html)。

该教程和上面的书籍一样，都是基于GLUT库的，因此代码的移植性相当好。

教程中示例程序的操作非常简单，基本没什么好说的。唯一需要注意的是，点击鼠标右键会弹出菜单。而且右边屏幕的菜单不可点击，否则程序会退出，但可以用菜单上标注的键盘快捷键选中。这些都是看源代码之后，才发现的。

# 环境准备

## Ubuntu

主要需要这几个包：

1. freeglut3-dev。最经典的跨平台OpenGL工具包。书籍和教程都用且只用了它。

2. libglew-dev。可以用来运行GLSL的相关例子。这个对于OpenGL新特性的支持要强于glut。

3. libglfw-dev。一个glut的替代品。glut适合做demo，而这个可以用来做产品。

4. libepoxy-dev。GTK项目选择的OpenGL库。

# OpenGL vs Direct X

这两者的争斗已经有近20年的历史了。我最初知道它们，还是在2003年，跟随同学W学习VC的时候。当时我的判断是由于Direct X集成了开发游戏所需的一系列工具，因此它在PC上将超越OpenGL。

不过坦率的说，我对Direct X的了解，仅限于DirectDraw和DirectSound，基本上只够开发一些2D应用。

去年底由于想实现一些特殊的Android特效，才接触到OpenGL。按照我的估计，今后随着移动应用越来越重要，OpenGL的应用前景要好于Direct X。而且这个要不了多久，估计也就是这两年的事情。特此备忘。

