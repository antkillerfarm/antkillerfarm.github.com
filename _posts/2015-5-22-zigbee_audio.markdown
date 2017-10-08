---
layout: post
title:  Zigbee音频, 6LowPAN, IEEE 802, 各种智能家居通信技术比较
category: technology 
---

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

# 开源音频编解码包

## 1.Speex

http://speex.org/

一个老项目，教程多，但官方已经放弃，转而推荐Opus。

## 2.Opus

http://opus-codec.org/

最近的一个新项目，基于IETF RFC 6716（2012.9）。由于比较新，教程很少。

## 3.G.726等一系列算法代码

http://www.itu.int/rec/T-REC-G.191-201003-I/en

ITU官网上的工具包，包含了G.711和G.726算法。

# 6LowPAN

## IPv6和IPv4的区别

![IPv4 vs IPv6](/images/article/ipv4_vs_ipv6.png)

一般来说，网络协议就是一层层的组帧和解帧的过程。因此比较帧头的差异，无疑是个建立感性认识的好方法。上图就是IPv6和IPv4在帧头方面的区别。

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

## CoAP & MQTT

CoAP（Constrained Application Protocol）协议，是IETF针对物联网提出的应用层协议。

参考：

http://blog.csdn.net/xukai871105/article/details/17734163

MQTT（MQ Telemetry Transport）是一个轻量级的machine-to-machine通信协议。适合于低带宽、不可靠连接、CPU内存资源紧张的嵌入式设备，它有可能成为物联网的重要协议。

官网：

http://mqtt.org/

## 无人机通信

无人机通信一般采用微波通信，微波是一种无线电波，它传送的距离一般可达几十公里。频段一般是902－928MHZ，常见有MDSEL805， 一般都选用可靠的跳频数字电台来实现无线遥控。

无线图像回传技术采用COFDM调制方式，频段一般为300MHZ,实现视频高清图像实时回传到地面，比如NV301等。

# IEEE 802

IEEE 802是一系列关于局域网和城域网的标准。

其中，最重要的有：

802.1 802系列协议的网络层管理。

802.3 Ethernet

802.11 Wi-Fi

802.15.1 Bluetooth

802.15.4 Zigbee

802.16 WiMax

http://www.ieee802.org/

可以在这个网址下载相关的标准文件。

## 无线网状网

Wireless mesh network（WMN），也叫wireless ad hoc network。

Ad Hoc源自于拉丁语，意思是“for this”引申为“for this purpose only”，即“为某种目的设置的，特别的”意思，即Ad hoc网络是一种有特殊用途的网络。IEEE802.11标准委员会采用了“Ad hoc网络”一词来描述这种特殊的自组织对等式多跳移动通信网络

它具有以下特点：

### 无中心

Ad hoc网络没有严格的控制中心。所有结点的地位平等，即是一个对等式网络。结点可以随时加入和离开网络。任何结点的故障不会影响整个网络的运行，具有很强的抗毁性。

### 自组织

网络的布设或展开无需依赖于任何预设的网络设施。结点通过分层协议和分布式算法协调各自的行为，结点开机后就可以快速、自动地组成一个独立的网络。

### 多跳路由

当结点要与其覆盖范围之外的结点进行通信时，需要中间结点的多跳转发。与固定网络的多跳不同，Ad hoc网络中的多跳路由是由普通的网络结点完成的，而不是由专用的路由设备（如路由器）完成的。

### 动态拓扑

Ad hoc网络是一个动态的网络。网络结点可以随处移动，也可以随时开机和关机，这些都会使网络的拓扑结构随时发生变化。　这些特点使得Ad hoc网络在体系结构、网络组织、协议设计等方面都与普通的蜂窝移动通信网络和固定通信网络有着显著的区别。

无线网状网可以基于802.11、802.15或802.16。具体到802.11就是Wi-Fi Mash。

## Wi-Fi Mash

和普通Wi-Fi相比，Wi-Fi Mash主要增加了路由协议。而这一块目前尚无标准，有70多个相互竞争的路由协议。其中主要有：

AODV (Ad hoc On-Demand Distance Vector)

B.A.T.M.A.N. (Better Approach To Mobile Adhoc Networking)

HWMP (Hybrid Wireless Mesh Protocol)

OLSR (Optimized Link State Routing protocol)

IEEE为了统一标准，提出了802.11s。目前该标准默认使用HWMP。

## 消息发送的类型

![](/images/article/cast.png)

# 各种智能家居通信技术比较

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