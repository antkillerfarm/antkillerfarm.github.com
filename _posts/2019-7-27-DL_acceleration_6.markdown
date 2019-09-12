---
layout: post
title:  深度加速（六）——知识蒸馏
category: DL acceleration 
---

# 知识蒸馏

知识蒸馏是另一大类的模型压缩方法。

Geoffrey Hinton的论文：

《Distilling the Knowledge in a Neural Network》

![](/images/img2/Distilling.jpeg)

老师网络可以被固定（正如在精炼过程中）或联合优化，甚至同时训练多个不同大小的学生网络。

![](/images/img2/Distilling_2.jpeg)

上图是另一篇论文的图：

《Object detection at 200 Frames Per Second》

该论文的中文版：

https://mp.weixin.qq.com/s/OCG1TiHl2dsuS24uacQ-MA

又快又准确，新目标检测器速度可达每秒200帧

论文：

《Articulatory and Spectrum Features Integration using Generalized Distillation Framework》

参考：

https://github.com/dkozlov/awesome-knowledge-distillation

知识蒸馏从入门到精通

https://zhuanlan.zhihu.com/p/24894102

《Distilling the Knowledge in a Neural Network》阅读笔记

https://luofanghao.github.io/2016/07/20/%E8%AE%BA%E6%96%87%E7%AC%94%E8%AE%B0%20%E3%80%8ADistilling%20the%20Knowledge%20in%20a%20Neural%20Network%E3%80%8B/

论文笔记《Distilling the Knowledge in a Neural Network》

http://blog.csdn.net/zhongshaoyy/article/details/53582048

蒸馏神经网络

https://mp.weixin.qq.com/s/QZ7PGvi27LiDOJaxici7Pw

数据蒸馏Dataset Distillation

https://mp.weixin.qq.com/s/mFuxCl0Mzv5hmDFewWZkrw

FAIR&MIT提出知识蒸馏新方法：数据集蒸馏

https://mp.weixin.qq.com/s/MDgqSwVEClNqNpaWuGTEpg

微软亚研院提出用于语义分割的结构化知识蒸馏

https://zhuanlan.zhihu.com/p/24337627

深度压缩之蒸馏模型

https://blog.csdn.net/xbinworld/article/details/83063726

知识蒸馏（Distilling the Knowledge in a Neural Network），在线蒸馏

https://mp.weixin.qq.com/s/ekKg46bQlWrlg9Hon01M5g

Hinton胶囊网络后最新研究：用“在线蒸馏”训练大规模分布式神经网络

https://mp.weixin.qq.com/s/SqxooZqSeD3wA4EFK5D3Kg

再生神经网络：利用知识蒸馏收敛到更优的模型

https://zhuanlan.zhihu.com/p/51563760

知识蒸馏（Knowledge Distillation）最新进展（一）

https://zhuanlan.zhihu.com/p/53864403

知识蒸馏（Knowledge Distillation）最新进展（二）

https://mp.weixin.qq.com/s/Nkxy0SUdbwIjp5swU6tS9g

深度互学习-Deep Mutual Learning：三人行必有我师

https://mp.weixin.qq.com/s/I08kMmUohWWbYVpPDgTJsw

Startdt AI提出：使用生成对抗网络用于One-Stage目标检测的知识蒸馏方法

https://mp.weixin.qq.com/s/yFyM5OVp1YLKQBlgXeAzhw

华为诺亚方舟实验室提出无需数据网络压缩技术

https://mp.weixin.qq.com/s/a0d0b1jSm5HxHso9Lz8MSQ

小版BERT也能出奇迹：最火的预训练语言库探索小巧之路

# Integrating Learning and Planning（续）

## Model Learning

所谓Model Learning是指：从experience$$\{S_1, A_1, R_2, \dots, S_T\}$$，学习

$$S_1, A_1\to R_2,S_2$$

$$S_2, A_2\to R_3,S_3$$

$$\dots$$

