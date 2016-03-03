---
layout: post
title:  GitHub, Google Code, and other
category: technology 
---

# GitHub

2014.12

自从最近google code日益难以访问以来，我就一直在思考着替代的方案。然后在大徐的blog的指引之下，找到了github。

应该说使用git和svn相比，在现在的网络条件下，还是有不少优势的。我托管代码的目的，只是给自己的blog提供一个代码链接的地方而已，基本无意使用这个和他人协作。由于git的版本库是在本地的，即使github由于某种原因倒掉了，我也可以很方便的换一个替代品。

## GitHub导入其他版本库的代码

由于Google Code日益难以访问，因此我灵机一动，何不使用GitHub的导入功能，从Google Code中导出代码，然后再访问之？

以box2d为例，它的svn地址是：

https://box2d.googlecode.com/svn/

GitHub导入版本库功能的地址是：

https://import.github.com/new

## 搭建本地GitHub Blog服务

1.安装ruby

2.修改gem的源

作为生活在水深火热的墙内人民，有必要进行下面一步修改gem的源，方便我们更快的下载所需的组件：

{% highlight bash %}
sudo gem sources --add https://ruby.taobao.org/ --remove https://rubygems.org/
{% endhighlight %}

3.安装ruby-dev

在ubuntu上可以这样：

`sudo apt-get install ruby-dev`

在windows下需要下载一个Dev-kit的安装包。但是Ruby的网站经常访问不了，所以其实还可以这样安装：

`gem install devkit`

4.安装jekyll和rdiscount

`gem install jekyll rdiscount`

5.进入blog的根目录之后,运行

`jekyll serve`

## Markdown

自从我在github建立blog以来，一直都在使用markdown语言。这里仅针对我使用过程中遇到的问题做一个笔记。

### markdown渲染器

Jekyll原生支持maruku，rdiscount，kramdown，redcarpet等markdown渲染器。其中的maruku由于已经不维护，在Jekyll 3.0以后被抛弃。

其实使用哪个markdown渲染器对外观的影响都不大，外观更多的是css的事情，因此够用就好。

开始用的是大徐模板里的rdiscount，最近发现该渲染器不支持代码中的空行。

接着换成kramdown，但是又不支持HTML表格里的rowspan。

然后换成redcarpet，而redcarpet会对下划线进行改写，需要加上no_intra_emphasis扩展以关闭之。修改提交之后，github又告诉我，马上要停用redcarpet，统一使用kramdown。

于是又换回kramdown，反复折腾之后，才发现kramdown可以支持rowspan，但是它的语法要求非常严格，必须符合XHTML才行。

错误写法：`<td rowspan=2>`

正确写法：`<td rowspan="2">`

### 语法高亮

之前一直使用pygments作为语法高亮的着色器。近来，github推荐我使用rouge。经过一番研究才发现，pygments是用python写的，难怪windows环境下的Jekyll老是无法集成pygments。

## git常用命令

1.创建版本库

`git init`

2.撤销add

`git rm --cached FILE`

3.暂存本地修改及恢复

如果`git pull`的时候，本地有修改，就需要暂存，并在`git pull`之后，恢复之。

`git stash`

`git stash pop`

4.查看远程仓库的地址

有的时候时间一长，就会搞忘当初下载代码时的远程仓库的地址。这时可以使用`git remote -v`查看之。

5.check out有submodule的版本库

`git clone --recursive URL`

## 如何git超大版本库

自从两次git完整的linux kernel，都因为网络问题，而中途失败之后，不甘心的我，继续在网上寻找答案。

目前已知的答案是git不支持断点续传，也不支持object原子下载。网上所谓的git fetch能断点续传一说，纯属误会。那只不过是重新下载的命令，即使成功，那也是由于第二次的网络环境变好导致的。

其他的办法包括git bundle，但是这个需要服务器支持才行，而多数站点都没有这个功能。Android代码的网站就采用了这种方法。

其实对付超大版本库，git已经提供了相应的办法：

`git pull --depth N`

N表示深度，具体的含义我也不太清楚。基本上，N=1就是只下载当前的版本，N>1的话，还会下载以前的历史版本。这样我们可以通过不断增加N的方式，将完整的版本库下载到本地。

N增加到多大，才能把整个版本库下载完呢？

使用命令行的话，如果增加N，没有继续下载，就说明版本库已经完全下载到本地了。

如果使用TortoiseGit的话，下载完全之后，再git pull的话，就没有depth选项了。

参考文章：

http://blogs.atlassian.com/2014/05/handle-big-repositories-git/

## git查看远端仓库地址

`git remote -v`

# Google Code

2013.12

一直以来，在sohu写blog都面临一个很大的问题。代码全贴上的话，太占篇幅，而以附件形式提供代码，又不为blog系统所支持。

之前的解决办法是使用网盘，但是网盘时间一长之后，就不再可用，而我显然也不可能经常去刷新网盘，使得其随时可用。再者，网盘也不是专业的代码托管方式。

好在现在有了google code。

# Google code的倒掉

2015.3

