---
layout: post
title:  开源社区分裂史, SDL, FFmpeg, 中文编码格式问题
category: technology 
---

* toc
{:toc}

# 开源社区分裂史

有人的地方，就有江湖，开源社区也同样如此。本来一个战壕的弟兄，因为种种原因，而分道扬镳（或者专业一点叫做fork），从而导致社区的分裂，这并不是什么稀罕事。比如最知名的开源软件Linux，在早期的时候，就有所谓的ac分支，也就是Alan Cox发布的分支。只不过Alan Cox没有另起山头，因此这个只算分支，而不算分裂。但本文下面所要讲述的，可就不是这么小打小闹了。

## ffmpeg vs libav

2011年的时候，一群ffmpeg开发者，由于对项目管理者（肯定不是Fabrice Bellard，从log可以看出，这位老兄当时已经不管这个项目很多年了。）不满，而另立山头，创建了libav项目。这从两个项目的图标就可以看出来。话说libav的图标其实是ffmpeg早先的图标，只不过图标的创建者加入了libav项目，因此ffmpeg不得不换了一个类似的图标。

应该说libav项目开局时的思路还是比较好的。但ffmpeg毕竟是个老项目，即使因为惯性，也足以继续吸引开发者开发并使用。这从提交的次数就可以看的很明显，从2011年8月（也就是libav起义时）到2012年底，libav提交6600次，而ffmpeg同期提交了16600次！

当然，ffmpeg项目提交的内容实际上有很多都来自libav。Libav开发者指责FFmpeg项目负责人Michael Niedermayer盗用了他们的成果。据说Michael将Libav的代码合并到FFmpeg，让他成为了FFmpeg最大的贡献者，他递交的代码一度占了新增代码的80%。但正是因为有这样的搬运工的存在，使得ffmpeg最终赢得了这场竞争。

2011年的时候，由于Debian的一个维护者，本身加入了libav项目的缘故，导致Debian/Ubuntu这个linux最大的发行版采用了libav。（典型的以权谋私，哈哈）但最终到了2015年，Debian社区在历时一年的讨论之后，还是放弃了libav项目。

结论：ffmpeg胜。一般来说，后来者需要付出更大的努力，才能夺取先行者的领先地位。而ffmpeg在市场领先的情况下，仍采取虚心接纳的策略，基本杜绝了libav胜出的可能性。

## OpenOffice vs LibreOffice

同样是2011年，LibreOffice从OpenOffice中fork出来。fork的原因是由于Oracle收购Sun。众所周知，Oracle是仅次于MS的开源社区二号公敌。因此当Google挑头发起LibreOffice项目之后，开发者几乎一边倒的投向LibreOffice，基本没花什么功夫就打垮了OpenOffice。

结论：LibreOffice胜。开源软件的生命力和魅力都在于开源，人心向背是开源软件胜负的根本。

![](/images/img4/office.jpg)

----

2020.5

和LibreOffice同样遭遇的还有MySQL vs MariaDB。Oracle果然是开源社区的公敌。。。

## DivX vs Xvid

DivX本来是个开源项目。但有一天，几个项目管理者建立了一个商业公司，出于商业目的，而将其变成了闭源项目。这样一来，就激怒了为该项目无偿贡献代码的开源社区。作为回应，开源社区发起了Xvid项目。

事情的结局有点童话色彩：开源的Xvid最终战胜了不厚道的DivX。但背后的逻辑并没有那么童话。开源软件由于拥有成本为0，天生就是商业软件，尤其是收费商业软件的克星，这其实是经济规律在起作用。一般来说，如果以100分为满分，免费软件只要拿80分，收费软件就没有什么戏了。而开源软件只要拿70分，闭源软件也同样没戏了。

# SDL

目前网上查到的中文教程，多是针对SDL v1.2的。至于SDL v2.0的例子，Github上已经有不少了，可惜多是英文，查找起来还是不太方便。因此这里我也提供一个自己写的SDL v2.0的Hello World代码。

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/HelloSDL

可以用这个代码确认SDL v2.0的环境搭建是否正确。

`sudo apt install libsdl2-dev`

## SFML

这几年出了一个替代品——SFML，是用C++写的。

官网：

https://www.sfml-dev.org/

# FFmpeg

## 官网 & 安装

http://www.ffmpeg.org/

这是FFmpeg的官网。

值得注意的是，在官网的下载页面默认下载的是源代码。对于不需要源代码的使用者来说，需要在左下角的“Get the Package”处，根据所用平台选择合适的安装包。

安装包分为三类：

1.Static。只有一个可以自解压的exe文件。

2.Shared。相当于要用解压工具来解压的压缩文件。

3.Dev。相关链接库的的头文件和链接文件。

一般没有二次开发需要的话，下载Shared包是最佳的选择。

## 工具的使用

Fabrice Bellard是我崇拜的一位高人。他除了发明ffmpeg之外，还是Qemu和TinyCC的作者和圆周率计算记录的获得者（2009）。

2012年，Fabrice Bellard和Frank Spinelli一起创立了软件公司Amarisoft，这家公司专注在电信领域，致力于为4G/5G社区提供高质量的解决方案。

https://www.zhihu.com/question/28388113

Fabrice Bellard是个什么水平的程序员？

---

