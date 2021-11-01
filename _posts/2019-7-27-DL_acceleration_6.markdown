---
layout: post
title:  深度加速（六）——硬件加速技巧
category: DL acceleration 
---

* toc
{:toc}

# 知识蒸馏

## 基本概念（续）

一个很大的DNN往往训练出来的效果会比较好，并且多个DNN一起ensemble的话效果会更好。但是实际应用中，过于庞大的DNN ensemble会增大计算量，从而影响应用。于是一个问题就被提出了：有没有一个方法，能使降低网络的规模，但是保持（一定程度上的）精确度呢？

Hinton举了一个仿生学的例子，就是昆虫在幼生期的时候往往都是一样的，适于它们从环境中摄取能量和营养；然而当它们成长到成熟期，会基于不同的环境或者身份，变成另外一种形态以适应这种环境。

那么对于DNN是不是存在类似的方法？在一开始training的过程中比较庞杂，但是当后来需要拿去deploy的时候，可以转换成一个更小的模型。他把这种方法叫做**Knowledge Distillation(KD)**。

![](/images/img2/Distilling.jpg)

上图是KD的网络结构图。它的主要思想就是通过一个performance非常好的大网络（有可能是ensemble的）来教一个小网络进行学习。这里我们可以把大网络叫为：teacher network，小网络叫为：student network。

teacher network可以被固定（正如在精炼过程中）或联合优化，甚至同时训练多个不同大小的student network。

![](/images/img2/Distilling_2.jpg)

上图是另一篇论文的图：

《Object detection at 200 Frames Per Second》

该论文的中文版：

https://mp.weixin.qq.com/s/OCG1TiHl2dsuS24uacQ-MA

又快又准确，新目标检测器速度可达每秒200帧

## soft target

由于有teacher network的存在，student network的训练也和普通的监督学习有所不同。

论文：

《Articulatory and Spectrum Features Integration using Generalized Distillation Framework》

![](/images/img3/KD.png)

上图是两者的训练过程。

这里解释一下，何为soft target？

Hinton给了个例子：比如说在MNIST数据集中，有两个数字“2”，但是写法是不一样的：一个可能写的比较像3（后面多出了一点头），一个写的比较像7（出的头特别的短）。在这样的情况下，grund truth label都是“2”，然而一个学习的很好的大网络会给label“3”和“7”都有一定的概率值。通常叫这种信息为“soft targets”；相对的，gt label是一种“hard targets”因为它是one－hot label。总的来说就是，通过大网络的“soft targets”，能得到更加多的信息来更好的训练小网络。

soft targets的计算方法如下：

$$q_i = \frac{exp(z_i/T)}{\sum_j exp(z_j / T)}$$

