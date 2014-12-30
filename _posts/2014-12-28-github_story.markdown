---
layout: post
title:  GitHub, Google Code, and other
category: technology 
---

#GitHub

自从最近google code日益难以访问以来，我就一直在思考着替代的方案。然后在大徐的blog的指引之下，找到了github。

应该说使用git和svn相比，在现在的网络条件下，还是有不少优势的。我托管代码的目的，只是给自己的blog提供一个代码链接的地方而已，基本无意使用这个和他人协作。由于git的版本库是在本地的，即使github由于某种原因倒掉了，我也可以很方便的换一个替代品。

## 搭建本地GitHub Blog服务

1.安装ruby

2.修改gem的源

作为生活在水深火热的墙内人民，有必要进行下面一步修改gem的源，方便我们更快的下载所需组建：

{% highlight bash %}
sudo gem sources --remove http://rubygems.org/
sudo gem sources -a http://ruby.taobao.org/
{% endhighlight %}

3.安装ruby-dev

在ubuntu上可以这样：

`sudo apt-get install ruby-dev`

在windows下需要下载一个Dev-kit的安装包。但是Ruby的网站经常访问不了，所以其实还可以这样安装：

`gem install dev-kit`

4.安装jekyll和rdiscount

`gem install jekyll rdiscount`

5.进入blog的根目录之后,运行

`jekyll serve`

## 从git log看linux的发展史

linux稳定版本的git地址:

git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git

这个版本的提交那叫一个多啊，截止2014年12月25日，已经有482338次提交。这个次数多到什么程度呢？直接让gitk的内存占用超过2.5G，即使这样也才加载到40W次左右，还剩下8W次没有加载呢。。。

所以在万般无奈的情况下，只好使用原始的git log命令，这才统计出刚才的数字。相关命令如下:

`git log --pretty=format:'%h by %an at %cd'`

可能是git时间一长，就太大了的缘故吧，所以还有一个更古老的git地址：

git://git.kernel.org/pub/scm/linux/kernel/git/tglx/history.git

这里面是2002年至2005年期间的代码，估计以当时的网络条件，传这么个165M的庞然大物，也是件浩大的工程了。

这里有一个发现，Linus早先在Transmeta公司干活，这也是他打工时间最长的公司，有兴趣的同学可以自行wiki一下相关的信息。

最后再吐槽一下git，这么大的库，居然不支持断点续传，也不支持object的原子下载。。。相比之下，svn虽然也不能断点续传，但是这次下载一个文件，下次就不用再下载了。考虑到代码文件也不可能太大，这样也就基本够用了。

## Robert Love

这几天研究Android源代码，发现了一个牛人——Robert Love。最初注意他是因为他的姓氏挺有意思的。没想到过了几十分钟，就在另一个嵌入式操作系统方面的课件中看到了他的名字。他是Preempt Linux的作者，也是Linux Kernel Development一书的作者，同时还是Android的日志系统的作者。

最关键的是，他是1981年出生的人，比我也就大一点儿。而Preempt Linux是他2001年的作品。那个时候我貌似连C语言都没怎么弄利索。。。

至于国内的牛人，有个叫李云的诺西工程师，写了本嵌入式方面的书感觉还不错，不是随便复制粘贴的东西，可以看看。不过一者，李云比我大好几岁，二者他水平虽然比我高，但还没到仰视的地步。所以终究比不了那些老外的牛人啊。。。

#Google Code

一直以来，在sohu写blog都面临一个很大的问题。代码全贴上的话，太占篇幅，而以附件形式提供代码，又不为blog系统所支持。

之前的解决办法是使用网盘，但是网盘时间一长之后，就不再可用，而我显然也不可能经常去刷新网盘，使得其随时可用。再者，网盘也不是专业的代码托管方式。

好在现在有了google code。

#Other

## SVN

一直以来都是在Windows下使用TortoiseSVN客户端来操作svn。现在到了linux环境下，由于找了一圈貌似也没有什么很给力的GUI客户端。所以只有对svn的命令行使用做一些研究了。

 好在之前一直使用英文版的TortoiseSVN，因此在使用svn help后，基本操作方面倒是没有遇到什么大的问题。只有commit的时候，除了commit之外，还要update一下，然后当前目录的svn状态才会切换到提交了新版本之后的状态。

## Google Reader倒掉之后

好久没上Google Reader了，这几天有空想上去，才发现Google Reader已经在去年的7月被关闭了。记得当初自己还曾基于Google Reader的API，做了一个Android上的RSS阅读器，当然完成度只有40%左右。没想到Google Reader这么一关，这个作品也就彻底废品了。。。

之前使用Google Reader，主要的目的是浏览cnbeta。但cnbeta的rss是摘要型的，必须随时在线才可查阅正文。而我的碎片时间主要是上下班赶车的路上，这个场景是没有上网条件的。幸好，现在有人做了一个全文的rss：http://cnbeta.catke.com/

但是无论是这个全文rss，还是cnbeta本身的rss，都有条目数量的限制。超过这个数量的老条目，也就无法获得了。怎么样才能像Google Reader那样，无遗漏的保存一段时间以来的所有条目呢？倘若是以前的话，你必须拥有一台时刻在线的PC，不间断的刷新rss。

而现在的话，你可以有别的选择，比如ifttt.com。ifttt是If this then that的缩写。国内的山寨版本有“如果云”。这些网站允许你自己创建一定的规则，来完成一定的动作。具体到当前的目标，就是创建以下规则：一旦rss的内容有更新，就立即将新内容以电子邮件的方式发送到我的邮箱里。

剩下的问题就简单了，找一个好用的邮箱。使用邮箱的手机客户端，将邮件下载到手机上，这样每天的早报就有了:)

