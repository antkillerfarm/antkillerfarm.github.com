---
layout: post
title:  深度学习（四十）——模型压缩与加速, 多任务学习
category: DL 
---

# 模型压缩与加速

对于AI应用端而言，由于设备普遍没有模型训练端的性能那么给力，因此如何压缩模型，节省计算的时间和空间就成为一个重要的课题。

此外，对于一些较大的模型（如VGG），即使机器再给力，单位时间内能处理的图像数量，往往也无法达到实际应用的要求。这点在自动驾驶和视频处理领域显得尤为突出。

这里首先提到的是斯坦福的相关课程：

https://cs217.github.io/

CS 217: Hardware Accelerators for Machine Learning

其次是韩松的两篇论文：

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

还可以看看图森科技的论文：

https://www.zhihu.com/question/62068158

如何评价图森科技连发的三篇关于深度模型压缩的文章？

图森的思路比较有意思。其中的方法之一，是利用L1规则化会导致结果的稀疏化的特性，制造出一批接近0的参数。从而达到去除不重要的参数的目的。

除此之外，矩阵量化、Kronecker内积、霍夫曼编码、模型剪枝等也是常见的模型压缩方法。

当然最系统的做法还属Geoffrey Hinton的论文：

《Distilling the Knowledge in a Neural Network》

![](/images/img2/Distilling.jpeg)

老师网络可以被固定（正如在精炼过程中）或联合优化，甚至同时训练多个不同大小的学生网络。

![](/images/img2/Distilling_2.jpeg)

上图是另一篇论文的图：

《Object detection at 200 Frames Per Second》

该论文的中文版：

https://mp.weixin.qq.com/s/OCG1TiHl2dsuS24uacQ-MA

又快又准确，新目标检测器速度可达每秒200帧

图森科技的后两篇论文也是在Hinton论文的基础上改进的。

论文：

《Articulatory and Spectrum Features Integration using Generalized Distillation Framework》

## 权值稀疏化实战

这里讲一下韩松论文提到的裁剪方法中，最简单的一种——“权值稀疏化“的工程实现细节。以darknet框架为例。

1.在src/parser.c中找到save_XXX_weights函数。判断权值是否接近0，如果是，则强制设为0。

2.使用修改后的weights进行re-train。训练好之后，重复第1、2步。

3.反复多次之后，进入最终prune阶段。修改src/network.c:update_network，令其不更新0权值。

## 参考

https://zhuanlan.zhihu.com/p/24337627

深度压缩之蒸馏模型

http://blog.csdn.net/shuzfan/article/details/51383809

神经网络压缩：Deep Compression

https://mp.weixin.qq.com/s/2NOFyu_twx1EciDeDPBLKw

深度神经网络加速与压缩

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

https://mp.weixin.qq.com/s/ekKg46bQlWrlg9Hon01M5g

Hinton胶囊网络后最新研究：用“在线蒸馏”训练大规模分布式神经网络

https://mp.weixin.qq.com/s/JnW7RnOQKG-dPOOAQeOmSA

当前深度神经网络模型压缩和加速都有哪些方法？

https://mp.weixin.qq.com/s/YUg2dZZhDSsRpSftdNfiIQ

极致的优化：智能手机是如何处理大型神经网络的

https://mp.weixin.qq.com/s/Ck_GDv1Xo-YMZcu-00gTOA

中星微夺冠国际人工智能算法竞赛，目标检测一步法精度速度双赢

https://mp.weixin.qq.com/s/qWJarPrjOrwxSX77xQ9rCw

面向卷积神经网络的卷积核冗余消除策略

https://mp.weixin.qq.com/s/QSGgvhkMUj3cXVlQwlzTFQ

深度神经网络加速和压缩新进展年度报告

https://zhuanlan.zhihu.com/p/37074222

CVPR 2018 高效小网络探密（上）

https://zhuanlan.zhihu.com/p/37919669

CVPR 2018 高效小网络探密（下）

https://mp.weixin.qq.com/s/SqxooZqSeD3wA4EFK5D3Kg

再生神经网络：利用知识蒸馏收敛到更优的模型

https://zhuanlan.zhihu.com/p/38046989

从ISCA论文看AI硬件加速的新技巧

https://mp.weixin.qq.com/s/jqRBrs9Y_-3qvemL0RTflA

支付宝如何优化移动端深度学习引擎？

https://mp.weixin.qq.com/s/-V6hlZAKp1vuARSibZDBQQ

深度学习高效计算与处理器设计

https://mp.weixin.qq.com/s/NJzGR-tY_WWeccbdshHckA

基于交错组卷积的高效深度神经网络

