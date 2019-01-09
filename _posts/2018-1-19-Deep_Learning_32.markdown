---
layout: post
title:  深度学习（三十二）——信息检索, NN Quantization
category: DL 
---

# 信息检索

Information Retrieval是用户进行信息查询和获取的主要方式，是查找信息的方法和手段。狭义的信息检索仅指信息查询（Information Search）。即用户根据需要，采用一定的方法，借助检索工具，从信息集合中找出所需要信息的查找过程。广义的信息检索是信息按一定的方式进行加工、整理、组织并存储起来，再根据信息用户特定的需要将相关信息准确的查找出来的过程。

这方面的DL应用可参见以下的综述文章：

《MatchZoo: A Toolkit for Deep Text Matching》

## ARC-I & ARC-II

《Convolutional neural network architectures for matching natural language sentences》

## DSSM

《Learning deep structured semantic models for web search using clickthrough data》

## CDSSM

《Learning semantic representations using convolutional neural networks for web search》

## MV-LSTM

《A deep architecture for semantic matching with multiple positional sentence representations》

## CNTN

《Convolutional Neural Tensor Network Architecture for Community-Based Question Answering》

## DRMM

《A deep relevance matching model for ad-hoc retrieval》

## MatchPyramid

《Text Matching as Image Recognition》

## Match-SRNN

《Match-SRNN: Modeling the Recursive Matching Structure with Spatial RNN》

## K-NRM

《End-to-End Neural Ad-hoc Ranking with Kernel Pooling》

## 参考

https://mp.weixin.qq.com/s/aZsj1FQnzHOr-YBcy_ljpw

DNN在搜索场景中的应用

https://mp.weixin.qq.com/s/1jgdI-Pt0PtN3oAs0Wh4XA

阿里提出电商搜索全局排序方法，淘宝无线主搜GMV提升5%

https://mp.weixin.qq.com/s/9Fcj5lO-JPfFVnRSSM_56w

深度学习在美团搜索广告排序的应用实践

https://mp.weixin.qq.com/s/wni3F9lKuO4OT32BVe0QDQ

谷歌发大招：搜索全面AI化，不用关键词就能轻松“撩书”

https://mp.weixin.qq.com/s/TrWwp-DBTrKqIT_Pfy_o5w

阿里妈妈首次公开新一代智能广告检索模型，重新定义传统搜索框架

https://mp.weixin.qq.com/s/fZv9FgbdQ1bWPoNdl9sF1A

“宝石迷阵”与信息检索

https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247488366&idx=1&sn=01baaf8b6c6a2c727bb9e0e2101f803b

电商搜索算法技术的演进

https://mp.weixin.qq.com/s/MpuUdZi8CWcu0b-ij-bHjA

Jeff Dean出品：用机器学习索引替代B-Trees，3倍性能提升，10-100倍空间缩小

https://mp.weixin.qq.com/s/uztYEW_azetOkOGiZcbCuw

JeffDean又用深度学习搞事情：这次要颠覆整个计算机系统结构设计。这篇blog介绍了如何用DL方法提高内存访问的命中率。

https://zhuanlan.zhihu.com/p/37020639

读论文系列：CVPR2018 SSAH

https://mp.weixin.qq.com/s/TdnstQaBcLaXg8BvuR7oYA

基于素描图的细粒度图像检索

https://mp.weixin.qq.com/s/N3JBHlqneG9dI0I26M3wHQ

如何做好大规模视觉搜索？eBay基于实践总结出了7条建议

# NN Quantization

## 概述

NN的量化计算是近来NN计算优化的方向之一。相比于传统的浮点计算，整数计算无疑速度更快，而NN由于自身特性，对单点计算的精确度要求不高，且损失的精度还可以通过retrain的方式恢复大部分，因此通常的科学计算的硬件（没错就是指的GPU）并不太适合NN运算，尤其是NN Inference。

>传统的GPU并不适合NN运算，因此Nvidia也好，还是其他GPU厂商也好，通常都在GPU中又集成了NN加速的硬件，因此虽然商品名还是叫做GPU，但是工作原理已经有别于传统的GPU了。

