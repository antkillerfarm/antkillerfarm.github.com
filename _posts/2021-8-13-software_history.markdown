---
layout: post
title: 上古软件开发
category: resource 
---

* toc
{:toc}

# 上古软件开发

金山影霸的VCD解压部分是盗用当时国外的一个叫Xing的收费共享软件里面的解码DLL（动态链接库）。金山影霸播放帧速高的原因是它在Windows操作系统播放时把屏幕设置成320x200全屏（VCD标准分辨率是352x240，所以抽行抽列显示后对画质影响不大），省去了图像放大和复制显示大图的步骤。金山影霸本质上是盗版Xing的DLL外面套一个320x200的全屏播放模式。

后来梁肇新单干后推出了可以解码VCD和DVD的超级解霸。超级解霸的MPEG2解码部分是拿开源软件mpeg2dec（后来改名叫libmpeg2，一直是GPL License，梁肇新又违反开源协议了）改出来的。

超级解霸里强调的主要优点其实没有太多技术含量。

1.强力纠错。（实际上是遇到读盘错误后直接跳过几秒钟，读下一段，CD/DVD读取光盘的默认行为是反复重读）

2.解码速度快。（实际上是梁肇新用汇编重写了部分C代码，比如像素从YUV转换到RGB的代码）

https://www.zhihu.com/question/20795371

超级解霸为什么当年那么火？后来又是为什么衰落的？

---

ABCDE五大高手：

Anders Hejlsberg：Turbo Pascal/Delphi/C#/TypeScript

Blake Stone：JBuilder

Chuck Jazdzewski：VCL/Angular 2

Danny Thorpe：Delphi

Erich Gamma：Eclipse/VS Code

https://mp.weixin.qq.com/s/xe7R2UZsyTzHN9xpdDTlXg

地球程序员之神

https://mp.weixin.qq.com/s/qWX17XcLQFRnip3JOkpRFg

孙悟空和Web争霸

---

作为一款早期计算机，IBM 705的编程是通过打孔卡输入machine instruction完成的。通过autocoder(作用相当于现在的assembler，不过部分用电路实现)，705可以把自定义的虛拟instruction转成实际对应的一系列machine instruction。这样做其中一个好处就是可以減小程序体积，用更少的punch card，写起来更快，输入进计算机时也更快。

因为是相对于定义在machine instruction下一层的micro instruction(现在叫microcode多些，没错这也是IBM搞出来的)的概念，所以取名macro instruction。

https://www.zhihu.com/question/482003599

在计算机编程领域里，宏(macro)是哪里来的？为什么要叫这个名字呢？

---

1999年年底，在全世界程序员在为千年虫问题焦虑的的时候，日本程序员却灵机一动：如果继续沿用昭和（1926年开始）年号的话，千年虫会足足延后到2025年。

25年的时间总该可以解决这次的问题了。当然，如果真的打算解决的话……

然而日本在2019年改元为令和时，不但要更改年号，而且昭和时代年号计算的“新千年虫”（昭和100年）马上就要来临。

而且不幸的是，不少系统的源代码经过30至40年都已经丢失了。

更加不巧的是，据说在日本IT界还有一个叫“2007年问题”的问题。

也就是说，当年建立电脑系统的工程师，大部分都会集中在2007年退休。到现在，已经基本上没有多少人知道如何维护旧的系统了。

---

在PC开始出现的初期，IBM的Lotus 1-2-3是PC的杀手应用，它被认为是PC所以成功的因素之一。Lotus 1-2-3纪元（epoch）从1900年开始，所以第一天是1900年1月1日，其后所有的日期都是在它上面加一个Delta差值，这也是为什么我们上面1900年2月28是的年份表示是00的原因。Lotus从诞生起就有个bug，它认为1900年是闰年！

微软因为Lotus的大卖而自己研发的表格系统Multiplan，它和它的继任者Excel为了能够与Lotus兼容，不但要做到外观十分相似，而且为了能够读取Lotus的文件而故意引入了一样的bug。

---

