---
layout: post
title:  深度学习（三十八）——深度强化学习（2）, AlphaGo（2）, CNN进阶
category: DL 
---

# 深度强化学习

## 参考

https://mp.weixin.qq.com/s/K2DW_ntSWrlySpxgorF9dA

Python强化学习实战，Anaconda公司的高级数据科学家讲解

https://zhuanlan.zhihu.com/p/32089849

概要：NIPS 2017 Deep Learning for Robotics Keynote

https://mp.weixin.qq.com/s/CCQOHRCAolsorm8FEPdjoQ

什么时候强化学习未必好用？

https://mp.weixin.qq.com/s/I8IwPCY6-zocJKFXMr6rUg

深度强化学习的18个关键问题

https://mp.weixin.qq.com/s/gFHbLF-q91sddMAX1CRbEQ

俞扬：“审时度势”的高效强化学习

https://mp.weixin.qq.com/s/lstCIiNs_qA6k7GCYUBv2w

阿尔伯塔大学提出新型多步强化学习方法，结合已有TD算法实现更好性能

https://mp.weixin.qq.com/s/af10gqXNfohPEISSnpJ0tQ

UCB吴翼&FAIR田渊栋等人提出强化学习环境Hourse3D

https://mp.weixin.qq.com/s/ybyZpaHr-JJg7CCdXGOl5A

Seq2seq强化学习实战

https://mp.weixin.qq.com/s/TUk1PWT9CfPGEW77UKxpjw

三招武林绝学带你玩转“强化学习”

https://mp.weixin.qq.com/s/_dskX5U8gHAEl6aToBvQvg

从Q学习到DDPG，一文简述多种强化学习算法

https://mp.weixin.qq.com/s/p2hlc2PsLgrvxOF8wBZANg

李飞飞高徒范麟熙解析强化学习在游戏和现实中的应用

http://mp.weixin.qq.com/s/EPbKE-TAnAPugJDhXHEyNA

DeepMind开源Psychlab平台——搭建AI和认知心理学的桥梁

https://mp.weixin.qq.com/s/TlcVxjhHXXGSpIvptC4H8g

使用Gym和CNN构建多智能体自动驾驶马里奥赛车

https://mp.weixin.qq.com/s/4qeHfU9GS4aDWOHsu4Dw2g

记忆增强蒙特卡洛树搜索细节解读

https://mp.weixin.qq.com/s/Vdt5h8APAFoeVxFYKlywpg

用DeepMind的DQN解数学题，准确率提升15%

https://mp.weixin.qq.com/s/1zJyw67B6DqsHEJ3avbsfQ

DeepMind推出分布式深度强化学习架构IMPALA，让一个Agent学会多种技能

https://mp.weixin.qq.com/s/xJ_g3BvbM-WaIyLthHdhEw

DeepMind发布通用强化学习新范式，自主机器人可学会任何任务

https://mp.weixin.qq.com/s/_lmz0l1vP_CQ6p6DdFnHWA

谷歌大脑工程师的深度强化学习劝退文

https://mp.weixin.qq.com/s/zRcq1LMtK72Cl4T3877tgQ

牛津教授吐槽DeepMind心智神经网络，还推荐了这些多智能体学习论文

https://mp.weixin.qq.com/s/To3pnx1hVq_4p7UnQVMw9A

斯坦福大学&DeepMind联合提出机器人控制新方法，RL+IL端到端地学习视觉运动策略

https://mp.weixin.qq.com/s/U0K79ELLj4wsOR4sd5G4Vw

Vicarious详解新型图式网络：赋予强化学习泛化能力

https://zhuanlan.zhihu.com/p/29019246

基于策略的增强学习

https://mp.weixin.qq.com/s/C8hsGkHGtoaS9Vzm6Ub4tw

Berkeley提出“随机搜索”训练线性策略，提高RL的性能

https://mp.weixin.qq.com/s/SckTPgEw7KWmfKXWriNIRg

浅谈强化学习的方法及学习路线

https://mp.weixin.qq.com/s/vYDb1rTdPxO1sIS38VX5xA

DeepMind的AI学会了画画，利用强化学习完全不需人教：SPIRAL

