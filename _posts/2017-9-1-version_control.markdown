---
layout: post
title:  版本管理工具的前世今生, Kannel, Win10历险记, 运维工具集, Android研究（二）, ANTLR
category: technology 
---

* toc
{:toc}

# 版本管理工具的前世今生

参考Wiki的相关词条，可将版本工具分为三代：

1.本地版本管理

开源：SCCS (1972) RCS (1982)

私有：PVCS (1985) QVCS (1991)

以RCS最为著名，不过由于年代久远，我从来没用过。

2.客户端/服务器版本管理

开源：CVS (1986, 1990 in C) CVSNT (1998) QVCS Enterprise (1998) Subversion (2000)

私有：Software Change Manager (1970s) Panvalet (1970s) Endevor (1980s) DSEE (1984) Synergy (1990) ClearCase (1992) CMVC (1994) Visual SourceSafe (1994) Perforce (1995) StarTeam (1995) Integrity (2001) Surround SCM (2002) AccuRev SCM (2002) SourceAnywhere (2003) Vault (2003) Team Foundation Server (2005) Team Concert (2008)

我用过的包括CVS、SVN、ClearCase和Visual SourceSafe。

这一代的工具，以CVS为开端。ClearCase和Visual SourceSafe与CVS差不多同时期，因此功能上也多有相同，总的来说就是ClearCase功能强，但不好用。SourceSafe好用，但功能差。

其中，ClearCase我在之前的公司有用过，当时为了简单的check in和check out，竟然还需要编写脚本以简化流程。不过功能的确是没的说，分支合并权限都不在话下。

VSS以现在的角度来看，基本就是个垃圾了，它是MS收购的一家公司的产品，目的是填补VS在这方面的空白。但这样弱的工具，即使MS内部的人也基本不用，这也是后来MS发布Team Foundation Server的重要原因。

SVN是这一代的集大成者，使用简单的同时，仍保有相当强度的功能，唯一诟病的就是合并功能太弱。

3.分布式版本管理

开源：GNU arch (2001) Darcs (2002) DCVS (2002) ArX (2003) Monotone (2003) SVK (2003) Codeville (2005) Bazaar (2005) Git (2005) Mercurial (2005) Fossil (2007) Veracity (2010)

私有：TeamWare (1990s?) Code Co-op (1997) BitKeeper (1998) Plastic SCM (2006)

早期比较有名的是BitKeeper。它在2002~2005年是Linux Kernel所使用的版本工具。Linus曾经非常喜欢它。可惜后来由于商业利益的关系，开发者的要挟惹毛了Linus。大神发威开发了Git，从此BitKeeper也就无人问津了。

从这件事情可以看出，Linus其实并不是一个像Richard Stallman那样的开源洁癖狂，现有的东西够用，他就懒得节外生枝了。但也不要因此小看他的水平，大神发起威来，搞死像BitKeeper这样的工具还是绰绰有余的。

目前较为主流的Bazaar、Git和Mercurial都出现在2005年，这并不是偶然的。实际上都是BitKeeper和开源社区之间战争的产物。

后Git时代的工具，如Fossil和Veracity，相比Git来说，对权限、BUG跟踪之类的功能做了进一步的扩展。

# Kannel

Kannel是一个开源的WAP&SMS网关项目。

官网：

http://www.kannel.org/

这里提到这个项目，并非它有多么重要——实际上它从2010年之后就几乎没有更新了，最新的版本定格在2014年。它所代表的WAP已经无人问津，至于SMS吧，似乎又用不到这么复杂的框架。

但我之所以要提它，主要在于情怀。2007年底的时候，公司给我安排了一个在App中集成彩信发送功能的任务。当时由于能力尚浅，虽然努力了一个月，最终却没有实际成果，很是让领导质疑了一阵，幸好接手的哥们同样做不出来，而我的下一个任务——使用jpeglib，获得了成功，才算将事情平息下来。

过了一年，闲暇无聊之际，旧事重提，于是发现了Kannel，并做了一个demo。可惜公司时局变化，这一切都变得无足轻重了。

# Win10历险记

我大概在2015年4、5月间，听说了Win10免费升级的消息。于是一直很期待7月29日的到来。果然到了29日当天下午的时候，公司电脑就收到了升级的通知。然而由于网速不给力，当天并未对公司电脑进行升级。倒是晚上在家里的电脑上，虽然耗时2小时，但却一路顺利的升级成功。

Win10给人的第一感觉，其实和Win8差不多，只不过加了个更像Win7的开始菜单而已。不过既来之则安之，一段时间用下来，总算还是要比Win7强不少的。

又过了几天，公司的电脑也升级成功。正得意间，忽然发现VirtualBox在Win10下工作不正常。查了VirtualBox官网方知，其目前尚不支持Win10 Host。于是不得不重新降级到Win7。看来尝鲜也是有得有失的。对于工作用的电脑，有的时候够用就好，没必要什么都求新的。

# 运维工具集

## Zabbix

zabbix是一个基于WEB界面的提供分布式系统监视以及网络监视功能的企业级的开源解决方案。

http://www.zabbix.com/

## Cacti

Cacti是一套基于PHP,MySQL,SNMP及RRDTool开发的网络流量监测图形分析工具。

http://cacti.net/

## Nagios

Nagios是一款开源的免费网络监视工具，能有效监控Windows、Linux和Unix的主机状态，交换机路由器等网络设备，打印机等。

