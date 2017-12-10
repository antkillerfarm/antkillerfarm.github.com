---
layout: post
title:  深度学习（二十三）——深度强化学习, LSTM进阶
category: DL 
---

# 深度强化学习

## 教程

http://incompleteideas.net/sutton/book/the-book-2nd.html

《Reinforcement Learning: An Introduction》，Richard S. Sutton和Andrew G. Barto著。

>注：Richard S. Sutton，加拿大计算机科学家，麻省大学阿姆赫斯特分校博士（1984年），阿尔伯塔大学教授。强化学习之父，研究该领域长达三十余年。

>Andrew G. Barto，麻省大学阿姆赫斯特分校教授。Richard S. Sutton的导师。

http://incompleteideas.net/sutton/609%20dropbox/slides%20(pdf%20and%20keynote)/

Sutton的pdf和keynote

>注：资料中的.key文件即为keynote文件。这种格式是苹果设备上的专用ppt格式，在其他系统中查看不了。

http://www0.cs.ucl.ac.uk/staff/D.Silver/web/Teaching.html

UCL Course on RL

>David Silver，剑桥大学本科（1997年）+阿尔伯塔大学博士（2011年）。伦敦大学学院讲师。现为DeepMind研究员。AlphaGo之父。

Silver的名声直追Sutton，这个教程也流传很广。后续介绍的教程中，多有对它的抄袭。

http://www.meltycriss.com/2017/09/09/note-reinforcement-learning/

课程笔记《UCL强化学习》。这个blog包含大量的思维导图。

https://mp.weixin.qq.com/s/_PVe7Gcq7Yk8nOFJFPcUQw

叶强：David Silver《深度强化学习》公开课教程学习笔记完整版

http://web.stanford.edu/class/cs234/syllabus.html

CS234: Reinforcement Learning

http://www.eecs.wsu.edu/~taylorm/17_580/index.html

CptS 580: Reinforcement Learning

http://www.eecs.wsu.edu/~taylorm/2011_cs420/index.html

Artificial Intelligence。这个课程名义上叫AI，实则包括状态空间搜索、强化学习和贝叶斯网络三部分内容。

http://www.eecs.wsu.edu/~taylorm/2010_cs414/index.html

Introduction to Machine Learning。Matthew E. Taylor的本行是RL，所以不管什么课程，都有RL的内容。

>Matthew E. Taylor，安默斯特学院本科（2001年）+德州大学奥斯汀分校博士（2008年）。华盛顿州立大学副教授。

https://katefvision.github.io/

CMU: Deep Reinforcement Learning and Control

https://github.com/aikorea/awesome-rl

提供了RL方面的资源网页。aikorea还提供了同类的资源收集网页：awesome-rnn, awesome-deep-vision, awesome-random-forest。

## 论文

《A Brief Survey of Deep Reinforcement Learning》

## blog

https://zhuanlan.zhihu.com/sharerl

强化学习知识大讲堂

https://zhuanlan.zhihu.com/intelligentunit

一个DL+RL的专栏

## 参考

https://www.nervanasys.com/demystifying-deep-reinforcement-learning/

深度强化学习揭秘

https://zhuanlan.zhihu.com/p/24446336

深度强化学习Deep Reinforcement Learning学习整理

https://mp.weixin.qq.com/s/7BsXPQ8wC6_fHulU63ZQiQ

当强化学习遇见泛函分析

https://mp.weixin.qq.com/s/K82PlSZ5TDWHJzlEJrjGlg

深度学习与强化学习

https://mp.weixin.qq.com/s/KNXD-MpVHQRXYvJKTqn6WA

完善强化学习安全性：UC Berkeley提出约束型策略优化新算法

http://mp.weixin.qq.com/s/lLPRwInF5qaw7ewYHOpPyw

深度强化学习资料

https://mp.weixin.qq.com/s/aVWHlwOmNIqOlu3025_RXQ

DeepMind提出多任务强化学习新方法Distral

