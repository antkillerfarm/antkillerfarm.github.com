---
layout: post
title:  深度加速（六）——知识蒸馏
category: DL acceleration 
---

# 知识蒸馏

## 基本概念

知识蒸馏是另一大类的模型压缩方法。

这类方法的开山之作，当属Geoffrey Hinton和Jeff Dean的论文：

《Distilling the Knowledge in a Neural Network》

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

## KD的进化史

![](/images/img3/KD.jpg)

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

https://zhuanlan.zhihu.com/p/51563760

知识蒸馏（Knowledge Distillation）最新进展（一）

https://zhuanlan.zhihu.com/p/53864403

知识蒸馏（Knowledge Distillation）最新进展（二）

https://zhuanlan.zhihu.com/p/81467832

知识蒸馏（Knowledge Distillation）简述（一）

https://mp.weixin.qq.com/s/pXoENwz4Z-eok9y3P9rQvg

知识蒸馏（Knowledge Distillation）简述（二）

https://zhuanlan.zhihu.com/p/92166184

知识蒸馏简述（一）

https://zhuanlan.zhihu.com/p/92269636

知识蒸馏简述（二）

http://coderskychen.cn/2019/02/23/distilling/

知识蒸馏三部曲：从模型蒸馏、数据蒸馏到任务蒸馏

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
