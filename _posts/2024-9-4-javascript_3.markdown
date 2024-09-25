---
layout: post
title:  Javascript（三）
category: language 
---

* toc
{:toc}

# Javascript

## Traffic Demo

2019.9

最近心血来潮，翻出了本科时代的作业。其中有一个交通仿真的小demo，最早是用Java Applet写的。岂料，现在别说浏览器了，就连专门看这个的AppletViewer在新版的JDK中，都不见踪影了。。。

于是，只好做现代化移植。本来首选JavaFX的，不料刚开始写，就发现JavaFX对于多线程渲染做的很差，而这个Demo正是个多线程的版本。

反正都要大改，还不如直接移植到js上，连编译都省了。

原始版本：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/java/trafic

新版本：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/nodejs/js/traffic

众所周知，js是单线程的，所以这个版本也是单线程的，逻辑稍微复杂了一些。

## JS引擎

目前主要有Chrome使用的V8引擎和Safari使用的JavaScriptCore引擎。

https://www.cnblogs.com/meituantech/p/9528285.html

深入理解JSCore

https://zhuanlan.zhihu.com/p/55479666

JavaScriptCore全面解析

## JS特效资料

https://github.com/wagerfield/flat-surface-shader

这个网站提供一种浮动的多边形表面的特效，适合作为登录页的背景。

https://mp.weixin.qq.com/s/p8ll1R9aXc5aELtL3MAEcA

用H5打造用户专属的3D机房（WebGL）

https://github.com/hustcc/canvas-nest.js

动态线条特效

https://mp.weixin.qq.com/s/0dJeYLuXkmJpqvuNHx7L8A

爆红Github！20多个练手前端小型项目的集合，随你造！

## WebAssembly

WebAssembly，简称Wasm，是一种能在浏览器上执行的二进制虚拟机字节码。

官网：

https://webassembly.org/

参考：

https://www.ibm.com/developerworks/cn/web/wa-lo-webassembly-status-and-reality/index.html

WebAssembly现状与实战

https://www.jianshu.com/p/bff8aa23fe4d

几张图让你看懂WebAssembly

https://www.zhihu.com/question/31415286

如何评论浏览器最新的WebAssembly字节码技术？

https://mp.weixin.qq.com/s/LRGNOuFwHXALs_lhPyN3Zw

爱奇艺直播WebAssembly优化之路

https://mp.weixin.qq.com/s/I0grz0IfFZo98sfaCNh-Vw

可能是世界上最简单的用Go来写WebAssembly的教程

https://mp.weixin.qq.com/s/yTebereUkcyvwJbLssZjPQ

5分钟看懂WebAssembly

https://mp.weixin.qq.com/s/QDjxfoY7gjcCmOeV0kq1pw

web多媒体技术在视频编辑场景的应用

https://mp.weixin.qq.com/s/G3NGVV9wSHMMtFtq0MB6SA

快速上手WebAssembly应用开发：Emscripten使用入门

https://zhuanlan.zhihu.com/p/25800318

WebAssembly系列（一）生动形象地介绍WebAssembly

https://zhuanlan.zhihu.com/p/25669120

WebAssembly系列（二）JavaScript Just-in-time (JIT)工作原理

https://zhuanlan.zhihu.com/p/25718411

WebAssembly系列（三）编译器如何生成汇编

https://zhuanlan.zhihu.com/p/25754084

WebAssembly系列（四）WebAssembly工作原理

---

V8开发者Vyacheslav Egorov和Mozilla Hacks之间的source-map之战了。对于同一份source-map库的JS代码，Vyacheslav所魔改出的纯JS版本，其性能一举反超了Mozilla重写的Rust版。

https://www.zhihu.com/answer/1322391162

有没有让JavaScript在JS引擎上稳定、更快运行的Style Guide?

## Javascript在客户端的使用

Javascript在服务器前端的成功，促使人们思考其在客户端的使用。

最早的尝试，是MS提供的web broswer控件（例如MFC的CHtmlView类）。然而，当时的目的，并不是美化应用程序外观，而只是给程序提供一个访问互联网的机会。其最常见的用处，就是给About添加一个网站链接。这种方式不光用途简陋，更关键的是从外观来看，网页和应用程序完全是两种风格。

