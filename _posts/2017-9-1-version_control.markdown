---
layout: post
title:  版本管理工具的前世今生, OA办公软件, CMake, Ninja, Git（一）
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

参考：

https://blog.51cto.com/panzhengming/1607175

git版本控制器

https://zhuanlan.zhihu.com/p/95179354

VCS发展简史：SCCS->RCS->CVS->SVN->Git

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

示例代码：

https://github.com/ttroy50/cmake-examples

---

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

---

FetchContent如果网络不好的话，也可使用`FETCHCONTENT_SOURCE_DIR_<uppercaseName>`引用已经下载好的外部路径。

https://zhuanlan.zhihu.com/p/102050750

CMake之引入外部项目的三种方法

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

# Git

## 教程

https://learngitbranching.js.org

这个网站提供了一个可视化的交互式git教程。非常不错。

## git实现原理

![](/images/img3/git.jpg)

git采用如上图所示的树状结构，因此分支的速度很快。

svn本身不支持分支功能，所谓分支无非是创建一个新的副本文件夹，而这是一个代价很高的操作，而git就简单多了，增加一个指向新分支的指针即可。

https://zhuanlan.zhihu.com/p/142289703

一文讲透Git底层数据结构和原理

## 如何git下载超大版本库

自从两次git完整的linux kernel，都因为网络问题，而中途失败之后，不甘心的我，继续在网上寻找答案。

目前已知的答案是git不支持断点续传，也不支持object原子下载。网上所谓的git fetch能断点续传一说，纯属误会。那只不过是重新下载的命令，即使成功，那也是由于第二次的网络环境变好导致的。

其他的办法包括git bundle，但是这个需要服务器支持才行，而多数站点都没有这个功能。Android代码的网站就采用了这种方法。

其实对付超大版本库，git已经提供了相应的办法：

`git pull --depth=N`

N表示深度，具体的含义我也不太清楚。基本上，N=1就是只下载当前的版本，N>1的话，还会下载以前的历史版本。这样我们可以通过不断增加N的方式，将完整的版本库下载到本地。

N增加到多大，才能把整个版本库下载完呢？

使用命令行的话，如果增加N，没有继续下载，就说明版本库已经完全下载到本地了。

如果使用TortoiseGit的话，下载完全之后，再git pull的话，就没有depth选项了。

下载失败，会在本地留下一堆无用的临时文件，可用`git prune`清除之。

日常使用中，git版本库更新次数越多，碎小文件越多。这时可用`git gc`将之打包成几个大文件。

参考文章：

http://blogs.atlassian.com/2014/05/handle-big-repositories-git/

How to handle big repositories with Git

## gitee

gitee是github的国内山寨产品。

官网：

https://gitee.com

近来，github的国内访问速度很慢，可以使用gitee的导入功能，将代码下载到国内，然后再下载之。

## Git Extensions

除了Tortoisegit之外，Git Extensions也是一个常用的GUI shell。

官网：

https://gitextensions.github.io/

## Git Server

### Bonobo Git Server

一个基于IIS的Git服务器。操作简单，但是没有提供文件夹一级的权限管理。

官网：

https://bonobogitserver.com/

### Gerrit

Gerrit，一种免费、开放源代码的代码审查软件，它使用Git作为底层版本控制系统。

官网：

https://www.gerritcodereview.com/

### GitLab self-managed

GitLab self-managed是GitLab出品的本地Git Server。

---

研发人员太多的时候，还需要分布式的代码托管平台。基于GitLab生态开源组件二次开发，是其中的一种选择。

https://mp.weixin.qq.com/s/aY-QJN2Fkp86ATdvqVvpZQ

Code：美团代码托管平台的演进与实践

## Monorepo

上面提到的多项目管理，一般称为MultiRepo，即每个项目对应一个单独的仓库。与之相对的则是Monorepo，即把多个项目放在一个仓库里面。

![](/images/img4/Monorepo.png)

参考：

https://juejin.cn/post/6944877410827370504

现代前端工程为什么越来越离不开Monorepo?

https://zhuanlan.zhihu.com/p/77577415

Monorepo是什么，为什么大家都在用？

## Git多项目管理

项目越来越大，一些通用的模块我们希望将他抽离出来作为单独的项目，以便其他项目也可以使用，或者使用一些第三方库，可能我们并不想将代码直接拷贝进我们的项目里面，而仅仅只是单纯的引用。

基于Git有多种方式来解决这个问题：

- Git Submodule。

- Git Subtree。这两个已经集成到git中。

- GitSlave。一个git插件，需要额外安装。

- Google Repo。基于git的python脚本。Android项目的官方工具。

`git submodule update --init --remote --rebase`

单独更新子module AAA：

`git submodule update AAA`

参考：

https://www.jianshu.com/p/284ded3d191b

Git多项目管理
