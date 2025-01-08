---
layout: post
title:  并行 & 框架 & 优化（六）——KV Cache, 快速Transformer
category: DL acceleration 
---

* toc
{:toc}

# Megatron-LM（续）

代码：

https://github.com/NVIDIA/Megatron-LM

微软还有一个项目将DeepSpeed和Megatron-LM结合了起来：

https://github.com/microsoft/Megatron-DeepSpeed

参考：

https://zhuanlan.zhihu.com/p/522198082

Megatron-LM 第三篇Paper总结——Sequence Parallelism & Selective Checkpointing

https://zhuanlan.zhihu.com/p/366906920

Megatron论文和代码详细分析(1)

https://zhuanlan.zhihu.com/p/388830967

Megatron论文和代码详细分析(2)

https://blog.csdn.net/v_JULY_v/article/details/132462452

通俗理解Megatron-DeepSpeed之模型并行与数据并行

https://mp.weixin.qq.com/s/bvF50XRaA9cO2O4oB31kbg

大语言模型分布式训练的量化分析与最佳实践,以GPT-175B为例

# KV Cache

![](/images/img5/KV_Cache.png)

对于每个输入的prompt，在计算第一个token输出的时候，每个token的attention肯定是都要从头计算。但是在后续token的生成中，都需要计算self-attention，也就是输入prompt以及前面输出的token的attention。这是就需要用到前面每一个token的K和V，由于每一层的参数矩阵是不变的，此时只有刚生成的那个token的K和V需要从头计算，输入prompt和之前生成的token的K和V其实是跟上一轮一样的。

我们可以把每一层的K、V矩阵缓存起来，这就是所谓的KV Cache。

---

上图中，由于每次只有一个Q进行计算，所以并没有Q cache.

https://www.zhihu.com/question/653658936

为什么加速LLM推断有KV Cache而没有Q Cache？

---

![](/images/img5/QKV_cache.png)

![](/images/img5/KV_Cache_2.png)

上图中的Prompt Phase又称为Prefill Phase，Token generation Phase又称为Decoding Phase。

---

![](/images/img5/sharing_wide.jpg)

KV Cache的使用方式一般如上图所示。其中蓝色表示输入里可以share的部分（即KV Cache），绿色表示输入里不可以share的部分，黄色表示输出。

---

某些非Dense Attention的KV cache的例子。

![](/images/img5/StreamingLLM.png)

---

![](/images/img5/KV_Cache_3.png)

---

https://zhuanlan.zhihu.com/p/630832593

大模型推理性能优化之KV Cache解读

https://zhuanlan.zhihu.com/p/662498827

大模型推理加速：看图学KV Cache

https://zhuanlan.zhihu.com/p/700197845

大模型推理优化技术-KV Cache

## MQA & GQA

![](/images/img5/GQA.jpg)

首先是原始的MHA(Multi-Head Attention)，QKV 三部分有相同数量的头，且一一对应。每次做Attention，head1的QKV就做好自己运算就可以，输出时各个头加起来就行。

而MQA则是，让Q仍然保持原来的头数，但K和V只有一个头，相当于所有的Q头共享一组K和V头，所以叫做Multi-Query了。

实现改变了会不会影响效果呢？确实会影响，但相对它30%-40%的吞吐收益，性能的些微降低是可以接受的。

而GQA呢，是MHA和MQA的折衷方案，既不想损失性能太多，又想获得MQA带来的推理加速好处。

https://zhuanlan.zhihu.com/p/647130255

为什么现在大家都在用MQA和GQA？

## MLA

MLA是幻方量化投资的DeepSeek公司的DeepSeek-V2模型发明的。该模型1元/百万输入Tokens的价格，只有GPT4价格的1/100。

![](/images/img5/MLA_2.png)

![](/images/img5/MLA.png)

MLA的创新点，其实不是低秩分解，因为如果你想的话，GQA也可以重写成低秩分解的形式。

MLA的关键是低秩分解后面的事情，GQA是split and repeat，MLA则一般化为dense projection，从而实现了同样的低秩分解下更好的效果，或者同样效果下更低的秩，后者就是它能比GQA更进一步压缩KV Cache的根本原因。

推理时，MLA需要利用恒等变换才能实现低秩的KV Cache，但代价是每个头的Q/K的head_size变大了不小，所以理论上MLA推理的计算量是增加的。它最后之所以还能提高效率，是充分结合了LLM推理主要瓶颈还是访存而不是计算这一特性。

https://kexue.fm/archives/10091

缓存与效果的极限拉扯：从MHA、MQA、GQA到MLA

https://www.zhihu.com/question/655172528

如何看待DeepSeek发布的MoE大模型DeepSeek-V2？

# 快速Transformer

轻量化Transformer是从计算量/时间/空间的角度出发，对于传统Transformer的优化。而快速Transformer主要着眼于软件工程角度，如何更好的利用各种硬件加速Transformer的计算。典型的有NVIDIA的FasterTransformer和腾讯的TurboTransformer。

## FasterTransformer

![](/images/img5/FasterTransformer.png)

FasterTransformer作为这一流派的开山鼻祖，其使用的手段，以现在的眼光来看，已经过于平常了。但从工程的角度，它首次揭示了FLOP少，不等于快。

上图是其中的一处优化点，NV将所有非矩阵乘法的运算，都合成一个算子，从而大大减少了算子的数量，以及相应的调度时间。

## EffectiveTransformer/ByteTransformer

EffectiveTransformer/ByteTransformer都是ByteDance的作品，基于FasterTransformer的进一步优化。

![](/images/img5/EffectiveTransformer.png)

![](/images/img5/EffectiveTransformer_2.png)

Transformer模型运算中，padding部分带来了的无效计算。比如Bert一个输入Batch的固定句长是64，但平均句长只有40，那么EffectiveTransformer在FasterTransformer的基础上还可以再多获得约1.5倍的加速。

具体的方法就是：对mask矩阵求前缀和，然后根据前缀和矩阵搬运内存，实现删除和恢复padding。

ByteTransformer在此基础上对self attention部分的padding做了优化。

https://zhuanlan.zhihu.com/p/139255930

使用EffectiveTransformer加速BERT

https://www.thepaper.cn/newsDetail_forward_23343189

大幅优化推理过程，字节高性能Transformer推理库获IPDPS 2023最佳论文奖

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

---

![](/images/img6/FlashAttention.png)

FlashAttention V1里Q在内层循环，而V2里K在内层循环。V1对于计算softmax不友好，因为每次计算的中间结果只是部分和，只有全算完才能释放相关存储。

---

https://www.zhihu.com/question/611236756

FlashAttention的速度优化原理是怎样的？

https://blog.csdn.net/v_JULY_v/article/details/133619540

通透理解FlashAttention与FlashAttention2：全面降低显存读写、加快计算速度

https://zhuanlan.zhihu.com/p/668888063

原理篇: 从Online-Softmax到FlashAttention V1/V2/V3

https://zhuanlan.zhihu.com/p/665170554

FlashAttention核心逻辑以及V1 V2差异总结

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

https://crfm.stanford.edu/2023/10/12/flashdecoding.html

Flash-Decoding for long-context inference

---

New hardware features on Hopper GPUs - WGMMA, TMA, FP8

https://www.zhihu.com/question/661395457

FlashAttention-3发布！有什么新优化点？

https://zhuanlan.zhihu.com/p/661478232

FlashAttenion-V3: Flash Decoding详解

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

https://blog.csdn.net/v_JULY_v/article/details/144218958

一文通透vLLM与其核心技术PagedAttention：减少KV Cache碎片、提高GPU显存利用率(推理加速利器)
