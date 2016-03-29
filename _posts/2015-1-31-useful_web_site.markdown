---
layout: post
title:  有用的网址集合, IT杂谈, 沉思录, Fedora, WireShark
category: technology 
---
# 有用的网址集合

## 教程类

http://packages.ubuntu.com/

使用apt-get获取软件虽然方便，但是从ubuntu的源获得的软件包和直接使用源码编译安装的包相比，包中的各个文件被分散在好多个文件夹中，查找起来很不方便。这时可以到这个网址，去查找软件包里的文件清单，以弄清楚XX软件官网上所说的YY文件在ubuntu中到底放在哪里。

http://softwaredev.blog.sohu.com/105412003.html

C++库大全

http://linux.die.net/man/

Linux手册（相当于Linux的MSDN）

www.alldatasheet.com

可以查找各类芯片的手册。

www.hellogcc.org

一个有关GCC和GDB的博客。其中的大牛teawater（朱辉）开发了一个Linux动态跟踪器KGTP，他的blog：http://teawater.github.io/

http://www.linuxvirtualserver.org/zh/lvs1.html

章文嵩博士的LVS经典论文，非常值得一读。

LVS的代码已经集成进Linux内核，在net/netfilter/ipvs目录下。

http://blog.csdn.net/leixiaohua1020/

中国传媒大学雷霄骅博士的blog，专注ffmpeg等的音视频研究。

## 工具软件及其官网

Box2D---www.box2d.org

一个游戏物理引擎。

WinPcap---www.winpcap.org

一个windows下的网络抓包工具。

Bonobo Git Server---www.bonobogitserver.com

一个基于IIS的Git服务器。操作简单，但是没有提供文件夹一级的权限管理。

CommMonitor---http://www.ceiwei.com/CommMonitor.html

一个串口监控工具。

Android FFmpeg---https://github.com/appunite/AndroidFFmpeg

Android上的一个FFmpeg开源实现。

Conky---http://conky.sourceforge.net/

自由软件，用于X视窗系统的系统监视。

ocenaudio---http://www.ocenaudio.com.br/download

用于音频剪辑处理的工具。免费但不开源。

https://www.tulingss.com/

一个技术类的搜索引擎。

## 有用的blog

http://www.tinylab.org/learn-x86-language-courses-on-the-ubuntu-qemu-cs630/

http://www.tinylab.org/using-qemu-simulation-inserts-the-type-system-to-produce-the-whole-process/

这两篇文章讲述如何使用qemu运行各种汇编程序和微内核

## 几个开源软件代码下载地址（国内可访问）

GNU

ftp://mirrors.ustc.edu.cn/gnu/

GNOME

http://ftp.gnome.org/pub/GNOME/sources/

# IT杂谈

这篇文章太短，所以加些料谈一些技术之外的东西，就不新起一篇了。不排除将来写的多了，将之单独为一篇的可能，现在就这样吧。

## Bill Gates和MS-DOS

众所周知，MS-DOS的最初版本，是西雅图电脑公司的Tim Paterson写的。但这是否和Bill Gates一点关系都没有呢？

其实不然。Tim Paterson写86-DOS只花了6个星期，就算他是个大牛，也断没有这么快的道理。这只能说明他在做这个东西的时候，手里已经有了相当多的素材，使得他只需要做少量的工作即可。

素材是什么呢？Wiki上给出了答案：CP/M和FAT。

前者是Digital Research公司的产品，也就是那个由于价钱高，而被IBM在选择PC OS时放弃的公司。CP/M 8-bit是70年代中期最流行的OS。但到了1978年以后，随着技术的扩散和相关书籍的出现，这在当时的技术圈里已经是大路货了。

后者和Bill Gates可就大有关系了。Wiki上告诉我们，Bill Gates是FAT的两个发明人之一。