https://zhuanlan.zhihu.com/p/27699682

荐译一篇通俗易懂的策略梯度（Policy Gradient）方法讲解

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

https://mp.weixin.qq.com/s/5Go20MyBxdVI1r5SkwA6lw

面向星际争霸：DeepMind提出多智能体强化学习新方法

https://mp.weixin.qq.com/s/nYOOwVoijl1p4V0A7yaI3w

机遇与挑战：用强化学习自动搜索优化算法

https://mp.weixin.qq.com/s/uymKtR_7IgMpfXcekfkCDg

从强化学习基本概念到Q学习的实现，打造自己的迷宫智能体

https://mp.weixin.qq.com/s/6_cW22DCzSw3DpUDrLXLcA

OpenAI提出强化学习近端策略优化，可替代策略梯度法

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

https://www.zhihu.com/question/49230922

强化学习（reinforcement learning)有什么好的开源项目、网站、文章推荐一下？

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

https://mp.weixin.qq.com/s/SqU74jYBrjtp9L-bnBuboA

教你完美实现深度强化学习算法DQN

https://zhuanlan.zhihu.com/p/31579144

让我们从零开始做一个机械手臂(强化学习)

https://mp.weixin.qq.com/s/FiR_GRYqJYpJRO-2p44-Cg

伯克利强化学习新研究：机器人只用几分钟随机数据就能学会轨迹跟踪

https://mp.weixin.qq.com/s/u49cuDV21ITs1aV9tJR85g

Pieter Abbeel：《深度学习在机器人中的应用》

https://mp.weixin.qq.com/s/_dHjZQ_7_7H34PHhV_lC3w

全新强化学习算法详解，看贝叶斯神经网络如何进行策略搜索

https://mp.weixin.qq.com/s/RH4ifA46njdC7fyRI9kVMg

深度Q网络与视觉格斗类游戏

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

# LSTM进阶

## 《Long short-term memory》

这是最早提出LSTM这个概念的论文。这篇论文偏重数学推导，实话说不太适合入门之用。但既然是起点，还是有列出来的必要。

## 《LSTM Neural Networks for Language Modeling》

这也是一篇重要的论文。

## 《Sequence to Sequence - Video to Text》

https://vsubhashini.github.io/s2vt.html

![](/images/article/S2VTarchitecture.png)

## 《Long-term Recurrent Convolutional Networks for Visual Recognition and Description》

Long-term Recurrent Convolutional Networks是LSTM的一种应用方式，它结合了LSTM、CNN、CRF等不同网络组件。

![](/images/article/LSTM_X.png)

上图展示了LSTM在动作识别、图片和视频描述等任务中的网络结构。

![](/images/article/LSTM_X_2.png)

上图展示了图片描述任务中几种不同的网络连接方式：

1.单层LRCN。

2.双层LRCN。CNN连接在第一个LSTM层。传统的LSTM只有一个输入，这里的CNN是第二个输入，也就是所谓的静态输入。可参看caffe的LSTM实现。

2.双层LRCN。CNN连接在第二个LSTM层。

![](/images/article/LSTM_X_3.png)

![](/images/article/LSTM_X_4.png)

![](/images/article/LSTM_X_5.png)

这是视频描述任务中LSTM和CRF结合的示例。

## 《Training RNNs as Fast as CNNs》

这篇论文提出了如下图所示的Simple Recurrent Unit的新结构：

![](/images/article/SRU.jpg)

由于普通LSTM计算步骤中，很多当前时刻的计算都依赖$$h_{t-1}$$的值，导致整个网络的计算无法并行化。SRU针对这一点去掉了当前时刻计算对于$$h_{t-1}$$的依赖，而仅保留$$C_{t-1}$$（这个计算较为廉价）以记忆信息，大大改善了整个RNN网络计算的并行性。

参考：

https://mp.weixin.qq.com/s/4IHzOAvNhHG9c8GP0zXVkQ

Simple Recurrent Unit For Sentence Classification

