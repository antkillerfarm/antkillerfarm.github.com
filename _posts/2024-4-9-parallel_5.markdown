---
layout: post
title:  并行 & 框架 & 优化（六）——LLM Inference, Alpa
category: DL acceleration 
---

* toc
{:toc}

# 快速Transformer

## PagedAttention（续）

https://blog.vllm.ai/2023/06/20/vllm.html

vLLM: Easy, Fast, and Cheap LLM Serving with PagedAttention

https://zhuanlan.zhihu.com/p/691038809

vLLM核心技术PagedAttention原理

## 参考

https://mp.weixin.qq.com/s/1R_plHqxTLE-Fw3TjYnlJQ

GPU BERT上线性能不合格，看看微信AI的PPoPP论文

https://mp.weixin.qq.com/s/OgTQ3O_6lvOG07U-tjpTDA

如何让Transformer在GPU上跑得更快？快手：需要GPU底层优化

https://zhuanlan.zhihu.com/p/638468472

从FlashAttention到PagedAttention, 如何进一步优化Attention性能

# LLM Inference

https://zhuanlan.zhihu.com/p/653352979

LLM七种推理服务框架总结

https://zhuanlan.zhihu.com/p/671347964

大模型(LLM)推理框架汇总

https://zhuanlan.zhihu.com/p/642412124

LLM的推理优化技术纵览

https://github.com/DefTruth/Awesome-LLM-Inference

Awesome LLM Inference

---

一次用户请求，实际上既包含prefill，也包含decode。一个是计算密集型，一个是访存密集型。

prefill（用户输入）和decode（模型输出）的token量在不同场景下也是不一样的。如果是简单对话场景，通常模型的decode输出会更多一些，而如果是超长上下文场景，用户先上传一本几十万字的书再进行问答，这本书的prefill会直接起飞。在Agent场景下，大量预设的prompt也会占据非常多的prefill，不过prompt的prefill有不少机会可以提前算好KV而无需每个用户请求单独重复计算。

当整个推理系统服务几千万用户时，一个batch的几十个用户请求支持开胃菜。每个用户会不间断地和大模型进行交互，发出大量请求，但这些请求的间隔时间短则几秒，长则几分钟几小时。考虑人机交互的频率，一个用户请求结束后，对应的KV-cache继续常驻在高速内存中实际意义不大。

---

![](/images/img5/llm.png)

https://www.zhihu.com/tardis/zm/art/647813179

大模型文本生成——解码策略（Top-k & Top-p & Temperature）

https://b23.tv/OfdfBnz

如何设置大模型推理参数，top_k，top_p, temperature, num_beams

---

投机采样使用两个模型：一个是原始目标模型，另一个是比原始模型小得多的近似模型。近似模型用于进行自回归串行采样，而大型模型则用于评估采样结果。

https://zhuanlan.zhihu.com/p/651359908

大模型推理妙招—投机采样（Speculative Decoding）

## TensorRT-LLM

TensorRT-LLM是NVIDIA推出的基于TensorRT的LLM推理工具。

代码：

https://github.com/NVIDIA/TensorRT-LLM/

## vLLM

https://docs.vllm.ai/en/latest/

Easy, fast, and cheap LLM serving for everyone

# Alpa

Alpa是一个自动探索分布式策略的工具。

论文：

《Alpa: Automating Inter- and Intra-Operator Parallelism for Distributed Deep Learning》

代码：

https://github.com/openxla/xla/tree/main/xla/hlo/experimental/auto_sharding

文档：

https://alpa.ai/index.html

---

在介绍Alpa之前，先介绍一下Google的optimization库：

https://github.com/google/or-tools

文档：

https://developers.google.com/optimization

ILP可以分为下列几种类型：

（1）纯整数线性规划(Pure integer linear programming)：指全部决策变量都必须取整数值的整数线性规划。有时，也称为全整数规划。

（2）混合整数线性规划(Mimed integer linear programming)：指决策变量中有一部分必须取整数值，另一部分可以不取整数值的整数线性规划。

