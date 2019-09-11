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
