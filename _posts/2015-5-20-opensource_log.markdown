---
layout: post
title:  从版本库看开源项目的发展史
category: technology 
---

* toc
{:toc}

# 前言

自从发现git和github之后，由于git方便的查看版本历史的功能，使我能够对一些著名开源项目的发展史有一定的了解。并以此为契机，一定程度的揭开开源项目的运作之谜。

PS：不要提svn的版本历史，那个在局域网里查看还算方便，对于互联网的查看来说，即使只看最近1000条，也要耗费非常非常多的时间，而且还是每次查看都是这样费劲。

# 从git log看linux的发展史

linux稳定版本的git地址:

git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git

这个版本的提交那叫一个多啊，截止2014年12月25日，已经有482338次提交。这个次数多到什么程度呢？直接让gitk的内存占用超过2.5G，即使这样也才加载到40W次左右，还剩下8W次没有加载呢。。。

所以在万般无奈的情况下，只好使用原始的git log命令，这才统计出刚才的数字。相关命令如下:

`git log --pretty=format:'%h by %an at %cd'`

可能是git时间一长，就太大了的缘故吧，所以还有一个更古老的git地址：

git://git.kernel.org/pub/scm/linux/kernel/git/tglx/history.git

这里面是2002年至2005年期间的代码，估计以当时的网络条件，传这么个165M的庞然大物，也是件浩大的工程了。

（PS：了解BitKeeper和Git的恩恩怨怨的同学，估计看到“2002年至2005年”，应该就能猜出这个git是怎么回事了吧。我好奇的是，在BitKeeper之前，Linus用什么来管理代码。2015.3.20）

这里有一个发现，Linus早先在Transmeta公司干活，这也是他打工时间最长的公司，有兴趣的同学可以自行wiki一下相关的信息。

最后再吐槽一下git，这么大的库，居然不支持断点续传，也不支持object的原子下载。。。相比之下，svn虽然也不能断点续传，但是这次下载一个文件，下次就不用再下载了。考虑到代码文件也不可能太大，这样也就基本够用了。

2017.3

提交数量至662282。

# 从git log看git的发展史

Linux源代码由于历史久远，最初的版本历史已不可考证，至少无法从git log考证。但git本身的历史，则是一笔笔的都记录在git log中。

网上的说法，git是Linus一个周末的产物。但从log来看，项目开始的时间——2005.4.8，那是个周五。所以这个说法不成立。不过看了下代码的长度，也就是2000多行吧，理论上的确是一个周末的工作量。

从初始版本还可以看出，最早的git并不是单个可执行文件，而是一个子命令对应一个可执行文件。而且命令的名字也不是clone、pull之类的东西。

git-pull是在2005.4.19添加的，是个bash脚本。实际上，git所采用的设计思路和通常的教科书里的自顶而下的方法相反。它是自底而上，首先设计基本数据结构和算法，然后再在使用中，根据实际的需求，编写各种脚本，以使之便于使用。这种设计思路在当时看不出有多大的好处。但时至今日，随着各种项目的进展，版本库的规模日益增长，某些项目的提交总数甚至达数十万之巨，而git仍能高效处理。这不得不使人佩服Linus的设计功力。

2005.4.24 添加了网络传输的功能，这样git才算是个完整的分布式版本控制系统。

2005.6.8 添加cvs到git的工具。

2005.7.11 第一个里程碑版本0.99发布。

2005.12.21 v1.0发布。

从上面的历程可以看出，即使参与项目的人都是牛人，git也差不多经过了8个多月，近3000次commit，才达到了基本可用的水平。绝不是什么天才人物一周时间的作品。

# 从git log看svn的发展史

这个标题并没有错。众所周知的，svn查看修改历史是需要远程登录服务器的。而这对于一些历史悠久的版本库来说，简直是是个灾难。因此这里的log实际上是从Apache官方的git服务器上git下来的。

svn的开端实际上和git是有区别的。这个项目从2000.3.1开始。但是最先动手的，并非代码，而是文档。三个作者足足讨论了3个月，才在2000.6.10提交了第一行代码。而git一开始就是代码。

这实际上是有原因的。svn的目的是替代cvs，因此它的主要任务是修补cvs设计中不好的地方。因此它在设计阶段花费的时间较长。

而git在设计之初，有BitKeeper和Monotone作为参考，且从功能角度而言，BitKeeper并无重大问题，因此设计的任务并不是很大。

反倒是开源社区急需一个产品用以摆脱BitKeeper，因此git一上来就是代码，也就不难理解了。

顺便说一句，Mercurial只比git晚了10天诞生。可见那时候开源社区对于分布式版本控制系统的需求之迫切了。

# 从git log看sqlite的发展史

第一版发布于2000.5.29，已经有相当多的代码。

项目的committer只有9人，其中九成以上的代码出自一个人。

这个项目最初使用CVS管理代码，2009.8.12开始该用fossil管理代码。顺便提一句fossil使用sqlite作为对象存储的工具。

# 从git log看emacs的发展史