1991年，鲍比·柯提科和一群投资人收购了经营不善的Mediagenic，这间公司成为了动视的前身。为了帮助公司摆脱债务，柯提科对公司进行了重组，改名为动视（Activision），并将总部迁至加州。1997年，当公司终于重回盈利之后，动视走上了收购之路，在之后的十年里收购了约25个独立游戏工作室，以拓宽公司的产品矩阵。《托尼·霍克的滑板》、《使命召唤》和《吉他英雄》等知名游戏系列均在这一时期形成。

1991年，麦可·莫怀米、艾伦·阿德汗和弗兰克·皮尔斯这三名加州大学洛杉矶分校的毕业生为了自己的游戏理想，创办了一家名叫硅与神经键的公司，公司于1994年改名为暴雪娱乐。

https://view.inews.qq.com/wxn/20220119A03BL400

687亿美元！微软买下80后、90后的青春

---

所谓的Voxel Space引擎，说白了就是高度图+Raycast的伪3D方案，适用于描绘起伏不定的山野开阔地形。野外大场景的一个特点是超级扁平化（水平尺度能大到几公里，垂直高度差却只有几十米、百十米）这种情况，BSP很不擅长去负担，树会超级不平衡，效力底下，遮挡关系很少，基于BSP的PVS会很可怜。Voxel在当时可以说是效果惊人，处理野外场景的能力，得到的表现力可以说甩开多边形3D引擎几条街。

杯具的是后面行业的发展是GPU和相关的图形硬件突飞猛进。DF即使是野外场景也毫无优势，能支持的表现力和性能大幅度落后当时主流走多边形路线的FPS。

参考：

https://s-macke.github.io/VoxelSpace/

Voxel Space

https://www.zhihu.com/question/51667350

曾经赫赫有名的FPS射击游戏《Delta Force 三角洲特种部队》为什么没落了？

---

Ken Thompson在贝尔实验室的时候，他总是能在一台装了Unix的服务器上黑进他人的账户，不管他人怎么修改账户密码都没有用，当时贝尔实验室里面聚集的都是智商爆表、专业知识过硬的科学家，Ken的行为无疑让他们非常不爽。

有个人分析了Unix的代码之后，找到了后门，重新编译部署了Uinx，但是让他们崩溃的事情再次发生，Ken还是能黑进他们的账户，这个事情让他们百思不得其解。

一直到1983年，Ken获得图灵奖，在大会上解开了这个秘密，原来这个密码后门是通过他写的一个C编译器植入的，而当时那台Unix的机器必须通过这个C编译器编译之后才能运行，所以不管unix怎么修改都没有用，毕竟是要编译的。

---

https://github.com/chrislgarry/Apollo-11

Apollo 11登月代码成Github热度第一

https://mp.weixin.qq.com/s/0WAXiveeT3BPyowya0y2Ag

揭秘阿波罗11号大脑：人类的一大步，也是机器的一大步！

https://mp.weixin.qq.com/s/5aezQzLIqNwJBEYR62-TQg

我在1969年写代码

https://mp.weixin.qq.com/s/ksAz2w5F7GF5TGJtF13OLA

50年前的程序员们，拯救了阿波罗号登月飞船

https://mp.weixin.qq.com/s/t8q-i5k5V44RfbCtSVQonw

改变世界的代码行

https://mp.weixin.qq.com/s/t8q-i5k5V44RfbCtSVQonw

诺基亚经典游戏《贪吃蛇》的前世今生

https://mp.weixin.qq.com/s?__biz=MzA4MDExMDEyMw==&mid=2247487079&idx=1&sn=9d29f53cd518b4dcb8ab8c27159dee41

互联网大佬学历背景大调查，不要再相信“学习无用论”

https://www.zhihu.com/answer/658973076

为什么上古编程语言（比如COBOL）总喜欢把代码全部写成大写字母？

https://mp.weixin.qq.com/s/gv0X080hHtKakjZMzVaZ-w

80岁COBOL码农：“扶我起来，这个bug我会修。”

https://mp.weixin.qq.com/s/VeSBuwWMx0xd2GuPSpE0ZA

上古语言从入门到精通：COBOL教程登上GitHub热榜

