---
layout: post
title:  设计模式, 数据结构 & 普通CS算法（二）
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

# 数据结构 & 普通CS算法

https://www.zhihu.com/question/623575952

算法复杂度为什么经常不考虑系数?

https://mp.weixin.qq.com/s/JiYRhcTv2qgLfVyGzI8uHQ

印度小哥用Python和Java实现所有AI算法

https://mp.weixin.qq.com/s/52JFdyaLwZq7nWiZq6z6qA

二叉堆是什么鬼？

https://mp.weixin.qq.com/s/4V1E2L14c3k-5i0c-JyTVQ

如何在10亿数中找出前1000大的数

https://mp.weixin.qq.com/s/yNn57Uw_-itzZXCSEAzrJw

一起搞定面试中的二叉树（一）

https://mp.weixin.qq.com/s/msv2NZ0aG96d3xQhX3HhNA

一起搞定面试中的二叉树（二）

https://mp.weixin.qq.com/s/t7Q0slX3q8Qlhg8F8pXrZQ

如何找到字符串中的最长回文子串？

https://mp.weixin.qq.com/s/d9yNsUVFg9UZN62xuOdxow

为什么MySQL数据库要用B+树存储索引？

https://mp.weixin.qq.com/s/o0JFTpGa4MLtDKHf4B2Ing

快速理解为啥这个查询使用索引，那个查询不使用索引

https://mp.weixin.qq.com/s/CtPywscoA_FvF2d9NLEenw

如何从100亿URL中找出相同的URL？

https://mp.weixin.qq.com/s/bdZ5e8CaiPuI8TpLNRrFUQ

大数据近似最近邻搜索哈希方法综述（上）

https://mp.weixin.qq.com/s/jiZw-x6EMhUIySIObm5XjA

大数据近似最近邻搜索哈希方法综述（下）

https://mp.weixin.qq.com/s/jeQawOIomUAjIp7GhuBk3A

一致性哈希算法及其在分布式系统中的应用

https://blog.csdn.net/cywosp/article/details/23397179

五分钟理解一致性哈希算法(consistent hashing)

https://mp.weixin.qq.com/s/jxr2titD0BXBUEmAwkOi7A

一致性哈希

https://www.cnblogs.com/BCOI/p/9072444.html

TREAP

https://blog.csdn.net/simpsonk/article/details/72832959

史上最强图解Treap总结

https://mp.weixin.qq.com/s/SIsNagNKudVUFPKyCaUYCw

Treap——堆和二叉树的完美结合，性价比极值的搜索树

https://blog.csdn.net/yishizuofei/article/details/81660841

多路查找树：2-3树、2-3-4树、B树、B+树、B*树、R树

https://mp.weixin.qq.com/s/D9kdAPws1XXZUyd0IKUzyw

理解B+树

https://mp.weixin.qq.com/s/l08OYNlTxDKGEQnULjPV6g

你管这破玩意叫B+树?

https://mp.weixin.qq.com/s/8gDVqlywLBl-MZa6XrtXug

为什么磁盘存储引擎用b+树来作为索引结构？

https://mp.weixin.qq.com/s/M5syxE9Ln4UDLThPh5iuJg

各种字符串Hash函数比较

https://blog.csdn.net/wo541075754/article/details/54632929

Merkle Tree（默克尔树）算法解析（Merkle Tree，通常也被称作Hash Tree，顾名思义，就是存储hash值的一棵树。）

https://mp.weixin.qq.com/s/M8U9B7UA2AdfnJi5EpTv-g

算法面试中经常问的“字符串”问题

https://mp.weixin.qq.com/s/QHounf4el7nmXnpMSJHjvg

这个问题不简单：寻找缺失元素

https://blog.csdn.net/changtao381/article/details/8936765

Splay Tree（一种二叉排序树）

https://mp.weixin.qq.com/s/p4tddWB4kjFufkv3x2SYpw

图解6种树，你心中有数吗

https://mp.weixin.qq.com/s/fAYjIcFlHoXK2E38JJSFpA

C++优先队列priority_queue

https://mp.weixin.qq.com/s/KZq5SjPESQnQaNU1Mn5a-A

一文把三个经典求和问题吃的透透滴

https://www.zhihu.com/question/20298134

即时战略游戏中实用的寻路算法都有哪些，比较如何？

http://blog.codinglabs.org/articles/algorithms-for-cardinality-estimation-part-i.html

解读Cardinality Estimation算法

https://mp.weixin.qq.com/s/zQJve_w5OoM6u-WcSWArdQ

神速Hash

https://www.codeproject.com/Articles/69941/Best-Square-Root-Method-Algorithm-Function-Precisi

Best Square Root Method

https://zhuanlan.zhihu.com/p/332996578

陈丹琦分治算法
