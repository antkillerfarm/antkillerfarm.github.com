---
layout: post
title:  深度学习（三十八）——深度强化学习（2）, Spiking Neuron Networks, 问答系统, CNN进阶
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

https://mp.weixin.qq.com/s/I8IwPCY6-zocJKFXMr6rUg

深度强化学习的18个关键问题

https://mp.weixin.qq.com/s/gFHbLF-q91sddMAX1CRbEQ

俞扬：“审时度势”的高效强化学习

https://mp.weixin.qq.com/s/lstCIiNs_qA6k7GCYUBv2w

阿尔伯塔大学提出新型多步强化学习方法，结合已有TD算法实现更好性能

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

https://mp.weixin.qq.com/s/SpCorsAiTWGYGiohT_gZtg

如何训练智能体Agent玩毁灭战士ViZDoom？

https://mp.weixin.qq.com/s/ewC2_8O29Gg0blxK7mHsYA

比TD、MC、MCTS指数级快，性能超越A3C、DDQN等模型，这篇RL算法论文在Reddit上火了

https://mp.weixin.qq.com/s/Q9QzXo_M3jYKjBWSsLYwgQ

强化学习+树搜索：一种新型程序合成方法

https://mp.weixin.qq.com/s/2SOHQElaYbplse3QqG9tYw

强化学习介绍

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

# Spiking Neuron Networks

除了基于BP算法的NN之外，Spiking Neuron Networks也是一大类NN。Spiking NN和人脑结构更相似，功耗也更小，但是相关训练和数据量化的算法尚不成熟，属于潜力股。

参考：

https://homepages.cwi.nl/~sbohte/publication/paugam_moisy_bohte_SNNChapter.pdf

Computing with Spiking Neuron Networks

https://mp.weixin.qq.com/s/6dpKSaLFVo-ge4gtbG8GQg

简述脉冲神经网络SNN：下一代神经网络

https://mp.weixin.qq.com/s/0n50YO1jIv_mxqe0EeS6kw

综述AI未来：神经科学启发的类脑计算

https://mp.weixin.qq.com/s/5KA7jtlRmnXxijGQhU1k4A

DeepMind哈萨比斯狂推的神经科学，入门需要看什么书？

https://mp.weixin.qq.com/s/TWdeHVCgEf54STvdA1QUPg

DeepMind哈萨比斯长文：伟大的AI离不开神经科学

https://mp.weixin.qq.com/s/8ibcyvyBLYArAMhQElqRzg

Cell研究揭示生物神经元强大新特性，是时候设计更复杂的神经网络了！

https://mp.weixin.qq.com/s/cb6JBlb11xW0Xw0RWI4vFA

浙大&川大提出脉冲版ResNet：继承ResNet优势，实现当前最佳

# 问答系统

GA-Reader

Match-LSTM

Bi-DAF

R-Net

QA-Net

S-Net

R3

## 参考

https://mp.weixin.qq.com/s/IahvlkiACOAjicX68teA0A

套路深深深几许？管窥问答系统、阅读理解的小江湖

https://mp.weixin.qq.com/s/h51gh-9LISEBdJx8X--Lsg

近期有哪些值得读的QA论文？

https://mp.weixin.qq.com/s/lB44D_RFBqbNB6YX5gEULg

近期值得读的QA论文！专题论文解读

http://www.taodocs.com/p-4795349.html

基于大规模问答语料的问题检索系统

https://wenku.baidu.com/view/d38ab1e8856a561252d36fab.html

短文本相似度计算在用户交互式问答系统中的应用

https://mp.weixin.qq.com/s/5ajhhc4mpIhoqkl5VkGnaw

深度学习实现问答机器人

https://mp.weixin.qq.com/s/2aKoOx18RB0GGQTzb3Tjzg

Facebook开源DrQA的PyTorch实现：基于维基百科的问答系统

https://mp.weixin.qq.com/s/4TC7180LmD7V6u1CdVYRFw

漫谈机器阅读理解之Facebook提出的DrQA系统

https://mp.weixin.qq.com/s/SYUq1Qd-IOmVjgs7Kp4OAg

