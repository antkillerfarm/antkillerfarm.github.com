---
layout: post
title:  深度加速（七）——模型压缩与加速进阶（2）, NN Quantization（三）
category: DL acceleration 
---

* toc
{:toc}

# 模型压缩与加速进阶

https://mp.weixin.qq.com/s/uXbLb5ITHOU0dZRSWNobVg

算力限制场景下的目标检测实战浅谈

https://mp.weixin.qq.com/s/DoeoPGnS88HQmxagKJWLlg

小米开源FALSR算法：快速精确轻量级的超分辨率模型

https://mp.weixin.qq.com/s/wT39oUWfrQK-dg7hGXRynQ

实时单人姿态估计，在自己手机上就能实现

https://mp.weixin.qq.com/s/RVsXUnAJ2f0Cby7BPaWifA

人物属性模型移动端实验记录

https://mp.weixin.qq.com/s/yCcK6UJqm850HON7xU3R6g

模型压缩重要方向-动态模型，如何对其长期深入

https://zhuanlan.zhihu.com/p/93020471

轻量型网络：IdleBlock

https://mp.weixin.qq.com/s/AjuTXFmxHYdUUqodSpP_4w

10倍加速！爱奇艺超分辨模型加速实践

https://mp.weixin.qq.com/s/rzv8VCAxBQi0HsUcnLqqUA

处理移动端传感器时序数据的深度学习框架：DeepSense

https://mp.weixin.qq.com/s/UYk3YQmFW7-44RUojUqfGg

上交大ICCV：精度保证下的新型深度网络压缩框架

https://mp.weixin.qq.com/s/ZuEi32ZBSjruvtyUimBgxQ

揭秘支付宝中的深度学习引擎：xNN

https://mp.weixin.qq.com/s/0KlnQ8UUxpyhBRdeo0EOAA

用于网络压缩的滤波器级别剪枝算法ThiNet

https://mp.weixin.qq.com/s/FvR6loJ8KUxm7qwclestcQ

专门为卷积神经网络设计的训练方法：RePr

https://mp.weixin.qq.com/s/67GSnZnJySFrCESvmwhO9A

论文解读Channel pruning for Accelerating Very Deep Neural Networks

https://mp.weixin.qq.com/s/Lkxc_9sbRY157sMWaD5c7g

视频分割在移动端的算法进展综述

https://mp.weixin.qq.com/s/ie2O5BPT-QxTRhK3S0Oa0Q

剪枝需有的放矢，快手&罗切斯特大学提出基于能耗建模的模型压缩

# NN Quantization

## 二值神经网络

二值神经网络的主要缺点在于：它们无法实现与完全精度的深层网络一样高的精度。但这一直在缓慢地变化，已经有了很多进步。

综述：

《BiBench: Benchmarking and Analyzing Network Binarization》

![](/images/img5/BNN.png)

BNN,DoReFA,Bi-Real,ReActNet一脉相承。

XNOR,XNOR++,ReCU为另一个流派。

参考：

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

## Optimizer Quantization

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

bitsandbytes--Facebook 推出 8 比特优化器大大减少显存

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

https://www.zhihu.com/question/627484732

目前针对大模型进行量化的方法有哪些？

# 并行 & 框架 & 优化=

http://engineering.skymind.io/distributed-deep-learning-part-1-an-introduction-to-distributed-training-of-neural-networks

神经网络的分布式训练

https://mp.weixin.qq.com/s/nvuflLfOolidDDXJVe2DZA

美团深度学习系统的工程实践

https://mp.weixin.qq.com/s/IE6blClvhYlq3-QAGHo5ww

TensorFlow分布式计算机制解读：以数据并行为重

https://mp.weixin.qq.com/s/4Ii3um3jqfm5yKKxZAFdmA

继1小时训练ImageNet之后，大批量训练扩展到了3万2千个样本

https://mp.weixin.qq.com/s/kOCftzSbHe2mvDmlRp-ihA

Jeff Dean：AI对计算机系统设计的影响

https://mp.weixin.qq.com/s/XjNPaL6PC9LHX1PEGn5UZg

微软实时AI系统“脑波计划”有多牛？看完秒懂！

https://mp.weixin.qq.com/s/OkqUulFYHQSdgAbf9Fi9LA

CoCoA：大规模机器学习的分布式优化通用框架

https://mp.weixin.qq.com/s/ToIDncp9dS_qk47PsdZm5A

杜克大学：分布式深度学习训练算法TernGrad

https://mp.weixin.qq.com/s/rhtrN2qDspGkpJYDAVSX7w

UC Berkeley展示全新并行处理方法

https://mp.weixin.qq.com/s/ASqpPSIgW_bcFPBfRYz7Xg

哈佛大学提出在云、边缘与终端设备上的分布式深度神经网络DDNN

http://blog.sina.com.cn/s/blog_81f72ca70101kuk9.html

《Large Scale Distributed Deep Networks》中译文

https://mp.weixin.qq.com/s/X7XG51yohLnEZ_Jg6XK9oQ

Caffe作者贾扬清教你怎样打造更加优秀的深度学习架构

https://zhuanlan.zhihu.com/p/529388795

训练千亿参数大模型，离不开四种GPU并行策略

https://mp.weixin.qq.com/s/_mrYI7McMBUx0lEh4rNiYQ

百度开源移动端深度学习框架MDL，手机部署CNN支持iOS GPU

https://mp.weixin.qq.com/s/ZCNSq5FC2REoVTKAK2mJQg

分布式深度学习原理、算法详细介绍

https://mp.weixin.qq.com/s/Ewiil56vMkzhO2xDWgo-Wg

苹果发布Turi Create机器学习框架，5行代码开发图像识别

https://mp.weixin.qq.com/s/jOVUPhrCBI9W9vPvD9eKYg

UC Berkeley提出新型分布式框架Ray：实时动态学习的开端
