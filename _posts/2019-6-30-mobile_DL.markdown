---
layout: post
title:  移动端推理框架, Kubernetes, Dubbo, Arm ML, DRL实战, GPU通信技术
category: AI 
---

# 移动端推理框架

最知名的移动端推理框架毫无疑问是Google的Tensorflow Lite和Android NN。这两个框架在本blog的Tensorflow章节已有描述。

TensorRT在Nvidia章节亦有描述。

## SNPE

Snapdragon Neural Processing Engine SDK是Qualcomm推出的NN加速包。

官网：

https://developer.qualcomm.com/software/qualcomm-neural-processing-sdk

和它配套的还有Qualcomm Math Library。

https://developer.qualcomm.com/software/qualcomm-math-library

## NCNN

NCNN是腾讯的作品。

代码：

https://github.com/Tencent/ncnn

## MACE

MACE是小米的作品。

代码：

https://github.com/XiaoMi/mace

## 百度系

百度的Inference-engine以多变且不兼容著称。

2017年9月底，baidu发布移动端深度学习框架mobile-deep-learning(MDL)。

2018年5月底，baidu发布跨平台AI推理加速引擎anakin。

2018年5月底，baidu移动端深度学习框架完全重构为paddle-mobile。

2018年8月，baidu为ARM移动端开发精简版anakin引擎anakin-lite。

2019年8月中，baidu发布纯自研的移动端框架paddle-lite。

## MNN

MNN是阿里的作品。

代码：

https://github.com/alibaba/MNN

参考：

https://mp.weixin.qq.com/s/yDvTDTk8VtGZjA3RK4dLMQ

阿里巴巴开源轻量级深度神经网络推理引擎MNN

https://mp.weixin.qq.com/s/EcwmGKpCfVy22BWTB0Ro2g

淘宝开源深度学习推理引擎MNN，移动AI的挑战与应对全面解读

## ONNX Runtime

ONNX Runtime是MS的作品。

官网：

https://microsoft.github.io/onnxruntime/

代码：

https://github.com/microsoft/onnxruntime

# Kubernetes

Kubernetes是Google开源的容器集群管理系统，基于Docker构建一个容器的调度服务，提供资源调度、均衡容灾、服务注册、动态扩缩容等功能套件。

官网：

https://kubernetes.io/

Kubernetes还有个ML的工具集叫KubeFlow。

https://github.com/kubeflow/kubeflow

参考：

https://mp.weixin.qq.com/s/HEUIWK4skqMge8oziQg6Nw

kubernetes简介和实战

http://violetgo.com/blogs/2015/10/14/k8s.html

docker学习笔记(k8s)

https://mp.weixin.qq.com/s/wwwB8OHQmbwqmKKE8Lyv-w

容器技术演化史

https://mp.weixin.qq.com/s/LpNlUlXpt2poRKXEvuIDjw

Kubernetes NetworkPolicy工作原理浅析

https://mp.weixin.qq.com/s/s5eDSQF6mFt2F9_nVtYemQ

PaaS将吞噬云计算？Kubernetes的市场冲击波

https://mp.weixin.qq.com/s/19xreKtNlllfFx43eUGhuQ

如何使用Kubernetes轻松部署深度学习模型

https://mp.weixin.qq.com/s/4e9l-GP22T-myP54hJJjPA

Kubernetes如何打赢容器之战？ 

https://mp.weixin.qq.com/s/iTfHv8EFx4O4G1sNxsuMkg

再见，Yarn！滴滴机器学习平台架构演进

https://mp.weixin.qq.com/s/CmHok-9gKVrHfiXpYUQ4nw

浅析Kubernetes资源管理

# Dubbo

Dubbo是一个分布式服务框架，致力于提供高性能和透明化的RPC远程服务调用方案，是阿里巴巴SOA服务化治理方案的核心框架，每天为2,000+个服务提供3,000,000,000+次访问量支持，并被广泛应用于阿里巴巴集团的各成员站点。

>阿里巴巴算是国内开源较多的IT企业了。但是早期仅仅满足于开源本身，对于开源项目的维护没有章法。Dubbo就是典型一例，开源之后的数年，没有任何官方升级和维护。社区由于官方的缺位，也没有大的动静。直到2016年，才纳入正轨。

