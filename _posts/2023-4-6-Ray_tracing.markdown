---
layout: post
title:  Ray Tracing
category: technology 
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
