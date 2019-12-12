---
layout: post
title:  Ubuntu使用技巧（三）, Mac OS X
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

# Mac OS X

最近对iOS开发产生了兴趣，于是准备在PC上搭建一个iOS的开发环境。

首先，我搜了一下在Linux上搭建相关环境的方法，搜到了一些结果。但历史比较老，基本都是3、4年前的东西，就算搭好，也不见得有什么用。

于是，目标改为在PC上使用Virtual Box搭建Mac OS X虚拟机。目标版本为Mac OS X 10.10。

1.下载镜像文件。

镜像文件主要有dmg和iso两种。前者必须在Mac OS X中才能执行，而后者和其他OS镜像差别不大。

2.boot

原版镜像由于Apple的硬件检测机制，并不能在PC上运行。这时就需要破解，这一步一般是在boot中做的。

可用的boot工具，早期有empireEFI、HackBoot。较新的有chameleon、Niresh。

# DRL参考资源

https://mp.weixin.qq.com/s/S2eGPTON3XmfN830m4vaaA

腾讯AI Lab：可自适应于不同环境和任务的强化学习方法

https://mp.weixin.qq.com/s/Rvc5fXnbDPMJftp13xF4hg

强化学习应用金融投资组合优化

https://mp.weixin.qq.com/s/dlOFM7LuOF2npDP_EaITvg

效率提高50倍！谷歌提出从图像中学习世界的强化学习新方法（PlaNet）

https://mp.weixin.qq.com/s/Maoy2feVWj5hpn4Ysh_47A

你追踪，我逃跑：一种用于主动视觉跟踪的对抗博弈机制

https://mp.weixin.qq.com/s/4w3wIyy4H0UGeqOGhNcIbA

强化学习落地！京东等发布综述《深度强化学习在搜索，推荐和在线广告中的应用》

https://mp.weixin.qq.com/s/F_GsfAJRFJ_o8PETqUL35g

谷歌大脑AI飞速解锁雅达利，训练不用两小时：预测能力“前所未有”

https://mp.weixin.qq.com/s/6wPtb9Qdhr9FiMk15xrUsQ

强化跨模态匹配和自监督模仿学习

https://mp.weixin.qq.com/s/lU3_ONAIGDUv_AVv2Xn14w

仅需2小时学习，基于模型的强化学习方法可以在Atari上实现人类水平

https://mp.weixin.qq.com/s/w0_g5FlC6vx2MRAhADPq2g

深度强化学习在智能对话上的应用

https://mp.weixin.qq.com/s/4SZ1NN5hUUcO_dSe4Bv0NQ

利用鲁棒控制实现深度强化学习驾驶策略的迁移

https://mp.weixin.qq.com/s/rwqtw5b2Nap5UPU9DWBXqg

强化学习与文本生成

https://mp.weixin.qq.com/s/VPCtsv2Q73qVcNAa4Xufag

从虚拟到现实，北大等提出基于强化学习的端到端主动目标跟踪方法

https://mp.weixin.qq.com/s/6Sj2QIELQvI28Rpp7A39Fg

如何通过结构化智能体完成物理构造任务？

https://mp.weixin.qq.com/s/lR6BSa_pJzcinkSaSWsM2A

伯克利提出强化学习新方法，可让智能体同时学习多个解决方案

https://mp.weixin.qq.com/s/P-iSI80IVmb5s-Q15Re2HQ

All In!我学会了用强化学习打德州扑克

https://zhuanlan.zhihu.com/p/36322095

最前沿：从虚拟到现实，DRL让小狗机器人跑起来了！

https://mp.weixin.qq.com/s/xr-2cNoSYpCftLI3dV6zEw

如何使用深度强化学习帮助自动驾驶汽车通过交叉路口？

https://mp.weixin.qq.com/s/R_pfTXDMaLHmiCaSV2t_YA

英特尔Nervana发布强化学习库Coach：支持多种价值与策略优化算法

https://mp.weixin.qq.com/s/AyW7oOC7yxVtmswaMT1DGQ

腾讯AI Lab获得计算机视觉权威赛事MSCOCO Captions冠军

https://mp.weixin.qq.com/s/4aENmxUMEEPVPnexLKrg7Q

新型强化学习算法ACKTR

https://mp.weixin.qq.com/s/5PzTiPoXPC1gH3xszzT2dQ

邓力等人提出BBQ网络：将深度强化学习用于对话系统

https://mp.weixin.qq.com/s/pM8oykHmtu5O5jYJBZjO_w

伯克利研究人员使用内在激励，教AI学会好奇

https://mp.weixin.qq.com/s/3WI3QgfHXcrCPbvmHWOEkg

强化学习在生成对抗网络文本生成中的作用

https://mp.weixin.qq.com/s/IvR0O6dpz2GJCG7UQb5kUQ

清华大学冯珺：基于强化学习的关系抽取和文本分类

https://zhuanlan.zhihu.com/p/31579144

让我们从零开始做一个机械手臂(强化学习)

https://mp.weixin.qq.com/s/FiR_GRYqJYpJRO-2p44-Cg

伯克利强化学习新研究：机器人只用几分钟随机数据就能学会轨迹跟踪

https://mp.weixin.qq.com/s/_dHjZQ_7_7H34PHhV_lC3w

全新强化学习算法详解，看贝叶斯神经网络如何进行策略搜索

https://mp.weixin.qq.com/s/YCPXkFzYdC1gMfnprZsVXg

如何让机器人多技能？通过最大熵强化学习

https://mp.weixin.qq.com/s/dbtdNsT3nvLt4UKsrNsn_Q

量化深度强化学习算法的泛化能力

https://mp.weixin.qq.com/s/K-z_dX2-NepkEHbr45QlvQ

微软研究院开源项目TextWorld：可用于强化学习训练的文本游戏

https://mp.weixin.qq.com/s/K2DW_ntSWrlySpxgorF9dA

Python强化学习实战，Anaconda公司的高级数据科学家讲解

https://zhuanlan.zhihu.com/p/32089849

概要：NIPS 2017 Deep Learning for Robotics Keynote

https://mp.weixin.qq.com/s/CCQOHRCAolsorm8FEPdjoQ

什么时候强化学习未必好用？

https://mp.weixin.qq.com/s/Ctn1Wr68lph1UK_wjfCY1Q

策略梯度搜索：不使用搜索树的在线规划和专家迭代

https://mp.weixin.qq.com/s/78ir-Z4ch8_aVpjC6aCPGg

DeepMind综述深度强化学习中的快与慢，智能体应该像人一样学习

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

https://mp.weixin.qq.com/s/JtUuFdTK4Q5YwnVj3BFU2w

全参数化分布，提升强化学习中的收益分布拟合能力
