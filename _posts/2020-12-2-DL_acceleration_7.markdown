---
layout: post
title:  深度加速（七）——硬件加速技巧, 模型优化工具, 模型压缩与加速进阶（1）
category: DL acceleration 
---

* toc
{:toc}

# 知识蒸馏（续）

https://mp.weixin.qq.com/s/6K5FvjMIVer-_fXJazU20Q

深度学习中的3个秘密：集成，知识蒸馏和蒸馏

https://mp.weixin.qq.com/s/-Rzvx9RMg9uZK5NFDs6cNg

语义分割的结构知识蒸馏

https://zhuanlan.zhihu.com/p/160206075

Knowledge Distillation（知识蒸馏）Review--20篇paper回顾

https://mp.weixin.qq.com/s/E7-MF18Y-UeKx694kGFHzA

深度学习中的知识蒸馏技术（上）

https://mp.weixin.qq.com/s/Noac4YLIimr1HM2fln2bjg

深度学习中的知识蒸馏技术（下）

https://mp.weixin.qq.com/s/IkKig7I5_97y_siixEj72w

协同训练

https://mp.weixin.qq.com/s/SUcz-Ba37CzUoAG52zW7YA

强化学习推荐模型的知识蒸馏探索之路

# 硬件加速技巧

https://mp.weixin.qq.com/s/KPT4P5SQ4E4ofPdjhhjRvA

如何加速深度神经网络计算效率？看NVIDIA-ISSCC2021教程，附93页Slides与视频

---

多通道卷积操作最终可以转化为矩阵运算，如下图所示：

![](/images/article/conv.png)

![](/images/img4/im2col.jpg)

![](/images/img4/im2col_2.jpg)

这种将卷积运算变为矩阵乘法运算的方法，一般被称为GEMM（General Matrix Matrix multiplication）。因为卷积变为矩阵这一步运算在Caffe中是用im2col函数实现的，因此，也有使用im2col来指代这类方法的。

要点：

- forward的时候，只有input需要im2col。

- backward的时候，先算好input_grad，再col2im将之变换到input的形状即可。

和GEMM类似的还有GEMV（General Matrix Vector multiplication）。

参见：

http://blog.csdn.net/u014114990/article/details/51125776

多通道(比如RGB三通道)卷积过程

https://www.zhihu.com/question/28385679

在Caffe中如何计算卷积？

https://buptldy.github.io/2016/10/01/2016-10-01-im2col/

Implementing convolution as a matrix multiplication（中文blog）

https://zhuanlan.zhihu.com/p/63974249

im2col方法实现卷积算法

https://zhuanlan.zhihu.com/p/66958390

通用矩阵乘（GEMM）优化与卷积计算

https://mp.weixin.qq.com/s/Q1Ovl1LrT5Y6amVqlYpdbA

基于GEMM实现的CNN底层算法被改？Google提出全新间接卷积算法

https://jackwish.net/2019/gemm-optimization.html

通用矩阵乘（GEMM）优化算法

https://mp.weixin.qq.com/s/lqVsMDutBwsjiiM_NkGsAg

详解Im2Col+Pack+Sgemm策略更好的优化卷积运算

https://mp.weixin.qq.com/s/EgC2puTsIfEk1uvgWlHXZA

基于how-to-optimize-gemm初探矩阵乘法优化

https://mp.weixin.qq.com/s/w0YCm8TEPxFg0CR6g4A28w

再探矩阵乘法优化

https://mp.weixin.qq.com/s/moQnarr1U-8v834bNJ10Zw

GPU上的高效softmax近似

https://mp.weixin.qq.com/s/PMOrY5ZElyPGOVxZgXFVzw

如果只能做整数Integer运算还能用BERT吗？

https://mp.weixin.qq.com/s/Fes8FHngKnL8jklB7DhNCQ

图计算加速架构综述

# 模型优化工具

## Amazon SageMaker Neo

官网：

https://aws.amazon.com/cn/sagemaker/neo/

## 参考

https://mp.weixin.qq.com/s/T9AUFnLjNDUaE9zKmOhbEw

将GEMM的性能提升200倍!AutoKernel算子优化工具正式开源

https://mp.weixin.qq.com/s/L9kYXFXYmKadghAhd-51pA

