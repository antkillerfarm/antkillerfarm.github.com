---
layout: post
title: 计算机体系结构
category: Chip 
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

Intel的SSE/AVX，ARM的Neon/SVE，都属于SIMD指令。

参考：

https://mp.weixin.qq.com/s/n_V_-TOvu5hLZ0BwKpgy9A

计算机体系结构发展史

https://zhuanlan.zhihu.com/p/31271788

SIMD指令集

https://mp.weixin.qq.com/s/UExKPAMbA9EfRW35P3WnFA

RISC-V其实是反潮流，但是……

## SPMD和MPMD

Single Program Multiple Data (SPMD)：

![](/images/img4/spmd_model.gif)

单个程序：所有任务同时执行同一程序的副本。该程序可以是线程，消息传递，并行数据或混合数据。

多个数据：所有任务可能使用不同的数据。

Multiple Program Multiple Data (MPMD)：

![](/images/img4/mpmd_model.gif)

多个程序：任务可以同时执行不同的程序。程序可以是线程，消息传递，并行数据或混合数据。

多个数据：所有任务可能使用不同的数据。

SPMD和MPMD都是编程模型的概念。

**编程模型就是对编程共性的抽象。而体系结构是对于硬件的抽象。**

---

SIMT: Single Instruction Multiple Threads，多个thread运行同一指令，但各自处理的数据可以不同（一般每个线程的数据，尤其是寄存器是独立的）。

SIMT最早是Nvidia发明的概念，它也是一种编程模型。

SIMD的编程模型除了SIMT之外，还有subword SIMD、vector SIMD、vector-thread (VT)等。

SMT（Simultaneously multithreading），指多个thread中并行运行不同的指令。

详见论文：

《Simplified Vector-Thread Architectures for Flexible and Efficient Data-Parallel Accelerators》

https://zhuanlan.zhihu.com/p/562135333

SIMT、SIMD和DSA（1）

https://zhuanlan.zhihu.com/p/113360369

从现代GPU编程角度看SIMD与SIMT

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

---

英特尔并不是首个将x86前端解码器与所谓的RISC风格后端结合起来的x86 CPU制造商，被AMD收购的NexGen同样如此。NexGen 5×86 CPU于1994年3月首次亮相，而奔腾Pro直到1995年11月才推出。

https://mp.weixin.qq.com/s/HlOqQUCPljvnxNxqathWcA

都2021年了，还把x86和ARM归为CISC和RISC？

https://blog.csdn.net/windeal3203/article/details/51985832

RISC诞生与发展的缩影

---

![](/images/img3/ISA.png)

![](/images/img3/ISA_2.png)

https://mp.weixin.qq.com/s/8w3V-ndKBD2fReM9QZokwA

ARM架构要逆天？

https://www.zhihu.com/question/267873770

请问X86与ARM的功耗控制有什么区别？

---

No instruction set computing：上面提到指令需要分解成微指令才能执行，如果编译器能直接生成微指令的话，那么指令集也就不需要了，这就是NISC。

One-instruction set computer：使用Bit-manipulating之类的技术，可以用1条指令搭建一个图灵完备的机器。

## 定律

Amdahl's rule（阿姆达尔定律）：整个计算系统的性能提升取决于速度最慢的这一部分。

如果f是被并行化代码的比率，并且如果并行化版本在一个有p个处理器的机器上运行，且没有任何通信或者并行化开销，那么此时的加速比是:

$$\frac{1}{(1 - \mathcal{f}) + (\mathcal{f} / \mathcal{p})}$$

Gustafson's law（古斯塔夫森定律）：问题规模足够大（必须串行执行的时间相对小）时，增加核心数目是好的。

两个定律，一个悲观，一个乐观，但是并不矛盾。

https://www.jianshu.com/p/2a756f9c379f

阿姆达尔定律和古斯塔夫森定律

## 术语

Shared Virtual Memory, SVM

Memory consistency model

