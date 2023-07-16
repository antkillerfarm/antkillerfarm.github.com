---
layout: post
title:  Ray Tracing, 数字成像（二）
category: resource 
---

* toc
{:toc}

# Ray Tracing

教程：

Ray Tracing in One Weekend

Ray Tracing The Next Week

Ray Tracing The Rest Of Your Life

---

IMG定义的光线追踪6个等级：

- Level 0：传统解决方案。

早期在光线追踪领域的探索，例如魔改GPU架构，使用非标的API等。毫无疑问，这些早期的尝试为现在的实时光线追踪解决方案铺平了道路，但它们并没有成功，因此称之为Level 0解决方案。

- Level 1：传统GPU上的软件。

由于Level 0解决方案的主要缺陷是生态系统和兼容性。那一个过渡的解决方案就是使用现有的图形和计算API来实现光线跟踪。然而光线追踪本身的计算成本很高，而且过于复杂，无法以通用API类型的方式处理它。这意味着必须将问题空间缩小，并且在使用大量技巧（算法优化）的情况下才能发挥作用。因此由软件实现的光线追踪，通常不能做到真正的real-time rendering。软件解决方案在帮助建立功能需求方面发挥着关键作用，但因为在性能、功耗和带宽效率方面的缺陷，软件天生就会远落后于专用的硬件解决方案。

- Level 2：硬件中的Ray-box和Ray-Triangle求交测试器。

用专门的fix-funciton硬件来做光线的求交测试，将极大的释放GPU ALU的算力。以最简单的形式来说，这是实现实时光线追踪的基础。IMG称之为Level 2。

- Level 3：硬件实现BVH遍历。

在Level 2中，除了Ray-box和Ray-Triangle求交测试有专门的硬件做，其他的还是由软件做。level 3就是进一步扩展光线追踪硬件，每条射线的BVH遍历完全由专用逻辑处理，除了从ALU上卸载更多负载外，还增加了缓存和数据流从而实现更广泛的并行效率。

- Level 4：具备硬件BVH处理和相关性分类功能。

光线追踪遇到的发散问题，破坏了GPU能高效工作的基本前提。针对此问题IMG提出一种相干性引擎，该引擎可以追踪光线，并且在场景中所有混乱的光线之间找到一些行为类似的光线。相干射线引擎(Coherency Engine for rays)实现了将射线分类成相干束，可以有效地共享内存访问，保证完美的缓存命中，从而使用更少的带宽。

Level 5：带有BVH硬件生成器的相干性BVH处理。

在这之前，这个BVH结构的创建是在软件中完成的，就是说使用CPU和/或GPU计算路径完成的。Level 5使用专门的硬件优化实现BVH创建，并与3级和4级解决方案中启用的BVH遍历方法一起工作。

---

光线跟踪也不一定是全局光照(Global Illumination)，只有考虑全反射（包括跟踪漫反射）的光线跟踪才是全局光照。

辐射度(Radiosity)算法是计算全局光照的的算法。

全局光照 = 直接光照(Direct Light) + 间接光照(Indirect Light)。

全局光照是既考虑场景中直接来自光源的光照（Direct Light）又考虑经过场景中其他物体反射后的光照（Indirect Light）的一种渲染技术。

---

https://www.cnblogs.com/mengdd/p/3237991.html

图形学理论知识BRDF双向反射分布函数（Bidirectional Reflectance Distribution Function）

http://blog.csdn.net/pizi0475/article/details/6650851

全局光照模型与Rendering Equation

https://www.embree.org/

Intel Embree：一个光线追踪方面的库。

https://www.jianshu.com/p/4ec47871973c

Path Tracing介绍

https://mp.weixin.qq.com/s/95P3UgCiqlfPHL1H8C6sRw

关注光线追踪技术

https://mp.weixin.qq.com/s/4NckWjtMCyNGtf83OEy2Hw

什么是光线追踪？它如何实现实时三维图形？

https://www.zhihu.com/question/29863225

Ray Tracing，Ray Casting，Path Tracing，Ray Marching的区别？

https://www.zhihu.com/question/310930978

实时光线追踪（real-time ray tracing）技术还有哪些未攻克的难题？

https://blog.csdn.net/qq_35312463/article/details/121754345

Global Illumination_Voxel Global Illumintaion (VXGI)

https://www.cnblogs.com/machong8183/p/7543724.html

全局光照:光线追踪、路径追踪与GI技术进化编年史

https://gamebaby.blog.csdn.net/article/details/80399195

光线追踪渲染（RayTracing Render）核心原理详解

# CPU渲染 vs GPU渲染

CPU渲染可以在像素级别操作，而GPU渲染单位是三角形，同时需要有拼接三角形的过程。而拼接三角形可能会存在一些精度问题，可能导致嵌接之后容易出现噪点、不期望的实线等问题。

皮克斯公司、AfterEffect软件使用的是CPU渲染。影视场景主流是CPU渲染。由于GPU渲染比CPU渲染快，所以实时渲染如网页、游戏的场景主流是GPU渲染。

https://zhuanlan.zhihu.com/p/618925299

从两个图形库看CPU与GPU渲染的差异（Cairo与Skia）

# 数字成像

https://mp.weixin.qq.com/s/Fpy3_kljryrjEqoZz4IlNg

电子后视镜（一）——相关标准汇总

https://mp.weixin.qq.com/s/aUAD-SWJ6PEplrFRb5SHZg

