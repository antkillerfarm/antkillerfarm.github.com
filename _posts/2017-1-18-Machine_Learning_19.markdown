---
layout: post
title:  机器学习（十九）——关联规则挖掘（1）
category: ML 
---

* toc
{:toc}

# 关联规则挖掘

## 基本概念

关联规则挖掘（Association rule mining）是机器学习的一个子领域。它最早的案例就是以下的**尿布和啤酒**的故事：

>沃尔玛曾今对数据仓库中一年多的原始交易数据进行了详细的分析，发现与尿布一起被购买最多的商品竟然是啤酒。   
>借助数据仓库和关联规则，发现了这个隐藏在背后的事实：**美国妇女经常会嘱咐丈夫下班后为孩子买尿布，而30%~40%的丈夫在买完尿布之后又要顺便购买自己爱喝的啤酒。**   
>根据这个发现，沃尔玛调整了货架的位置，把尿布和啤酒放在一起销售，大大增加了销量。

这里借用一个引例来介绍关联规则挖掘的基本概念。

| 交易号TID | 顾客购买的商品 | 交易号TID | 顾客购买的商品 |
|:--:|:--|:--:|:--|
| T1 | bread, cream, milk, tea | T6 | bread, tea |
| T2 | bread, cream, milk | T7 | beer, milk, tea |
| T3 | cake, milk | T8 | bread, tea |
| T4 | milk, tea | T9 | bread, cream, milk, tea |
| T5 | bread, cake, milk | T10 | bread, milk, tea |

**定义一**：设$$I=\{i_1,i_2,\dots,i_m\}$$，是m个不同的项目的集合，每个$$i_k$$称为一个**项目**。项目的集合I称为**项集**。其元素的个数称为项集的长度，长度为k的项集称为k-项集。引例中每个商品就是一个项目，项集为$$I=\{bread, beer, cake,cream, milk, tea\}$$，I的长度为6。

**定义二**：每笔**交易T**是项集I的一个子集。对应每一个交易有一个唯一标识交易号，记作TID。交易全体构成了**交易数据库D**，$$\mid D\mid$$等于D中交易的个数。引例中包含10笔交易，因此$$\mid D\mid=10$$。

**定义三**：对于项集X，设定$$count(X\subseteq T)$$为交易集D中包含X的交易的数量，则项集X的**支持度**为：

$$support(X)=\frac{count(X\subseteq T)}{\mid D\mid }$$

引例中$$X=\{bread, milk\}$$出现在T1，T2，T5，T9和T10中，所以支持度为0.5。

**定义四**：**最小支持度**是项集的最小支持阀值，记为$$SUP_{min}$$，代表了用户关心的关联规则的最低重要性。支持度不小于$$SUP_{min}$$的项集称为频繁集，长度为k的频繁集称为k-频繁集。如果设定$$SUP_{min}$$为0.3，引例中$$\{bread, milk\}$$的支持度是0.5，所以是2-频繁集。

**定义五**：**关联规则**是一个蕴含式：

$$R：X\Rightarrow Y$$

其中$$X\subset I$$，$$Y\subset I$$，并且$$X\cap Y=\varnothing$$。表示项集X在某一交易中出现，则导致Y以某一概率也会出现。用户关心的关联规则，可以用两个标准来衡量：支持度和可信度。

**定义六**：关联规则R的**支持度**是交易集同时包含X和Y的交易数与$$\mid D\mid$$之比。即：

$$support(X\Rightarrow Y)=\frac{count(X\cap Y)}{\mid D\mid }$$

支持度反映了X、Y同时出现的概率。关联规则的支持度等于频繁集的支持度。

**定义七**：对于关联规则R，**可信度**是指包含X和Y的交易数与包含X的交易数之比。即：

$$confidence(X\Rightarrow Y)=\frac{support(X\Rightarrow Y)}{support(X)}$$

可信度反映了如果交易中包含X，则交易包含Y的概率。一般来说，只有支持度和可信度较高的关联规则才是用户感兴趣的。

