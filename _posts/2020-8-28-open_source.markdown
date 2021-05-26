---
layout: post
title:  开源社区分裂史, MiniGUI
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

2020.5

和LibreOffice同样遭遇的还有MySQL vs MariaDB。Oracle果然是开源社区的公敌。。。

## DivX vs Xvid

DivX本来是个开源项目。但有一天，几个项目管理者建立了一个商业公司，出于商业目的，而将其变成了闭源项目。这样一来，就激怒了为该项目无偿贡献代码的开源社区。作为回应，开源社区发起了Xvid项目。

事情的结局有点童话色彩：开源的Xvid最终战胜了不厚道的DivX。但背后的逻辑并没有那么童话。开源软件由于拥有成本为0，天生就是商业软件，尤其是收费商业软件的克星，这其实是经济规律在起作用。一般来说，如果以100分为满分，免费软件只要拿80分，收费软件就没有什么戏了。而开源软件只要拿70分，闭源软件也同样没戏了。

# MiniGUI

2015.5

MiniGUI虽然是开源项目，但是并没有提供版本库，也就没有版本历史了。

作为一个曾经辉煌的国产开源项目，MiniGUI在00年代曾与LVS、SCIM并称为三大国产开源软件。更是当时嵌入式GUI开发方面不多的几个选择之一，江湖地位也很高。

但是自从Android横空出世之后，MiniGUI的日子就不太好过了。其主线版本停在了v3.0.12（2012年），距离它的配套开发工具产品mStudio的推出不过3年时间。等于是辛辛苦苦将产品的版图扩充完整，却发现产品本身已经没有市场前途了……

出现这种情况的原因，其实与MiniGUI本身没有太大关系。当年的主要竞争对手Qt和GTK在嵌入式领域同样一败涂地。这一方面是由于Android实在是太强太好了，另一方面更重要的是硬件的进步，导致原先的这些资源消耗较小的GUI系统，变得不是那么非用它不可了。而比较功能的话，一个单纯的GUI和一个全功能的框架之间根本就没有可比性，就连最重量级的Qt也一样不行。

MiniGUI的作者魏永明所创建的飞漫公司，我曾经在2009年的时候，去那里面试过。当时的考官是个中年人，也不知道是不是魏老师本人。应该说笔试面试的情况还是很好的。这家公司的笔试题比较基础细致，在那次找工作的笔试中算比较难的，超过了腾讯。但给的薪水却非常低，甚至比我毕业时的第一份工作还低。可见在国内，以开源软件开发为主业的公司，日子过的还是很困难的。

MiniGUI惨淡之后，飞漫公司转战移动APP领域，但从网站上的版本更新状态来看，这次的转型似乎并不成功。

由此延伸开去，Android兴起前后，一大堆公司的命运都被随之改变。

1.博望科技。我所关注的牛人李先静之前所在的公司。当初为了开发彩信相关的功能，找到了牛人李的博客，于是也就持续关注了这个公司好几年。博望科技早先的目标是基于GTK搭建一个智能手机平台。

可惜直到Android出来的时候，完工度也不高。后来及时掉头，研究Android技术，成为最早的一批国产Android手机制造商和解决方案提供商。可惜从根本来说，Android的目的是降低智能手机的门槛。随着会的人越来越多，这类解决方案商就变得可有可无了。

同样的例子还有德信无线。

牛人李由于过度劳累，大病一场，大概3年前离开了该公司，至今仍处于半修养状态。

应该说，Android最开始的思路，实际上就和博望不同。Android盯着的是未来的硬件，或者说是硬件期货，所以一开始硬件的规格就比较高。尤其在最初的时候，不是顶级配置根本跑不了Android。而飞漫、博望的思路是如何利用现有的硬件做出好的产品，或者说是用尽可能少的钱（这通常意味着硬件的规格是向下的，或者至少是维持原状的）做出产品来。

但硬件的规格总是向上发展的，于是最初顶配才能跑的Android，现在随便什么硬件都能跑的了。这个时候，飞漫、博望之类的产品也就无人问津了。毕竟GUI应用，在嵌入式中还算是重量级的，真的低端设备，如传感器之类的，也用不上这个。