（3）0-1型整数线性规划(Zero-one integer linear programming)：指决策变量只能了取值0或1的整数线性规划。

---

为了评估不同Sharding策略的好坏，我们需要对Sharding策略建立cost model。

这里的cost主要包括：

- Communication cost
- Computation cost
- Memory cost
- Resharding cost

其中，Memory cost为该ILP问题的约束条件，其他几个为决策变量的影响因子。

Resharding cost是不同sharding之间切换产生的开销：

![](/images/img5/resharding.svg)

---

MLIR中有mesh dialect用于描述Sharding Spec：

```mlir
module @sharding_test {
  mesh.mesh @mesh_2d(shape = 4x8)

  func.func @matmul_on_operand_shard_batch_and_k(%arg0: tensor<32x1000x4096xf32>, %arg1: tensor<32x4096x8192xf32>) -> tensor<32x1000x8192xf32> {
    %sharding_annotated = mesh.shard %arg0 to <@mesh_2d, [[0], [], [1]]> annotate_for_users : tensor<32x1000x4096xf32>
    %sharding_annotated_0 = mesh.shard %arg1 to <@mesh_2d, [[0], [1]]> annotate_for_users : tensor<32x4096x8192xf32>
    %0 = tosa.matmul %sharding_annotated, %sharding_annotated_0 : (tensor<32x1000x4096xf32>, tensor<32x4096x8192xf32>) -> tensor<32x1000x8192xf32>
    %sharding_annotated_1 = mesh.shard %0 to <@mesh_2d, [[0]], partial = sum[1]> : tensor<32x1000x8192xf32>
    return %sharding_annotated_1 : tensor<32x1000x8192xf32>
  }
}
```

---

如何用数学语言表示一个二维的one-hot：

$$
\begin{aligned}

\forall (v,u) \in E, \
& \forall i \in [0, k_v), & \sum_{j \in [0, k_u)}\mathbf{e}_{vu} [i,j] \leq \mathbf{s}_v[i] \\
& \forall j \in [0, k_u), & \sum_{i \in [0, k_v)}\mathbf{e}_{vu} [i,j] \leq \mathbf{s}_u[j] \\

\end{aligned}
$$

![](/images/img5/one_hot_decision_vector.svg)

---

参考：

https://zhuanlan.zhihu.com/p/487588274

用ILP和DP自动探索DL分布式策略——Alpa

# 工具

FairScale是由Facebook Research开发的PyTorch扩展库。FSDP就是首发于这个库。

---

https://zhuanlan.zhihu.com/p/412118353

Kokkos:一个异构并行计算通用平台

# 数据流并行

数据流并行是Pipeline并行的高阶版本。广义的数据流希望通过图编译找到全局最优策略，本质上是一种把编译器当万金油的惰性做法，深度学习框架在系统调度这种比较粗放的尺度，围绕数据流做了这么多年的自动并行化，最后业界主流实际上的并行策略还是预设的这些Pipeline、Tensor并行的组合，而不是编译器搜出来的自动化的并行策略。

# 参考

https://mp.weixin.qq.com/s/iAHvfgn54zIwfM9K8KFJnw

DLM：微信大规模分布式n-gram语言模型系统

https://mp.weixin.qq.com/s/s7sHzzLANOp8-1LxgXQskA

谷歌开发者大会上，蚂蚁金服开源ElasticDL分布式深度学习系统

https://mp.weixin.qq.com/s/IQMXg6nIJO-9-IG3mJpvRg

ElasticDL：同时提升集群利用率和研发效率的分布式深度学习框架

https://mp.weixin.qq.com/s/uQzwqcGwC9ZveuW64Lzkmg

分布式训练怎么还减速了呢？

https://zhuanlan.zhihu.com/p/294698838

DLPerf—分布式深度学习最佳入门(踩坑)指南

https://zhuanlan.zhihu.com/p/76638962

Pytorch分布式训练

https://zhuanlan.zhihu.com/p/360405558

