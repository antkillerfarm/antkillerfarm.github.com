---
layout: post
title:  Javascript（三）
category: language 
---

* toc
{:toc}

# ES6（续）

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

----

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

## JS细节

### onclick与href='javascript:function()'的区别

1.onclick事件会比href属性先执行。

2.`<a href="javascript:void(0);" onclick="function()"></a>`或者`<a href="javascript:;" onclick="function()"></a>`表示这个链接不跳转，而执行一段js脚本。

### import js的时机

和C/Java/Python等不同，js的加载并不一定需要在html的开头，而可以在任意位置。有些特效甚至必须在布局元素之后，再加载。

比如D3的绘图代码，必须出现在需要绘图的div之后。否则d3.select之类的操作选择不到任何对象。

### 如何在HTTPS里调用HTTP资源

浏览器默认是不允许在HTTPS里面引用HTTP资源的，一般都会弹出提示框，用户确认后才会继续加载，用户体验非常差。

对于同时支持HTTPS和HTTP的资源，引用的时候要把引用资源的URL里的协议头去掉，例如：//www.example.com/scirpt.js，这样相当于相对路径，即浏览器会自动根据当前是HTTPS还是HTTP来给资源URL补上协议头的，可以达到无缝切换。

## Web Animation API

教程：

danielcwilson.com/blog/2015/07/animations-intro/

Web Animation API目前已经被大多数主流浏览器支持，对于不支持的，可以使用如下库，进行兼容调用：

https://github.com/web-animations/web-animations-js

参考：

https://zhuanlan.zhihu.com/p/27570643

CSS Animations vs Web Animations API

# JS引擎

目前主要有Chrome使用的V8引擎和Safari使用的JavaScriptCore引擎。

https://www.cnblogs.com/meituantech/p/9528285.html

深入理解JSCore

https://zhuanlan.zhihu.com/p/55479666

JavaScriptCore全面解析

# JS特效资料

https://github.com/wagerfield/flat-surface-shader

这个网站提供一种浮动的多边形表面的特效，适合作为登录页的背景。

https://mp.weixin.qq.com/s/p8ll1R9aXc5aELtL3MAEcA

用H5打造用户专属的3D机房（WebGL）

https://github.com/hustcc/canvas-nest.js

动态线条特效

https://mp.weixin.qq.com/s/0dJeYLuXkmJpqvuNHx7L8A

爆红Github！20多个练手前端小型项目的集合，随你造！

# Javascript参考资源

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