**定义八**：设定关联规则的最小支持度和最小可信度为$$SUP_{min}$$和$$CONF_{min}$$。规则R的支持度和可信度均不小于$$SUP_{min}$$和$$CONF_{min}$$，则称为**强关联规则**。关联规则挖掘的目的就是找出强关联规则，从而指导商家的决策。

这八个定义包含了关联规则相关的几个重要基本概念，关联规则挖掘主要有两个问题：

1.找出交易数据库中所有大于或等于用户指定的最小支持度的频繁项集。

2.利用频繁项集生成所需要的关联规则，根据用户设定的最小可信度筛选出强关联规则。

其中，步骤1是关联规则挖掘算法的难点，下文介绍的Apriori算法和FP-growth算法，都是解决步骤1问题的算法。

参考：

http://blog.csdn.net/OpenNaive/article/details/7047823

关联规则挖掘（一）：基本概念

# 天文杂谈+

https://mp.weixin.qq.com/s/cllhuj8m3RmG_JE7QrG4Uw

生的伟大，死的光荣——致敬卡西尼号土星探测器

https://mp.weixin.qq.com/s/sg24WvUClEZnLxhJio0BuQ

被磨难环绕的“伽利略号”的一生

https://mp.weixin.qq.com/s/ros-kBOIgTgQF2SaGc99qQ

哈佛名镜的前世今生

https://mp.weixin.qq.com/s/DKMs8sZtE_JzWaxr8o-DWA

你好，冥王星

https://mp.weixin.qq.com/s/sv6McqKzoqJ3snUeDvFfwg

这地儿跟中国一个时区，你却从来没听说过？这个blog介绍了西澳大学和它的天文学成就。

https://mp.weixin.qq.com/s/EZWl8UwhiAkIXWkWE0abUQ

半个世纪的角逐：二战后的国际脉冲星搜寻竞赛

https://mp.weixin.qq.com/s/tmSL5NIyjBpivtbGpbdKqQ

威廉·赫歇尔：我一个德国音乐家，怎么就跑到英国当了天文学家？

https://mp.weixin.qq.com/s/R2Hco-nE-e0S_gmTSImrAA

它曾“超长待机”俯瞰地球，最终在“回家”路上粉身碎骨

https://mp.weixin.qq.com/s/yD3THPdd1rvx3oqBF1tayQ

那个提出“戴森球”的物理学家去世了（Freeman Dyson）

https://mp.weixin.qq.com/s/3dByllGuY9fsvUjoy7fPwg

戴森传奇

https://mp.weixin.qq.com/s/5NyYCE1vt99i2O_r-ThAGA

下一次去哪？金星、木卫一，还是海卫一？

https://mp.weixin.qq.com/s/lNh3jxjvRbpZ3zZYSZuOVg

曾为地球“挺身而出”的磁场

https://mp.weixin.qq.com/s/6acgwIaDYC3VjMmdVSYYKQ

未来的望远镜，能带我们看到宇宙大爆炸的边缘吗？（卡塞格林望远镜）

https://mp.weixin.qq.com/s/UDHAT3vLMS_yo4w0wbcpJQ

十问北斗

https://zhuanlan.zhihu.com/p/29213504

从沙漏和星盘到六分仪和航海钟——恼人的航海定位

https://mp.weixin.qq.com/s/SQPVTNYBU8JIRPYFqsXgtA

神秘力量？上帝使者？彗星的“人设”早就崩塌了

https://mp.weixin.qq.com/s/pAUiirbHyfrEgWUE21x93Q

另一个费曼（Richard Feynman的妹妹Joan Feynman）

https://mp.weixin.qq.com/s/iznVJzjkYA2I5Y-P0NKrdA

美丽天文图片下，有哪些不为人知的“套路”

https://mp.weixin.qq.com/s/hZBYHvYktdJeOrAHTCKTTA

如何肉眼识别夜空中的人造卫星？
