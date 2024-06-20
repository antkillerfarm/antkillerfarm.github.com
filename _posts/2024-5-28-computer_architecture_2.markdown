---
layout: post
title: 计算机体系结构（二）
category: Chip 
---

* toc
{:toc}

# 计算机体系结构

## VLIW & superscalar

超长指令字(VLIW:Very long instruction word)和超标量（superscalar）都在同一个CPU中，集成了数套运算单元。

超标量**用硬件来决定**哪些指令可以并行执行，而VLIW采**用软件来决定**哪些指令并行，通过把指令调度的复杂度交给编译器来降低硬件复杂度。

VLIW的代表是Intel的Itanium处理器。当处理器的执行宽度(execution width)，指令执行延迟时间，执行单元个数(function unit)改变时，VLIW需要重新编译程序来适应。但是Superscalar却不需要。

因此，VLIW在GPU中用的比较多。GPU程序并不直接生成GPU指令，而是通过厂商提供的DX/OpenGL库操作GPU。因此这些重新编译程序的任务已经由厂商完成，对于使用者透明。

https://mp.weixin.qq.com/s/OQ3KUAi6HMyOS8k3J_1e6g

关于超长指令集VLIW的一些讨论

https://www.zhihu.com/question/430177243

CPU长指令(VLIW)失败的主要原因是什么，VLIW真的无药可救吗？

---

VLIW和superscalar的设计差异，实际上是Brainiac和Speed-Demon两种路线之争，前者需要额外使用硬件来处理指令并行，而后者主张用编译器来节省硬件。

然而编译器由于只有编译期的信息，而没有执行期的信息辅助指令生成，设计难度和优化程度远远不如硬件处理，所以VLIW在CPU领域很快就败下阵来，现在流行的CPU，几乎都是superscalar的。

## Out-of-Order Execution

![](/images/img5/Out_of_Order_Execution.png)

superscalar采用了如上图所示乱序执行的方案来并行执行。而且这个能力是硬件提供的，串行的代码不需要做任何修改，就能比原来执行的速度快很多。

## 硬件多线程

在处理器中多开辟几份线程状态，当线程发生切换时，处理器切换到对应的线程状态执行，这种方式叫做硬件多线程（Hardware Multithreading）。

这里的线程不是软件概念，而是指的指令流水线，也就是下图中Instruction stream。

有两种方式实现硬件多线程：

- 并发多线程(SMT)

- 时域多线程

![](/images/img5/SMT.png)

SMT相当于是多线程版本的Out-of-Order Execution。

![](/images/img5/HT.png)

![](/images/img5/HT_2.png)

时域多线程，又称Hyper-Threading，实际上就是多级流水线。

硬件多线程和Cache一样，都是隐藏指令延迟的常用手段。

---

超线程其实就是两个线程共享一个计算单元和内存控制单元，但有自身独立的一些硬件，目的是两个都不是全负荷工作的线程可以轮流使用计算单元来实现比较好的throughput。但只要有一个很忙，那就可能饿死另一个。

进行乱序执行的处理器必须记住这些指令之间的依赖性，使用可重命名的寄存器可以容易地实现这个目的：例如将寄存器中的值存储到内存中，接着加载内存中的其他值到同名寄存器中，这里的”同名寄存器“并不一定是物理上的同一个寄存器。更进一步说，如果把这两条指令映射到不同的物理寄存器中，则他们可以并行执行，这就是实现乱序执行的重点。

## 加长流水线 vs 加宽流水线

奔腾四（P4）处理器所使用的核心有三个发展阶段：威廉核心（Willamette）、北木核心（northwoog）、波塞冬核心（Prescott）

第一代P4威廉核心（Willamette）只有13级流水线，频率基本没有上2G，性能中规中矩；

第二代P4北木核心（Northwoog）使用了20级流水线，这个级数比较符合当时的处理能力，没有浪费执行效率，所以北木被认为是P4系列里最成功的一个架构。当时P4非常成功地把AMD速龙XP系列CPU成功压制住了，于是英特尔在加长流水线的路上越走越远；

第三代P4波塞冬核心（Prescott）把流水线长度增加到了31级，缓存也加大了，相应也把频率频率拉的很高，但是大家很快就发现虽然频率高但是实际运行效率却没有北木那么高，3.2G以下波塞冬竟然打不过北木，发热和功耗也成为了劣势，高频低能说的就是波塞冬。

