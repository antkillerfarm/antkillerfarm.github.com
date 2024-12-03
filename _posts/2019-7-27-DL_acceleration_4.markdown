---
layout: post
title:  深度加速（四）——知识蒸馏
category: DL acceleration 
---

* toc
{:toc}

# 模型压缩与加速（续）

## AutoML

由于模型压缩，本质上是一个精益求精的优化问题，因此采用AutoML技术对于各个超参数进行优化，就成为了一件很有必要的事情。

这里主要的问题在于超参数数量众多，导致状态空间过大。

论文：

《AMC: AutoML for Model Compression and Acceleration on Mobile Devices》

这是韩松组的何宜晖的作品。该论文采用深度强化学习的DDPG网络来优化目标网络，从而大大减少了需要搜索的状态空间。

## EfficientNet

**反思**：为什么有些模型FLOPs很低，以EfficientNet为代表，其推理速度却很慢。

https://zhuanlan.zhihu.com/p/122943688

FLOPs与模型推理速度

---

参考：

https://mp.weixin.qq.com/s/on1YdDexq5ICZL70mvikyw

谷歌大脑提出EfficientNet平衡模型扩展三个维度，取得精度-效率的最大化！

https://mp.weixin.qq.com/s/tCdG9gvpav1SvEzyAyBZXA

谷歌EfficientNet缩放模型，PyTorch实现出炉，登上GitHub热榜

https://mp.weixin.qq.com/s/NPM4E2gGOf3awQw7-_s6Uw

令人拍案叫绝的EfficientNet和EfficientDet

https://mp.weixin.qq.com/s/_eJ27nKULYzUNzDEf62x2w

何恺明团队最新力作RegNet：超越EfficientNet，GPU上提速5倍

https://mp.weixin.qq.com/s/WYG0XAhoPTOn_qCilT9yfw

​一文读懂EfficientDet

https://mp.weixin.qq.com/s/Jklyqt55-8NfKJ5oUXCmHw

EfficientDet框架详解

## 2:4 Sparsity

NV从硬件角度提出的稀疏化方案：

![](/images/img5/2_4_Sparsity.png)

对于一个R×C的矩阵，首先按行以4个元素为一组，每一组中按两个数保留，其余值置0的方式稀疏化成R×C/2的矩阵，为了保留原来的数值位置，根据原来的索引构造一个尺寸为R×C/2的2bit索引矩阵，以上图为例，索引矩阵的第一行的值为：`[[0, 3], [1, 2]]`。这种稀疏方式叫做Structured-sparse。

![](/images/img5/2_4_Sparsity_2.png)

这是稀疏化的权值进行GEMM运算的图示。

## 参考

https://github.com/memoiry/Awesome-model-compression-and-acceleration

模型压缩与加速相关资源汇总

https://mp.weixin.qq.com/s/bndECrtEcNCkCF5EG0wO-A

移动端机器学习资源合集

https://blog.csdn.net/hw5226349/article/details/84888416

Deep Compression/Acceleration：模型压缩加速论文汇总

https://zhuanlan.zhihu.com/p/58805980

深度学习的模型加速与模型裁剪方法

https://mp.weixin.qq.com/s/pAEoVs8xu0SY9tfBqOJHHA

Google DeepMind最新报告—深度神经网络压缩进展

http://blog.csdn.net/shuzfan/article/details/51383809

神经网络压缩：Deep Compression

https://mp.weixin.qq.com/s/2NOFyu_twx1EciDeDPBLKw

深度神经网络加速与压缩

https://mp.weixin.qq.com/s/lO2UM04PfSM5VJYh6vINhw

为模型减减肥：谈谈移动／嵌入式端的深度学习

https://mp.weixin.qq.com/s/cIGuJvYr4lZW01TdINBJnA

深度压缩网络：较大程度减少了网络参数存储问题

https://mp.weixin.qq.com/s/1JwLP0FmV1AGJ65iDgLWQw

神经网络模型压缩技术

http://mp.weixin.qq.com/s/iapih9Mme-VKCfsFCmO7hQ

简单聊聊压缩网络

https://mp.weixin.qq.com/s/dEdWz4bovmk65fwLknHBhg

韩松毕业论文：面向深度学习的高效方法与硬件

https://mp.weixin.qq.com/s/f1SCK0J5oTWNJvtld3UAHQ

神经网络修剪最新研究进展

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

上式实际上是Boltzmann distribution的PDF。（参见[《数学狂想曲（五）》](/math/2017/03/02/math_5.html#Boltzmann)）所以T也被称为温度（Temperature），通常默认1。对于分类任务来说使用$$T=1$$往往会导致不同类的概率差距很大，过度集中于某一个类，而其他类别的信息难以利用，这就是所谓的hard targets。

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

KD大概分为以下几个流派：

![](/images/img6/RB_KD.png)

Response-based KD侧重于学习teacher model最后一层的响应输出，如在两个模型的logit层建立蒸馏损失。

![](/images/img6/FB_KD.png)

feature-based侧重于学习teacher model中各个hint layer的feature map，注意力蒸馏就属于feature-based distillation。

![](/images/img6/R2B_KD.png)

relation-based更加强调teacher model中各个layer之间的关系或者teacher model对于data samples的关联性的响应。

---

自蒸馏(self knowledge distillation)是指不通过新增一个大模型的方式找到一个教师模型，同样可以提供有效增益信息给学生模型，这里的教师模型往往不会比学生模型复杂，但提供的增益信息对于学生模型是有效的增量信息，以提升学生模型效率。该方式可以避免使用更复杂的模型，也可以避免通过一些聚类或者是元计算的步骤生成伪标签。

![](/images/img6/Self_Distillation.png)

Self Distillation通过新增的深层子网络分类器作为teacher，对原网络的浅层部分进行蒸馏学习。

![](/images/img6/KDs.png)

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