https://mp.weixin.qq.com/s/ccFccLb2UTyFyMwFPjsDaA

让CNN跑得更快，腾讯优图提出全局和动态过滤器剪枝

https://mp.weixin.qq.com/s/vswtn3D1-VZZlyKLJmHc7A

纪荣嵘：深度神经网络压缩及应用

https://mp.weixin.qq.com/s/cSYCT1I1asaSCIc5Hgu0Jw

计算成本降低35倍！谷歌发布手机端自动设计神经网络MnasNet

https://zhuanlan.zhihu.com/p/42474017

MnasNet：终端轻量化模型新思路

https://mp.weixin.qq.com/s/p_qdKcQwQ8y_JUw3gQUEnA

谷歌大脑用强化学习为移动设备量身定做最好最快的CNN模型

https://mp.weixin.qq.com/s/OyEIcS5o6kWUu2UzuWZi3g

这么Deep且又轻量的Network，实时目标检测

https://mp.weixin.qq.com/s/mWfZ4jfuby4myGfi6TW3wQ

从超参数到架构，一文简述模型优化策略

https://mp.weixin.qq.com/s/IxVMMu_7UL5zFsDCcYfzYA

AutoML自动模型压缩再升级，MIT韩松团队利用强化学习全面超越手工调参

https://mp.weixin.qq.com/s/fU-AeaPz-lHlg0CBgqnpZQ

轻量化神经网络综述

https://mp.weixin.qq.com/s/BMsvhXytSy2nWIsGOSOSBQ

自动生成高效DNN，适用于边缘设备的生成合成工具FermiNets

https://mp.weixin.qq.com/s/_JlaxwEYqdTTuS4hNSQTTw

这么Deep且又轻量的Network，实时目标检测

https://mp.weixin.qq.com/s/d6HFVbbHwkxPGdnbyVuMyQ

密歇根州立大学提出NestDNN：动态分配多任务资源的移动端深度学习框架

https://mp.weixin.qq.com/s/lUTusig94Htf7_4Z3X1fTQ

清华&伯克利ICLR论文：重新思考6大剪枝方法

https://mp.weixin.qq.com/s/g3y9mRhkFtzSuSMAornnDQ

韩松博士论文：面向深度学习的高效方法与硬件 

# 多任务学习

https://mp.weixin.qq.com/s/guAgXdhZSbEAkERSB1sLRA

多任务学习-Multitask Learning概述

https://mp.weixin.qq.com/s/A-CVKTz_moaFzTYywSt2gg

张宇 杨强：多任务学习概述

https://mp.weixin.qq.com/s/ZlCI02UdRuFBc-uKqIPE_w

深度学习多任务学习综述

https://mp.weixin.qq.com/s/QXOy2jo4RhCZrD5bSVzBOQ

共享相关任务表征，一文读懂深度神经网络多任务学习

https://mp.weixin.qq.com/s/mm9bXXTEzd8DwyYlMgGMZg

NLP多任务学习：一种层次增长的神经网络结构

https://mp.weixin.qq.com/s/X6FwTgr282hbqgOz3oBX-w

多任务学习概述论文：从定义和方法到应用和原理分析

https://blog.csdn.net/CoderPai/article/details/80080455

多任务学习与深度学习

https://blog.csdn.net/CoderPai/article/details/80087188

利用TensorFlow一步一步构建一个多任务学习模型

https://mp.weixin.qq.com/s/fcFb6WkJVP8TYpoxkQgiWQ

CMU提出“十字绣网络”，自动决定多任务学习的最佳共享层

https://mp.weixin.qq.com/s/i7WAFjQHK1NGVACR8x3v0A

自然语言十项全能：转化为问答的多任务学习

https://mp.weixin.qq.com/s/NpO1UP_mzyaeqW26xLY1Xg

CVPR 2018最佳论文作者亲笔解读：研究视觉任务关联性的Taskonomy

https://mp.weixin.qq.com/s/DUSa3SW1AgvJ0szL030NHQ

一个AI玩57个游戏，DeepMind离真正“万能”的AGI不远了！

https://mp.weixin.qq.com/s/P81I5vl99mV-4StNHmd_6A

作为多目标优化的多任务学习：寻找帕累托最优解

https://mp.weixin.qq.com/s/cyX_FzHF-6df9_AMUDu1aQ

One Model To Learn Them All

https://mp.weixin.qq.com/s/oY2yGrTmIpr3H2qvzYKInA

谷歌一个模型解决所有问题《One Model to Learn Them All》论文深度解读

https://mp.weixin.qq.com/s/VMHyEOn8uyRYt39QXVWUww

西湖大学张岳：自然语言处理中的多任务联合学习（384页PPT）
