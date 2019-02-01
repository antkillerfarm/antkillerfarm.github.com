---
layout: post
title:  深度学习（三十八）——深度强化学习, Apollo, 多任务学习, RNN进阶
category: DL 
---

# 深度强化学习

https://mp.weixin.qq.com/s/oZDDP59o-1qwfz8prK3nJQ

伯克利最新研究：如何用目标图像进行机器视觉强化学习？

https://zhuanlan.zhihu.com/p/41467058

Policy Optimization with Demonstrations

https://openreview.net/pdf?id=rJzoujRct7

深度学习之斗地主

https://mp.weixin.qq.com/s/00zHwpw2xWP2fR9sDHE2Xw

BAIR讲述如何利用深度强化学习控制灵活手

https://mp.weixin.qq.com/s/V7RESEm4xzhW8tXEjKjn1Q

层次强化学习、记忆与预测模型

https://mp.weixin.qq.com/s/aNskPERmekw9yQVb7A3GPQ

Google大脑最新研究成果：使用强化学习实现动态系统的韧性计算

https://mp.weixin.qq.com/s/pJkCOCl6o70le1WsE9p3pg

在全景视频中预测头部运动：一种深度强化学习方法

https://mp.weixin.qq.com/s/fodjmmh_jJMh4hD3m2OrLg

凭借幻想的目标进行视觉强化学习

https://mp.weixin.qq.com/s/6HVSh7_9Akmf6OE8PGNy6Q

怎样让AI完成人类搞不定的任务？OpenAI提出迭代扩增法给AI设目标

https://mp.weixin.qq.com/s/JpZimrHALjuc-H9WF8sPZg

智能体只想看电视？谷歌新型好奇心方法让智能体离开电视继续探索

https://mp.weixin.qq.com/s/dic_ssebe32L30pAUxlP6w

谷歌AI-强化学习中的好奇和拖延

https://mp.weixin.qq.com/s/tieGV_tDWkVVW2YFes4AqA

学习何时做分类决策，深度好奇提出强化学习模型Jumper

https://mp.weixin.qq.com/s/GUyZ0U5_JlXCI-5mO796SA

超越DQN和A3C：深度强化学习领域近期新进展概览

https://mp.weixin.qq.com/s/THgo4YzhUN2PUkyI5sSnpw

开源啦：连DeepMind也捉急的游戏，OpenAI给你攻破第一关的高分算法

https://mp.weixin.qq.com/s/loH6M0_U1DVrod0Drkl4eg

深度强化学习教机器人自己穿衣服！

https://mp.weixin.qq.com/s/VqPPQnH22Y-XeojNEZn3YQ

CoRL 2018最佳系统论文：如此鸡贼的机器手，确定不是人在控制？

https://mp.weixin.qq.com/s/eEQhwV1cA4nEgEBcOKDenA

将逆向课程生成用于强化学习：伯克利新研究让智能体掌握全新任务

https://mp.weixin.qq.com/s/cO1VlYGwdRBAbPs7IgvcAA

超越传统强化学习的价值分布方法

https://mp.weixin.qq.com/s/oyWyb58l4ztqIEyrk0g01A

强化学习在美团“猜你喜欢”的实践

https://zhuanlan.zhihu.com/p/48186354

robot浅谈

https://mp.weixin.qq.com/s/f-rmdWq3kJUJGhuPwU8JtQ

深度策略梯度算法是真正的策略梯度算法吗？

https://zhuanlan.zhihu.com/p/51202503

强化学习在视觉上的应用（RL for computer Vision）

https://mp.weixin.qq.com/s/iBWjobr9srhB3MTiE_Wwmg

史上最强Atari游戏通关算法：蒙特祖玛获分超过200万！

https://mp.weixin.qq.com/s/S2eGPTON3XmfN830m4vaaA

腾讯AI Lab：可自适应于不同环境和任务的强化学习方法

# Apollo

Apollo是百度开源的无人驾驶平台，也是目前已开源的平台中最专业的。

官网：

http://apollo.auto/

代码：

https://github.com/ApolloAuto/apollo

参考：

https://github.com/TeamStuQ/skill-map/blob/master/data/map-Apollo.md

Apollo自动驾驶工程师技能图谱

http://mp.weixin.qq.com/s/baXCaD1EdHKCJbCIh2HDeA

Apollo高精地图技术与应用

https://mp.weixin.qq.com/s/qtoHF4Mnj_aeGBJbkjJUMA

