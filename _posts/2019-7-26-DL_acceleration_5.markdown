---
layout: post
title:  深度加速（五）——模型压缩与加速
category: DL acceleration 
---

* toc
{:toc}

# NN Quantization

## 量化技巧（续）

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

https://zhuanlan.zhihu.com/p/431680710

谈谈BNN二值化神经网络的设计，以及几代学界工作的演进 -（1）架构与原理

https://zhuanlan.zhihu.com/p/433429767

谈谈BNN二值化神经网络的设计，以及几代学界工作的演进 -（2）二值训练

https://zhuanlan.zhihu.com/p/435285316

谈谈BNN二值化神经网络的设计，以及几代学界工作的演进 -（3）二值化设计法则、推理框架与发展潜力

https://mp.weixin.qq.com/s/lVja7woyFWpmr9sH0CitAA

BMXNet：基于MXNet的开源二值神经网络实现

https://mp.weixin.qq.com/s/naDk0mmxd08dNl9LawLUnw

不使用先验知识与复杂训练策略，从头训练二值神经网络！

## 参考

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

https://mp.weixin.qq.com/s/xnszH9WSKGBwqtHUuYua1g

混合精度训练，提速，减内存

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

![](/images/img4/Pruning.png)

此外还有Stripe-Wise Pruning：

https://mp.weixin.qq.com/s/HohsD57cQtTR5SvuykEDuA

优图NeurIPS 2020论文，刷新滤波器剪枝的SOTA效果

还可以看看图森科技的论文：

https://www.zhihu.com/question/62068158

如何评价图森科技连发的三篇关于深度模型压缩的文章？

图森的思路比较有意思。其中的方法之一，是利用L1规则化会导致结果的稀疏化的特性，制造出一批接近0的参数。从而达到去除不重要的参数的目的。

除此之外，矩阵量化、Kronecker内积、霍夫曼编码、模型剪枝等也是常见的模型压缩方法。

---

彩票假说（ICLR2019会议的best paper）：随机初始化的密集神经网络包含一个初始化的子网，当经过隔离训练时，它可以匹配训练后最多相同迭代次数的原始网络的测试精度。

https://mp.weixin.qq.com/s/wOaCjSifZqkndaGbst1-aw

一文带你了解NeurlPS2020的模型剪枝研究

## 权值稀疏化实战

这里讲一下韩松论文提到的裁剪方法中，最简单的一种——“权值稀疏化“的工程实现细节。以darknet框架为例。

1.在src/parser.c中找到save_XXX_weights函数。判断权值是否接近0，如果是，则强制设为0。

2.使用修改后的weights进行re-train。训练好之后，重复第1、2步。

3.反复多次之后，进入最终prune阶段。修改src/network.c:update_network，令其不更新0权值。

>re-train时的learning rate一般不宜太大。如果出现re-train的效果，还不如直接prune的好，则多半是learning rate设置的问题。

一般采用稀疏化率来描述权值的稀疏化程度。每层的稀疏化率可以相同，也可以不同。前者被称作Magnitude Pruner，而后者被称作Sensitivity Pruner。

权值稀疏化的设置也和网络结构有关。比如分类网络，由于输入图片是高维数据，而分类结果是低维数据，因此在稀疏化处理的时候，**越靠近输出结果的Layer，其稀疏化程度就可以越高。**而最初的几层，即使只加少量稀疏化，也会导致精度的大幅下降，这时往往就不做或者少做稀疏化处理了。

上述方法的问题在于，分类网络的计算量主要集中在最初几层，所以这种triangle prune mode对于压缩计算量的效果一般。