如何打造智能问答引擎

https://mp.weixin.qq.com/s/Mzycy0chQUNWjJYdpERBOw

一文详解维基百科的开放性问答系统

https://www.cnblogs.com/combfish/p/6708667.html

(QA-LSTM)自然语言处理：智能问答IBM保险QA QA-LSTM实现笔记

https://mp.weixin.qq.com/s/BhDy55Mj5oMArO0MKwvnKw

基于异构社交网络学习的社区问答方法，同时建模问题、回答和回答者

https://mp.weixin.qq.com/s/bZgis3dxMbv6AnLNyk04Og

用数据做酷的事！手把手教你搭建问答系统

https://mp.weixin.qq.com/s/nIMk-xl8Wzy1ANcT3ApAng

百度提出问答模型GNR：检索速度提高25倍

https://mp.weixin.qq.com/s/eDA6-BqPmjGBOPsOom0VIw

自动组合神经网络做问答系统！

https://mp.weixin.qq.com/s/vazeuiFvCC8aIhP2uAXoew

达观数据智能问答技术研究

https://mp.weixin.qq.com/s/eaxyhk93PbEFzHk-qiniOQ

ReQuest: 使用问答数据产生实体关系抽取的间接监督

https://mp.weixin.qq.com/s/1VYdCLw6q4rS91_YV4SclA

理解智能问答系统（Ⅰ）

https://mp.weixin.qq.com/s/x4yXjl0YXytAgp8a64ebXg

智能问答开源项目之YodaQA（Ⅱ）

https://mp.weixin.qq.com/s/2VPfk7jX0eBP2NZMB9igBg

智能问答之使用UIMA进行文本挖掘（Ⅲ）

https://mp.weixin.qq.com/s/NohUxQKHw8Z3kX_riGM10g

智能问答之答案抽取（Ⅳ）

https://mp.weixin.qq.com/s/EU0O-WR0ynI04NsPZ-bn2g

无从下手落地问答系统？实用百度开源框架了解一下

https://mp.weixin.qq.com/s/F1z7evNDqghEzSD-_Xbgfw

基于卷积深度相关性计算的社区问答方法，建模问题和回答的匹配关系

https://mp.weixin.qq.com/s/_klvAhH-K8tcYmamlr6FcA

Logistic Regression Models分析交互式问答

https://mp.weixin.qq.com/s/_XyqNPkInWfy-gUQtHjNIg

基于深度神经网络的自动问答系统概述

https://mp.weixin.qq.com/s/GyE9qdXPGvrq12dMAf4nrQ

论文推荐：QA，增强学习，知识图谱，机器阅读理解

https://mp.weixin.qq.com/s/6dKticG2I2zqlxnZ3W0ZgQ

“猜你所想，答你所问”，携程智能客服算法实践

https://mp.weixin.qq.com/s/yiAC6SddIFFo3bR351b_0Q

Embodied Question Answering

# CNN进阶

https://mp.weixin.qq.com/s/gwH9s1ggMTj2dJkad9wUuw

从VGG到NASNet，一文概览图像分类网络

https://mp.weixin.qq.com/s/hIAIbpqItS09KDOSFxaeqg

从Inception v1到Inception-ResNet，一文概览Inception家族的“奋斗史”

https://mp.weixin.qq.com/s/r143qYj8bziu_N-27RWRRw

机器学习5年大跃进，可能是个错觉

https://mp.weixin.qq.com/s/pollD4LN_GHJckzJAjwPqg

侧抑制”卷积神经网络，了解一下？

https://mp.weixin.qq.com/s/gil2K-JKzfRqdzc-abnh6A

王井东：深度融合——一种神经网络结构设计模式

https://mp.weixin.qq.com/s/7l0J5LIAawlkCIrUEGG9aA

卷积神经网络“失陷”，CoordConv来填坑

https://mp.weixin.qq.com/s/4GPLeJiQoDfBZJzF4QmuAg

Excel再现人脸识别：CNN用于计算机视觉任务不再神秘

https://mp.weixin.qq.com/s/30_AwwHS0eqfmYwTvRQ85Q

pooling去哪儿了？
