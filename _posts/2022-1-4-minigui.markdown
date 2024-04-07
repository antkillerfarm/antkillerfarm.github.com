---
layout: post
title:  MiniGUI, RT-Thread, Spring
category: technology 
---

* toc
{:toc}

# MiniGUI

2015.5

MiniGUI虽然是开源项目，但是并没有提供版本库，也就没有版本历史了。

作为一个曾经辉煌的国产开源项目，MiniGUI在00年代曾与LVS、SCIM并称为三大国产开源软件。更是当时嵌入式GUI开发方面不多的几个选择之一，江湖地位也很高。

但是自从Android横空出世之后，MiniGUI的日子就不太好过了。其主线版本停在了v3.0.12（2010.10），距离它的配套开发工具产品mStudio的推出不过3年时间。等于是辛辛苦苦将产品的版图扩充完整，却发现产品本身已经没有市场前途了……

出现这种情况的原因，其实与MiniGUI本身没有太大关系。当年的主要竞争对手Qt和GTK在嵌入式领域同样一败涂地。这一方面是由于Android实在是太强太好了，另一方面更重要的是硬件的进步，导致原先的这些资源消耗较小的GUI系统，变得不是那么非用它不可了。而比较功能的话，一个单纯的GUI和一个全功能的框架之间根本就没有可比性，就连最重量级的Qt也一样不行。

MiniGUI的作者魏永明所创建的飞漫公司，我曾经在2009年的时候，去那里面试过。当时的考官是个中年人，也不知道是不是魏老师本人。应该说笔试面试的情况还是很好的。这家公司的笔试题比较基础细致，在那次找工作的笔试中算比较难的，超过了腾讯。但给的薪水却非常低，甚至比我毕业时的第一份工作还低。可见在国内，以开源软件开发为主业的公司，日子过的还是很困难的。

MiniGUI惨淡之后，飞漫公司转战移动APP领域，但从网站上的版本更新状态来看，这次的转型似乎并不成功。

由此延伸开去，Android兴起前后，一大堆公司的命运都被随之改变。

1.博望科技。我所关注的牛人李先静之前所在的公司。当初为了开发彩信相关的功能，找到了牛人李的博客，于是也就持续关注了这个公司好几年。博望科技早先的目标是基于GTK搭建一个智能手机平台。

可惜直到Android出来的时候，完工度也不高。后来及时掉头，研究Android技术，成为最早的一批国产Android手机制造商和解决方案提供商。可惜从根本来说，Android的目的是降低智能手机的门槛。随着会的人越来越多，这类解决方案商就变得可有可无了。

同样的例子还有德信无线。

牛人李由于过度劳累，大病一场，大概3年前离开了该公司，至今仍处于半修养状态。

应该说，Android最开始的思路，实际上就和博望不同。Android盯着的是未来的硬件，或者说是硬件期货，所以一开始硬件的规格就比较高。尤其在最初的时候，不是顶级配置根本跑不了Android。而飞漫、博望的思路是如何利用现有的硬件做出好的产品，或者说是用尽可能少的钱（这通常意味着硬件的规格是向下的，或者至少是维持原状的）做出产品来。

但硬件的规格总是向上发展的，于是最初顶配才能跑的Android，现在随便什么硬件都能跑的了。这个时候，飞漫、博望之类的产品也就无人问津了。毕竟GUI应用，在嵌入式中还算是重量级的，真的低端设备，如传感器之类的，也用不上这个。

我之前的公司，早先还曾经有个单核跑双系统的AP+BP融合方案，目标是造出千元级别的Android手机。可惜也是逆了技术潮流，后来的AP都已经是双核的了，根本不需要你这样的方案来节省内核数。而最终，千元或以下的Android手机被造了出来，但根本就和这个方案无关，完全得益于硬件制造成本的下降。

但单核跑双系统的虚拟化方案本身还是很有技术含量的，国内当时根本没人会。这个项目主要是法国的团队主导的。

>现在回想起来，这种方案大概和Xenomai项目差不多吧。   
>https://blog.csdn.net/pwl999/article/details/109412539   
>Xenomai(学习笔记)

2.播思通讯。中移动为了搞OMS成立的公司，我有同事在那里工作，本人也曾经去那里面试过。可惜由于技术实力太差，纵有中移动在背后站台，也还是烂泥扶不上墙。很快就被Google半年一次的更新甩在了身后。当然这也是Google的江湖地位使然。虽然播思也做出了输入法框架，但不是你主导的项目，你的再好，我也不用，很快就让你的劳动成了无用功。总的来说，这些Android改的系统，越深度定制越死的快，反倒是换皮的MIUI之类的，活的挺好的。

3.魅族。魅族在Wince时代，曾有一款深度定制的M8，当初曾寄望于成为国产的iPhone。事实上，如果没有Android的话，这个目标差不多就实现了。并不是说达到iPhone的水平，而是说除了iPhone之外，别人都没有我的好。

