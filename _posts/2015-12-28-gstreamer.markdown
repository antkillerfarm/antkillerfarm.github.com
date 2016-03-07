---
layout: post
title:  GStreamer
category: technology 
---

# 概况

当前GStreamer主要有两个大的版本分支：

1)0.10.x系列。这个版本系列的历史较久，相关资源比较丰富。但目前官方已经不再发展和支持该版本。该系列有中文版的用户手册。

2)1.x系列。2012年以来发布的版本系列，也是官方推荐的版本系列。只有英文的用户手册，但手册的内容与0.10.x相差不大，尽管API已经不再兼容旧版本。以下的描述以1.x系列为准。1.x系列被设计为可以和0.10.x系列在系统中共存，因此在同一台电脑上，同时安装0.10.x系列和1.x系列是完全没有冲突的。

# GStreamer插件

## 插件分类

GStreamer本质上只是一个多媒体应用框架，具体的多媒体播放功能由插件来完成。

http://gstreamer.freedesktop.org/documentation/plugins.html

这个网页就是gstreamer的插件列表。表中列出的插件，分属4个不同的插件集：

* gst-plugins-base。这类插件格式规范，维护的也很好。

* gst-plugins-good。这类插件有高质量的代码（但格式未必规范），而且许可证也符合要求（LGPL或与LGPL兼容的许可证）。

* gst-plugins-ugly。这类插件有高质量的代码，但许可证方面有问题。

* gst-plugins-bad。这类插件尚不成熟，需要更多文档、测试和应用。

插件安装方法，以gst-plugins-base为例。

`sudo apt-get install gstreamer1.0-plugins-base`

除了上面列出的插件之外，目前的做法，更倾向于使用ffmpeg作为后端编解码库，尤其是编解码更复杂的视频文件。因此在0.10.x时代，提供了gstreamer0.10-ffmpeg插件，而1.x时代，则有gstreamer1.0-libav提供对avcodec、avformat等ffmpeg库的支持。

## 核心插件

核心插件又称gst-plugins-core，目前已经集成到gstreamer代码中，没有独立的库。

和上面提到的四类插件不同。它不提供具体的多媒体编解码功能，而是配合框架，搭建完整的多媒体流水线。核心插件无须选择（也不能选择），默认已经集成到gstreamer的插件库中，它的相关资料参见：

http://gstreamer.freedesktop.org/data/doc/gstreamer/head/gstreamer-plugins/html/

这里仅对其中一部分插件的功能描述如下：

fakesink：一个数据只进不出的“黑洞”。例如，一个视频文件一般包括视频流和音频流。如果设备只能播放音频（例如音箱），那么视频流对于设备来说，就是没有意义的东西。这时可以用fakesink插件将之吃掉。否则，由于GStreamer会在视频流和音频流之间进行同步，如果视频流没有被消耗，音频流也无法向前进。

tee：1路分成N路的分路器。与之相对应的是funnel：N路合成1路的合路器。

## 万能插件

GStreamer除了那些完成具体功能的插件以外，还有一些抽象的高级插件，如playbin插件。该插件使用了GStreamer的自动加载（Auto plugging）机制，可以自动根据媒体类型，选择不同的管道播放，相当于是个万能播放插件。对于GStreamer应用开发人员来说，是个相当好用的东西。

playbin插件负责媒体播放的全过程，还有其他一些只负责某个步骤的全能插件：

decodebin：解码插件。

autoaudiosink：音频播放插件

autovideosink：视频播放插件

# GStreamer应用

## 相关工具软件

GStreamer提供了一个工具软件集——gstreamer-tools。其安装方法如下：

`sudo apt-get install gstreamer-tools gstreamer1.0-tools`

具体的工具列表参见

http://blog.csdn.net/jackyy313/article/details/6275673

注意1.x系列的工具名和0.10.x系列的略有不同，例如`gst-launch`在1.x系列中叫做`gst-launch-1.0`。

这里对其中的一些工具，稍作阐述。

gst-launch：这个工具可以用于创建并运行GStreamer管道。下面是一个播放mp3文件的示例:

`gst-launch filesrc location=1.mp3 ! mad ! audioconvert ! autoaudiosink`

需要注意的是上面的命令中，`!`两边都要留空格，不然命令会执行错误。

## 播放视频

播放视频也可以使用playbin插件。这里主要存在以下几个问题：

