---
layout: post
title:  NN Quantization（三）——混合精度训练
category: DL acceleration 
---

* toc
{:toc}

# 量化策略（续）

## 量化技巧

1.设计模型时，需要对输入进行归一化，缩小输入值的值域范围，以减小量化带来的精度损失。

2.tensor中各分量的值域范围最好相近。这个的原理和第1条一致。比如YOLO的结果中，同时包含分类和bbox，而且分类的值域范围远大于bbox，导致量化效果不佳。

3.最好不要使用ReluN这样的激活函数，死的神经元太多。神经元一旦“死亡”，相应的权值就不再更新，而这些值往往不在正常范围内。

4.对于sigmoid、tanh这样的S形函数，其输入在$$\mid x \mid > \sigma$$范围的值，最终的结果都在sigmoid、tanh的上下限附近。因此，可以直接将这些x值量化为$$\sigma$$。这里的$$\sigma$$的取值，对于sigmoid来说是6，而对于tanh来说是3。

# NN硬件的指标术语

MACC：multiply-accumulate，乘法累加。

FLOPS：Floating-point Operations Per Second，每秒所执行的浮点运算次数。

显然NN的INT8计算主要以MACC为单位。

# gemmlowp

gemmlowp是Google提出的一个支持低精度数据的GEMM（General Matrix Multiply）库。

代码：

https://github.com/google/gemmlowp

# FBGEMM

FBGEMM（Facebook General Matrix Multiplication）是一个专为服务器端推理设计的低精度、高效率的矩阵乘法和卷积库。它提供了小批量大小的高效低精度矩阵乘法，并支持行向量量化和异常感知量化等减少精度损失的技术，以实现极致的计算性能。

代码：

https://github.com/pytorch/FBGEMM

# 论文

《Quantizing deep convolutional networks for efficient inference: A whitepaper》

# Optimizer Quantization

![](/images/img5/DTQ.png)

某些特别大或特别小的异常值，对量化会产生较大的精度影响。动态树量化（dynamic tree quantization）就是一种以较低的量化精度损失处理这种情况的方法。

- 首位是符号位

- 符号位后连续的0的数量表示指数大小

- 再之后的第一个值为1的是指示位

- 线性量化区域

指示位是可以动态移动的，通过移动指示位可以灵活选择更大的范围，还是更高的精度。

论文：

8-BIT OPTIMIZERS VIA BLOCK-WISE QUANTIZATION

论文作者为此开发了bitsandbytes库：

https://github.com/bitsandbytes-foundation/bitsandbytes

因为经常使用`import bitsandbytes as bnb`导入，所以该库又被称为bnb。

参考：

https://www.cnblogs.com/chentiao/p/17388568.html

bitsandbytes--Facebook推出8比特优化器大大减少显存

# 混合精度训练

论文：

《Mixed Precision Training》

AMP：Automatic Mixed Precision

LLM的成功证明了增大神经网络的参数规模能够提升模型性能，但同时也增大了对加速器内存、算力及通信传输带宽的要求。为了减少内存占用加快收敛速度，LLM训练往往采用16位半精度浮点格式，例如float16或者bfloat16。

大量实验已经证明可以使用半精度浮点数训练大模型，也不会对模型性能带来显著影响，相反低精度计算作为正则化的一部分，反而能够给模型泛化能力带来好处。但目前低精度训练对模型的统计学影响也并不那么清晰，所以整个训练过程单纯使用低精度浮点运算非常具有挑战性。

PyTorch提供了自动混合精度（AMP）的机制，AMP按需自动调整张量的数据类型（dtype）。例如在AMP autocast上下文时，矩阵乘法matmul的输入张量会被自动转化为半精度浮点类型。AMP也提供了GradScaler，通过自动调整Loss的缩放来防止梯度的下溢和上溢。

