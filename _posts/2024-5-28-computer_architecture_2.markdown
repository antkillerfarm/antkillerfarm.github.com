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

# AI Chip+

https://zhuanlan.zhihu.com/p/231302709

聊聊GPU峰值计算能力

https://zhuanlan.zhihu.com/p/340775090

从Thinker到Evolver：对可演化AI芯片的探索

https://mp.weixin.qq.com/s/MwZ9j1MIwRBrJK4iWKzRqQ

芯故事：和AMD有渊源的那些AI创业公司

https://mp.weixin.qq.com/s/S1fVrSfAM_UJh06Q43s8vA

网络芯片架构的新改变

https://zhuanlan.zhihu.com/p/47904879

AI芯片在5G中的机会

https://mp.weixin.qq.com/s/Z_QVN7OCLqeyMrwK3Sc7qA

AI芯片和传统芯片的区别

https://mp.weixin.qq.com/s/mMiAGH2Yz42xes7jicyygA

“超级芯片”或在十年内诞生，摩尔定律再续一命！（自旋电子逻辑器件“MESO器件”）

https://zhuanlan.zhihu.com/p/503371595

AI芯片（下），自动驾驶里的“水浒卡”

https://mp.weixin.qq.com/s/Vt23nK5zC7Wwv7iiFAFvrQ

高通，玩转DSP和NPU

# 芯片行业+

Tape out是指芯片完成了设计，将设计数据交给fab开始生产，很多年前，完成的设计数据都是写到磁带里传给fab，设计团队将数据写入磁带叫tape in，fab读取磁带的数据叫tape out，现在科技发展了已经不用磁带了，但这个词还是沿用了下来。

wafer out是指wafer在fab完成了生产，设计的集成电路已经制造在硅基上了，开始要封装测试了。

---

全世界芯片行业目前的薪酬排名（仅对普通工程师而言）：

美国西部硅谷附近 > 洛杉矶附近 > 上海和深圳 > 美国东部 > 欧洲，欧洲约等于日韩台湾 > 印度 > 东南亚

---

当年韩国为了发展半导体，没有专家和技术人才就去日本请人家半导体公司的专家，周六，周日来到韩国给他们指导一下，给的工资是日本的三倍。

---

https://zhuanlan.zhihu.com/p/342808728

谈谈ARM/MIPS的Archi-license架构许可模式、IP core授权模式，及其商业规则

---

就晶圆代工而言，台湾在全球前十晶圆代工中占据了4位，分别是台积电，联电，世界先进和力积电，其中台积电的实力世界第一。

就芯片设计而言，台湾在全球前十芯片设计中占据了2位，分别是联发科和丽台。其它8位都是美国的公司。

就芯片封装而言，台湾在全球前十封装中占据了5位，分别是日月光，力成，京元电，南茂和欣帮。其中日月光的实力世界第一。

https://www.zhihu.com/question/658713475

台湾的芯片行业到底在世界上是什么水平？

# 天文杂谈+

恩克彗星是人们发现的第一颗短周期彗星，至今也还没有找到过周期比它更短的彗星。恩克用6个星期的时间，计算出这颗彗星的轨道，周期为3.3年，并且预言1822年5月24日再回到近日点，果然它准时回来了，成为继哈雷彗星之后，第二颗按预言回归的彗星。

>Johann Franz Encke，1791～1865，德国天文学家。University of Göttingen毕业，导师Gauss。1825年担任柏林天文台台长。Leopold Kronecker的导师。他还发现了土星环的Encke Gap。

---

1929年，余青松接任天文研究所第二任所长后，秉承高鲁的宏愿，费时五载，于公元1935年，建立紫金山天文台。1938年因抗日战争，他主持该台的内迁工作，并在昆明东郊建成了昆明凤凰山天文台。

https://mp.weixin.qq.com/s/vMe5RmD8uBW3AeGDYwbI5A

紫金山天文台的创立（高鲁、余青松）

---

https://view.inews.qq.com/wxn/20211224A03SC600

韦伯望远镜即将发射！梭哈百亿美元上天，是为了看啥？

https://view.inews.qq.com/wxn/20211225A0A9SJ00

韦伯望远镜成功背后：成本约110亿美元，共推迟发射18次

https://view.inews.qq.com/wxn/20211225A08N9G00

“鸽王之王”韦伯终于上天！比哈勃看得更远，烧钱更多

https://mp.weixin.qq.com/s/AIMccIbtB6W5nwu-6yRuJw

“鸽王” 韦伯上天之前，你需要知道这些

https://mp.weixin.qq.com/s/BYw9HJehGC528WkX_q4JLw

情人节将至，韦伯望远镜首次太空“睁眼”！

https://www.sohu.com/a/569342145_161795

韦伯太空望远镜主镜被太空陨石砸中，受损情况比最初想象要大

---

第四宇宙速度（fourth cosmic velocity），是指在地球上发射的物体摆脱银河系引力束缚，飞出银河系所需的最小初始速度。但由于人们尚未知道银河系的准确大小与质量，因此只能粗略估算，其数值在525公里/秒以上。

---

https://mp.weixin.qq.com/s/aY4gFbK_aKWcgwvr0Rk5gQ

“波多黎各，波多黎各，世界最大射电望远镜，倒闭了”（Arecibo望远镜）

https://www.zhihu.com/question/432120058

阿雷西博望远镜馈源舱支撑平台塌了，现已决定退役拆除，如何评价它的一生？

https://zhuanlan.zhihu.com/p/330003494

美国天眼塌了
