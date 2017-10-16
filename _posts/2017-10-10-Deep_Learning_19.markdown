---
layout: post
title:  深度学习（十九）——深度强化学习, Ultra Deep Network
category: theory 
---

# 深度强化学习

## 教程

http://incompleteideas.net/sutton/book/the-book-2nd.html

《Reinforcement Learning: An Introduction》，Richard S. Sutton和Andrew G. Barto著。

>注：Richard S. Sutton，加拿大计算机科学家，麻省大学阿姆赫斯特分校博士（1984年），阿尔伯塔大学教授。强化学习之父，研究该领域长达三十余年。

>Andrew G. Barto，麻省大学阿姆赫斯特分校教授。Richard S. Sutton的导师。

http://web.stanford.edu/class/cs234/syllabus.html

CS234: Reinforcement Learning

## 概述

![](/images/article/reinforcement_learning.png)

## 参考

https://www.nervanasys.com/demystifying-deep-reinforcement-learning/

深度强化学习揭秘

http://blog.csdn.net/young_gy/article/details/73485518

强化学习之Q-learning简介

https://zhuanlan.zhihu.com/p/24446336

深度强化学习Deep Reinforcement Learning学习整理

https://mp.weixin.qq.com/s/KNXD-MpVHQRXYvJKTqn6WA

完善强化学习安全性：UC Berkeley提出约束型策略优化新算法

http://mp.weixin.qq.com/s/gHM7qh7UTKzatdg34cgfDQ

强化学习全解

http://mp.weixin.qq.com/s/lLPRwInF5qaw7ewYHOpPyw

深度强化学习资料

https://mp.weixin.qq.com/s/f6sq8cSaU1cuzt7jhsK8Ig

强化学习（Reinforcement Learning）基础介绍

https://mp.weixin.qq.com/s/TGN6Zhrea2LPxdkspVTlAw

算法工程师入门——增强学习

https://mp.weixin.qq.com/s/aVWHlwOmNIqOlu3025_RXQ

DeepMind提出多任务强化学习新方法Distral

https://mp.weixin.qq.com/s/laKJ_jfNR5L1uMML9wkS1A

强化学习（Reinforcement Learning）算法基础及分类

https://zhuanlan.zhihu.com/p/27699682

荐译一篇通俗易懂的策略梯度（Policy Gradient）方法讲解

https://mp.weixin.qq.com/s/Cvk_cePK9iQd8JIKKDDrmQ

强化学习的核心基础概念及实现

http://lamda.nju.edu.cn/yangjw/project/drlintro.html

深度强化学习初探

https://zhuanlan.zhihu.com/p/21498750

深度强化学习导引

https://mp.weixin.qq.com/s/RnUWHa6QzgJbE_XqLeAQmg

深度强化学习，决策与控制

https://mp.weixin.qq.com/s/W9yhj7_frLYWJocoBR1TMQ

避免AI错把黑人识别为大猩猩：伯克利大学提出协同反向强化学习

https://mp.weixin.qq.com/s/R308ohdMU8b7Ap4CLofvDg

OpenAI开源算法ACKTR与A2C：把可扩展的自然梯度应用到强化学习

https://mp.weixin.qq.com/s/-JHHOQPB6pKVuge64NkMuQ

DeepMind主攻的深度强化学习3大核心算法及7大挑战

https://mp.weixin.qq.com/s/YQpuYuzk0jv5OngH5u8bEg

阿里菜鸟物流：使用深度强化学习方法求解一类新型三维装箱问题

https://mp.weixin.qq.com/s/OY56lJ_NFf5vVAgKfKyx2A

利用强化学习自动搜索最优化方法

https://mp.weixin.qq.com/s/zWo2iSiJBEBwnFF478xxfQ

DeepMind：探索人类行为中的强化学习机制

https://mp.weixin.qq.com/s/R30quVGK0TgjerLpiIK9eg

从算法到训练，综述强化学习实现技巧与调试经验

https://mp.weixin.qq.com/s/TpRXxP25-3uqpgC9CBi-3Q

Yoshua Bengio等人提出MILABOT：强化学习聊天机器人

https://mp.weixin.qq.com/s/uDFsWebfLmka-zZX3Y_8kg

深度强化学习在面向任务的对话管理中的应用

https://mp.weixin.qq.com/s/GNbCu1lbOmwJDCU3vgMbtQ

OpenAI发布多智能体深度强化学习新算法LOLA

https://mp.weixin.qq.com/s/nYOOwVoijl1p4V0A7yaI3w