https://mp.weixin.qq.com/s/XPuJJm3k8gDl17oVmtdGtA

代码回忆录：第一行Hello World，第一个弹窗广告的代码是怎么写的？

https://www.zhihu.com/answer/840708044

怎样评价《数码宝贝》第一部中的泉光子郎的编程水平？

https://www.zhihu.com/answer/2154627154

上古游戏引擎

https://mp.weixin.qq.com/s/yDaIuhjhjqL-Vtl1gwcd0A

计算机编程的历史演进：用50种编程语言写“Hello,World!”程序

https://mp.weixin.qq.com/s/Dqe5B0ooayRU79CGulq71g

重启尘封十年的代码！回到未来的人人网，如何用新技术唤醒老数据？

https://www.zhihu.com/question/50076174

为什么魂斗罗只有128KB却可以实现那么长的剧情？

https://mp.weixin.qq.com/s/_rybr3Pnmt6Rr3RWxGTCXQ

为什么有32个关卡的超级马里奥兄弟只要64KB？

https://www.w3cschool.cn/article/leijuncode

分享雷军22年前编写的代码

https://mp.weixin.qq.com/s/movz5aesYdZZZHle-hHRIw

诞生于穿孔纸带时期的语言，ALGOL 60今年60岁了

https://mp.weixin.qq.com/s/aYGcJaLwbuXzjI5z1Crsrw

LeCun自曝使用C语言23年之久，2年前才上手Python，还曾短暂尝试Lua

https://mp.weixin.qq.com/s/vBFOQjJr27AeRsqCej85iA

70年前，在苏联第一家AI LAB从事AI研究是种什么体验？

https://mp.weixin.qq.com/s/7cf14CEDa-LHFvO8gyedzg

为什么老编辑器Vim这么难用，却很受欢迎？

https://mp.weixin.qq.com/s/b-34Y230B_I56Epf64hYDQ

宇宙第一IDE到底是谁？

https://mp.weixin.qq.com/s/gSW5lxCoo9JkH1JRp6hfzA

30年前未曾发行的任天堂红白机游戏，被这个团队从21张软盘中重新恢复了，还是3D的

https://mp.weixin.qq.com/s/E3_UrGoN1uSmD_4NYG-YWw

来看看比尔盖茨当年写的BASIC解释器源代码吧，你就知道泰勒级数有什么用了

https://mp.weixin.qq.com/s/RzrQpJ8M4SoY6YXhUQNWHg

你上世纪写的代码现在还work吗？挑战者：我需要一个读磁带的机器

https://www.zhihu.com/question/420656795

有哪个高手可以解读“世界黑客编程大赛第一名的作品（97年Mekka ’97 4K Intro）”?

https://mp.weixin.qq.com/s/21eCFfRZYyTstnw777-Fhg

Java的战争（Oracle vs Google）

https://mp.weixin.qq.com/s/af_MzLovqnt5JemXBQGdxg

Java的战争(后续)

https://mp.weixin.qq.com/s/oeusDoCSYiwwX61H_6wQHw

浏览器（内核）发展史

https://www.zhihu.com/question/439434803/answer/1679210108

如何评价大连车务段现在车系统瘫痪，“全力攻关一昼夜”恢复Flash运行?

https://mp.weixin.qq.com/s/YOcKv4InA1G7sdERanGZSA

《是男人就下100层》真的有隐藏剧情！

https://mp.weixin.qq.com/s/popvQv-CM7--5ByTMpeaxA

编程语言考古：曾经影响一代人的BASIC，原来还有前身

https://mp.weixin.qq.com/s/K-KIA18cCj6ANM4Sb7fEvw

50年长盛不衰，SQL为什么如此成功？

https://mp.weixin.qq.com/s/3ZszI7PupWsuTvy69BwPnw

“C语言之父”40年前搞的操作系统复活！Linux、Windows都借鉴过它（Plan 9）

https://mp.weixin.qq.com/s/ywTkMMP6ysBfByIXW3_xeQ

全世界下载量超100亿，curl怎样成为影响世界的开源项目？
