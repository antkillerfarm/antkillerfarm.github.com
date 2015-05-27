---
layout: post
title:  Cocos2d-x v3在Qt 5上的移植
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

4.为什么是Qt，而不是其他。现有的cocos2d-x粒子特效编辑器，只有Windows版本，没有Linux版本

https://github.com/ascetic85/quick-cocos2d-x-20130509

http://blog.csdn.net/honghaier/article/details/7897077

http://www.cnblogs.com/shadow21/archive/2012/11/09/2763158.html


# 编译环境



