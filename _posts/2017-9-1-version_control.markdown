---
layout: post
title:  版本管理工具的前世今生, 运维工具集, OA办公软件, CMake, Ninja
category: toolchain 
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

# 运维工具集

## Zabbix

zabbix是一个基于WEB界面的提供分布式系统监视以及网络监视功能的企业级的开源解决方案。

http://www.zabbix.com/

参考：

https://mp.weixin.qq.com/s/donTVjZFrkUFleswiJr-Bg

监控系统选型解析

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

Continuous Integration（CI）：持续集成

Continuous Delivery（CD）：持续交付

Continuous Deployment（CD）：持续部署

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

https://mp.weixin.qq.com/s/Tvmcwg8g85pompnyI3rPtg

用GitLab做CI/CD是什么感觉，太强了

https://mp.weixin.qq.com/s/2Yt1YS3QcVb_pxYqaKrxKA

蚂蚁构建服务演进史

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

## Sikuli

Sikuli是一种新颖的图形脚本语言，或者说是一种另类的自动化测试技术。它采用图像识别的方式进行自动检测。

参考：

https://mp.weixin.qq.com/s/MYA6l9V4BYIZO8Jgtds6GA

图像识别在测试中的应用

http://www.cnblogs.com/fnng/archive/2012/12/15/2819367.html

图形脚本语言sikuli

## Ansible

Ansible is Simple IT Automation——简单的自动化IT工具。这个工具的目标有这么几项：让我们自动化部署APP；自动化管理配置项；自动化的持续交付；自动化的（AWS）云服务管理。简单的说就是：**批量的在远程服务器上执行命令。**

官网：

https://www.ansible.com/

参考：

http://www.ansible.com.cn/

Ansible中文权威指南

https://mp.weixin.qq.com/s/ojpAOOnK5fEW12gG1zocBA

干货：一文详解Ansible的自动化运维

# OA办公软件

2016.6

工作9年，先后服务于4家公司，OA软件也算见识了一些。

第一家公司，用了一套作坊OA。说它作坊，是因为这是老板的一个朋友的作坊做出来的东西。代码极度差劲，以至于我这样的网站外行，都能改出不少问题来。

第二家公司是外企，用他们国外的OA系统。除了文字是英语之外，其他的中规中矩。

第三家公司，用的是用友致远OA系统。也是中规中矩，语言换成了中文，好用了一些。

第四家公司，使用企明岛的OA平台。上手感觉很惊艳，UI甩开之前的产品一条街。

详细了解之后，才知道：

1.yammer是OA 2.0的鼻祖。

2.国内的同类产品还有：明道，纷享，伙伴，企明岛，tita，UU社区，云之家等。

# CMake

添加头文件目录

`include_directories(../../../thirdparty/comm/include)`

添加需要链接的库文件目录

`link_directories("/home/server/third/lib")`

查找库所在目录

`find_library(RUNTIME_LIB rt /usr/lib  /usr/local/lib NO_DEFAULT_PATH)`

添加需要链接的库文件路径

`link_libraries(“/home/server/third/lib/libcommon.a”)`

设置要链接的库文件的名称

`target_link_libraries(myProject libcomm.so)`

为工程生成目标文件

`add_executable(demo main.cpp)`

下载文件

`file(DOWNLOAD url file)`

参考：

https://www.cnblogs.com/binbinjx/p/5626916.html

cmake添加头文件目录，链接动态、静态库

https://mp.weixin.qq.com/s/67lPVyWUXG0SPJm4AOHmBA

一份CMAKE中文实战教程

https://blog.csdn.net/lianshaohua/article/details/107904367

CMakeLists多目录通用模板

https://mp.weixin.qq.com/s/4iUTsx_rSjRI93b71YOyLg

万字长文带你从C++案例一步一步实操cmake

## cross compile

需要用`-DCMAKE_TOOLCHAIN_FILE=XXXX`来指定toolchain file。后者的示例如下：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/other/toolchain-arm-imx8qm.cmake

官方文档：

https://cmake.org/cmake/help/latest/manual/cmake-toolchains.7.html

# Ninja

Ninja是Make的替代品，它和后者的区别在于：

1.Ninja只实现了Make的常用功能，能力上没有后者强。

2.Ninja脚本易于人阅读（方便调试），但不易于人直接书写（方便机器解析）。需要借助CMake之类的高级构建系统生成Ninja脚本。

3.Make中有些功能虽然能实现，但需要复杂脚本，执行也很慢。Ninja将这些功能直接集成进程序，无需写脚本。

正因为这些设计上的不同，Ninja的执行速度远超Make。

官网：

https://ninja-build.org/

安装：

`sudo apt install ninja-build`

Cmake生成Ninja脚本：

`cmake .. -G Ninja`

Cmake这样的高级构建系统，也被称为meta-build software。

https://github.com/ninja-build/ninja/wiki/List-of-generators-producing-ninja-build-files

上面的网页列出了能生成Ninja脚本的构建系统，比较值得关注的有：

GN：Chromium项目的构建工具。

xmake：一个Lua编写的构建工具。

参考：

https://blog.codingnow.com/2021/05/make_to_ninja.html

构建工具从Make到Ninja
