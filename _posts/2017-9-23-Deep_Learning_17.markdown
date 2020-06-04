---
layout: post
title:  深度学习（十七）——自动求导, 无监督/半监督/自监督深度学习（1）
category: DL 
---

# 自动求导

DL发展到现在，其基本运算单元早就不止CNN、RNN之类的简单模块了。针对新运算层出不穷的现状，各大DL框架基本都实现了自动求导的功能。

论文：

《Automatic Differentiation in Machine Learning: a Survey》

## Numerical differentiation

数值微分最大的特点就是很直观，好计算，它直接利用了导数定义：

$$f'(x)=\lim_{h\to 0}{f(x+h)-f(x)\over h}$$

不过这里有一个很大的问题：h怎么选择？选大了，误差会很大；选小了，不小心就陷进了浮点数的精度极限里，造成舍入误差。

第二个问题是对于参数比较多时，对深度学习模型来说，上面的计算是不够高效的，因为每计算一个参数的导数，你都需要重新计算$$f(x+h)$$。

因此，这种方法并不常用，而主要用于做梯度检查（Gradient check），你可以用这种不高效但简单的方法去检查其他方法得到的梯度是否正确。

## Symbolic differentiation

符号微分的主要步骤如下：

1.需要预置基本运算单元的求导公式。

2.遍历计算图，得到运算表达式。

3.根据导数的代入法则和四则运算法则，求出复杂运算的求导公式。

这种方法没有误差，是目前的主流，但遍历比较费时间。

## Automatic differentiation

除此之外，常用的自动求导技术，还有Automatic differentiation。（请注意这里的AD是一个很狭义的概念。）

类比复数的概念：

$$x = a + bi \quad (i^2 = -1)$$

我们定义Dual number：

$$x \mapsto x = x + \dot{x} d \quad (d^2=0)$$

定义Dual number的运算法则：

$$(x + \dot{x}d) + ( y + \dot{y}d) = x + y + (\dot{x} + \dot{y})d$$

$$(x + \dot{x}d) ( y + \dot{y}d) = xy + \dot{x}yd + x\dot{y}d  +  \dot{x}\dot{y}d^2 = xy + (\dot{x}y+ x\dot{y})d$$

$$-(x + \dot{x}d) = - x - \dot{x}d$$

$$\frac{1}{x + \dot{x}d} = \frac{1}{x} - \frac{\dot{x}}{x^2}d$$

dual number有很多非常不错的性质。以下面的指数运算多项式为例：

$$f(x) = p_0 + p_1x + p_2x^2 + ... + p_nx^n$$

用$$x + \dot{x}d$$替换x，则有：

$$f(x + \dot{x}d) =   p_0 + p_1(x + \dot{x}d) + ... +  p_n(x + \dot{x}d)^n \\ 
= p_0 + p_1x + p_2x^2 + ... + p_nx^n + \\ 
p_1\dot{x}d + 2p_2x\dot{x}d + ... + np_{n-1}x\dot{x}d\\ 
= f(x) + f'(x)\dot{x}d$$

可以看出d的系数就是$$f'(x)$$。

## 不可导函数的求导

不可导函数的求导，一般采用泰勒展开的方式。典型的算法有PGD（Proximal Gradient Descent）。

参考：

https://blog.csdn.net/bingecuilab/article/details/50628634

Proximal Gradient Descent for L1 Regularization

## 参考

https://mp.weixin.qq.com/s/7Z2tDhSle-9MOslYEUpq6g

从概念到实践，我们该如何构建自动微分库

https://mp.weixin.qq.com/s/bigKoR3IX_Jvo-re9UjqUA

机器学习之——自动求导

https://www.jianshu.com/p/4c2032c685dc

自动求导框架综述

https://mp.weixin.qq.com/s/xXwbV46-kTobAMRwfKyk_w

自动求导--Deep Learning框架必备技术二三事

https://mp.weixin.qq.com/s/f0xFfA1inOVOdJnSZR4k6Q

自动微分技术

https://mp.weixin.qq.com/s/0tTlPG4hd9hcHORkZF6w1A

PyTorch的自动求导机制详细解析，PyTorch的核心魔法

https://mp.weixin.qq.com/s/PELBuCvu-7KQ33XBtlYfYQ

深度学习中的微分

https://zhuanlan.zhihu.com/p/24709748

矩阵求导术（上）

https://zhuanlan.zhihu.com/p/24863977

矩阵求导术（下）

https://mp.weixin.qq.com/s/2hu6a0wScJedwk3a5aKbIw

自动微分到底是什么？这里有一份自我简述

# 无监督/半监督/自监督深度学习

