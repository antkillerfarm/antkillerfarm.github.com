---
layout: post
title:  深度加速（四）——模型压缩与加速
category: DL acceleration 
---

# NN Quantization

## bfloat16（续）

论文：

《Mixed Precision Training》

参考：

https://www.zhihu.com/question/275682777

如何评价Google在TensorFlow中引入的bfloat16数据类型？

https://zhuanlan.zhihu.com/p/56114254

PAI自动混合精度训练---TensorCore硬件加速单元在阿里PAI平台落地应用实践

https://mp.weixin.qq.com/s/zBtpwrQ5HtI6uzYOx5VsCQ

模型训练太慢？显存不够用？这个算法让你的GPU老树开新花

## Flexpoint

Flexpoint是Nervana的作品。

论文：

《Flexpoint: An Adaptive Numerical Format for Efficient Training of Deep Neural Networks》

讲了Google的成功案例，这里来讲一个反面教材。

![](/images/img3/flex.png)

这实际上就是INT16的量化，用在inference上应该还是可以的，然而Nervana的目标还有training。

和bfloat16相比，它至少有如下问题：

- 格式转换比bfloat16复杂。

- Dynamic Range小，容易梯度消失，从而造成模型很难收敛。从指数位宽来看，Flexpoint和float16相同，都是5位。然而由于Flexpoint是共享指数，因此它真正的Dynamic Range是不如float16的。float16已经被证明是不适合training的，更遑论Flexpoint了。

事实上，Intel内部已有人评价道：

>Flexpoint16三个月converge不了一个网络，而BF16一天就可以converge三个。

- 指数保存在Host上，会造成反复通信的带宽问题。

总的来说，这个方案虽然精巧，但是由于没有对数据特点做充分分析，没有意识到**Dynamic Range比底数精度更重要**，从而导致了最终的失败。

参考：

https://www.intel.ai/flexpoint-numerical-innovation-underlying-intel-nervana-neural-network-processor/

Flexpoint: Numerical Innovation Underlying the Intel Nervana Neural Network Processor

https://zhuanlan.zhihu.com/p/33580205

Flexpoint——利用一种自适应的数据类型加速神经网络训练

https://mp.weixin.qq.com/s/z4OEPrAAtaNmBQoyvEd7Nw

从春秋到战国—论Nervana的倒掉

## TF32

![](/images/img3/tf32.png)

这是Nvidia推出的格式，有BF16珠玉在前，这个的设计只能说中规中矩了。

优点：底数精度虽然不如Dynamic Range重要，但对于运算结果还是有一定的影响的。这点在CNN中不太显著，但在RNN/Transformer中还是有所体现的。

缺点：毕竟不是16位，运算速度只有FP16/BF16的一半，但比FP32快一些。

参考：

https://mp.weixin.qq.com/s/cGKtvtZzR--sGL4oNSZfAw

深度分析NVIDIA A100显卡架构

https://zhuanlan.zhihu.com/p/143499632

NVIDIA A100 GPU中的TF32将AI训练与HPC速度提升20倍

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

## 参考

https://mp.weixin.qq.com/s/M79xGWWtJUB6wBVlHXw8ig

低精度神经网络：从数值计算角度优化模型效率

https://www.chiphell.com/thread-1620755-1-1.html

新Titan X的INT8计算到底是什么鬼

https://mp.weixin.qq.com/s/S9VcoS_59nbZWe_P3ye2Tw

减少模型半数内存用量：百度&英伟达提出混合精度训练法

https://zhuanlan.zhihu.com/p/35700882

CNN量化技术

https://mp.weixin.qq.com/s/9DXMqiPIK5P5wzUMT7_Vfw

基于交替方向法的循环神经网络多比特量化

https://mp.weixin.qq.com/s/PDeChj1hQqUrZiepxXODJg

ICLR oral：清华提出离散化架构WAGE，神经网络训练推理合二为一

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

https://mp.weixin.qq.com/s/lVja7woyFWpmr9sH0CitAA

BMXNet：基于MXNet的开源二值神经网络实现

https://mp.weixin.qq.com/s/naDk0mmxd08dNl9LawLUnw

不使用先验知识与复杂训练策略，从头训练二值神经网络！