网站的外观在随后的几年中进化的很快，由于CSS和Javascript的出现，网页前端不再是一成不变的静态网页，而是具有了一定的动画和交互能力。强大的功能促进了分工的发展，网站设计逐渐分成了前端和后端两大工种。这种分工又促进了网页交互技术的进步。

反观普通的应用程序，由于受限于编程的复杂度，前端人员一直难于介入，很多年都处于停滞阶段。这期间一些不甘平庸的公司，在UI技术方面也做了一些尝试。

首先是DirectUI。这个是MS对于Win32窗口模型的一个重大改进。

在原始的Win32窗口模型中，每个控件都是一个窗口，拥有一个窗口句柄（相当于窗口资源的描述符）。窗口事件的处理和资源管理都在OS层面进行，开销比较大。（比如包含10000个按钮的窗口怎么处理的问题）窗口之间的交互，比如透明、动画，也由于需要跨窗口句柄，而变得非常复杂。

DirectUI的思路，是将控件降级为贴图，并接管整体窗口事件的处理，以模拟的方式实现控件的行为。开销和扩展性得到了很大的提升。

DirectUI技术最早出现在Windows XP中。比如，“我的电脑”左侧的控制面板。由于它的HWND的名字叫做DirectUI，故名。GTK项目实际上也采用了类似的方案。

各种DirectUI技术，普遍引入了UI配置文件的概念，而且UI配置文件的功能也越来越强。比如，GTK的设计器Glade，早期的时候是根据UI设计，导出代码，但现在已经改为导出UI配置文件了。

然而，由于低层实现的限制，这些UI配置文件语法各异，虽然有设计器来简化设计难度，但注定不能做的太复杂。因此，功能上无论如何都无法与网页相比，更不必说和HTML 5相比了。

2012年以后，以CEF（Chromium Embedded Framework）和XULRunner为代表的浏览器派，开始逐渐崭露头角。从此，开发桌面应用程序，不再是Javascript的禁区。桌面应用UI和网页前端开始呈现融合的局面。

## TypeScript

TypeScript是JavaScript的一个超集，而且本质上向这个语言添加了可选的静态类型和基于类的面向对象编程。

官网：

https://www.typescriptlang.org/

安装：

`npm install -g typescript`

TypeScript文件的后缀是`.ts`，它不能直接在浏览器中运行，而需要编译成JS才能运行。也正因为如此，TS并没有什么兼容性问题。

编译：

`tsc helloworld.ts`

## 框架

React、Angular、Vue.js似乎是目前最流行的三个框架了。

https://zhuanlan.zhihu.com/p/35551654

基于React的高质量坦克大战复刻版

https://mp.weixin.qq.com/s/zfOZAmgpndcqgxHHaS1j3g

用Vue和React构建相同应用程序，区别在哪？

https://mp.weixin.qq.com/s/zDt92-5NVaAFZJRfEHCUvg

什么是Native、Web App、Hybrid、React Native和Weex？

https://mp.weixin.qq.com/s/RC3eVNfYeVx4mQT3WQHMcw

6个常用的React组件库

https://zhuanlan.zhihu.com/p/28305975

Vue.js&jQuery比较

https://zhuanlan.zhihu.com/p/20197803

VUE（现代库） VS jquery（传统库）

https://zhuanlan.zhihu.com/p/359540593

2021年Angular vs. React vs. Vue前端框架对比

## pdf.js

PDF.js由Mozilla提供支持。目标是创建一个通用的、基于Web标准的平台，用于解析和呈现PDF。

官网：

https://mozilla.github.io/pdf.js/

## Deeplink

“Deeplink”又名“深度链接”，是一种能将用户直接从网页带到App指定页面的技术。

https://zhuanlan.zhihu.com/p/394363004

深度链接(Deeplink)的实现与使用

# Javascript参考资源

https://mp.weixin.qq.com/s/OlnIqu5JvO2WjIA5re9KIg

JavaScript引擎深入剖析(一)：JSValue的内部实现

https://mp.weixin.qq.com/s/sqW3HlCTV5Yivwi_B1Rzww

这个文件下载问题难住了我至少三位同事

https://mp.weixin.qq.com/s/SdtEmzTwdrW_g14QBEgcrQ

JavaScript中哪一种循环最快呢？

https://mp.weixin.qq.com/s/7CD_F0hEZtYRK0fvBWb_gQ

从0到1实现浏览器端沙盒运行环境

https://hllvm-group.iteye.com/group/topic/37596

