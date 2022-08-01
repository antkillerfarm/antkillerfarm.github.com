---
layout: post
title:  深度学习（十八）——无监督/半监督/自监督深度学习（1）
category: DL 
---

* toc
{:toc}

# 自动求导（续）

https://mp.weixin.qq.com/s/xXwbV46-kTobAMRwfKyk_w

自动求导--Deep Learning框架必备技术二三事

https://mp.weixin.qq.com/s/f0xFfA1inOVOdJnSZR4k6Q

自动微分技术

https://zhuanlan.zhihu.com/p/79801410

PyTorch的自动求导机制详细解析，PyTorch的核心魔法

https://zhuanlan.zhihu.com/p/29904755

Autograd:PyTorch中的梯度计算

https://zhuanlan.zhihu.com/p/69294347

PyTorch的Autograd

https://zhuanlan.zhihu.com/p/83172023

Pytorch autograd,backward详解

https://mp.weixin.qq.com/s/PELBuCvu-7KQ33XBtlYfYQ

深度学习中的微分

https://zhuanlan.zhihu.com/p/24709748

矩阵求导术（上）

https://zhuanlan.zhihu.com/p/24863977

矩阵求导术（下）

https://mp.weixin.qq.com/s/2hu6a0wScJedwk3a5aKbIw

自动微分到底是什么？这里有一份自我简述

https://zhuanlan.zhihu.com/p/347385418

AI框架基础技术之自动求导机制 (Autograd)

https://www.zhihu.com/question/497827630

Pytorch的自动微分机制是自动创建一个可以记录所有数据操作的计算图（有向无环图(DAG)）吗？

https://www.cnblogs.com/royhoo/p/Autodiff.html

自动微分（Autodiff）

# 无监督/半监督/自监督深度学习

自监督学习是一种特殊目的的无监督学习。不同于传统的AutoEncoder等方法，仅仅以重构输入为目的，而是希望通过surrogate task学习到和高层语义信息相关联的特征。

## 对比学习

![](/images/img4/SimCLR.jpg)

https://mp.weixin.qq.com/s/r1uXn2jGsHZcZ8Nk7GnGFA

语义表征的无监督对比学习：一个新理论框架

https://zhuanlan.zhihu.com/p/346686467

对比学习（Contrastive Learning）综述

https://mp.weixin.qq.com/s/SOaA9XNnymLgGgJ5JNSdBg

对比学习（Contrastive Learning）相关进展梳理

https://mp.weixin.qq.com/s/U0pTQkW55evm94iQORwGeA

图解SimCLR框架，用对比学习得到一个好的视觉预训练模型

https://mp.weixin.qq.com/s/1RJ4bbfDC5LiN2PNIxdzew

SimCLR框架的理解和代码实现以及代码讲解

https://mp.weixin.qq.com/s/-Vtl_8nND7WCPLdL5bNlMw

探索孪生神经网络：请停止你的梯度传递

https://zhuanlan.zhihu.com/p/321642265

《探索简单孪生网络表示学习》阅读笔记

https://mp.weixin.qq.com/s/6qqFAQBaOFuXtaeRSmQgsQ

一文梳理2020年大热的对比学习模型

https://mp.weixin.qq.com/s/SeAZERYdfqDbtqTLnuWfGg

盘点近期大热对比学习模型：MoCo/SimCLR/BYOL/SimSiam

https://mp.weixin.qq.com/s/jHVg-BMRRVNjAf6ZFEoPxQ

自监督学习的SimCLRv2框架

https://mp.weixin.qq.com/s/7iBC_n6EARW3V8bNuKUqQA

Hinton团队力作：SimCLR系列

https://mp.weixin.qq.com/s/sH-G4g0EyQLu2l91Xvdefw

Neighbor2Neighbor：无需干净图像的自监督图像降噪

https://mp.weixin.qq.com/s/xYlCAUIue_z14Or4oyaCCg

对比学习研究进展精要

https://mp.weixin.qq.com/s/VlSoMmAGDblQ2UYhLD96gA

什么是contrastive learning？

https://mp.weixin.qq.com/s/qnG0YLf0yjs4aT9URRMDyw

有监督对比学习的一个简单的例子

https://mp.weixin.qq.com/s/h8loG3enT5U-5F2a2UflJg

对比学习小综述

https://mp.weixin.qq.com/s/v5p9QA3vDl-WTF3-7shp4g

对比学习简述

https://mp.weixin.qq.com/s/CeqoXqHjfa6UTWa8mmo_Ww

Paper和陈丹琦撞车是一种怎样的体验（ConSERT vs. SimCSE）

https://mp.weixin.qq.com/s/C4KaIXO9Lp8tlqhS3b0VCw

美团提出基于对比学习的文本表示模型（ConSERT）

## Prompt Learning

在BERT和Word2Vec相关章节中，我们已经看到了，如何采用类似完形填空的方式，来利用大量的无标签语料，对模型进行预训练。这里的完形填空就是一种Prompt Learning。

更一般的对于输入的文本$$x$$，有函数$$f_{prompt}(x)$$，将$$x$$转化成prompt的形式$$x'$$，即：

$$x'=f_{prompt}(x)$$

该函数通常会进行两步操作：

- 使用一个模板，模板通常为一段自然语言，并且包含有两个空位置：用于填输入x的位置[X]和用于生成答案文本的位置[Z]。

- 把输入x填到的位置[X]。

例如，在文本情感分类的任务中，假设输入是：

" I love this movie."

使用的模板是

" [X] Overall, it was a [Z] movie."

那么得到的$$x'$$就应该是 "I love this movie. Overall it was a [Z] movie."

在实际的研究中，prompts应该有空位置来填充答案，这个位置一般在句中或者句末。如果在句中，一般称这种prompt为cloze prompt；如果在句末，一般称这种prompt为prefix prompt。[X]和[Z]的位置以及数量都可能对结果造成影响，因此可以根据需要灵活调整。

如何设计合适的[X]和[Z]，就是Prompt Learning的主要议题了。

参考：

https://mp.weixin.qq.com/s/dkNH4BLOH36B5h_UCcRLnA

NLP新宠——浅谈Prompt的前世今生

https://mp.weixin.qq.com/s/2eA4PBd-wr9tVyyuzJ66Bw

Fine-tune之后的NLP新范式：Prompt越来越火，CMU华人博士后出了篇综述文章

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

https://mp.weixin.qq.com/s/_3DqXBpZhstVv6BkBR4oag

自监督学习综述

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

https://mp.weixin.qq.com/s/kNTRpDbKQIalzJi_rx0noQ

无标签表示学习，222页ppt，DeepMind

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

https://mp.weixin.qq.com/s/qyQjKsgktWv9SihotaQX3w

从顶会看自监督学习最新研究进展

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
