---
layout: post
title:  深度学习（九）——深度强化学习, CTC
category: theory 
---

# 依存分析（续）

## NLP的女学霸们

http://cs.stanford.edu/people/danqi/

陈丹琦，清华本科（姚班）（2012）+斯坦福博士生。

https://homes.cs.washington.edu/~luheng/

何律恒，上海交大本科（2010）+宾夕法尼亚大学硕士（2012）+华盛顿大学博士生。

## CNN在NLP中的应用

虽然基本上，CV界是CNN的天下，NLP界是RNN的地盘。然而，两者的界限并不是泾渭分明的。比如下图就是一个CNN在NLP中的应用示例：

![](/images/article/Convolutional_Sequence_to_Sequence_Learning.png)

## TWE

http://nlp.csai.tsinghua.edu.cn/~lzy/publications/aaai2015_twe.pdf

Topical Word Embeddings

TWE的代码：

https://github.com/largelymfs/topical_word_embeddings

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

# CTC

Connectionist Temporal Classification，是一种改进的RNN模型。它主要解决的是时序模型中，输入数大于输出数，输入输出如何对齐的问题。

参考：

https://www.zhihu.com/question/47642307

语音识别中的CTC方法的基本原理

https://www.zhihu.com/question/20398418

语音识别的技术原理是什么？

https://www.zhihu.com/question/55851184

基于CTC等端到端语音识别方法的出现是否标志着统治数年的HMM方法终结？

https://www.zhihu.com/question/46829056

语音识别领域的最新进展目前是什么样的水准？

https://zhuanlan.zhihu.com/p/27064536

用Wavenet做中文语音识别

https://www.zhihu.com/question/29168274

语音识别中，如何理解HMM是一个生成模型，而DNN是一个判别模型呢？

https://zhuanlan.zhihu.com/p/24979135

从声学模型算法总结2016年语音识别的重大进步

https://mp.weixin.qq.com/s/LsVhMaHrh8JgfpDra6KSPw

横向对比5大开源语音识别工具包

https://mp.weixin.qq.com/s/-NTQG7_-GqGQWrRhiGgAQQ

详述DeepMind wavenet原理及其TensorFlow实现

https://mp.weixin.qq.com/s/bFjXDQlxRbt1ia-DSfYazw

SampleRNN语音合成模型

https://zhuanlan.zhihu.com/p/21344595

端到端的OCR：验证码识别(LSTM+CTC)

https://mp.weixin.qq.com/s/zEqgDh6_fnDgXEI8MC9cmg

端对端的深度卷积神经网络在语音识别中的应用

