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

key cache对于量化更加敏感，一般采用FP16，而value cache可以看做都是同分布的，所以可以很容易找到相应的量化参数，一般使用INT8，甚至INT4量化。

key cache通常采用per channel量化，而value cache则主要采用per token量化。

https://zhuanlan.zhihu.com/p/691537237

量化那些事之KVCache的量化

---

https://zhuanlan.zhihu.com/p/630832593

大模型推理性能优化之KV Cache解读

https://zhuanlan.zhihu.com/p/662498827

大模型推理加速：看图学KV Cache

https://zhuanlan.zhihu.com/p/700197845

大模型推理优化技术-KV Cache

https://www.cnblogs.com/rossiXYZ/p/18799503

KV Cache

## MQA & GQA

![](/images/img5/GQA.jpg)

首先是原始的MHA(Multi-Head Attention)，QKV三部分有相同数量的头，且一一对应。每次做Attention，head1的QKV就做好自己运算就可以，输出时各个头加起来就行。

而MQA则是，让Q仍然保持原来的头数，但K和V只有一个头，相当于所有的Q头共享一组K和V头，所以叫做Multi-Query了。

实现改变了会不会影响效果呢？确实会影响，但相对它30%-40%的吞吐收益，性能的些微降低是可以接受的。

而GQA呢，是MHA和MQA的折衷方案，既不想损失性能太多，又想获得MQA带来的推理加速好处。

![](/images/img6/KV_cache.png)

https://zhuanlan.zhihu.com/p/647130255

为什么现在大家都在用MQA和GQA？

## MLA

MLA是幻方量化投资的DeepSeek公司的DeepSeek-V2模型发明的。该模型1元/百万输入Tokens的价格，只有GPT4价格的1/100。

![](/images/img5/MLA_2.png)

![](/images/img5/MLA.png)

MLA的创新点，其实不是低秩分解，因为如果你想的话，GQA也可以重写成低秩分解的形式。

MLA的关键是低秩分解后面的事情，GQA是split and repeat，MLA则一般化为dense projection，从而实现了同样的低秩分解下更好的效果，或者同样效果下更低的秩，后者就是它能比GQA更进一步压缩KV Cache的根本原因。

推理时，MLA需要利用恒等变换才能实现低秩的KV Cache，但代价是每个头的Q/K的head_size变大了不少，所以理论上MLA推理的计算量是增加的。它最后之所以还能提高效率，是充分结合了LLM推理主要瓶颈是访存而不是计算这一特性。

MLA推理时，使用了**矩阵吸收（matrix absorb）**的技巧。

标准Attention权重的计算：

$$A=(x * W_q) * (x * W_k)^T$$

通过矩阵的恒等变化，得到吸收后的权重计算方式为：

$$A=(x * (W_q * W_k^T)) * x$$

而$$W_q * W_k^T$$可以作为Q的投影矩阵出现。

