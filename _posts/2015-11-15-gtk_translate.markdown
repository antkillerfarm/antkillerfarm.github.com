---
layout: post
title:  Transifex与GTK文档翻译, Raspberry Pi（一）
category: technology 
---

* toc
{:toc}

# Transifex与GTK文档翻译

## 参与GTK+开发的一段小经历（2013.10）

最近忽然对GTK+产生了浓厚的兴趣，打算研究一下。学习一个新东西，最好的方法就是先阅读一下它的文档。应该说GTK的文档虽然比不了MSDN，但也颇为详尽。主要的问题在于文档都是英文的，阅读起来比较吃力。

考虑到GTK已经有15年的历史，所以试着在网上搜了一下参考手册的中文版，结果找到了这个网址：

http://code.google.com/p/gtk-doc-cn/

这是一个叫yetist的人发起的翻译项目。

http://forum.linuxfans.org/viewthread.php?tid=164933

https://wiki.ubuntu.org.cn/viewtopic.php?f=163&t=232005&start=15

这两个网页讲述了他发起这个项目的经历。

至于GTK官方的语言翻译地址，查了半天也没找到，所以也就没有办法贡献自己的力量了。

在学习兼翻译的过程中，发现了一个我搞不懂的地方，于是给GTK官网上的Bugzilla发了一个bug，结果很快就收到了回复，一段有趣而愉快的经历。看来以后可以多参加一些开源社区的活动，积攒自己的人品，^_^。

## Transifex（2015.6）

上面提到的yetist同学发起的翻译项目，使用Transifex作为翻译协作工具。其网址为：

https://www.transifex.com/projects/p/gobject-reference-manual/

yetist同学还同时主导了一系列的GTK文档翻译项目。有兴趣的同学，可以根据自己的情况，选择性的进行翻译。

2013年的时候，我曾经翻译了一部分GObject文档，并提交给yetist。但他近来已不活跃，未能及时处理我的提交请求。直到最近我才被接纳为GObject项目的翻译人员。于是就有了下面的对transifex的初体验。

另外说一下，我翻译的部分主要参考了TualatriX的成果，他的网址是：

http://imtx.me/

他也是ubuntu-tweak的作者。我经常用这个软件清理磁盘。

---

https://toshiocp.github.io/Gobject-tutorial/index.html

GObject Tutorial for beginners

https://www.cnblogs.com/silvermagic/p/9087883.html

Glib之GObject简介

## GTK文档翻译流程

由于Google Code即将关闭，翻译项目的网址搬迁到：

https://github.com/yetist/gtk-doc-cn

或者也可以在我的github下找到这个项目的fork：

https://github.com/antkillerfarm/gtk-doc-cn

欢迎大家向yetist或者我的github提交翻译成果。

从项目的log可以看出，yetist同学不仅自己翻译了很多内容，而且还编写了整个翻译自动处理的框架。

该框架的大概流程如下：

1.下载代码，并使用gtk-doc工具，抽取代码中的注释，生成帮助文档的po文件。

2.将po文件发布到Transifex平台，由网上的翻译志愿者，翻译po文件。

3.将翻译好的po文件，合并到代码中，并生成最终的html格式的文档。其中合并和生成，都可以使用框架提供的命令来完成。

前两步yetist同学已经做好了，我们需要做的主要是第3步。

框架的基本用法见项目的说明文档，这里仅列举文档中未提到的细节：

1.环境准备

以Ubuntu为例，可用以下命令安装依赖软件包：

`sudo apt install python python-pip fam gtk-doc-tools gnome-doc-utils`

框架需要初始化：

`make init`

2.Transifex客户端

Transifex客户端用于同步Transifex平台的翻译更新，其使用方法和git颇为类似。它的官方地址如下：

http://docs.transifex.com/client/

以下仅列出必要的操作，完整的操作参见官网。

1）安装

`pip install transifex-client`

2）初始化

`tx init`

3）配置

`tx set --auto-remote <url>`

4）下载

`tx pull -a`

3.合并po文件

1）修改项目目录下的AUTHORS文件，添加你要翻译的po文件。

2）将翻译后的po文件，放到po或者xmlpo文件夹中，并改为上一步时你起的名字。

3）合并。

`make merge`

4）生成最终文档。

`make docs`

这里不知是否存在bug，有的时候这个命令需要运行两次，第1次失败，第2次就可以看到最终的文档了。

# GIO网络应用开发

GIO提供了以GSocket为首的低级API，和以GSocketClient、GSocketConnection为首的高级API。

高级API的使用示例如下：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/helloworld/glib/network

需要注意的是，此例中Server端采用的是阻塞式操作，因此会将main loop阻塞住。如果main loop需要处理其他事件的话，这里可使用GThreadedSocketService启动单独的线程，处理之。

# Raspberry Pi

## 概述

树莓派（Raspberry Pi）在极客领域可谓大名鼎鼎，它的官网是：

https://www.raspberrypi.org/

从型号来看，它可以分为三大类型：

1）B型。面向开发者和学生。

2）A型和Zero型。面向批量制造类的客户。

从技术角度来说，树莓派虽然优秀，然而有实力制作这样开发板的公司，没有一千，也有几百。但世界范围内，只有Raspberry Pi和Arduino这两款开发板取得了成功。

