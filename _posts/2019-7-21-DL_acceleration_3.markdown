---
layout: post
title:  深度加速（三）——Winograd（3）, NN Quantization
category: DL acceleration 
---

* toc
{:toc}

# Winograd

## Winograd for CNN（续）

https://github.com/andravin/wincnn

这个项目可以很方便的计算不同大小的核的Winograd的结果。这个项目中还有一个pdf文件作为上述论文的补充材料，详细的给出了各矩阵的计算方法。

论文：

《Efficient Sparse-Winograd Convolutional Neural Networks》

![](/images/img3/Winograd.png)

这篇论文（2018.2）讨论了如何在稀疏矩阵中应用Winograd算法。

论文：

《Sparse Winograd Convolutional neural networks on small-scale systolic arrays》

![](/images/img3/Winograd_3.png)

这篇论文（2018.10）讨论了如何在脉动阵列上实现Winograd算法，还讨论了3D卷积的计算方法。

![](/images/img3/Winograd_2.png)

参考：

http://shuokay.com/2018/02/21/winograd/

Winograd方法快速计算卷积

## FFT与卷积

FFT是加速卷积运算的一种常用方法。但由于其原理，当卷积核小的时候，是没什么加速的，当核是3或者5时，速度有时更慢或者相当，而在CNN中卷积的核大多数比较小，FFT起到的加速作用很小，所以基本没人用。

此外，FFT是复数运算，如果没有特殊硬件，而用实数计算的话，还是比较费劲的。

![](/images/img2/Integer_multiplication_FFT.png)

说句题外话，上图是FFT进行整数乘法的示例图。对于超大整数，2维的FFT是远远不够的。例如2019年2月，David Harvey和Van Der Hoeven就使用了1729维的FFT，计算了两个10亿位的超大整数的乘法。

参见：

http://www.cnblogs.com/jianyingzhou/p/4303578.html

convolution,fft, 加速

http://www.cnblogs.com/luoqingyu/p/5930181.html

数字信号处理--FFT与蝶形算法

http://blog.csdn.net/xxinliu/article/details/7438429

Cooley-Tukey算法 （蝶形算法）

https://www.zhihu.com/question/264307400

为什么很少人用FFT加速CNN卷积层的运算？

https://zhuanlan.zhihu.com/p/80297169

快速数论变换（NTT）及蝴蝶操作构造详解

https://mp.weixin.qq.com/s/fLGCBeSc8yQVsXHLSdogrA

柯尔莫哥洛夫惨遭打脸，没想到这个乘法难题被澳大利亚数学家解决了！

## Strassen algorithm

![](/images/img3/Strassen.png)

上图展示的是Strassen algorithm的步骤，该算法也是在矩阵运算加速领域比较知名的算法。

然而，实际的情况要比理论计算复杂的多。比如有的硬件支持加法/乘法的跳零（忽略对0的加法/乘法）操作，这时无论是Strassen还是Winograd都不会有太好的效果。

## NCHWc layout

常见的NCHW和NHWC两种layout，在硬件加速方面各有所长。因此，后来又出现了NCHWc这样的layout。

![](/images/img3/NCHWc.jpg)

这种layout将C分成了两部分，局部的C放在了最后面，也就是NCHWc中的小写的c。这里c=4，因此又叫做NCHWc4。类似的还有NCHWc8。如果C不能被4整除的话，不足部分用0 pad即可。

c的大小主要由运算单元的block计算能力决定。比如Tensor core的尺寸，SIMD指令集单次运算的操作数个数等。

NCHWc对于CPU的SIMD指令集比较友好，因此被Intel广泛使用。

论文：

《Optimizing N-Dimensional, Winograd-Based Convolution for Manycore CPUs》

参考：

https://www.zhihu.com/question/337513515

Tensor中数据摆放顺序NC4HW4是什么意思，只知道NCHW格式，能解释以下NC4HW4格式吗？

https://mp.weixin.qq.com/s/1CToXRgyO0F8x0By31dneg

图解神秘的NC4HW4

## 参考

https://colfaxresearch.com/falcon-library/

FALCON Library: Fast Image Convolution in Neural Networks on Intel Architecture

https://www.intelnervana.com/winograd/

"Not so fast, FFT": Winograd

https://www.encyclopediaofmath.org/index.php/Winograd_small_convolution_algorithm

Winograd small convolution algorithm

https://mp.weixin.qq.com/s/VcGwT0rKYQr13MQ_PRVmyg

每个AI程序员都应该知道的基础数论

https://mp.weixin.qq.com/s/jCPHXVI_utwnxaHqvW5nyA

商汤联合提出基于FPGA的快速Winograd算法：实现FPGA之上最优的CNN表现与能耗

https://www.leiphone.com/news/201804/SpmfJVa3al3uBDh2.html