各JavaScript引擎的简介，及相关资料/博客收集帖

https://mp.weixin.qq.com/s/rDAAfeQmUhxaHkrt_zVCCg

如何避免JS内存泄漏？

https://zhuanlan.zhihu.com/p/438734215

揭秘Chromium渲染引擎（一）：RenderingNG

https://zhuanlan.zhihu.com/p/22989691

JavaScript世界万物诞生记

https://mp.weixin.qq.com/s/F5jCHDzgj1YSJBGrEi-RfA

JavaScript性能优化的小知识总结

https://mp.weixin.qq.com/s/WS1hQN5SmK5uavT_0fbrkg

一文说透为什么JavaScript最牛逼

https://mp.weixin.qq.com/s/OwJ2gBWvmj8rJ_vW5nzaPA

10个免费好用功能强大的网页动画效果库

https://mp.weixin.qq.com/s/pYtKpfL68lEy9bus9HHAMQ

Javascript将HTML页面生成PDF并下载

https://mp.weixin.qq.com/s/Sjg6jgl1D6IkYgsuQSgFHg

十个最流行的前端CSS库

https://mp.weixin.qq.com/s/DHxEqsTMOyc7pHfmJrfNEg

一篇文章理解JS继承

https://mp.weixin.qq.com/s/tNi5LJmotuXSoHbZhNgPcw

GitHub已完全弃用jQuery，问题是为什么？

https://mp.weixin.qq.com/s/WHh9v3icCc90PwiLyv0Hng

为什么Facebook的API以一个循环作为开头？

https://mp.weixin.qq.com/s/GQ2azFxcmXrY78rTAdxuVA

JS/CSS体积减少了67%，我们是如何做到的？

https://mp.weixin.qq.com/s/c1bMljAx1QWz9QQJX7sHmg

大部分教程不会告诉你的12个JS技巧

https://mp.weixin.qq.com/s/pdOVONHbjfIJPW45nEw1fg

前端本地文件操作与上传

http://chrome.360.cn/test/html5/index.html

一个用于检测浏览器对html 5支持情况的网页

https://www.zhihu.com/question/59578433

为什么现在又流行服务端渲染html？

https://mp.weixin.qq.com/s/aNPAfJIHL14NFtLfRvxUpQ

10万人的大场馆如何“画座位”？

https://www.jianshu.com/p/c8b86b09daf0

函数防抖和节流

https://mp.weixin.qq.com/s/vEbTP1SDP3GW20XAP825jw

一种字体，变成千姿百态艺术字，可尖可圆可开花，隔壁设计师馋哭了

https://mp.weixin.qq.com/s/0pI0F6c-BSLiGdLetQ5qNQ

彻底弄懂浏览器缓存策略

https://mp.weixin.qq.com/s/fEAfuVzfOwKjnTB-mdS5UA

用JS写一个同Excel表现的智能填充算法

https://mp.weixin.qq.com/s/D-XvKCSUCzMGcEz_xWTwqg

现代CSS进化史

https://mp.weixin.qq.com/s/IIWgNvqp0jxcD-J_CikV8w

代码变油画，精细到毛发，这个前端小姐姐只用HTML+CSS，让美术设计也惊叹

https://www.cnblogs.com/zzd0916/p/11977995.html

浏览器工作原理与实践

https://mp.weixin.qq.com/s/MwWC0doO_sp_eRkInbE0hw

今天网站都变成灰色了，这其中是怎么实现的？

https://mp.weixin.qq.com/s/VTULhAjEUNfAph-xkkUTsg

手写一个解析器

https://mp.weixin.qq.com/s/3sYrI9kxgAYLiNT-xavRLw

“秒开”浏览器实现起来有多难？

https://mp.weixin.qq.com/s/LciDtj6YmPI7WxcCQM-lIA

前端性能分析工具利器

https://mp.weixin.qq.com/s/c4saBdDZDehokU5gJ-9fPw

JavaScript与ES的25个重要知识点

https://mp.weixin.qq.com/s/HUknNfaxNULc4Yvf5ajRBA

五分钟了解互联网Web技术发展史

https://mp.weixin.qq.com/s/4YXoSrYueQFWOC8sZH8LvQ

跨平台Web Canvas渲染引擎架构的设计与思考

https://zhuanlan.zhihu.com/p/373271928

浏览器性能优化实战