Apollo小度车载系统

https://mp.weixin.qq.com/s/rWhnazEC35U7nsVpsxhFEg

Apollo 2.0 软硬件框架初探（一）

https://mp.weixin.qq.com/s/GccST3xJ1QRIVM7cFgsn3A

Apollo 2.0 软硬件框架初探（二）

https://mp.weixin.qq.com/s/UHQXF2Ju8erw9thm0Jjt2A

Apollo 2.0框架及源码分析(三)

https://mp.weixin.qq.com/s/3i8mJn4OPjt-KV1fp8bZQQ

Apollo Planning模块源代码分析（上）

https://mp.weixin.qq.com/s/DfdMDOQMncTau-9zGq9CYw

Apollo Planning模块源代码分析（下）

https://mp.weixin.qq.com/s/bH6khM8gO3JE4YT9i8hSlQ

Apollo 2.0自动驾驶平台技术解析与应用

https://mp.weixin.qq.com/s/EgsdlDhd8lIXU3bnTYJh0w

Apollo高精地图解析

https://mp.weixin.qq.com/s/027QogNXtW2NPIDVcGQGzw

Apollo“云+端”研发迭代新模式实战

https://mp.weixin.qq.com/s/Pjfhs-03HxFeyeKo1BmAPw

Apollo小度车载系统打造更舒心的出行

https://mp.weixin.qq.com/s/j4faPastB3nFmgH_nhVW6g

Apollo仿真平台如何Hold住99.9999%的复杂场景？

https://mp.weixin.qq.com/s/PFCCRBGWmaTQkeyOlG1qng

Apollo 2.0多传感器融合定位模块

https://mp.weixin.qq.com/s/KEdXmwUGHysy_LZAfdkmlw

Apollo的分布式可扩展计算平台探索

https://mp.weixin.qq.com/s/MfTsH9mowuWFA54z7vs5fA

Apollo Control模块源码解析

https://mp.weixin.qq.com/s/bIjnm74cJcHP8Zab4kt5zA

Apollo 2.5实时相对地图的分享

https://mp.weixin.qq.com/s/hU8__GPgxaCStSgv4FyLIw

无人驾驶行业及Apollo的Overview

https://mp.weixin.qq.com/s/48DcWP1kAoze0Lv8jHY3Ow

Apollo 2.5预测系统介绍

https://mp.weixin.qq.com/s/7ftd941pycD7h_R10cDecg

Apollo 2.5自动驾驶规划控制

https://mp.weixin.qq.com/s/y_mOKpgLEvDjoO-UdMBcyw

Apollo AdaperManager和Routing模块源代码分析

# 多任务学习

https://mp.weixin.qq.com/s/guAgXdhZSbEAkERSB1sLRA

多任务学习-Multitask Learning概述

https://mp.weixin.qq.com/s/A-CVKTz_moaFzTYywSt2gg

张宇 杨强：多任务学习概述

https://mp.weixin.qq.com/s/ZlCI02UdRuFBc-uKqIPE_w

深度学习多任务学习综述

https://mp.weixin.qq.com/s/QXOy2jo4RhCZrD5bSVzBOQ

共享相关任务表征，一文读懂深度神经网络多任务学习

https://mp.weixin.qq.com/s/mm9bXXTEzd8DwyYlMgGMZg

NLP多任务学习：一种层次增长的神经网络结构

https://mp.weixin.qq.com/s/X6FwTgr282hbqgOz3oBX-w

多任务学习概述论文：从定义和方法到应用和原理分析

https://blog.csdn.net/CoderPai/article/details/80080455

多任务学习与深度学习

https://blog.csdn.net/CoderPai/article/details/80087188

利用TensorFlow一步一步构建一个多任务学习模型

https://mp.weixin.qq.com/s/fcFb6WkJVP8TYpoxkQgiWQ

CMU提出“十字绣网络”，自动决定多任务学习的最佳共享层

https://mp.weixin.qq.com/s/i7WAFjQHK1NGVACR8x3v0A

自然语言十项全能：转化为问答的多任务学习

https://mp.weixin.qq.com/s/NpO1UP_mzyaeqW26xLY1Xg

CVPR 2018最佳论文作者亲笔解读：研究视觉任务关联性的Taskonomy

https://mp.weixin.qq.com/s/DUSa3SW1AgvJ0szL030NHQ

一个AI玩57个游戏，DeepMind离真正“万能”的AGI不远了！

https://mp.weixin.qq.com/s/P81I5vl99mV-4StNHmd_6A

