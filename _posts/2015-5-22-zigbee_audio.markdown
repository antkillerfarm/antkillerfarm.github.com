---
layout: post
title:  Zigbee音频, 6LowPAN, 各种智能家居通信技术比较, 边缘计算
category: technology 
---

* toc
{:toc}

# Zigbee音频

## 前言

本文是对Zigbee传输音频数据功能的一个资源汇总分析贴。

## 网上解决方案分析

### 基于MCF5213及Zigbee技术实现无线对讲系统

http://www.go-gddq.com/html/QiTa-ZongHe_tx/2011-03/580806p2.htm

要点：

采用G.726压缩音频，采样率8kHz，单次采样16bit，压缩比1:8，数据量16kbps

### 基于ZigBee节点的智能家居系统语音控制设计

http://www.elecfans.com/video/yinpinjishu/20141216360879.html

要点：

采用专门的芯片处理语音输入及识别，Zigbee仅传输控制指令，不传输音频数据。

### 基于ZigBee的无线音频传输系统的设计与实现

http://max.book118.com/html/2014/0407/7392964.shtm

要点：

1.长安大学硕士论文

2.采用ADPCM压缩音频，采样率15.2kHz，单次采样16bit，压缩比1:4，数据量61kbps

3.该码率大大超过TI官方提供的典型值，因此系统不稳定。论文最后指出，该方案只能点对点传输，不可中继路由。同时协调器亦不可传输其他任务的数据。

### ZIGBEE音频设计

http://download.csdn.net/detail/hongge586/5279694

要点：

1.西南科技大学本科论文

2.采样率8kHz，单次采样12bit，压缩算法未知，压缩比1:3, 数据量32kbps

3.不使用Zigbee协议，而仅使用802.15.4协议。适用于点对点传输，不可中继路由。

4.文中提到Jennic公司的JN5139实现了32kbps的基于802.15.4的语音传输。

### 基于Zigbee技术带有万能接口的无线耳机

http://www.aptchina.com/zhuanli/3684478/

要点：

1.这是一篇2006年的专利文档。

2.经查市面上并无Zigbee耳机产品面世。

## 结论

1.使用Zigbee传输音频数据，瓶颈在于数据传输速率，16kbps可以使用普通Zigbee协议，32kbps就只有使用802.15.4协议才可以达到商用程度。

2.音频压缩算法是系统实现的关键。

# 6LowPAN

## IPv6和IPv4的区别

![IPv4 vs IPv6](/images/article/ipv4_vs_ipv6.png)

一般来说，网络协议就是一层层的组帧和解帧的过程。因此比较帧头的差异，无疑是个建立感性认识的好方法。上图就是IPv6和IPv4在帧头方面的区别。

参考：

https://mp.weixin.qq.com/s/_-KrxPywEahojmA7_9ojKw

将IPv6照进现实，我们需要做些什么？

## IPv6和6LowPAN

在Zigbee技术的体系结构中，Zigbee只是一个mac子层协议。其下的mac层采用的是802.15.4协议。除了Zigbee协议之外，802.15.4协议之上还支持其他的mac子层协议，其中最有名的是6LowPAN。

![IPv6 & 6LowPAN](/images/article/6lowpan.png)

上图是IPv6和6LowPAN的帧结构图。其中上方是IPv6，下方是6LowPAN。由于IPv6帧的最大大小为1280字节，而6LowPAN只有127字节。因此6LowPAN通过特定的帧头，将IPv6帧分包并重新组包。

从上图可以看出，IPv6和6LowPAN的关系是十分密切的，两者的转换也比较容易。

这也是6LowPAN相较于Zigbee协议的一大优势。由于CC2530硬件支持802.15.4协议，因此只要替换软件协议栈就可以支持6LowPAN。目前支持是6LowPAN的软件协议栈有Contiki和TinyOS。

## 6LowPAN的Linux支持

Linux Kernel主线在v3.2的时候加入了对6LowPAN的支持，而此前的v2.6.31已经集成了对802.15.4的支持。

事实上在内核代码的/net文件夹下，可以找到ieee802154、6lowpan、mac802154等相关的文件夹。

问题来了，以CC2530这么差的配置，Linux内核无论怎么裁剪，都不可能运行起来，否则就没有Contiki项目什么事了。那么内核中的6LowPAN是在什么样的平台上运行的呢？

首先明白一点，这些硬件相关的代码肯定不在6lowpan文件夹下，因为6lowpan是个上层协议，并不直接和硬件打交道。

