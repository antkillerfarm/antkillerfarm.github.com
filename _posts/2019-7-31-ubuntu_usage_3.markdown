---
layout: post
title:  Ubuntu使用技巧（三）
category: linux 
---

# Ubuntu 18.04使用手记

又是两年过去了，这次是Ubuntu 18.04（2018.4.26发布）。

这次的变化还是有点大，Ubuntu舍弃了自己开发的Unity，转回Gnome，连带着好多软件的界面都出现了一定的调整。这个适应过程，要长于之前的几次升级。

>前些年由于Unity界面乏善可陈，Ubuntu的版本升级被吐槽为换壁纸。这次算是换主题吧。

由于这个改变是2017.4做出的，有了1年的过渡期，因此拿到手的Ubuntu 18.04的成品度还是蛮高的。

输入法比原来好，但有些软件存在兼容问题。

内核：4.15

LibreOffice：6.0

Emacs：25.2

# 桌面主题

用腻了系统自带的桌面主题之后，我打算换个新鲜一些的桌面主题，比如Mac OS X风格的。

1.安装主题修改工具

`sudo apt-get install unity-tweak-tool`

2.安装Mac OS X主题

{% highlight bash %}
sudo add-apt-repository ppa:noobslab/themes
sudo apt-get update
sudo apt-get install mac-ithemes-v3 mac-icons-v3
{% endhighlight %}

3.Cairo Dock

做完上面两步之后，基本的Mac OS X风格已经有了，但Mac最经典的Dock启动器还没有。这里介绍一下Cairo Dock。

安装方法：

`sudo apt-get install cairo-dock`

Cairo Dock不仅具有类似Mac OS X的风格，还有其他的风格可供选择下载。比如我使用的是Chrome风格。

4.其他主题

http://www.ubuntuthemes.org/

这个网站收集了很多桌面主题，但是需要注册，因为有些主题是收费的。

# 发行版乱战

Linux以发行版众多闻名于世。最近发现了以下网站，或可对各个发行版进行一个简单的比较。

http://distrowatch.com/

下面对几个主要的参数，进行一下点评：

## Office

主要是3个流派：

1.StarOffice->OpenOffice.org->LibreOffice。最初由Sun主导，后来改为Google主导。

2.KOffice->Calligra Office。KDE项目的成果。

3.GOffice。Gnome项目的成果，和前两个相比，GOffice的组件比较独立，没有什么协同能力。

# 常用英语缩写

FYI：for your information

IFF：if and only if

eta：estimated time of arrival

w/o：without

N.B.:nota bene 注意,留心

# DRL参考资源+

https://mp.weixin.qq.com/s/ij3bf61Pu7lrX0WijhbDeA

骑驴找马：利用深度强化学习模型定位新物体

https://mp.weixin.qq.com/s/iYxijHlE3sLJgKnwwd8Tgg

使用深度强化学习和贝叶斯优化获得巨额利润

https://mp.weixin.qq.com/s/_QkxCrQlyRM10eZK8aNCKA

强化学习在携程酒店推荐排序中的应用探索

https://mp.weixin.qq.com/s/fWySZWsYEKBRwYaFL3J2Xg

强化学习大规模应用还远吗？Youtube推荐已强势上线

https://mp.weixin.qq.com/s/nHBczPlffhZrJy4G4oJ1Ag

让神经网络懂得黄金法则

https://mp.weixin.qq.com/s/0dUlVC9I8qmv3f2BB0IFew

强化学习介绍及自动驾驶汽车应用

https://mp.weixin.qq.com/s/_Di73PkEWJV1-OLLHfz7yQ

组合在线学习：实时反馈玩转组合优化

https://mp.weixin.qq.com/s/aVWHlwOmNIqOlu3025_RXQ

DeepMind提出多任务强化学习新方法Distral

https://mp.weixin.qq.com/s/gFHbLF-q91sddMAX1CRbEQ

俞扬：“审时度势”的高效强化学习

https://mp.weixin.qq.com/s/lstCIiNs_qA6k7GCYUBv2w

阿尔伯塔大学提出新型多步强化学习方法，结合已有TD算法实现更好性能

https://mp.weixin.qq.com/s/ybyZpaHr-JJg7CCdXGOl5A

