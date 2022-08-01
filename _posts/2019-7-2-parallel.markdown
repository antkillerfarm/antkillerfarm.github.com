---
layout: post
title:  并行 & 框架 & 优化
category: DL acceleration 
---

* toc
{:toc}

# 并行 & 框架 & 优化

## 概述

![](/images/img4/Deep_Learning_System.png)

## 教程

https://hpc.llnl.gov/documentation/tutorials/introduction-parallel-computing-tutorial

Introduction to Parallel Computing Tutorial

https://www.cnblogs.com/Carrawayang/p/14691607.html

上文的中文版

http://www.cs.cmu.edu/~418/

15-418/15-618: Parallel Computer Architecture and Programming

## 论文

《Demystifying Parallel and Distributed Deep Learning: An In-Depth Concurrency Analysis》

《A Survey of Large-Scale Deep Learning Serving System Optimization: Challenges and Opportunities》

## blog

https://zhuanlan.zhihu.com/c_1174996853811335168

一个多核&并行的专栏

https://deeplearningsystems.ai

Deep Learning Systems: Algorithms, Compilers, and Processors for Large-Scale Production

## 并发和并行

并发(concurrency)是指能处理多个同时性活动的能力，并发事件之间不一定要同一时刻发生。

并行(parallelism)是指同时发生的两个并发事件，具有并发的含义，而并发则不一定并行。

来个比喻：并发和并行的区别就是一个人同时吃三个馒头和三个人同时吃三个馒头。

Erlang之父Joe Armstrong曾经以人们使用咖啡机的场景为例描述了这两个术语。

并发：如果多个队列可以交替使用某台咖啡机，则这一行为就是并发的。

并行：如果存在多台咖啡机可以被多个队列交替使用，则就是并行。

参考：

https://mp.weixin.qq.com/s/23QCWf0NOoXlwRGAHfx4oQ

还在疑惑并发和并行？

https://mp.weixin.qq.com/s/-kizIk3ZXqu7UNqAb3QlQw

C++并发编程（C++11到C++17）

## Distributed Data Parallel

https://mp.weixin.qq.com/s/52Wz4pUI8egKugMFuknWKw

Pytorch中的Distributed Data Parallel与混合精度训练（Apex）

https://mp.weixin.qq.com/s/x1Z4jkMvfo4mD-_rKqvjuw

在PyTorch中使用Distributed Data Parallel进行多GPU分布式模型训练

https://zhuanlan.zhihu.com/p/178402798

DDP系列第一篇：入门教程

https://zhuanlan.zhihu.com/p/187610959

DDP系列第二篇：实现原理与源代码解析

https://zhuanlan.zhihu.com/p/250471767

DDP系列第三篇：实战与技巧

# tf.distribute & MultiDevice

MirroredStrategy：单机多卡训练

MultiWorkerMirroredStrategy：多机训练

CentralStorageStrategy也执行同步训练，但是变量不会被镜像，而是放在CPU上。各操作(operation)在本地GPU之间复制进行。如果只有一个GPU，变量和操作都会放在GPU上。在对CPU上的变量进行更新前，该策略会先将所有 GPU副本的上的变量梯度进行聚合，然后应用到CPU变量更新中。

---

当数据量很大时，单个节点需要很长时间才能完成1轮训练，这对于动辄十几轮或几十轮的训练来说是不可忍受的，所以可以将数据进行划分并分配给多个节点，每个节点处理自己的一小部分数据，从而加快整体的训练速度。在TensorFlow中**数据并行**也被称为图间复制(Between-graph Replication)。

模型并行是指将模型切分为多个部分并将各个部分放置到不同的节点进行训练的分布式模式。因为当模型很复杂，参数很多时，由于内存的限制，单个节点无法将整个模型加载并进行训练，所以需要将模型切割为更小的部分，并将各个部分运行在不同的节点上以完成训练。在TensorFlow中**模型并行**也被称为图内复制 (In-graph Replication)。

![](/images/img4/Parallel.png)

![](/images/img4/Parallel_2.png)

---

tensorflow::ProcessFunctionLibraryRuntime::RunMultiDevice

https://www.cnblogs.com/rossiXYZ/p/16142677.html