$$S_{T-1}, A_{T-1}\to R_T,S_T$$

这实际上是一个监督学习的问题。其中，$$s,a\to r$$是一个回归问题(regression problem)，而$$s,a\to s'$$是一个密度估计问题(density estimation problem)。

选择一个损失函数，比如均方差，KL散度等，优化参数$$\eta$$来最小化经验损失(empirical loss)。所有监督学习相关的算法都可以用来解决上述两个问题。

根据使用的算法不同，可以有如下多种模型：查表式(Table lookup Model)、线性期望模型(Linear Expectation Model)、线性高斯模型(Linear Gaussian Model)、高斯决策模型(Gaussian Process Model)、和深信度神经网络模型(Deep Belief Network Model)等。

这里主要以查表模型来解释模型的构建。

## Table Lookup Model

查表模型适用于MDP的P，R都为已知的情况。我们通过visit得到各状态行为的转移概率和奖励，把这些数据存入表中，使用时直接检索。状态转移概率和奖励计算方法如下：

$$\hat{P}^a_{s,s'}=\frac{1}{N(s,a)}\sum_{t=1}^T 1(S_t,A_t,S_{t+1}=s,a,s')$$

$$\hat{R}^a_{s}=\frac{1}{N(s,a)}\sum_{t=1}^T 1(S_t,A_t=s,a)R_t$$

其中的$$N(s,a)$$表示state action pair$$<s,a>$$在visit中出现的次数。

这里也可以采用如下变种：

- 在每个time-step：t，记录experience tuple：$$<S_t, A_t, R_{t+1}, S_{t+1}>$$。

- 从模型采样构建虚拟经历时，从tuples中随机选择符合$$<s,a,\cdot,\cdot>$$的一个tuple。

这种方法，不再以Episode为最小学习单位，而是以时间步（time-step）为单位，一次学习一个状态转换。

## Planning with a Model

规划的过程相当于求解一个MDP的过程，即给定一个模型$$M_\eta = <P_\eta, R_\eta>$$，求解MDP$$<S, A, P_\eta, R_\eta>$$，找到基于该模型的最优价值函数或最优策略，也就是在给定的状态s下确定最优的行为a。

我们可以使用之前介绍过的价值迭代，策略迭代方法；也可以使用树搜索方法（Tree Search）。

## Sample-Based Planning

Sample-Based Planning使用Model生成采样数据，即一个time-step的虚拟经历。

有了这些虚拟采样，随后使用model-free RL方法来学习得到价值或策略函数。

## Planning with an Inaccurate Model

由于实际经历的不足或者一些无法避免的缺陷，我们通过实际经历学习得到的模型不可能是完美的模型，即：$$<P_\eta, R_\eta> = <P,R>$$

而model-based RL的性能取决于Model的准确度，如果Model不准确的话，往往只能得到一些次优解。

使用近似的模型解决MDP问题与使用价值函数或策略函数的近似表达来解决MDP问题并不冲突，他们是从不同的角度来近似求解MDP问题，有时候构建一个模型来近似求解MDP比构建一个近似的价值函数或策略函数要更加方便，这是model-based RL的可取之处。

同时由于个体经历的更新，模型处在一个动态变化的过程，当利用早期经历构建的模型后来被认为存在很大错误，不适用于当下经历的时候，需要放弃模型，转而使用model-free RL。

此外，可以在模型中，增加一些不确定性，以跳出次优解。

model-based RL适用于连续变量的状态和行为空间。

# Dyna-Q Algorithm

当构建了一个环境的模型后，个体可以有两种经历来源：

- 实际经历(real experience)。

$$S'\sim P_{s,s'}^a, R=R_s^a$$

- 模拟经历(simulated experience)。

$$S'\sim P_\eta(S' | S,A), R=R_\eta(R | S,A)$$

我们可以把不基于模型的真实经历和基于模型采样得到的模拟经历结合起来，提出一种新的架构：



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