$$
\require{cancel}\begin{array}{c|c} 
\text{训练/Prefill} & \text{Decoding} \\ 
\\ 
\begin{gathered} 
\boldsymbol{o}_t = \left[\boldsymbol{o}_t^{(1)}, \boldsymbol{o}_t^{(2)}, \cdots, \boldsymbol{o}_t^{(h)}\right] \\[10pt] 
\boldsymbol{o}_t^{(s)} = \frac{\sum_{i\leq t}\exp\left(\boldsymbol{q}_t^{(s)} \boldsymbol{k}_i^{(s)}{}^{\top}\right)\boldsymbol{v}_i^{(s)}}{\sum_{i\leq t}\exp\left(\boldsymbol{q}_t^{(s)} \boldsymbol{k}_i^{(s)}{}^{\top}\right)} \\[15pt] 
\boldsymbol{q}_i^{(s)} = \left[\boldsymbol{x}_i\boldsymbol{W}_{qc}^{(s)},\boldsymbol{x}_i\boldsymbol{W}_{qr}^{(s)}\color{#3ce2f7}{\boldsymbol{\mathcal{R}}_i}\right]\in\mathbb{R}^{d_k + d_r}\\ 
\boldsymbol{k}_i^{(s)} = \left[\boldsymbol{c}_i\boldsymbol{W}_{kc}^{(s)},\boldsymbol{x}_i\boldsymbol{W}_{kr}^{\color{#ccc}{\smash{\bcancel{(s)}}}}\color{#3ce2f7}{\boldsymbol{\mathcal{R}}_i}\right]\in\mathbb{R}^{d_k + d_r} \\ 
\boldsymbol{v}_i^{(s)} = \boldsymbol{c}_i\boldsymbol{W}_v^{(s)}\in\mathbb{R}^{d_v},\quad\boldsymbol{c}_i = \boldsymbol{x}_i \boldsymbol{W}_c\in\mathbb{R}^{d_c} 
\end{gathered} 
& 
\begin{gathered} 
\boldsymbol{o}_t = \left[\boldsymbol{o}_t^{(1)}\boldsymbol{W}_v^{(1)}, \boldsymbol{o}_t^{(2)}\boldsymbol{W}_v^{(2)}, \cdots, \boldsymbol{o}_t^{(h)}\boldsymbol{W}_v^{(h)}\right] \\[10pt] 
\boldsymbol{o}_t^{(s)} = \frac{\sum_{i\leq t}\exp\left(\boldsymbol{q}_t^{(s)} \boldsymbol{k}_i^{\color{#ccc}{\smash{\bcancel{(s)}}}}{}^{\top}\right)\boldsymbol{v}_i^{\color{#ccc}{\smash{\bcancel{(s)}}}} }{\sum_{i\leq t}\exp\left(\boldsymbol{q}_t^{(s)} \boldsymbol{k}_i^{\color{#ccc}{\smash{\bcancel{(s)}}}}{}^{\top}\right)} \\[15pt] 
\boldsymbol{q}_i^{(s)} = \left[\boldsymbol{x}_i\boldsymbol{W}_{qc}^{(s)}\boldsymbol{W}_{kc}^{(s)}{}^{\top}, \boldsymbol{x}_i\boldsymbol{W}_{qr}^{(s)}\color{#3ce2f7}{\boldsymbol{\mathcal{R}}_i}\right]\in\mathbb{R}^{d_c + d_r}\\ 
\boldsymbol{k}_i^{\color{#ccc}{\smash{\bcancel{(s)}}}} = \left[\boldsymbol{c}_i, \boldsymbol{x}_i\boldsymbol{W}_{kr}^{\color{#ccc}{\smash{\bcancel{(s)}}}}\color{#3ce2f7}{\boldsymbol{\mathcal{R}}_i}\right]\in\mathbb{R}^{d_c + d_r}\\ 
\boldsymbol{v}_i^{\color{#ccc}{\smash{\bcancel{(s)}}}} = \boldsymbol{c}_i= \boldsymbol{x}_i \boldsymbol{W}_c\in\mathbb{R}^{d_c} 
\end{gathered} \\ 
\end{array}
$$

不难发现，矩阵吸收不是MLA独有的，MHA也可以做这样的操作。它本质上是用计算量换空间的策略。

Prefill阶段的瓶颈是计算量，MLA的矩阵吸收并没有优势，甚至更慢；但在Decode阶段，由于推理是逐token进行的，计算量少但需要线性积累KV Cache，总的KV Cache的大小就成为了显存瓶颈，MLA此时起到显著的作用。

而MHA使用矩阵吸收，由于增加的计算量过于巨大，无论何种阶段都没有收益。

需要注意，上述矩阵吸收的技巧没有考虑ROPE的影响，实际情况还要更复杂一些。

类似的，还有GLA。

---

https://github.com/deepseek-ai/FlashMLA

deepseek的MLA官方实现

---

参考：

https://kexue.fm/archives/10091

缓存与效果的极限拉扯：从MHA、MQA、GQA到MLA

https://www.zhihu.com/question/655172528

如何看待DeepSeek发布的MoE大模型DeepSeek-V2？

https://huggingface.co/blog/Junrulu/mla-codebased-analysis

结合Deepseek代码探讨MLA的改进及收益

https://zhuanlan.zhihu.com/p/697781431

还在用MHA？MLA来了DeepSeek-v2的MLA的总结和思考

https://zhuanlan.zhihu.com/p/703862723

大模型KV Cache节省神器MLA学习笔记（包含推理时的矩阵吸收分析）

https://blog.csdn.net/v_JULY_v/article/details/141535986

一文通透DeepSeek V2——通俗理解多头潜在注意力MLA：改进MHA，从而压缩KV缓存，提高推理速度

https://mp.weixin.qq.com/s/uiBPpZDyVhySP8eKKaIygw

再读MLA，还有多少细节是你不知道的

## KV Cache Architecture

KV Cache从诞生之初就是一个工程问题，上面主要讨论的是算法优化，而本节则聚焦于Architecture领域。

https://zhuanlan.zhihu.com/p/706791646

Mooncake: Kimi's KVCache-centric Architecture for LLM Serving

# 快速Transformer

轻量化Transformer是从计算量/时间/空间的角度出发，对于传统Transformer的优化。但是计算量少，不等于计算速度快：同样的计算量，不同种类的算子，在硬件上的速度也是有差异的，这种差异有时甚至可以到几个数量级。而我们最终的目的终究是速度快，而非计算量少。

因此，本节的快速Transformer主要着眼于软件工程角度，如何更好的利用各种硬件加速Transformer的计算。典型的有NVIDIA的FasterTransformer和腾讯的TurboTransformer。

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
