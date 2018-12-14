---
layout: post
title:  深度学习（三十二）——词向量进阶, NN的量化计算
category: DL 
---

# 词向量进阶

## All is Embedding

向量化是机器学习处理非数值数据的必经之路。因此除了词向量之外，还有其他的Embedding。比如Network Embedding。

参考：

https://mp.weixin.qq.com/s/wcFlZPbB5dl6C87kdfjmKw

NE(Network Embedding)论文小览

https://mp.weixin.qq.com/s/rWn6Bi5T3JfucD4C0jYSBA

Network Embedding指南

https://mp.weixin.qq.com/s/luSTZVzaK6fPw4veIDuMwg

网络表示学习综述：一文理解Network Embedding

https://mp.weixin.qq.com/s/HjzZJIA77gUlkG4doeT3uQ

网络表示学习介绍

https://zhuanlan.zhihu.com/p/36788547

网络表示学习论文引介

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

https://mp.weixin.qq.com/s/4ZlVisY08XvisjbK5zSheg

NLP中“词袋”模型和词嵌入模型的比较

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

https://blog.csdn.net/malefactor/article/details/50767711

利用Word Embedding自动生成语义相近句子

https://mp.weixin.qq.com/s/4sqMV3tS146VNrTyyncYnQ

中英文词向量评测理论与实践

https://mp.weixin.qq.com/s/p7sCt5Xj1U5vIZftRXBvzw

cw2vec理论及其实现

https://mp.weixin.qq.com/s/df-k5kJcJXhSWbFMu2mtCA

当前最好的词句嵌入技术概览：从无监督学习转向监督、多任务学习

https://mp.weixin.qq.com/s/nmuLM38Q_0MVN_6EtQ_7fQ

艾伦人工智能研究所提出新型深度语境化词表征

https://mp.weixin.qq.com/s/rYvbV4ssmXgblOg20KQVyQ

文本嵌入的经典模型与最新进展

https://mp.weixin.qq.com/s/lpQWcGg7uRBAorj_5QyunA

使用三重损失网络学习位置嵌入：让位置数据也能进行算术运算

https://mp.weixin.qq.com/s/BHA-tFCQjvhf1Tj53SBEaw

基线系统需要受到更多关注：基于词向量的简单模型

https://mp.weixin.qq.com/s/WQlSghxG89JCroNZSmop8w

朱军：关于图的表达学习

https://mp.weixin.qq.com/s/x_4k1ICo0GUFJzo4jRSoIQ

中文词向量论文综述（一）

https://mp.weixin.qq.com/s/79kTL79JIpB-YxbjSoQ-Xw

复旦大学黄萱菁教授带你了解自然语言理解中的表示学习

https://mp.weixin.qq.com/s/WMpcamrHjUDnYwqyISdooA

斯坦福Jure Leskovec图表示学习：无监督和有监督方法

https://mp.weixin.qq.com/s/0QyLIvH1McooGLc19hIAHA

预测你的下一步-动态网络节点表示学习

https://mp.weixin.qq.com/s/QW7t7iaIN1yaHYzOtwgYAQ

Representation Learning on Network网络表示学习

https://mp.weixin.qq.com/s/F2jF1vuzK4u8ZPrDK_CyLw

KDD2018 网络表示学习最新教程：DeepWalk作者Perozzi等人带你探索最前沿

https://mp.weixin.qq.com/s/KK-_G7Sc9HyYlxH1nLxVfA

斯坦福大学提出全新网络嵌入方法—GraphWave

https://zhuanlan.zhihu.com/p/44755895

"Social Rank":节点重要度在网络表示学习中的影响

https://mp.weixin.qq.com/s/2GPPC1LfpKLj80EJq7rcDA

如何帮助大家找工作？领英利用深度表征学习提升人才搜索和推荐系统

https://mp.weixin.qq.com/s/I2Gh3f_8cmfeOt0uEGj8hA

FAIR动态元嵌入:动态选择词嵌入模型

https://mp.weixin.qq.com/s/5ko0-W5giZ7wBCLUmkQeKA

神经网络词嵌入：如何将《战争与和平》表示成一个向量？

https://mp.weixin.qq.com/s/4gqpdTxt7Lip-hfuuUweAw

词嵌入获得的信息远比我们想象中的要多得多

https://mp.weixin.qq.com/s/-hh9iRIZtxh8Q-ZU_oCekA

网络嵌入之STNE model

https://mp.weixin.qq.com/s/iLaGGJXLwgI7qzcgkySlSQ

工程实践也能拿KDD最佳论文？解读Embeddings at Airbnb

# NN的量化计算

## 概述

NN的INT8计算是近来NN计算优化的方向之一。相比于传统的浮点计算，整数计算无疑速度更快，而NN由于自身特性，对单点计算的精确度要求不高，且损失的精度还可以通过retrain的方式恢复大部分，因此通常的科学计算的硬件（没错就是指的GPU）并不太适合NN运算，尤其是NN Inference。

