---
layout: post
title: 计算机体系结构
category: resource 
---

* toc
{:toc}

# 计算机体系结构

按照Michael J. Flynn的分类方法，计算机的体系结构可分为如下四类：

**Single instruction stream single data stream (SISD)**

**Single instruction stream, multiple data streams (SIMD)**

**Multiple instruction streams, single data stream (MISD)**

**Multiple instruction streams, multiple data streams (MIMD)**

![](/images/article/simd_mimd.png)

原图地址：

https://en.wikipedia.org/wiki/Flynn%27s_taxonomy

论文：

《Very High-Speed Computing Systems》

>Michael J. Flynn，1934年生，美国计算机科学家。Manhattan College本科（1955）+Syracuse University硕士（1960）+Purdue University博士（1961）。Stanford教授，IEEE Fellow，ACM Fellow。

CPU通常是SISD和SIMD的，而GPU则是SIMD的，超级计算机则是MIMD的。

SIMT: Single Instruction Multiple Threads.

SIMT最早是Nvidia发明的概念，仅用于GPU领域。它和SIMD的差异很小。

参考：

https://zhuanlan.zhihu.com/p/31271788

SIMD指令集

https://zhuanlan.zhihu.com/p/113360369

从现代GPU编程角度看SIMD与SIMT

https://mp.weixin.qq.com/s/UExKPAMbA9EfRW35P3WnFA

RISC-V其实是反潮流，但是……

## RISC vs. CISC

一直以来大部分课本上的观点是这样的：RISC由于速度快，必将取代CISC，而之所以没取代，主要是由于现在的大部分软件只能运行在X86体系上，也就是说是由于非技术的市场原因导致的。

市场原因当然不在我这篇文章的考量范畴，但“RISC比CISC快”这个结论却值得打个问号。

课本中给出的解释是：CISC由于指令复杂，所以需要将每条指令分解成微指令，才能执行，因此指令的CPI（Cycles Per Instruction）要比RISC高，所以就慢。RISC尽管完成同样的功能需要更多的指令，但根据80/20的原则，总的来说还是要比CISC快。

应该说这个结论，在上个世纪90年代中期以前，的确是颇有道理的。但主板倍频技术的出现，让这个问题有了新的变化。在早期的X86计算机中，CPU和内存工作在同样的频率下，也就是说CPU和内存一样快。而使用倍频技术后，CPU频率和内存频率之间就产生了差距。这种差距到后期，竟然几乎相差了一个数量级，这个时候CPI就不再是决定CISC和RISC快慢的决定因素了。CISC由于指令密度高，在某些场合甚至比RISC快也就不足为奇了。

事实上，现在商用的CPU已经无法再用老的定义来分类了，因为他们都借鉴了对方的某些优点。

1.提高指令密度。ARM按照传统来说是RISC，但它引入了Thumb指令来提高指令密度，因此它的指令集不再是传统RISC的定长指令集。

2.减少CPI。Intel的X86 CPU最初只有一个指令解码器，而现在为了处理最常用的20%的简单指令，它引入了简单指令解码器的概念，再加上指令流水线的引入，它在CPI上的表现与传统的RISC已经没有什么太大的差别了。

顺便说一句，Linux在多核机器上引入了“自旋锁”的概念。自旋锁的实现，说白了就是一种有条件的死循环，从表面看，死循环空占CPU，似乎是种很低效的方案，但由于CPU和其他设备之间速度的巨大差异，有时候这种方案反而是最快的方案。道理和上面的RISC vs. CISC是类似的。

总之，每个时代限制系统速度的瓶颈不同，因而完全有可能昨天的结论和今天的结论完全相反。

![](/images/img3/ISA.png)

![](/images/img3/ISA_2.png)

https://mp.weixin.qq.com/s/8w3V-ndKBD2fReM9QZokwA

ARM架构要逆天？

---

No instruction set computing：上面提到指令需要分解成微指令才能执行，如果编译器能直接生成微指令的话，那么指令集也就不需要了，这就是NISC。

One-instruction set computer：使用Bit-manipulating之类的技术，可以用1条指令搭建一个图灵完备的机器。

## 术语

Shared Virtual Memory，SVM

Memory consistency model

Multisample anti-aliasing，MSAA

Amdahl's rule，整个计算系统的性能提升取决于速度最慢的这一部分。

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

https://mp.weixin.qq.com/s/OQ3KUAi6HMyOS8k3J_1e6g

关于超长指令集VLIW的一些讨论

https://mp.weixin.qq.com/s/Ud4f5L2tYNbP1oURVBszag