自监督学习是一种特殊目的的无监督学习。不同于传统的AutoEncoder等方法，仅仅以重构输入为目的，而是希望通过surrogate task学习到和高层语义信息相关联的特征。

## 参考

https://mp.weixin.qq.com/s/sDkGAhnFC027XjpUImeatw

自监督、半监督、无监督学习，傻傻分不清楚？最新综述来帮你！

https://mp.weixin.qq.com/s/L4GQF0eE7MjLPrb8UygCww

无监督深度学习全景教程（193页PDF）

https://mp.weixin.qq.com/s/kbqTHIOzAj1aERl4tm-kVA

2017上半年无监督特征学习研究成果汇总

https://mp.weixin.qq.com/s/J50L6hESBROfT8IIAnofQQ

Yan LeCun109页最新报告：图嵌入, 内容理解，自监督学习

https://mp.weixin.qq.com/s/s440gdbUhLP41rLPjfgsmQ

Yann Lecun自监督学习指南（附114页Slides全文下载）

https://mp.weixin.qq.com/s/foP1xSa5G8oNtAv_pI6AqQ

深度神经网络自监督视觉特征学习综述

https://mp.weixin.qq.com/s/sEHA6fb0XIXQWsmJGf3fTA

DeepMind发布自监督学习最新教程，附122页全文资料下载

https://mp.weixin.qq.com/s/-JoB1MJ0ZpkYLlToS7-AOA

牛津大学&DeepMind：自监督学习教程，141页ppt

https://mp.weixin.qq.com/s/HfqH-b8x8SsE6zb8pcF3Og

自监督学习（Self-Supervised Learning） 2018-2020年发展综述

https://mp.weixin.qq.com/s/2Wm6eQodwlc5XkjGKqhwCg

南京大学周志华教授综述论文：弱监督学习

https://mp.weixin.qq.com/s/aCWAU2RXk9fTzfFqOyjqUw

能自主学习的人工突触，为无监督学习开辟新的路径

https://mp.weixin.qq.com/s/9kMz-eNRwC51Fi0-7BfKzA

Active Learning: 一个降低深度学习时间，空间，经济成本的解决方案

https://mp.weixin.qq.com/s/ZvTm9omnIRqPXcLFbZtoeg

深度学习的关键：无监督深度学习简介

https://mp.weixin.qq.com/s/GHjmiB6F2W3Zo8gVllTyyQ

重现“世界模型”实验，无监督方式快速训练

https://mp.weixin.qq.com/s/3_VtdZNKBwNtMEMf2xc7qw

CVPR智慧城市挑战赛：无监督交通异常检测，冠军团队技术分享

https://mp.weixin.qq.com/s/3aAaM1DWsnCWEEbP7dOZEg

伯克利等提出无监督特征学习新方法，代码已开源

https://mp.weixin.qq.com/s/ZDPPWH570Vc6e1irwP1b1Q

精细识别现实世界图像：李飞飞团队提出半监督适应性模型

https://mp.weixin.qq.com/s/X1Alcl7rVfTtZGZ40iXjXw

Spotlight 论文：非参数化方法实现的极端无监督特征学习

https://mp.weixin.qq.com/s/kxEfoSjCF8n2jxlDfMaNDA

半监督学习在图像分类上的基本工作方式

https://mp.weixin.qq.com/s/uUMPUdG2TI10W5RumPaXkA

DeepMind无监督表示学习重大突破：语音、图像、文本、强化学习全能冠军！

https://mp.weixin.qq.com/s/_VC6PGdCjlhcsndpunIteg

何恺明等人提出新型半监督实例分割方法：学习分割Every Thing

https://mp.weixin.qq.com/s/qaxzSSDuuscwL5tt0QCQ0Q

破解人类识别文字之谜：对图像中的字母进行无监督学习

https://mp.weixin.qq.com/s/IsLlzDWnUXe8LVp4Y1Jb_A

35亿张图像！Facebook基于弱监督学习刷新ImageNet基准测试记录

https://mp.weixin.qq.com/s/TEk_i4kEjUqmAqF8LgTVjg

FAIR提出用聚类方法结合卷积网络，实现无监督端到端图像分类

https://mp.weixin.qq.com/s/dSncg1pDHpIFOT4mXrFntA

Yan Lecun自监督学习：机器能像人一样学习吗？ 110页PPT

https://mp.weixin.qq.com/s/W4zwKqkVQN4v-IKzGrkudg

通过传递不变性实现自监督视觉表征学习

https://zhuanlan.zhihu.com/p/30265894

自监督学习近期进展

https://mp.weixin.qq.com/s/cTlXMxcpzc7_5NVsTm1jcA

