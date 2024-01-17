---
layout: post
title:  Javascript（三）
category: language 
---

* toc
{:toc}

# D3.js

D3.js是一个数据可视化的库。原理上可以将之看成是是svg版的jQuery，然后进行相应的dom操作即可。

官网：

https://d3js.org/

参考：

https://www.cnblogs.com/fastmover/p/7779660.html

D3.js从入门到“放弃”指南

https://blog.csdn.net/qq_31052401/article/details/93786425

可视化工具D3教程

# Monaco Editor

Monaco Editor是MS开源的一个在线代码编辑工具，可提供类似VS code的功能。

官网：

https://microsoft.github.io/monaco-editor/

参考：

https://www.cnblogs.com/isaboy/p/monaco-editor.html

微软开源代码编辑器monaco-editor

# 多线程编程

Javascript原则上是单线程的，阻塞和其他异步的需求是通过循环来解决的。当线程需要处理大规模的计算的时候，需要使用Web Worker进行多线程操作。

参考：

https://mp.weixin.qq.com/s/87C9GAFb0Y_i5iPbIL5Hzg

Javascript多线程编程​的前世今生

# Flex

http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

Flex布局教程：语法篇

http://www.ruanyifeng.com/blog/2015/07/flex-examples.html

Flex布局教程：实例篇

https://mp.weixin.qq.com/s/9f4UaZWzYSJB_ZdwhS3A3A

40个CSS布局技巧

# ES6

JS的语言规范被称为ECMAScript标准。在ES6之前，各版本的历史如下：

ES1 1997

ES2 1998

ES3 1999

ES4 Abandoned

ES5 2009

ES6 2015

可以看出ES3是第一个普及版，没有谁不支持。ES4由于和ES3差异过大，而被废弃，从未发布。ES5是第二个普及版，现在（2020.8）即使再考虑兼容性，也就到ES5为止了。

ES6在2018年以后基本普及，新项目如无特殊需要，至少应支持ES6。

ES6加入了若干面向对象开发的特性，使得可以用来编写复杂的大型应用程序。

环境：

https://www.caniuse.com/#search=es6

ES的兼容性概览

教程：

https://www.runoob.com/w3cnote/es6-tutorial.html

从ES7开始，ES标准的制定原则是成文标准要从事实标准中诞生，实现先于标准存在。进入标准草案必须有JavaScript引擎实现的支持（按照流程，起码要2个JavaScript引擎的稳定实现）。发布节奏也改为一年一发。因此，后续标准每版增加的内容非常少。

一般来说，Chrome、Firefox和Node.js是支持的比较及时的JS平台。

---

在ES6之前，JS的面向对象编程通常使用一种叫做“**原型链**”的技术来实现。

参见：

https://www.cnblogs.com/chengzp/p/prototype.html

详谈JavaScript原型链

ES6的新语法，虽然糖了不少，但背后的基础仍然是原型链。

参考：

https://www.cnblogs.com/libin-1/p/7127481.html

彻底搞清楚javascript中的require、import和export

# 动画

HTML动画一般有三种实现方式：

1.JS。JS脚本通过动态改变HTML、CSS的内容来实现动画效果。这种方式功能全面，且可在旧版本浏览器中执行。

2.CSS3。CSS3引入了一些动画属性，它由浏览器直接解释执行，执行效率很高（可利用硬件加速），但需要浏览器本身支持CSS3。并且，有些复杂的动画，可能会超出CSS3的能力范围，这时不可避免的还是会用到JS。

3.Web Animation API。为了解决CSS3的问题，2014年Google又提出了Web Animation API。这套API让JS拥有了不逊于CSS3的性能。

## Animate.css

Animate.css是Daniel Eden使用CSS3的animation制作的动画效果的CSS集合。其官网是：

http://daneden.github.io/animate.css/

教程：

http://www.dowebok.com/98.html

animate.css–齐全的CSS3动画库

## Step1：事件触发动画

网上的CSS动画例子，多数是加载网页时直接触发（这种最简单），少部分是鼠标移动到控件上时触发（这种方式主要使用了:hover选择器）。

这里介绍一下，click事件触发动画的机制。示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/nodejs/js/hello/super_button.html

1.在sb.css中，自定义按钮旋转动画的样式rotate_mill。

2.在click事件处理函数中，使用addClass函数，将rotate_mill应用到控件上，就可以触发动画效果。

3.动画结束时，会触发AnimationEnd事件。在该事件处理函数中，使用removeClass函数，去掉rotate_mill样式，以恢复原状。否则，下次click时，由于样式没变化，就不会触发动画效果了。

4.和AnimationEnd类似的事件，还有AnimationIteration和AnimationStart。

## Step2：回调函数嵌套问题

在上面的例子中，所有的button都是同步动画的。如果想要一个接着一个播放动画的话。一种思路就是：在上一个动画的AnimationEnd事件处理函数中，启动下一个动画。但这种方法会导致回调函数的嵌套问题。

首先需要明确一点：回调嵌套并没有执行效率的问题。JS脚本都是单线程执行的，因此无论采用何种写法，都不会改变函数的执行顺序。回调嵌套的问题主要出在可读性方面。

回调嵌套的解决方法有三种：

1.使用Promise。

2.使用Generator。

3.使用递归函数。

