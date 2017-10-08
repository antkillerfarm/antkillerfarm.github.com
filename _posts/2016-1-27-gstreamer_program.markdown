---
layout: post
title:  GStreamer（二）
category: technology 
---

# GStreamer应用（续）

## TCP远程播放

除了本地播放之外，GStreamer亦支持远程播放。以下仅以TCP远程播放为例。

TCP远程播放采用Client/Server模式。

### step1

1.首先打开播放端软件。（Server端）

`gst-launch-1.0 tcpserversrc host="127.0.0.1" port=3000 ! decodebin ! autoaudiosink`

2.打开多媒体发送端软件。（Clinet端）

`gst-launch-1.0 filesrc location=./1.mp3 ! tcpclientsink host="127.0.0.1" port=3000`

示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gstreamer/tutorials/cs

### step2

来一个更复杂的例子：

`gst-launch-1.0 filesrc location=./1.mp3 ! tee name=tee0 tee0. ! queue ! tcpclientsink port=3000 tee0. ! queue ! decodebin ! autoaudiosink`

这个例子中，一个音频文件被tee分成了2份，一份远程播，一份自己播。

注意事项：

1.Server端的管道状态和一般情况下不同。初始情况下，就要设置为PLAY，否则Client会连接不上。

2.对Client管道状态的改变，如PAUSE等，不会改变Server的管道状态。因此，需要另外建立控制管道控制Server的播放操作。

3.Client的EOS（End of Stream）不会触发Server的EOS，只有Client的断开才会触发Server的EOS。

4.Server的EOS处理，需要先将管道状态设置为NULL，然后再设置为PLAY。否则，会导致新的Client无法连接到Server。

示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gstreamer/tutorials/cs2

### step3

前面的例子中，管道都是一次性创建好的。这里来个动态创建的例子：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gstreamer/tutorials/cs3

## RTP远程播放

TCP远程播放的优点是数据传输较快，但缺点是无法控制接收端的播放操作。为此人们提出了RTP/RTCP协议，两者一般配合使用，前者用于传输媒体流，后者用于传输控制流。

参考文档：

https://gstreamer.freedesktop.org/documentation/rtp.html

https://cgit.freedesktop.org/gstreamer/gst-plugins-good/tree/gst/rtp/README

1.首先打开播放端软件。（Server端）

`gst-launch-1.0 udpsrc port=3000 caps="application/x-rtp, media=(string)application, payload=(int)96, clock-rate=(int)90000, encoding-name=(string)X-GST" ! rtpgstdepay ! decodebin ! autoaudiosink`

2.打开多媒体发送端软件。（Clinet端）

`gst-launch-1.0 filesrc location=./03.flac ! decodebin ! rtpgstpay ! udpsink port=3000`

注意事项：

1.RTP本身没有EOS标志，因此需要通过其他手段告诉接收端——现在已经EOS了。参见：

http://gstreamer-devel.966125.n4.nabble.com/Not-getting-GST-MESSAGE-EOS-from-recording-bus-td4662992.html

示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gstreamer/tutorials/rtp

2.如果Client的歌曲已经切换，但Server管道没有重置的话，会出现播放杂音。同样，这个事件也不能仅通过RTP，告知Server。

3.虽然GStreamer已经提供了很多格式的rtp插件，然而仍然有很多格式不支持rtp。于是就存在转码的问题，但遗憾的是支持rtp的格式，多是一些老的有损压缩格式，对最近日趋流行的无损压缩支持的不够。

4.clock-rate似乎不能设置为90000之外的值，否则无法播放，原因不详。

综上，GStreamer提供的原始的RTP播放，只适合诸如监控之类的媒体格式固定的管道。对于媒体格式不固定的管道，支持的并不好。

## 多设备的网络时钟同步

多个设备协同播放同一个媒体流的时候，设备之间存在着时钟同步的问题。针对这个问题，GStreamer提供了网络时钟同步的功能。

这个功能主要涉及两个对象：GstNetTimeProvider和GstNetClientClock。前者用于提供时钟源，而后者负责获取时钟源的时钟。