Multisample anti-aliasing, MSAA

## 模拟器

计算机系统架构模拟器gem5。

https://zhuanlan.zhihu.com/p/487737252

gem5模拟器入门

https://zhuanlan.zhihu.com/p/678502885

从零开始实现GameBoy模拟器

## 多核

SMP(Symmetric Multiprocessing)架构：即多处理器架构，是目前最常见的多处理器计算机架构。

AMP(Asymmetric Multiprocessing)架构：即非对称多处理器架构，则是与SMP架构相对的概念。

https://zhuanlan.zhihu.com/p/53596593

所有CPU内核一定生来平等吗？Intel非对称异构多处理器：Lakefield

https://en.wikipedia.org/wiki/ARM_big.LITTLE

ARM的big.LITTLE架构

## NUMA

在SMP架构中，又可以分为NUMA架构和UMA架构。

NUMA(non-Uniform Memory Access)非均匀内存访问架构是指多处理器系统中，内存的访问时间是依赖处理器和内存之间相对位置的。在这种设计里面存在和处理器相近的内存，通常称作本地内存；还有和处理器相对远的内存，通常称之为远端内存。

UMA(Uniform Memory Access)均匀内存访问架构则是与NUMA架构相反，所以处理器对共享内存的访问距离和时间是相同的。

![](/images/img2/NUMA.jpg)

![](/images/img4/NUMA.jpg)

上图是NUMA（Non-Uniform Memory Access）的结构图。可以看到CPU和一部分内存直接相连，而和其他内存通过总线相连。接近NUMA结点的内存称为本地内存，其他NUMA节点的内存称之为远端内存。

如果我们把局部数据放到直连内存上，CPU显然就能够更快速的访问数据。

与NUMA相关的还有一个著名的BUG：

https://zhuanlan.zhihu.com/p/387117470

十年后数据库还是不敢拥抱NUMA？

参考：

https://software.intel.com/en-us/articles/optimizing-applications-for-numa

Optimizing Applications for NUMA

https://mp.weixin.qq.com/s/uH0XRjDNfVQe5r1aes0zDw

性能之殇：从冯·诺依曼瓶颈谈起

https://zhuanlan.zhihu.com/p/476411477

NUMA架构详解

## VLIW & superscalar

超长指令字(VLIW:Very long instruction word)和超标量（superscalar）都在同一个CPU中，集成了数套运算单元。

超标量用硬件来决定哪些指令可以并行执行，而VLIW采用软件来决定哪些指令并行，通过把指令调度的复杂度交给编译器来降低硬件复杂度。

VLIW的代表是Intel的Itanium处理器。当处理器的执行宽度(execution width)，指令执行延迟时间，执行单元个数(function unit)改变时，VLIW需要重新编译程序来适应。但是Superscalar却不需要。

因此，VLIW在GPU中用的比较多。GPU程序并不直接生成GPU指令，而是通过厂商提供的DX/OpenGL库操作GPU。因此这些重新编译程序的任务已经由厂商完成，对于使用者透明。

https://mp.weixin.qq.com/s/OQ3KUAi6HMyOS8k3J_1e6g

关于超长指令集VLIW的一些讨论

https://www.zhihu.com/question/430177243

CPU长指令(VLIW)失败的主要原因是什么，VLIW真的无药可救吗？

## DSM

Distributed Shared Memory是分布式系统的所有节点（处理器）共享的虚拟地址空间。程序访问DSM中的数据的方式与访问传统计算机虚拟内存中的数据的方式非常相似。

分布式共享内存(DSM)在10几年前是OS领域的研究热点，不过因为网络传输的性能太差了，所以凉了。而基于消息传递模型的分布式计算发展了起来。其中数据以消息的形式从处理器传递到处理器。RPC实际上也是相同的模型。

https://blog.csdn.net/JiangTao2333/article/details/124530699

分布式共享内存（DSM - Distributed Shared Memory）

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
