---
layout: post
title:  深度学习（三十二）——词向量进阶, NN的INT8计算, 视频目标分割
category: DL 
---

# 词向量进阶

## All is Embedding

向量化是机器学习处理非数值数据的必经之路。因此除了词向量之外，还有其他的Embedding。比如Network Embedding。

参考：

https://mp.weixin.qq.com/s/wcFlZPbB5dl6C87kdfjmKw

NE(Network Embedding)论文小览

https://mp.weixin.qq.com/s/zTNX_LeVMeHhJG7kPewn2g

除了自然语言处理，你还可以用Word2Vec做什么？

https://mp.weixin.qq.com/s/7dsrvHp6KIvlE-VXiUH1Rw

HIN2Vec：异质信息网络中的表示学习

https://mp.weixin.qq.com/s/CqJ7o1-ptCBBocB3PfEuXg

万物向量化：用协作学习的方法生成更广泛的实体向量

https://mp.weixin.qq.com/s/UtfidoBCJ0Wjpnl_C1a7iw

浅谈向量化与Hash-Trick

https://mp.weixin.qq.com/s/0JmB0sMUVsiuwrVObN_10g

浙江大学提出设计网络嵌入算法的度惩罚原则，可有效保留无标度特性

## 参考

https://mp.weixin.qq.com/s/L6_BV-cWR4wge2ritmyqzA

让你上瘾的网易云音乐推荐算法，用Word2vec就可以实现

https://mp.weixin.qq.com/s/ktzqTUbqMeTLvc-pLJbUvA

蚂蚁金服公开最新基于笔画的中文词向量算法

https://mp.weixin.qq.com/s/GGXI-ZzPc9LLjVQpf5ia1A

词嵌入2017年进展全面梳理：趋势和未来方向

https://mp.weixin.qq.com/s/gI82_PHc94OAsZQBk6DwQw

一次搞定多种语言：Facebook展示全新多语言嵌入系统

https://mp.weixin.qq.com/s/diIzbc0tpCW4xhbIQu8mCw

阿里凑单算法首次公开！基于Graph Embedding的打包购商品挖掘系统解析

https://mp.weixin.qq.com/s/8uyIuO_A0NwusyJV-bKRug

个性化序列推荐：卷积序列嵌入方法

https://mp.weixin.qq.com/s/chiHw5gKnJyTJTQeF6gViw

在向量空间中启用网络分析和推理，清华大学崔鹏博士最新分享

https://mp.weixin.qq.com/s/oKwxWbCkH-xqYSJIBdb92A

2018超网络节点表示学习

https://mp.weixin.qq.com/s/kQlxLDHLI6xxFzwJVjFj7w

GraRep: 基于全局结构信息的图结点表示学习

https://mp.weixin.qq.com/s/vrdprH_8J80J3VHQqA-bKg

通过动态融合方式学习多模态词表示，中科院自动化所宗成庆老师团队最新工作

https://mp.weixin.qq.com/s/7Ujxc5_CwhYxXTLAjac0-g

基于异构网络节点表示的推荐系统

https://mp.weixin.qq.com/s/98k1fNHDIwITFyLnNutt4A

Entity Embeddings: 利用深度学习训练结构化数据的实体嵌入

# NN的INT8计算

## 概述

NN的INT8计算是近来NN计算优化的方向之一。相比于传统的浮点计算，整数计算无疑速度更快，而NN由于自身特性，对单点计算的精确度要求不高，且损失的精度还可以通过retrain的方式恢复大部分，因此通常的科学计算的硬件（没错就是指的GPU）并不太适合NN运算，尤其是NN Inference。

>传统的GPU并不适合NN运算，因此Nvidia也好，还是其他GPU厂商也好，通常都在GPU中又集成了NN加速的硬件，因此虽然商品名还是叫做GPU，但是工作原理已经有别于传统的GPU了。

这方面的文章以Xilinx的白皮书较为经典：

https://china.xilinx.com/support/documentation/white_papers/c_wp486-deep-learning-int8.pdf

