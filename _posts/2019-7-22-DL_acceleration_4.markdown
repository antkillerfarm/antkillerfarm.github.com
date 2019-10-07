---
layout: post
title:  深度加速（四）——模型压缩与加速（1）
category: DL acceleration 
---

# NN Quantization

## 参考（续）

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

https://mp.weixin.qq.com/s/wCx7rQFwC2mW45FMR77tGQ

二值网络，围绕STE的那些事儿

https://mp.weixin.qq.com/s/M3NcH30zY5Wlj76BDPQlMA

模型压缩一半，精度几乎无损，TensorFlow推出半精度浮点量化工具包，还有在线Demo

https://mp.weixin.qq.com/s/7L26ghhDqdMU6LRV0iD6vQ

模型量化从1bit到8bit，二值到三值

https://mp.weixin.qq.com/s/D3ZKidCV7OhAeqWqWg521w

如何训练和部署FP16/Int8等低精度机器学习模型?

# 模型压缩与加速

对于AI应用端而言，由于设备普遍没有模型训练端的性能那么给力，因此如何压缩模型，节省计算的时间和空间就成为一个重要的课题。

此外，对于一些较大的模型（如VGG），即使机器再给力，单位时间内能处理的图像数量，往往也无法达到实际应用的要求。这点在自动驾驶和视频处理领域显得尤为突出。

课程：

https://cs217.github.io/

CS 217: Hardware Accelerators for Machine 

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

还可以看看图森科技的论文：

https://www.zhihu.com/question/62068158

如何评价图森科技连发的三篇关于深度模型压缩的文章？

图森的思路比较有意思。其中的方法之一，是利用L1规则化会导致结果的稀疏化的特性，制造出一批接近0的参数。从而达到去除不重要的参数的目的。

除此之外，矩阵量化、Kronecker内积、霍夫曼编码、模型剪枝等也是常见的模型压缩方法。

## 权值稀疏化实战

这里讲一下韩松论文提到的裁剪方法中，最简单的一种——“权值稀疏化“的工程实现细节。以darknet框架为例。

1.在src/parser.c中找到save_XXX_weights函数。判断权值是否接近0，如果是，则强制设为0。

2.使用修改后的weights进行re-train。训练好之后，重复第1、2步。

3.反复多次之后，进入最终prune阶段。修改src/network.c:update_network，令其不更新0权值。

>re-train时的learning rate一般不宜太大。如果出现re-train的效果，还不如直接prune的好，则多半是learning rate设置的问题。

一般采用稀疏化率来描述权值的稀疏化程度。每层的稀疏化率可以相同，也可以不同。前者被称作Magnitude Pruner，而后者被称作Sensitivity Pruner。

权值稀疏化的设置也和网络结构有关。比如分类网络，由于输入图片是高维数据，而分类结果是低维数据，因此在稀疏化处理的时候，**越靠近输出结果的Layer，其稀疏化程度就可以越高。**而最初的几层，即使只加少量稀疏化，也会导致精度的大幅下降，这时往往就不做或者少做稀疏化处理了。

上述方法的问题在于，分类网络的计算量主要集中在最初几层，所以这种triangle prune mode对于压缩计算量的效果一般。

除了训练后的权值稀疏化之外，权值稀疏化训练也是一种方法。

论文：

《FLOPs as a Direct Optimization Objective for Learning Sparse Neural Networks》

这篇论文，将计算量也就是FLOPs作为Loss function设计的一部分，由于稀疏化的权值没有运算量，因此，采用这种Loss训练出的网络，天生就是稀疏化的。

## AutoML

由于模型压缩，本质上是一个精益求精的优化问题，因此采用AutoML技术对于各个超参数进行优化，就成为了一件很有必要的事情。

这里主要的问题在于超参数数量众多，导致状态空间过大。

论文：

《AMC: AutoML for Model Compression and Acceleration on Mobile Devices》

这是韩松组的何宜晖的作品。该论文采用深度强化学习的DDPG网络来优化目标网络，从而大大减少了需要搜索的状态空间。

## 参考

https://mp.weixin.qq.com/s/yp5gExPzpDiXaGk9oXEMVA

最新综述：模型压缩与加速

https://github.com/memoiry/Awesome-model-compression-and-acceleration

模型压缩与加速相关资源汇总

https://blog.csdn.net/hw5226349/article/details/84888416

Deep Compression/Acceleration：模型压缩加速论文汇总

https://mp.weixin.qq.com/s/bndECrtEcNCkCF5EG0wO-A

移动端机器学习资源合集

https://zhuanlan.zhihu.com/p/58805980

深度学习的模型加速与模型裁剪方法

https://mp.weixin.qq.com/s/pAEoVs8xu0SY9tfBqOJHHA

Google DeepMind最新报告—深度神经网络压缩进展

https://mp.weixin.qq.com/s/PraNMo4skR-VjEYIIqt1Cw

深度学习模型压缩与加速综述

