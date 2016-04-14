---
layout: post
title:  Zigbee音频, 6LowPAN, IEEE 802, 图像处理理论
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

# 图像处理理论

## 对比度和亮度

$$
g(i,j)=a\times f(i,j)+b
$$

上式中$$f(i,j)$$和$$g(i,j)$$表示位于第i行，第j列的像素。上述线性变换中，a表示对比度，b表示亮度。

## 邻域

$$\left[ \begin{array}{ccc} A_0&A_1&A_2\\ A_3&A&A_4 \\ A_5&A_6&A_7\end{array} \right]$$

$$A_0$$~$$A_7$$被称作像素A的1度8-邻域(即$$U(A,1)$$)，相应的上下左右的四个像素$$A_1$$、$$A_3$$、$$A_4$$、$$A_6$$被称作像素A的1度4-邻域。下文如无特别指出，邻域均为8-邻域。

定义$$U^+(A,N)=A\bigcup\limits_{i=1}^N U(A,i)$$。

$$U(A,2)$$的定义如下：

如果$$B\in U(A,1)\land C\in U(B,1)\land C\notin U^+(A,1)$$，那么$$C\in U(A,2)$$。

类似的$$U(A,N)$$的定义为：

如果$$B\in U(A,N-1)\land C\in U(B,1)\land C\notin U^+(A,N-1)$$，那么$$C\in U(A,N)$$。

这里的N被称为度数，也就是两点间的距离，即$$L(A,C)=N$$。

## 相关算子

相关（Correlation)算子

$$g=f\bigotimes h$$

的定义为：

$$g(i,j)=\sum_{k,l}f(i+k,j+l)h(k,l)$$

其中，h称为相关核(Kernel)，即滤波器的加权系数矩阵。相关核有个叫做锚点（anchor）的属性，也就是被滤波的那个点在核中的位置。以3*3的h矩阵为例，如果锚点在矩阵中央的话，则$$i-1\le k\le i+1,j-1\le l\le j+1$$。如果锚点在左上角的话，则$$i\le k\le i+2,j\le l\le j+2$$。

此外，h矩阵还有是否归一化的属性。这里将计算矩阵中所有元素之和的操作，记作$$SUM(h)$$.则当$$SUM(h)=1$$时，h为归一化核。$$\frac{h}{SUM(h)}$$，称作核的归一化。

## 卷积算子

卷积（Convolution)算子

$$g=f\ast h$$

的定义为：

$$g(i,j)=\sum_{k,l}f(i-k,j-l)h(k,l)$$

显然

$$f\ast h=f\bigotimes rot180(h)$$

其中，rotN表示将矩阵元素绕中心逆时针旋转N度，显然这里的N只有为90的倍数，才是有意义的。

## 方框滤波（box Filter）

$$h=\alpha
\begin{pmatrix}
     1 & 1 & 1 & \cdots & 1 \\
     1 & 1 & 1 & \cdots & 1 \\
     \cdots & \cdots & \cdots & \cdots & \cdots \\
     1 & 1 & 1 & \cdots & 1    
\end{pmatrix}
,\alpha =
\begin{cases}
\frac{1}{SUM(h)},  & \text{normalize=true} \\
1, & \text{normalize=false}  \\
\end{cases}
$$

当normalize=true时的方框滤波，也被称为均值滤波。

## 高斯滤波（Gauss filter）

高斯平滑滤波器对于抑制服从正态分布的噪声非常有效。

正态分布的概率密度函数为：

$$f(x)=\frac{1}{\sqrt{2\pi}\sigma}e^{-\frac{(x-\mu)^2}{2\sigma^2}}$$

其标准化后的概率密度函数为：

$$f(x)=\frac{1}{\sqrt{2\pi}}e^{-\frac{x^2}{2}}$$

标准二维正态分布的概率密度函数为：

$$f(x,y)=\frac{1}{2\pi}e^{-\frac{x^2+y^2}{2}}=f(x)f(y)$$

这个公式表明标准二维正态分布，可以分解为两个正交方向上的标准一维正态分布。也就是说标准二维正态分布不仅是中心对称，也是轴对称的。

正态分布的性质：

1.两个正态分布密度的乘积、卷积，还是正态分布。

2.正态分布的傅立叶变换、共轭分布，还是正态分布。

3.正态分布和其它具有相同均值、方差的概率分布相比，具有最大熵。

4.二项分布、泊松分布、$$\chi^2$$分布、t分布等在样本增大的情况下，都趋向于正态分布。

