---
layout: post
title:  深度学习（三十四）——深度强化学习（1）
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

Silver的名声直追他的导师Sutton，这个教程也流传很广。后续介绍的教程中，多有对它的抄袭。

http://www.meltycriss.com/2017/09/09/note-reinforcement-learning/

课程笔记《UCL强化学习》。这个blog包含大量的思维导图。

https://mp.weixin.qq.com/s/_PVe7Gcq7Yk8nOFJFPcUQw

叶强：David Silver《深度强化学习》公开课教程学习笔记完整版

http://web.stanford.edu/class/cs234/syllabus.html

CS234: Reinforcement Learning

http://rail.eecs.berkeley.edu/deeprlcourse/

CS 294: Deep Reinforcement Learning

>以上1本书+4个课程，基本就是目前RL领域的黄金搭档了。Stanford的课程内容比较新，但是很浅。UCB的课程通常都是给入门以后的人准备的，无论DL还是RL，都是这样。Sutton和Silver的课程内容比较老，但是很有深度。和CV领域只需要学习DL，而不需要学习传统方法不同，按照Sutton的说法，基本算法原理远比神经网络更重要。

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

https://mp.weixin.qq.com/s/dS0oQbGtrdd4rS25cBNyoQ

面向Open AI, TensorFlow, Keras的强化学习书籍《Reinforcement Learning》

https://102.alibaba.com/downloadFile.do?file=1517812754285/reinforcement_learning.pdf

《强化学习在阿里的技术演进与业务创新》，这是阿里出品的RL实战类书籍。

https://mp.weixin.qq.com/s/RbUcEOctRm8kX6_Ar4J0CA

446页简易Python强化学习教程书籍

https://mp.weixin.qq.com/s/7DlbuJI_gARJRABnlZQcxQ

伯克利大学ICML2018强化学习80页教程

https://mp.weixin.qq.com/s/7WnlNvxk0KKVPYqvSi7fKA

40页 PPT，BMM夏令营《强化学习简明教程》

https://mp.weixin.qq.com/s/VelM7ndXfevXKfPno-T9jQ

微软亚研130PPT教程：强化学习简介

https://sites.ualberta.ca/~szepesva/RLBook.html

《Algorithms for Reinforcement Learning》

https://mp.weixin.qq.com/s/tZjIdNSLvIVzho-IlCvm6A

93页随机近似与强化学习教程分享

https://mp.weixin.qq.com/s/o1wLREqtIZpzH2NxLl9M7A

深度强化学习简介

## 论文

《A Brief Survey of Deep Reinforcement Learning》

《Asynchronous Methods for Deep Reinforcement Learning》

《Deep Reinforcement Learning for Dialogue Generation》

## blog

https://zhuanlan.zhihu.com/sharerl

强化学习知识大讲堂

https://zhuanlan.zhihu.com/intelligentunit

一个DL+RL的专栏

## 概述

![](/images/img2/RL_intro.png)

上图是深度强化学习的脉络图，参考文献中还有该领域的关键论文列表。

参考：

https://mp.weixin.qq.com/s/781fVvKr4yuq4q2GI1Y1gA

从Zero到Hero，OpenAI重磅发布深度强化学习资源

https://mp.weixin.qq.com/s/t5dvg5OaNgBNz1eF6Ok3SQ

深度强化学习关键论文列表

## Deep Q-learning Network

Deep Q-learning Network是DL在RL领域的开山之作。它的思想主要来自于Deepmind的两篇论文：

《Playing Atari with Deep Reinforcement Learning》

《Human-level control through deep reinforcement learning》

Deepmind是当今DL领域最前沿的科研机构，尤其在RL领域更是领先同行一大截，是当之无愧的RL王者。

![](/images/img2/DQN.png)

上图是DQN的网络结构图。由于这里的任务是训练Atari游戏的AI，因此网络的输入实际上就是游戏的画面。而理解游戏画面，就需要一定的CNN结构。所以DQN的结构实际上和一般的CNN是一致的，其关键要害在于loss函数的设定。

由《机器学习（三十一）》中的“价值函数的近似表示”可知，

$$$$

![](/images/img2/DQN.jpg)

参考：

https://zhuanlan.zhihu.com/p/21262246

DQN从入门到放弃1 DQN与增强学习

https://zhuanlan.zhihu.com/p/21292697

DQN从入门到放弃2 增强学习与MDP

https://zhuanlan.zhihu.com/p/21340755

DQN从入门到放弃3 价值函数与Bellman方程

https://zhuanlan.zhihu.com/p/21378532

DQN从入门到放弃4 动态规划与Q-Learning

https://zhuanlan.zhihu.com/p/21421729

