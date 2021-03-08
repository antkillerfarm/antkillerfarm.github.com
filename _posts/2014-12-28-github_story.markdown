---
layout: post
title:  GitHub, Google Code, and other, BBR
category: technology 
---

* toc
{:toc}

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

## GitHub使用技巧

https://github.com/trending/python?since=monthly

这个可以看到python的月度趋势，便于分析技术热点。其他语言可以此类推。

----

`svn checkout https://github.com/rain1024/slp2-pdf/trunk/complete-book-pdf`

上述命令可以下载单个文件或文件夹。

要点：将`tree/master`换成`trunk`。

----

在搜索栏输入`stars:>10000`可以查看star最多的项目。

----

在浏览器中输入：

`https://api.github.com/repos/antkillerfarm/antkillerfarm.github.com`

可以查看repo的基本信息，比如repo的大小（单位：KB）。

## 搭建本地GitHub Blog服务

1.安装ruby

如果是Windows的话，还要在安装ruby之前，安装MSYS2，并安装make、gcc等应用。

install Ruby on Windows：

https://rubyinstaller.org/

2.修改gem的源

作为生活在水深火热的墙内人民，有必要进行下面一步修改gem的源，方便我们更快的下载所需的组件：

```bash
sudo gem sources --add https://gems.ruby-china.com/ --remove https://rubygems.org/
```

>**关于 Windows 下证书无法验证问题 (certificate verify failed)**   
>ruby 没有包含 SSL 证书，所以 https 的链接被服务器拒绝。   
>解决方法很简单，首先在这里下载证书 http://curl.haxx.se/ca/cacert.pem, 然后在环境变量里设置SSL_CERT_FILE这个环境变量，并指向cacert.pem文件。

3.安装ruby-dev

在ubuntu上可以这样：

`sudo apt install ruby-dev`

在windows下需要下载一个Dev-kit的安装包。但是Ruby的网站经常访问不了，所以其实还可以这样安装：

`gem install devkit`

4.安装jekyll和rdiscount

`sudo gem install jekyll rdiscount`

5.进入blog的根目录之后,运行

`jekyll serve`

参考：

http://jekyllthemes.org/

jekyll的官方主题站点

https://mp.weixin.qq.com/s/ih0jurQ4OzvLh3lmhGGxTA

Ruby 3编程: 从小白到专家，598页pdf

## GitHub下载速度慢

219.76.4.4  http://github-cloud.s3.amazonaws.com

https://zhuanlan.zhihu.com/p/314071453

提高国内访问github速度的9种方法！

## arxiv下载速度慢

http://xxx.itp.ac.cn

## Git LFS

Git LFS（Large File Storage）是Github开发的一个Git的扩展，用于实现Git对大文件的支持。

Git LFS可以把音乐、图片、视频等指定的任意文件存在Git仓库之外，而在Git仓库中用一个占用空间1KB不到的文本指针来代替文件的存在。

通过把大文件存储在Git仓库之外，可以减小Git仓库本身的体积，使克隆Git仓库的速度加快，也使得Git不会因为仓库中充满大文件而损失性能。

官网：

https://git-lfs.github.com

中文简介：

https://gitee.com/help/articles/4235

安装：

```bash
curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | sudo bash
sudo apt-get install git-lfs
git lfs install
```

没有带宽问题的话，可以直接`git lfs fetch --all`。但是考虑到国内的访问速度，最好还是指定单个文件，逐个下载比较好。

白名单：

`git config lfs.fetchinclude '<file path>'`

黑名单：

`git config lfs.fetchexclude '<file path>'`

然后：

`git lfs fetch --recent`

>gitee由于将LFS当作收费功能，竟然丧心病狂的将黑名单设为`*`，导致fetch不到任何东西。。。

最后：

`git lfs checkout`

使用中遇到问题，可以查询设置：

`git lfs env`

列出repo里所有lfs文件：

`git lfs ls-files`

参考：

https://mp.weixin.qq.com/s/yoSeUTy13LoKyS1UFKaUbA

如何存储Git大文件？

# Markdown

自从我在github建立blog以来，一直都在使用markdown语言。这里仅针对我使用过程中遇到的问题做一个笔记。

https://mp.weixin.qq.com/s/hSbphAIJTjDn_Th7Oab46g

Markdown速查表

## markdown渲染器

Jekyll原生支持maruku，rdiscount，kramdown，redcarpet等markdown渲染器。其中的maruku由于已经不维护，在Jekyll 3.0以后被抛弃。

其实使用哪个markdown渲染器对外观的影响都不大，外观更多的是css的事情，因此够用就好。

