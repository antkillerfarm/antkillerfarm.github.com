---
layout: post
title:  Raspberry Pi
category: technology 
---

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

Raspberry Pi官方OS是Raspbian，这是一个基于Debian的Linux发行版。除此之外，还有几个Ubuntu定制版。甚至微软也为此专门推出了Windows 10 IOT，其地址为：

https://developer.microsoft.com/en-us/windows/iot

这一点是很有象征意义的，这表明Raspberry Pi及其社区的影响力，已经到了MS这样的巨头也不能无视的地步了。

## Raspberry Pi的成功之道

嵌入式开发板这种东西，在国内已经有十多年的历史。我至今仍然记得，2007年的时候，公司的一套类智能手机的开发板，居然要3000元。所以宝贝的不得了，不相干的人根本没机会把玩。

从知名度来说，友善之臂和周立功，算是国内开发板卖的比较好的了，但前者日子过得一般，后者的主要业务也转向工控领域。

那么Raspberry Pi的成功之道是什么呢？我个人总结起来，有以下几点：

1.把握住了市场对于廉价计算的需求。单片机讲究价格便宜，性能够用就好，而PC追求功能强大。因此，在单片机和PC之间，存在一个巨大的细分市场。这个市场既需要强大的计算能力，也需要便宜的价格。Raspberry Pi很好的满足了这一点。

2.通用的计算平台。很多手机开发板的计算能力和Raspberry Pi类似，但为什么Raspberry Pi取得了成功呢？因为，手机OS主要面向普通用户，对于程序开发不太友好，而Raspberry Pi则更多强调它是一个功能完整的PC。

它使用了普通的桌面Linux，集成了完整的开发环境，对于小程序，甚至可以直接在Raspberry Pi上编译执行，就和在PC上一样。

一般的服务器应用，如Apache等，也可以像在PC上那样安装运行。这些都使得它的应用场景较手机平台有了极大的扩展。

而国内的开发板，很多仍然停留在手机开发板的阶段，对于通用计算，理解支持都不到位。

3.开放的态度。Raspberry Pi的开放不仅体现在它使用了很多开源软件，更在于它的软硬件都是开源的。这样，也就给了极客群体扩展使用它的机会，反过来又促进了Raspberry Pi的发展。Raspberry Pi和极客群体之间的互动，使得它突破了产品或平台的限制，而构成了一个有机的生态系统。

反观国内的开发板生产商，或曰“解决方案提供商”，实际上陷入了一个怪圈。它们为了推销自己的硬件或者软件，而有意对某些部分闭源。但实际上，生态那么差，你就算免费我都懒得用。因为，嵌入式平台都是专有平台，需要程序员投入额外的精力，去理解一些离开该平台就用不到的知识，而这是需要成本的。

Raspberry Pi成立之初的非营利性质，反而帮助它们赚到了这个细分市场中最多的钱，这对于国内众解决方案提供商，真是一个莫大的讽刺。

4.完善的服务。很多国内厂商提供的所谓服务，无非也就是建个网站，让人下载一些资料而已。这样的等级实在太低了。

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

### 登录Raspberry Pi

1.串口登录

Raspberry Pi 3B的GPIO接口图如下所示：

![](/images/article/raspberry_pi_gpio.jpg)

其中，串口和树莓派的连线方式如下：

{% highlight text %}
串口   树莓派
--------------------
VCC      +3.3V
RX       TXD
TX       RXD
GND      Ground
{% endhighlight %}

这里我犯了一个错误，将VCC接到+5V上，差点将串口转接板烧掉。用户如果无法判断自己串口设备的VCC，建议先接上+3.3V试试。

然而这样做之后，串口并不稳定，无法顺利登录设备。原因在于：

http://ju.outofmemory.cn/entry/245310

2.网口登录

Raspberry Pi默认支持SSH登录。这里我使用putty作为SSH客户端。

首先，用网线将Pi和PC连接到同一个局域网中。然后进入路由器的界面，如下图：

![](/images/article/raspberry_pi_ip1.png)

“客户端列表”如下图：

![](/images/article/raspberry_pi_ip2.png)

其中raspberrypi就是Pi的hostname。

选择对应的IP地址，进行SSH登录。Pi的默认用户名是pi，密码为raspberry。需要注意的是，putty首次登录不会成功，需要关闭会话，并再次登录，才能获得相关密钥，并登录成功。

3.远程桌面登录

VNC或者MS远程桌面都能登录Pi，这里使用VNC协议。

Raspbian和Ubuntu一样，使用apt-get来安装软件包。但它默认使用的是国外的软件源，因此速度很慢。

我们首先在下面的网页中，查找适合的镜像软件源：

http://www.raspbian.org/RaspbianMirrors

国内的话，一般推荐清华和中科大的源。东软的源，虽然支持的开源软件数量最多，但速度完全不敢恭维。

http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/

http://mirrors.ustc.edu.cn/raspbian/raspbian/

修改源的方法：

`sudo vi /etc/apt/sources.list`

安装TightVncServer：

`sudo apt-get install tightvncserver`

首次运行tightvncserver，会让你设置远程访问的密码。

相应的客户端下载地址：

http://www.tightvnc.com/download.php

打开TightVncViewer，在Remote Host中填入Raspberry Pi的IP地址，注意IP后需要加“:1”，否则连接不上。

### 文件传输

Raspberry Pi默认支持SFTP协议。打开FileZilla，主机一栏填写：

sftp://192.168.1.102 （换成您的树莓派的IP地址。前面的sftp://一定要加）

填入登录的用户名、密码，点击快速连接即可。

### 扩展系统分区

官方镜像中，root分区只有2～3GB，这个对于较大的TF卡来说，是一个很大的浪费。因此，需要扩展系统分区。

方法如下图所示：

![](/images/article/raspberry_pi_resize1.png)

![](/images/article/raspberry_pi_resize2.png)

