---
layout: post
title: 上古软件开发（一）
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

《编程高手箴言》

https://www.zhihu.com/question/20795371

超级解霸为什么当年那么火？后来又是为什么衰落的？

https://www.zhihu.com/question/576538000

当年在Linux上运行Windows程序的豪杰兼容层，是基于Wine二次开发还是另起炉灶的？

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

《电脑爱好者》是我中学时期最喜欢的杂志之一，一代人的回忆啊。

https://zhuanlan.zhihu.com/p/1985760942458419091

时代的眼泪！《电脑爱好者》注销了

---

在1999年至2015年期间，共有超700名邮局分局负责人因Horizon审计软件的bug而遭到起诉，之后富士通又陆续提出多项并不属实的财务问题。数百人因此被监禁或破产，期间至少四人因此选择自杀。

https://www.36kr.com/p/2598433889188741

系统bug致百人入狱，砸了2.8亿元仍上云失败，二十年了，这家大企业被日本软件坑惨了

---

在PC开始出现的初期，IBM的Lotus 1-2-3是PC的杀手应用，它被认为是PC所以成功的因素之一。Lotus 1-2-3纪元（epoch）从1900年开始，所以第一天是1900年1月1日，其后所有的日期都是在它上面加一个Delta差值，这也是为什么我们上面1900年2月28日的年份表示是00的原因。Lotus从诞生起就有个bug，它认为1900年是闰年！

微软因为Lotus的大卖而自己研发表格系统Multiplan，它和它的继任者Excel为了能够与Lotus兼容，不但要做到外观十分相似，而且为了能够读取Lotus的文件而故意引入了一样的bug。

---

1991年，鲍比·柯提科和一群投资人收购了经营不善的Mediagenic，这间公司成为了动视的前身。为了帮助公司摆脱债务，柯提科对公司进行了重组，改名为动视（Activision），并将总部迁至加州。1997年，当公司终于重回盈利之后，动视走上了收购之路，在之后的十年里收购了约25个独立游戏工作室，以拓宽公司的产品矩阵。《托尼·霍克的滑板》、《使命召唤》和《吉他英雄》等知名游戏系列均在这一时期形成。

1991年，麦可·莫怀米、艾伦·阿德汗和弗兰克·皮尔斯这三名加州大学洛杉矶分校的毕业生为了自己的游戏理想，创办了一家名叫硅与神经键的公司，公司于1994年改名为暴雪娱乐。

https://view.inews.qq.com/wxn/20220119A03BL400

687亿美元！微软买下80后、90后的青春

https://mp.weixin.qq.com/s/ggu2IMSzOTmHtthQhepX2g

暴雪国服正式停服，与网易14年合作结束！数百万玩家纷纷祭奠，再见青春

---

Ken Thompson在贝尔实验室的时候，他总是能在一台装了Unix的服务器上黑进他人的账户，不管他人怎么修改账户密码都没有用，当时贝尔实验室里面聚集的都是智商爆表、专业知识过硬的科学家，Ken的行为无疑让他们非常不爽。

有个人分析了Unix的代码之后，找到了后门，重新编译部署了Uinx，但是让他们崩溃的事情再次发生，Ken还是能黑进他们的账户，这个事情让他们百思不得其解。

一直到1983年，Ken获得图灵奖，在大会上解开了这个秘密，原来这个密码后门是通过他写的一个C编译器植入的，而当时那台Unix的机器必须通过这个C编译器编译之后才能运行，所以不管unix怎么修改都没有用，毕竟是要编译的。

---

最开始的C++编译器是CFront，由C++之父Bjarne在1982年春到1983年夏完成，而直到1988年6月前，几乎所有的C++编译器都是CFront的移植版本。很多人认为Borland C++与VC++算真正意义的首先非CFront的C++编译器实现，而Bjarne在他书写的C++历史中，认为Zortech才算。

四大高手：Borland C++, Microsoft C++, Watcom C++， Symantec C++。