PyTorch分布式训练

https://mp.weixin.qq.com/s/0aSBHvscloEnPMRLyNjQsg

PyTorch分布式训练简明教程

https://blog.csdn.net/orangerfun/article/details/123887725

torch分布式训练

https://mp.weixin.qq.com/s/r7kt1k7D1wurWs_uxdLCtg

PyTorch源码解读之分布式训练

https://mp.weixin.qq.com/s/_85oWK2plv2QOX5Qfg_-ZA

大规模机器学习优化，195页ppt与视频

https://mp.weixin.qq.com/s/soruo90Dbtzi6d1kA63Akg

阿里提出智能算力引擎DCAF，节省20%GPU算力

https://mp.weixin.qq.com/s/oDak7peTT5ynNYrH7LSWTg

分布式层次GPU参数服务器架构

https://zhuanlan.zhihu.com/p/28226956

浮点峰值那些事儿

https://zhuanlan.zhihu.com/p/285994980

针对深度学习的GPU共享

https://mp.weixin.qq.com/s/Np4w7RC2JFlB7ZGIduu71w

爱奇艺机器学习平台的建设实践

https://mp.weixin.qq.com/s/DwjvEn04lGzKU8mDu-5q4g

大幅提升训练性能，字节跳动与清华提出新型分布式DNN训练架构

https://mp.weixin.qq.com/s/dJa5zOXgJJQOM5uWog3JZA

Local Parallesim：一种新并行训练方法

https://zhuanlan.zhihu.com/p/335116835

推荐系统Serving架构分析

https://mp.weixin.qq.com/s/DdsJ-ZB_cX9UhbQNK6dCag

分布式深度学习训练网络综述

https://mp.weixin.qq.com/s/qpwBGlTtTLEAhYAUpPyXTQ

CMU：分布式机器学习原理与策略 AAAI2021教程，附221页ppt

https://mp.weixin.qq.com/s/nK-9ck5S6noIETOb8b2dJw

vivo AI计算平台弹性分布式训练的探索和实践

https://mp.weixin.qq.com/s/IzLtn1SR-aFuxXM3GNZbFw

蘑菇街自研服务框架如何提升在线推理效率？

https://mp.weixin.qq.com/s/GheEA0Ag0vbhZeyzqpTl0A

分布式优化：在大数据时代应运而生

https://mp.weixin.qq.com/s/3uu50NWFJqA_MTb8wSxIKA

如何优雅地训练大型模型？

https://mp.weixin.qq.com/s/RMDEvy-3-L-Rag1OrZLYhg

深度学习模型的训练时内存次线性优化

https://mp.weixin.qq.com/s/8PUIJykzoNe-fYht5ozrcQ

新一代CTR预测服务的GPU优化实践

https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650771181&idx=1&sn=30b2a5abc7261b4f2ea122e8e96fdabf

世界第一超算跑深度学习模型，2.76万块V100 GPU将分布式训练扩展到极致

https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650771231&idx=2&sn=6907d6d7a98eab353a076ed48352aadc

15分钟完成Kinetics视频识别训练，除了超级计算机你还需要TSM

https://www.zhihu.com/question/404721763

如何评价Google的GShard论文？

https://mp.weixin.qq.com/s/eTwSo3GnxSnK-BwwZeWmKA

Jeff Dean等提出自动化分层模型，优化CPU、GPU等异构环境，性能提升超60%

https://mp.weixin.qq.com/s/q0VENBNgolpeWmDapF5q_g

在有池化层、1步幅的CNN上减少冗余计算，一种广泛适用的架构转换方法

https://mp.weixin.qq.com/s/YusIuUtvTwoskNRV_OV7iw

100万帧数据仅1秒！AI大牛颜水成团队强化学习新作，代码已开源

https://www.zhihu.com/answer/2259890109

资源受限的人工智能

https://mp.weixin.qq.com/s/ai_XI8ddP5I2m3ChCqnQsA

高效大规模机器学习训练，198页PDF带你概览领域前沿进展

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