之前的想法，ffmpeg主要是一套编解码框架，其本身的功能有限，需要进行二次开发方可使用。没想到其实它自带的程序，功能已经相当强大了。

安装包中包含三个命令行程序：

1.ffmpeg。编解码工具。功能十分强大，使用它可以很轻松的改变视频文件格式或者压缩视频文件。不过也因为视频格式及编辑选项实在太多，以至于想给ffmpeg做一个好用且功能齐全的GUI外壳都不是件容易的事情。

2.ffplay。播放工具。

3.ffprobe。多媒体流分析工具。

## 教程

官方教程《FFmpeg Basics》，在CSDN可以下载到。但该书偏重于如何使用ffmpeg的命令行工具以及编解码的基本流程。对于ffmpeg源代码，以及如何使用ffmpeg做二次开发讲的很少。

http://dranger.com/ffmpeg/

这篇文章是使用ffmpeg做二次开发的入门手册，写的不错。

## ffmpeg常见用法

1.转换视频格式

`ffmpeg -i src.avi des.mp4`

2.转换视频的尺寸

`ffmpeg -i src.mp4 -s 540x960 -acodec copy des.mp4`

3.屏幕录像

`ffmpeg -f alsa -ac 2 -i pulse -f x11grab -r 30 -s 1024x768 -i :0.0 -acodec pcm_s16le -vcodec libx264 -preset ultrafast -crf 0 -threads 0 output.mkv`

4.多张图片生成视频

`ffmpeg -r 30 -i out_%4d_xx.png out.mp4`

这里的序号最好写成0000这样的形式，ffmpeg在这里的处理并不鲁棒。

5.合并视频文件

filelist.txt：

```text
file 'input1.mkv'
file 'input2.mkv'
file 'input3.mkv'
```

`ffmpeg -f concat -i filelist.txt -c copy output.mkv`

参考：

http://www.cnblogs.com/dwdxdy/p/3240167.html

FFmpeg常用基本命令

https://mp.weixin.qq.com/s/5S_NgjxoswrGcrQzBOyoYQ

视频数据处理方法！关于开源软件FFmpeg视频抽帧的学习

https://blog.csdn.net/u012587637/article/details/51670975

使用ffmpeg合并视频文件的三种方法

## ffmpeg + cuda

```bash
ffmpeg -codecs | grep nv
ffmpeg -i ./1.VOB -c:v nvenc ./1.mp4
```

经我的实测，硬件加速能快3倍左右，但生成文件的尺寸较大（大5倍），压缩比不是太理想。

https://blog.csdn.net/weixin_41868104/article/details/124420500

Ubuntu20配置ffmpeg进行gpu硬件加速视频编码记录

## Other

https://baijiahao.baidu.com/s?id=1660308952680999991

电影片源命名格式大扫盲，看懂WEB-DL/BDRip/BluRay/REMUX/4K区别

# 中文编码格式问题

常用的中文编码格式，主要包括大陆的GB系列和台湾的BIG5系列。

GB系列按照发布时间的顺序，又包括GB2312、GBK和GB18030三种格式。越晚的编码格式，其包含的字符数越多，但同时又兼容之前的编码格式。

除此之外，能表示中文的还有Unicode系列。比如，Java内部使用的UTF16BE，以及网络上用的比较多的UTF8。

需要指出的是，由于各种编码格式的字节数不尽相同，单独对文章中的某些字段进行转码，常会由于字节对齐的问题，而产生异常的结果。最好是在一种编码下生成整个文档之后，统一转换成另一种格式。

参考：

https://mp.weixin.qq.com/s/q-VmuWrsKSBMhWxjafPsng

Unicode与UTF-8的区别

# 秦汉=

康成文婢：东汉郑玄，博通今古，对经学颇有造诣。家中的奴婢，因耳濡目染多了，也知书达礼，动辄说话便引经文。

郑玄家奴婢皆读书。尝使一婢不称旨，将挞之，方自陈说，玄怒，使人曳著泥中。须臾，复有一婢来，问曰：“胡为乎泥中?”答曰：“薄言往诉，逢彼之怒。”

“胡为乎泥中”：出自《诗·邶风·式微》。

“薄言往诉，逢彼之怒”：出自《诗·邶风·柏舟》。

郑玄家里的奴婢都曾读书。他曾经指使一婢女不称意旨，将要鞭打她，那婢女正在自我辩解，郑玄生了气，让人把她拉到泥水中。过了一会儿，又有一婢女过来，问道：“为什么在泥中?”她回答说：“我急忙地申说，谁知反碰上他发怒。”

---

过去朝廷的官员从汉代开始每工作五天就要休息一天，称为“休沐”。就跟咱们现在公务员和国企员工休星期天一样。五天一休这个制度一直延续到隋代。而到了唐代则变成了十日一休，也就是每旬休一次，上旬，中旬，下旬都是逢十必休。一直延续。

---

矿工向来是敢战之人，西汉苏令率二百二十八名矿工剑圣纵横几十郡国，杀死太守级别地方长官，比明代七十二倭寇剑圣不知道高到哪里去了。。。

汉成帝永始三年十二月，山阳铁官徒苏令等二百二十八人攻杀长吏，盗库兵，自称将军，经历郡国十九，杀东郡太守、汝南都尉。。。