官网：

http://dubbo.io/

官网的用户指南写的不错，非常值得一看。

https://mp.weixin.qq.com/s/bcwIMIir2RHPbQQr8HgTOQ

如何快速开发一个Dubbo应用？

https://mp.weixin.qq.com/s/fnrGjiywiySA8iAZh_cF0Q

阿里巴巴新开源项目Nacos发布第一个版本，助力构建Dubbo生态

https://mp.weixin.qq.com/s/AAcQRHZPvW11jvlbrLfRJA

携程的Dubbo之路

https://mp.weixin.qq.com/s/ZW4tO01gC65kZgOUappL9Q

漫话：什么是RPC

# Arm ML

Arm ML是Arm的作品。它包括了两部分：Arm NN和Arm Compute Library。

官网：

https://mlplatform.org/

## Arm NN

官网：

https://developer.arm.com/ip-products/processors/machine-learning/arm-nn

代码：

https://github.com/Arm-software/armnn

## Arm Compute Library

官网：

https://developer.arm.com/ip-products/processors/machine-learning/compute-library

代码：

https://github.com/ARM-software/ComputeLibrary

## CMSIS NN

CMSIS NN是ARM提供的一个针对Cortex-M CPU的NN计算库。

论文：

《CMSIS-NN: Efficient Neural Network Kernels for Arm Cortex-M CPUs》

官网：

http://www.keil.com/pack/doc/CMSIS_Dev/NN/html/index.html

代码：

https://github.com/ARM-software/CMSIS_5

## 概述

![](/images/img3/arm_nn_frameworks.png)

![](/images/img3/arm_ml_platforms.png)

## 其他

https://www.veryarm.com/872.html

armel、armhf和arm64区别选择

# DRL实战

## FlappyBird

Flappy Bird的action只有两种：动/不动，算是非常简单的游戏了。正好可以作为DQN的入门教程。

参考代码：

https://github.com/floodsung/DRL-FlappyBird

这个代码参考了：

https://github.com/yenchenlin/DeepLearningFlappyBird

Flappy Bird的python版本基于pygame，代码：

https://github.com/sourabhv/FlapPyBird

安装：

`pip3 install opencv-python pygame tensorflow`

实战心得：

- 奖励的设计：

通过柱子成功：+1

死亡：-1

活着，但未通过柱子：+0.1

这个游戏每50个time step通过一根柱子，也就是说，每50个time step才有一次大的奖励（无论正负）。

- 开始的时候，由于Agent没有什么经验，所以INITIAL_EPSILON不能太小。然而这个游戏和下棋不一样，下棋的每一步都有差不多的奖励，而这个游戏，多数的时间只是在飞而已，也就是说大奖励是很稀疏的。所以，INITIAL_EPSILON也不能太大，随机运动未必收敛最快。因此，INITIAL_EPSILON设置为0.5似乎是个不错的选择。（纯经验之谈）

- 为了快速收敛，忽略其他无关要素，例如把背景换成纯黑色、不显示得分等。图片的处理过程中，采用了二值化的方法，将彩色图片转换为黑白图片，以便于后面的机器学习。

- REPLAY_MEMORY是一个实用的DQN技巧，但在教程中往往被忽略。它可用来消除训练数据间的相关性。

- 作者创建两个Q网络，以及交换权值的代码，非常优雅。（copyTargetQNetworkOperation）

实战效果：

- 训练5W个time step：有60%的概率能通过第1根柱子。

- 训练40W个time step：平均能通过10根柱子。

- 训练140W个time step：平均能通过100根柱子。

- 为了验证模型的泛化性能，我将两根之间柱子的横向间隔随机化，性能立马掉到10根柱子的水平，但继续训练之后，性能提升速度比原先快的多。

如果嫌原始代码太大的话（140MB+），也可以下载本人的魔改版本：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/RL/DRL-FlappyBird

参考：

http://blog.csdn.net/songrotek/article/details/50951537

用Tensorflow基于DQN玩Flappy Bird

https://zhuanlan.zhihu.com/p/21434933

DQN实战篇 从零开始安装Ubuntu, Cuda, Cudnn, Tensorflow, OpenAI Gym

https://zhuanlan.zhihu.com/p/21477488