利用Xilinx器件的INT8优化开展深度学习

论文：

《On the efficient representation and execution of deep acoustic models》

![](/images/img2/INT8.png)

《Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference》

![](/images/img2/INT8_2.png)

参考：

https://www.chiphell.com/thread-1620755-1-1.html

新Titan X的INT8计算到底是什么鬼

https://mp.weixin.qq.com/s/S9VcoS_59nbZWe_P3ye2Tw

减少模型半数内存用量：百度&英伟达提出混合精度训练法

## NN硬件的指标术语

MACC：multiply-accumulate，乘法累加。

FLOPS：Floating-point Operations Per Second，每秒所执行的浮点运算次数。

显然NN的INT8计算主要以MACC为单位。

# 视频目标分割

视频目标分割任务和语义分割有两个基本区别：

1.视频目标分割任务分割的是一般的、非语义的目标；

2.视频目标分割添加了一个时序模块：它的任务是在视频的每一连续帧中寻找感兴趣目标的对应像素。

![](/images/article/Segmentation.png)

上图是Segmentation的细分，其中的每一个叶子都有一个示例数据集。

基于视频任务的特性，我们可以将问题分成两个子类：

无监督（亦称作视频显著性检测）：寻找并分割视频中的主要目标。这意味着算法需要自行决定哪个物体才是“主要的”。

半监督：在输入中（只）给出视频第一帧的正确分割掩膜，然后在之后的每一连续帧中分割标注的目标。

参考：

http://mp.weixin.qq.com/s/pGrzmq5aGoLb2uiJRYAXVw

一文概览视频目标分割

https://www.zhihu.com/question/52185576

视频中的目标检测与图像中的目标检测具体有什么区别？

https://mp.weixin.qq.com/s/noljXreGfoMfiZb_n90R3w

模仿人类的印象机制，商汤提出精确实时的视频目标检测方法

http://mp.weixin.qq.com/s/-Av3-ZNi6UGlKNv_jduAeQ

微软新论文：如何利用深度特征流提高视频识别准确率？

https://mp.weixin.qq.com/s/z1APyCxlOEPHn48OeJAHkQ

基于深度学习的视频内容识别

https://mp.weixin.qq.com/s/WMakTEN68KPi7X9kMQetiw

OpenAI:3段视频演示无人驾驶目标检测强大的对抗性样本！

https://mp.weixin.qq.com/s/-kXWmaIfw67rvb8IRi5mCQ

视频行为理解新边界

https://mp.weixin.qq.com/s/j5YPHYEPioLiEIDc6lK3kA

在线视频衣物精确检索技术，开启刷剧败明星同款时代

https://mp.weixin.qq.com/s/CXKuSMi0Vd43BGDf5BgoqA

弱监督视频物体识别新方法：香港科技大学联合CMU提出TD-Graph LSTM

https://mp.weixin.qq.com/s/Ns8oD_HBpFoumMMDbMq8-g

DeepMind视频行为分类竞赛，百度IDL获第一

https://mp.weixin.qq.com/s/nZVIVJ9z0AWA8VFouNpafg

深度学习之视频摘要简述

https://mp.weixin.qq.com/s/7ccEaDRngVo42OSU6FBlVg

从视频到语句，优必选获TRECVID 2017子任务冠军

https://mp.weixin.qq.com/s/XorPkuIdhRNI1zGLwg-55A

斯坦福新深度学习系统 NoScope：视频对象检测快1000倍

https://mp.weixin.qq.com/s/t29ITfRPD3yCmRD5wJyq7g

ICCV2017 PoseTrack challenge优异方法：基于检测和跟踪的视频中人体姿态估计

https://mp.weixin.qq.com/s/7w5nYWfZO-YOJ4cA47kQXg

无监督视频物体分割新思路：实例嵌入迁移

https://mp.weixin.qq.com/s/PhMPa-e4sbzqWKmFzRZE4Q

实时替换视频背景：谷歌展示全新移动端分割技术