http://blog.csdn.net/shuzfan/article/details/51383809

神经网络压缩：Deep Compression

https://mp.weixin.qq.com/s/2NOFyu_twx1EciDeDPBLKw

深度神经网络加速与压缩

https://www.zhihu.com/question/50519680

如何理解soft target这一做法？

https://mp.weixin.qq.com/s/0KlnQ8UUxpyhBRdeo0EOAA

用于网络压缩的滤波器级别剪枝算法ThiNet

https://mp.weixin.qq.com/s/lO2UM04PfSM5VJYh6vINhw

为模型减减肥：谈谈移动／嵌入式端的深度学习

https://mp.weixin.qq.com/s/cIGuJvYr4lZW01TdINBJnA

深度压缩网络：较大程度减少了网络参数存储问题

https://mp.weixin.qq.com/s/1JwLP0FmV1AGJ65iDgLWQw

神经网络模型压缩技术

https://mp.weixin.qq.com/s/Xqc4UgcfCUWYOeGhjNpidA

CNN模型压缩与加速算法综述

https://mp.weixin.qq.com/s/rzv8VCAxBQi0HsUcnLqqUA

处理移动端传感器时序数据的深度学习框架：DeepSense

https://mp.weixin.qq.com/s/UYk3YQmFW7-44RUojUqfGg

上交大ICCV：精度保证下的新型深度网络压缩框架

https://mp.weixin.qq.com/s/ZuEi32ZBSjruvtyUimBgxQ

揭秘支付宝中的深度学习引擎：xNN

http://mp.weixin.qq.com/s/iapih9Mme-VKCfsFCmO7hQ

简单聊聊压缩网络

https://mp.weixin.qq.com/s/3qstz-KoRuxwpmfE4XDI-Q

面向卷积神经网络的卷积核冗余消除策略

https://mp.weixin.qq.com/s/dEdWz4bovmk65fwLknHBhg

韩松毕业论文：面向深度学习的高效方法与硬件

https://mp.weixin.qq.com/s/GFE2XYHZXPP0doQ5nd0JNQ

当前深度神经网络模型压缩和加速方法速览

https://mp.weixin.qq.com/s/Faej1LKqurtwEIreUVJ0cw

普林斯顿新算法自动生成高性能神经网络，同时超高效压缩

https://mp.weixin.qq.com/s/uK-HasmiavM3jv6hNRY11A

深度梯度压缩：降低分布式训练的通信带宽

https://mp.weixin.qq.com/s/_MDbbGzDOGHk5TBgbu_-oA

中大商汤等提出深度网络加速新方法，具有强大兼容能力

https://mp.weixin.qq.com/s/gbOmpP7XO1Hz_ld4iSEsrw

三星提出移动端神经网络模型加速框架DeepRebirth

https://mp.weixin.qq.com/s/rTFLiZ7DCo6vzD5O64UnMQ

阿里提出新神经网络算法，压缩掉最后一个比特

https://mp.weixin.qq.com/s/f1SCK0J5oTWNJvtld3UAHQ

神经网络修剪最新研究进展

https://mp.weixin.qq.com/s/3oL0Bso3mwbsfaG8X5-xoA

英特尔提出新型压缩技术DeepThin，适合移动端设备深度神经网络

https://mp.weixin.qq.com/s/JnW7RnOQKG-dPOOAQeOmSA

当前深度神经网络模型压缩和加速都有哪些方法？

https://mp.weixin.qq.com/s/YUg2dZZhDSsRpSftdNfiIQ

极致的优化：智能手机是如何处理大型神经网络的

https://mp.weixin.qq.com/s/Ck_GDv1Xo-YMZcu-00gTOA

中星微夺冠国际人工智能算法竞赛，目标检测一步法精度速度双赢

https://mp.weixin.qq.com/s/qWJarPrjOrwxSX77xQ9rCw

面向卷积神经网络的卷积核冗余消除策略

https://mp.weixin.qq.com/s/QSGgvhkMUj3cXVlQwlzTFQ

深度神经网络加速和压缩新进展年度报告

https://zhuanlan.zhihu.com/p/37074222

CVPR 2018 高效小网络探密（上）

https://zhuanlan.zhihu.com/p/37919669

CVPR 2018 高效小网络探密（下）

https://zhuanlan.zhihu.com/p/38046989

从ISCA论文看AI硬件加速的新技巧

https://mp.weixin.qq.com/s/jqRBrs9Y_-3qvemL0RTflA

支付宝如何优化移动端深度学习引擎？

https://mp.weixin.qq.com/s/-V6hlZAKp1vuARSibZDBQQ

深度学习高效计算与处理器设计

https://mp.weixin.qq.com/s/NJzGR-tY_WWeccbdshHckA

基于交错组卷积的高效深度神经网络

https://mp.weixin.qq.com/s/ccFccLb2UTyFyMwFPjsDaA

让CNN跑得更快，腾讯优图提出全局和动态过滤器剪枝
