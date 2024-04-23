---
layout: post
title:  深度加速（四）——NN Quantization（2）
category: DL acceleration 
---

* toc
{:toc}

# NN Quantization（续）

## UINT量化

论文：

《Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference》

![](/images/img2/INT8_2.png)

UINT量化使用bias将数据搬移到均值为0的区间。

$$r=S(q-Z)$$

r为fp32表示；q则是low-bit(如int8)表示；S是自low-bit（int8）到fp32的scale；Z为零点shift，用于使q的某数值对应于r中的0.0。

一般情况下，一个Tensor共享同一个S。有的时候为了提升精度，也可以一个channel共享同一个S，这也被称为per channel quantization。如果不共享S，则退化为普通的浮点数表示。

对于矩阵乘法来说：

$$q_3^{(i,k)}=Z_3+M\sum_{j=1}^N(q_1^{(i,j)}-Z_1)(q_2^{(j,k)}-Z_2)$$

则：

$$S_3(q_3^{(i,k)}-Z_3)=\sum_{j=1}^N S_1(q_1^{(i,j)}-Z_1)S_2(q_2^{(j,k)}-Z_2)$$

其中，$$M=\frac{S_1S_2}{S_3}$$。如果令$$S_3=S_1S_2$$，则上述运算将全部转为整数运算。

这篇论文的另一个贡献在于：原先的INT8量化是针对已经训练好的模型。而现在还可以在训练的时候就进行量化——前向计算进行量化，而反向的误差修正不做量化。这也就是所谓的训练中量化(Quantization Aware Training)。

与之相对的则是训练后量化(Post training quantization)。

![](/images/img4/quantize.jpg)

`tf.quantization.fake_quant_XXXX`系列API可用于前向计算时的量化。

Fake quant之所以叫伪量化，是因为虽然可量化weights/activations，但不是真正意义上的量化，即变量类型还是floating point，而不是integer。

## bfloat16

bfloat16是Google针对AI领域的特殊情况提出的浮点格式。目前已有Intel的AI processors和Google的TPU，提供对该格式的原生支持。

![](/images/img3/bfloat16.png)

上图比较了bfloat16和IEEE fp32/fp16的差异。可以看出bfloat16有如下特点：

1.bfloat16可以直接截取float32的前16位得到，所以在float32和bfloat16之间进行转换时非常容易。

2.bfloat16的Dynamic Range比float16大，不容易下溢。这点在training阶段更为重要，梯度一般都挺小的，一旦下溢变成0，就传递不了了。

3.bfloat16既可以用于训练又可以用于推断。Amazon也证明Deep Speech模型使用BFloat的训练和推断的效果都足够好。Uint8在大部分情况下不能用于训练，只能用于推断。

论文：

《Mixed Precision Training》

参考：

https://www.zhihu.com/question/275682777

如何评价Google在TensorFlow中引入的bfloat16数据类型？

https://zhuanlan.zhihu.com/p/56114254

PAI自动混合精度训练---TensorCore硬件加速单元在阿里PAI平台落地应用实践

https://mp.weixin.qq.com/s/zBtpwrQ5HtI6uzYOx5VsCQ

模型训练太慢？显存不够用？这个算法让你的GPU老树开新花

https://mp.weixin.qq.com/s/cYGMZuY7jSrjhUAXlDwD_w

Mixed Precision Training

https://zhuanlan.zhihu.com/p/441591808

混合精度训练原理

## Flexpoint

Flexpoint是Nervana的作品。

论文：

《Flexpoint: An Adaptive Numerical Format for Efficient Training of Deep Neural Networks》

讲了Google的成功案例，这里来讲一个**反面教材**。

![](/images/img3/flex.png)

这实际上就是INT16的量化，用在inference上应该还是可以的，然而Nervana的目标还有training。

和bfloat16相比，它至少有如下问题：

- 格式转换比bfloat16复杂。

- Dynamic Range小，容易梯度消失，从而造成模型很难收敛。

从指数位宽来看，Flexpoint和float16相同，都是5位。然而由于Flexpoint是共享指数，因此它真正的Dynamic Range是不如float16的。

![](/images/img4/scale.png)

上图是模型训练过程中，相关值的典型范围。

![](/images/img4/FP16.png)

float16已经被证明是不适合training的，更遑论Flexpoint了。

事实上，Intel内部已有人评价道：

>Flexpoint16三个月converge不了一个网络，而BF16一天就可以converge三个。

- 指数保存在Host上，会造成反复通信的带宽问题。

总的来说，这个方案虽然精巧，但是由于没有对数据特点做充分分析，没有意识到**Dynamic Range比底数精度更重要**，从而导致了最终的失败。

目前，整个芯片行业，已经由过去芯片专家根据以往经验（比如摩尔定律），定义下一代产品的规格，逐渐过渡到根据实际应用定义芯片的阶段，即所谓的“**软件定义硬件**”。

BF16的成功经验表明，算法专家在AI芯片中的重要程度，甚至超过了IC专家。

需要注意的是Flexpoint的失败，主要在于Dynamic Range和底数的位宽取舍上。他的设计思路本身还是有可取之处的。采用同样思路的MSFP就获得了成功。

MSFP由微软提出，在微软Project Brainwave产品上得到了广泛的应用。

参考：

https://www.intel.ai/flexpoint-numerical-innovation-underlying-intel-nervana-neural-network-processor/

Flexpoint: Numerical Innovation Underlying the Intel Nervana Neural Network Processor

https://zhuanlan.zhihu.com/p/33580205

Flexpoint——利用一种自适应的数据类型加速神经网络训练

https://mp.weixin.qq.com/s/z4OEPrAAtaNmBQoyvEd7Nw

从春秋到战国—论Nervana的倒掉

## TF32