PyTorch的AMP优化级别使用apex.amp的O1级，这意味着PyTorch AMP使用黑白名单自动决定使用FP16、BF16还是FP32进行计算，但还有一些特定模型相关的精度敏感的运算并不在AMP的自动upcast名单中，需要开发者手动干预。

---

不同算子对计算精度的要求：

Softmax：softmax中有指数运算，容易发生上溢出和下溢出，并且分母有累加操作，目前主流的算子库只使用FP32来实现softmax。

Cross Entropy：cross_entropy用来计算交叉熵损失，和很多损失函数一样，需要使用FP32计算以保持数值稳定性，所以算子实现只接受FP32输入。

Matmul：matmul一般来说可以使用半精度数，但在大语言模型的attention层，attention分数与V张量的矩阵乘法需要用FP32，以便维持数值稳定性，避免上溢，减小累积错误。

layer_norm、Batch_norm、RMSNorm：需要用FP32计算。layer_norm要计算方差，有累加操作，如果用FP16，则计算过程中容易发生上溢和比较大的误差，从而影响最后的收敛。Batch_norm与layer_norm类似，也需要将输入张量upcast到FP32计算。RMSNorm是layer_norm的扩展，在开源LLAMA模型中使用，PyTorch目前并没有提供RMSNorm的实现。如果开发者自己实现，需要注意将输入张量强制upcast到FP32计算，结果可以转回半精度数，主要原因也是它需要计算L2均值, 涉及累加操作，低精度运算容易累积误差。

Conv/Pool：卷积操作属于逐单元矩阵乘法，对精度不敏感，可以使用BF16，不会对模型收敛性产生影响。和卷积类似的还有池化，同样可以使用BF16。ReLU和GeLU属于逐点运算，同样对精度不敏感。

RNNs：RNN一般对精度比较敏感 。LSTM-cell对精度不敏感，可以用BF16。

模型起始与结束层：通常大语言模型的第一层和最后一层对精度敏感，比如GPT的embedding层（词嵌入和位置嵌入层）需要使用FP32。一般的输出层即便不是softmax，计算和输出结果也需要是FP32。

通信算子：涉及到梯度或者激活值累加的通信算子如all_reduce、reduce_scatter，都需要把输入张量upcast到FP32以保证数值稳定性。

批量大小与精度的关系：大规模网络中，batch越大，对数值精度越敏感，所以当batch加大引起收敛问题时，要优先考虑某些运算是否有数值稳定性问题。

浮点精度问题：在GLM-130B等大型模型训练中，浮点精度问题引起的模型Loss起飞常常找不到明确的原因，有些会自动恢复，有些会有GNorm起飞的前兆随之有Loss起飞甚至NaN Loss。此时一种有效策略是跳过异常步的数据或者调整超参数。

BF16训练：用BF16训练深度网络，如果发现不收敛现象，应该尝试使用权重衰减技术，例如设置PyTorch adam优化器的weight_decay超参数（0.1）。

大语言模型预训练中的混合精度：在大语言模型预训练过程中，对于未知的混合精度引起的数值稳定性问题，GLM-130B训练过程中一个可以借鉴的经验是将embedding层的梯度收缩到原来的0.1，或者对embedding层做特别的梯度裁剪。

---

pytorch的AMP库使用示例：

```python
model, optimizer = amp.initialize(model, optimizer, opt_level="O1")
with amp.scale_loss(loss, optimizer) as scaled_loss:
    scaled_loss.backward()
```

---

https://tensorflow.google.cn/guide/mixed_precision

tensorflow/python/keras/mixed_precision/loss_scale_optimizer.py

https://mp.weixin.qq.com/s/xnszH9WSKGBwqtHUuYua1g

混合精度训练，提速，减内存

https://zhuanlan.zhihu.com/p/56114254

PAI自动混合精度训练---TensorCore硬件加速单元在阿里PAI平台落地应用实践

https://mp.weixin.qq.com/s/zBtpwrQ5HtI6uzYOx5VsCQ

