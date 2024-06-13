---
layout: post
title:  并行 & 框架 & 优化（六）——LLM Inference, LLM System, Alpa
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

# LLM System

## RAG

![](/images/img5/RAG.jpg)

Retrieval Augmented Generation（检索增强生成）：通过检索获取相关的知识并将其融入Prompt，让大模型能够参考相应的知识从而给出合理回答。因此，可以将RAG的核心理解为“检索+生成”，前者主要是利用向量数据库的高效存储和检索能力，召回目标知识；后者则是利用大模型和Prompt工程，将召回的知识合理利用，生成目标答案。

https://zhuanlan.zhihu.com/p/668082024

一文搞懂大模型RAG应用

## 向量数据库

向量搜索在搜索、推荐、NLP等众多应用领域被广泛的使用，典型的互联网业务，包括电商、出行、点评、地图等都大量使用相关技术。随着ChatGPT带来的AI技术应用新热潮，向量数据库又一次地获得了更多的关注。它可以解决LLM不长记性（Memory，记忆）的问题。

普遍认为 LLM + Vector Search + API pool 会变成复杂AI场景的标准解决方案。

类似Pinecone，Weaviate，Qdrant，Chroma这样的专用向量数据库最初是为了解决ChatGPT的记忆能力不足而出现的Workaround。

最发布的ChatGPT 3.5的上下文窗口只有4K Token，也就是不到两千个汉字。然而当下GPT 4的上下文窗口已经发展到了128K，扩大了32倍，足够塞进一整篇小说了。而且未来还会更大。这时候，用作临时周转的垫脚石——向量数据库SaaS就处在一个尴尬的位置上了。

https://www.zhihu.com/question/603117242

为什么各大VC最近都在投向量数据库？

https://zhuanlan.zhihu.com/p/668509885

向量数据库凉了吗？

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