消除摩尔纹的光学方法

https://mp.weixin.qq.com/s/yBd9qcmMHKIGx88voovFgw

有趣的偏振相机

https://mp.weixin.qq.com/s/eVtbmy8rJ6V2j-6XJ0JJCA

夜景拍照之技术创新

https://mp.weixin.qq.com/s/DH3mZgh4YbnceaFGs03kOQ

曹汛：计算摄像学研究

https://mp.weixin.qq.com/s/f2BIrZ8if7_rX__J9hMHog

广色域---iphone X/8 camera的色彩进化

https://mp.weixin.qq.com/s/VjzXYgkzAGloczaaPXni2A

PDAF进化史

https://mp.weixin.qq.com/s/vg64EpcCVsk7gQAoEozSRQ

PDAF进阶

https://mp.weixin.qq.com/s/m7q6jHbdnb7fw4lKIDfVWw

camera接口之MIPI联盟浅谈

https://mp.weixin.qq.com/s/DbDrDhbQAhXOsfxQ8UJVZg

三星S9凝时摄影背后的sensor技术

https://mp.weixin.qq.com/s/gOikxUxWpqdDRr6_KT2jxQ

图像处理，计算机视觉与machine learning的区别与联系

https://mp.weixin.qq.com/s/7fvVmmpPSldwa3TZ3dVb2g

3D LUT--色彩校正的利器

https://mp.weixin.qq.com/s/m_EKOLjWjTyx4RBxsAAAGg

4x4阵列摄/“深感”摄

https://mp.weixin.qq.com/s/dX_thGA2V_-AMjDGTUgWrg

计算摄影技术：身怀绝技的扫地僧

https://mp.weixin.qq.com/s/ZSczU1_J_exb2oreiHtK3g

夜景拍照之sensor篇

https://mp.weixin.qq.com/s/eyIeLaBZ0f_EsxglsUuH8A

深度学习自动构图研究报告

https://mp.weixin.qq.com/s/k2JLF_aM3j68GjEByCZF-g

模拟/数字增益对图像噪声的影响

https://mp.weixin.qq.com/s/QmEQuEk2B_fAleewTuNClg

3D成像技术和CMOS传感器的发展方向简析

https://mp.weixin.qq.com/s/R7wnyHT6M-KF3ZMz0bPrVg

这台相机没镜头！美国教授新发明，一块玻璃可成像，拍照给计算机看

https://mp.weixin.qq.com/s/j970Qp8Cz2fpbImujkS2Xg

自动驾驶应用中的LED flicker问题

https://mp.weixin.qq.com/s/Yy-XEyM0YGTRFUX2uWgNKw

夜景拍照之算法篇

https://mp.weixin.qq.com/s/h7MQhZjnP3lLkLD0cKNixA

高动态范围成像的理论基础

https://mp.weixin.qq.com/s/Wd6LD8Rme8XJwtYWU-ALtQ

动态范围压缩（DRC）的四种方法

https://mp.weixin.qq.com/s/7_mDPCEq3S-843PwUQSwXA

车载相机image sensor选型

https://mp.weixin.qq.com/s/byeQKa7d5FSLS7JrBwP_Zw

什么是多光谱/高光谱成像

https://mp.weixin.qq.com/s/nz9NHdyhRqxdhYopFQ_5Ag

红外热成像与快速发热病患检测

https://mp.weixin.qq.com/s/Htn1Il96fus7s8NkD2_98w

Google Pixel4相机解密(1)夜景

https://mp.weixin.qq.com/s/2uNcJxU-USpb-LZkka__eQ

剖析苹果的激光雷达及参数估计

https://mp.weixin.qq.com/s/DLsmHtUz3gAXMdzNsUTyFA

苹果的DTOF中的SPAD原理和特性

https://mp.weixin.qq.com/s/S8Wda_orUyco43FzV0cEeQ

紫边/紫晕与图像传感器的关系

https://zhuanlan.zhihu.com/p/74085115

深度学习在3-D环境重建中的应用

https://mp.weixin.qq.com/s/k9gff0Ote_V9qtN7HAruBA

工业相机的sensor

https://mp.weixin.qq.com/s/kvINl4pL4_wD71e33jaUSg

下一代cmos image sensor---有机光导薄膜图像传感器

https://mp.weixin.qq.com/s/024h8tPvCPPUyuT53CCPWQ

液态镜头的发展与应用

https://mp.weixin.qq.com/s/0XlH3fFKZBfX3UMq7eTbYw

什么是event camera

https://mp.weixin.qq.com/s/5fQDFlR5NZNW_HS2cOfTzg

高光谱成像技术的介绍

https://mp.weixin.qq.com/s/tSc_VKlohhhpT-3l37Bx0w

激荡二十年——手机相机技术的发展历程

https://mp.weixin.qq.com/s/GYpuj1rPRknWsRX6Av9XsQ

各种有趣的屏下摄像头

https://mp.weixin.qq.com/s/XihwGVU-8QJORfvENhhLnA

图像传感器厂家大盘点（上）

https://mp.weixin.qq.com/s/wSgSZA2c0fP3E8JgHn6yRQ

AI时代的去马赛克算法

https://mp.weixin.qq.com/s/YKL0Cwp1l9_4lAj8knMIqQ

高级AI视觉算法是否还需要ISP?

https://mp.weixin.qq.com/s/x4cz9mW1HOF8rtRv4uqLCQ

门控相机：车载ADAS相机的未来？