TensorFlow模型优化工具包—剪枝API

https://mp.weixin.qq.com/s/asPSPeBaRF_4eXcRXU-Zfw

TensorFlow模型优化工具包—训练时量化

https://mp.weixin.qq.com/s/fa5S3o1somvdAAJF1FGqvA

TensorFlow模型优化工具包正式推出

# 模型压缩与加速进阶

https://zhuanlan.zhihu.com/p/138059904

一文看懂深度学习模型压缩和加速

https://zhuanlan.zhihu.com/p/179945324

一文深入深度学习模型压缩和加速

https://mp.weixin.qq.com/s/QSGgvhkMUj3cXVlQwlzTFQ

深度神经网络加速和压缩新进展年度报告

https://zhuanlan.zhihu.com/p/37074222

CVPR 2018 高效小网络探密（上）

https://zhuanlan.zhihu.com/p/37919669

CVPR 2018 高效小网络探密（下）

https://zhuanlan.zhihu.com/p/38046989

从ISCA论文看AI硬件加速的新技巧

https://mp.weixin.qq.com/s/s6Z8P8bUkyoKU2mW3z-rNQ

轻量级网络/检测/分割综述

https://mp.weixin.qq.com/s/-V6hlZAKp1vuARSibZDBQQ

深度学习高效计算与处理器设计

https://mp.weixin.qq.com/s/ccFccLb2UTyFyMwFPjsDaA

让CNN跑得更快，腾讯优图提出全局和动态过滤器剪枝

https://mp.weixin.qq.com/s/cSYCT1I1asaSCIc5Hgu0Jw

计算成本降低35倍！谷歌发布手机端自动设计神经网络MnasNet

https://zhuanlan.zhihu.com/p/42474017

MnasNet：终端轻量化模型新思路

https://mp.weixin.qq.com/s/p_qdKcQwQ8y_JUw3gQUEnA

谷歌大脑用强化学习为移动设备量身定做最好最快的CNN模型

https://mp.weixin.qq.com/s/OyEIcS5o6kWUu2UzuWZi3g

这么Deep且又轻量的Network，实时目标检测

https://mp.weixin.qq.com/s/8NDOf_8qxMMpcuXIZGJCGg

Google又发大招：高效实时实现视频目标检测

https://mp.weixin.qq.com/s/IxVMMu_7UL5zFsDCcYfzYA

AutoML自动模型压缩再升级，MIT韩松团队利用强化学习全面超越手工调参

https://mp.weixin.qq.com/s/BMsvhXytSy2nWIsGOSOSBQ

自动生成高效DNN，适用于边缘设备的生成合成工具FermiNets

https://mp.weixin.qq.com/s/nEMvoiqImd0RxrskIH7c9A

仅17KB、一万个权重的微型风格迁移网络！

https://mp.weixin.qq.com/s/pc8fJx5StxnX9it2AVU5NA

基于手机系统的实时目标检测

https://mp.weixin.qq.com/s/6wzmyhIvUVeAN4Xjfhb1Yw

论文解读：Channel pruning for Accelerating Very Deep Neural Networks

https://mp.weixin.qq.com/s/-X7NYTzOzljzOaQL7_jOkw

惊呆了！速度高达15000fps的人脸检测算法！

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

https://mp.weixin.qq.com/s/F0ykoKv027ycinsAZZjbWQ

ThunderNet：国防科大、旷视提出首个在ARM上实时运行的通用目标检测算法

https://mp.weixin.qq.com/s/J3ftOKDPBY5YYD4jkS5-aQ

ThunderNet：Two-stage形式的目标检测也可很快而且精度很高

https://mp.weixin.qq.com/s/ie2O5BPT-QxTRhK3S0Oa0Q

剪枝需有的放矢，快手&罗切斯特大学提出基于能耗建模的模型压缩

https://mp.weixin.qq.com/s/m9I5TM9uJcgZvMusO667OA

5MB的神经网络也高效，Facebook新压缩算法造福嵌入式设备

https://mp.weixin.qq.com/s/FFs0-ROvbXSAIOspW_rMbw

超越MobileNetV3！谷歌大脑提出MixNet轻量级网络

https://mp.weixin.qq.com/s/nys9R6xCJXt0vG06gnQzFQ

模型剪枝，不可忽视的推断效率提升方法