可惜Android的出现，使得一般的手机厂商也能造出现代的智能手机，于是魅族的这番努力，其实并没有得到多少的回报。总算魅族并不是手机解决方案商，而是制造商，没办法标新立异，却也不至于被别人甩下来。因此，魅族现在的日子，也还说的过去。

---

在本文写作的2015年，魅族尚算是第二梯队里的佼佼者，后面还有一堆第三梯队垫底。可现在（2021.9），第三梯队全军覆没，第二梯队也已经溃不成军。。。

---

2021.11

魏永明最近的发言：

笔者开发MiniGUI二十多年，知道使用C/C++如何开发界面，多写几行C/C++代码也能做出很多精妙的界面来。但有那么几年，笔者搞了几次Web前端开发，被Web前端技术开发界面的精妙之处所折服。后来再用C/C++语言开发界面，笔者心里也是百般地不情愿。要不是为了钱，都2020年了，谁愿意用C/C++开发GUI？

>这差不多也是我的感觉了，回不去的老GUI岁月。。。最近几年我也没有造小工具的兴趣了，偶尔写，也是python脚本。。。

MiniGUI从2017年开始，继续更新，现在已经是v5.0.6（2021.5）了。但从网站和访谈来看，飞漫大概也就定位于工作室了。。。毕竟魏老师真的很老了。。。

---

2022.1

好几年不做GUI了，现在的选手已经换了一轮：

https://zhuanlan.zhihu.com/p/448126678

各种GUI Builder体验TouchGFX，AppWizard，GUIX Studio，Embedded Wizard，AWTK，柿饼UI，LVGL，Qt fot MCU等

AWTK是李先静加盟周立功之后的作品。相比于魏老师这样的历史成就和包袱都很重的人，年轻一些的李先静明显已经后来居上。他养病阶段就已经转向JS开发，当时我还有点纳闷。看来GUI的JS化是一个趋势，无非有人先知先觉，而我后知后觉罢了。

目前风头最盛的还是LVGL。

官网：

https://lvgl.io/

代码：

https://github.com/lvgl/lvgl

LVGL在桌面领域，一般使用SDL作为后端。

Demo:

```bash
git clone --recursive https://github.com/lvgl/lv_port_pc_vscode
cd lv_port_pc_vscode
make
./build/bin/demo
```

---

emwin是segger开发的闭源软件，由于ST购买了它的版权，所以在ST芯片领域用的比较多。

# RT-Thread

RT-Thread是另一位我所仰慕的牛人熊谱翔的作品，一个嵌入式OS。

官网：

https://www.rt-thread.org/

熊谱翔的经历，差不多是我进入AI行业以前的经历的翻版，只是我的成就远不如他。一样都是从内核入门，GUI入道，然后进入物联网领域。所以我对他的经历很有共鸣。

RT-Thread提出的柿饼UI，是一款专注于嵌入式领域、JS脚本化开发的GUI解决方案。可谓和魏老师、李先静，英雄所见略同了。

## NuttX

NuttX是一个实时操作系统，于07年由Gregory Nutt开源，2016年被三星选为TizenRT操作系统的内核，2019年在小米的推动下正式进入Apache基金会。Fitbit最近两代的手环产品和索尼多款消费级产品都是基于NuttX开发的。

官网：

https://nuttx.apache.org/

# Spring

Spring是一个Java Web应用框架。官网：

http://spring.io/

## SSH & SSM

SSH：Spring+Struts2+Hibernate

SSM：Spring+Spring MVC+MyBatis

早期的Spring还需要和其他开源项目配合，但随着其自身实力的强大，目前已经可以用Spring全家桶覆盖各个层面了。

## Ubuntu安装Eclipse、Spring

1.安装Eclipse

`sudo apt install eclipse`

2.安装Spring

`sudo apt install libspring-web-portlet-java`

注意：ubuntu软件仓库中还有一个叫做spring的游戏引擎，不要弄错了。

http://www.mkyong.com/spring/quick-start-maven-spring-example/

Maven+Spring hello world example

http://wiki.jikexueyuan.com/project/spring/

Spring 教程

## Restful

http://spring.io/guides/gs/rest-service/

## Spring Boot

https://www.tianmaying.com/tutorial/deploy-spring-boot-application

部署Spring Boot应用

http://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html

Spring Boot默认的配置文件

https://mp.weixin.qq.com/s/7I5OfGnGtXk2u9PtGLObCA

七个开源的Spring Boot前后端分离项目

## WebService

https://spring.io/guides/gs/producing-web-service/

# Serverless

https://mp.weixin.qq.com/s/geT7x5RG4xhD-Ro1eZvrdg

深入浅出Serverless：优势、意义与应用

https://mp.weixin.qq.com/s/6Rl1uvIeysG844W2TtGocg

一文看懂当红Serverless

https://www.zhihu.com/question/506704568

如何评价无服务器计算（serverless）的未来前景？你认为serverless有未来吗？