TensorFlow之分布式变量（该作者写了一系列的TF分布式文章）

示例：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/xla/multi_device_lenet_xla.py

## Pathways

https://blog.csdn.net/OneFlow_Official/article/details/124054450

解读谷歌Pathways架构（一）：Single-controller与Multi-controller

https://blog.csdn.net/OneFlow_Official/article/details/124113864

解读谷歌Pathways架构（二）：向前一步是OneFlow

## Bucketing

Bucketing是一种训练多个不同但又相似的结构的网络的方法，这些网络共享相同的参数集。

一个典型的应用是循环神经网络（RNNs）。实现RNNs通常会沿时间轴将网络显式地展开。为了处理序列中的所有元素，我们需要将网络展开成最大可能的序列长度。然而这很浪费资源，因为对于较短的序列，大部分计算都浪费在填充的数据的执行上了。

Bucketing不再将网络展开成最大可能长度，而是展开成多个不同长度的实例（比如，长度为5, 10, 20, 30）。在训练过程中，对于不同长度的最小批数据，使用最恰当的展开模型。

https://blog.csdn.net/xuezhisdc/article/details/54927869

how_to——bucketing

## 框架设计

![](/images/img4/mindspore.jpg)

https://zhuanlan.zhihu.com/p/547878945

五谈AI软件栈--无责乱弹AI软件栈研发方法论

## 大模型

目前部分深度学习框架，例如Pytorch和Tensorflow，没有办法满足超大规模模型训练的需求，于是微软基于Pytroch开发了DeepSpeed，腾讯基于Pytroch开发了派大星PatricStar，达摩院基于Tensoflow开发的分布式框架Whale。像是华为昇腾的MindSpore、百度的PaddlePaddle，还有国内的一流科技OneFlow等厂商，对超大模型训练进行了深度的跟进与探索，基于原生的AI框架支持超大模型训练。

https://zhuanlan.zhihu.com/p/432813821

大模型的发展与解决的问题

https://zhuanlan.zhihu.com/p/432289008

从分布式训练到大模型训练

https://www.zhihu.com/question/498271491

为什么说大模型训练很难？

## 参考

https://mp.weixin.qq.com/s/ai_XI8ddP5I2m3ChCqnQsA

高效大规模机器学习训练，198页PDF带你概览领域前沿进展

https://openmlsys.github.io

机器学习系统：设计和实现

https://mp.weixin.qq.com/s/RAjusu-Jyqb8K19N8KZ_3w

一份552页《大规模数据系统：Large-scale Data Systems》硬核课程PPT

https://mp.weixin.qq.com/s/AeCQK2hFy60pq6y1tRcs_A

20页pdf，A Survey on Large-scale Machine

https://mp.weixin.qq.com/s/_1Yr_BbFhlNEW7UtYvAaoA

分布式深度学习，93页ppt概述最新DDL技术发展

https://mp.weixin.qq.com/s/jC5v9BKQvlxa2_6cikXV9w

分布式算法与优化，118页pdf

https://zhuanlan.zhihu.com/p/58806183

深度学习的分布和并行处理系统

https://zhuanlan.zhihu.com/p/56991108

一文说清楚Tensorflow分布式训练必备知识

https://zhuanlan.zhihu.com/p/26552293

Dataflow架构和神经网络加速器

https://zhuanlan.zhihu.com/p/28445511

浅析深度学习框架设计中的关键技术

https://mp.weixin.qq.com/s/wu32LBwrkkBIANMdknHlCA

C++并行实战，592页pdf，C++ Concurrency in Action

https://mp.weixin.qq.com/s/heVQ9AIZKxTiCNiAtYKaag

新加坡国立大学最新“大规模深度学习优化”综述论文，带你全面了解最新深度学习准确率和效率的优化方法

https://mp.weixin.qq.com/s/B4aQp_0YvS0jyUHNLQ5rRA

IBM发布新型分布式深度学习系统：结合软硬件实现当前最优性能

http://engineering.skymind.io/distributed-deep-learning-part-1-an-introduction-to-distributed-training-of-neural-networks

神经网络的分布式训练

https://mp.weixin.qq.com/s/nvuflLfOolidDDXJVe2DZA