![](/images/img3/tf32.png)

这是Nvidia推出的格式，相当于把FP32的指数和FP16的底数拼到了一起。有BF16珠玉在前，这个的设计只能说中规中矩了。

优点：底数精度虽然不如Dynamic Range重要，但对于运算结果还是有一定的影响的。这点在CNN中不太显著，但在RNN/Transformer中还是有所体现的。

缺点：毕竟不是16位，运算速度只有FP16/BF16的一半，但比FP32快一些。

BF16和TF32的先例一开，各种格式如火山爆发一般涌现。例如AMD的fp24，Pixar的pxr24，Enflame的ef32。

壁仞原创定义了TF32+，相较于TF32，在满足同样动态表示范围的前提下，增加了5位尾数。实际上就是pxr24。。。

参考：

https://zhuanlan.zhihu.com/p/143499632

NVIDIA A100 GPU中的TF32将AI训练与HPC速度提升20倍

https://www.cnblogs.com/zhouronghua/p/15170247.html

AI中各种浮点精度概念集合：fp16，fp32，bf16，tf32，fp24，pxr24，ef32

https://zhuanlan.zhihu.com/p/449857213

那些年，AI芯片里的浮点(FloatPoint)格式

## x86 Extended Precision Format

Intel在早期的8087芯片上引入了一种80bit的浮点格式：1 Sign + 15 Exponent + 80 Significand

这个格式设计不知道是否启发了BF16，因为它采用了和IEEE 754中128bit相同的Exponent，正如BF16使用FP32的Exponent一样，都是高一个档次的Exponent搭配低档次的Significand。

## FP8

![](/images/img5/FP8.png)

FP8包括两种常见的变种：E4M3(4位指数和3位尾数)和E5M2(5位指数和2位尾数)。

NVIDIA在H100中，添加了FP8的支持，但是去掉了对INT1/INT4的支持。。。看起来后两者还是实用价值偏低了。

参考：

https://zhuanlan.zhihu.com/p/521631165

Nvidia H100 中的FP8

## BF8 and Tesla CFloat

当我们将浮点的继续降低到8-bit的时候，BF8遇到的挑战越来越大。

在论文HFP8中，模型inference的时候，FP8(1-4-3)在mobilenet和transformer任务上明显的精度降低。

对于training来说，遇到的挑战进一步增大，weight/gradients/activation的范围相差更大，没有办法选择一个合适的格式来满足所有数值的要求。

HFP8就提出了一种Hybrid的方式：forward的时候用FP-1-4-3，backward的时候用FP-1-5-2。forward的时候，更关注精度，backward的时候更注重范围。这样的话，HFP8就能够在训练的过程中获得接近FP32的表现。

在工业界Tesla DoJo提出了一种可配置的CFloat，exponent和mantissa的位数可以动态的调整，只要满足总共的bit数就可以了。这样，由软件来选择合适的浮点类型CFormat，来最大化的利用硬件的性能。

《8-BIT NUMERICAL FORMATS FOR DEEP NEURAL NETWORKS》

CFormat这种动态调整exponent和mantissa位数的量化方式，又被称为Dynamic Fixed Point Quantization。

## W4A16

![](/images/img5/W4.png)

这种量化方式只对Weight使用4bit量化，而其他Tensor仍然使用16bit的浮点数。A是Activation的意思。

W4A16主要有GPTQ和AWQ等实现。

## q4f16 & q3f16

q3f16（q3指使用Quantize 3 bit来量化，f16是指核心计算使用fp 16来计算）。

## FP4

![](/images/img5/FP4.png)

IEEE版本的FP4(E2M1)如上图所示，但由于过于粗糙，一般使用更多的是所谓的NormalFloat的NF4。

NF4只能表示[-1, 1]之间的浮点数。由于在神经网络中，预训练的权重通常具有零中心的正态分布。所以根据正态分布的累积分布函数来量化，比均匀量化，效果要好一些。

所有可能的NF4值为:

```
[-1.0, -0.6961928009986877, -0.5250730514526367, -0.39491748809814453,
  -0.28444138169288635, -0.18477343022823334, -0.09105003625154495, 0.0,
  0.07958029955625534, 0.16093020141124725, 0.24611230194568634, 0.33791524171829224,
  0.44070982933044434, 0.5626170039176941, 0.7229568362236023, 1.0]
```

注意：0左侧和右侧的量化是非对称的，因为[-1, 0]之间有8个值，而[0, 1]之间有9个值。

NF4是QLoRA引入的，而FP4目前只有NV的GPU支持。

## Posit

![](/images/img4/Posit.png)

上图是Posit格式的示意图，除了符号位、指数和底数之外，它还包括了regime bits。

regime bits不知道怎么翻译，这里不妨意译为**超指数**。它的公式是：

$$useed^k$$

其中，

$$useed=2^{2^{es}}$$

es表示指数位的宽度，这里是3，所以$$useed=2^{2^{3}}=2^8=256$$

k的表示没有采用补码，而是一种特殊的方法：

![](/images/img4/Posit_2.png)

k=从左面开始数0或者1的个数。

需要注意的是，同样总位宽的Posit格式，其每个部分（符号位除外，固定占1位）的宽度是不定的。除了regime bits是必须有的（但宽度不定）之外，指数和底数都是可选项。

Posit的设计思路其实是很自然的：

- 底数增加1位，Dynamic Range增加2倍。

- 指数增加1位，Dynamic Range增加$$2^2$$倍。

- 如果还想增加Dynamic Range，自然就需要引入超指数了。

https://www.sigarch.org/posit-a-potential-replacement-for-ieee-754

Posit: A Potential Replacement for IEEE 754

常见的软件实现：

https://github.com/cjdelisle/libposit

https://github.com/stillwater-sc/universal