机遇与挑战：用强化学习自动搜索优化算法

https://mp.weixin.qq.com/s/uymKtR_7IgMpfXcekfkCDg

从强化学习基本概念到Q学习的实现，打造自己的迷宫智能体

https://mp.weixin.qq.com/s/6_cW22DCzSw3DpUDrLXLcA

OpenAI提出强化学习近端策略优化，可替代策略梯度法

https://mp.weixin.qq.com/s/4H2isN27elFL0dxTuGPw6Q

穆黎森讲增强学习

http://mp.weixin.qq.com/s/S4jhpNKYZP5YQWaiiOQGFA

DeepMind：“预测地图”海马体催生强化学习新算法

http://mp.weixin.qq.com/s/TBVVdX3erOpXNjXmhLmxOw

学“深度强化学习”，看懂DeepMind这篇文章就够了!

https://mp.weixin.qq.com/s/SZHMyWOXHM8T3zp_aUt-6A

DeepMind提出Rainbow：整合DQN算法中的六种变体

https://mp.weixin.qq.com/s/_Di73PkEWJV1-OLLHfz7yQ

组合在线学习：实时反馈玩转组合优化

https://mp.weixin.qq.com/s/lR6BSa_pJzcinkSaSWsM2A

伯克利提出强化学习新方法，可让智能体同时学习多个解决方案

https://mp.weixin.qq.com/s/P-iSI80IVmb5s-Q15Re2HQ

All In!我学会了用强化学习打德州扑克

https://mp.weixin.qq.com/s/xr-2cNoSYpCftLI3dV6zEw

如何使用深度强化学习帮助自动驾驶汽车通过交叉路口？

# Ultra Deep Network

## FractalNet

论文：

《FractalNet: Ultra-Deep Neural Networks without Residuals》

![](/images/article/FractalNet.png)

## Resnet in Resnet

论文：

《Resnet in Resnet: Generalizing Residual Architectures》

![](/images/article/RiR.png)

## Highway

论文：

《Training Very Deep Networks》

![](/images/article/highway.png)

Resnet对于残差的跨层传递是无条件的，而Highway则是有条件的。这种条件开关被称为gate，它也是由网络训练得到的。

## DenseNet

DenseNet是康奈尔大学博士后黄高（Gao Huang）、清华大学本科生刘壮（Zhuang Liu）、Facebook人工智能研究院研究科学家Laurens van der Maaten 及康奈尔大学计算机系教授Kilian Q. Weinberger于2016年提出的。论文当选CVPR 2017最佳论文。

论文：

《Densely Connected Convolutional Networks》

代码：

https://github.com/liuzhuang13/DenseNet

原始版本是Torch写的，官网上列出了其他框架的实现代码的网址。

![](/images/article/DenseNet_2.png)

上图是DenseNet的整体网络结构图。从整体层面来看，DenseNet主要由3个dense block组成。

![](/images/article/DenseNet.png)

参考：

https://www.leiphone.com/news/201708/0MNOwwfvWiAu43WO.html

CVPR 2017最佳论文作者解读：DenseNet 的“what”、“why”和“how”

https://zhuanlan.zhihu.com/p/28124810

为什么ResNet和DenseNet可以这么深？一文详解残差块为何有助于解决梯度弥散问题

https://mp.weixin.qq.com/s?__biz=MzI3MTA0MTk1MA==&mid=2651988934&idx=2&sn=0e5ffa195ef67a1371f3b5b223519121

ResNets、HighwayNets、DenseNets：用 TensorFlow 实现超深度神经网络

# NN的INT8计算

## 概述

NN的INT8计算是近来NN计算优化的方向之一。这方面的文章以Xilinx的白皮书较为经典：

https://china.xilinx.com/support/documentation/white_papers/c_wp486-deep-learning-int8.pdf

利用Xilinx器件的INT8优化开展深度学习

论文：

《On the efficient representation and execution of deep acoustic models》

参考：

https://www.chiphell.com/thread-1620755-1-1.html

新Titan X的INT8计算到底是什么鬼

https://mp.weixin.qq.com/s/S9VcoS_59nbZWe_P3ye2Tw

减少模型半数内存用量：百度&英伟达提出混合精度训练法

## NN硬件的指标术语

MACC：multiply-accumulate，乘法累加。

FLOPS：Floating-point Operations Per Second，每秒所执行的浮点运算次数。

显然NN的INT8计算主要以MACC为单位。