加长流水线既然不行，加宽流水线就成为了必然，超标量、GPU都是这方面的尝试。

https://www.zhihu.com/question/26289306

处理器（CPU）流水线长度是否存在理论极限?

https://www.starduster.me/2020/11/05/modern-microprocessors-a-90-minute-guide

现代微处理器架构 90 分钟指南

## 参考

https://blog.csdn.net/do2jiang/article/details/4545889

流水线、超流水线、超标量技术对比

https://blog.csdn.net/edonlii/article/details/8755205

单发射与多发射

https://mp.weixin.qq.com/s/FJX9eeRkxS5nJ4XIhRVwkg

从ServerSwitch到SONiC Chassis：数据中心交换机技术的十年探索历程

![](/images/img4/apple.png)

https://mp.weixin.qq.com/s/MCYocOY2pHHqiLhcE4SXyg

为什么苹果M1芯片这么快？

https://zhuanlan.zhihu.com/p/358167791

Vector的前世今生（1）：从辉煌到低谷

https://zhuanlan.zhihu.com/p/368640768

Vector的前世今生（2）：ARM SVE简述

https://zhuanlan.zhihu.com/p/594532014

一文搞懂Cortex-A77（ARMv8架构）工作原理

# 并行 & 框架 & 优化+

https://mp.weixin.qq.com/s/_1Yr_BbFhlNEW7UtYvAaoA

分布式深度学习，93页ppt概述最新DDL技术发展

https://mp.weixin.qq.com/s/jC5v9BKQvlxa2_6cikXV9w

分布式算法与优化，118页pdf

https://zhuanlan.zhihu.com/p/58806183

深度学习的分布和并行处理系统

https://zhuanlan.zhihu.com/p/56991108

一文说清楚Tensorflow分布式训练必备知识

https://mp.weixin.qq.com/s/r951Iasr4dke6MPHsUO0TA

开源DAWN，Stanford的又一力作

https://mp.weixin.qq.com/s/2jrMDeMcb47zpPfFLEcnIA

深度学习平台技术演进

https://mp.weixin.qq.com/s/L4CMKS53pNyvhhqvQhja0g

5种商业AI产品的技术架构设计

https://mp.weixin.qq.com/s/IqjKdAlGYREqCR9XQB5N1A

伯克利AI分布式框架Ray，兼容TensorFlow、PyTorch与MXNet

https://mp.weixin.qq.com/s/aNX_8UDYI_0u-MwMTYeqdQ

开发易、通用难，深度学习框架何时才能飞入寻常百姓家？

https://mp.weixin.qq.com/s/UbAHB-uEIvqYZCB7xIAJTg

机器学习新框架Propel：使用JavaScript做可微分编程

https://mp.weixin.qq.com/s/Ctl65r4iZNEOBxiiX2I2eQ

Momenta王晋玮：让深度学习更高效运行的两个视角

https://zhuanlan.zhihu.com/p/371499074

OneFlow——让每一位算法工程师都有能力训练GPT

https://mp.weixin.qq.com/s/LjdHBEyQhJq3ptMj8XVT-w

TensorFlow在推荐系统中的分布式训练优化实践

https://mp.weixin.qq.com/s/rEHhf32L09KXGJ9bbB2LEA

TensorFlow在美团外卖推荐场景的GPU训练优化实践

https://zhuanlan.zhihu.com/p/522759214

手把手推导分布式矩阵乘的最优并行策略

https://mp.weixin.qq.com/s/_o7fzCOeuZE6qFc5gHb26g

美团视觉GPU推理服务部署架构优化实践

https://mp.weixin.qq.com/s/UxN9ZRmKLN30s7uPqMpHPQ

Jeff Dean等提出动态控制流编程模型，大规模机器学习性能提升21%

https://mp.weixin.qq.com/s/fx0Pfu0MOPjSkzi5mL6U_A

清华&斯坦福提出深度梯度压缩DGC，大幅降低分布式训练网络带宽需求

https://mp.weixin.qq.com/s/wIdTDHEPffWqHA3_XWBLyw

没错，纯SQL查询语句可以实现神经网络。