所以只有到802.15.4里寻找答案了。经仔细查看代码发现ieee802154_ops这个结构保存了实际硬件操作的驱动接口。通过查找ieee802154_ops出现的地方，可以看到内核现在支持的硬件有at86rf230、cc2520、fakelb和mrf24j40。其中fakelb是个虚拟设备。

那么CC2520和CC2530是什么关系呢？

CC2520是802.15.4收发器，没有MCU，不能跑软件，只能作为上位机的外设存在。而CC2530是解决方案，不仅集成了收发器，还集成了MCU，可以独立存在。

## Contiki

http://contiki-os.org/

这是Contiki的官网。

http://blog.csdn.net/xukai871105

这是国内某牛人的blog。

https://github.com/xukai871105/contiki_cc2530_iar/

这是该牛人将Contiki移植到IAR下的源代码。

## 无人机通信

无人机通信一般采用微波通信，微波是一种无线电波，它传送的距离一般可达几十公里。频段一般是902－928MHZ，常见有MDSEL805， 一般都选用可靠的跳频数字电台来实现无线遥控。

无线图像回传技术采用COFDM调制方式，频段一般为300MHZ,实现视频高清图像实时回传到地面，比如NV301等。

# 各种智能家居通信技术比较

2015.12

<table>
  <tr>
    <th>名称</th>
    <th>概况</th>
    <th>优点</th>
    <th>缺点</th>
  </tr>
  <tr>
    <td>Zigbee</td>
    <td>基于IEEE 802.15.4的一种近程（10米~100米）、低速率（250Kbps标称速率）、低功耗的无线网络技术。</td>
    <td>具有低复杂度、低功耗、低速率、低成本、自组网、高可靠、超视距的特点。</td>
    <td>协议Profile比较复杂，导致厂家的各自为政，产品兼容性较差。</td>
  </tr>
  <tr>
    <td>Z-Wave</td>
    <td>由丹麦公司Zensys所一手主导的无线组网规格。</td>
    <td>Z-wave联盟的成员均是已经在智能家居领域有现行产品的厂商，产品兼容性好。</td>
    <td>技术把持在Zensys手中，产业链较封闭，芯片可选余地不大，渐趋式微。</td>
  </tr>
  <tr>
    <td>KNX</td>
    <td>Konnex协会提出的家庭、楼宇自动化的有线解决方案。</td>
    <td>历史悠久，功能丰富，标准完善，厂商支持度比较高。在西欧和北欧比较流行。</td>
    <td>虽然新的标准中可以使用868 MHz的RF进行无线传输，但总的来说，还是个有线方案。适合楼宇建筑时的预装，而不适合后期的智能改造。</td>
  </tr>
  <tr>
    <td>BlueTooth Low Energy（BLE）</td>
    <td>基于IEEE 802.15.1的的低功耗无线通讯技术。</td>
    <td>只是协议升级，无需硬件升级，可充分利用现有手机的蓝牙功能，快速部署智能终端。</td>
    <td>不支持网状网络，不支持多跳。因此要求传输双方都必须在无线信号可直接到达的范围之内。这一点直接限制了传输的距离。BLE 4.1之后，虽然可以自组Mesh网，但硬件须升级。</td>
  </tr>
  <tr>
    <td>SUB 1G</td>
    <td>1GHz以下的ISM无线通讯技术。</td>
    <td>传输距离可达2~100km。</td>
    <td>协议栈依赖芯片厂商提供。更像一种通讯方式，而非通讯解决方案。</td>
  </tr>
  <tr>
    <td>Low Power Wifi</td>
    <td>低功耗的wifi技术，有两个变种：普通型和802.11ah。</td>
    <td>传输速度高。普通型兼容现有wifi设备。802.11ah穿透性好。</td>
    <td>功耗仍然比Zigbee和BLE大一个数量级。</td>
  </tr>
  <tr>
    <td>Thread</td>
    <td>Google在IEEE 802.15.4的基础上构建的6LowPan方案。</td>
    <td>由于MAC层和Zigbee相同，因此具备Zigbee的大多数优点，且协议更友好（设计思想接近IPV6）。</td>
    <td>不兼容现有设备，市场前景有待观察。</td>
  </tr>
  <tr>
    <td>UWB(Ultra Wideband)</td>
    <td>一种无载波通信技术，利用纳秒至微微秒级的非正弦波窄脉冲传输数据。</td>
    <td>UWB能在10米左右的范围内实现数百Mbit/s至数Gbit/s的数据传输速率。</td>
    <td>尚处于试验阶段，没有成功的产品。</td>
  </tr>
  <tr>
    <td>电力载波</td>
    <td>基于电力线传输的有线通讯协议。</td>
    <td>无须布线，传输速度高（500Mbps），距离远（500m）。</td>
    <td>不适合无电力线的场景。</td>
  </tr>
  <tr>
    <td>NB-LTE & NB-CIoT</td>
    <td>都是蜂窝无线通信网向物联网进军的产物。前者由Nokia、Ericsson和Intel提出，而后者由华为提出。</td>
    <td>号称与现有LTE网络兼容，但2015年9月才推出，具体规格不详。</td>
    <td></td>
  </tr>
