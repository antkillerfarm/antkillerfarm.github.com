---
layout: post
title:  TensorFlow（五）
category: AI 
---

# TensorFlow

https://zhuanlan.zhihu.com/p/28475975

如何优雅地用TensorFlow预测时间序列：TFTS库详细教程

https://mp.weixin.qq.com/s/zZCEOdNQsPovn_i-C57Z9g

如何使用最流行框架Tensorflow进行时间序列分析？

https://mp.weixin.qq.com/s/CqOo7Fu6t5-yJiYhzo03oQ

利用TensorFlow和神经网络来处理文本分类问题

https://mp.weixin.qq.com/s/VlvQmrS7Qi2qq6fTBXKTYw

从零开始用TensorFlow搭建卷积神经网络

https://mp.weixin.qq.com/s/hETnA81WlkMG3rftAHg9bw

PyTorch和TensorFlow哪家强：九项对比读懂各自长项短板

https://mp.weixin.qq.com/s/7R-Gvegnta9XBwIaSPBL_Q

基于Tensorflow的验证码识别

https://mp.weixin.qq.com/s/3QgtemxxsQmuNQVEdpiMwA

如何做准确率达98%的交通标志识别系统？

https://mp.weixin.qq.com/s/pSE2V8wD3_KHMI71kLTXng

如何基于TensorFlow使用LSTM和CNN实现时序分类任务

https://mp.weixin.qq.com/s/dHkmDvFVUGmt4Ch-gv3s1g

一步一步带你用TensorFlow玩转LSTM

https://mp.weixin.qq.com/s/Bx5Djj-RE0jPJ7LjyQ7GPg

基于gym和tensorflow的强化学习算法实现

https://mp.weixin.qq.com/s/3URLEdhB8hs0XXekKbvsnw

使用TensorFlow在卷积神经网络上实现L2约束的softmax损失函数

https://mp.weixin.qq.com/s/FdPrRQr0LukcWh7B703MlQ

利用tf.gradients在TensorFlow中实现梯度下降

https://github.com/indiejoseph/cnn-text-classification-tf-chinese

CNN for Chinese Text Classification in Tensorflow

https://mp.weixin.qq.com/s/yc1ssCzaPzI4UUsl4jl5Yw

用TensorFlow和TensorBoard从零开始构建ConvNet

https://mp.weixin.qq.com/s/6cFvh0OgouY_Lg7awxv_3g

快速开启你的第一个项目：TensorFlow项目架构模板

https://mp.weixin.qq.com/s/HshYcb98QyW0rR_svFpfUg

如何在TensorFlow中高效使用数据集

https://mp.weixin.qq.com/s/rSt6omyXe57WkBVXwp2AJg

数据载入过慢？这里有一份TensorFlow加速指南

https://mp.weixin.qq.com/s/ffW21oBKTDOc4sB8POhcnw

聊一聊TensorFlow的数据导入机制

https://mp.weixin.qq.com/s/nwymOr03cqm0ifpoBjL9Eg

TensorFlow变量保存和恢复

http://www.holmesconan.me/%E6%B7%B1%E5%BA%A6%E5%AD%A6%E4%B9%A0/%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0/2018/04/03/cifar10-vgg.html

CIFAR-10 Estimator之Vgg模型

https://mp.weixin.qq.com/s/5_oCSK6MtsK_4xNU6T76UA

计算机图形学遇上深度学习，针对3D图像的TensorFlow Graphics面世

https://mp.weixin.qq.com/s/ndnbTCr51k_FSDRCLi0UOg

添加新操作

https://mp.weixin.qq.com/s/G3fbhTsHA22b02_B5WCPcg

TensorFlow调试程序（一）

https://mp.weixin.qq.com/s/1mSF2HKa72HgWq_q14HMtQ

TensorFlow调试程序（二）

https://mp.weixin.qq.com/s/L9kYXFXYmKadghAhd-51pA

TensorFlow模型优化工具包—剪枝API

https://mp.weixin.qq.com/s/OGsvcfU1VlhNZYgEGLimsQ

爱奇艺基于TensorFlow Lite的移动端AR解决方案SmileAR

https://mp.weixin.qq.com/s/sm4D9TBSllAONgeOxOiWkA

TF.Text来啦！

https://mp.weixin.qq.com/s/9etR8QEk4UXtoLqkJFQIHA

深入理解TensorFlow中的tf.metrics算子

