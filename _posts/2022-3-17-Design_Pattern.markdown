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

# 新冠疫苗+

在美国，日增20万，也能准确命中那大约一半没打疫苗的人。充分体现了欧美产品宣传的高精度。

在以色列，仅有四成人没打疫苗的情况下，疫苗的防感染有效率先从90+降到64再降到39直至现在近乎无统计学差别。八月以来，以色列新增的400多例重症患者有240多例打完两针疫苗，这意味着：或许防重症可能也没有统计差别了。充分体现了欧美产品到了中东就准头全无，胡乱杀伤的传统。

---

https://mp.weixin.qq.com/s/_XODQnloTRlmlCDWez5_zg

全球首支获批新冠疫苗竟然临床试验都没做完！战斗民族太疯狂，Nature和Science齐质疑

https://mp.weixin.qq.com/s/FoBmj4jCAaqfclS3F8WraQ

全球群体免疫，需要多少新冠疫苗？

https://mp.weixin.qq.com/s/7_L7pvZ2KyQxKRteWKq4QQ

聊聊中美两国的新冠疫苗-安全性和有效性

https://www.zhihu.com/question/441919684

在英国试验有效率达到89%的Novavax新冠疫苗，与其他公司的疫苗相比有何不同？

https://mp.weixin.qq.com/s/8QPLyP9EhX8QuX6ErN5Yhw

疫苗科普

https://mp.weixin.qq.com/s/sKwYTW-HZzGY-Qs9_s_5nw

快如闪电！新冠疫苗的研发速度是如何实现的？

https://mp.weixin.qq.com/s/COjWJWU5d8hQ4afFN5mZ3g

一亿剂疫苗打出去了，何时能见效？

https://mp.weixin.qq.com/s/8L3euDhDzu9NUb15sUCpcw

疫苗接种6个月的真实世界数据

https://view.inews.qq.com/wxn/20210105A037TJ00

30多个国家和地区发现变异毒株，疫苗效果会受影响吗？

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

# 海军

不要说两艘巡洋舰打一艘战列舰，就算是四五艘巡洋舰打一艘战列舰，除非能够冲到鱼雷距离，否则都是被战列舰点名的命。但一万多吨的巡洋舰显然不能学驱逐舰扭来扭去，因此“除非能够冲到鱼雷距离”这句话可以直接删掉。

相比之下，轻巡洋舰学派提倡的“轻巡洋舰”或者说现实中的超级驱逐舰，她们靠着高闪避，高雷击，多用途和快速建造成为了二战性价比最高的水面舰艇。比她小的被她欺压，比她大的被她围殴。

如果是一个超级驱逐舰中队遭遇一条落单的战列舰，则战局可能变得非常微妙。因为太平洋战争的实战表明，舰炮未必能在超级驱逐舰进入鱼雷射程前阻止其前进。

CVSSDD三分天下，这是二战对海战的最终结论。至今这个结论仍未过时。

CV：航空母舰

SS：潜艇

DD：驱逐舰

https://www.zhihu.com/question/414077387

二战时期，在理想状况下，两艘落单巡洋舰能不能硬刚一艘落单战列舰？

---

1949年10月，时任四野第十二兵团司令员的萧劲光接毛泽东急电，日夜兼程赶到北京中南海。毛泽东让他去当海军司令员。“我是个‘旱鸭子’，还晕船，哪能当海军司令？”

1950年3月17日，海军司令员萧劲光乘渔船视察刘公岛。

2019.12.17 山东号航空母舰入列。

我国先后获得四艘航空母舰，分别是从澳大利亚购买的墨尔本号，从韩国购买的明斯克号，从乌克兰购买的瓦良格号，从俄罗斯购买的基辅号。

![](/images/img3/Carrier.jpg)

![](/images/img3/Carrier_2.jpg)

如果你有一艘航母，你就破坏了你所在地区的稳定。

如果你有两艘，你就会对全球秩序构成威胁。

如果你有一打，你就是全球维和人员。

CTOL（conventional take-off and landing）

STOVL（Short Take Off Vertical Landing）

052D/055 导弹驱逐舰

075 两栖攻击舰

HMS是His/Her Majesty's Ship的缩写，意为国王/女王陛下的船舰，是某些君主国家对其海军船舰的正式或非正式船舰称号。

作为我国第二款隐身战斗机，也是被称为“海飞丝”的四代隐身舰载机。

https://zhuanlan.zhihu.com/p/196773456

中国的底牌：万一战争爆发，我们能否顶住？

https://www.guancha.cn/ShiYang/2016_10_26_378389.shtml

施洋：“辽宁号”会像“库兹涅佐夫”一样冒黑烟么

https://mp.weixin.qq.com/s/n1ys7Np2vwPiAHjCnHbUSw

现代级福州舰升级，详细数据忆当年：导弹亚音速高炮靠手摇

https://zhuanlan.zhihu.com/p/427871314

珠联璧合：当003型航母加上歼-31，2025年的中国海军究竟有多强？

https://zhuanlan.zhihu.com/p/438944213

海军20艘056全部转隶海警部队，中国周边想“挑事儿”的要当心了！

---

巡洋舰以上：由国务院特别命名（航母）。

巡洋舰：以行政省（区）或直辖市命名（当然国内没有巡洋舰，国内最后一艘巡洋舰是“重庆”号）。

驱逐舰：以“大、中城市”命名。

护卫舰：以“中、小城市”命名。

综合补给舰：以“湖泊”命名。

弹道导弹核潜艇/攻击型核潜艇：以“长征”加序号命名。

常规鱼雷潜艇：以“长城”加序号命名。

扫雷舰/猎潜艇：以“州”或“县”命名。

船坞登陆舰、坦克登陆舰：以“山”命名。

步兵登陆舰:以“河”命名辅助舰船以所在海区和性质的名称+序号命名。

技术验证船/训练船以人员名称命名（世昌号国防动员舰）。

---

镇海号水上飞机母舰俘获三江号，也是我国航母或水上飞机母舰至今为止取得的惟一一次战果。

https://www.zhihu.com/answer/1516015740

海军史上有什么开挂一样的史实？

https://mp.weixin.qq.com/s/JQg1idg_mPnkForuuJQSoA

写在辽宁号之前，说一说中国海军早期的航空舰船