这方面的文章以Xilinx的白皮书较为经典：

https://china.xilinx.com/support/documentation/white_papers/c_wp486-deep-learning-int8.pdf

利用Xilinx器件的INT8优化开展深度学习

## Distiller

https://nervanasystems.github.io/distiller/index.html

Intel AI Lab推出的Distiller是一个关于模型压缩、量化的工具包。这里是它的文档，总结了业界主要使用的各种方法。

## Conservative vs. Aggressive

Quantization主要分为两大类：

1."Conservative" Quantization。这里主要指不低于INT8精度的量化。

实践表明，由于NN训练时采用的凸优化算法，其最终结果一般仅是局部最优。因此，即便是两次训练（数据集、模型完全相同，样本训练顺序、参数初始值随机）之间的差异，通常也远大于FP64的精度。所以，一般而言，FP32对于模型训练已经完全够用了。

FP16相对于FP32，通常会有不到1%的精度损失。即使是不re-train的INT8，通常也只有3%～15%的精度损失。因此这类量化被归为"Conservative" Quantization。其特点是完全采用FP32的参数进行量化，或者在此基础上进行re-train。

1."Aggressive" Quantization。这里指的是INT4或更低精度的量化。

这种量化由于过于激进，re-train也没啥大用，因此必须从头训练。而且由于INT4表达能力有限，模型结构也要进行一定的修改，比如增加每一层的filter的数量。

## INT量化

论文：

《On the efficient representation and execution of deep acoustic models》

![](/images/img2/INT8.png)

一个浮点数包括底数和指数两部分。将两者分开，就得到了一般的INT量化。

量化的过程一般如下：

1.使用一批样本进行推断，记录下每个layer的数值范围。

2.根据该范围进行量化。

量化的方法又分为两种：

1）直接使用浮点数表示中的指数。也就是所谓的fractional length，相当于2的整数幂。

2）使用更一般的scale来表示。这种方式的精度较高，但运算量稍大。

量化误差过大，一般可用以下方法减小：

1.按照每个channel的数值范围，分别量化。

2.分析weight、bias，找到异常值，并消除之。这些异常值通常是由于死去的神经元所导致的误差无法更新造成的。

如何确定每个layer的数值范围，实际上也有多种方法：

1.取整批样本在该layer的数值范围的并集，也就是所有最大（小）值的极值。

2.取所有最大（小）值的平均值。

## UINT量化

论文：

《Quantization and Training of Neural Networks for Efficient Integer-Arithmetic-Only Inference》

![](/images/img2/INT8_2.png)

UINT量化使用bias将数据搬移到均值为0的区间。

这篇论文的另一个贡献在于：原先的INT8量化是针对已经训练好的模型。而现在还可以在训练的时候就进行量化——前向计算进行量化，而反向的误差修正不做量化。

## Saturate Quantization

上述各种量化方法都是在保证数值表示范围的情况下，尽可能提高fl或者scale。这种方法也叫做Non-saturation Quantization。

NVIDIA在如下文章中提出了一种新方法：

http://on-demand.gputechconf.com/gtc/2017/presentation/s7310-8-bit-inference-with-tensorrt.pdf

8-bit Inference with TensorRT

![](/images/img2/INT8_3.png)

Saturate Quantization的做法是：将超出上限或下限的值，设置为上限值或下限值。

如何设置合理的Saturate threshold呢？

可以设置一组门限，然后计算每个门限的分布和原分布的相似度，即KL散度。然后选择最相似分布的门限即可。

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

https://mp.weixin.qq.com/s/tbRj5Wd69n9gvSzW4oKStg

异或神经网络

https://mp.weixin.qq.com/s/KgM1k1bziLTCec67hQ8hlQ

超全总结：神经网络加速之量化模型

https://mp.weixin.qq.com/s/7dzQhgblEm-kzRnpddweSw

嵌入式端CNN网络计算的量化-动态定点法（1）

https://mp.weixin.qq.com/s/XzLJzfvpP93cDYplf6-LXA

港科腾讯等提出Bi-Real net：超XNOR-net 10%的ImageNet分类精度