https://mp.weixin.qq.com/s/Xvlxs-Os2meduHrEQFc7vg

第一次胜过MobileNet的二值神经网络，-1与+1的三年艰苦跋涉

https://mp.weixin.qq.com/s/tbRj5Wd69n9gvSzW4oKStg

异或神经网络

https://mp.weixin.qq.com/s/KgM1k1bziLTCec67hQ8hlQ

超全总结：神经网络加速之量化模型

https://mp.weixin.qq.com/s/7dzQhgblEm-kzRnpddweSw

嵌入式端CNN网络计算的量化-动态定点法（1）

https://mp.weixin.qq.com/s/XzLJzfvpP93cDYplf6-LXA

港科腾讯等提出Bi-Real net：超XNOR-net 10%的ImageNet分类精度

https://mp.weixin.qq.com/s/wCx7rQFwC2mW45FMR77tGQ

二值网络，围绕STE的那些事儿

https://mp.weixin.qq.com/s/M3NcH30zY5Wlj76BDPQlMA

模型压缩一半，精度几乎无损，TensorFlow推出半精度浮点量化工具包，还有在线Demo

https://mp.weixin.qq.com/s/7L26ghhDqdMU6LRV0iD6vQ

模型量化从1bit到8bit，二值到三值

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

https://mp.weixin.qq.com/s/xnszH9WSKGBwqtHUuYua1g

混合精度训练，提速，减内存

https://mp.weixin.qq.com/s/YImszcJDsvw5ygo2wCj3Hw

模型量化的核心技术点有哪些，如何对其进行长期深入学习

https://mp.weixin.qq.com/s/bK0n9u6DIl4SY7mxS8CVRw

模型量化技术原理及其发展现状和展望

# 模型压缩与加速

对于AI应用端而言，由于设备普遍没有模型训练端的性能那么给力，因此如何压缩模型，节省计算的时间和空间就成为一个重要的课题。

此外，对于一些较大的模型（如VGG），即使机器再给力，单位时间内能处理的图像数量，往往也无法达到实际应用的要求。这点在自动驾驶和视频处理领域显得尤为突出。

## 课程

https://cs217.github.io/

CS 217: Hardware Accelerators for Machine

https://mp.weixin.qq.com/s/RcEPWRxQXv6B4wqLHGyQHg

深度神经网络的高效处理:从算法到硬件架构，140页ppt

https://mp.weixin.qq.com/s/yp5gExPzpDiXaGk9oXEMVA

最新综述：模型压缩与加速

https://mp.weixin.qq.com/s/PraNMo4skR-VjEYIIqt1Cw

深度学习模型压缩与加速综述

https://mp.weixin.qq.com/s/Xqc4UgcfCUWYOeGhjNpidA

CNN模型压缩与加速算法综述

## 复杂度分析

https://zhuanlan.zhihu.com/p/31575074

卷积神经网络的复杂度分析

## Network Pruning

首先是韩松的两篇论文：

《Deep Compression: Compressing Deep Neural Networks with Pruning, Trained Quantization and Huffman Coding》

《Learning both Weights and Connections for Efficient Neural Networks》

>韩松，清华本科（2012）+Stanford博士（2017）。MIT AP（from 2018）。   
>个人主页：   
>https://stanford.edu/~songhan/

韩松也是SqueezeNet的二作。

![](/images/article/nn_compression.png)

韩松论文的中心思想如上图所示。简单来说，就是去掉原有模型的一些不重要的参数、结点和层。

参数的选择，相对比较简单。参数的绝对值越接近零，它对结果的贡献就越小。这一点和稀疏矩阵有些类似。这种方法一般被称为Weight Pruning。

结点和层的选择，相对麻烦一些，需要通过算法得到不重要的层。删除结点一般被称为Filter Pruning，而删除层则相应的被称作Layer Pruning。

比如可以逐个将每一层50%的参数置零，查看模型性能。对性能影响不大的层就是不重要的。

Weight Pruning需要相关硬件支持跳零操作才能真正加速运算，而Filter/Layer Pruning则无需特殊硬件支持。

虽然这些参数、结点和层相对不重要，但是去掉之后，仍然会对准确度有所影响。这时可以对精简之后的模型，用训练样本进行re-train，通过残差对模型进行一定程度的修正，以提高准确度。