</table>

----

https://mp.weixin.qq.com/s/J700Ifq5FfJqtgmO2VVn6g

Cat 1大热背后，NB-IoT该何去何从？

# 边缘计算

![](/images/img2/edge_computing.png)

https://mp.weixin.qq.com/s/i5KkUVaEULrlxeNtU_PDtQ

天津大学最新“边缘计算与深度学习的融合”综述论文

https://mp.weixin.qq.com/s/TXZh-bn9VhwGxJRNjzecRw

重新认识“边缘计算”

https://mp.weixin.qq.com/s/6Nr7LF9VxYQ4NlLlk8bCVw

不了解边缘计算，你可能就要被“边缘”了

https://mp.weixin.qq.com/s/3d7CEQ0iQzwnJ_tE7eB9Yg

边缘计算芯片格局分析

https://mp.weixin.qq.com/s/U9vnEzumNgGlYFR-lEPfhw

智能边缘计算：计算模式的再次轮回

https://mp.weixin.qq.com/s/x6MWXZAFgyv2HDaVll9S6A

一文读懂边缘计算和业务场景

https://mp.weixin.qq.com/s/eXRy9kZqDlph96BdWwWwXQ

49页ppt，Edge Computing for Infrastructure

# Raspberry Pi+

https://mp.weixin.qq.com/s/pPTaxiCOTL_Y89TuKGHQmg

微软放弃的游戏被他们复活了：Windows经典“三维弹球”现实版，CAD建模、Arduino编程、数控机床打造，硬核致敬童年

https://mp.weixin.qq.com/s/mgdDjjIJX4y1-DXzazDhPA

不会编程的外国小姐姐，3天、850块，徒手用树莓派DIY了个数码相机

https://mp.weixin.qq.com/s/rM3SkUiQIxsGz9MNIg1skQ

用树莓派DIY波士顿机器狗，帮你省下50万：教程开源，人人皆可上手

https://zhuanlan.zhihu.com/p/301860804

如何把树莓派400打造成小霸王游戏机，Retropie包含上万经典老游戏

# 生物+++

https://mp.weixin.qq.com/s/HEcqyGkGJgtUMVewMBDNLg

小龙虾又泛滥成灾，英国人表示捕食没用，中国吃货表示你们方法不对

https://mp.weixin.qq.com/s/X-_yFro-MdyISeqci6YUGA

昆虫界的金针菇，被吃后从屁股逃生

https://mp.weixin.qq.com/s/nGiveZG6297A9e1KiPYTYA

你吃的葵花籽里，藏着大自然的黄金比例

https://mp.weixin.qq.com/s/4kWsQp9jUUpZas09lccOiA

科学家发现1亿年前的精子，长度惊人还刷新了一项记录

https://www.zhihu.com/question/430020050

既然碘在自然界中并不富裕，那么为什么甲状腺激素一定要用碘来合成？

https://mp.weixin.qq.com/s/mD-RbgvcbDfgJCrGrTMKYA

鹦鹉重骑扫奥陶

https://mp.weixin.qq.com/s/EGCCycVCa17fvlJvSUttQQ

甲虫的硬壳是怎么来的？

https://mp.weixin.qq.com/s/a0BREVS0BcUHS3RhE-Oo4w

生存技能篇：研蛇者说

https://mp.weixin.qq.com/s/jSP_YsKAXHKgmOpLqhQhVQ

蝙蝠竟然会倒立生孩子，这些生娃瞬间看得我肚子疼

https://mp.weixin.qq.com/s/3y8BF4sI2MYPMQ4q9xRGqw

除了新鲜，你对巴氏牛奶了解多少？

https://zhuanlan.zhihu.com/p/333361694

印尼，侏罗纪危机（科莫多巨蜥）