从以上的信息可以分析出，Tim Paterson虽然不在MS，但是和Bill Gates的关系可不是一般的密切。事实上Tim Paterson在编写86-DOS之前的工作，就是帮MS的软件设计硬件卡。（当时的PC，处理能力有限，很多软件都被设计成了硬件卡的形式。老的PC用户应该对286时代的汉字卡和386时代的音视频解压卡还有些印象吧。史玉柱就是靠卖汉卡起家的。）所以这也成为Bill Gates能够知道86-DOS存在，也敢于花大价钱买的重要原因。此外，两人的岁数相当，Bill Gates只比Tim Paterson大两岁，基本上就是同一个战壕的兄弟。我想这也是Tim Paterson三进三出MS的重要原因。

顺便提一句，2.5W美元的价格其实是个公道价，微软只买走了使用权，没买走版权，这也为之后西雅图电脑公司和MS之间的官司埋下了伏笔。相比之下，两年前Steve Jobs只花了1.3W美元就搞定了OS。

归根到底，西雅图电脑公司只是个硬件公司，它并不重视软件的价值。所以虽然它的软件技术比MS这样的软件公司还强，但是却不懂得如何利用软件。因此没有把握好时代的脉搏，让MS占了便宜，也就不足为奇了。

## CP/M和BeOS

这两个东西的共同点，相信熟悉那段历史的人都能看的出来。没错，这就是：

1.两者都是技术上的先行者和领先者，而且直到最后失败，也不是由于技术被反超导致的。

2.两者都有很好的机会，能够改写历史。

3.两者都是由于要价过高，而被机会所放弃。

4.他们的对手尽管有种种不足，但是最终改写了历史。他们是MS-DOS和Mac OS X。

## Robert Love

这几天研究Android源代码，发现了一个牛人——Robert Love。最初注意他是因为他的姓氏挺有意思的。没想到过了几十分钟，就在另一个嵌入式操作系统方面的课件中看到了他的名字。他是Preempt Linux的作者，也是Linux Kernel Development一书的作者，同时还是Android的日志系统的作者。

最关键的是，他是1981年出生的人，比我也就大一点儿。而Preempt Linux是他2001年的作品。那个时候我貌似连C语言都没怎么弄利索。。。

至于国内的牛人，有个叫李云的诺西工程师，写了本嵌入式方面的书感觉还不错，不是随便复制粘贴的东西，可以看看。不过一者，李云比我大好几岁，二者他水平虽然比我高，但还没到仰视的地步。所以终究比不了那些老外的牛人啊。。。

还有一个关于他的笑话：Robert Love是佛罗里达大学的毕业生，这所大学在名校众多的美国，只是个不入流的二本院校。于是有人调侃到：千万别去佛罗里达大学上学，因为Robert Love是从那里毕业的，而Robert Love是研究Linux Kernel的，Linux Kernel是无趣的，所以佛罗里达大学也是无趣的。

这个笑话见诸他的个人主页，但不知道是否真有此事。如果有的话，他的影响力可见一般。就像刘路之于中南大学一样。

# 新的开始

PS：这是一篇自己写于2011.11的文章，转眼间已经3年半过去了。

又是一年多没更新了。告别了北京，告别了第二家公司，开始了第三段程序员生涯。一直以来对移动互联网很有兴趣。第一家公司同一批的同事之中，绝大多数进了互联网行业，部分就在移动互联网行业。我则阴差阳错的进入了芯片设计行业，虽然设计的还是软件，而非芯片本身。

一直想借着这次换工作的机会，去一个移动互联网公司，但是很少有公司对我第一家公司的经验感兴趣。从现实角度，我也不可能接受新人的薪水，重新开始。所以也就随遇而安了。

当然，到了我这个阶段，公司层面也并非什么障碍。爱因斯坦的相对论是当职员的时候提出的。钱德拉塞卡的钱德拉塞卡极限是在从印度到剑桥大学的船上推导出来的。有之前的相关经验，自己私下里研究一些移动互联网的相关技术，不过是轻车熟路而已。

最近这半年，软件层面的长进，比较有限。主要是新学习了SPARC体系结构和浮点运算协处理器的东西，以及对链接器的功能有了更深入的理解。其他的也就没啥了，倒是数学知识长进了不少。

既然没有野心去自己创业，那么继续加强自己的技术水平，“将以有为也”，也是不错的选择。

首先是数论的研究。印度人在这方面很有天份。拉马努扬发现了分划数的公式，实在很难想像一个有着40多个符号的公式是人能想出来的。

