---
layout: post
title:  并行 & 框架 & 优化（一）
category: DL acceleration 
---

* toc
{:toc}

# MPI（续）

## 参考

https://zhuanlan.zhihu.com/p/69497154

高性能计算--mpi

https://mpitutorial.com/tutorials/

MPI Tutorials

https://zhuanlan.zhihu.com/p/363710263

集体通信

https://downey.io/notes/omscs/cse6220/distributed-memory-model-mpi-collectives/

distributed memory model and mpi collectives

https://blog.csdn.net/q19149/article/details/102594031

集合通信函数图解

https://zhuanlan.zhihu.com/p/465967735

分布式训练硬核技术——通讯原语

https://zhuanlan.zhihu.com/p/276122469

分布式训练常用技术简介

https://zhuanlan.zhihu.com/p/425830285

最理想的点到点通信库究竟是怎样的？

# 并行 & 框架 & 优化

## 概述

![](/images/img4/Deep_Learning_System.png)

并行计算的必要性：

美国Sandia国家实验室一项模拟测试证明：由于存储机制和内存带宽的限制，16核、32核甚至64核处理器对于超级计算机来说，不仅不能带来性能提升，甚至可能导致效率的大幅度下降。必须研究并行计算的算法，才能有效克服这些缺陷。

![](/images/img2/speed.jpg)

最早的并行计算项目是斯坦福大学的BrookGPU：

http://graphics.stanford.edu/projects/brookgpu/

科学家们很早就意识到GPU进行通用数值计算的可行性。然而GPU从创建之初，就主要针对图形渲染，早期的通用计算需要通过很复杂的处理，才能安排到GPU的pipeline中进行运算。BrookGPU的作用就是给通用数值计算提供一个相对简单的接口。

随着GPU厂商逐渐意识到通用数值计算的重要性，并陆续提出了一些新框架。BrookGPU项目也就过时了。

常见并行计算框架

| 名称 | 优点 | 缺点 |
|:--:|:--:|:--:|
| CUDA | 生态系统良好，发展成熟。 | 只能NV的卡。 |
| OpenCL | 跨平台，很容易异构计算。 | 大家都把它当作干儿子，没人认真写它的驱动。 |
| C++ AMP | 基于Direct compute，易于使用。 | Windows Only。 |
| OpenMP | 移植改动少 | CPU多线程而已，和GPU无关。 |
| Metal | for iOS | - |

其他的并行计算框架还有：OpenACC、AMD stream、PGI。

## OpenCL vs CUDA

![](/images/article/opencl_cuda.jpg)

## OpenMp

https://github.com/lapesd/libgomp

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

## Parallelism Bubble

![](/images/img5/bubble.jpg)

模型经过拆分后的上一个rank的stage需要长期持续处于空闲状态，等待其他rank的stage计算完成，才可以开始计算，这极大降低了设备的平均使用率。这种现象被称为并行空泡（Parallelism Bubble）。

https://zhuanlan.zhihu.com/p/618590870

大模型训练Pipeline Parallel流水并行性能分析

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

## tf.distribute & MultiDevice

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

---

Rendezvous是一个法语单词，发音也比较特殊，一般直译为“约会、相会、会和”，而在TensorFlow中，Rendezvous是用来完成消息传输的通信组件。

消息传输的唯一标识符——ParsedKey

Send和RecvAsync二者的相对顺序是不能保证先后的，经常出现需求比供给在时间片上先到的情况，总是迟到的一方执行waiter函数。

Send方——将Ready的Tensor挂入本地Table

Recv方——向Send方主动发出请求，触发通信过程

所以，真正的通信过程由Recv方触发，而不是Send方。

TensorFlow已经支持包括gRPC，RDMA（Remote Direct Memroy Access），GDR（GPU Direct）和MPI四种通信协议。

BFC（Best-Fit with Coalescing）是dlmalloc的一个简单实现版本。

https://www.cnblogs.com/deep-learning-stacks/p/10354258.html

TensorFlow中的通信机制——Rendezvous（一）本地传输

https://www.cnblogs.com/deep-learning-stacks/p/10355770.html

TensorFlow中的通信机制——Rendezvous（二）gRPC传输

https://blog.csdn.net/gaofeipaopaotang/article/details/80736452

模型优化之分布式执行

https://xieyu.github.io/blog/tensorflow/rendezvous.html

Tensorflow Rendezvous

---

Host to Device：

SameWorkerRecvDone -> CopyTensor::ViaDMA -> CopyHostToDevice -> XlaDeviceContext::CopyCPUTensorToDevice ->

GenericTransferManager::TransferLiteralToDeviceAsync -> TransferManager::TransferBufferToDevice -> Stream::ThenMemcpy

---

大模型不光模型的训练是分布式的，数据集也是分布式的。

https://www.alanshawn.com/tech/2022/02/27/tensorflow-big-dataset.html

Using Huge, Heterogenous Datasets in TensorFlow

https://tensorflow.google.cn/datasets/beam_datasets?hl=zh-cn

使用Apache Beam生成大型数据集

https://www.tensorflow.org/tutorials/distribute/input?hl=zh-cn

分布式输入

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

## 数据下沉

数据下沉是将数据一次性传输到端侧，减少频繁的Host-Device数据传输来加速的技术。因为数据在Device上通过通道传输，Host侧和Device侧之间每个epoch进行一次数据交互，所以每个epoch只返回一次结果。

https://zhuanlan.zhihu.com/p/397481167

MindSpore的桎梏和破局

## 大模型训练

目前部分深度学习框架，例如Pytorch和Tensorflow，没有办法满足超大规模模型训练的需求，于是微软基于Pytroch开发了DeepSpeed，腾讯基于Pytroch开发了派大星PatricStar，达摩院基于Tensoflow开发的分布式框架Whale。像是华为昇腾的MindSpore、百度的PaddlePaddle，还有国内的一流科技OneFlow等厂商，对超大模型训练进行了深度的跟进与探索，基于原生的AI框架支持超大模型训练。

https://zhuanlan.zhihu.com/p/432813821

大模型的发展与解决的问题

https://zhuanlan.zhihu.com/p/432289008

从分布式训练到大模型训练
