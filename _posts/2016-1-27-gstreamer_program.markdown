---
layout: post
title:  GStreamer编程
category: technology 
---

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

https://autotools.io/index.html

http://francesco-cek.com/cmake-and-gtk-3-the-easy-way/