俄罗斯自研Elbrus CPU参数曝光，CEO年近九旬仍未退休

>Boris Babayan，1933年生，俄罗斯科学院院士，Intel院士。俄罗斯CPU之父。

https://mp.weixin.qq.com/s/iBgTyDiNNTUuuCgWJCyKig

深度报告：GPU产业纵深及国产化替代

## DPU

如同GPU是针对图像显示领域的加速，DPU（Data Processing Unit）则是对于数据传输方面的加速。

https://zhuanlan.zhihu.com/p/145142691

什么是DPU？

https://mp.weixin.qq.com/s/bL1PoUjZ_sH2VKcBxI6N5A

Wave公司发布数据流处理架构DPU

https://zhuanlan.zhihu.com/p/409507738

写一下DPU

## GPU体系结构

![](/images/img4/SGI.png)

上图是SGI公司1993年的一款显卡的模块图。可以看出，早期的显卡是一个具有固定流水线的专用芯片，只能处理特定任务，没有多少可编程的能力。

![](/images/img4/Tesla.png)

到了2005年左右，这些渲染流水线的功能被统一到一个叫做Shader Processor的可编程单元中。此后，显卡也被称为GPU（Graphics Processing Unit）。

![](/images/img4/Shader.png)

无论是CPU，还是GPU，或者是现在比较火的NPU，一个Processing Unit，一般都是由上图所示的三部分组成：

- Fetch/Decode。数据的输入/输出单元。

- ALU（arithmetic and logic unit）。运算单元。

- Execution Context。寄存器单元。

CPU强调单core的极致性能，因此广泛使用了如下技术：

- Out-of-order control logic

- Fancy branch predictor 

- Memory pre-fetcher

![](/images/img4/GPU.png)

GPU不追求单core的性能，因此也没有那些复杂的控制逻辑。这样省下来的电路，可以变成更多的ALU。由于数据处理的局部性，通常可以若干ALU共享一套Fetch/Decode和Ctx。

当然，如果ALU非常多的话，就不能都共享一套了，这时可以采用下图所示的分组共享。

![](/images/img4/GPU_2.png)

其中的每个组，被称为一个**core**。

有利就有弊，下面看看GPU是如何执行条件语句的。

![](/images/img4/GPU_3.png)

上图展示了8个ALU并行执行条件语句的过程：

- 计算每个数据的条件，生成对应的标志。

- 执行True分支。

- 执行False分支。

可以看出，GPU对于条件语句的执行时间=True分支执行时间+False分支执行时间。而CPU只需要根据条件，执行一个分支就可以了。显然，GPU对于程序的控制能力，相对CPU简直弱爆了。GPU并不擅长做这些东西。

>CPU的SSE、AVX之类的指令集，也借鉴了GPU的设计思想，但是CPU的core数，相对于GPU而言，差了10～1000倍。

参考：

http://www.cnblogs.com/geniusalex/archive/2008/12/26/1941766.html

CPU GPU设计工作原理

https://mp.weixin.qq.com/s/-Wg1GtVGUxfshJ5d5NDd-Q

聊聊GPU的计算能力上限

https://mp.weixin.qq.com/s/zdr7BfJxVepQL1TCDXQoJA

一文带你了解GPU的前生今世

## Programming Model vs Hardware Execution Model

- **Programming Model**: how the programmer expresses the code.

Sequential (von Neumann), Data Parallel (SIMD), Dataflow, Multi-threaded (MIMD, SPMD), …

- **Execution Model**: how the hardware executes the code underneath.

Out-of-order execution, Vector processor, Array processor, Dataflow processor, Multiprocessor, Multithreaded processor, …

- Execution Model can be very different from the Programming Model.

von Neumann model implemented by an OoO processor

SPMD model implemented by a SIMD processor (a GPU)

GPU虽然是一个SIMD机器，但是并没有采用SSE、AVX之类的SIMD编程模型，而是SPMD模型。

# 机器人/无人驾驶参考资源++

https://zhuanlan.zhihu.com/p/57781001

自动驾驶系统的硬件平台讨论

https://mp.weixin.qq.com/s/ZUxkZXLC0tPkzptFKtudhw

让机器人也能“问路”的视觉语言导航新方法

https://mp.weixin.qq.com/s/6OkLjK1bMbw6a3bvYn_DCQ

深度学习在自动驾驶感知领域的应用

https://zhuanlan.zhihu.com/p/57029694

自动驾驶中单目摄像头检测输出3-D边界框的方法一览

https://mp.weixin.qq.com/s/SZlwjnZrxCyaqRBg_sjQaA

浅谈自动驾驶中的行为风险识别（一）
