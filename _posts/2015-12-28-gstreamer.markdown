---
layout: post
title:  GStreamer（一）
category: technology 
---

* toc
{:toc}

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

`sudo apt install gstreamer1.0-plugins-base`

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

`sudo apt install gstreamer-tools gstreamer1.0-tools`

它包括以下工具：

gst-launch：创建GStreamer管道的原型验证工具。它是其中用的最广泛的工具——网上关于GStreamer的问题讨论，多数并不贴出代码，而是给出gst-launch形式的命令。

下面是一个播放mp3文件的示例:

`gst-launch filesrc location=1.mp3 ! mad ! audioconvert ! autoaudiosink`

需要注意的是上面的命令中，`!`两边都要留空格，不然命令会执行错误。

gst-inspect：用于查询GStreamer插件的相关信息，也非常常用。比如，如果某个媒体文件无法播放，首先使用gst-inspect查询一下相关插件是否正确安装。

其他的还有gst-typefind、gst-xmllaunch、gst-feedback。

注意1.x系列的工具名和0.10.x系列的略有不同，例如`gst-launch`在1.x系列中叫做`gst-launch-1.0`。

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

## 处理播放列表（Playlist）

常用的播放列表文件格式包括.m3u/.m3u8、.pls、.xspf等。详细列表参见：

https://en.wikipedia.org/wiki/Playlist

可以在rhythmbox中，生成一个播放列表，然后保存成文件。rhythmbox支持.m3u、.pls、.xspf格式的输出。

但是rhythmbox毕竟只是播放软件，一些高阶的播放列表功能，比如批量生成、修改TAG之类的还是力有未逮。这时就需要专门的Tag Editor了。

这里推荐EasyTag，但它只支持生成.m3u文件。它的安装方法：

`sudo apt install easytag`

GStreamer本身不能处理播放列表文件。官方的插件库中也没有处理播放列表文件的插件。

从实现角度看，处理播放列表主要有两种方式：

1.编写GStreamer插件，在GStreamer层解决问题。这方面的例子参见：

https://github.com/mopidy/mopidy/pull/460

2.在应用层处理播放列表。比如Totem。

我比较认可第2种方法。从程序设计的角度看，播放列表更多是一种应用层的呈现方式，而非GStreamer所擅长的流处理。从设计原则来看，GStreamer作为底层库，应提供“机制”，而非“方法”。据说GStreamer官方也对这个问题争议了很久，但最终还是不打算支持对播放列表的处理。

## Totem Playlist Parser

Totem中处理播放列表的代码，被单独抽离出来，形成了Totem Playlist Parser库。其官网为：

https://developer.gnome.org/totem-pl-parser

安装方法：

`sudo apt install libtotem-plparser-dev`

代码示例参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gstreamer/tutorials/step1

## Totem Playlist Parser的openwrt移植

### 安装依赖软件包

Totem Playlist Parser除了依赖常见的glib2、libxml、libsoup之外，还依赖gmime和quvi。其中后者不是必须的，如果不打算网上下载视频的话，可以不装。（quvi包主要是一堆lua脚本，用于解析类似youtube、foxnews之类的媒体网站的内容。）

完成上面的操作之后，编译阶段基本就没有什么大问题了。然而，上板调试，却出现解析失败的现象。

### 安装MIME解析文件

Totem Playlist Parser最重要的函数是plparse/totem-pl-parser.c: totem_pl_parser_parse_internal。这个函数的主要思路是在special_types和dual_types数组中，根据MIME查找相关的处理函数。其代码片段如下：

```c
static PlaylistTypes special_types[] = {
	PLAYLIST_TYPE ("audio/x-mpegurl", totem_pl_parser_add_m3u, NULL, FALSE),
	PLAYLIST_TYPE ("video/vnd.mpegurl", totem_pl_parser_add_m4u, NULL, FALSE),
	PLAYLIST_TYPE ("audio/x-scpls", totem_pl_parser_add_pls, NULL, FALSE),
	...
};
```

从这里可以看出，找到正确的MIME才是开始解析的关键。这里使用了glib提供的g_content_type_guess函数判断文件的MIME。

判断MIME的方式有两种：

1.根据文件名判断。

这种方式的主要函数是glib/gio/xdgmime.c: xdg_mime_get_mime_types_from_file_name。

2.根据文件的内容（主要是magic number）判断。

这种方式的主要函数是glib/gio/xdgmime.c: xdg_mime_get_mime_type_for_data。

无论何种方式，这里实际上都需要有一个MIME解析文件提供给程序，用以确定文件的MIME类型。

这里以文件名方式为例，讲一下MIME解析文件的基本知识。

1.存储路径

通常在/usr/share/mime下，其他可能的路径，可在代码中查到。

2.格式类型

主要有三种：

1）XML型。这种类型的解析文件功能和可阅读性都很强，但所占空间较大。

2）glob型。分为glob和glob2两种格式。功能一般，可阅读，占用空间一般。

3）cache型。二进制文件，不可阅读，空间最小，效率最高。

我这里采用glob2文件，既方便修改，其占用空间也在可接受的范围内。