作为多目标优化的多任务学习：寻找帕累托最优解

https://mp.weixin.qq.com/s/cyX_FzHF-6df9_AMUDu1aQ

One Model To Learn Them All

https://mp.weixin.qq.com/s/oY2yGrTmIpr3H2qvzYKInA

谷歌一个模型解决所有问题《One Model to Learn Them All》论文深度解读

https://mp.weixin.qq.com/s/VMHyEOn8uyRYt39QXVWUww

西湖大学张岳：自然语言处理中的多任务联合学习（384页PPT）

https://mp.weixin.qq.com/s/5zo-2WB9v2hOvKAC7ZhKuQ

基于学习的多任务框架L2MT，为多任务问题选择最优模型

# RNN进阶

## IndRNN

https://mp.weixin.qq.com/s/cAqpclkkeVrTiifz07HC1g

新型循环神经网络IndRNN：可构建更长更深的RNN

https://mp.weixin.qq.com/s/7-K-nZTijoYCaprRNYXxFg

新型RNN：将层内神经元相互独立以提高长程记忆

## 参考

https://mp.weixin.qq.com/s/0TLaC8ACXAFEK5aMNK9O-Q

简单循环单元SRU：像CNN一样快速训练RNN

https://zhuanlan.zhihu.com/p/27104240

CW-RNN收益率时间序列回归

https://mp.weixin.qq.com/s/SeR_zNZTu4t7kqB6ltNrmQ

从循环到卷积，探索序列建模的奥秘

https://mp.weixin.qq.com/s/_q69BV1r46S9X5wnLuFPSw

关于序列建模，是时候抛弃RNN和LSTM了

https://mp.weixin.qq.com/s/mIuAn4G9l3AKFAswpbaQdA

时间卷积网络（TCN）将取代RNN成为NLP预测领域王者

https://mp.weixin.qq.com/s/m5GRNp6qDfVfC0mkQ4m4Yw

神经语言模型如何利用上下文信息：长距离上下文的词序并不重要

https://mp.weixin.qq.com/s/kuoUnt2Vhz9NhfnNqMFAhQ

DeepMind提出关系RNN：构建关系推理模块，强化学习利器

https://mp.weixin.qq.com/s/wfOzCxe3L2t11VguYLGC9Q

上海交大搞出SRNN，比普通RNN也就快135倍

https://mp.weixin.qq.com/s/f0sv7c-H5o5L_wy2sUonUQ

CNN取代RNN？当序列建模不再需要循环网络

https://mp.weixin.qq.com/s/h3fF6Zvr1rSzSMpqdu8B0A

电子科大提出BT-RNN：替代全连接操作而大幅度提升LSTM效率

https://mp.weixin.qq.com/s/OgN4rVDKH5WABIaRY7CHog

如何让RNN神经元拥有基础通用的注意力能力

https://mp.weixin.qq.com/s/KBLCrupGIuPa5nVrxcS5WQ

新研究将GRU简化成单门架构，或更适用于语音识别

https://mp.weixin.qq.com/s/kQozftKd_n_kYIF7KKCc8g

短视频那么多，快手如何利用GRU实现各种炫酷的语音应用

https://mp.weixin.qq.com/s/xwuM2Vj8G7UyuEyzTyO13A

将CNN与RNN组合使用，天才还是错乱？

https://mp.weixin.qq.com/s/c7XkzjLH1n5EtqdQik618g

Dropout在RNN中的应用综述

https://mp.weixin.qq.com/s/K6LK47_GCTeZJPAW0-Xp4Q

多伦多大学提出可逆RNN：内存大降，性能不减！

https://mp.weixin.qq.com/s/lvaWx7J4HFTvYxy7-B9vYg

周志华等提出RNN可解释性方法，看看RNN内部都干了些什么

https://mp.weixin.qq.com/s/YbdiEHb8ld1pp1ehgBzTOQ

将未来信息作为正则项，Twin Networks加强RNN对长期依赖的建模能力

https://mp.weixin.qq.com/s/ty8RyPREo_EA7O8vA2pQuQ

AI编曲震撼人心，RNN生成流行音乐

https://mp.weixin.qq.com/s/vIL-bKHZK-6eXZYWxrc9vw

这种有序神经元，像你熟知的循环神经网络吗？

https://mp.weixin.qq.com/s/GGK9T0DeyIdD5ahHy5uvfg

LightRNN：存储和计算高效的RNN