学习一帧，为整段黑白视频上色：谷歌提出自监督视觉追踪模型

https://mp.weixin.qq.com/s/Amr34SdrPZho1GQpFS7WBA

见微知著：语义分割中的弱监督学习

https://mp.weixin.qq.com/s/zOWA1oKbopZJuYIAYYlKTA

港中文-商汤联合论文：自监督语义分割的混合与匹配调节

https://mp.weixin.qq.com/s/5xlSoC5sgzsAwMYMSFCjnw

TextTopicNet:CMU开源无标注高精度自监督模型

https://mp.weixin.qq.com/s/343DfjOvkaozuxNK89V3zQ

前景目标检测的无监督学习

https://mp.weixin.qq.com/s/DwY0oGu-G30Szs-ArI5WaQ

程明明：面向弱监督的图像理解

https://mp.weixin.qq.com/s/LFOljv-Hr6JqyI6TQ2X4sw

半监督学习也能自动化？南大和第四范式提出Auto-SSL

https://mp.weixin.qq.com/s/83xAXrc_H_OExW3vii08hA

谷歌提出新方法：基于单目视频的无监督深度学习结构化

https://mp.weixin.qq.com/s/gr0_p4WFToTrDfy47h-p0A

基于自监督学习的视听觉信息同一性判断

https://mp.weixin.qq.com/s/Dqz97_U5pw_4d9KFblJfLg

基于自编码器的表征学习：如何攻克半监督和无监督学习？

https://mp.weixin.qq.com/s/LaIvAuBHYGNMug3NZ1pLhQ

半监督深度学习小结：类协同训练和一致性正则化

https://mp.weixin.qq.com/s/aBDgV7u93MAv2MogZKBmvw

Google提出Grasp2Vec模型：利用自监督方法学习物体表示

https://mp.weixin.qq.com/s/YfDZMEkOnxp0_ei2Oam-YQ

基于弱监督的视频时序动作检测的介绍

https://mp.weixin.qq.com/s/RiL-s50oOI--PZyIOd2E0g

弱监督语义分割最新方法资源列表

https://mp.weixin.qq.com/s/USOWECXk_az4b6eTssfOBw

基于弱监督深度学习的图像分割方法综述

https://mp.weixin.qq.com/s/8oEdQOmSRrkIaTVQdhk2Dw

无监督领域特定单图像去模糊

https://mp.weixin.qq.com/s/FpIaa8XoJ9GsHxL-W1Cl5Q

斯坦福AI实验室机器学习编程新范式：弱监督

https://mp.weixin.qq.com/s/ys9iiiBL3iL2SJL247AMlA

多伦多大学&NVIDIA最新成果：图像标注速度提升10倍！

https://mp.weixin.qq.com/s/V6xiG931OUJyVx15QFb_mQ

弱监督视觉理解笔记

https://mp.weixin.qq.com/s/HopNSLS75TgE28LfY02qog

不同视角构造cycle-consistency，降低视频标注成本

https://mp.weixin.qq.com/s/XiLBHkraT8lJcOu2faqK5g

关于弱监督学习，这可能是目前最详尽的一篇科普文

https://mp.weixin.qq.com/s/VnOfYuHQQf_q92VHVE3mrQ

谷歌新发布的半监督学习算法降低4倍错误率

https://mp.weixin.qq.com/s/rOj_J1zNYf-Vj9tqLG5KOQ

超强半监督学习MixMatch

https://zhuanlan.zhihu.com/p/66389797

虚拟对抗训练（VAT）：一种新颖的半监督学习正则化方法

https://mp.weixin.qq.com/s/DAtHXSfCpqCAZ0iVsfWkDA

半监督学习理论及其研究进展概述

https://mp.weixin.qq.com/s/eHzNIO-RSY-uf-K-OwtWfw

集多种半监督学习范式为一体，谷歌新研究提出新型半监督方法MixMatch

https://mp.weixin.qq.com/s/3el7bPAeJrTQGfWW29ewuA

新技术“红”不过十年，半监督学习为什么是个例外？

https://mp.weixin.qq.com/s/alnji5kgTxc34O7k78uGiA

无监督学习中的目标检测

https://mp.weixin.qq.com/s/8FtDhpgc-1j3TSL771N-Ng

无标注数据是鸡肋还是宝藏？阿里工程师这样用它

https://mp.weixin.qq.com/s/LdfLd2cZCdpvNYLKHUNwuA

简述无监督图像分类发展现状

https://mp.weixin.qq.com/s/qaLQK3uzaeyp68AbL0aOOQ

怎么在视频标注上省钱？这里有一个面向视频推荐的多视图主动学习
