---
layout: post
title:  深度学习（三十四）——深度强化学习（2）, 模型压缩
category: DL 
---

# 深度强化学习

## 参考

https://mp.weixin.qq.com/s/xr-2cNoSYpCftLI3dV6zEw

如何使用深度强化学习帮助自动驾驶汽车通过交叉路口？

https://www.zhihu.com/question/49230922

强化学习（reinforcement learning)有什么好的开源项目、网站、文章推荐一下？

https://mp.weixin.qq.com/s/R_pfTXDMaLHmiCaSV2t_YA

英特尔Nervana发布强化学习库Coach：支持多种价值与策略优化算法

https://mp.weixin.qq.com/s/AyW7oOC7yxVtmswaMT1DGQ

腾讯AI Lab获得计算机视觉权威赛事MSCOCO Captions冠军

https://mp.weixin.qq.com/s/4aENmxUMEEPVPnexLKrg7Q

新型强化学习算法ACKTR

https://mp.weixin.qq.com/s/5PzTiPoXPC1gH3xszzT2dQ

邓力等人提出BBQ网络：将深度强化学习用于对话系统

https://mp.weixin.qq.com/s/pM8oykHmtu5O5jYJBZjO_w

伯克利研究人员使用内在激励，教AI学会好奇

https://mp.weixin.qq.com/s/3WI3QgfHXcrCPbvmHWOEkg

强化学习在生成对抗网络文本生成中的作用

https://mp.weixin.qq.com/s/IvR0O6dpz2GJCG7UQb5kUQ

清华大学冯珺：基于强化学习的关系抽取和文本分类

https://mp.weixin.qq.com/s/SqU74jYBrjtp9L-bnBuboA

教你完美实现深度强化学习算法DQN

https://zhuanlan.zhihu.com/p/31579144

让我们从零开始做一个机械手臂(强化学习)

https://mp.weixin.qq.com/s/FiR_GRYqJYpJRO-2p44-Cg

伯克利强化学习新研究：机器人只用几分钟随机数据就能学会轨迹跟踪

https://mp.weixin.qq.com/s/u49cuDV21ITs1aV9tJR85g

Pieter Abbeel：《深度学习在机器人中的应用》

https://mp.weixin.qq.com/s/_dHjZQ_7_7H34PHhV_lC3w

全新强化学习算法详解，看贝叶斯神经网络如何进行策略搜索

https://mp.weixin.qq.com/s/RH4ifA46njdC7fyRI9kVMg

深度Q网络与视觉格斗类游戏

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

# 模型压缩

对于AI应用端而言，由于设备普遍没有模型训练端的性能那么给力，因此如何压缩模型，节省计算的时间和空间就成为一个重要的课题。

此外，对于一些较大的模型（如VGG），即使机器再给力，单位时间内能处理的图像数量，往往也无法达到实际应用的要求。这点在自动驾驶和视频处理领域显得尤为突出。

这里首先提到的是韩松的两篇论文：

《Deep Compression: Compressing Deep Neural Networks with Pruning, Trained Quantization and Huffman Coding》

《Learning both Weights and Connections for Efficient Neural Networks》

>韩松，清华本科（2012）+Stanford博士（2017）。MIT AP（from 2018）。   
>个人主页：   
>https://stanford.edu/~songhan/

韩松也是SqueezeNet的二作。

![](/images/article/nn_compression.png)

韩松论文的中心思想如上图所示。简单来说，就是去掉原有模型的一些不重要的参数、结点和层。

参数的选择，相对比较简单。参数的绝对值越接近零，它对结果的贡献就越小。这一点和稀疏矩阵有些类似。

结点和层的选择，相对麻烦一些，需要通过算法得到不重要的层。

比如可以逐个将每一层50%的参数置零，查看模型性能。对性能影响不大的层就是不重要的。

虽然这些参数、结点和层相对不重要，但是去掉之后，仍然会对准确度有所影响。这时可以对精简之后的模型，用训练样本进行re-train，通过残差对模型进行一定程度的修正，以提高准确度。

其次还可以看看图森科技的论文：

https://www.zhihu.com/question/62068158

如何评价图森科技连发的三篇关于深度模型压缩的文章？