https://mp.weixin.qq.com/s/rpPN2rgru6krRz2fr1RhsQ

模拟世界的模型：谷歌大脑与Jürgen Schmidhuber提出“人工智能梦境”

https://mp.weixin.qq.com/s/0v57oHMEDcJuUivs8D5pnQ

善于单挑却难以协作，构建多智能体AI系统为何如此之难？

https://simoninithomas.github.io/Deep_reinforcement_learning_Course/

老外写的简易深度强化学习入门

https://mp.weixin.qq.com/s/AelAD57G4GOh7qm-_rvYsg

伯克利提出DeepMimic：使用强化学习练就18般武艺

https://zhuanlan.zhihu.com/p/35567591

强化学习在关系抽取、QA场景的应用

https://mp.weixin.qq.com/s/oOslkEklaZSbRb8eDDCRBw

天津大学、东京大学等研究：用深度强化学习检测模型缺陷

https://mp.weixin.qq.com/s/DNT9rMynbN4Th0AVDHeY_w

BAIR提出人机合作新范式：教你如何高效安全地在月球着陆

https://zhuanlan.zhihu.com/p/36375292

最前沿：当我们以为Rainbow就是Atari游戏的巅峰时，Ape-X出来把Rainbow秒成了渣！

https://mp.weixin.qq.com/s/KqLCTSYk1C0wYpJw-hpc1g

论强化学习和概率推断的等价性：一种全新概率模型

https://mp.weixin.qq.com/s/FROyReDu7i5amGv-J4cmtg

“世界模型”实现，一步步让机器掌握赛车和躲避火球的技能

https://mp.weixin.qq.com/s/curzH8WrFyl5WWT4lDfpwQ

Chrome暗藏的恐龙跳一跳，已经被AI轻松掌握了

https://mp.weixin.qq.com/s/1b3_AiFhwXqxb7FozdRYIQ

最in强化学习+NLP技术分享会

https://mp.weixin.qq.com/s/RKQb7-mQ-ELRRq18db02Pg

DeepMind大突破！AI模拟大脑导航功能，学会像动物一样“抄近路”

https://mp.weixin.qq.com/s/qBwszD9rn4gKazXdwqexSQ

MIT提出使用“深度强化学习”帮助智能体在运动中做出“动作决策”

https://mp.weixin.qq.com/s/i-udn1M4kiJpF8U7u5Uepg

专家解读DeepMind最新论文：深度学习模型复现大脑网格细胞

https://mp.weixin.qq.com/s/AS1VFjBFnSk19QJ28tBVWA

NIPS 2017斯坦福赛题大公开：强化学习模拟人类肌肉骨骼模型

https://mp.weixin.qq.com/s/TWjFWe6-dZWDoTi5gN1BxA

深度强化学习在指代消解中的一种尝试

https://mp.weixin.qq.com/s/YnMgJDAh3XhyyNdI8RXmtw

腾讯知文等提出新型生成式摘要模型：结合主题信息和强化训练生成更优摘要

https://mp.weixin.qq.com/s/LZbprcnben6vPqsoC1DgDA

DeepMind提出元梯度强化学习算法，显著提高大规模深度强化学习应用的性能

https://mp.weixin.qq.com/s/JbCIBFDRvg5qcpZ11g58dw

DeepMind提出关系性深度强化学习：在星际争霸2任务中获得最优水平

# AlphaGo（续）

## 参考

https://mp.weixin.qq.com/s/Sfv-jzQAkN0PsZOGZUQhkQ

AlphaGo Zero横空出世，DeepMind Nature论文解密不使用人类知识掌握围棋

https://mp.weixin.qq.com/s/oAxouYX7-wDC5okbu--Wuw

Nature重磅：人工智能从0到1, 无师自通完爆阿法狗100-0

https://zhuanlan.zhihu.com/p/30262872

关于AlphaGo Zero

https://zhuanlan.zhihu.com/p/30263585

DeepMind新一代围棋程序AlphaGo Zero再次登上Nature

https://www.zhihu.com/question/66861459

如何评价DeepMind发表在Nature上的AlphaGo Zero？

http://www.alphago-games.com/

AlphaGo的棋谱

https://deepmind.com/blog/alphago-zero-learning-scratch/