https://zhuanlan.zhihu.com/p/27087310

《安娜卡列尼娜》文本生成——利用TensorFlow构建LSTM模型

# Integrating Learning and Planning（续）

## Dyna-2

如果我们把基于模拟的前向搜索应用到Dyna算法中来，就变成了Dyna-2算法。

使用该算法的个体维护了两套特征权重：

- 一套反映了个体的长期记忆，该记忆是从真实经历中使用TD学习得到，它反映了个体对于某一特定强化学习问题的普遍性的知识、经验；

- 另一套反映了个体的短期记忆，该记忆从基于模拟经历中使用TD搜索得到，它反映了个体对于某一特定强化学习在特定条件（比如某一Episode、某一状态下）下的特定的、局部适用的知识、经验。

# Exploration & Exploitation

几个基本的探索方法：

- **朴素探索(Naive Exploration)**: 在贪婪搜索的基础上增加一个$$\epsilon$$以实现朴素探索；

- **乐观初始估计(Optimistic Initialization)**: 优先选择当前被认为是最高价值的行为，除非新信息的获取推翻了该行为具有最高价值这一认知；

- **不确定优先(Optimism in the Face of Uncertainty)**: 优先尝试不确定价值的行为；

- **概率匹配（Probability Matching)**: 根据当前估计的概率分布采样行为；

- **信息状态搜索(Information State Search)**: 将已探索的信息作为状态的一部分联合个体的状态组成新的状态，以新状态为基础进行前向探索。

根据搜索过程中使用的数据结构，可以将搜索分为：

- **依据状态行为空间的探索(State-Action Exploration)**。针对每一个当前的状态，以一定的算法尝试之前该状态下没有尝试过的行为。

- **参数化搜索（Parameter Exploration)**。直接针对策略的函数近似，此时策略用各种形式的参数表达，探索即表现为尝试不同的参数设置。

Parameter Exploration的优点：得到基于某一策略的一段持续性的行为。

缺点：对个体曾经到过的状态空间毫无记忆，也就是个体也许会进入一个之前曾经进入过的状态而并不知道其曾到过该状态，不能利用已经到过这个状态这个信息。

# 模仿学习

https://zhuanlan.zhihu.com/p/27935902

机器人学习Robot Learning之模仿学习Imitation Learning的发展

https://zhuanlan.zhihu.com/p/25688750

模仿学习（Imitation Learning）完全介绍

https://mp.weixin.qq.com/s/naq73D27vsCOUBperKto8A

从监督式到DAgger，综述论文描绘模仿学习全貌

https://mp.weixin.qq.com/s/LNNqp2KsEAljG26hY43mUw

ICML2018 模仿学习教程

https://mp.weixin.qq.com/s/f9vSgH1HQwGXBDb0UGHQyQ

深度学习进阶之无人车行为克隆

# RL与神经科学

Pavlov Model（1901）

Rescorla-Wagner Model（1972）

Thorndike’s Puzzle Box（1910）

参考：

https://zhuanlan.zhihu.com/p/24437724

学习理论之Rescorla-Wagner模型

# RL参考资源

https://mp.weixin.qq.com/s/Fn1s9Ia8L1ckgn6iP24FhQ

如何让神经网络具有好奇心

https://mp.weixin.qq.com/s/PBf-YrkhwhPYXuiOGyahxQ

强化学习遭遇瓶颈！分层RL将成为突破的希望

https://zhuanlan.zhihu.com/p/58815288

强化学习之值函数学习

https://mp.weixin.qq.com/s/8Cqknze_iosz6Z6cqnuK5w

谷歌提出强化学习新算法SimPLe，模拟策略学习效率提高2倍

https://mp.weixin.qq.com/s/hKGS4Ek5prwTRJoMCaxQLA

强化学习Exploration漫游

https://zhuanlan.zhihu.com/p/65116688

值分布强化学习（01）

https://zhuanlan.zhihu.com/p/65364484

值分布强化学习（02）

https://zhuanlan.zhihu.com/p/62363784

强化学习之策略搜索

https://mp.weixin.qq.com/s/j9Cs5M9gyITu2u_XDkKm-g

Policy Gradient——一种不以loss来反向传播的策略梯度方法

https://mp.weixin.qq.com/s/x6gKTuYIx8y25KX-fCc5bA

蒙特卡洛梯度估计方法（MCGE）简述
