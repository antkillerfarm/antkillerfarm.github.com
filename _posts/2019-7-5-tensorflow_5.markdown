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

https://mp.weixin.qq.com/s/6y7AEGz2cs4QUP0Cikhnmw

272页PPT讲述Tensorflow2.0在图形学方面的应用

https://mp.weixin.qq.com/s/VN2O6faf4spdD3qaqf3aiw

使用AMD显卡加速TensorFlow

# Exploration & Exploitation

## UCB（续）

UCB是一种乐观的算法，选择置信区间上界排序，如果是悲观保守的做法，是选择置信区间下界排序。

置信区间的度量有很多方式，由此产生了很多UCB的变种算法。最常用的UCB1算法，采用了如下度量方式：

$$I_i=\bar{x}_i+\sqrt{2\frac{\log t}{n_i}}$$

其中$$x_i$$是第i个臂的奖励均值，$$n_i$$是臂i当前累积被选择的次数。

UCB还可以应用到MCTS中，这就是UCT（Upper Confidence Bound Apply to Tree）算法了。即UCT=MCTS+UCB。

参考：

https://www.cnblogs.com/Ryan0v0/p/11366578.html

多臂赌博机问题(MAB)的UCB算法介绍

https://blog.csdn.net/yw8355507/article/details/48579635

UCB算法

https://blog.csdn.net/LegenDavid/article/details/71082124

LinUCB算法

https://zhuanlan.zhihu.com/p/32356077

Multi-Armed Bandit: UCB (Upper Bound Confidence)

# Inverse Reinforcement Learning

逆强化学习问题定义：

- 已知：

智能体在变化的环境中的行为

（optional）智能体传感器的数据

（optional）环境的模型

- 求：

最优化的Reward function

逆强化学习研究意义：

- 对动物人类行为用强化学习建模（生物学、计量经济学...）模仿学习。

- reward function在强化学习里面非常非常重要，是对行为的抽象精简的描述，因此IRL (Inverse Reinforcement Learning)可能是一种很高效的模仿学习范式。

通俗的说法就是：**我们用优化控制或强化学习得到的策略能用来解释人类的行为吗？**

比如让一个人去拿桌子上的一个橘子，那手的轨迹一定不是一条从起点到目标的直线，而是有一些弯曲的轨迹，也就是带有偏差的较优行为，但是这种偏差其实并不重要，只要最后拿到橘子就行了，也就是说：

1.有过程中一些偏差是不重要的，但另一些偏差就比较重要了（如最后没拿到）。

2.每次拿橘子的动作也是不一样的，因此人的动作带有一定的随机性。

3.我们可以认为人类行为轨迹分布是以最优策略为峰的随机分布。

![](/images/img3/GAN_vs_IRL.png)

参考：

https://zhuanlan.zhihu.com/p/61674994

Algorithms for Inverse Reinforcement Learning

https://zhuanlan.zhihu.com/p/32502503

CS 294：IRL

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

https://mp.weixin.qq.com/s/To3pnx1hVq_4p7UnQVMw9A

斯坦福大学&DeepMind联合提出机器人控制新方法，RL+IL端到端地学习视觉运动策略

https://mp.weixin.qq.com/s/O0Q1XoTA-7Yshr1ZqOZ90w

加州理工：什么是模仿学习, 这62页ppt带你了解进展

# RL与神经科学

Pavlov Model（1901）

Rescorla-Wagner Model（1972）

Thorndike’s Puzzle Box（1910）

参考：

https://zhuanlan.zhihu.com/p/24437724

学习理论之Rescorla-Wagner模型

# RL参考资源+

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