昨天收到了Google Code即将关闭的邮件，很是震惊。不过想想也在情理之中，用过GitHub之后，对Google Code也就不那么有爱了。就像用了svn，对cvs无爱；用了git，对svn无爱一样。不过对svn的无爱，只是部分的。如果是公司性质的小团队开发的话，仍然推荐svn，因为权限管理比较方便，概念也比git简单。而如果是不方便上网的环境的话，git的优势就比较明显了。

# Other

## SVN

一直以来都是在Windows下使用TortoiseSVN客户端来操作svn。现在到了linux环境下，由于找了一圈貌似也没有什么很给力的GUI客户端。所以只有对svn的命令行使用做一些研究了。

 好在之前一直使用英文版的TortoiseSVN，因此在使用svn help后，基本操作方面倒是没有遇到什么大的问题。只有commit的时候，除了commit之外，还要update一下，然后当前目录的svn状态才会切换到提交了新版本之后的状态。

## Google Reader倒掉之后

好久没上Google Reader了，这几天有空想上去，才发现Google Reader已经在去年的7月被关闭了。记得当初自己还曾基于Google Reader的API，做了一个Android上的RSS阅读器，当然完成度只有40%左右。没想到Google Reader这么一关，这个作品也就彻底废品了。。。

之前使用Google Reader，主要的目的是浏览cnbeta。但cnbeta的rss是摘要型的，必须随时在线才可查阅正文。而我的碎片时间主要是上下班赶车的路上，这个场景是没有上网条件的。幸好，现在有人做了一个全文的rss：http://cnbeta.catke.com/

但是无论是这个全文rss，还是cnbeta本身的rss，都有条目数量的限制。超过这个数量的老条目，也就无法获得了。怎么样才能像Google Reader那样，无遗漏的保存一段时间以来的所有条目呢？倘若是以前的话，你必须拥有一台时刻在线的PC，不间断的刷新rss。

而现在的话，你可以有别的选择，比如ifttt.com。ifttt是If this then that的缩写。国内的山寨版本有“如果云”。这些网站允许你自己创建一定的规则，来完成一定的动作。具体到当前的目标，就是创建以下规则：一旦rss的内容有更新，就立即将新内容以电子邮件的方式发送到我的邮箱里。

剩下的问题就简单了，找一个好用的邮箱。使用邮箱的手机客户端，将邮件下载到手机上，这样每天的早报就有了:)

## 版本管理工具的前世今生

参考Wiki的相关词条，可将版本工具分为三代：

1.本地版本管理
	
开源：SCCS (1972) RCS (1982)

私有：PVCS (1985) QVCS (1991)

以RCS最为著名，不过由于年代久远，我从来没用过。

2.客户端/服务器版本管理

开源：CVS (1986, 1990 in C) CVSNT (1998) QVCS Enterprise (1998) Subversion (2000)

私有：Software Change Manager (1970s) Panvalet (1970s) Endevor (1980s) DSEE (1984) Synergy (1990) ClearCase (1992) CMVC (1994) Visual SourceSafe (1994) Perforce (1995) StarTeam (1995) Integrity (2001) Surround SCM (2002) AccuRev SCM (2002) SourceAnywhere (2003) Vault (2003) Team Foundation Server (2005) Team Concert (2008)

我用过的包括CVS、SVN、ClearCase和Visual SourceSafe。

这一代的工具，以CVS为开端。ClearCase和Visual SourceSafe与CVS差不多同时期，因此功能上也多有相同，总的来说就是ClearCase功能强，但不好用。SourceSafe好用，但功能差。

其中，ClearCase我在之前的公司有用过，当时为了简单的check in和check out，竟然还需要编写脚本以简化流程。不过功能的确是没的说，分支合并权限都不在话下。

VSS以现在的角度来看，基本就是个垃圾了，它是MS收购的一家公司的产品，目的是填补VS在这方面的空白。但这样弱的工具，即使MS内部的人也基本不用，这也是后来MS发布Team Foundation Server的重要原因。

SVN是这一代的集大成者，使用简单的同时，仍保有相当强度的功能，唯一诟病的就是合并功能太弱。

3.分布式版本管理

开源：GNU arch (2001) Darcs (2002) DCVS (2002) ArX (2003) Monotone (2003) SVK (2003) Codeville (2005) Bazaar (2005) Git (2005) Mercurial (2005) Fossil (2007) Veracity (2010)

私有：TeamWare (1990s?) Code Co-op (1997) BitKeeper (1998) Plastic SCM (2006)

早期比较有名的是BitKeeper。它在2002~2005年是Linux Kernel所使用的版本工具。Linus曾经非常喜欢它。可惜后来由于商业利益的关系，开发者的要挟惹毛了Linus。大神发威开发了Git，从此BitKeeper也就无人问津了。

从这件事情可以看出，Linus其实并不是一个像Richard Stallman那样的开源洁癖狂，现有的东西够用，他就懒得节外生枝了。但也不要因此小看他的水平，大神发起威来，搞死像BitKeeper这样的工具还是绰绰有余的。

目前较为主流的Bazaar、Git和Mercurial都出现在2005年，这并不是偶然的。实际上都是BitKeeper和开源社区之间战争的产物。

后Git时代的工具，如Fossil和Veracity，相比Git来说，对权限、BUG跟踪之类的功能做了进一步的扩展。

