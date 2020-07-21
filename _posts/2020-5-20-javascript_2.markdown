---
layout: post
title:  Javascript（二）
category: language 
---

# Javascript

### Step2：回调函数嵌套问题（续）

回调嵌套的解决方法有三种：

1.使用Promise。

2.使用Generator。

3.使用递归函数。

虽然JS递归函数的例子在教程中不太多，但和C语言类似，JS也拥有定义递归函数的能力，且语法也和C类似。这里使用递归函数解决回调嵌套的问题，代码在：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/nodejs/js/hello/super_button2.html

### Step3:延迟动画

除了Step2的办法之外，还可以用设置延迟属性animation-delay的办法，设定动画的播放次序。这种方法的灵活性超过前种方法，但控制难度较高，需要通过公式计算各动画的起始时间，以达到正确的效果。示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/nodejs/js/hello/super_button3.html

这里还解决了Step2的一个Bug。在Step2中，每点击一次按钮，就会添加一次AnimationEnd事件处理函数，但是新添加的函数并不会覆盖原来的事件处理函数。而是在原来的事件处理函数之后，执行新添加的事件处理函数。利用console.log可以很容易的确认这一点。

因此，AnimationEnd事件处理函数仅需要在开始时，添加一次即可。

Step3还添加了fadeOutLeft动画，为了在两种动画之间切换，引入了全局变量flag，作为状态变量。

由于JS事件处理函数，不能传递参数。因此，也就不能很方便的将动画开始播放时的flag值传递过去，留待动画结束时使用。所以，flag值只有在所有动画都结束之后，才能改变。

这里通过添加引用计数的方法，来判定所有动画是否都已结束。不用担心引用计数的临界问题，因为JS是单线程执行的。

此外，还要注意display和opacity的差异，前者如果不显示了，就会彻底消失，而后者不显示时，仍然还会占据它原来所在的位置。

### JS细节

#### onclick与href='javascript:function()'的区别

1.onclick事件会比href属性先执行。

2.`<a href="javascript:void(0);" onclick="function()"></a>`或者`<a href="javascript:;" onclick="function()"></a>`表示这个链接不跳转，而执行一段js脚本。

#### import js的时机

和C/Java/Python等不同，js的加载并不一定需要在html的开头，而可以在任意位置。有些特效甚至必须在布局元素之后，再加载。

比如D3的绘图代码，必须出现在需要绘图的div之后。否则d3.select之类的操作选择不到任何对象。

## JS特效资料

https://github.com/wagerfield/flat-surface-shader

这个网站提供一种浮动的多边形表面的特效，适合作为登录页的背景。

https://mp.weixin.qq.com/s/p8ll1R9aXc5aELtL3MAEcA

用H5打造用户专属的3D机房（WebGL）

https://github.com/hustcc/canvas-nest.js

动态线条特效

## Monaco Editor

Monaco Editor是MS开源的一个在线代码编辑工具，可提供类似VS code的功能。

官网：

https://microsoft.github.io/monaco-editor/

参考：

https://www.cnblogs.com/isaboy/p/monaco-editor.html

微软开源代码编辑器monaco-editor

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

## D3.js

D3.js是一个数据可视化的库。

官网：

https://d3js.org/

参考：

https://www.cnblogs.com/fastmover/p/7779660.html

D3.js从入门到“放弃”指南

## Traffic Demo

2019.9

最近心血来潮，翻出了本科时代的作业。其中有一个交通仿真的小demo，最早是用Java Applet写的。岂料，现在别说浏览器了，就连专门看这个的AppletViewer在新版SDK中，都不见踪影了。。。

于是，只好做现代化移植。本来首选JavaFX的，不料刚开始写，就发现JavaFX对于多线程渲染做的很差，而这个Demo正是个多线程的版本。

反正都要大改，还不如直接移植到js上，连编译都省了。

原始版本：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/java/trafic

新版本：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/nodejs/js/traffic

众所周知，js是单线程的，所以这个版本也是单线程的，逻辑稍微复杂了一些。

## 框架

React、Angular、Vue.js似乎是目前最流行的三个框架了。

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

----

V8开发者Vyacheslav Egorov和Mozilla Hacks之间的source-map之战了。对于同一份source-map库的JS代码，Vyacheslav所魔改出的纯JS版本，其性能一举反超了Mozilla重写的Rust版。

https://www.zhihu.com/question/402807137/answer/1322391162

有没有让JavaScript在JS引擎上稳定、更快运行的Style Guide?

## 多线程编程

Javascript原则上是单线程的，阻塞和其他异步的需求是通过循环来解决的。当线程需要处理大规模的计算的时候，需要使用Web Worker进行多线程操作。

参考：

https://mp.weixin.qq.com/s/87C9GAFb0Y_i5iPbIL5Hzg

Javascript多线程编程​的前世今生

## Flex

http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

Flex布局教程：语法篇

http://www.ruanyifeng.com/blog/2015/07/flex-examples.html

Flex布局教程：实例篇

https://mp.weixin.qq.com/s/9f4UaZWzYSJB_ZdwhS3A3A

40个CSS布局技巧

## 参考

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

https://zhuanlan.zhihu.com/p/35551654

基于React的高质量坦克大战复刻版

https://mp.weixin.qq.com/s/zfOZAmgpndcqgxHHaS1j3g

用Vue和React构建相同应用程序，区别在哪？

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