Portable C Compiler，一种早期的C语言编译器，由Stephen C. Johnson于1970年代中期，在贝尔实验室写作。

虽然现在PCC已经式微了，但80年代初C编译器家族们可都是要么基于PCC，要么大量参考了PCC的设计的。

除了PCC之外，Stephen C. Johnson还是YACC的作者。

SGI的MIPSPro -> Pro64 -> Open64 -> PathScale这条线。Fred Chow（周志德）是Open64项目的主导者。

https://www.zhihu.com/question/39661628

历史上出现过的主流C/C++编译器都有哪些？

---

Borland Graphics Interface，或者Turbo C Graphics，是上世纪八九十年代Borland公司出品的Turbo C/Turbo C++/Borland C++等C语言产品中自带的绘图库。因为其简单易用而广受欢迎。

它的现代版本主要有：SDL_bgi/libXbgi/WINBGIm等。

https://sdl-bgi.sourceforge.io/

当然还有当时最流行的bgidemo：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/other/bgidemo.c

---

Afx前缀是微软MFC一个小组的名称简写。

---

DISKMAN由国人李大海编写。

https://www.zhihu.com/question/47503655

如何评价DiskGenius这款软件？

---

中国第一批互联网创业者有不少来自惠多网，马化腾是深圳的DE站站长，金山的求伯君也是一个站长，好像丁磊也玩。

当时中国的互联网出口有两个，一个是中科院的网络，一个是教育科研网。教育网的国际出口在清华大学，清华到中科院也就三四公里的路，但清华的电脑要连上曙光站，得从国际出口去美国兜一圈回来，既慢又不稳定。

清华的学生一直熬到1995年8月，有个网名ACE的用户，一怒之下在自己实验室的386电脑上装了套台湾的BBS软件，把站点起名“水木清华”。水木清华BBS是中国大陆第一个同时在线超过100人的“大型”网站。


https://www.zhihu.com/question/19848288

中国最早的论坛（BBS）是哪个？

---

2021年，美国前总统特朗普团队使用了Mastodon的源代码，开发了他们的新社交媒体平台，即所谓的Truth Social。

https://mp.weixin.qq.com/s/EG65kmRlKAdTzn8kPmU0BA

反Twitter平台用户激增250万，这名29岁程序员如何凭一己之力扛住超8倍流量增长？

---

从Sun走出的著名人物：

Whitfield Diffie：图灵奖获得者，公钥密码体系先驱

James Duncan Davidson：Tomcat作者

Marc Fleury：JBoss作者

Bob Scheifler：X-Windows领导者

Paul Buchheit：GMail发明人

Joshua Bloch：Java大牛，Effective Java作者

Brendan Gregg：DTRace作者

Lars Bak：Java HotSpot作者，V8作者

---

固态硬盘消灭了“清理硬盘碎片”的需求，其他什么手动内存整理、修改注册表、操作系统瘦身等等“屠龙技”也逐渐失去了价值。

---

雷军说，WPS2000彻底杜绝了宏病毒问题。

NB啊。安装完一看，原来根本不支持宏，原来如此，相当于说男性不会得子宫颈瘤。

1987年湖北省高考理科状元叫刘赛游，毕业于新洲一中，考了635分；文科状元叫胡大华，和雷军一个学校。所以江湖传言雷军高考700分，湖北省状元这些都是谣言。他只是仙桃中学一个班上的第5名。

1998年前后，雷军担任金山软件总经理，而周鸿祎当时是方正科技一个事业部的经理。据说当时雷军和周鸿祎经常一起下班，因为两人的妻子是闺蜜，经常一起逛街购物，所以他们两个“大男人”就只能在旁边等着。周鸿祎后来还调侃说，那时候雷军已经开上了桑塔纳，而他还在骑自行车，雷军还会开车送他回家。

https://www.w3cschool.cn/article/leijuncode

分享雷军22年前编写的代码

https://mp.weixin.qq.com/s/7AtuIcic3ubzYAHnkDpONA

雷军：穿越人生低谷的感悟
