---
layout: post
title:  GStreamer（二）, OpenCV, Javascript
category: technology 
---

# GStreamer应用（续）

## TCP远程播放

除了本地播放之外，GStreamer亦支持远程播放。以下仅以TCP远程播放为例。

TCP远程播放采用Client/Server模式。

1.首先打开播放端软件。（Server端）

`gst-launch-1.0 tcpserversrc port=3000 ! decodebin ! autoaudiosink`

2.打开多媒体发送端软件。（Clinet端）

`gst-launch-1.0 filesrc location=./1.mp3 ! tcpclientsink port=3000`

示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gstreamer/tutorials/cs

来一个更复杂的例子：

gst-launch-1.0 filesrc location=./1.mp3 ! tee name=tee0 tee0. ! queue ! tcpclientsink port=3000 tee0. ! queue ! decodebin ! autoaudiosink

这个例子中，一个音频文件被tee分成了2份，一份远程播，一份自己播。

## RTP远程播放

TCP远程播放的优点是数据传输较快，但缺点是无法控制接收端的播放操作。为此人们提出了RTP/RTCP协议，两者一般配合使用，前者用于传输媒体流，后者用于传输控制流。

参考文档：

https://gstreamer.freedesktop.org/documentation/rtp.html

https://cgit.freedesktop.org/gstreamer/gst-plugins-good/tree/gst/rtp/README

1.首先打开播放端软件。（Server端）

`gst-launch-1.0 udpsrc port=3000 caps="application/x-rtp, media=(string)application, payload=(int)96, clock-rate=(int)90000, encoding-name=(string)X-GST" ! rtpgstdepay ! decodebin ! autoaudiosink`

2.打开多媒体发送端软件。（Clinet端）

`gst-launch-1.0 filesrc location=./03.flac ! decodebin ! rtpgstpay ! udpsink port=3000`

RTP远程播放的问题在于，RTP本身没有EOS标志，因此需要通过其他手段告诉接收端——现在已经EOS了。参见：

http://gstreamer-devel.966125.n4.nabble.com/Not-getting-GST-MESSAGE-EOS-from-recording-bus-td4662992.html

示例代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/gstreamer/tutorials/rtp

## GStreamer对URI的支持

GStreamer的playbin、uridecodebin插件都可以处理URI，但dataurisrc是个例外，它接收的不是如`http://`或`file://`这样的URI，而是RFC 2397格式的URI，如下所示：

{% highlight bash %}
gst-launch-1.0 -v dataurisrc uri="data:image/png;base64,iVBORw0KGgo...." \
! pngdec ! videoconvert ! imagefreeze ! videoconvert ! autovideosink
{% endhighlight %}

如果想做一个urisrc的话，可以使用giosrc插件，或者分不同情况，使用filesrc（file）或souphttpsrc（http）插件。

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

# OpenCV

## 参考资料

OpenCV是一套跨平台计算机视觉库。其官网为：

http://opencv.org/

代码下载地址：

https://github.com/Itseez/opencv

OpenCV项目目前由itseez团队维护，他们的网站是：

http://itseez.com/

官方教程：

http://docs.opencv.org/2.4/doc/tutorials/tutorials.html

其他教程：

http://wiki.opencv.org.cn/

国人办的OpenCV中文网。

http://blog.csdn.net/morewindows/article/category/1291764

国人写的OpenCV入门指南。

http://blog.csdn.net/column/details/opencv-tutorial.html

另一个国人写的OpenCV入门指南。

http://blog.csdn.net/abcjennifer/

一个浙大妹子的blog，关注计算机视觉、机器学习。

http://blog.csdn.net/mmz_xiaokong/article/details/7916163

机器视觉开源处理库汇总。

http://blog.csdn.net/mmz_xiaokong/article/details/7916189

介绍n款计算机视觉库/人脸识别开源库/软件。

http://deeplearning.net/

一个深度学习方面的资料网站。从该网站提供的招聘信息来看，caffe、Theano、Torch是目前主流的三大框架库。

http://caffe.berkeleyvision.org/

caffe是贾扬清写的一个深度学习框架。这哥们是清华的本硕+UCB的博士。

https://github.com/BVLC/caffe

caffe的代码地址。

http://deeplearning.net/software/theano/

Theano的主页

https://github.com/Theano/Theano

Theano的代码地址。

http://www.scratchapixel.com/

一个学习图像处理的网站。

http://www.52nlp.cn/

一个国内的自然语言处理的网站。

## 使用细节

### saturate_cast

saturate_cast宏会对结果进行转换，以确保它在有效范围之内。

### 硬件加速

OpenCV中的运算，除了软件实现之外，还有若干种硬件加速，包括OpenCL、CUDA和IPPICV。

Intel针对自身的硬件加速，推出了IPP（Integrated Performance Primitives）软件包，但这个包是收费的。从OpenCV 3.0开始，Intel将IPP的一个子集提取出来，免费供OpenCV项目使用。这个子集，俗称“IPPICV”。

# Javascript

## 参考指南 & 教程

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference

http://www.javascriptkit.com/jsref/

上面两个网址都是Javascript的参考指南，便于查找语法规则和标准函数的用法。

http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000

一个中文入门教程。该作者还编写了Python教程。

## Javascript和C的互相调用

Javascript本质上是服务器发出的，由客户端执行的脚本。出于安全原因，本地功能比较弱。所谓Javascript和C的互相调用，基本上都依赖于浏览器的实现。比如，在IE中依赖于ActiveX插件，在Firefox中依赖于JSAPI。

## node.js

为了在服务器后端使用Javascript，Ryan Dahl发起了node.js项目。这个项目可以看作是php等的竞争者。它的官网为：

https://nodejs.org/

安装方法：

`sudo apt-get install nodejs-dev nodejs-legacy npm`

npm是node.js的软件包管理工具。它的官网是：

https://www.npmjs.com/

可用于搜索有用的软件包，避免重复造轮子。

教程：

http://nqdeng.github.io/7-days-nodejs/

这是阿里的技术团队出的一份中文版简易教程。

http://www.runoob.com/nodejs/nodejs-tutorial.html

这是“菜鸟教程”提供的node.js教程。该网站还有一系列的编程语言教程。

https://github.com/tianmaying/node-blog-demo

这是“天码营”提供的node.js实现网站的示例。

https://www.npmjs.com/package/hbs

http://jade-lang.com/

![](/images/article/web.png)

## CEF

Chromium Embedded Framework的官网是：

https://bitbucket.org/chromiumembedded/cef

由于编译极为复杂，所以通常直接下载它的SDK。其网址：

https://cefbuilds.com/

然而，由于图片认证被“墙”的缘故，而无法下载。

## NW.js

NW.js，也就是之前的node-webkit，也是一个Javascript客户端开发SDK。其官网为：

http://nwjs.io/

安装方法：

`sudo npm install -g nw`

代码示例：

https://github.com/zcbenz/nw-sample-apps

替换国内的源：

编辑~/.npmrc，添加

`registry=http://npmreg.mirrors.ustc.edu.cn/`

## jslint

http://www.jslint.com/

jslint是一个JavaScript语法的检查工具。