我之前的公司，早先还曾经有个单核跑双系统的AP+BP融合方案，目标是造出千元级别的Android手机。可惜也是逆了技术潮流，后来的AP都已经是双核的了，根本不需要你这样的方案来节省内核数。而最终，千元或以下的Android手机被造了出来，但根本就和这个方案无关，完全得益于硬件制造成本的下降。

但单核跑双系统的虚拟化方案本身还是很有技术含量的，国内当时根本没人会。这个项目主要是法国的团队主导的。

2.播思通讯。中移动为了搞OMS成立的公司，我有同事在那里工作，本人也曾经去那里面试过。可惜由于技术实力太差，纵有中移动在背后站台，也还是烂泥扶不上墙。很快就被Google半年一次的更新甩在了身后。当然这也是Google的江湖地位使然。虽然播思也做出了输入法框架，但不是你主导的项目，你的再好，我也不用，很快就让你的劳动成了无用功。总的来说，这些Android改的系统，越深度定制越死的快，反倒是换皮的MIUI之类的，活的挺好的。

3.魅族。魅族在Wince时代，曾有一款深度定制的M8，当初曾寄望于成为国产的iPhone。事实上，如果没有Android的话，这个目标差不多就实现了。并不是说达到iPhone的水平，而是说除了iPhone之外，别人都没有我的好。

可惜Android的出现，使得一般的手机厂商也能造出现代的智能手机，于是魅族的这番努力，其实并没有得到多少的回报。总算魅族并不是手机解决方案商，而是制造商，没办法标新立异，却也不至于被别人甩下来。因此，魅族现在的日子，也还说的过去。

# 并行 & 框架+

https://mp.weixin.qq.com/s/_85oWK2plv2QOX5Qfg_-ZA

大规模机器学习优化，195页ppt与视频

https://mp.weixin.qq.com/s/soruo90Dbtzi6d1kA63Akg

阿里提出智能算力引擎DCAF，节省20%GPU算力

https://zhuanlan.zhihu.com/p/79030485

AllReduce算法的前世今生

https://mp.weixin.qq.com/s/oDak7peTT5ynNYrH7LSWTg

分布式层次GPU参数服务器架构

https://mp.weixin.qq.com/s/4XMVYXnzpYZ4DrIabuTUig

Ring All-reduce: 分布式深度学习的巧妙同步

https://zhuanlan.zhihu.com/p/28226956

浮点峰值那些事儿

https://zhuanlan.zhihu.com/p/285994980

针对深度学习的GPU共享

https://mp.weixin.qq.com/s/Np4w7RC2JFlB7ZGIduu71w

爱奇艺机器学习平台的建设实践

https://mp.weixin.qq.com/s/9k6PDusoDHjmz58HAZxZcw

GPipe: 小批量流水线带来的大模型训练

https://mp.weixin.qq.com/s/DwjvEn04lGzKU8mDu-5q4g

大幅提升训练性能，字节跳动与清华提出新型分布式DNN训练架构

https://mp.weixin.qq.com/s/dJa5zOXgJJQOM5uWog3JZA

Local Parallesim：一种新并行训练方法

https://zhuanlan.zhihu.com/p/335116835

推荐系统Serving架构分析

https://mp.weixin.qq.com/s/DdsJ-ZB_cX9UhbQNK6dCag

分布式深度学习训练网络综述

https://mp.weixin.qq.com/s/qpwBGlTtTLEAhYAUpPyXTQ

CMU：分布式机器学习原理与策略 AAAI2021教程，附221页ppt

https://mp.weixin.qq.com/s/nK-9ck5S6noIETOb8b2dJw

vivo AI计算平台弹性分布式训练的探索和实践

https://mp.weixin.qq.com/s/EqLCyHn9we2zn-lCnunDNA

WeNet更新：支持多机并行训练

https://mp.weixin.qq.com/s/QpTZSW2TZ1clEBt4iwoe4Q

WeNet更新：支持语言模型

https://mp.weixin.qq.com/s/RMDEvy-3-L-Rag1OrZLYhg

深度学习模型的训练时内存次线性优化