https://www.nagios.org/

## Ganglia

Ganglia是伯克利开发的一个集群监控软件。可以监视和显示集群中的节点的各种状态信息，比如如：cpu 、mem、硬盘利用率， I/O负载、网络流量情况等，同时可以将历史数据以曲线方式通过php页面呈现。

官网：

http://ganglia.sourceforge.net/

## Jenkins

Jenkins是一个开源软件项目，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。

https://jenkins.io/index.html

自动化部署的其中一种方案：

gitlab管理代码版本，触发jenkins自动构建+测试，然后走迭代或者发布，全部环境都在docker内。

和Jenkins同类型的工具还有Travis CI。

参考：

https://mp.weixin.qq.com/s/jcpynCa6CToITUGD9hRylw

推荐10款最佳Jenkins插件

www.ruanyifeng.com/blog/2017/12/travis_ci_tutorial.html

持续集成服务Travis CI教程

https://mp.weixin.qq.com/s/wmpbdj2GMb10xRtjRUpesw

携程旅行App iOS工程编译优化实践

## Walle

Walle一个web部署系统工具，配置简单、功能完善、界面流畅、开箱即用！支持git、svn版本管理，支持各种web代码发布，PHP，Python，JAVA等代码的发布、回滚，可以通过web来一键完成。

https://walle-web.io/

## JMeter

Apache JMeter是Apache组织开发的基于Java的压力测试工具。用于对软件做压力测试，它最初被设计用于Web应用测试，但后来扩展到其他测试领域。 它可以用于测试静态和动态资源，例如静态文件、Java 小服务程序、CGI 脚本、Java 对象、数据库、FTP 服务器， 等等。JMeter 可以用于对服务器、网络或对象模拟巨大的负载，来自不同压力类别下测试它们的强度和分析整体性能。

http://jmeter.apache.org/

## H5ai

H5ai是一款功能强大php文件目录列表程序，由德国开发者Lars Jung主导开发，它提供多种文件目录列表呈现方式。

官网：

https://larsjung.de/h5ai/

参考：

https://www.tok9.com/archives/374/

H5ai完整安装及使用教程

# Android研究

## Flutter

Flutter是Google用以帮助开发者在Ios和Android两个平台开发高质量原生应用的全新移动UI框架。Beta1版本于2018年2月27日在2018世界移动大会上公布。

官网：

https://flutter.dev/

参考：

https://mp.weixin.qq.com/s/pU75twMDry4VUYtTHeV_IQ

一文深入了解Flutter界面开发

https://mp.weixin.qq.com/s/yPvaB7sLuJoGfsjj7x7wcg

深入理解Flutter的编译原理与优化

https://mp.weixin.qq.com/s/SWP7Nu9DEUhvAyvdIG-Ixw

聊一聊Flutter Engine线程管理与Dart Isolate机制

https://mp.weixin.qq.com/s/cJjKZCqc8UuzvEtxK1BJCw

Flutter的原理及美团的实践

https://mp.weixin.qq.com/s/l6xvmnLE6HfRtw6upo6yUA

高效开发与高性能并存的UI框架——携程Flutter实践

https://mp.weixin.qq.com/s/vcbHMtaJEkZhSgiRBST1YA

移动开发这十年

https://mp.weixin.qq.com/s/VlleaiIzsZHZDkDrGgYi1g

如何用Flutter实现混合开发？闲鱼公开源代码实例

https://mp.weixin.qq.com/s/n2avWVS0FAFIEB4w0W0Xsw

学习Dart的10大理由

https://mp.weixin.qq.com/s/CBp1pneBa_t8bkVlh1RGVg

2019年五大跨平台移动应用开发工具

https://mp.weixin.qq.com/s/ygPRdtRMlNW3-nfo0PonAA

揭秘！如何用Flutter设计一个100%准确的埋点框架？

## Litho

Litho是Facebook推出的一套高效构建Android UI的声明式框架，主要目的是提升RecyclerView复杂列表的滑动性能和降低内存占用。

官网：

https://fblitho.com/

参考：

https://mp.weixin.qq.com/s/RS7O7prvkCvKyxkK3YQxtA

Litho的使用及原理剖析

## adb

手机->PC：

`adb pull sdcard/contacts_app.db`

PC->手机：

`adb push aaa/contacts_app.db /sdcard/`

----

https://www.cnblogs.com/caoxinyu/p/10568463.html

Ubuntu adb报错：no permissions (user in plugdev group; are your udev rules wrong?);

## 参考

https://mp.weixin.qq.com/s/twfpUMf9CfXcgwtFFkJ4Ig

Android整体设计及背后意义

https://mp.weixin.qq.com/s/eEuNPtTaPwJ7hSghgeU32g

Android Hook技术防范漫谈

# ANTLR

ANTLR—Another Tool for Language Recognition，其前身是PCCTS，它为包括Java，C++，C#,python在内的语言提供了一个通过语法描述来自动构造自定义语言的识别器（recognizer），编译器（parser）和解释器（translator）的框架。

官网：

http://www.antlr.org/

参考：

http://yuzhouwan.com/posts/55501/

Antlr

https://www.ibm.com/developerworks/cn/java/j-lo-antlr/index.html

使用Antlr开发领域语言

# MPS

MPS是jetbrains推出的用于构建DSL的工具。

官网：

https://www.jetbrains.com/mps/