150行代码实现DQN算法玩CartPole。这是参考代码的作者本人写的另一个blog，其中的框架部分沿用到了Flappy Bird上。

https://mp.weixin.qq.com/s/nmqX05c2qS3Jeg66W5NOzA

尝试用强化学习算法来玩下FlappyBird？这篇blog只用Q-Learning算法，而不是DQN来玩FlappyBird。

## CartPole

这是Google官方的教程，使用的是A3C算法：

https://github.com/tensorflow/models/tree/master/research/a3c_blogpost

安装依赖：

`pip3 install gym pyglet tensorflow matplotlib`

首先是训练：

`python3 ./a3c_cartpole.py --train`

这里用的是Gym环境，不需要渲染游戏画面，而直接就可以训练。这个的训练速度很快，即使CPU也就是十分钟的事情。

展示效果：

`python3 ./a3c_cartpole.py`

果然稳如狗。。。

参考：

https://mp.weixin.qq.com/s/atQHJ5U2pJpSG6PguN7J4Q

如何保持运动小车上的旗杆屹立不倒？TensorFlow利用A3C算法训练智能体玩CartPole游戏

# GPU通信技术

## RDMA

RDMA网卡（Remote Direct Memory Access，这是一种硬件的网络技术，它使得计算机访问远程的内存时无需远程机器上CPU的干预）已经可以提供50~100Gbps的网络带宽和微秒级的传输延迟。

目前许多以深度学习为目标应用的GPU机群都部署了这样的网络。

参考：

https://mp.weixin.qq.com/s/_xcE8RUs0m4gwk3kxpe9jA

基于HTM/RDMA的可扩展内存事务处理系统

## NVLink

NVLink技术提供比PCIe 3更高的带宽与更多的链路，并可提升多GPU和多GPU/CPU系统配置的可扩展性。

官网：

https://www.nvidia.cn/data-center/nvlink/

![](/images/img3/nvlink.png)

Tesla V100中以NVLink连接的GPU至GPU和GPU至CPU通信。

![](/images/img3/nvlink_2.png)

在DGX-1V服务器中，混合立体网络拓扑使用NVLink连接8个Tesla V100加速器。每个GPU有6条nvlink通道，总带宽高达300GB/s。

从上图可以看到，即使每个GPU拥有6条nvlink通道，仍然无法做到“全连接”（即任意两个GPU之间存在双向通道）。这就引出了下一个更加疯狂的技术：nvswitch。

![](/images/img3/nvswitch.png)

NVSwitch是首款节点交换架构，可支持单个服务器节点中16个全互联的GPU，并可使全部8个GPU对分别以300GB/s的惊人速度进行同时通信。这16个全互联的GPU还可作为单个大型加速器，拥有0.5 TB统一显存空间和2 PetaFLOPS计算性能。

## 参考

https://www.infoq.cn/article/3D4MsRVS8ZOtGCj7*krT

GPU通信技术初探

https://zhuanlan.zhihu.com/p/67785062

不止显卡！这些硬件因素也影响着你的深度学习模型性能

# 新冠肺炎

## 口罩（续）

人民需要什么，五菱就造什么。

1928年，桂系领袖李宗仁在这里修建了一家柳州机械厂，开始组装飞机，制造步枪、手榴弹；

1933年，因为广西被封锁，石油匮乏，又造出了时速40公里/小时的木炭车，不烧油，烧炭跑，成为战时大后方的主要运输工具；

https://mp.weixin.qq.com/s/8rP-ZIvqAPlIW_j99jtMvQ

五菱的76个小时，改变了中国口罩

----

https://mp.weixin.qq.com/s/IXp36BDIi_cblf1nk-iidw

40万公里的征途，一场消毒除菌产品与病毒的赛跑（立白集团）

----

LVMH集团宣布，旗下Dior、Givenchy和娇兰三个品牌的香水生产线，将用于生产含酒精的消毒洗手液。

奢侈品牌Prada将一家在佩鲁贾开设的工厂，改造成了口罩及医疗用品生产工厂，并计划在4月6日前，向意大利托斯卡纳医院运送8万件防护服，及11万只口罩。

重症监护用肺呼吸机制造商Siare Engineering取消了占其营业额90％的外国订单，转而专为意大利提供机械，该公司产能从每月125台提升到到现在的500台。
