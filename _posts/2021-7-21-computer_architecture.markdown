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

参考：

https://zhuanlan.zhihu.com/p/31271788

SIMD指令集

https://zhuanlan.zhihu.com/p/113360369

从现代GPU编程角度看SIMD与SIMT

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

SIMT: Single Instruction Multiple Threads.

SIMT最早是Nvidia发明的概念，它也是一种编程模型。

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

---

No instruction set computing：上面提到指令需要分解成微指令才能执行，如果编译器能直接生成微指令的话，那么指令集也就不需要了，这就是NISC。

One-instruction set computer：使用Bit-manipulating之类的技术，可以用1条指令搭建一个图灵完备的机器。

## 定律

Amdahl's rule（阿姆达尔定律）：整个计算系统的性能提升取决于速度最慢的这一部分。

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

https://www.zhihu.com/question/430177243

CPU长指令(VLIW)失败的主要原因是什么，VLIW真的无药可救吗？

https://mp.weixin.qq.com/s/Ud4f5L2tYNbP1oURVBszag

俄罗斯自研Elbrus CPU参数曝光，CEO年近九旬仍未退休

>Boris Babayan，1933年生，俄罗斯科学院院士，Intel院士。俄罗斯CPU之父。

# DPU

如同GPU是针对图像显示领域的加速，DPU（Data Processing Unit）则是对于数据传输方面的加速。

![](/images/img4/DPU.png)

![](/images/img4/DPU_2.png)

https://zhuanlan.zhihu.com/p/145142691

什么是DPU？

https://mp.weixin.qq.com/s/bL1PoUjZ_sH2VKcBxI6N5A

Wave公司发布数据流处理架构DPU

https://zhuanlan.zhihu.com/p/409507738

写一下DPU

https://www.zhihu.com/question/471238373

dpu芯片发展前景如何？

https://mp.weixin.qq.com/s/xRvXCpHpDnMqSNjJyIf3XQ

大话DPU

https://mp.weixin.qq.com/s/hN8tZ7xCRttIc-3pXdqElQ

中科院计算所牵头发布《专⽤数据处理器DPU技术白皮书》，94页pdf

# CPU+

https://mp.weixin.qq.com/s/lz6EoLiLtPtWFMG1FJCuQw

化合物半导体的机遇

https://mp.weixin.qq.com/s/tTtySk3u6m2BGUkDto5Ojw

可重构芯片的方法学原理

https://mp.weixin.qq.com/s/RkRZX_scr__Q_mLPhaaWAA

AMD Zen 2处理器架构为何要如此设计？

https://mp.weixin.qq.com/s/dRv4xWV8laCYBaZ0DwOC3A

如何从零开始设计一颗芯片？

https://mp.weixin.qq.com/s/eAzdtKvfw7EVz5Ax3nXeZg

自己动手设计专用处理器

https://mp.weixin.qq.com/s/NfNJVoAC5aW1sgToAmYsGw

3nm芯片终结者

https://mp.weixin.qq.com/s/o8C6n-ZRw3vFS8E-hikXJQ

移动的王者：深入分析ARM最强处理器Cortex A77

https://www.cnblogs.com/f-ck-need-u/p/11141636.html

关于CPU的一些基本知识总结

https://mp.weixin.qq.com/s/vTOGmcahRxQBH8RgkTJ2GA

中文系同学都能听懂的集成电路设计流程

https://mp.weixin.qq.com/s/O-2ecxYVOgEfRuwEoS7gRA

Fab的那些事儿

>Fab的全称叫Fabrication，也就是集成电路制造的工厂、车间。

https://mp.weixin.qq.com/s/_NcQ-PxNpEtpYdo450Uukg

半导体材料全球格局
