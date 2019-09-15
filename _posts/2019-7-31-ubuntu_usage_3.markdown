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

# 模仿学习

https://zhuanlan.zhihu.com/p/27935902

机器人学习Robot Learning之模仿学习Imitation Learning的发展

https://zhuanlan.zhihu.com/p/25688750

模仿学习（Imitation Learning）完全介绍

https://mp.weixin.qq.com/s/naq73D27vsCOUBperKto8A

从监督式到DAgger，综述论文描绘模仿学习全貌

https://mp.weixin.qq.com/s/LNNqp2KsEAljG26hY43mUw

ICML2018 模仿学习教程

https://mp.weixin.qq.com/s/f9vSgH1HQwGXBDb0UGHQyQ

深度学习进阶之无人车行为克隆

# RL与神经科学

Pavlov Model（1901）

Rescorla-Wagner Model（1972）

Thorndike’s Puzzle Box（1910）

参考：

https://zhuanlan.zhihu.com/p/24437724

学习理论之Rescorla-Wagner模型

# RL参考资源

https://mp.weixin.qq.com/s/Fn1s9Ia8L1ckgn6iP24FhQ

如何让神经网络具有好奇心

https://mp.weixin.qq.com/s/PBf-YrkhwhPYXuiOGyahxQ

强化学习遭遇瓶颈！分层RL将成为突破的希望

https://zhuanlan.zhihu.com/p/58815288

强化学习之值函数学习

https://mp.weixin.qq.com/s/8Cqknze_iosz6Z6cqnuK5w

谷歌提出强化学习新算法SimPLe，模拟策略学习效率提高2倍

https://mp.weixin.qq.com/s/hKGS4Ek5prwTRJoMCaxQLA

强化学习Exploration漫游

https://zhuanlan.zhihu.com/p/65116688

值分布强化学习（01）

https://zhuanlan.zhihu.com/p/65364484

值分布强化学习（02）

https://zhuanlan.zhihu.com/p/62363784

强化学习之策略搜索

https://mp.weixin.qq.com/s/j9Cs5M9gyITu2u_XDkKm-g

Policy Gradient——一种不以loss来反向传播的策略梯度方法

https://mp.weixin.qq.com/s/x6gKTuYIx8y25KX-fCc5bA

蒙特卡洛梯度估计方法（MCGE）简述
