---
layout: post
title:  在线激活流程研究, GStreamer
category: technology 
---

# 在线激活流程研究

在世界范围内，软件的盗版问题都是个令程序员苦恼的问题。相应的，很多反盗版的措施也就应运而生。其中以输入序列号、激活码的产品激活策略应用最为广泛。本文就从流程的角度粗略的描述一下这个过程。之所以文章的题目没有写成“算法研究”，实在是因为我的算法太菜了。

首先当然是老规矩，回顾历史。

## 1.黑暗时代

最早采用软件注册流程的，并不是公认的被盗版大户微软。微软早期的销售策略是向PC厂商按照CPU数量收费。在linux出现之前，能在Intel x86上运行的OS基本只有MS的Dos。所以在最初阶段，厂商虽然对这种被戏称为“微软税”的收费颇有些看法，但却不得不接受。因此，Dos是没有反盗版策略的，因为相关的钱，已经由厂商支付了。这种收费模式和今天的GPS的收费模式类似，美国军方只向生产GPS芯片的公司收费，而全球只有数家公司获得技术可以生产该芯片。而这笔费用对一般消费者来说是透明的。

这种情况一直延续到90年代初，当其他的Dos替代品出现之后，微软的这种做法便有垄断之嫌。在经历了一系列的法律官司之后，其销售策略逐步过度到了今天的状况。但正是这七八年的功夫，奠定了微软的王朝霸业。

所以第一批采用软件注册流程的，是其他的商业公司或者共享软件的作者，其中尤以后者居多。因为这个时代PC还是很昂贵的奢侈品，虽然名为个人电脑，但拥有的人却并不多。很少有大公司为PC写代码。

早期的软件注册流程通常是这样的：

用户将钱交给共享软件作者时，会从他手里获得密码，然后输入密码，即可运行。在我印象中，采用这种方法最典型的是当时的一款Dos游戏——轩辕剑外转：枫之舞。游戏开始前，你需要完成一个拼图游戏，回答程序随机提问的三块拼图的颜色。而答案就在每个正版用户都会获得的一张彩色图片上。

这种方法显然是相当不安全的。如果这张图被人泄露了，那整个机制就会失去作用。事实上，当时就有同学靠强记的方法，完全背下了所有的拼图颜色。用穷举方式破解，运算量也不大。只不过它采用随机提问的方式，避免了人脑的穷举而已。

## 2.封建时代

一元密码，玩到枫之舞的地步，基本上也就黔驴技穷了。于是类似于用户名、密码之类的二元密码出现了。

二元密码从本质上来说可以表示如下：

密码P = F（用户名U）

F表示相关的算法。只有符合F算法的P和U，才能通过程序的验证。如果对U做一些限制和变换，防止破解者的明文穷举攻击，以及F足够复杂的话，这种方法的安全性还是不错的，至少在不知道匹配的P和U的情况下，穷举已经不太可行了。即使是现在，绝大多数的软件注册，仍然采用这种方法。但这种方法也有缺点，首先无法防止一个软件拷贝，在多台机器上使用。其次只要有一对合法的P和U被泄露，这个机制就被破了。

## 3.城堡时代

现在比较流行的在线注册方法，实际上是一种三元密码。典型的就是windows的在线激活方式。在这种方式下，用户获得正版软件的时候，会得到一个序列号。输入序列号之后，程序会生成能标识用户机器的特征码，这个码通常称作“注册码”。程序将序列号和注册码发送到服务器，如果是合法用户的话，会返回一个激活码，从而完成认证过程。这个过程也可以通过电话等手工方式完成。

具体的流程如下图所示：

![alt txt](/images/article/encrypt.jpg)

1)序列号的生成。序列号肯定是要包含产品信息的，通常将这种不变的东西叫做特征值。但是序列号不是一个而是一批，如何生成呢？这就需要随机数的介入了。举个最简单的例子，假设我们使用椭圆方程来创建序列号，那么就可以将特征值定为长轴a和短轴b，使用随机数作为x（当然这里的x可能存在一定的有效定义域），根据方程生成相对应的y。我们就可以将y当作序列号了。只要x符合一定的规则，不是连续的，那么生成的y也就不是连续的。换句话说，不是随便一个号都是合法的序列号。这个步骤通常是用专门的序列号生成工具完成的。

2)注册码的生成。选取能够唯一标识用户的设备硬件信息，如网卡的MAC号、硬盘编号、手机IMEI号等。通过算法F2，生成注册码。为了防止对注册码编码方式的破解，可以让序列号也参与计算，这样即使同一机器上，序列号不同，注册码也不同。

3)激活码的生成与验证。服务器端将激活码和注册码，通过算法F3，生成激活码。由于引入了服务器，我们可以很容易的知道某个序列号是否已经被使用了，从而有效的防止一号多用。程序通过算法F4，从激活码中获得特征值，如果该值与该产品的特征值一致的话。整个验证步骤就结束了。

## 4.帝王时代

道和魔的斗争永无止境。但是随着软件免费，服务收费模式的兴起。越来越多的软件开始放弃使用反盗版措施。所以或许这个帝王时代也就是故事的终点了。

# GStreamer

## 概况

当前GStreamer主要有两个大的版本分支：

1)0.10.x系列。这个版本系列的历史较久，相关资源比较丰富。但目前官方已经不再发展和支持该版本。该系列有中文版的用户手册。

2)1.x系列。2012年以来发布的版本系列，也是官方推荐的版本系列。只有英文的用户手册，但手册的内容与0.10.x相差不大，尽管API已经不再兼容旧版本。以下的描述以1.x系列为准。1.x系列被设计为可以和0.10.x系列在系统中共存，因此在同一台电脑上，同时安装0.10.x系列和1.x系列是完全没有冲突的。

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

官方教程参见：

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

# GStreamer的Python开发教程

## Step 0

教程的起点——helloworld。这是一个最基本的GStreamer播放器的例子，使用GTK作为GUI工具。

代码参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/python/python-gst-player-example.py

这个例子不能直接运行，需要根据具体情况，略作修改，修改的地方如下：

1）self.uri存放用于播放的媒体文件的URI，注意这里是URI，而不是普通的路径，如果要指定本地文件的话，需要使用`file://`。

2)出错的时候，先用`gst-inspect`检查一下，相应的插件是否安装好了。

## Step 1

在这一步中，我们给播放器添加了暂停和进度条控制的功能。

代码参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/gstreamer/step1/my-gst-player.py

## Step 2

在这一步中，我们的修改如下：

1.添加了快进和慢进的功能。

2.使用gst_parse_launch创建pipeline。该pipeline可以播放视频文件。

代码参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/gstreamer/step2/my-gst-player.py

## Step 3

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

