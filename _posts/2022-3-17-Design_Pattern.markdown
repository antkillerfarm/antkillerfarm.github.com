---
layout: post
title:  设计模式, Autoware
category: resource 
---

* toc
{:toc}

# 设计模式

Better Code, Better Doc, Empowering teams, Modern Code, Better Architecture

---

面向对象的设计模式有七大基本原则：

**开闭原则（Open Closed Principle，OCP）**：由勃兰特·梅耶（Bertrand Meyer）提出，他在1988年的著作《面向对象软件构造》（Object Oriented Software Construction）中提出：软件实体应当对扩展开放，对修改关闭（Software entities should be open for extension，but closed for modification）。

**单一职责原则（Single Responsibility Principle，SRP）**：由罗伯特·C.马丁（Robert C. Martin）于《敏捷软件开发：原则、模式和实践》一书中提出的。这里的职责是指类变化的原因，单一职责原则规定一个类应该有且仅有一个引起它变化的原因，否则类应该被拆分（There should never be more than one reason for a class to change）。

**里氏代换原则（Liskov Substitution Principle，LSP）**：麻省理工学院计算机科学实验室的里斯科夫（Liskov）女士在 1987 年的“面向对象技术的高峰会议”（OOPSLA）上发表的一篇文章《数据抽象和层次》（Data Abstraction and Hierarchy）里提出来的，她提出：继承必须确保超类所拥有的性质在子类中仍然成立（Inheritance should ensure that any property proved about supertype objects also holds for subtype objects）。

**依赖倒转原则（Dependency Inversion Principle，DIP）**：Object Mentor公司总裁罗伯特·马丁（Robert C.Martin）于1996年在C++ Report上发表的文章。高层模块不应该依赖低层模块，两者都应该依赖其抽象；抽象不应该依赖细节，细节应该依赖抽象（High level modules shouldnot depend upon low level modules.Both should depend upon abstractions.Abstractions should not depend upon details. Details should depend upon abstractions）。其核心思想是：要面向接口编程，不要面向实现编程。

**接口隔离原则（Interface Segregation Principle，ISP）**：2002年罗伯特·C.马丁给“接口隔离原则”的定义是：客户端不应该被迫依赖于它不使用的方法（Clients should not be forced to depend on methods they do not use）。该原则还有另外一个定义：一个类对另一个类的依赖应该建立在最小的接口上（The dependency of one class to another one should depend on the smallest possible interface）。

**合成/聚合复用原则（Composite/Aggregate Reuse Principle，CARP）**：它要求在软件复用时，要尽量先使用组合或者聚合等关联关系来实现，其次才考虑使用继承关系来实现。

**最少知识原则（Least Knowledge Principle，LKP）或者迪米特法则（Law of Demeter，LOD）**：1987年美国东北大学（Northeastern University）的一个名为迪米特（Demeter）的研究项目，由伊恩·荷兰（Ian Holland）提出，被UML创始者之一的布奇（Booch）普及，后来又因为在经典著作《程序员修炼之道》（The Pragmatic Programmer）提及而广为人知。

它的定义是：只与你的直接朋友交谈，不跟“陌生人”说话（Talk only to your immediate friends and not to strangers）。其含义是：如果两个软件实体无须直接通信，那么就不应当发生直接的相互调用，可以通过第三方转发该调用。其目的是降低类之间的耦合度，提高模块的相对独立性。

在架构领域，有两种常见架构方法RUP和TOGAF。

## GoF

《Design Patterns: Elements of Reusable Object-Oriented Software》，由 Erich Gamma、Richard Helm、Ralph Johnson 和 John Vlissides 合著（Addison-Wesley，1995）。这几位作者常被称为“四人组（Gang of Four）”。

## AOP

IoC（Inversion of control）：控制反转/反转控制。

DI（Dependency Injection）：依赖注入。

AOP（Aspect oriented programming）：面向切面编程

参考：

https://zhuanlan.zhihu.com/p/144241957

面试被问了几百遍的IoC和AOP ，还在傻傻搞不清楚？

https://mp.weixin.qq.com/s/IOV4FLJyxKM1q7Avh2j93g

漫画:AOP面试造火箭事件始末

https://segmentfault.com/a/1190000007469968

彻底征服Spring AOP之理论篇

https://segmentfault.com/a/1190000007469982

彻底征服Spring AOP之实战篇

https://mp.weixin.qq.com/s/5UwgQQHA-D0il0_8fDIW0A

Python面向切面编程AOP和装饰器

## 参考

https://mp.weixin.qq.com/s/9gDGQhzRAL3pj35VAinZbQ

设计模式在外卖营销业务中的实践

https://mp.weixin.qq.com/s/KYq_nEXQ-5WYdYzZvGLGGg

我向面试官讲解了单例模式，他对我竖起了大拇指

http://c.biancheng.net/design_pattern/

23种设计模式全面解析

https://mp.weixin.qq.com/s/H2toewJKEwq1mXme_iMWkA

设计模式二三事

https://refactoringguru.cn/design-patterns

一个设计模式的教学站

# Autoware

Autoware是另一个开源的无人驾驶平台。不像Apollo，没有百度这样的强势公司的介入，社区氛围更浓一些，相对的，功能也要弱一些。

官网：

https://www.autoware.org/

主要由一下组件构成：

- autoware.ai

https://www.autoware.ai/

这个组件基于ROS 1.0，是目前的方案。

- autoware.auto

https://www.autoware.auto/

这个组件基于ROS 2.0，是面向未来的方案。

- autoware.io

https://www.autoware.io/

autoware提供的模拟器。

代码仓库：

https://gitlab.com/autowarefoundation/autoware.ai

# 机器人/无人驾驶参考资源+