还有玻色，他所发现的玻色-爱因斯坦凝聚，虽然没有为他本人带来诺贝尔奖，但是却给后来者带来了3个诺贝尔奖。

最后就是钱德拉塞卡，天体演化学的奠基人，其诺贝尔奖获奖成果是在19岁，还没有上大学之前作出的，尽管他直到晚年才获得了诺贝尔奖。美国的钱德拉X射线太空望远镜，就是为了纪念他而命名的。

其次是信号与系统，大二时本来是学过的。可惜荒了快十年，基本都还给老师了，好在目前工程上只需要使用结论就可以了，不像学校里偏重于推导。所以，研究的广度，竟然比学校广了不少。主要就是各类FFT算法，还有载波的各种调制方式。

还有矩阵论，工作上主要是用它来求解多元线性方程组。如果不是线性方程组的话，还要使用Jacobi矩阵将之线性化，然后矩阵求逆，计算出最终结果。当然还有Kalman滤波的使用，考虑到Kalman滤波和Google的PageRank算法都是基于随机过程的，所以Kalman滤波的应用范围应该还是很广的。

可惜对随机过程完全不了解，打算最近研究研究。

# Fedora

Fedora作为主要的Linux发行版之一，我虽然用的不多，但实际上这却是我最早接触的Linux发行版。后来换用Ubuntu，很大的原因是因为：这是Google为Android选择的开发平台。

最近因为工作需要重新捡起了Fedora。但公司所用的版本太过古老，还是2009年的Fedora 12。所以想了一下，开始试用最新的Fedora 22。这里是使用过程中的一些操作笔记。

## 安装

https://getfedora.org/

这是官方的下载地址。这里我用的是Workstation版本。

Fedora 22的默认桌面是GNOME 3.16，这一版的外观借鉴了Mac OS X的一些设计，让人眼前一亮。

## 安装软件

Fedora 22使用dnf替代yum。因此安装基本gcc开发环境，可用如下命令：

`dnf install gcc kernel-devel patch bison flex subversion`

如果下载速度较慢的话，可以在/etc/dnf/dnf.conf最后添加：

`fastestmirror=true`

保存后，执行

{% highlight bash %}
$ sudo dnf clean all
$ sudo dnf makecache
{% endhighlight %}

此外，和Ubuntu一样，Fedora也有自己的网站可以查询软件包信息：

https://admin.fedoraproject.org/pkgdb/

## 共享文件夹

我用的是VirtualBox的虚拟环境，因此除了在VirtualBox中，设置共享文件夹之外，还需对Fedora进行如下操作：

1.添加用户到vboxsf中。

`usermod -a -G vboxsf <your user name>`

2.重启。（这一步必不可少，否则上面的配置不会生效。）

这样就可以在Fedora中浏览共享文件夹了。

# WireShark

WireShark是一个网络协议包分析工具，最初名叫Ethereal。它的官网是：

www.wireshark.org

## 在ubuntu上的安装

`sudo apt-get install wireshark`

安装好了之后，还不能立即使用。需要给/usr/bin/dumpcap提升权限，才能使用WireShark的抓包功能。否则会出`no interfaces`的错误。

提升权限的方法有：

1.root方式。

命令行：`sudo wireshark`

桌面图标：`gksudo wireshark`

2. 非root方式，这也是官方推荐的方式。

`sudo dpkg-reconfigure wireshark-common`

`sudo usermod -a -G wireshark <your user name>`

`sudo chgrp wireshark /usr/bin/dumpcap`

`sudo chmod 4750 /usr/bin/dumpcap`

`sudo setcap cap_net_raw,cap_net_admin=eip /usr/bin/dumpcap`

最后注销当前用户，重新登陆即可。

## 过滤器规则

WireShark以丰富的过滤器著称，现将我使用到的过滤器规则摘录如下：

`ip.src == 10.3.9.234 || ip.dst == 10.3.9.234`

过滤源地址和目标地址。

`tcp.segment_data matches Bob`

匹配特定字符串。

`tcp.stream eq id`

将一次TCP交互的包过滤出来，id表示是第几次交互。