具体实现可参考以下文章：

https://fosdem.org/2016/schedule/event/synchronised_gstreamer/attachments/slides/889/export/events/attachments/synchronised_gstreamer/slides/889/synchronised_multidevice_media_playback_with_GStreamer.pdf

文章中的代码需要GStreamer v1.6以上才可编译。此外，compile文件也需要少许修改方可正常使用。这里给出一个autoconf版的demo：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gstreaamer/tutorials/sync_demo

对于更精确的时钟同步，在GStreamer v1.6之后，还提供了GstPtpClock对象。这个对象仅提供了PTP协议的Client功能。

PTP协议相关的规范是IEEE1588:2008。其服务器实现有：

ptpd：http://ptpd.sourceforge.net/

参考：

http://www.tinylab.org/gstreamer-sdk-a-cross-platform-multimedia-framework/

这篇文档提到queue可通过设置缓存大小，来达到延迟播放的效果，但实际上，并没有这个效果，原因不详。

最简单的多设备协同播放，可使用一主多从式的RTP分发管道。需要注意的是，主设备不要使用本地解码管道，而要和从设备一样使用RTP传输播放管道（也就是自己发自己收），否则它和从设备之间会有播放不同步的情况发生。

代码示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gstreamer/multi-room/gmediarender-2013-12-04

这是一个在gmediarender的基础上修改而成的，多音箱协同播放的软件。支持DLNA协议，使用的时候，手机App向Master音箱推送歌曲即可。

## RTP播放状态问题

RTP管道和其他GStreamer管道不同，其PLAYING状态更多表示它可以接收网络发过来的数据，但这个时候是否有数据正在播放，实际上是不得而知的。

解决的办法是：

1.向管道中添加queue组件。queue组件中的overrun、underrun事件可用于指示管道的微观状态。

2.从宏观来看，如果管道处于播放状态，那么underrun事件会不断产生。一旦underrun事件停止产生，那也就表明近期没有数据发过来了，也就是管道处于空闲状态。

一旦发现RTP管道从播放状态进入空闲状态，就要及时重置管道，不然扬声器可能会产生低音量的噪声。这可能和管道中残存的数据有关，原因不详。

简单的将管道设置为NULL，再设为PLAYING的方式，可以消除噪声，但接下去的播放就不正常了。解决办法暂未找到。

## GStreamer对URI的支持

GStreamer的playbin、uridecodebin插件都可以处理URI，但dataurisrc是个例外，它接收的不是如`http://`或`file://`这样的URI，而是RFC 2397格式的URI，如下所示：

{% highlight bash %}
gst-launch-1.0 -v dataurisrc uri="data:image/png;base64,iVBORw0KGgo...." \
! pngdec ! videoconvert ! imagefreeze ! videoconvert ! autovideosink
{% endhighlight %}

如果想做一个urisrc的话，可以使用giosrc插件，或者分不同情况，使用filesrc（file）或souphttpsrc（http）插件。

注意事项：

1.giosrc在不同平台的支持是不一样的。比如在Raspberry Pi上就无法获取http资源，原因不详。

2.giosrc不支持seek功能，而souphttpsrc支持。

3.开源项目Rygel，最近（v0.30.3）放弃使用giosrc。

## GStreamer应用的内存占用情况

| 场景 | 内存占用 |
|:--:|:--:|
| 播放本地音频文件 | ～10MB |
| 播放远程音频文件 | ～25MB |
| gmediarender（非播放状态） | 50MB～65MB |
| gmediarender（播放状态）| 60MB～85MB |

## audioconvert

audioconvert用于转换不同格式的音频数据。这里的格式指的是位宽、大小端、采样率等格式，而不是音频编解码格式。

如果没有使用audioconvert做转换，可能会导致音频文件在某些设备上无法播放。毕竟设备不可能支持所有的位宽、大小端、采样率。

类似的，还有videoconvert、autoconvert插件。

## GstPlayer

GstPlayer是GStreamer官方推出的播放器项目，旨在简化GStreamer API，以便于更多人使用GStreamer。

