---
layout: post
title:  GStreamer（二）, Javascript（二）
category: technology 
---

# GStreamer应用（续）

## 多设备的网络时钟同步

多个设备协同播放同一个媒体流的时候，设备之间存在着时钟同步的问题。针对这个问题，GStreamer提供了网络时钟同步的功能。

这个功能主要涉及两个对象：GstNetTimeProvider和GstNetClientClock。前者用于提供时钟源，而后者负责获取时钟源的时钟。

对于更精确的时钟同步，在GStreamer v1.6之后，还提供了GstPtpClock对象。这个对象仅提供了PTP协议的Client功能。

PTP协议相关的规范是IEEE1588:2008。其服务器实现有：

ptpd：http://ptpd.sourceforge.net/

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

gst-launch-1.0 filesrc location=./1.mp3 ! tee name=tee0 tee0. ! queue ! tcpclientsink port=3000 tee0. ! queue ! decodebin ! autoaudiosink

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

场景 | 内存占用
|:--:|:--:|
播放本地音频文件 | ～10MB
播放远程音频文件 | ～25MB
gmediarender（非播放状态） | 50MB～65MB
gmediarender（播放状态）| 60MB～85MB

## audioconvert

audioconvert用于转换不同格式的音频数据。这里的格式指的是位宽、大小端、采样率等格式，而不是音频编解码格式。

如果没有使用audioconvert做转换，可能会导致音频文件在某些设备上无法播放。毕竟设备不可能支持所有的位宽、大小端、采样率。

类似的，还有videoconvert、autoconvert插件。

## 多音箱网络同步播放

http://www.tinylab.org/gstreamer-sdk-a-cross-platform-multimedia-framework/

https://fosdem.org/2016/schedule/event/synchronised_gstreamer/attachments/slides/889/export/events/attachments/synchronised_gstreamer/slides/889/synchronised_multidevice_media_playback_with_GStreamer.pdf

https://github.com/luisbg/synchronised_media_playback

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

# Javascript

## jslint

http://www.jslint.com/

jslint是一个JavaScript语法的检查工具。

## 动画

HTML动画一般有两种实现方式：

1.JS。JS脚本通过动态改变HTML、CSS的内容来实现动画效果。这种方式功能全面，且可在旧版本浏览器中执行。

2.CSS3。CSS3引入了一些动画属性，它由浏览器直接解释执行。这种方式执行效率很高，但需要浏览器本身支持CSS3。并且，有些复杂的动画，可能会超出CSS3的能力范围，这时不可避免的还是会用到JS。

### Animate.css

Animate.css是Daniel Eden使用CSS3的animation制作的动画效果的CSS集合。其官网是：

http://daneden.github.io/animate.css/

教程：

http://www.gbtags.com/technology/css/20120812-animate-css/

## UI控件库

### jQuery UI

这是jQuery官方推出的UI库。官网：

http://jqueryui.com/

### jQuerytools

另一个基于jQuery的UI库。

### YUI

Yahoo User Interface library。这是一个大型的JS工具库，已经停止更新及维护。官网：

http://yuilibrary.com/

### Other

Bootstrap、Foundation、Semantic UI。