模型训练太慢？显存不够用？这个算法让你的GPU老树开新花

https://mp.weixin.qq.com/s/cYGMZuY7jSrjhUAXlDwD_w

Mixed Precision Training

https://zhuanlan.zhihu.com/p/441591808

混合精度训练原理

https://mp.weixin.qq.com/s/uUxwMFGF9nJiraVQsIqu2Q

PyTorch重大更新：将支持自动混合精度训练！

# 参考

https://mp.weixin.qq.com/s/M79xGWWtJUB6wBVlHXw8ig

低精度神经网络：从数值计算角度优化模型效率

https://www.chiphell.com/thread-1620755-1-1.html

新Titan X的INT8计算到底是什么鬼

https://mp.weixin.qq.com/s/5LhLbzyWTlP2R_zGAIKuiA

INT8量化训练

https://mp.weixin.qq.com/s/S9VcoS_59nbZWe_P3ye2Tw

减少模型半数内存用量：百度&英伟达提出混合精度训练法

https://zhuanlan.zhihu.com/p/35700882

CNN量化技术

https://mp.weixin.qq.com/s/9DXMqiPIK5P5wzUMT7_Vfw

基于交替方向法的循环神经网络多比特量化

https://mp.weixin.qq.com/s/PDeChj1hQqUrZiepxXODJg

ICLR oral：清华提出离散化架构WAGE，神经网络训练推理合二为一

https://mp.weixin.qq.com/s/KgM1k1bziLTCec67hQ8hlQ

超全总结：神经网络加速之量化模型

https://mp.weixin.qq.com/s/7dzQhgblEm-kzRnpddweSw

嵌入式端CNN网络计算的量化-动态定点法（1）

https://mp.weixin.qq.com/s/M3NcH30zY5Wlj76BDPQlMA

模型压缩一半，精度几乎无损，TensorFlow推出半精度浮点量化工具包，还有在线Demo

https://www.zhihu.com/question/498135156

如何看待FAIR提出的8-bit optimizer：效果和32-bit optimizer相当？

https://mp.weixin.qq.com/s/D3ZKidCV7OhAeqWqWg521w

如何训练和部署FP16/Int8等低精度机器学习模型?

https://jackwish.net/neural-network-quantization-introduction-chn.html

神经网络量化简介

https://mp.weixin.qq.com/s/70GuFnJGhtIZEA-PECHjaA

混合精度对模型训练和推理的影响

https://mp.weixin.qq.com/s/xIbF3rNv2mC2G4RBDhIvJQ

哈佛大学在读博士：模型量化——更小更快更强

https://zhuanlan.zhihu.com/p/128018221

8比特数值也能训练模型？商汤提出训练加速新算法

https://zhuanlan.zhihu.com/p/132561405

模型量化了解一下？

https://mp.weixin.qq.com/s/YImszcJDsvw5ygo2wCj3Hw

模型量化的核心技术点有哪些，如何对其进行长期深入学习

https://mp.weixin.qq.com/s/bK0n9u6DIl4SY7mxS8CVRw

模型量化技术原理及其发展现状和展望

https://zhuanlan.zhihu.com/p/223018242

NNIE量化算法及实现

https://zhuanlan.zhihu.com/p/79744430

Tensorflow模型量化(Quantization)原理及其实现方法

https://mp.weixin.qq.com/s/du3hb2oM5X6bMocdOab4dg

模型量化: 只有整数计算的高效推理

https://mp.weixin.qq.com/s/7Si6GQlj8IvYajoVnwm5DQ

INT4量化用于目标检测

https://mp.weixin.qq.com/s/7VEiQ0y8kB4nODtLCx1UQA

模型量化打怪升级之路

https://mp.weixin.qq.com/s/TXWdx3bbBNfaG3yp2G56ew

提速还能不掉点！深度解析MegEngine 4 bits量化开源实现

https://www.zhihu.com/question/627484732

目前针对大模型进行量化的方法有哪些？