GstPlayer的最终目标，是替换Totem项目当前的GStreamer实现。但从该项目目前还在Bad Plugins中，可以看出它距离自己的目标尚有一段距离。

该项目的主页：

https://gstreamer.freedesktop.org/projects/gstplayer.html

示例代码：

https://github.com/sdroege/gst-player

https://coaxion.net/blog/2014/08/gstreamer-playback-api/

这是其中一个主创的blog，讲述了项目的缘起和路线图。

## GStreamer的声道处理

GStreamer的声道处理包含3个层次：媒体文件、媒体流、声道。简单来说就是：

1.一个媒体文件包含若干媒体流。比如视频文件就至少包含一个视频流和一个音频流。而某些DVD媒体文件中，针对不同语言，往往有不同的音频流。比如一个汉语的音频流+一个英语的音频流。

2.一个音频流包含若干声道。比如常见的2.1声道、5.1声道等。

### 媒体信息的解析

可以使用gst-discoverer工具获得媒体信息，也可以使用gst_discoverer_XXX系列的函数编程实现相同的功能。

从gst_discoverer_XXX系列的命名规则可以看出:

gst_discoverer_info_XXX: 处理媒体文件

gst_discoverer_stream_info_XXX: 处理媒体流

gst_discoverer_audio_info_XXX: 处理音频流

GstPlayer也提供了GstPlayerMediaInfo、GstPlayerStreamInfo、GstPlayerAudioInfo类用于解析媒体信息。可使用gst_player_get_media_info函数获得相关GstPlayerMediaInfo。

参考：

http://blog.csdn.net/sakulafly/article/details/22216775

http://docs.gstreamer.com/display/GstSDK/Basic+tutorial+9%3A+Media+information+gathering

### 媒体信息的设置

Sender:

`gst-launch filesrc location=/home/file.mp3 ! mad ! audioconvert ! audio/x-raw-int,channels=1,depth=16,width=16, rate=44100 ! rtpL16pay  ! udpsink host=192.168.1.103 port=5000`

Receiver:

`gst-launch udpsrc port=5000 caps="application/x-rtp, media=(string)audio, clock-rate=(int)44100, width=(int)16, height=(int)16, encoding-name=(string)L16, encoding-params=(string)1, channels=(int)1, channel-position=(int)1, payload=(int)96" ! gstrtpjitterbuffer do-lost=true ! rtpL16depay ! audioconvert ! alsasink sync=false`

# GStreamer编程

## 开发环境搭建

`sudo apt-get install libgstreamer1.0-dev`(1.x系列)

`sudo apt-get install libgstreamer0.10-dev`(0.10.x系列)

helloworld程序在

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gstreamer/helloworld

这里的代码尽管是针对1.x系列的，但实际上对于0.10.x系列也同样有效，你需要做的只是将Makefile中的

{% highlight bash %}
CFLAGS = `pkg-config --cflags gstreamer-1.0`
LDFLAGS = `pkg-config --libs gstreamer-1.0`
{% endhighlight %}

改为

{% highlight bash %}
CFLAGS = `pkg-config --cflags gstreamer-0.10`
LDFLAGS = `pkg-config --libs gstreamer-0.10`
{% endhighlight %}

这个例子同时也是如何使用pkg-config来管理同一软件的不同版本的范例。GTK+ 2.x和GTK+ 3.x的共存，也是采用了同样的方法。

## 教程

官方开发指南参见：

http://gstreamer.freedesktop.org/data/doc/gstreamer/head/manual/html/index.html

这是应用开发指南。

http://gstreamer.freedesktop.org/data/doc/gstreamer/head/pwg/html/index.html

这是插件开发指南。

这两本书是最基础的教程，尤其是前者。建议首先通读一遍，然后再进行具体的编程实践。不然，你可能连基本的概念和术语都不清楚。这两个指南都已有中文版，尽管比较老，是针对0.10.x系列的。

官方入门代码教程参见：

http://docs.gstreamer.com/display/GstSDK/Tutorials

其他参考示例：

https://github.com/rubenrua/GstreamerCodeSnippets