Arduino是一款微控制器，主要用于电子工程领域，比如工业设备、传感器控制等。程序设计偏单片机风格，价格低廉，计算能力有限。它的官网：

https://www.arduino.cc/

而Raspberry Pi的定位是一个廉价的PC。其计算能力和目前的智能手机相当，但操作系统却和普通的桌面系统类似，因此，普通的PC应用可以很方便的移植过来。

Raspberry Pi官方OS是Raspbian，这是一个基于Debian的Linux发行版。

除此之外，还有几个Ubuntu定制版：

http://ubuntu-mate.org/raspberry-pi/

甚至微软也为此专门推出了Windows 10 IOT，其地址为：

https://developer.microsoft.com/en-us/windows/iot

这一点是很有象征意义的，这表明Raspberry Pi及其社区的影响力，已经到了MS这样的巨头也不能无视的地步了。

## Raspberry Pi的成功之道

嵌入式开发板这种东西，在国内已经有十多年的历史。我至今仍然记得，2007年的时候，公司的一套类智能手机的开发板，居然要3000元。所以宝贝的不得了，不相干的人根本没机会把玩。

从知名度来说，友善之臂和周立功，算是国内开发板卖的比较好的了，但前者日子过得一般，后者的主要业务也转向工控领域。

那么Raspberry Pi的成功之道是什么呢？我个人总结起来，有以下几点：

- **把握住了市场对于廉价计算的需求**

单片机讲究价格便宜，性能够用就好，而PC追求功能强大。因此，在单片机和PC之间，存在一个巨大的细分市场。这个市场既需要强大的计算能力，也需要便宜的价格。Raspberry Pi很好的满足了这一点。

- **通用的计算平台**

很多手机开发板的计算能力和Raspberry Pi类似，但为什么Raspberry Pi取得了成功呢？因为，手机OS主要面向普通用户，对于程序开发不太友好，而Raspberry Pi则更多强调它是一个功能完整的PC。

它使用了普通的桌面Linux，集成了完整的开发环境，对于小程序，甚至可以直接在Raspberry Pi上编译执行，就和在PC上一样。

一般的服务器应用，如Apache等，也可以像在PC上那样安装运行。这些都使得它的应用场景较手机平台有了极大的扩展。

而国内的开发板，很多仍然停留在手机开发板的阶段，对于通用计算，理解支持都不到位。

- **开放的态度**

Pi的开放不仅体现在它使用了很多开源软件，更在于它的软硬件都是开源的

这样，也就给了极客群体扩展使用它的机会，反过来又促进了Raspberry Pi的发展。Raspberry Pi和极客群体之间的互动，使得它突破了产品或平台的限制，而构成了一个有机的生态系统。

反观国内的开发板生产商，或曰“解决方案提供商”，实际上陷入了一个怪圈。它们为了推销自己的硬件或者软件，而有意对某些部分闭源。但实际上，生态那么差，你就算免费我都懒得用。因为，嵌入式平台都是专有平台，需要程序员投入额外的精力，去理解一些离开该平台就用不到的知识，而这是需要成本的。

Raspberry Pi成立之初的非营利性质，反而帮助它们赚到了这个细分市场中最多的钱，这对于国内众解决方案提供商，真是一个莫大的讽刺。

- **完善的服务**

很多国内厂商提供的所谓服务，无非也就是建个网站，让人下载一些资料而已。这样的等级实在太低了。

Raspberry Pi建有专门的软件仓库，安装软件就和PC上的Ubuntu一样方便。

这里，我们可以拿友善之臂的Nano PC作为一个对比。

两者的设计风格和外设接口，基本一致。Nano PC T2的硬件略好于Raspberry Pi 3B，好得不多，价钱也基本相当。

但是，资料、软件、生态，完全没得玩啊。你就算再便宜50块钱，我也会选择Raspberry Pi。新手绝对不推荐Nano PC！

唯一值得欣慰的是，友善之臂也开始在Github上创建自己的代码仓库，并借助了Debian的软件仓库，这在一定程度上，挽回了一些劣势。

## 卡片PC

常见的卡片PC，除了Raspberry Pi之外，还有Intel的NUC。但是后者除了体积小之外，售价和普通PC相当，不适合当玩具。

## Raspberry Pi的成功案例（不定期更新）

http://dcaoyuan.github.io/papers/rpi_cluster/component.html

这是一个Raspberry Pi的集群。

## Raspberry Pi 3B初体验

采购的Raspberry Pi 3B，今天（2016.5.10）终于到货了，比想象中要小巧一些。这里需要注意的是，35美元（或者类似价钱的RMB），除了板子之外，什么都没有。你必须自己准备电源和TF卡，好在这些东西都是标准件，并不难找。

### 安装OS

官方推荐使用NOOBS，但其实直接烧镜像更简单快捷。这里我使用的是Raspbian OS。

安装步骤：

1.列出设备。

`lsblk`

2.烧写。

`unzip -p 2018-11-13-raspbian-stretch.zip | sudo dd of=/dev/sdX bs=4M status=progress conv=fsync`

上述命令中的`progress`用于展示进度条。

官方步骤：

https://www.raspberrypi.org/documentation/installation/installing-images/linux.md

参考：

https://zhuanlan.zhihu.com/p/33030757

为树莓派装上CentOS 7系统
