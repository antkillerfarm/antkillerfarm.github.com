---
layout: post
title:  Raspberry Pi（二）, 运维工具集
category: technology 
---

* toc
{:toc}

# Raspberry Pi

### 登录Raspberry Pi

1.串口登录

Raspberry Pi 3B的GPIO接口图如下所示：

![](/images/article/raspberry_pi_gpio.jpg)

其中，串口和树莓派的连线方式如下：

| 串口 | 树莓派 |
|:--:|:--:|
| VCC | +3.3V |
| RX | TXD |
| TX | RXD |
| GND | Ground |

这里我犯了一个错误，将VCC接到+5V上，差点将串口转接板烧掉。用户如果无法判断自己串口设备的VCC，建议先接上+3.3V试试。

然而这样做之后，串口并不稳定，无法顺利登录设备。原因在于：

http://ju.outofmemory.cn/entry/245310

树莓派3串口(UART)使用问题的解决方法！！！！

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

Raspbian和Ubuntu一样，使用apt来安装软件包。但它默认使用的是国外的软件源，因此速度很慢。

我们首先在下面的网页中，查找适合的镜像软件源：

http://www.raspbian.org/RaspbianMirrors

国内的话，一般推荐清华和中科大的源。东软的源，虽然支持的开源软件数量最多，但速度完全不敢恭维。

http://mirrors.tuna.tsinghua.edu.cn/raspbian/raspbian/

http://mirrors.ustc.edu.cn/raspbian/raspbian/

修改源的方法：

`sudo vi /etc/apt/sources.list`

安装TightVncServer：

`sudo apt install tightvncserver`

首次运行tightvncserver，会让你设置远程访问的密码。

相应的客户端下载地址：

http://www.tightvnc.com/download.php

`sudo apt install tigervnc-viewer`

打开TightVncViewer，在Remote Host中填入Raspberry Pi的IP地址，注意IP后需要加“:1”，否则连接不上。

### 文件传输

Raspberry Pi默认支持SFTP协议。打开FileZilla，主机一栏填写：

sftp://192.168.1.102 （换成您的树莓派的IP地址。前面的sftp://一定要加）

填入登录的用户名、密码，点击快速连接即可。

### 扩展系统分区

官方镜像中，root分区只有2～3GB，这个对于较大的TF卡来说，是一个很大的浪费。因此，需要扩展系统分区。

方法一如下图所示：

![](/images/article/raspberry_pi_resize1.png)

![](/images/article/raspberry_pi_resize2.png)

方法二：

`sudo raspi-config`

## VNC进阶教程

### 开机自动启动

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/other/vncserver

将上面的脚本放到/etc/init.d/vncserver中。

然后执行以下命令：

`sudo chmod +x /etc/init.d/vncserver`

`sudo update-rc.d vncserver defaults`

`sudo service vncserver start`

### 调整分辨率

`vncserver -geometry 1440x900 :1`

并不是所有的分辨率Pi都支持。Pi的默认分辨率是800x600，其他支持的分辨率还有1440x900。（不全，有兴趣的自己慢慢试）

## 修改hostname

默认的hostname是raspberrypi。将/etc/hostname和/etc/hosts中的相应字段，改成你想要的名字，保存重启即可。

## Raspberry Pi 4

2019.6

时间过的好快啊，现在Raspberry Pi 4也出来了。

| 串口 | Raspberry Pi 3 | Raspberry Pi 4 |
|:--:|:--:|:--:|
| CPU | BCM2837 | BCM2711 |
| CPU core | 4 Cortex-A53, 1.2GHz | 4 Cortex-A72, 1.5GHz |
| GPU | 400MHz VideoCore IV | 500MHz VideoCore IV |
| USB | 4 USB 2.0 | 2 USB 2.0+2 USB 3.0 |
| Video | 2560x1600 | Dual 4K |

>VideoCore是Broadcom旗下的GPU产品。

## Raspberry Pi Pico

2021.1，树莓派基金会发布了首款微控制器级产品：Raspberry Pi Pico。

https://mp.weixin.qq.com/s/rfOTxVUpqyfqNQQJh_WhNg

树莓派Pico VS Arduino该选哪个？

## 参考

树莓派的博通芯片是专门定制的，这帮英国佬精的很，现在github上很多树莓派项目都深度适配博通的那款芯片，比如有一个用来驱动spi屏幕的fbcp项目，只针对树莓派自己的videocore来写，而山寨派一般用的是国产全志芯片，虽然配置是高，但从硬件上就决定了山寨派不能直接进入树莓派生态。

国产也有各种派，芒果派，菠萝派啥的，价格是树莓派的1/2～1/4，性能一致，生态系统不如树莓派丰富，需要一定经验才能玩。

---

https://mp.weixin.qq.com/s/YeoBILcLy2LNzDAnxygKAg

如何手动养成一只“咖啡女仆”？

https://mp.weixin.qq.com/s/CLwGEVgkGk7Zm7Acp8NAmg

树莓派上利用Tensorflow实现小车的自动驾驶

https://mp.weixin.qq.com/s/1wxA7jnCgmXt5j9DXxH1UA

超有趣！手把手教你使用树莓派实现实时人脸检测

https://mp.weixin.qq.com/s/tW54lcv9aRz9xXC9V-DsWw

Keras+树莓派，130行代码找到圣诞老人

https://mp.weixin.qq.com/s/8zD3GqbQrQnZmuD59OoFuQ

让树莓派记下你的笑颜

https://mp.weixin.qq.com/s?__biz=MzU1OTMyNDcxMQ==&mid=2247484969&idx=1&sn=29d04fbb56a4464b08407eb87d26325e

基于源代码为Raspberry Pi设备构建TensorFlow

https://mp.weixin.qq.com/s/onN61A13xuJzNk9zE0hM-w

你有特斯拉，我有树莓派，纯手工打造车载车牌识别检测系统，家用车秒变智能车

https://mp.weixin.qq.com/s/rqIZSX-zRNQ3uyae3LlnZQ

树莓派也能实时训练agent玩Atari

https://mp.weixin.qq.com/s/pPTaxiCOTL_Y89TuKGHQmg

微软放弃的游戏被他们复活了：Windows经典“三维弹球”现实版，CAD建模、Arduino编程、数控机床打造，硬核致敬童年

https://mp.weixin.qq.com/s/mgdDjjIJX4y1-DXzazDhPA

不会编程的外国小姐姐，3天、850块，徒手用树莓派DIY了个数码相机

https://mp.weixin.qq.com/s/rM3SkUiQIxsGz9MNIg1skQ

用树莓派DIY波士顿机器狗，帮你省下50万：教程开源，人人皆可上手

https://zhuanlan.zhihu.com/p/301860804

如何把树莓派400打造成小霸王游戏机，Retropie包含上万经典老游戏

https://www.zhihu.com/question/339388119

树莓派做网站靠谱不？

https://mp.weixin.qq.com/s/MEiee-2zlamaqFXv2ih4WQ

树莓派相机的自动曝光算法

https://zhuanlan.zhihu.com/p/668807915

Beepy——基于香橙派 Zero 2W 的 DIY 开源掌上电脑配置与优化分享

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

和Jenkins同类型的工具还有Travis、Codeship、Strider等。

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
