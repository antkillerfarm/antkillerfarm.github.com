---
layout: post
title:  计算机图形学
category: CV 
---

* toc
{:toc}

# 计算机图形学

## 教程

《Fundamentals Of Computer Graphics》，Steve Marschner著，中文常称为“虎书”。

《Real–time Rendering》，Tomas Akenine-Moller等著。

GPU精粹三部曲：

- GPU Gems 1 （2004）
- GPU Gems 2 （2005）
- GPU Gems 3 （2006）
- GPU Pro 1 （2010）
- GPU Pro 2 （2011）
- GPU Pro 3 （2012）
- GPU Pro 4 （2013）
- GPU Pro 5 （2014）
- GPU Pro 6 （2015）
- GPU Pro 7 （2016）
- GPU Zen （2017）
- GPU Zen 2 （2019）

GPU Gems列的主编是Randima Fernando。

https://developer.nvidia.com/gpugems/

GPU Gems的在线版本

GPU Pro和GPU Zen系列的主编是Wolfgang Engel。他还有如下系列作品：

- ShaderX 1 （2002）
- ShaderX 2 （2003）
- ShaderX 3 （2004）
- ShaderX 4 （2006）
- ShaderX 5 （2006）
- ShaderX 6 （2008）
- ShaderX 7 （2009）

## 历史

Ivan Edward Sutherland，1938年生。计算机图形学之父。图灵奖获得者（1988）。

https://zhuanlan.zhihu.com/p/121868664

科学，艺术，天才，一篇计算科学的史诗，一场兑现的资本盛宴

## PBR

Physically Based Rendering: From Theory to Implemention

https://zhuanlan.zhihu.com/p/161950497

10分钟了解PBR流程-PBR基本原理和概念

https://mp.weixin.qq.com/s/h-vgHkzLjh9AAa6Z0lZqlg

基于物理渲染(PBR)的车漆技术

## 参考

实时(Realtime)光照：在运行时的每一帧进行光照计算。并且可以自由地修改物体和光源的位置和属性。

烘焙(Baked)光照：Editor提前在场景中进行光照计算，生成对应的光照数据，这个过程叫烘焙，后续在游戏运行时，不会再对该类型的光照进行计算，而是直接从光照贴图中获取数据。适用于场景中静态的光照和物体。

---

Vulkan加入VK_EXT_graphics_pipeline_library扩展，简称Vulkan GPL。

游戏加载时，不再是编译成千上万个完整的PSO（Pipeline State Object），而是独立地编译管线库。当游戏需要一个特定的渲染状态组合时，再调用vkCreateGraphicsPipelines，但这次不是从源码编译，而是让Vulkan把之前已经编译好的那库链接起来，生成一个可执行的完整管线。链接比编译要快上两到三个数量级，完全不涉及复杂的指令调度和寄存器分配，这个过程通常在亚毫秒级别就能完成，快到足以在运行时动态执行而不会被玩家察觉到卡顿。

https://www.zhihu.com/answer/1980711619291018138

为何dx12的游戏总有着色器编译的问题，而dx11和之前的游戏没有？

---

https://mp.weixin.qq.com/s/hI9Z3l2eVJxkPbL8zG5uGA

图形学基础，427页pdf

https://zhuanlan.zhihu.com/p/430541328

图形学基础篇

http://15462.courses.cs.cmu.edu/fall2020/courseinfo

Computer Graphics

https://www.tomlooman.com/stanford-cs193u/

Stanford CS193u: Video Game Development in C++ and Unreal Engine

https://mp.weixin.qq.com/s/oFcqOPQriTgWMvcUGXHlRQ

计算机图形学入门总结

https://www.zhihu.com/column/graphicon

一个图形学方面的专栏

https://github.com/KrisYu/computer-graphics-from-scratch-Notes

一个图形学方面的专栏

https://www.zhihu.com/column/c_1635772272538648576

现代图形引擎入门指南

https://zhuanlan.zhihu.com/p/32095589

在《硬影像》与罗登老师/导演聊渲染技术

https://blog.csdn.net/jaccen2012/article/details/80328043

跨平台渲染引擎简介

https://zhuanlan.zhihu.com/p/163305630

如何判断点在三角形内部

https://www.zhihu.com/column/c_1165601616035618816

一个图形学方面的专栏

https://mp.weixin.qq.com/s/MhGrLydVsbvkhZ5U820zTQ

哈佛小哥这个github仓库从零开始教你计算机图形学

https://mp.weixin.qq.com/s/7SurDN5gvLCEDbxe2gR58w

面向工程师的图像处理，438页pdf

https://www.zhihu.com/question/49812837

256字节3D程序是如何实现3D引擎的呢？

https://zhuanlan.zhihu.com/p/22337544

不只是噪音（Perlin噪音）

https://mp.weixin.qq.com/s/jq4B_4kUOZE8yN5y0u-5yg

万字长文！UCLA蒋陈凡夫12年自我回顾，图形学的终极浪漫

https://zhuanlan.zhihu.com/p/649971173

GPU渲染之路：从图形引擎到内核驱动(一、计算机图形系统概述)

https://zhuanlan.zhihu.com/p/650510512

GPU渲染之路：从图形引擎到内核驱动(二、跨平台引擎层)

https://zhuanlan.zhihu.com/p/651364842

GPU渲染之路：从图形引擎到内核驱动(三、用户态图形驱动层)

