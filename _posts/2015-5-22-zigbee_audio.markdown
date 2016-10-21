---
layout: post
title:  Zigbee音频, 6LowPAN, IEEE 802, Kettle
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

## 无线路由器方面的专业术语

### Wifi设备的三种模式

通常情况下，两个Wifi设备的连接方式如下所示：

Internet`---`设备A`***`设备B （`---`表示有线连接，`***`表示Wifi连接，下同。）

其中设备A的工作模式被称作AP（Access Point），设备B的工作模式被称作STA（Station）。

如果两个Wifi设备是这样连接的话：

Internet`---`设备A`***`设备B`***`设备C

这种情况下，设备B对于设备C来说是AP，但对于设备A来说，却只是一个Client，因此这种模式又被称为AP Client或者AP+STA。这种模式也称作Wifi repeater。其实现原理有以下几种：

1.两个独立的物理网卡，一个AP，一个STA。

2.一个物理网卡上虚拟两个虚拟网卡，一个AP，一个STA。

这个类型又有两个子类型：

1）有的厂家芯片支持模式共存。这时只要驱动支持即可。

参考文献：

http://blog.csdn.net/xiaojsj111/article/details/30482001

2）芯片不支持的，只能采用分时复用的方式，支持模式共存，又称softAP。

# Kettle

ETL，是英文Extract-Transform-Load的缩写，用来描述将数据从来源端经过抽取（extract）、转换（transform）、加载（load）至目的端的过程。这个过程有时又被叫做“数据清洗”。

Kettle是一款国外开源的ETL工具。其官网为：

http://community.pentaho.com/projects/data-integration/

Sqoop是另一个数据转换工具，但是没有IDE。其官网：

http://sqoop.apache.org/

## 文本处理

Kettle的文本处理以“行”为单位。下图是一个实际的使用流程图：

![](/images/article/kettle.png)

1.原始数据来自网络爬虫抓取的数据，它的主体是一个json文件，然而在每一条记录的前后都有一些特殊的字符串，因此从整体来说，并不是一个合法的json文件。

2.采用“字符串替换”插件，去除非法字符串。这里需要注意的是，由于整个过程是数据流形式的，因此，无法在一个步骤中，同时去掉前后两个字符串，而必须分为两个步骤。

3.替换之后，原先有字符串的行，可能变成空行，这是可以使用“过滤记录”插件。

## 文件的增量处理

![](/images/article/kettle_2.png)

kettle没有提供直接的插件用于增量处理，因此需要自己设计增量处理的方法。

增量处理的方法很多，这里仅展示其中一种方法：

1.获得需要处理文件总表A。

2.获得已经处理过的文件列表B。这个列表可以来源于数据库，也可来源于文本文件。这里采用后者。

3.使用“合并记录”插件，从A中过滤掉B。“合并记录”插件的flagfield字段，会给出合并的结果。

4.使用“过滤记录”插件，根据flagfield字段的结果，得到过滤后的列表C。C就是真正需要处理的文件列表了。

## HDFS

HDFS的处理比较简单，将普通例子中的本地文件路径，替换为hdfs://形式的hdfs路径即可。

## Hbase

Hbase插件中的其他概念都比较好理解。唯一比较费解的是Mapping name这个名词，实际上它就是Hbase中表的列族/列簇(column family)。

http://www.cnblogs.com/wxjnew/p/3620792.html

http://blog.csdn.net/scorpio3k/article/details/7872179

