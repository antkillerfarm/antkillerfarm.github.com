---
layout: post
title:  深度加速（三）——Winograd（3）, NN Quantization
category: DL acceleration 
---

# Winograd

## FFT与卷积

FFT是加速卷积运算的一种常用方法。但由于其原理，当卷积核小的时候，是没什么加速的，当核是3或者5时，速度有时更慢或者相当，而在CNN中卷积的核大多数比较小，FFT起到的加速作用很小，所以基本没人用。

此外，FFT是复数运算，如果没有特殊硬件，而用实数计算的话，还是比较费劲的。

![](/images/img2/Integer_multiplication_FFT.png)

说句题外话，上图是FFT进行整数乘法的示例图。对于超大整数，2维的FFT也是远远不够的。例如2019年2月，David Harvey和Van Der Hoeven就使用了1729维的FFT，计算了两个10亿位的超大整数的乘法。

参见：

http://www.cnblogs.com/jianyingzhou/p/4303578.html

convolution,fft, 加速

http://www.cnblogs.com/luoqingyu/p/5930181.html

数字信号处理--FFT与蝶形算法

http://blog.csdn.net/xxinliu/article/details/7438429

Cooley-Tukey算法 （蝶形算法）

https://www.zhihu.com/question/264307400

为什么很少人用FFT加速CNN卷积层的运算？

## Strassen algorithm

![](/images/img3/Strassen.png)

上图展示的是Strassen algorithm的步骤，该算法也是在矩阵运算加速领域比较知名的算法。

然而，实际的情况要比理论计算复杂的多。比如有的硬件支持加法/乘法的跳零（忽略对0的加法/乘法）操作，这时无论是Strassen还是Winograd都不会有太好的效果。

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

![](/images/img2/bfloat16.jpg)

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
