---
layout: post
title:  设计模式
category: resource 
---

* toc
{:toc}

# 设计模式

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

---

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

---

参考：

https://mp.weixin.qq.com/s/9gDGQhzRAL3pj35VAinZbQ

设计模式在外卖营销业务中的实践

https://mp.weixin.qq.com/s/KYq_nEXQ-5WYdYzZvGLGGg

我向面试官讲解了单例模式，他对我竖起了大拇指

http://c.biancheng.net/design_pattern/

23种设计模式全面解析

https://mp.weixin.qq.com/s/H2toewJKEwq1mXme_iMWkA

设计模式二三事

# 新冠疫苗+

https://mp.weixin.qq.com/s/ZXWV8joWMNCbOfH04_TbXA

美国CDC的“Delta末日文档”：感染人数激增，接种疫苗仍传播病毒？

https://mp.weixin.qq.com/s/krRGv6N20YALpG-STMbIzg

以色列新冠疫苗真实世界数据，有什么用？

https://mp.weixin.qq.com/s/U1eteX7FwhyW-4qhVX4_bQ

多国暂停牛津新冠疫苗接种，接下来呢？

https://mp.weixin.qq.com/s/5E9h6snRo_2OuPJqInYvjw

阿斯利康疫苗接种后出现脑血栓，意味着什么？

https://mp.weixin.qq.com/s/qzUObhyykBn88EgV0PK9Dg

姗姗来迟，科兴新冠疫苗究竟怎么样？

https://mp.weixin.qq.com/s/YEt7XH9mNPmx3QURebbBmg

什么是最好的新冠疫苗？

https://mp.weixin.qq.com/s/pkgbDDBv-OPGHyeEBVDXQQ

国产疫苗，对新冠病毒突变株效果如何？
