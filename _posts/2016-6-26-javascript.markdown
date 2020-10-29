---
layout: post
title:  Javascript（一）
category: language 
---

* toc
{:toc}

# 参考指南 & 教程

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference

http://www.javascriptkit.com/jsref/

上面两个网址都是Javascript的参考指南，便于查找语法规则和标准函数的用法。

http://www.w3school.com.cn

这是一个中文的参考网站。内容包括HTML、CSS、JS等前端技术，以及其他一些后端技术。

http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000

一个中文入门教程。该作者还编写了Python教程。

http://www.bootcss.com/

这个网站虽然只是Bootstrap的教程网站，然而它首页的项目推荐，几乎涵盖了前端开发所用的各种JS库。

# Javascript和C的互相调用

Javascript本质上是服务器发出的，由客户端执行的脚本。出于安全原因，本地功能比较弱。所谓Javascript和C的互相调用，基本上都依赖于浏览器的实现。比如，在IE中依赖于ActiveX插件，在Firefox中依赖于JSAPI。

# CDN

CDN的全称是Content Delivery Network，即内容分发网络。这里主要使用它来存储一些通用的JS库，比如JQuery，以达到节省带宽和提高加载速度的目的。

以下是一些国内比较好使的CDN地址：

http://lib.sinaapp.com/js/jquery/1.9.1/jquery-1.9.1.min.js

http://libs.baidu.com/jquery/1.9.1/jquery.min.js

http://libs.useso.com/js/jquery/1.9.1/jquery.min.js

这里是百度CDN库的说明：

http://developer.baidu.com/wiki/index.php?title=docs/cplat/libs

# Template Engine

![](/images/article/web.png)

在传统的Web开发模式中，HTML文件由CGI负责生成。然而生成HTML文件本身，就是一件麻烦事。纯用printf之类的方式，显然是一件费时费力的工作。

这时，就需要Template Engine来加速这个过程。Template Engine会将Template Text转换成HTML。因此，只要Template Text的文法比HTML简单，则这个转换就是有意义的。

Template Engine有很多种。例如：

## hbs

https://www.npmjs.com/package/hbs

## jade

http://jade-lang.com/

# jslint

http://www.jslint.com/

jslint是一个JavaScript语法的检查工具。

# UI控件库

## jQuery UI

这是jQuery官方推出的UI库。官网：

http://jqueryui.com/

## jQuerytools

另一个基于jQuery的UI库。

## YUI

Yahoo User Interface library。这是一个大型的JS工具库，已经停止更新及维护。官网：

http://yuilibrary.com/

## Other

Bootstrap、Foundation、Semantic UI。

# node.js

为了在服务器后端使用Javascript，Ryan Dahl发起了node.js项目。这个项目可以看作是php等的竞争者。它的官网为：

https://nodejs.org/

安装方法：

`sudo apt install nodejs-dev npm`

执行：

`node hello.js`

教程：

http://nqdeng.github.io/7-days-nodejs/

这是阿里的技术团队出的一份中文版简易教程。

http://www.runoob.com/nodejs/nodejs-tutorial.html

这是“菜鸟教程”提供的node.js教程。该网站还有一系列的编程语言教程。

https://github.com/tianmaying/node-blog-demo

这是“天码营”提供的node.js实现网站的示例。

https://mp.weixin.qq.com/s/3HoR-PVuwcJFGWEw3X2Y8w

Nodejs探秘：深入理解单线程实现高并发原理

https://mp.weixin.qq.com/s/PkwL5ExoMroYP7kLZYf-MA

如何在Docker容器中调试Node.js应用？

https://mp.weixin.qq.com/s/kX23axjtgPRb64jb1HQLOQ

苏宁的Node.js实践：不低于Java的渲染性能、安全稳定迭代快

https://mp.weixin.qq.com/s/mCOHF2MMIOD4QZJzyfi7Vw

携程机票Node.js开发实践

https://mp.weixin.qq.com/s/uDwX0iq9RWs1sK1ct0tiCg

浅谈Node.js在携程的应用

https://mp.weixin.qq.com/s/L4Vq7BhjuAw7WBY7sV2ICw

有意思的Node.js内存泄漏问题（JS虚拟机）

## NPM

npm是node.js的软件包管理工具。它的官网是：

https://www.npmjs.com/

可用于搜索有用的软件包，避免重复造轮子。

国内镜像：

https://npm.taobao.org

>虽然淘宝镜像号称5分钟同步一次，然而有些时候某些包由于比较大，可能存在只有部分更新的情况。这时可以考虑使用老一点的版本。

npm有两种安装方式：

1.本地安装。

`npm install xxx@1.0.0`

这种方式下，相关依赖库被放到安装目录的node_modules文件夹下。

2.全局安装。

`sudo npm install -g xxx`

这种方式下，相关依赖库被放到/usr/local/lib/node_modules下。

普通的包管理工具，一般一个模块只有一个副本，但npm不是这样。比如，A和B都用到了C，那么npm安装之后，A和B的node_modules下都能找到C，无论是本地安装，还是全局安装。

这样做的优点在于，A和B可以使用C的不同版本，且互不影响，缺点是占用的空间过大。

然而也不用太过担心空间问题，npm有cache功能。下载的npm包放在~/.npm下，且每个包只有一份。所以暂时不用的工程，把node_modules删掉就行了。再次安装时，由于不用下载文件，速度还是非常快的。

## yarn

