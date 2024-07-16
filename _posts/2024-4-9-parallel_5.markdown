---
layout: post
title:  并行 & 框架 & 优化（六）——LLM Inference, LLM System
category: DL acceleration 
---

* toc
{:toc}

# 快速Transformer（续）

## FlashAttention

代码：

https://github.com/Dao-AILab/flash-attention

![](/images/img5/FlashAttention.png)

对于self-attention块，除了两个大矩阵乘法是计算受限的，其他都是内存受限的逐点运算。

FlashAttention主要使用分块矩阵的思想，对矩阵乘法进行优化。

分块之后，参与运算的小块可以放入速度更快的存储单元，例如原来的运算需要反复读取HBM，而现在只需要读取一次HBM，然后就可以在SRAM或者Cache中完成所有的运算。

但Self-Attention中有softmax计算，而softmax的分母包含了所有元素相关的求和项，所以对Self-Attention进行分块计算的真正难点在于对softmax的分块计算。

FlashAttention提出了一种叫做Online Softmax的增量算法：我们首先计算一个分块的局部softmax值，然后存储起来。当处理完下一个分块时，可以根据此时的新的全局最大值和全局EXP求和项来更新旧的softmax值。接着再处理下一个分块，然后再更新。当处理完所有分块后，此时的所有分块的softmax值都是“全局的”。具体计算公式略。

其他优化点：

- softmax recomputing。前向通过保存logsumexp结果，反向利用logsumexp重新计算softmax。

- Dropout，前向阶段只保存dropout seed与offset，反向宁愿再算一遍dropout，放弃保存dropout mask。

https://www.zhihu.com/question/611236756

FlashAttention的速度优化原理是怎样的？

https://blog.csdn.net/v_JULY_v/article/details/133619540

通透理解FlashAttention与FlashAttention2：全面降低显存读写、加快计算速度

## FlashDecoding

FlashDecoding是FlashAttention项目的一部分，但由于优化方向有所不同，特别单列出来。

![](/images/img5/FlashDecoding.gif)

Prefill阶段在Q的seqlen维度以及batch_size维度做并行。

![](/images/img5/FlashDecoding.webp)

但是在Decoding阶段，是逐token生成，在利用KV Cache的情况下，每次推理实际的queries token数为1，已经无法通过queries进行并行了。

既然，Q和BS无法进一步并行了，那么对K,V进行并行是不是就可以了呢？

- 首先，将K/V切分成更小的块，比如5块；
- 然后在这些K/V块上，使用标准FlashAttention进行计算，得到所有小块的局部结果。
- 最后，使用一个额外的kernel做全局的reduce，得到正确输出。

FlashDecoding有时也被称为FlashAttention V2。

---

New hardware features on Hopper GPUs - WGMMA, TMA, FP8

https://www.zhihu.com/question/661395457

FlashAttention-3发布！有什么新优化点？

## PagedAttention

PagedAttention是UC Berkeley的作品。

![](/images/img5/PagedAttention.gif)

PagedAttention使用分页管理的方式管理KV Cache，将每个序列的KV Cache划分为块，每个块包含固定数量token的K/V。

![](/images/img5/PagedAttention_2.gif)

Parallel Sampling：我给模型发送一个请求，希望它对prompt做续写，并给出三种不同的回答。我们管这个场景叫parallel sampling。

显然这里的prompt部分的KV cache是完全重复。这时可以使用如上图所示的进阶版本，通过类似MMU的逻辑地址和物理地址的映射，来解决存储问题。

这个方法也可以推广到Beam Search、Shared prefix等场景。

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

A Survey on Efficient Inference for Large Language Models

---

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

https://blog.csdn.net/HUSTHY/article/details/125990877

Contrastive Search Decoding——一种对比搜索解码文本生成算法

https://zhuanlan.zhihu.com/p/656707466

DoLa：层对比解码改善LLM的真实性

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

![](/images/img5/RAG.png)

Retrieval Augmented Generation（检索增强生成）：通过检索获取相关的知识并将其融入Prompt，让大模型能够参考相应的知识从而给出合理回答。因此，可以将RAG的核心理解为“检索+生成”，前者主要是利用向量数据库的高效存储和检索能力，召回目标知识；后者则是利用大模型和Prompt工程，将召回的知识合理利用，生成目标答案。

https://zhuanlan.zhihu.com/p/668082024

一文搞懂大模型RAG应用

https://blog.csdn.net/v_JULY_v/article/details/137711599

RAG进阶之通用文档处理：从RAGFlow、TextMonkey到mPLUG-DocOwl 1.5

https://zhuanlan.zhihu.com/p/695299235

RAG路线图： Reliable, Adaptable, and Attributable Language Models with Retrieval

---

假设有一个10w的外部数据，我们的原始输入Prompt长度为100，长度限制为4k，通过查询-检索的方式，我们能将最有效的信息提取集中在这4k的长度中，与Prompt一起送给大模型，从而让大模型得到更多的信息。此外，还能通过多轮对话的方式不断提纯外部数据，达到在有限的输入长度限制下，传达更多的信息给大模型。

https://blog.csdn.net/qq_40491305/article/details/130898052

一文看懂LlamaIndex用法，为LLMs学习私有知识

## 向量数据库

向量搜索在搜索、推荐、NLP等众多应用领域被广泛的使用，典型的互联网业务，包括电商、出行、点评、地图等都大量使用相关技术。随着ChatGPT带来的AI技术应用新热潮，向量数据库又一次地获得了更多的关注。它可以解决LLM不长记性（Memory，记忆）的问题。

普遍认为 LLM + Vector Search + API pool 会变成复杂AI场景的标准解决方案。

类似Pinecone，Weaviate，Qdrant，Chroma这样的专用向量数据库最初是为了解决ChatGPT的记忆能力不足而出现的Workaround。

最发布的ChatGPT 3.5的上下文窗口只有4K Token，也就是不到两千个汉字。然而当下GPT 4的上下文窗口已经发展到了128K，扩大了32倍，足够塞进一整篇小说了。而且未来还会更大。这时候，用作临时周转的垫脚石——向量数据库SaaS就处在一个尴尬的位置上了。

https://www.zhihu.com/question/603117242

为什么各大VC最近都在投向量数据库？

https://zhuanlan.zhihu.com/p/668509885

向量数据库凉了吗？

## 实战

https://blog.csdn.net/v_JULY_v/article/details/132178447

七月论文审稿GPT第1版：通过3万多篇paper和10多万的review数据微调RWKV

https://blog.csdn.net/v_JULY_v/article/details/134183799

七月论文审稿GPT第2版：用一万多条paper-review数据微调LLaMA2 7B最终反超GPT4

https://blog.csdn.net/v_JULY_v/article/details/131552592

基于LangChain+LLM的本地知识库问答：从企业单文档问答到批量文档问答

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