很遗憾，早期的历史在log中，已经残缺不全了，

按照wiki的说法：

* GNU Emacs最早广泛发布的版本是15.34，出现于1985年。

而现有代码库的第一个可用的版本是从大约1990年开始的，之前的历史版本虽然有，但是根本不可编译使用。

当时这个项目使用RCS管理代码，这也是发展至今的诸多开源软件中很少见的一例。因为同时期大部分的软件，都已经成为了历史。而像emacs这样，至今仍然相当活跃的项目实在是凤毛麟角。

# 从git log看SDL的发展史

SDL尽管使用广泛，但从代码来看基本上是Sam Lantinga的个人作品。

Sam Lantinga早年创建了Loki Software，专门将Windows游戏移植到Linux平台上，SDL正是这个时期的产物。著名的《英雄无敌3》Linux版就是Loki Software制作的。

后来他先后供职于Blizzard Entertainment和Valve Software，是暴雪的主力程序员之一。

# 物理+

过去玻璃窗的制作方法基本都是吹个球或者静置，但这种方法制作的玻璃基本都不平，所以那时的玻璃窗只能小块用木头框隔开。

英国皮尔金顿平板玻璃兄弟有限公司的阿拉斯塔、皮尔金顿于1952年在一次偶然的机会，看到一层油漂浮在水面上，该公司根据上述这一设想，发明了一种生产平板玻璃的革命性的方法-浮法工艺。它是因玻璃液漂浮在熔融的金属表面上获得抛光成型而得名的。

----

顾静薇被安排和一名哲学教授去相亲。相亲对象非常浪漫，带她去看晚霞。

顾静薇：“他带我去看晚霞，我跟他讲，这是瑞利散射。他那个表情你是没有看见哦！”

https://mp.weixin.qq.com/s/RPkEHmI2soIsHvukUgpZDg

导演朱虹璇问气象科学家：为什么黄昏能看见红色的晚霞？

>顾静薇，或作顾静徽，1900～1983，女，物理学家。1920年入上海大同大学，成为胡刚复的学生。1923年考取留美公费生，入美国康奈尔大学文理学院。康奈尔大学本科（1926）+耶鲁大学硕士（1928）+密歇根大学博士（1931）。中国近代史上第一个物理学女性博士。先后执教于南开大学、大同大学、广西大学和北京钢铁学院。她和丈夫施汝为创办了中国科学院物理研究所，后者为所长。学生有吴健雄、郭永怀等。

----

https://mp.weixin.qq.com/s/lB6dfjS5TAxjMndLCXMWgQ

憋住不放的屁最后去哪了？知道真相的我再也不忍屁了！

https://mp.weixin.qq.com/s/BIy4Ly3eIMOrXRbpjFRFYQ

男人最喜欢在家干的这件事，居然是在马桶上......

https://mp.weixin.qq.com/s/mDgnsn1xsI1WN3UhxPGPrQ

通过这些动图，让你秒懂高中物理原理!

https://mp.weixin.qq.com/s/2n-60v-ocfszU9xhbG4Ofg

达·芬奇和宋代画家告诉你，除了诗和远方，世上还有散射现象（瑞利散射、米氏散射）

https://mp.weixin.qq.com/s/foZaIzpyxiRf9_9O7f6EaQ

985大学首次登上Nature封面（超疏水材料）

https://mp.weixin.qq.com/s/hnDjc8H9US4vZM4PZADZYg

如何修建岩浆护城河

https://mp.weixin.qq.com/s/KsGGMCKsQlHYxXONOTthGQ

当你按下“除湿”键时，空调内究竟发生了什么？

https://mp.weixin.qq.com/s/fbJQs87R9c8_j_B5-zTsVg

古代没有冰箱，夏天冰块是怎么来的？

https://mp.weixin.qq.com/s/oweK30KIVInGK6wcFSRrTQ

三伏天，我在伊朗的风塔里惬意乘凉

https://mp.weixin.qq.com/s/Kfiz22KpxHtkQof3C5g81A

卡路里的身世变迁

https://mp.weixin.qq.com/s/b22vJHOrm_mOMa71TidN4w

八大数学预言

https://mp.weixin.qq.com/s/-4yzwG_LS1J_svR1CiOcpQ

一道闪电掉进海里，会电死多少鱼？

https://mp.weixin.qq.com/s/0CHFXs8IAxFIwvy7hOcnnw

不加色素做出彩虹色巧克力，科学家浪漫起来也太硬核了

https://mp.weixin.qq.com/s/GuXzR6Xf46ByomcVLzai3Q

干洗店用的一种东西，居然跟生命起源有关系？

https://mp.weixin.qq.com/s/ZxdeUFnkwfBH18hDrBDVcQ

为什么夏天更容易出轨？

https://mp.weixin.qq.com/s/dIJS9gebnqFh-nnaoosumA

科学家还是灭世者？记那些原子弹背后的人

https://mp.weixin.qq.com/s/cEGUr0kmP9UwlAoxgFuKGQ

想减肥么？要不把体重秤摆在树荫下试试？（光压）