https://mp.weixin.qq.com/s/ao5hC_3A7fn8_L_PFw399A

轨迹规划技术分享

https://mp.weixin.qq.com/s/mPiAPT5hBlhR5gINIEMnkw

轨迹规划——算法综述

https://mp.weixin.qq.com/s/pEA7mN7AhxrDqLb3ku1CxQ

ADAS以及自动驾驶车辆运动特性

https://mp.weixin.qq.com/s/HSvy_mWcpoS4_-2izgXlzg

第一次有人把V2X讲的这么通俗易懂！

https://mp.weixin.qq.com/s/AekhB7D1W5UhkTmLikvpxA

ADAS系统横纵向控制策略之碰撞时间计算方法

https://mp.weixin.qq.com/s/ZEvNniUUzcsCbU-UpzxtEA

ADAS高级辅助驾驶视觉系统（Advanced Driver Assistant System）

https://zhuanlan.zhihu.com/p/73073753

基于深度学习的多传感器标定

https://mp.weixin.qq.com/s/m4KtRGoBgqcgF8ZBMjG6Hg

深层卷积神经网络在路面分类中的应用

https://mp.weixin.qq.com/s/nQq2tzK_2y2lEt9H14zdwA

自动驾驶中的决策规划算法概述

https://mp.weixin.qq.com/s/yIFgwxU-DI6NBogfmxqqKQ

基于深度学习的计算机视觉技术在无人驾驶中的应用

https://mp.weixin.qq.com/s/rA9AAVx7AlNuS1l4IUuG0w

深度学习技术在自动驾驶中的应用

https://mp.weixin.qq.com/s/4_jtb9gv20F6h1Ljw4JwEw

车载以太网通信的“套娃游戏”

https://zhuanlan.zhihu.com/p/86184886

行人的行为意图建模和预测(上)

https://zhuanlan.zhihu.com/p/86185203

行人的行为意图建模和预测(下)

https://zhuanlan.zhihu.com/p/90773462

多传感器数据深度图的融合：最近基于深度学习的方法

https://mp.weixin.qq.com/s/4tOYmCRiFN0xsG6vbedrrg

ADAS系统中的动态目标感知策略（一）

https://mp.weixin.qq.com/s/JT4p03m77ohOufL3JFecvA

从硬件角度剖析自动驾驶，为什么说它是复杂的系统工程？

https://mp.weixin.qq.com/s/UGdZCC80gQRTgHz9GV6USA

MEMS IMU/陀螺仪对准基础

https://mp.weixin.qq.com/s/v3gsmCWSI9pEwRolN8qWNA

基于深度卷积网络的自动驾驶多模态轨迹预测

https://mp.weixin.qq.com/s/R-17JgcGHG67KZ6yRYNFlA

基于Frenet优化轨迹的无人车动作规划方法

https://mp.weixin.qq.com/s/qZwqp5x6yEXbMBHGzevm0g

ACC自适应巡航控制系统介绍

https://zhuanlan.zhihu.com/p/57077589

自动驾驶中路上行人的行为和意图理解及预测

https://zhuanlan.zhihu.com/p/109900137

传感器融合-任务篇

https://zhuanlan.zhihu.com/p/109895639

传感器融合-数据篇

https://mp.weixin.qq.com/s/1sbL2vmugiIlSn_ehIOuig

车载多传感器融合定位方案：GPS +IMU+MM

https://mp.weixin.qq.com/s/5kJfhp3vi9uuSFaeONKfrA

打破传统方法，MIT新芯片帮自动驾驶汽车穿越浓雾

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

https://mp.weixin.qq.com/s/QBnvLrD93b8cDEeNeZ5kAw

车联网正步入歧途，命悬一线的开始

https://zhuanlan.zhihu.com/p/553588646

最新的“视觉为中心的BEV感知”综述论文

https://blog.csdn.net/v_JULY_v/article/details/141113628

2024自动驾驶(多模态)大模型综述：从DriveGPT4、DriveMLM到DriveLM、DriveVLM

# 常凯申+

在中美贸易战开始到现在，对台湾的凤梨、带鱼、山竹、红虾等农产品进行了多轮制裁，导致台湾的人均GDP“跌至”世界第七，亚洲第一，第一次超过日韩水平。人均年收入在2015年后持续升高，从马英九时代的11900美元升到2022年的15700美元。

---

重庆号上只有管武器的枪炮长是共产党，说是起义其实就是武装夺取。舰长实际没多少想法，只是投的比较快罢了。

---

张发奎，918事变后不久的1931年11月便宣布抗日，宣称要率军北上黑龙江，支援在黑龙江反正的马占山；南京政府听闻之后非常高兴，但是考虑到张发奎的部队在广西，北上黑龙江的路途过于遥远，不如直接率军前往上海，日军正准备进攻上海，在上海一样能抗日，但是你猜最后怎么着？

张发奎不愿意去上海，张发奎在后来在写回忆录时非常诚实的表示，第四军北上黑龙江是不现实的，所谓的抗日只是他的烟雾弹而已。他宣布抗日仅仅是因为与桂系不和，担心第四军被桂系吞掉，所以要找个高大上的借口离开广西。

1933年2月，日军进攻热河，东北军不战自退，随后爆发了长城抗战，长城抗日期间，冯玉祥始终未有任何动作，1933年5月19日，古北口战役结束，长城抗战基本结束，5月25日，中日双方宣布停火，同时进行停战谈判。

就在双方宣布挺火的第二天，也就是1933年5月26日，冯玉祥蹦出来宣布抗日了，在张家口发动政变，组建抗日政府，时任察哈尔省主席宋元哲正在北平指挥抗日，冯玉祥是他的老上司，虽然不赞成这一举动，但是也不便与老上司争执，所以只得让出了省主席的职务。