https://zhuanlan.zhihu.com/p/650597410

图形学八股

https://www.zhihu.com/answer/2046754950697564025

怎么用只有黑和白的点阵，让人眼看到连续的灰度？（Floyd-Steinberg抖动）

## 术语

Material Point Method（物质点法）

## Taichi

Taichi是胡渊鸣开发的一套计算机图形库。它拥有一套特有的DSL，并可将之编译为能在不同backend硬件上运行程序。

>胡渊鸣，清华本科（2017）+MIT博士（2021）。

官网：

https://taichi.graphics/

代码：

https://github.com/taichi-dev/taichi

胡渊鸣还开发了一个物理模拟方面的自动微分器——DiffTaichi：

https://github.com/yuanming-hu/difftaichi

一个卡门涡街的demo：

https://github.com/hietwll/LBM_Taichi

参考：

https://www.bilibili.com/video/BV1aL4y1a7pv

B站的太极图形课

https://zhuanlan.zhihu.com/p/97700605

99行代码的《冰雪奇缘》

https://mp.weixin.qq.com/s/zPvvf1VptQ1M7iVEcxN-AQ

计算机图形也能自动可微：MIT学神的微分太极框架开源

https://zhuanlan.zhihu.com/p/507362284

99行代码能干啥？造个体素小世界！

https://www.zhihu.com/question/535601383

Taichi和PyTorch有哪些相似和不同？

https://zhuanlan.zhihu.com/p/573894977

用Taichi实现GPU图像处理：从入门到入魔

https://zhuanlan.zhihu.com/p/612102573

Taichi NeRF（上）：不写CUDA也能开发、部署Instant NGP

https://zhuanlan.zhihu.com/p/613679756

Taichi NeRF (下): 关于3D AIGC的务实探讨

## ZENO

https://mp.weixin.qq.com/s/6qtCwJpJ5zu65Wr15e08Lg

皮克斯华人CG老鸟深圳创业！低代码实现好莱坞大片特效

https://zhuanlan.zhihu.com/p/390717137

ZENO：一份不详细的使用说明

## 花边

胡渊鸣有多次开人都是在试用期的最后一天开的记录，开人之前几个月，先逼着榨干剩余价值，然后一脚踹，自己请客就是排队也要打票报销，实习生的差旅能不报尽量不报。。。

知道了这些，你就会觉得这样的pip对他来说太正常不过。更不用提什么请实习生吃火锅，自己先点土豆青菜，让实习生不好意思继续往下点菜，这样的挨屁馊德了。

https://www.zhihu.com/question/583806166

如何看待胡渊鸣太极图形团队新出绩效管理规则？

https://www.zhihu.com/question/676629193

如何评价张心欣开除zeno核心开发者小彭老师？

# 非洲=

统治已逾43载，92岁的保罗·比亚刚刚续任喀麦隆总统。

统治46年之久，83岁的特奥多罗·奥比亚克刚刚续任赤道几内亚总统。

统治41年不倒，80岁的丹尼斯·萨苏·恩格索刚刚续任刚果共和国总统。

统治39年有余，80岁的约韦里·穆塞韦尼刚刚续任乌干达总统。

罗伯特-穆加贝，坚持为津巴布韦人民服务37年（1980-2017），直到93岁高龄被政变推翻。

乍得的伊德里斯-代比和穆罕默德-代比父子，两代人已经坚持为人民服务35年（1990至今）。

多哥的纳辛贝-埃亚德马和福雷-纳辛贝父子，两代人已经坚持为人民服务58年（1967至今）。

加蓬的奥马尔-邦戈和阿里-邦戈父子，两代人坚持为人民服务55年（1967-2023），直到2023年被政变推翻。

吉布提总统伊斯梅尔-奥马尔-盖莱，已经坚持为人民服务26年（1999至今）。

---

蒙博托的一辈子，要分成两段来看，他的前半生干废了刚果奴隶主阶级，创造了刚果经济奇迹，一度让刚果和南非齐名，并称为撒哈拉以南双雄。

---

赤道几内亚的马西埃，经济上推行所谓的“非洲社会主义”，将经济资源收归国有也就是他自家所有；取消了国家的中央银行，下令将国家外汇存到自己的个人账户里。

政治上大搞清洗和个人崇拜，政府十二个部长，马西埃一个月内杀了十个；取缔了宗教，要求教会宣讲“马西埃是唯一真神”，“没有马西埃，就没有赤道几内亚”。

极端敌视知识，下令废除公立教育，禁止私人学校存在，关闭了全国的图书馆、报纸和杂志，搞的全国上下没有一个知识分子，教育程度最高的，也就是十来个技工。

赤道几内亚发现石油后，便是个人均gdp过万的国家.

奥比昂吸取了叔叔的教训，在比奥科岛上投资了数十亿美元，打造自己的豪华官邸，但是全国民众依然处于赤贫之中，他给出的唯一解决方案，就是买了几条护卫舰，不让民众过来。

---

内战爆发初期（2023–2024上半年），俄私兵→RSF；乌克兰国家力量→SAF，天然对立。

2024年下半年至今，俄罗斯和布尔汉（SAF高层）敲定红海海军基地长期租赁协议，官方全面倒向SAF。这个变化出现后，乌克兰的处境变得尴尬：自己全力扶持的SAF，成了俄罗斯现在的盟友。