yarn是facebook开源的一款代替npm的js包管理工具。注意别和Hadoop YARN搞混了。

https://www.jianshu.com/p/f05eabdf3ab6

使用yarn包管理工具

## NW.js

NW.js，也就是之前的node-webkit，是一个基于node.js的客户端开发SDK。其官网为：

http://nwjs.io/

安装方法：

`sudo npm install -g nw`

代码示例：

https://github.com/zcbenz/nw-sample-apps

nw打包是指将nw脚本打包成可执行文件的过程。打包的目的如下：

1.安装依赖的软件库。

2.避免源代码的泄漏。

这两点基本上都是客户端才需要的功能，因此nw打包工具通常也就是nodejs打包工具了。

常用的nw打包工具有nw-builder和nwjs-builder。前者支持nw v0.12及以前的版本，而后者支持最新版本。

后者的使用步骤如下：

1.`sudo npm install -g nwjs-builder`

2.`nwb nwbuild .`

第2步，在首次执行时，会下载依赖文件，这需要root权限。下载之后，用chmod命令将/home/user/.nwjs-builder/caches中的相关文件，修改权限。这样以后就不需要root权限来执行了。

## Electron

Electron是另一套node.js的客户端开发SDK。其官网为：

http://electron.atom.io/

教程：

https://www.sdk.cn/news/732

Electron与NW.js差异主要是：

1.NW.js的入口点是main.html，JS代码的执行就和在浏览器中的一样，这种模式被称为浏览器模式。

2.Electron的入口点是JS代码，它更像是一个普通的Node.js程序，这种模式也被称为Node模式。

3.Electron也有打包工具，且打包后的安装文件体积较小（其实两者打包后的结果都100MB+了，囧），但代码只压缩不加密，安全性不如NW.js。

4.NW.js打包的文件加密有30%的性能损失。

还有一个叫做Electron-forge的工具包：

https://www.electronforge.io/

参考：

http://molunerfinn.com/Electron-vs-nwjs/

Electron vs nwjs

http://www.yhz.me/blog/Packaging-Electron-win32.html

Electron打包成windows桌面程序

https://github.com/electron/electron/blob/master/docs/development/atom-shell-vs-node-webkit.md

Technical Differences Between Electron and NW.js (formerly node-webkit)

https://www.jianshu.com/p/57d910008612

Electron: 从零开始写一个记事本app

https://mp.weixin.qq.com/s/pBKoRBO_Grp1otGlDp5pUw

教你做个属于自己的Markdown编辑器

https://mp.weixin.qq.com/s/vq2znBRXqW3kAtk5Ixn_Jg

JavaScript和Node.js简史，前端未来走向何方？

## CEF

Chromium Embedded Framework的官网是：

https://bitbucket.org/chromiumembedded/cef

由于编译极为复杂，所以通常直接下载它的SDK。其网址：

https://cefbuilds.com/

然而，由于图片认证被“墙”的缘故，而无法下载。

## NeDB

http://www.alloyteam.com/2016/03/node-embedded-database-nedb/

Node嵌入式数据库——NeDB

# HTML & CSS

## name、id、class的区别

name、id、class都可以用来标识元素。

name是个普通的属性，一个页面可以有多个相同name的元素。

id在一个页面中是唯一的。但这个唯一性由程序员保证，浏览器并不检查id是否唯一。不唯一的话，相应的JS操作可能会出错。（通常的实现中，选择器只选择第一个id出现时的那个元素。）

class允许多个类型的组合，比如`<p class="a b"/>`表示p既属于a类，也属于b类。

## CSS选择器

CSS的语法如下：

selector {property1: value1; property2: value2; ... propertyN: valueN}

其中，CSS选择器用于将样式和内容绑定。常用语法如下：

| 元素 | 类 | ID | 属性 | 后代 | 子元素 | 相邻兄弟 |
| :--: | :--: | :--: | :--: | :--: | :--: | :--: |
| p | .p | #p | p[attr] | p0 p1 | p0>p1 | p0+p1 |

name没有快捷的选择方法，但可以采用属性的方式进行选择，例如：`input[name="you"]`

## 参考

https://www.zhihu.com/question/21775016

网页布局都有哪种？一般都用什么布局？

https://mp.weixin.qq.com/s/DUOfYS-0k-F1p1-_YO9udA

前端开发者们值得了解的11项前端技巧

# Traffic Demo

2019.9

最近心血来潮，翻出了本科时代的作业。其中有一个交通仿真的小demo，最早是用Java Applet写的。岂料，现在别说浏览器了，就连专门看这个的AppletViewer在新版SDK中，都不见踪影了。。。

于是，只好做现代化移植。本来首选JavaFX的，不料刚开始写，就发现JavaFX对于多线程渲染做的很差，而这个Demo正是个多线程的版本。

反正都要大改，还不如直接移植到js上，连编译都省了。

原始版本：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/java/trafic

新版本：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/nodejs/js/traffic

众所周知，js是单线程的，所以这个版本也是单线程的，逻辑稍微复杂了一些。

# WebAssembly

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

----

V8开发者Vyacheslav Egorov和Mozilla Hacks之间的source-map之战了。对于同一份source-map库的JS代码，Vyacheslav所魔改出的纯JS版本，其性能一举反超了Mozilla重写的Rust版。

https://www.zhihu.com/answer/1322391162

有没有让JavaScript在JS引擎上稳定、更快运行的Style Guide?
