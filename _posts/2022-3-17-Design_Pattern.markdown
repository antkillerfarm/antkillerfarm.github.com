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

# 机器人/无人驾驶参考资源+

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

# 隋唐+

第一战，756年6月，灵宝之战哥舒翰败于崔乾佑。

第二战，756年10月，陈涛斜之战房琯败于安守忠。

第三战，757年5月，清渠之战郭子仪败于安守忠。

第四战，757年8月，香积寺之战郭子仪战胜安守忠。

第五战，757年10月，陕郡之战郭子仪战胜安庆绪。

第六战，759年2月，邺城之战，郭子仪为首的唐军败于史思明。

第七战，761年2月，洛阳之战，李光弼仆固怀恩败于史思明。

第八战，762年10月，洛阳之战仆固怀恩战胜史朝义。

https://www.zhihu.com/question/498799583

安禄山造反一年就死了, 为何安史之乱却还是打了八年呢？

---

以唐军举例，唐朝一个标准合成军包括4000骑兵+10000步兵+6000军医铁匠木匠工兵砲兵马夫厨师等非一线作战人员。

怛罗斯之战高仙芝战败，大量技术人员被俘，造纸术登技术传到了中东，促进了中东欧洲技术进步。

不仅造纸术，营建巴格达也是这批唐朝俘虏，就是因为他们营建巴格达的功绩，所以被阿拉伯给予自由身，这批唐朝俘虏在东地中海地区旅游一圈（耶路撒冷到亚历山大都去过）后搭船返回唐朝，其中一个叫杜环的将经历写成《经行记》。

---

边塞诗人的典型代表高适是沧州人，在河西节度使哥舒翰幕府工作过，岑参是荆州人，在安西节度使高仙芝幕府工作过。

他们不是不喜欢长安的繁花锦秀，实在是蹲在长安没出路，才到边塞谋前程的。

这些边塞诗人是有名的，其他没名气的更多。

沧州人严庄，在大唐的官职体系下没有出头之日，就去幽州投奔安禄山，三四年时间便出任主簿，成为安禄山的亲信谋士。

幽州人高尚，年轻时穷困潦倒，为了出人头地也投奔安禄山，数年时间便成为安禄山的掌书记，随时可以出入安禄山的卧室，亲信程度可见一斑。

---

唐朝科举不是宋明清，是权贵选拔。放现在就是，不论你啥水平，先找县委书记给你开推荐信，这叫举，老百姓上哪里找推荐信去？

有了推荐信直接去京城考进士，没有中间层次考试。

去了京城，先拜见考官，吹自己出身：小人家庭是五百年的婆罗门……

考试卷子不糊名不誊写，考官点卷子就是喊：汉东赵瑞龙公子的卷子给我找出来先……

---

鲁炅坚守南阳历时一年，挡住安禄山叛军南下襄、邓的道路。

---

https://www.zhihu.com/question/544661058

为什么古代政府机构名称以及官职名称很难理解？

https://www.zhihu.com/question/475910380

突厥民族为什么消失在历史长河中？