斯坦福ICLR 2018录用论文：高效稀疏Winograd卷积神经网络

https://www.cnblogs.com/shine-lee/p/10906535.html

卷积神经网络中的Winograd快速卷积算法

https://mp.weixin.qq.com/s/RaW_WVKoLBk6jkoA6A3D-A

如何实现高速卷积？深度学习库使用了这些“黑魔法”

https://mp.weixin.qq.com/s/1JVPTv0ggD0IOEiZAa8mqA

深度神经网络卷积层计算加速与优化

https://zhuanlan.zhihu.com/p/80361782

卷积神经网络性能优化

https://mp.weixin.qq.com/s/Ji2PjZowifkIdGlHJPwEaw

解析卷积的高速计算中的细节，一步步代码带你飞

https://zhuanlan.zhihu.com/p/102351953

详解Winograd变换矩阵生成原理

# NN Quantization

![](/images/img3/NN_Quantization.jpg)

## 概述

NN的量化计算是近来NN计算优化的方向之一。相比于传统的浮点计算，整数计算无疑速度更快，而NN由于自身特性，对单点计算的精确度要求不高，且损失的精度还可以通过retrain的方式恢复大部分，因此通常的科学计算的硬件（没错就是指的GPU）并不太适合NN运算，尤其是NN Inference。

>传统的GPU并不适合NN运算，因此Nvidia也好，还是其他GPU厂商也好，通常都在GPU中又集成了NN加速的硬件，因此虽然商品名还是叫做GPU，但是工作原理已经有别于传统的GPU了。

这方面的文章以Xilinx的白皮书较为经典：

https://china.xilinx.com/support/documentation/white_papers/c_wp486-deep-learning-int8.pdf

利用Xilinx器件的INT8优化开展深度学习

## IEEE 754

在开始进一步介绍之前，我们首先回顾一下浮点数在硬件上的表示方法。其中最重要的就是IEEE 754标准。

|  | Sign | Exponent | Significand |
|:--:|:--:|:--:|:--:|
| FP16 | 1 | 5 | 10 |
| FP32 | 1 | 8 | 23 |
| FP64 | 1 | 11 | 52 |
| FP128 | 1 | 15 | 113 |
| FP256 | 1 | 19 | 237 |

![](/images/img3/float.jpg)

目前，clang编译器已经原生支持FP16，但gcc还不行，不过可以用以下项目支持之：

http://half.sourceforge.net/

half - IEEE 754-based half-precision floating point library

显然，Significand的位数决定Accuracy，而Exponent的位数决定Dynamic Range。

上溢：超出所能表示的最大数（$$\to \infty$$）。

下溢：超出所能表示的最小数（$$\to 0$$）。

除了IEEE 754之外，还有IBM hexadecimal floating point。相比于IEEE 754，IBM格式的Significand位数多一些，而Exponent的位数少一些。

参考：

https://en.wikipedia.org/wiki/IEEE_754

http://blog.codinglabs.org/articles/trouble-of-x86-64-platform.html

x86-64体系下一个奇怪问题的定位

## Microsoft Binary Format

MS早期的Basic产品中使用了一种特殊的浮点表示方法，被称作Microsoft Binary Format（1975年）。与后来的浮点标准主要由硬件厂商主导不同，这时的浮点运算还是以软件的形式运行在定点运算单元上。

## 浮点数取整

浮点数取整的方法一般有：

- 截断取整。

- 向上/向下取整。

- 四舍五入。

- 四舍六入五成双，也称为向偶数取整。例子：3.5->4, 4.5->4.

一般来说，后两种用的较多，尤其是最后一种。

## Distiller

https://nervanasystems.github.io/distiller/index.html

Intel AI Lab推出的Distiller是一个关于模型压缩、量化的工具包。这里是它的文档，总结了业界主要使用的各种方法。

参考：

https://mp.weixin.qq.com/s/A5ka8evElmcuHdowof7kww

Intel发布神经网络压缩库Distiller：快速利用前沿算法压缩PyTorch模型

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

## bfloat16

bfloat16是Google针对AI领域的特殊情况提出的浮点格式。目前已有Intel的AI processors和Google的TPU，提供对该格式的原生支持。

![](/images/img3/bfloat16.png)

上图比较了bfloat16和IEEE fp32/fp16的差异。可以看出bfloat16有如下特点：

1.bfloat16可以直接截取float32的前16位得到，所以在float32和bfloat16之间进行转换时非常容易。

2.bfloat16的Dynamic Range比float16大，不容易下溢。这点在training阶段更为重要，梯度一般都挺小的，一旦下溢变成0，就传递不了了。

3.bfloat16既可以用于训练又可以用于推断。Amazon也证明Deep Speech模型使用BFloat的训练和推断的效果都足够好。Uint8在大部分情况下不能用于训练，只能用于推断。