Seq2seq强化学习实战

https://mp.weixin.qq.com/s/TUk1PWT9CfPGEW77UKxpjw

三招武林绝学带你玩转“强化学习”

https://mp.weixin.qq.com/s/W9yhj7_frLYWJocoBR1TMQ

避免AI错把黑人识别为大猩猩：伯克利大学提出协同反向强化学习

https://mp.weixin.qq.com/s/p2hlc2PsLgrvxOF8wBZANg

李飞飞高徒范麟熙解析强化学习在游戏和现实中的应用

http://mp.weixin.qq.com/s/EPbKE-TAnAPugJDhXHEyNA

DeepMind开源Psychlab平台——搭建AI和认知心理学的桥梁

https://mp.weixin.qq.com/s/xJ_g3BvbM-WaIyLthHdhEw

DeepMind发布通用强化学习新范式，自主机器人可学会任何任务

https://mp.weixin.qq.com/s/U0K79ELLj4wsOR4sd5G4Vw

Vicarious详解新型图式网络：赋予强化学习泛化能力

https://mp.weixin.qq.com/s/C8hsGkHGtoaS9Vzm6Ub4tw

Berkeley提出“随机搜索”训练线性策略，提高RL的性能

https://mp.weixin.qq.com/s/vYDb1rTdPxO1sIS38VX5xA

DeepMind的AI学会了画画，利用强化学习完全不需人教：SPIRAL

https://mp.weixin.qq.com/s/rpPN2rgru6krRz2fr1RhsQ

模拟世界的模型：谷歌大脑与Jürgen Schmidhuber提出“人工智能梦境”

https://mp.weixin.qq.com/s/AelAD57G4GOh7qm-_rvYsg

伯克利提出DeepMimic：使用强化学习练就18般武艺

https://mp.weixin.qq.com/s/0AM4eASolsPZ7GtPYVBqDQ

伯克利今年大热的DeepMimic开源了~

https://zhuanlan.zhihu.com/p/35567591

强化学习在关系抽取、QA场景的应用

https://mp.weixin.qq.com/s/zWo2iSiJBEBwnFF478xxfQ

DeepMind：探索人类行为中的强化学习机制

https://mp.weixin.qq.com/s/oOslkEklaZSbRb8eDDCRBw

天津大学、东京大学等研究：用深度强化学习检测模型缺陷

https://mp.weixin.qq.com/s/DNT9rMynbN4Th0AVDHeY_w

BAIR提出人机合作新范式：教你如何高效安全地在月球着陆

https://mp.weixin.qq.com/s/KqLCTSYk1C0wYpJw-hpc1g

论强化学习和概率推断的等价性：一种全新概率模型

https://mp.weixin.qq.com/s/AI3i3ZLZ-fynavbeNAMKgA

强化学习应用介绍，41页报告带你快速了解RL的最新应用价值

https://mp.weixin.qq.com/s/Dq4HsRg05bVMjvrsrxfOOQ

从这篇YouTube论文，剖析强化学习在工业级场景推荐系统中的应用

https://mp.weixin.qq.com/s/cyaQt-SO7sgG49kuUyPJbQ

旷视开源的深度强化学习绘画智能体论文解读

https://mp.weixin.qq.com/s/amXiNKJPEkAnu2m5NAERVw

Top-K Oﬀ-Policy Correction

https://mp.weixin.qq.com/s/kNtzy9-6GbsRhlL-mxksew

基于强化学习的人机对话

https://mp.weixin.qq.com/s?__biz=MzU1NTMyOTI4Mw==&mid=2247494226&idx=1&sn=a96ea0ad8961ec3698301cf0c4514843

以YouTube论文学习如何在推荐场景应用强化学习

https://mp.weixin.qq.com/s/uppNSwxNrw4_8NGBQv85xw

今日头条首次改进DQN网络，解决推荐中的在线广告投放问题

https://mp.weixin.qq.com/s/Uin1gOmJEa6cvkiFJm6cHw

你当年没玩好的《愤怒的小鸟》，AI现在也犯难了

https://mp.weixin.qq.com/s/YY1FIMjDIMABdwRC5x9w4g

17种深度强化学习算法用Pytorch实现
