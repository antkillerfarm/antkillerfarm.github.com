---
layout: post
title:  移动端推理框架, Kubernetes, Dubbo, Arm ML
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
