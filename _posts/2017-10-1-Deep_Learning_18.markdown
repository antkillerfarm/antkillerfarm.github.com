---
layout: post
title:  深度学习（十八）——无监督/半监督/自监督深度学习（1）
category: DL 
---

* toc
{:toc}

# 自动求导（续）

## 实现方式

虽然自动微分的数学原理已经明确，包括正向和反向的数学逻辑和模式。但具体的实现方法则可以有很大的差异。

目前主要有三类方案：

**基本表达式**：基本表达式或者称元素库（Elemental Libraries），基于元素库中封装一系列基本的表达式（如：加减乘除等）及其对应的微分结果表达式，作为库函数。用户通过调用库函数构建需要被微分的程序。

**操作符重载**：操作符重载或者称运算重载（Operator Overloading，OO），利用现代语言的多态特性（例如C++/JAVA/Python等高级语言），使用操作符重载对语言中基本运算表达式的微分规则进行封装。

**源代码变换**（Source Code Transformation，SCT）：源代码变换或者叫做源码转换（Source Code Transformation，SCT）则是通过对语言预处理器、编译器或解释器的扩展，将其中程序表达（如：源码、AST抽象语法树 或 编译过程中的中间表达 IR）的基本表达式微分规则进行预定义，再对程序表达进行分析得到基本表达式的组合关系，最后使用链式法则对上述基本表达式的微分结果进行组合生成对应微分结果的新程序表达，完成自动微分。

## 不可导函数的求导

不可导函数的求导，一般采用泰勒展开的方式。典型的算法有PGD（Proximal Gradient Descent）。

参考：

https://blog.csdn.net/bingecuilab/article/details/50628634

Proximal Gradient Descent for L1 Regularization

## Tape

无论前向还是后向求导，都需要记录前向传播的所有计算过程以及中间结果。这种记录和调用的过程类似于磁带的读写，是一种顺序访问，而非随机访问，故名为Tape方法。

针对动态图的自动求导，TF提出了GradientTape方法。

https://zhuanlan.zhihu.com/p/102207302

tensorflow计算图与自动求导——tf.GradientTape

## 其他

Jacobian-vector product function,JVP

vector-Jacobian product function,VJP

Hessian Vector Product,HVP

https://blog.csdn.net/apache/article/details/113925886

Pytorch,Tensorflow Autograd/AutoDiff nutshells: Jacobian,Gradient,Hessian,JVP,VJP,etc

## 参考

https://mp.weixin.qq.com/s/7Z2tDhSle-9MOslYEUpq6g

从概念到实践，我们该如何构建自动微分库

https://mp.weixin.qq.com/s/bigKoR3IX_Jvo-re9UjqUA

机器学习之——自动求导

https://www.jianshu.com/p/4c2032c685dc

自动求导框架综述

http://txshi-mt.com/2018/10/04/NMT-Tutorial-3b-Autodiff/

自动微分

https://mp.weixin.qq.com/s/WiZ00mkEB7CND3VyIM5Swg

最新《自动微分手册》77页pdf

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
