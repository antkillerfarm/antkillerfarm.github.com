---
layout: post
title:  版本管理工具的前世今生, Kannel, Win10历险记, 运维工具集, 负载均衡
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

2015.8

我大概在4、5月间，听说了Win10免费升级的消息。于是一直很期待7月29日的到来。果然到了29日当天下午的时候，公司电脑就收到了升级的通知。然而由于网速不给力，当天并未对公司电脑进行升级。倒是晚上在家里的电脑上，虽然耗时2小时，但却一路顺利的升级成功。

Win10给人的第一感觉，其实和Win8差不多，只不过加了个更像Win7的开始菜单而已。不过既来之则安之，一段时间用下来，总算还是要比Win7强不少的。

又过了几天，公司的电脑也升级成功。正得意间，忽然发现VirtualBox在Win10下工作不正常。查了VirtualBox官网方知，其目前尚不支持Win10 Host。于是不得不重新降级到Win7。看来尝鲜也是有得有失的。对于工作用的电脑，有的时候够用就好，没必要什么都求新的。

2020.11

由于硬盘出了些故障，原来在备份分区准备着的Ghost镜像，恢复系统失败，不得不使用U盘启动并重装系统。

除非是从Win7升级，否则Win10强制要求硬盘是UEFI启动的。修改硬盘分区格式的方法如下：

https://docs.microsoft.com/zh-cn/windows-server/storage/disk-management/change-an-mbr-disk-into-a-gpt-disk

将MBR磁盘转换为GPT磁盘

应该说这次U盘安装Win10的体验还是不错的。MS官方提供了相关工具，一切都是傻瓜操作。不像早期的光盘安装，还要注意启动盘和数据盘的差异。当然U盘其实也存在这两者的差异问题，不过工具已经帮你处理好了。

这样一来，光盘确实是没啥用了，即使我的第一台PC的光驱，实际使用也不过数十次，更多的主要是用于重装系统，如今连这也用不着了。

最后，吐槽一下系统的体积，Win10启动需要8G以上的U盘，DVD看来也不够用了。

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

# 负载均衡

正向代理，“它代理的是客户端，代客户端发出请求”。

反向代理，“它代理的是服务端，代服务端接收请求”。

http://blog.csdn.net/gzh0222/article/details/8540604

lvs、haproxy、nginx负载均衡的比较分析

https://mp.weixin.qq.com/s/SO1ZLSPaRkk8WwUl9u-JPA

仅需这一篇，吃透“负载均衡”妥妥的

https://mp.weixin.qq.com/s/3KZ99d94yqRDxEByn7nGWg

QPS比Nginx提升60%，阿里Tengine负载均衡算法揭秘

https://mp.weixin.qq.com/s/XsyXkaOIJ3m8sdCt8Pl5Ig

计算密集型服务的负载均衡策略

https://mp.weixin.qq.com/s/SmpUPJRK0TMdA0rkb_Q6VQ

什么是集群？什么又是负载均衡？

https://mp.weixin.qq.com/s/sCGI3XPga5gTZF0iskJlMA

全网最详尽的负载均衡原理图解

## Nginx

https://mp.weixin.qq.com/s/31DGDYpDHcYg-4qM4OczxA

深入浅出Nginx

https://mp.weixin.qq.com/s/h9sXZc4Y62f4KOSVXAB1Pg

Nginx基本功能极速入门

https://www.cnblogs.com/wcwnina/p/8728391.html

Nginx相关介绍

https://mp.weixin.qq.com/s/aCAuJQEB2pmL92OTEb_4Xg

Nginx为什么这么快？

https://mp.weixin.qq.com/s/3GSuB1tLe-G5hZPUiAXuVA

Nginx，为什么依旧这么“香”？

https://blog.csdn.net/yujing1314/article/details/107000737

搞懂Nginx一篇文章就够了

https://mp.weixin.qq.com/s/-8Ka8nMYpH7cipzfm36utw

写给小白的Nginx文章

https://mp.weixin.qq.com/s/gd59U50tlJPa4B4avXRG1Q

Nginx架构浅析

https://mp.weixin.qq.com/s/eUindWNZiCv_tSLQeYfmLg

Nginx常用配置清单