>SQL跑神经网络固然没有太大意义，然而分布式数据库已经有数十年的历史，对于设计分布式深度学习框架亦有重大的启发意义。

https://mp.weixin.qq.com/s/F10UaaoxGPOE4pc59LBCRw

数据并行化对神经网络训练有何影响？谷歌大脑进行了实证研究

https://mp.weixin.qq.com/s/UF7DDenUQJ3bL83IHxOkIw

分布式优化算法及其在多智能体系统与机器学习中的应用

https://mp.weixin.qq.com/s/6h9MeBs89hTtWsYSZ4pZ5g

蚂蚁金服核心技术：百亿特征实时推荐算法揭秘

https://mp.weixin.qq.com/s/xV5cLbCPb7Nh6i4i7DxJIQ

没人告诉你的大规模部署AI高效流程！

https://mp.weixin.qq.com/s/8R7YhcZ_Dt0oFIF3bQovxw

为了提升DL模型性能，阿里工程师打造了流式编程框架

https://mp.weixin.qq.com/s/z6gXp-EeDID1ed8_DsUbOg

90秒训练AlexNet！商汤刷新纪录

https://mp.weixin.qq.com/s/HQW2bPyDY_3ecZWP6NYr-w

大规模机器学习在LinkedIn预测模型中的应用实践

https://mp.weixin.qq.com/s/i1PLA1xr3CefKx1EcVUVIg

谷歌破世界纪录！圆周率计算到小数点后31.4万亿位

https://mp.weixin.qq.com/s/rX8L63-jDGJT6lCAj04I3Q

独家解读！阿里重磅发布机器学习平台PAI 3.0

https://mp.weixin.qq.com/s/Ye2GVTFIrX3SbU1-4cDLoQ

你天天叫的外卖，你知道这里面深度学习的水有多深吗

https://mp.weixin.qq.com/s/FIWfbCLgckVzeNvfThIl4Q

阿里线下智能方案进化史

https://mp.weixin.qq.com/s/pqxiF6yEZzrw8qXu2hEsaA

单机训练速度提升640倍！独家解读快手商业广告模型GPU训练平台Persia

https://mp.weixin.qq.com/s/Jcz4XWDjMmbhmAiI_zBQXQ

流式计算优化：时效性

https://zhuanlan.zhihu.com/p/33351291

基于忆阻器（ReRAM），Computing-in-Memory的DLA

https://mp.weixin.qq.com/s/UbZtUL6Iveb4S3nTU0liGw

深度神经网络的分布式训练概述：常用方法和技巧全面总结

https://mp.weixin.qq.com/s/kLXJsHbBnRIFC3NLChPhzA

如何高效进行大规模分类？港中文联合商汤提出新方法

https://www.zhihu.com/question/454589636

为什么模型和数据都在gpu上，却打不满GPU的使用率？

https://mp.weixin.qq.com/s/sn8fMAbJbeT6JUbCpBpN6A

Jeff Dean与David Patterson：不思考体系结构的深度学习研究者不是好工程师

https://mp.weixin.qq.com/s/6zLrWJ4nE0bHFlVe5dMxHw

分布式深度学习新进展：让“分布式”和“深度学习”真正深度融合

https://mp.weixin.qq.com/s/hjC-WTMIpbWWpmXoLBfD2g

腾讯大规模分布式机器学习系统无量是如何进行技术选型的？

https://mp.weixin.qq.com/s/mg-d1W5i9rzaLMNrvq0tSQ

32分钟训练神经机器翻译，速度提升45倍

https://mp.weixin.qq.com/s/iW0k80TUPuWDE9xwHvX91g

为什么你需要Raven：全球首个真正分布式深度学习训练协议

https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650750181&idx=1&sn=156dac3c5646143fc2577972f1506836

GPU捉襟见肘还想训练大批量模型？谁说不可以

https://mp.weixin.qq.com/s/-CTVyKWtdTK0RIfzzPVyNQ

分布式与高效深度学习，140页ppt详述深度学习压缩与联邦学习训练技术进展

https://mp.weixin.qq.com/s/nTBuYuW7h9wZYuo3w1xGmQ

分布式训练的方案和效率对比

https://mp.weixin.qq.com/s/LOTQfD9KKtAq0zz4rObCGA

EB级系统空中换引擎：阿里调度执行框架如何全面升级？（DAG 2.0）