开始用的是大徐模板里的rdiscount，最近发现该渲染器不支持代码中的空行。

接着换成kramdown，但是又不支持HTML表格里的rowspan。

然后换成redcarpet，而redcarpet会对下划线进行改写，需要加上no_intra_emphasis扩展以关闭之。修改提交之后，github又告诉我，马上要停用redcarpet，统一使用kramdown。

于是又换回kramdown，反复折腾之后，才发现kramdown可以支持rowspan，但是它的语法要求非常严格，必须符合XHTML才行。

错误写法：`<td rowspan=2>`

正确写法：`<td rowspan="2">`

## 语法高亮

之前一直使用pygments作为语法高亮的着色器。近来，github推荐我使用rouge。经过一番研究才发现，pygments是用python写的，难怪windows环境下的Jekyll老是无法集成pygments。

## StackEdit

StackEdit是一个在线的markdown编辑工具，被CSDN等网站所使用。

官网：

https://stackedit.io/

## Hexo

Hexo也是一个常见的blog框架，确实默认主题比jekyll的好。然而，它必须本地编译，然后上传github，而不能如jekyll那样，由github自动编译。所以，我还是观望一下吧。

参考：

https://mp.weixin.qq.com/s/0Bd1bxoWx6MnGccnHc15ZA

利用GitHub从零开始搭建一个自己的专属博客

## 参考

https://mp.weixin.qq.com/s/CHca0V7LR4NhomtLIPhc4w

如何用Markdown做幻灯？

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

2014.1

好久没上Google Reader了，这几天有空想上去，才发现Google Reader已经在去年的7月被关闭了。记得当初自己还曾基于Google Reader的API，做了一个Android上的RSS阅读器，当然完成度只有40%左右。没想到Google Reader这么一关，这个作品也就彻底废品了。。。

之前使用Google Reader，主要的目的是浏览cnbeta。但cnbeta的rss是摘要型的，必须随时在线才可查阅正文。而我的碎片时间主要是上下班赶车的路上，这个场景是没有上网条件的。幸好，现在有人做了一个全文的rss：http://cnbeta.catke.com/

但是无论是这个全文rss，还是cnbeta本身的rss，都有条目数量的限制。超过这个数量的老条目，也就无法获得了。怎么样才能像Google Reader那样，无遗漏的保存一段时间以来的所有条目呢？倘若是以前的话，你必须拥有一台时刻在线的PC，不间断的刷新rss。

而现在的话，你可以有别的选择，比如ifttt.com。ifttt是If this then that的缩写。国内的山寨版本有“如果云”。这些网站允许你自己创建一定的规则，来完成一定的动作。具体到当前的目标，就是创建以下规则：一旦rss的内容有更新，就立即将新内容以电子邮件的方式发送到我的邮箱里。

剩下的问题就简单了，找一个好用的邮箱。使用邮箱的手机客户端，将邮件下载到手机上，这样每天的早报就有了:)

# BBR

**B**ottleneck **B**andwidth and **R**ound-trip propagation time是Google于2016年10月提出的TCP拥塞控制算法，其相关代码目前已经加入Linux内核中。

经典的拥塞控制算法设计于1980年代，当时将丢包作为拥塞的信号，这是符合当时落后的实际情况的。

但是网络丢包存在两种情况：第一为拥塞丢包，第二为错误丢包。因此丢包通常并不等同于拥塞。

随着带宽的增加，第一类丢包已经大为减少。目前广域网普遍属于高带宽，高延迟的情况。这种情况术语叫做**长肥管道**（**long-fat pipe**，即延迟高、带宽大的链路）。

BBR就是针对长肥管道而设计的新式算法。

参考：

http://netdevconf.org/1.2/slides/oct5/04_Making_Linux_TCP_Fast_netdev_1.2_final.pdf

Making Linux TCP Fast

https://www.zhihu.com/question/53559433

Linux Kernel 4.9 中的BBR算法与之前的TCP拥塞控制相比有什么优势？

https://zhuanlan.zhihu.com/p/144273871

TCP拥塞控制详解

http://blog.csdn.net/dog250/article/details/52895080

Google's BBR拥塞控制算法模型解析

https://zhuanlan.zhihu.com/p/24431669

BBR是个什么鬼？-1 带宽与RTT探测

https://zhuanlan.zhihu.com/p/26321951

BBR是个什么鬼？-2 外皮后的真相

https://mp.weixin.qq.com/s/NWNMfykpJQ-LD9oV1X6zTw

基于bbr拥塞控制的云盘提速实践

https://mp.weixin.qq.com/s/P3zAW3lxj8aowXtxXCOwqA

Google BBR拥塞控制算法背后的数学解释
