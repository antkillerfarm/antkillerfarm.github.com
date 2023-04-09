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

# 俄乌战争+

普京搁这儿玩绝命时刻呢。

什么步兵将军，坦克将军，导弹将军，空天将军，大炮将军，核弹将军都得轮流换一遍。

每换一个媒体还配合介绍下这位将军特殊能力是啥。

真当玩游戏么是吧。

---

7月9日，俄军放出了一段视频，视频显示俄军正在使用OTR-21，即圆点-U弹道导弹打击乌军目标。视频存活的时间非常短，不到数分钟就被俄罗斯人撤下，因为他们突然想起来，自己在几个月前否认了还有圆点-U这一导弹的库存！如果大家忘记了，我想提醒各位的是，4月8日，俄军使用圆点-U攻击了乌占区的克拉玛托尔斯克火车站（关于乌占区很好例证，因为这地方是乌军纵深现在还在乌克兰手里），造成了大量平民伤亡。事后发现自己的所作所为过于反人类之后，俄军立刻“官宣”自己早已淘汰了所有的圆点-U，这导弹是乌克兰人打的！

---

乌克兰公路：T字在现在这个季节就是烂泥路，P字是硬化路面，H是高速，条件最好。

---

印度首富阿达尼因为大量购买俄罗斯原油转手出售赚取差价被人恶意做空，一天损失超过200亿美元。

---

乌克兰人送给波兰指挥官的榴弹发射器，这个指挥官把玩了一下，以为没有弹药开了一下空枪，结果里面有弹药，就打中天花板了，天花板掉下来就把他自己和一名文职人员砸伤了。

---

俄驻哈萨克斯坦大使公开宣称：“哈萨克斯坦纳粹和民族主义猖獗，俄罗斯将帮助哈萨克斯坦去纳粹化。”

哈萨克知名记者阿尔曼做出回应。概括的说就是别催牛批了，你的京子跟乌萝玩了九个月了，还是这B样，敢对我们发起对线让你们一个都回不去。

---

当欧美限价60美元后，像印度之类阴诈至极的“伙伴”，又趁机再度索要折扣，把油价打压到仅仅维持俄罗斯石油公司成本运作平衡的40多美元。

---

在俄罗斯一直有军事游玩项目，其内容是游客花钱使用军事装备，比如坦克步战车等，碾压一下报废车，有钱的人多花个一千来元人民币还能干她娘的一炮。不过之前提供的军事装备都是T-55，PT-76等上个世纪五六十年代的，现在都快报废的装备。但是刚刚随手一划，他奶奶地竟然更新出T-80，BMP-2这种前线都缺的装备。

---

2014年7月，瑞典新纳粹分子、前本土防卫军狙击手米尔凯·斯基尔特加入“亚速”营。但他在2015年8月公开宣布：他在乌克兰的经历改变了自己，自己将不再相信纳粹主义，因为他在乌克兰的所见所闻让自己认识到——自己过去的纳粹思想是错误而愚蠢的。

https://mp.weixin.qq.com/s/wp8CBSc2cBJBddSws2ktzw

乌克兰正在进一步走向分裂！这次不在东部，而是西部。

---

126独立旅是去年春天在赫尔松方向损失最严重的俄军部队，虽然俄罗斯官方没有公布任何具体损失的消息，但去年3月份该旅被授予“近卫”称号——这是克里姆林宫安抚伤亡惨重部队的标准流程。

---

4月7日乌克兰方面宣布在顿涅茨克干掉一架俄军最新型的ORLAN-10无人机。

好奇的乌克兰军人拆开战利品后发现，号称单价10万美元的ORLAN无人机，燃料供应系统居然使用了一个矿泉水瓶，负责照片和视频的佳能相机，被用魔术贴和胶水与无人机连接。

他们在电话中与上司汇报这一拆解结果时，他们的上校坚定地说： “这不可能，你在和我们开玩笑”，直到上校收到士兵们拍摄的照片和视频，他们的上司才认可了这一事实。

乌克兰军方把ORLAN无人机的拆解视频发给与他们合作的北约军事专家，这些专家们也惊呆了。

---

芬兰模式：相当于直接承认割让领土。

https://zhuanlan.zhihu.com/p/619510199

刚刚，芬兰进去了！