1）不支持0.10.x系列。由于视频解码主要由ffmpeg来实现，而在Ubuntu14.04以后，官方已经移除了gstreamer0.10-ffmpeg插件，因此很多视频流已经无法处理。

2）xvimagesink错误问题。playbin插件默认使用autovideosink作为视频播放插件，而autovideosink优先使用xvimagesink插件。这个插件的优点是使用了硬件加速的功能，缺点是需要显卡驱动的支持。因此，无论在真实机器还是虚拟机上，都有显卡驱动不匹配，从而导致错误的问题。

这时可以换个思路，自己构建播放视频的管道，其核心是使用ximagesink替代xvimagesink。ximagesink是一个兼容性较好的videosink，缺点是速度没有xvimagesink快。

以下是一个播放视频文件的示例：

`gst-launch-1.0 filesrc location=1.avi ! decodebin name=dmux dmux. ! queue ! audioconvert ! autoaudiosink dmux. ! queue ! videoconvert ! ximagesink`

示例中的`dmux`是个用于定位的标记，两个`dmux.`实际上都指向同一个位置，这样就可以在一个线性的字符串中，表示若干个流水管道。这种表示法多用于多媒体流的分路和合路处理。标记的名称是无所谓的，将`dmux`改为其他字符串，并不影响管道的实际含义。

## 插件裁剪

这里以播放mp3文件为例，说明插件裁剪的相关原则。插件裁剪问题在嵌入式设备上遇到的比较多。受限于有限的存储空间，我们一般不可能将所有的插件都集成进设备中，而只能根据产品需求，将必要的插件集成进去。这时就遇到一个问题：我们该如何选择被集成的插件集合呢？

1.上层的应用程序一般都是通过playbin插件播放音频文件。因此playbin必选。

2.解码器根据音频格式的不同而不同。例如mp3文件可选用mad作为解码插件。

3.播放也需要相应的插件，比如Linux上最常用的alsasink。

4.由于playbin采用了Auto plugging机制，因此类似autodetect、audioconvert之类的auto插件也是必选，否则会导致playbin的工作异常。

此外，当播放某种文件格式失败的时候，可以尝试使用`gst-launch`命令播放，这时的log会比平时多一些，有助于确认问题所在。

## 多设备的网络时钟同步

多个设备协同播放同一个媒体流的时候，设备之间存在着时钟同步的问题。针对这个问题，GStreamer提供了网络时钟同步的功能。

这个功能主要涉及两个对象：GstNetTimeProvider和GstNetClientClock。前者用于提供时钟源，而后者负责获取时钟源的时钟。

对于更精确的时钟同步，在GStreamer v1.6之后，还提供了GstPtpClock对象。这个对象仅提供了PTP协议的Client功能。

PTP协议相关的规范是IEEE1588:2008。其服务器实现有：

ptpd：http://ptpd.sourceforge.net/

## 播放ape文件

1.ape文件的播放主要依赖于ffmpeg，因此需要添加gst1-libav库，作为ffmpeg和Gstreamer之间的桥梁。

Openwrt默认的gst1-libav库，并没有开启ape格式的支持，需要手动修改相关文件，具体方法可参考已有的ac3格式的实现。

2.运行过程中会出现如下错误：

`No decoder available for type 'application/x-apetag'`

这个错误的原因是：typefind的时候，`application/x-ape`的优先级没有`application/x-apetag`的高。而前者才是ffmpeg库播放ape文件时的MIME。

修改办法：在gst-plugins-base/gst/typefind/gsttypefindfunctions.c中，将`application/x-ape`和`application/x-apetag`交换一下优先级。

这里需要修改两处地方：

1）gst_type_find_suggest函数的参数。

2）TYPE_FIND_REGISTER宏。

## 播放wav文件

wav文件虽然是最简单的一类音频文件，但仍然不可以直接播放，能够直接被ALSA识别的是PCM流。播放wav的插件是wavparse。

## 处理播放列表

参考文献：

https://github.com/mopidy/mopidy/pull/460

https://developer.gnome.org/totem-pl-parser/stable/TotemPlParser.html

{% highlight python %}
register_typefind('audio/x-mpegurl', detect_m3u_header, [b'm3u', b'm3u8'])
register_typefind('audio/x-scpls', detect_pls_header, [b'pls'])
register_typefind('application/xspf+xml', detect_xspf_header, [b'xspf'])
{% endhighlight %}

UpnpDownloadXmlDoc

UpnpDownloadUrlItem