虽然JS递归函数的例子在教程中不太多，但和C语言类似，JS也拥有定义递归函数的能力，且语法也和C类似。这里使用递归函数解决回调嵌套的问题，代码在：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/nodejs/js/hello/super_button2.html

## Step3:延迟动画

除了Step2的办法之外，还可以用设置延迟属性animation-delay的办法，设定动画的播放次序。这种方法的灵活性超过前种方法，但控制难度较高，需要通过公式计算各动画的起始时间，以达到正确的效果。示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/nodejs/js/hello/super_button3.html

这里还解决了Step2的一个Bug。在Step2中，每点击一次按钮，就会添加一次AnimationEnd事件处理函数，但是新添加的函数并不会覆盖原来的事件处理函数。而是在原来的事件处理函数之后，执行新添加的事件处理函数。利用console.log可以很容易的确认这一点。

因此，AnimationEnd事件处理函数仅需要在开始时，添加一次即可。

Step3还添加了fadeOutLeft动画，为了在两种动画之间切换，引入了全局变量flag，作为状态变量。

由于JS事件处理函数，不能传递参数。因此，也就不能很方便的将动画开始播放时的flag值传递过去，留待动画结束时使用。所以，flag值只有在所有动画都结束之后，才能改变。

这里通过添加引用计数的方法，来判定所有动画是否都已结束。不用担心引用计数的临界问题，因为JS是单线程执行的。

此外，还要注意display和opacity的差异，前者如果不显示了，就会彻底消失，而后者不显示时，仍然还会占据它原来所在的位置。

## Web Animation API

教程：

danielcwilson.com/blog/2015/07/animations-intro/

Web Animation API目前已经被大多数主流浏览器支持，对于不支持的，可以使用如下库，进行兼容调用：

https://github.com/web-animations/web-animations-js

参考：

https://zhuanlan.zhihu.com/p/27570643

CSS Animations vs Web Animations API

## 参考

https://mp.weixin.qq.com/s/z-GxbF9SI0v1BJv3RiqjwA

React中的Canvas动画

# JS细节

## onclick与href='javascript:function()'的区别

1.onclick事件会比href属性先执行。

2.`<a href="javascript:void(0);" onclick="function()"></a>`或者`<a href="javascript:;" onclick="function()"></a>`表示这个链接不跳转，而执行一段js脚本。

## import js的时机

和C/Java/Python等不同，js的加载并不一定需要在html的开头，而可以在任意位置。有些特效甚至必须在布局元素之后，再加载。

比如D3的绘图代码，必须出现在需要绘图的div之后。否则d3.select之类的操作选择不到任何对象。

## 如何在HTTPS里调用HTTP资源

浏览器默认是不允许在HTTPS里面引用HTTP资源的，一般都会弹出提示框，用户确认后才会继续加载，用户体验非常差。

对于同时支持HTTPS和HTTP的资源，引用的时候要把引用资源的URL里的协议头去掉，例如：//www.example.com/scirpt.js，这样相当于相对路径，即浏览器会自动根据当前是HTTPS还是HTTP来给资源URL补上协议头的，可以达到无缝切换。

# 小程序

BOM（Browser Object Model）是指浏览器对象模型，它使JavaScript有能力与浏览器进行“对话”。例如查询浏览器的名称和浏览历史等。

DOM（Document Object Model）是指文档对象模型，通过它，可以访问HTML文档的所有元素。

最初微信WebView逐渐成为移动web重要入口，微信发布了一整套网页开发工具包，称之为JS-SDK，给所有的Web开发者打开了一扇全新的窗户，让所有开发者都可以使用到微信的原生能力，去完成一些之前做不到或者难以做到的事情。这就是后来的微信小程序。

![](/images/img4/light_app.jpg)

在小程序中，视图层通常与逻辑层分离。也就是所谓的双线程模型。

它不需要安装，并支持热更新，兼有Web和原生平台的功能，因此受到了一些超级应用程序的欢迎。目前比较流行的小程序平台，除了微信之外，还有支付宝、百度、头条、京东等。

各家的小程序虽然原理大同小异，但是写法有所不同，因此就产生了小程序的跨平台需求。

主流的跨平台小程序框架有：Taro和uni-app。

---

PWA（Progressive Web Apps）：从Google 2015年推出这一技术标准以来，已经有不少应用服务推出了PWA版本应用，来让更多可以运行web网页的设备也能获得类似原生应用的使用体验。PWA可以看作是浏览器版本的小程序。

https://zhuanlan.zhihu.com/p/362663779

全平台的轻量体验：PWA使用指南及应用推荐

---

参考：

https://www.cnblogs.com/liwenzhou/p/8011504.html

前端基础之BOM和DOM

https://zhuanlan.zhihu.com/p/114683001

小程序原理及模拟

https://zhuanlan.zhihu.com/p/142381013

小程序原理科普

https://zhuanlan.zhihu.com/p/66113575

浅谈小程序运行机制

https://zhuanlan.zhihu.com/p/55903320

Taro vs uni-app选型对比

https://mp.weixin.qq.com/s/rPZsFyJkoYjqwxjcMnivOQ

微信小程序开发

https://mp.weixin.qq.com/s/23YB5JyULJ1l08OJ7LeaGg

微信小程序30分钟从陌生到熟悉（上）

https://mp.weixin.qq.com/s/sYm-5jo-batq1fwv_tCmQw

微信小程序30分钟从陌生到熟悉（下）