上式实际上是Boltzmann distribution的PDF。（参见[《数学狂想曲（五）》](/math/2017/03/02/math_5.html#Boltzmann)）所以T也被称为温度，通常默认1。对于分类任务来说使用$$T=1$$往往会导致不同类的概率差距很大，过度集中于某一个类，而其他类别的信息难以利用，这就是所谓的hard targets。

如果增大T的话，不同类别的差异就变小了，这也就是soft targets。类似于Label smoothing Regularization(LSR)。

如果T接近于0，则最大的值会越近1，其它值会接近0，近似于one-hot编码。

如果T等于无穷，那就是一个均匀分布。

综上，KD的流程就很自然了：

- 先训练一个teacher网络。

- 然后使用这个teacher网络的输出和数据的真实标签去训练student网络。

![](/images/img3/KD_2.png)

![](/images/img3/KD_3.png)

参考：

https://www.zhihu.com/question/50519680

如何理解soft target这一做法？

https://mp.weixin.qq.com/s/XR8OlGv5Ciglq03Ul_jpvQ

谈谈我眼中的Label Smooth

## KD的进化史

![](/images/img3/KD.jpg)

## Theseus压缩

研究者受到著名哲学思想实验“忒修斯之船”（The Ship of Theseus）启发（如果船上的木头逐渐被替换，直到所有的木头都不是原来的木头，那这艘船还是原来的那艘船吗？），提出了Theseus Compression for BERT(BERT-of-Theseus)，该方法逐步将BERT的原始模块替换成参数更少的替代模块。研究者将原始模型叫做“前辈”（predecessor），将压缩后的模型叫做“接替者“（successor），分别对应KD中的教师和学生。

参考：

https://mp.weixin.qq.com/s/HdG3_CaSdZP3lCp8J_VRQA

只需一个损失函数、一个超参数即可压缩BERT，MSRA提出模型压缩新方法

## 参考

https://github.com/dkozlov/awesome-knowledge-distillation

知识蒸馏从入门到精通

https://zhuanlan.zhihu.com/p/24894102

《Distilling the Knowledge in a Neural Network》阅读笔记

https://luofanghao.github.io/blog/2016/07/20/%E8%AE%BA%E6%96%87%E7%AC%94%E8%AE%B0%20%E3%80%8ADistilling%20the%20Knowledge%20in%20a%20Neural%20Network%E3%80%8B/

论文笔记《Distilling the Knowledge in a Neural Network》

http://blog.csdn.net/zhongshaoyy/article/details/53582048

蒸馏神经网络

https://mp.weixin.qq.com/s/QZ7PGvi27LiDOJaxici7Pw

数据蒸馏Dataset Distillation

https://mp.weixin.qq.com/s/vGTqHif48O2GZhuxWFhOLw

知识蒸馏总结、应用与扩展（2015-2019）

https://zhuanlan.zhihu.com/p/24337627

深度压缩之蒸馏模型

https://mp.weixin.qq.com/s/xcd9CHgE2_vEXrQ4MK019Q

知识蒸馏方法的演进历史综述

https://zhuanlan.zhihu.com/p/265906295

知识蒸馏：如何用一个神经网络训练另一个神经网络

https://mp.weixin.qq.com/s/qE1makMUIaFNrWk4nqOxDw

最新《知识蒸馏》2020综述论文，30页pdf，悉尼大学

https://zhuanlan.zhihu.com/p/51563760

知识蒸馏（Knowledge Distillation）最新进展（一）

https://zhuanlan.zhihu.com/p/53864403

知识蒸馏（Knowledge Distillation）最新进展（二）

https://zhuanlan.zhihu.com/p/81467832

知识蒸馏（Knowledge Distillation）简述（一）

https://mp.weixin.qq.com/s/pXoENwz4Z-eok9y3P9rQvg

知识蒸馏（Knowledge Distillation）简述（二）

https://zhuanlan.zhihu.com/p/102038521

知识蒸馏(Knowledge Distillation) 经典之作

https://zhuanlan.zhihu.com/p/92166184

知识蒸馏简述（一）

https://zhuanlan.zhihu.com/p/92269636

知识蒸馏简述（二）

http://coderskychen.cn/2019/02/23/distilling/

知识蒸馏三部曲：从模型蒸馏、数据蒸馏到任务蒸馏

https://mp.weixin.qq.com/s/5_qgj33tyVTHivpXkU4LDw

一个知识蒸馏的简单介绍

https://zhuanlan.zhihu.com/p/93287223

从入门到放弃：深度学习中的模型蒸馏技术

https://zhuanlan.zhihu.com/p/113549023

浅谈知识蒸馏方法研究综述

https://mp.weixin.qq.com/s/mFuxCl0Mzv5hmDFewWZkrw

FAIR&MIT提出知识蒸馏新方法：数据集蒸馏

https://mp.weixin.qq.com/s/MDgqSwVEClNqNpaWuGTEpg

微软亚研院提出用于语义分割的结构化知识蒸馏

https://blog.csdn.net/xbinworld/article/details/83063726

知识蒸馏（Distilling the Knowledge in a Neural Network），在线蒸馏

https://mp.weixin.qq.com/s/ekKg46bQlWrlg9Hon01M5g

Hinton胶囊网络后最新研究：用“在线蒸馏”训练大规模分布式神经网络

https://mp.weixin.qq.com/s/SqxooZqSeD3wA4EFK5D3Kg

再生神经网络：利用知识蒸馏收敛到更优的模型

https://mp.weixin.qq.com/s/Nkxy0SUdbwIjp5swU6tS9g

深度互学习-Deep Mutual Learning：三人行必有我师

https://mp.weixin.qq.com/s/I08kMmUohWWbYVpPDgTJsw

Startdt AI提出：使用生成对抗网络用于One-Stage目标检测的知识蒸馏方法

https://mp.weixin.qq.com/s/yFyM5OVp1YLKQBlgXeAzhw

华为诺亚方舟实验室提出无需数据网络压缩技术

https://mp.weixin.qq.com/s/0f0aToVaAsU7yWK4xz-HzQ

剪枝量化初完结，蒸馏学习又上线

https://github.com/patrickwaters1000/DistillingNeuralNets

Implements the technique of distillation

https://mp.weixin.qq.com/s/ckn4RERri-mfqLVPDRHGog

让学生网络相互学习，为什么深度相互学习优于传统蒸馏模型？

https://mp.weixin.qq.com/s/wwtsqjjUGt7MTEWDc5bSvQ

一种无需原始训练数据的Teacher-Student模型压缩方法

https://mp.weixin.qq.com/s/9dHRO80mMTGdRHaa0AdihQ

无需数据集的Student Networks

https://mp.weixin.qq.com/s/fQAkNdNhwkFichSZCwnNqA

北大、华为联合提出无需数据集的Student Networks

https://mp.weixin.qq.com/s/fA5NWLvLQN6kbB563pJnKg

从16.6%到74.2%，谷歌新模型刷新ImageNet纪录（Noisy Student）

https://mp.weixin.qq.com/s/UPm02RtTwhQhP_YhtmheBg

面向视觉智能的知识蒸馏和Student-Teacher方法，附37页pdf下载

https://zhuanlan.zhihu.com/p/143155437

知识蒸馏在推荐系统的应用

https://mp.weixin.qq.com/s/OFCzl8stFU5b1MWrkDU7NA

阿里电商推荐中如何进行特征蒸馏提升模型效果

https://mp.weixin.qq.com/s/ZNjC30F28uX2lBkHBAAU3g

双DNN排序模型：在线知识蒸馏在爱奇艺推荐的实践

https://mp.weixin.qq.com/s/_Wq7qawac1nTfZnV_AKG6w

模型压缩中知识蒸馏技术原理及其发展现状和展望

https://mp.weixin.qq.com/s/W8mLxU48dgWBB4eEFnU2rQ

知识蒸馏经典解读

https://zhuanlan.zhihu.com/p/144982430

强化学习如何用于模型蒸馏？

https://zhuanlan.zhihu.com/p/144987182

模型蒸馏的核心技术点有哪些，如何对其进行长期深入学习

https://mp.weixin.qq.com/s/3zpri6pfVtp-3-5_004B1Q

优势特征蒸馏在淘宝推荐中的应用

https://zhuanlan.zhihu.com/p/163477538

知识蒸馏与推荐系统

https://mp.weixin.qq.com/s/QpOx58M7lUfkONt-3SP8yg

知识蒸馏与推荐系统

https://mp.weixin.qq.com/s/TJVMuaDVZIjwqzuw6gd8uA

无数据知识蒸馏

https://zhuanlan.zhihu.com/p/90049906

知识蒸馏是什么？一份入门随笔

https://mp.weixin.qq.com/s/rxwHFjl0FEPWEcfMcwXL8w

BERT蒸馏完全指南

https://mp.weixin.qq.com/s/xgCtgEMRZ1VgzRZWjYIjTQ

知乎搜索文本相关性与知识蒸馏

https://mp.weixin.qq.com/s/6K5FvjMIVer-_fXJazU20Q

深度学习中的3个秘密：集成，知识蒸馏和蒸馏

https://mp.weixin.qq.com/s/-Rzvx9RMg9uZK5NFDs6cNg

语义分割的结构知识蒸馏

https://zhuanlan.zhihu.com/p/160206075

Knowledge Distillation（知识蒸馏）Review--20篇paper回顾

https://mp.weixin.qq.com/s/E7-MF18Y-UeKx694kGFHzA

深度学习中的知识蒸馏技术（上）

https://mp.weixin.qq.com/s/Noac4YLIimr1HM2fln2bjg

深度学习中的知识蒸馏技术（下）

https://mp.weixin.qq.com/s/IkKig7I5_97y_siixEj72w

协同训练

# 硬件加速技巧

https://mp.weixin.qq.com/s/KPT4P5SQ4E4ofPdjhhjRvA

如何加速深度神经网络计算效率？看NVIDIA-ISSCC2021教程，附93页Slides与视频
