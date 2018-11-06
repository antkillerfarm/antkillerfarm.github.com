---
layout: post
title:  深度学习（三十八）——深度强化学习（2）, Transformer
category: DL 
---

# 深度强化学习（续）

## 参考

https://mp.weixin.qq.com/s/K2DW_ntSWrlySpxgorF9dA

Python强化学习实战，Anaconda公司的高级数据科学家讲解

https://zhuanlan.zhihu.com/p/32089849

概要：NIPS 2017 Deep Learning for Robotics Keynote

https://mp.weixin.qq.com/s/CCQOHRCAolsorm8FEPdjoQ

什么时候强化学习未必好用？

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

https://mp.weixin.qq.com/s/To3pnx1hVq_4p7UnQVMw9A

斯坦福大学&DeepMind联合提出机器人控制新方法，RL+IL端到端地学习视觉运动策略

https://mp.weixin.qq.com/s/U0K79ELLj4wsOR4sd5G4Vw

Vicarious详解新型图式网络：赋予强化学习泛化能力

https://mp.weixin.qq.com/s/R308ohdMU8b7Ap4CLofvDg

OpenAI开源算法ACKTR与A2C：把可扩展的自然梯度应用到强化学习

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

https://mp.weixin.qq.com/s/SpCorsAiTWGYGiohT_gZtg

如何训练智能体Agent玩毁灭战士ViZDoom？

https://mp.weixin.qq.com/s/ewC2_8O29Gg0blxK7mHsYA

比TD、MC、MCTS指数级快，性能超越A3C、DDQN等模型，这篇RL算法论文在Reddit上火了

https://mp.weixin.qq.com/s/Q9QzXo_M3jYKjBWSsLYwgQ

强化学习+树搜索：一种新型程序合成方法

https://mp.weixin.qq.com/s/l46yJdRaftMlwr6odiSiHg

Yoshua Bengio团队基于深度强化学习打造聊天机器人MILABOT

https://mp.weixin.qq.com/s/6_cW22DCzSw3DpUDrLXLcA

OpenAI提出强化学习近端策略优化，可替代策略梯度法

http://mp.weixin.qq.com/s/S4jhpNKYZP5YQWaiiOQGFA

DeepMind：“预测地图”海马体催生强化学习新算法

https://mp.weixin.qq.com/s/KZHidOi5KYgg393R_JspBA

DeepMind都拿不下的游戏，刚刚被OpenAI玩出历史最高分

https://mp.weixin.qq.com/s/VrVdsxn94ux_46mIyS8f0w

谷歌大脑实现更宽广的智能体视野，在Atari2600上可持续超越人类玩家！

https://mp.weixin.qq.com/s/P5EysBHBaR6L3IfeSgo6fw

强化学习20分钟，剑桥博士教汽车学会自动驾驶！

https://mp.weixin.qq.com/s/IYyGgnoXZm6YfamLejqoNQ

深度强化学习试金石：DeepMind和OpenAI攻克蒙特祖玛复仇的真正意义

https://mp.weixin.qq.com/s/w3SsadgKaL8-tlzYLvMm-A

讲真？一天就学会了自动驾驶——强化学习在自动驾驶的应用

https://mp.weixin.qq.com/s/oyxqA_LYtze1f_YDwDZziQ

从零开始自学设计新型药物，UNC提出结构进化强化学习

https://zhuanlan.zhihu.com/p/39999667

强化学习路在何方？

https://mp.weixin.qq.com/s/oA98YyLqn1B22QZ5b_iDVA

IJCAI 2018：聚焦强化学习的学习效率

https://mp.weixin.qq.com/s/qWuoo6cGLWLk4OKlunR-Og

滴滴KDD 2018论文详解：基于强化学习技术的智能派单模型

https://zhuanlan.zhihu.com/p/43496459

解决Sparse Reward RL任务的简单回顾

https://mp.weixin.qq.com/s/G99vqIYeWzgQ4kL4p77cKA

用强化学习做神经机器翻译：中山大学&MSRA填补多项空白

https://zhuanlan.zhihu.com/p/43843955

BAIR：基于人类演示&RL的夹爪训练——高效、通用、低成本

https://mp.weixin.qq.com/s/ADZlLx6gMTFU6IoBCF669g

快1万倍！伯克利提出用深度RL优化SQL查询

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

# Transformer

之前的文章已经介绍了Attention和《Attention is All You Need》。但实际上，《Attention is All You Need》不仅提出了两种Attention模块，而且还提出了如下图所示的Transformer模型。该模型主要用于NMT领域，由于Attention不依赖上一刻的数据，同时精度也不弱于LSTM，因此有很好并行计算特性，在工业界得到了广泛应用。阿里巴巴和搜狗目前的NMT方案都是基于Transformer模型的。

![](/images/img2/Transformer.png)

$$FFN(x) = \max(0,xW_1 + b_1)W_2 + b_2$$

代码：

https://github.com/Kyubyong/transformer

参考：

http://jalammar.github.io/illustrated-transformer/

The Illustrated Transformer

http://jalammar.github.io/visualizing-neural-machine-translation-mechanics-of-seq2seq-models-with-attention/

Visualizing A Neural Machine Translation Model (Mechanics of Seq2seq Models With Attention)

https://zhuanlan.zhihu.com/p/39034683

Attention is all you need模型笔记

https://zhuanlan.zhihu.com/p/40920384

真正的完全图解Seq2Seq Attention模型

https://mp.weixin.qq.com/s/HquT_mKm7x_rbDGz4Voqpw

阿里巴巴最新实践：TVM+TensorFlow提高神经机器翻译性能

https://mp.weixin.qq.com/s/S_xhaDrOaPe38ZvDLWl4dg

从技术到产品，搜狗为我们解读了神经机器翻译的现状

https://mp.weixin.qq.com/s/vzjKU_0qhapWKOYZ4Rnj-Q

谷歌的机器翻译模型Transformer，现在可以用来做任何事了

https://mp.weixin.qq.com/s/lgGDTCF3qg84njv2IeHC9A

大规模集成Transformer模型，阿里达摩院如何打造WMT 2018机器翻译获胜系统

https://mp.weixin.qq.com/s/_UC2jlOfb34tfB_tsEXjMg

谷歌全新神经网络架构Transformer：基于自注意力机制，擅长自然语言理解

https://mp.weixin.qq.com/s/w3IKoygTLDsAxk1MB5JrGg

详细讲解Transformer新型神经网络在机器翻译中的应用

https://mp.weixin.qq.com/s/HzzDG8PpDlyilQjr2PH6PA

Transformer注解及PyTorch实现（上）

https://mp.weixin.qq.com/s/YDaSv5oHLEtyJrp4Y5e64A

Transformer注解及PyTorch实现（下）