图森的思路比较有意思。其中的方法之一，是利用L1规则化会导致结果的稀疏化的特性，制造出一批接近0的参数。从而达到去除不重要的参数的目的。

除此之外，矩阵量化、Kronecker内积、霍夫曼编码、模型剪枝等也是常见的模型压缩方法。

当然最系统的做法还属Geoffrey Hinton的论文：

《Distilling the Knowledge in a Neural Network》

图森科技的后两篇论文也是在Hinton论文的基础上改进的。

论文：

《Articulatory and Spectrum Features Integration using Generalized Distillation Framework》

参考：

https://zhuanlan.zhihu.com/p/24337627

深度压缩之蒸馏模型

http://blog.csdn.net/shuzfan/article/details/51383809

神经网络压缩：Deep Compression

https://zhuanlan.zhihu.com/p/24894102

《Distilling the Knowledge in a Neural Network》阅读笔记

https://luofanghao.github.io/2016/07/20/%E8%AE%BA%E6%96%87%E7%AC%94%E8%AE%B0%20%E3%80%8ADistilling%20the%20Knowledge%20in%20a%20Neural%20Network%E3%80%8B/

论文笔记 《Distilling the Knowledge in a Neural Network》

http://blog.csdn.net/zhongshaoyy/article/details/53582048

蒸馏神经网络

https://www.zhihu.com/question/50519680

如何理解soft target这一做法？

https://mp.weixin.qq.com/s/0KlnQ8UUxpyhBRdeo0EOAA

用于网络压缩的滤波器级别剪枝算法ThiNet

https://mp.weixin.qq.com/s/lO2UM04PfSM5VJYh6vINhw

为模型减减肥：谈谈移动／嵌入式端的深度学习

https://mp.weixin.qq.com/s/cIGuJvYr4lZW01TdINBJnA

深度压缩网络：较大程度减少了网络参数存储问题

https://mp.weixin.qq.com/s/1JwLP0FmV1AGJ65iDgLWQw

神经网络模型压缩技术

https://mp.weixin.qq.com/s/Xqc4UgcfCUWYOeGhjNpidA

CNN模型压缩与加速算法综述

https://mp.weixin.qq.com/s/rzv8VCAxBQi0HsUcnLqqUA

处理移动端传感器时序数据的深度学习框架：DeepSense

https://mp.weixin.qq.com/s/UYk3YQmFW7-44RUojUqfGg

上交大ICCV：精度保证下的新型深度网络压缩框架

https://mp.weixin.qq.com/s/ZuEi32ZBSjruvtyUimBgxQ

揭秘支付宝中的深度学习引擎：xNN

http://mp.weixin.qq.com/s/iapih9Mme-VKCfsFCmO7hQ

简单聊聊压缩网络

https://mp.weixin.qq.com/s/3qstz-KoRuxwpmfE4XDI-Q

面向卷积神经网络的卷积核冗余消除策略

https://mp.weixin.qq.com/s/dEdWz4bovmk65fwLknHBhg

韩松毕业论文：面向深度学习的高效方法与硬件

https://mp.weixin.qq.com/s/GFE2XYHZXPP0doQ5nd0JNQ

当前深度神经网络模型压缩和加速方法速览

https://mp.weixin.qq.com/s/Faej1LKqurtwEIreUVJ0cw

普林斯顿新算法自动生成高性能神经网络，同时超高效压缩

https://mp.weixin.qq.com/s/uK-HasmiavM3jv6hNRY11A

深度梯度压缩：降低分布式训练的通信带宽

https://mp.weixin.qq.com/s/_MDbbGzDOGHk5TBgbu_-oA

中大商汤等提出深度网络加速新方法，具有强大兼容能力

https://mp.weixin.qq.com/s/gbOmpP7XO1Hz_ld4iSEsrw

三星提出移动端神经网络模型加速框架DeepRebirth

https://mp.weixin.qq.com/s/rTFLiZ7DCo6vzD5O64UnMQ

阿里提出新神经网络算法，压缩掉最后一个比特

https://mp.weixin.qq.com/s/f1SCK0J5oTWNJvtld3UAHQ

神经网络修剪最新研究进展

https://mp.weixin.qq.com/s/3oL0Bso3mwbsfaG8X5-xoA

英特尔提出新型压缩技术DeepThin，适合移动端设备深度神经网络



