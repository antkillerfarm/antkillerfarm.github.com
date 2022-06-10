---
layout: post
title:  深度加速（四）——NN Quantization（2）
category: DL acceleration 
---

* toc
{:toc}

# NN Quantization

## UINT量化（续）

这篇论文的另一个贡献在于：原先的INT8量化是针对已经训练好的模型。而现在还可以在训练的时候就进行量化——前向计算进行量化，而反向的误差修正不做量化。

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

Mixed Precision Traning

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

参考：

https://mp.weixin.qq.com/s/cGKtvtZzR--sGL4oNSZfAw

深度分析NVIDIA A100显卡架构

https://zhuanlan.zhihu.com/p/143499632

NVIDIA A100 GPU中的TF32将AI训练与HPC速度提升20倍

## x86 Extended Precision Format

Intel在早期的8087芯片上引入了一种80bit的浮点格式：1 Sign + 15 Exponent + 80 Significand

这个格式设计不知道是否启发了BF16，因为它采用了和IEEE 754中128bit相同的Exponent，正如BF16使用FP32的Exponent一样，都是高一个档次的Exponent搭配低档次的Significand。

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

## Saturate Quantization

上述各种量化方法都是在保证数值表示范围的情况下，尽可能提高fl或者scale。这种方法也叫做Non-saturation Quantization。

NVIDIA在如下文章中提出了一种新方法：

http://on-demand.gputechconf.com/gtc/2017/presentation/s7310-8-bit-inference-with-tensorrt.pdf

8-bit Inference with TensorRT

![](/images/img2/INT8_3.png)

Saturate Quantization的做法是：将超出上限或下限的值，设置为上限值或下限值。

如何设置合理的Saturate threshold呢？

可以设置一组门限，然后计算每个门限的分布和原分布的相似度，即KL散度，选择最相似分布的门限即可。

参考：

https://blog.csdn.net/u013010889/article/details/90295078

int8量化和tvm实现

## Block-wise Quantization

经过观察，在正态分布下，绝对值很大的参数的比例会很少，所以一起归一会使得大多数参数变得很小，从而使得量化过程中的一些数字范围对应的int8没有被充分利用，导致更多的信息丢失。

把参数划分为了小Block，在进行量化的时候，按照block内绝对值最大的数对这个block进行归一化，使得所有参数都落在 [-1, 1] 这个范围，这就是Block-wise Quantization。

## Trainning Quantization

除了上面这些无条件Quantization之外，训练中的Quantization也是一大类算法。

比如下面提到的PACT量化，不仅对weight进行量化，还通过不断训练，限制每一层tensor的数值范围。

参考：

https://mp.weixin.qq.com/s/7rMnzbvp1hjDLuw_oifbng

我们是这样改进PACT量化算法的

## 量化技巧

1.设计模型时，需要对输入进行归一化，缩小输入值的值域范围，以减小量化带来的精度损失。

2.tensor中各分量的值域范围最好相近。这个的原理和第1条一致。比如YOLO的结果中，同时包含分类和bbox，而且分类的值域范围远大于bbox，导致量化效果不佳。

3.最好不要使用ReluN这样的激活函数，死的神经元太多。神经元一旦“死亡”，相应的权值就不再更新，而这些值往往不在正常范围内。

4.对于sigmoid、tanh这样的S形函数，其输入在$$\mid x \mid > \sigma$$范围的值，最终的结果都在sigmoid、tanh的上下限附近。因此，可以直接将这些x值量化为$$\sigma$$。这里的$$\sigma$$的取值，对于sigmoid来说是6，而对于tanh来说是3。

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

## 二值神经网络

二值神经网络的主要缺点在于，它们无法实现与完全精度的深层网络一样高的精度。但这一直在缓慢地变化，已经有了很多进步。

http://blog.csdn.net/tangwei2014/article/details/55077172

二值化神经网络介绍

https://mp.weixin.qq.com/s/0twiT2mrVdnwyS-mqgrjVA

低比特量化之XNOR-Net

https://mp.weixin.qq.com/s/oumf8l28ijYLxc9fge0FMQ

嵌入式深度学习之神经网络二值化（1）

https://mp.weixin.qq.com/s/tbRj5Wd69n9gvSzW4oKStg

嵌入式深度学习之神经网络二值化（2）

https://mp.weixin.qq.com/s/RsZCTqCKwpnjATUFC8da7g

嵌入式深度学习之神经网络二值化（3）

https://blog.csdn.net/stdcoutzyx/article/details/50926174

二值神经网络（Binary Neural Network，BNN）

https://mp.weixin.qq.com/s/Q54AdQmqa5JD0v9CEeFtSQ

二值化神经网络(BNN)综述

https://mp.weixin.qq.com/s/lVja7woyFWpmr9sH0CitAA

BMXNet：基于MXNet的开源二值神经网络实现

https://mp.weixin.qq.com/s/naDk0mmxd08dNl9LawLUnw

不使用先验知识与复杂训练策略，从头训练二值神经网络！

https://mp.weixin.qq.com/s/Xvlxs-Os2meduHrEQFc7vg

第一次胜过MobileNet的二值神经网络，-1与+1的三年艰苦跋涉

https://mp.weixin.qq.com/s/Ak9Yh_MBDR6i7J2rDR99eQ

低成本的二值神经网络介绍以及它能代替全精度网络吗?

https://mp.weixin.qq.com/s/tbRj5Wd69n9gvSzW4oKStg

异或神经网络

https://mp.weixin.qq.com/s/XzLJzfvpP93cDYplf6-LXA

港科腾讯等提出Bi-Real net：超XNOR-net 10%的ImageNet分类精度

https://mp.weixin.qq.com/s/wCx7rQFwC2mW45FMR77tGQ

二值网络，围绕STE的那些事儿

https://mp.weixin.qq.com/s/7L26ghhDqdMU6LRV0iD6vQ

模型量化从1bit到8bit，二值到三值