美团深度学习系统的工程实践

https://mp.weixin.qq.com/s/IE6blClvhYlq3-QAGHo5ww

TensorFlow分布式计算机制解读：以数据并行为重

https://mp.weixin.qq.com/s/4Ii3um3jqfm5yKKxZAFdmA

继1小时训练ImageNet之后，大批量训练扩展到了3万2千个样本

https://mp.weixin.qq.com/s/kOCftzSbHe2mvDmlRp-ihA

Jeff Dean：AI对计算机系统设计的影响

https://mp.weixin.qq.com/s/XjNPaL6PC9LHX1PEGn5UZg

微软实时AI系统“脑波计划”有多牛？看完秒懂！

https://mp.weixin.qq.com/s/OkqUulFYHQSdgAbf9Fi9LA

CoCoA：大规模机器学习的分布式优化通用框架

https://mp.weixin.qq.com/s/ToIDncp9dS_qk47PsdZm5A

杜克大学：分布式深度学习训练算法TernGrad

https://mp.weixin.qq.com/s/rhtrN2qDspGkpJYDAVSX7w

UC Berkeley展示全新并行处理方法

https://mp.weixin.qq.com/s/ASqpPSIgW_bcFPBfRYz7Xg

哈佛大学提出在云、边缘与终端设备上的分布式深度神经网络DDNN

http://blog.sina.com.cn/s/blog_81f72ca70101kuk9.html

《Large Scale Distributed Deep Networks》中译文

https://mp.weixin.qq.com/s/X7XG51yohLnEZ_Jg6XK9oQ

Caffe作者贾扬清教你怎样打造更加优秀的深度学习架构

https://mp.weixin.qq.com/s/_mrYI7McMBUx0lEh4rNiYQ

百度开源移动端深度学习框架MDL，手机部署CNN支持iOS GPU

https://mp.weixin.qq.com/s/ZCNSq5FC2REoVTKAK2mJQg

分布式深度学习原理、算法详细介绍

https://mp.weixin.qq.com/s/Ewiil56vMkzhO2xDWgo-Wg

苹果发布Turi Create机器学习框架，5行代码开发图像识别

https://mp.weixin.qq.com/s/jOVUPhrCBI9W9vPvD9eKYg

UC Berkeley提出新型分布式框架Ray：实时动态学习的开端

https://mp.weixin.qq.com/s/r951Iasr4dke6MPHsUO0TA

开源DAWN，Stanford的又一力作

https://mp.weixin.qq.com/s/2jrMDeMcb47zpPfFLEcnIA

深度学习平台技术演进

https://mp.weixin.qq.com/s/L4CMKS53pNyvhhqvQhja0g

5种商业AI产品的技术架构设计

https://mp.weixin.qq.com/s/IqjKdAlGYREqCR9XQB5N1A

伯克利AI分布式框架Ray，兼容TensorFlow、PyTorch与MXNet

https://mp.weixin.qq.com/s/aNX_8UDYI_0u-MwMTYeqdQ

开发易、通用难，深度学习框架何时才能飞入寻常百姓家？

https://mp.weixin.qq.com/s/UbAHB-uEIvqYZCB7xIAJTg

机器学习新框架Propel：使用JavaScript做可微分编程

https://mp.weixin.qq.com/s/Ctl65r4iZNEOBxiiX2I2eQ

Momenta王晋玮：让深度学习更高效运行的两个视角

https://mp.weixin.qq.com/s/UxN9ZRmKLN30s7uPqMpHPQ

Jeff Dean等提出动态控制流编程模型，大规模机器学习性能提升21%

https://mp.weixin.qq.com/s/fx0Pfu0MOPjSkzi5mL6U_A

清华&斯坦福提出深度梯度压缩DGC，大幅降低分布式训练网络带宽需求

https://mp.weixin.qq.com/s/wIdTDHEPffWqHA3_XWBLyw

没错，纯SQL查询语句可以实现神经网络。

>SQL跑神经网络固然没有太大意义，然而分布式数据库已经有数十年的历史，对于设计分布式深度学习框架亦有重大的启发意义。

https://zhuanlan.zhihu.com/p/33351291

基于忆阻器（ReRAM），Computing-in-Memory的DLA