DQN从入门到放弃5 深度解读DQN算法

https://zhuanlan.zhihu.com/p/21547911

DQN从入门到放弃6 DQN的各种改进

https://zhuanlan.zhihu.com/p/21609472

DQN从入门到放弃7 连续控制DQN算法-NAF

https://zhuanlan.zhihu.com/p/21434933

DQN实战篇 从零开始安装Ubuntu, Cuda, Cudnn, Tensorflow, OpenAI Gym

https://mp.weixin.qq.com/s/0HukwNmg3k-rBrIBByLhnQ

深度Q学习：一步步实现能玩《毁灭战士》的智能体

https://mp.weixin.qq.com/s/x-qCA0TzoVUtZ8VAf8ey0A

深度Q学习介绍

https://mp.weixin.qq.com/s/snNvN4FFP0VEZwDA0TAp1w

强化学习训练Chrome小恐龙Dino Run：最高超过4000分

## reward modeling

![](/images/img2/reward_modeling.jpg)

训练一个奖励模型，其中包含来自用户的反馈，从而捕捉他们的意图。与此同时，我们通过强化学习训练一个策略，使奖励模型的奖励最大化。换句话说，我们把学习做什么(奖励模型)和学习怎么做(策略)区分开来。

参考：

https://mp.weixin.qq.com/s/4yGQtHtMqWlaB7MAsr8T_g

DeepMind重磅论文：通过奖励模型，让AI按照人类意图行事

## 参考

https://www.nervanasys.com/demystifying-deep-reinforcement-learning/

深度强化学习揭秘

https://zhuanlan.zhihu.com/p/24446336

深度强化学习Deep Reinforcement Learning学习整理

https://mp.weixin.qq.com/s/7BsXPQ8wC6_fHulU63ZQiQ

当强化学习遇见泛函分析

https://mp.weixin.qq.com/s/K82PlSZ5TDWHJzlEJrjGlg

深度学习与强化学习

https://mp.weixin.qq.com/s/6n5HawyR4AgH8Dq0gJMw2g

强化学习的基本概念与代码实现

http://mp.weixin.qq.com/s/lLPRwInF5qaw7ewYHOpPyw

深度强化学习资料

https://zhuanlan.zhihu.com/p/27699682

荐译一篇通俗易懂的策略梯度（Policy Gradient）方法讲解

http://lamda.nju.edu.cn/yangjw/project/drlintro.html

深度强化学习初探

https://zhuanlan.zhihu.com/p/21498750

深度强化学习导引

https://mp.weixin.qq.com/s/RnUWHa6QzgJbE_XqLeAQmg

深度强化学习，决策与控制

https://mp.weixin.qq.com/s/RLEuaiRdq6AbTSUcYQ5O3A

深度强化学习在NLP怎么用？看清华黄民烈老师这一份120页《自然语言处理和搜索中的深度强化学习应用》讲义

https://mp.weixin.qq.com/s/I8IwPCY6-zocJKFXMr6rUg

深度强化学习的18个关键问题

https://mp.weixin.qq.com/s/_dskX5U8gHAEl6aToBvQvg

从Q学习到DDPG，一文简述多种强化学习算法

https://zhuanlan.zhihu.com/p/29019246

基于策略的增强学习

https://mp.weixin.qq.com/s/SckTPgEw7KWmfKXWriNIRg

浅谈强化学习的方法及学习路线

https://mp.weixin.qq.com/s/-JHHOQPB6pKVuge64NkMuQ

DeepMind主攻的深度强化学习3大核心算法及7大挑战

https://mp.weixin.qq.com/s/OY56lJ_NFf5vVAgKfKyx2A

利用强化学习自动搜索最优化方法

https://simoninithomas.github.io/Deep_reinforcement_learning_Course/

老外写的简易深度强化学习入门

https://mp.weixin.qq.com/s/2SOHQElaYbplse3QqG9tYw

强化学习介绍

https://mp.weixin.qq.com/s/R30quVGK0TgjerLpiIK9eg

从算法到训练，综述强化学习实现技巧与调试经验

https://mp.weixin.qq.com/s/uDFsWebfLmka-zZX3Y_8kg

深度强化学习在面向任务的对话管理中的应用

https://mp.weixin.qq.com/s/nYOOwVoijl1p4V0A7yaI3w

机遇与挑战：用强化学习自动搜索优化算法

https://mp.weixin.qq.com/s/uymKtR_7IgMpfXcekfkCDg

从强化学习基本概念到Q学习的实现，打造自己的迷宫智能体

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

https://zhuanlan.zhihu.com/p/36322095

最前沿：从虚拟到现实，DRL让小狗机器人跑起来了！

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