https://mp.weixin.qq.com/s/EuT-4_eEtIVKh6QdLDbohg

解读小米MoGA：超过MobileNetV3的移动端GPU敏感型搜索

https://mp.weixin.qq.com/s/L49cqZ2PXP-x4Y6xBJG5cQ

旷视研究院提出MetaPruning：基于元学习和AutoML的模型压缩新方法

https://mp.weixin.qq.com/s/F_p414ezQ0RDhPnZj36SUA

网络剪枝中的AutoML方法

https://zhuanlan.zhihu.com/c_151876233

如何Finetune一个小网络到移动端（时空性能分析篇）

https://mp.weixin.qq.com/s/kU0_BuQW8VQihUwBuZ90cA

发布可伸缩超网SCARLET，小米AutoML团队NAS三部曲杀青

https://mp.weixin.qq.com/s/ZfW-jDSo6uaaqdmJtBizOA

从模型精简，硬件实现，到模型剪枝

https://mp.weixin.qq.com/s/5ywMyedmplCSLzWlnoFDSQ

模型剪枝技术原理及其发展现状和展望

https://mp.weixin.qq.com/s/8jyQ_7DYn7lHMcAWokKbcA

超Mask RCNN速度4倍，仅在单个GPU训练的实时实例分割算法

https://mp.weixin.qq.com/s/TC_Ju2vuKDP6d538v2F8CQ

剪枝需有的放矢，快手&罗切斯特大学提出基于能耗建模的模型压缩

https://mp.weixin.qq.com/s/UkqwPBYgYQuIB9_jGMt2QQ

Rocket Training: 一种提升轻量网络性能的训练方法

https://mp.weixin.qq.com/s/xCzS7sYMFmk5K4ClB1I2YQ

Uber提出SBNet：利用激活的稀疏性加速卷积网络

https://mp.weixin.qq.com/s/6Wj0Y4y30BVA75WrU4oZbQ

SBNet: 提高自动驾驶系统的感知效率

https://mp.weixin.qq.com/s/HXxnhMjAchxKSidu45kOeg

网络压缩最新进展：2019年最新文章概览

https://mp.weixin.qq.com/s/Bl7-hGIxZMsHxscqb7DnMA

200～1000+fps！谷歌公布亚毫秒级人脸检测算法BlazeFace，面向移动GPU

https://mp.weixin.qq.com/s/l2_N-PXjDMCqSRwYxU4BEA

模型加速概述与模型裁剪算法技术解析

https://mp.weixin.qq.com/s/af-z73asc-PmpEsI_yEulA

北邮提出新AI模型压缩算法，显著降低计算复杂度

https://mp.weixin.qq.com/s/AOI2LUjiKPUJFE0D7zX0Hw

谷歌新研究：基于数据共享的神经网络快速训练方法

https://mp.weixin.qq.com/s/Q0XyKIrbOIrA3YsYHmwK1Q

移动端高效率的分组网络都发展到什么程度了？

https://mp.weixin.qq.com/s/l3790PuutrOF27RRmVqJhQ

面对千万级推荐，如何压缩模型最高效？这是腾讯看点新框架

https://mp.weixin.qq.com/s/n_LY6mmJRH5k_cubYOTq1A

模型剪枝有哪些关键技术，如何对其进行长期深入学习

https://mp.weixin.qq.com/s/NsvjADgQZrYkUGNN6fzXVg

驭势科技推出“东风网络”：如何找到速度-精度的最优解？

https://mp.weixin.qq.com/s/HzgRHtVwdmW6_m7OJwK-ew

SysML 2019论文解读：Accurate and Efficient 2-Bit Quantized Neural Netowrks

https://mp.weixin.qq.com/s/oah61YozMB2fMfpDqPwHjw

Deep Compression神经网络压缩经典之作

https://mp.weixin.qq.com/s/ulrPhfsPunKAWYohBhkh9w

寻找最佳的神经网络架构，韩松组两篇论文解读

https://mp.weixin.qq.com/s/oDwvMtET0moHVGgtQLfCow

5个可以让你的模型在边缘设备上高效推理的算法

https://mp.weixin.qq.com/s/nbEa0csbaMvEM3TCI3fn0Q

当前模型剪枝有哪些可用的开源工具？
