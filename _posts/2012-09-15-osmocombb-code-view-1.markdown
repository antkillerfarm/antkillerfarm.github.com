---
layout: post
title: OsmocomBB 代码分析(1)
category: technology
---
# 前言 #
这篇文档是去年发布在我的[CSDN blog](http://blog.csdn.net/alexu2002/article/details/6783021#),我觉的还是应该保存在我个人博客中为好。

# OsmocomBB简介 #

OsmocomBB是GSM协议栈(Protocols stack)的开源实现，全称是Open source mobile communication Baseband.目的是要实现手机端从物理层(layer1)到layer3的三层实现。但是目前来看，真正的理层(physical layer)并没有真正的开源实现，暂时也没看到实施计划。只有物理层控制。因为真正的物理层是运行在baseband processor的DSP core上,涉及到许多信号处理算法的实现，而且还要牵扯很多硬件RF的东西。

OsmocomBB项目始于2010年，到目前，还没有实时操作系统支持，没有GPRS的实现，以及移动管理(Mobility Management)的实现，所以还需很多志愿者的加入。

# OsmocomBB准备 #

## 硬件 ##

1. OsmocomBB支持的手机列表。
2. PC Desktop。
3. 和手机相配的串口线。

以我个人为例，我从旧货市场上面买了Moto C118，然后从网站上买的T191串口线，当然如果买那种RS232转USB的转换线的话，不要买便宜的，便宜的cable通常不能用于OsmocomBB。

## 软件 ##

1. Linux操作系统(Ubuntu)
2. arm-elf编译器

确保能够编译代码之前，我们需要安装一些软件包，在ubuntu or Debian中运行以下命令:

    $ sudo aptitude install libtool shtool autoconf git-core pkg-config make gcc

# 获取代码 #

用git来获取代码，在一个目录下，执行以下命令：

    $ git clone git://git.osmocom.org/osmocom-bb.git
    $ cd osmocom-bb
    $ git pull --rebase 

# 设置编译器 #

如果要编译 Motorora C118,需要ARM7的elf编译器，下面是下载地址：

[bu-2.15_gcc-3.4.3-c-c++-java_nl-1.12.0_gi-6.1.tar.bz2](http://www.gnuarm.com/bu-2.15_gcc-3.4.3-c-c++-java_nl-1.12.0_gi-6.1.tar.bz2) has no title attribute.

下载后在一个目录解压缩，比如解压缩的路径是 /opt/gnuarm-3.4.3, 那就在ubuntu的home路径下的.bashrc的最后面加上：
 
    export PATH=$PATH:/opt/gnuarm-3.4.3/bin
 
这样就把交叉编译器的路径加到了系统环境变量中了，我们可以在shell中调用编译工具链。

# 编译，运行，测试 #

## 编译 ##

Osmocom-bb的主分支(main branch)不支持对SIM卡的读写相关操作，如果要对真实的SIM卡与网络进行试验，必须转 到另外一个分支上 去，即sylvain/testing， 我们可以运行以下命令转到这个分支上:
    $ cd osmocom-bb
    $ git checkout -b testing remotes/origin/sylvain/testing
下一步就是将 $osmocom-bb/src/target/firmwire/下的Makefile中的 CONFIG_TX_ENABLE宏打开：
 
    # Uncomment this line if you want to enable Tx (Transmit) Support.
    -#CFLAGS +=-DCONFIG_TX_ENABLE
    +CFLAGS +=-DCONFIG_TX_ENABLE
 
然后你就可以运行$osmocom-bb/src/目录下的makefile来编译出可以进行实网测试的binary.

## 运行 ##

现在可以通过命令在测试机上运行layer1，以及短按手机上得电源按钮来将layer1的binary加载到手机的RAM中:
 
    $ cd src/host/osmocon/
    $ ./osmocon -p /dev/ttyUSB0 -m c123xor ../../target/firmware/board/compal_e88/layer1.compalram.bin
 
现在运行Desktop端的mobile程序，负责layer2,layer3的功能实现，开启新的终端，运行命令如下：
 
    $ cd src/host/layer23/src/mobile
    $ ./mobile -i 127.0.0.1
 
还可以用著名的wireshark 1.4来抓取GSM的信令消息，开启新的终端，运行的命令如下：
 
    $ nc -u -l 4729>/dev/null &
    $ sudo wireshark -k -i lo -f 'port 4729'
 
在开启telnet服务进行手机控制之前，先要建立配置文件/etc/osmocom/osmocom.cfg，用touch命令来建立：
 
    $ sudo mkdir /etc/osmocom/
    $ sudo touch /etc/osmocom/osmocom.cfg
 
运行命令：
 
    $ telnet localhost 4247
 
如果telnet连接成功，则有以下输出：
 
    Trying 127.0.0.1...Connected to localhost.
    Escape character is'^]'.
    Welcome to the OpenBSC Control interface
    OsmocomBB>
 
这是mobile程序建立的telnet服务，等待客户端去连接，OsmocomBB目前用这个用户接口来控制手机各种功能。在此输入命令：
 
    OsmocomBB>enable 
    OsmocomBB# sim reader 1
 
运行此命令后，如果观察Wireshark的log输出，我们应该可以看到Location Update 的信令流程。然后还可以通过命令查看SIM卡信息：
 
    OsmocomBB# show subscriber
 
如果要打电话，运行以下命令：
 
    OsmocomBB# call 1 112
 
在运行过程中，wireshark, layer1, mobile三个程序都有相应输出，分别是GSM信令，layer1的代码流程，mobile的layer2,layer3的流程。

# OsmocomBB总体架构 #

## RF信号链 ##

首先从信号的接收模块开始，这块都是硬件实现，看下图：

![alt text](\images\article\RF_signal_chain.png "Title")

空中的GSM得RF信号通过天线（Antenna）接收到，送到RF混频器（Mixer）,混频器将信号降频（down-conversion）到模拟的baseband的I/Q信号,这个过程是一些数学运算，我会在以后的读书笔记中叙述这个过程。模拟I/Q信号接着被送到ABB进行AD转换，转化成数字I/Q信号，I/Q数字信号继续被送到数字基带处理器（Digital Baseband Processor）的串口（baseband serial port）上。上图的Calypso指的是TI推出的GSM基带处理芯片，Mixer也叫RF transciver, 是一个专门的芯片，在OsmocomBB推荐的手机方案中用的芯片是TI TRF6151C，ABB（Analog Baseband Processor）也是单独芯片，TI TWL3025BZGMR。不过现在集成化程度比以前有提高，很多芯片商都把RF transciver和ABB集成到一块芯片上，比如Infineon PMB6272, RDA 6220。

## 手机端的数字基带处理器及相关软件 ##

请先看图：

![alt text](\images\article\Calypso_DBB.png "Title")

在数字基带处理器中，一般有两个CPU core。Calypso方案是个ARM7的core，用来运行协议栈软件，一般是一个小巧的实时操作系统（realtime OS）与协议栈应用的组合。另外一个CPU是 DSP core，Calypso方案使用的是TI的dsp core，带有硬件运算单元，用来处理数字信号，比如FFT变换，滤波，以及信道编解码。两个CPU core通过共享内存（shared memory）来通信。比如上图中，数字基带采样信号通过基带串口（BSP）传入DSP，在dsp中这些信号被处理，被解调（demodulation）,去交织（deinterleaved）,解码等。然后通过sharememory传送给ARM core。在ARM core上，Osmocom-BB的layer1程序处理来自DSP的MAC数据块，然后通过L1CTL模块向串口发送。

## PC端软件结构 ##

结构图如下：

![alt text](\images\article\pc_desktop.png "Title")

在PC端，程序osmocon通过串口接收到L1CTL消息，然后将其通过unix socket发送到layer23程序（比如mobile程序）。

# 参考资料 #

[bb.osmocom.org/](http://bb.osmocom.org/).