>传统的GPU并不适合NN运算，因此Nvidia也好，还是其他GPU厂商也好，通常都在GPU中又集成了NN加速的硬件，因此虽然商品名还是叫做GPU，但是工作原理已经有别于传统的GPU了。

这方面的文章以Xilinx的白皮书较为经典：

https://china.xilinx.com/support/documentation/white_papers/c_wp486-deep-learning-int8.pdf

利用Xilinx器件的INT8优化开展深度学习

## INT量化

论文：

《On the efficient representation and execution of deep acoustic models》

![](/images/img2/INT8.png)

一个浮点数包括底数和指数两部分。将两者分开，就得到了一般的INT量化。

量化的过程一般如下：

1.使用一批样本进行推断，记录下每个layer的数值范围。

2.根据该范围进行量化。

量化的方法又分为两种：

1）直接使用浮点数表示中的指数。也就是所谓的fractional length，相当于2的整数幂。

2）使用更一般的scale来表示。这种方式的精度较高，但运算量稍大。

量化误差过大，一般可用以下方法减小：

1.按照每个channel的数值范围，分别量化。

2.分析weight、bias，找到异常值，并消除之。这些异常值通常是由于死去的神经元所导致的误差无法更新造成的。

如何确定每个layer的数值范围，实际上也有多种方法：

1.取整批样本在该layer的数值范围的并集，也就是所有最大（小）值的极值。

2.取所有最大（小）值的平均值。

## UINT量化

论文：

《Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference》

![](/images/img2/INT8_2.png)

UINT量化使用bias将数据搬移到均值为0的区间。

这篇论文的另一个贡献在于：原先的INT8量化是针对已经训练好的模型。而现在还可以在训练的时候就进行量化——前向计算进行量化，而反向的误差修正不做量化。

## Saturate Quantization

上述各种量化方法都是在保证数值表示范围的情况下，尽可能提高fl或者scale。这种方法也叫做Non-saturation Quantization。

NVIDIA在如下文章中提出了一种新方法：

http://on-demand.gputechconf.com/gtc/2017/presentation/s7310-8-bit-inference-with-tensorrt.pdf

8-bit Inference with TensorRT

![](/images/img2/INT8_3.png)

Saturate Quantization的做法是：将超出上限或下限的值，设置为上限值或下限值。

如何设置合理的Saturate threshold呢？

可以设置一组门限，然后计算每个门限的分布和原分布的相似度，即KL散度。然后选择最相似分布的门限即可。

## NN硬件的指标术语

MACC：multiply-accumulate，乘法累加。

FLOPS：Floating-point Operations Per Second，每秒所执行的浮点运算次数。

显然NN的INT8计算主要以MACC为单位。

## gemmlowp

gemmlowp是Google提出的一个支持低精度数据的GEMM（General Matrix Multiply）库。

代码：

https://github.com/google/gemmlowp

## 论文

《Quantizing deep convolutional networks for efficient inference: A whitepaper》

## 参考

https://www.chiphell.com/thread-1620755-1-1.html

新Titan X的INT8计算到底是什么鬼

https://mp.weixin.qq.com/s/S9VcoS_59nbZWe_P3ye2Tw

减少模型半数内存用量：百度&英伟达提出混合精度训练法

https://zhuanlan.zhihu.com/p/35700882

CNN量化技术

https://mp.weixin.qq.com/s/9DXMqiPIK5P5wzUMT7_Vfw

基于交替方向法的循环神经网络多比特量化

https://mp.weixin.qq.com/s/PDeChj1hQqUrZiepxXODJg

ICLR oral：清华提出离散化架构WAGE，神经网络训练推理合二为一

http://blog.csdn.net/tangwei2014/article/details/55077172

二值化神经网络介绍

https://mp.weixin.qq.com/s/oumf8l28ijYLxc9fge0FMQ

嵌入式深度学习之神经网络二值化（1）

https://mp.weixin.qq.com/s/tbRj5Wd69n9gvSzW4oKStg

嵌入式深度学习之神经网络二值化（2）

https://mp.weixin.qq.com/s/RsZCTqCKwpnjATUFC8da7g

嵌入式深度学习之神经网络二值化（3）

https://blog.csdn.net/stdcoutzyx/article/details/50926174

二值神经网络（Binary Neural Network，BNN）

https://mp.weixin.qq.com/s/lVja7woyFWpmr9sH0CitAA

BMXNet：基于MXNet的开源二值神经网络实现

https://mp.weixin.qq.com/s/naDk0mmxd08dNl9LawLUnw

不使用先验知识与复杂训练策略，从头训练二值神经网络！

https://mp.weixin.qq.com/s/tbRj5Wd69n9gvSzW4oKStg

异或神经网络

https://mp.weixin.qq.com/s/KgM1k1bziLTCec67hQ8hlQ

超全总结：神经网络加速之量化模型

https://mp.weixin.qq.com/s/7dzQhgblEm-kzRnpddweSw

嵌入式端CNN网络计算的量化-动态定点法（1）

https://mp.weixin.qq.com/s/XzLJzfvpP93cDYplf6-LXA

港科腾讯等提出Bi-Real net：超XNOR-net 10%的ImageNet分类精度
