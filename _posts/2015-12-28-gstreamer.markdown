---
layout: post
title:  GStreamer
category: technology 
---

## 概况

当前GStreamer主要有两个大的版本分支：

1)0.10.x系列。这个版本系列的历史较久，相关资源比较丰富。但目前官方已经不再发展和支持该版本。该系列有中文版的用户手册。

2)1.x系列。2012年以来发布的版本系列，也是官方推荐的版本系列。只有英文的用户手册，但手册的内容与0.10.x相差不大，尽管API已经不再兼容旧版本。以下的描述以1.x系列为准。1.x系列被设计为可以和0.10.x系列在系统中共存，因此在同一台电脑上，同时安装0.10.x系列和1.x系列是完全没有冲突的。

## GStreamer插件

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

## 万能插件

GStreamer除了那些完成具体功能的插件以外，还有一些抽象的高级插件，如playbin插件。该插件使用了GStreamer的自动加载（Auto plugging）机制，可以自动根据媒体类型，选择不同的管道播放，相当于是个万能播放插件。对于GStreamer应用开发人员来说，是个相当好用的东西。

playbin插件负责媒体播放的全过程，还有其他一些只负责某个步骤的全能插件：

decodebin：解码插件。

autoaudiosink：音频播放插件

autovideosink：视频播放插件

## 播放视频

播放视频也可以使用playbin插件。这里主要存在以下几个问题：

1）不支持0.10.x系列。由于视频解码主要由ffmpeg来实现，而在Ubuntu14.04以后，官方已经移除了gstreamer0.10-ffmpeg插件，因此很多视频流已经无法处理。

2）xvimagesink错误问题。playbin插件默认使用autovideosink作为视频播放插件，而autovideosink优先使用xvimagesink插件。这个插件的优点是使用了硬件加速的功能，缺点是需要显卡驱动的支持。因此，无论在真实机器还是虚拟机上，都有显卡驱动不匹配，从而导致错误的问题。

这时可以换个思路，自己构建播放视频的管道，其核心是使用ximagesink替代xvimagesink。ximagesink是一个兼容性较好的videosink，缺点是速度没有xvimagesink快。

以下是一个播放视频文件的示例：

`gst-launch-1.0 filesrc location=1.avi ! decodebin name=dmux dmux. ! queue ! audioconvert ! autoaudiosink dmux. ! queue ! videoconvert ! ximagesink`

示例中的`dmux`是个用于定位的标记，两个`dmux.`实际上都指向同一个位置，这样就可以在一个线性的字符串中，表示若干个流水管道。这种表示法多用于多媒体流的分路和合路处理。标记的名称是无所谓的，将`dmux`改为其他字符串，并不影响管道的实际含义。

## 核心插件

核心插件又称gst-plugins-core，目前已经集成到gstreamer代码中，没有独立的库。

和上面提到的四类插件不同。它不提供具体的多媒体编解码功能，而是配合框架，搭建完整的多媒体流水线。核心插件相关的资料参见：

http://gstreamer.freedesktop.org/data/doc/gstreamer/head/gstreamer-plugins/html/

这里仅对其中一部分插件的功能描述如下：

fakesink：一个数据只进不出的“黑洞”。例如，一个视频文件一般包括视频流和音频流。如果设备只能播放音频（例如音箱），那么视频流对于设备来说，就是没有意义的东西。这时可以用fakesink插件将之吃掉。否则，由于GStreamer会在视频流和音频流之间进行同步，如果视频流没有被消耗，音频流也无法向前进。

tee：1路分成N路的分路器。与之相对应的是funnel：N路合成1路的合路器。


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

## 多设备的网络时钟同步

多个设备协同播放同一个媒体流的时候，设备之间存在着时钟同步的问题。针对这个问题，GStreamer提供了网络时钟同步的功能。

这个功能主要涉及两个对象：GstNetTimeProvider和GstNetClientClock。前者用于提供时钟源，而后者负责获取时钟源的时钟。

对于更精确的时钟同步，在GStreamer v1.6之后，还提供了GstPtpClock对象。这个对象仅提供了PTP协议的Client功能。

PTP协议相关的规范是IEEE1588:2008。其服务器实现有：

ptpd：http://ptpd.sourceforge.net/

## 教程

官方开发指南参见：

http://gstreamer.freedesktop.org/data/doc/gstreamer/head/manual/html/index.html

这是应用开发指南。

http://gstreamer.freedesktop.org/data/doc/gstreamer/head/pwg/html/index.html

这是插件开发指南。

这两本书是最基础的教程，尤其是前者。建议首先通读一遍，然后再进行具体的编程实践。不然，你可能连基本的概念和术语都不清楚。这两个指南都已有中文版，尽管比较老，是针对0.10.x系列的。

官方入门代码教程参见：

http://docs.gstreamer.com/display/GstSDK/Tutorials

这里还有一个更全的代码示例：

https://github.com/rubenrua/GstreamerCodeSnippets

以下是教程的一些细节的学习心得。

### basic-tutorial-1.c

{% highlight c %}
/* Build the pipeline */
pipeline = gst_parse_launch ("playbin2 uri=http://docs.gstreamer.com/media/sintel_trailer-480p.webm", NULL);
{% endhighlight %}

从这个教程可以看出，我们可以直接使用gst_parse_launch创建pipeline。

### basic-tutorial-7.c

{% highlight c %}
tee_src_pad_template = gst_element_class_get_pad_template (GST_ELEMENT_GET_CLASS (tee), "src%d");
{% endhighlight %}

这是代码的其中一段，这里只谈谈`src%d`是怎么来的。使用gst-inspect工具查询tee插件的信息，得到如下内容：

{% highlight bash %}
Pad Templates:
  SRC template: 'src%d'
    Availability: On request
      Has request_new_pad() function: gst_tee_request_new_pad
    Capabilities:
      ANY

  SINK template: 'sink'
    Availability: Always
    Capabilities:
      ANY
{% endhighlight %}

从中可知，tee插件SRC Pad的模板名就是`src%d`。

## GStreamer的Python开发教程

### Step 0

教程的起点——helloworld。这是一个最基本的GStreamer播放器的例子，使用GTK作为GUI工具。

代码参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/python-gst-player-example.py

这个例子不能直接运行，需要根据具体情况，略作修改，修改的地方如下：

1）self.uri存放用于播放的媒体文件的URI，注意这里是URI，而不是普通的路径，如果要指定本地文件的话，需要使用`file://`。

2)出错的时候，先用`gst-inspect`检查一下，相应的插件是否安装好了。

### Step 1

在这一步中，我们给播放器添加了暂停和进度条控制的功能。

代码参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/gstreamer/step1/my-gst-player.py

### Step 2

在这一步中，我们的修改如下：

1.添加了快进和慢进的功能。

2.使用gst_parse_launch创建pipeline。该pipeline可以播放视频文件。

代码参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/gstreamer/step2/my-gst-player.py

### Step 3

在这一步中，我们使用一般的GStreamer函数构建和Step 2相同的pipeline。

代码参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/gstreamer/step3/my-gst-player.py

这里需要注意以下几点：

1.随机Pad只能用pad-add消息回调的方式添加。

2.以下代码片段在这里都可用，尽管不完全等效，请注意用法和差别：

{% highlight python %}
new_pad_type = new_pad.get_current_caps().get_structure(0).get_name()
new_pad_type = new_pad.query_caps(None).to_string()
{% endhighlight %}

从这里也可以看出，gst_parse_launch会自动处理媒体流的格式匹配问题，而使用普通函数的时候，必须自己编程处理格式匹配的问题。