AlphaGo Zero官方声明

https://zhuanlan.zhihu.com/mathNote

某牛的专栏，主要讲自制AlphaGo

https://mp.weixin.qq.com/s/DC9QqHdWT0xFnowEBuJDbw

自动化所解读“深度强化学习”：从AlphaGo到AlphaGoZero

https://mp.weixin.qq.com/s/uZtaxRwROCqYmL2k6Muxaw

从阿尔法狗元(AlphaGo Zero)的诞生看终极算法的可能性

https://mp.weixin.qq.com/s/i5OmLu8aNbypiTUmP4teeQ

刘遥行：深入浅出看懂AlphaGo Zero

https://mp.weixin.qq.com/s/aBrwbB_DOGTen-6XL7LGFQ

邓侃：白话蒙特卡洛树搜索和ResNet

https://mp.weixin.qq.com/s/nbTkr0PImlXUSYl91HD91Q

AlphaGo背后的力量：蒙特卡洛树搜索入门指南

https://mp.weixin.qq.com/s/-tH7DQo1cK9gA0bcpBJSDA

AlphaGo Zero：笔记与伪代码

https://mp.weixin.qq.com/s/CJuVoOf7idUChFIn7dH0Lg

围棋中的数学原理

https://mp.weixin.qq.com/s/d46qNFaftt4wxpV4sZnG-w

一张图看懂AlphaGo Zero

https://zhuanlan.zhihu.com/p/31749249

比AlphaGo Zero更强的AlphaZero问世，8小时解决一切棋类！

https://mp.weixin.qq.com/s/L7bZMkqyncwEt6D5tK1OdQ

AlphaZero炼成最强通用棋类AI，DeepMind强化学习算法8小时完爆人类棋类游戏

https://mp.weixin.qq.com/s/tFdnxqV5a5xZrFtB6E0AiQ

新AlphaZero出世称霸棋界，8小时搞定一切棋类！自对弈通用强化学习无师自通！

https://mp.weixin.qq.com/s/qYWsFBKNCKCGUmizX_1sVg

AlphaGo 教学工具终于上线了！

https://mp.weixin.qq.com/s/JxbIeDk8_wnYu_ewUHp29g

深度学习与围棋实战书籍《Deep Learning and the Game of Go》

https://mp.weixin.qq.com/s/gsRnbknytz2FY2dWgdWEYg

精通国际象棋的AI研究员：AlphaZero真的是一次突破吗？

https://mp.weixin.qq.com/s/Przl4ivbNuOFmz4pcYTrpQ

浅述：从Minimax到AlphaZero，完全信息博弈之路（1）

https://zhuanlan.zhihu.com/p/32089487

AlphaZero实战：从零学下五子棋

http://mp.weixin.qq.com/s/72riTTC3w0q9oF5H-51kXA

手把手教你搭建AlphaZero（使用Python和Keras）

https://mp.weixin.qq.com/s/Qw2tT7H1PwDvPgOYy8YUsQ

AlphaGo Zero代码迟迟不开源，TF等不及自己推了一个

https://mp.weixin.qq.com/s/Vq-osjgNXJQu5avGkxQdsw

手把手：AlphaGo有啥了不起，我也能教你做一个

https://mp.weixin.qq.com/s/ajajJ9yJZsOy4Vc0ULBxXg

国际象棋版AlphaZero出来了诶，还开源了Keras实现

https://mp.weixin.qq.com/s/snNvN4FFP0VEZwDA0TAp1w

强化学习训练Chrome小恐龙Dino Run：最高超过4000分

https://mp.weixin.qq.com/s/kmk3mKASZPixA8VS9sipcA

走近流行强化学习算法：最优Q-Learning

# CNN进阶

https://mp.weixin.qq.com/s/gwH9s1ggMTj2dJkad9wUuw

从VGG到NASNet，一文概览图像分类网络

https://mp.weixin.qq.com/s/hIAIbpqItS09KDOSFxaeqg

从Inception v1到Inception-ResNet，一文概览Inception家族的“奋斗史”

https://mp.weixin.qq.com/s/r143qYj8bziu_N-27RWRRw

机器学习5年大跃进，可能是个错觉
