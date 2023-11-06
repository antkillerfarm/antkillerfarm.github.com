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

# 中东+

https://mp.weixin.qq.com/s/3Frx_ZmmmEC_aj4tLUPleg

苏莱曼尼被斩首后，这篇文章把两伊根本问题说清楚了

https://mp.weixin.qq.com/s/Wt56L0Tdj8H1s4zVh43cMw

决战亮马桥！美国、伊朗驻华大使馆于微博隔空斗法

https://mp.weixin.qq.com/s/sCiWfFweGzgjPTVGhl3hfw

美国向伊朗的求和，是以斗争求和平的伟大实践！

https://zhuanlan.zhihu.com/p/373781337

巴勒斯坦，其实已经内部分裂了

https://mp.weixin.qq.com/s/cHPdmOJvvQde6DGlWBcHvw

伊拉克化学博士身陷IS占领区，导师为督促其完成论文，派雇佣兵将其救出

https://www.zhihu.com/question/303195046

如何评价阿夫沙尔王朝开创者，猛男纳迪尔沙？

https://www.zhihu.com/answer/790409921

阿富汗历史

https://zhuanlan.zhihu.com/p/370329132

神经病气质的塔利班为何能统一阿富汗？它又是如何极速滑落的？

https://view.inews.qq.com/wxn/20210725A084BY00

“两个塔利班”的前世与今生（巴基斯坦塔利班和阿富汗塔利班）

https://zhuanlan.zhihu.com/p/395139812

什么是“杀死印度人山脉”？（Hindu kush）

https://mp.weixin.qq.com/s/dw8GRXm9U_YEaiWWQlVO5A

阿富汗之变：被抛弃和被遗忘的命运

https://mp.weixin.qq.com/s/fNGUgM7mM9elHZ4mQqAuhQ

同样是打美国，为啥塔利班能卷土重来，萨达姆政权却被彻底消灭？

https://mp.weixin.qq.com/s/3y7c2c6a2CsilQIDpcpTmA

阿富汗战事以总统跑路告终：美军20年来兵力与军费如何？

https://mp.weixin.qq.com/s/i95tMMhW_VoGtmD_RPehlg

趣谈美国在阿富汗的天价军费：最高峰时超过当时中国全部军费

https://zhuanlan.zhihu.com/p/409195354

塔利班秒变人民公仆？市长亲自扫大街，银行行长端着冲锋枪去上班

https://zhuanlan.zhihu.com/p/438002125

全球唯一器官交易合法国家：穷人拿命换钱，富人花钱续命（伊朗器官交易合法化）

https://mp.weixin.qq.com/s/giuTx7hS3HD4GdiD_dyRNg

黎巴嫩央行破产：一个悲情国家的不可承受之重

https://xw.qq.com/cmsid/20200710A0JO9X00

阿勒颇监狱保卫战，堪称现代版斯大林格勒，叙利亚五百勇士死守三年终获胜利

https://baijiahao.baidu.com/s?id=1711492174611034081

19岁沙特公主因私奔遭极刑，男友被斩首，差点造成英国沙特断交

https://www.zhihu.com/question/455642975

一战英国都没想过土耳其战略地位吗，为什么要扣土耳其战舰？

https://new.qq.com/omn/20210917/20210917A012II00.html

美军噩梦幽灵狙击手“朱巴”，曾经狙杀634名美军，他到底是谁？

https://mp.weixin.qq.com/s/nfPaPOpIjPrgTEGytCPf0w

埃及前总统穆巴拉克去世：毁誉参半的一生

http://history.people.com.cn/GB/198306/14325368.html

奴隶？工人？金字塔的建造者竟是一群自由民

https://www.zhihu.com/question/443184252

叙利亚在军事作战中为什么一败涂地？

https://zhuanlan.zhihu.com/p/588730371

中东传奇女王卡塔尔太后——谢赫.莫扎

https://zhuanlan.zhihu.com/p/598834668

为什么生为奥斯曼帝国的王子不一定是一件好事？

https://www.zhihu.com/question/603396425

伊朗边防军与阿富汗塔利班边防士兵在边境地区发生武装冲突，目前双方局势情况如何？

https://www.sohu.com/a/648356428_121656712

美军从阿富汗逃跑后，到底留下了多少武器装备？

https://zhuanlan.zhihu.com/p/636102801

叙利亚叛军的山寨肯德基餐厅

https://zhuanlan.zhihu.com/p/658669798

伊朗在东北修长城，是防着谁？（戈尔甘长城）

# 商业风云+

可口可乐：雪碧、芬达、美汁源、维他命水、冰露

百事可乐：美年达、七喜、激浪、果缤纷、佳得乐、纯果乐、乐事薯片、肯德基、必胜客、小肥羊、黄记煌

宝洁：海飞丝、潘婷、飘柔、沙宣、伊卡璐、威娜、舒肤佳、SK-II、玉兰油、护舒宝、汰渍、佳洁士、吉列

强生：强生婴儿、露得清、可伶可俐、娇爽、邦迪、达克宁

联合利华：夏士莲、力士、清扬、多芬、凡士林、和路雪、立顿茶、金纺、奥妙、中华牙膏

亿滋国际：奥利奥、太平梳打、闲趣饼干、王子饼干、怡口莲、趣多多、优冠、闲趣、太平、乐之、荷氏、炫迈、妙卡、果珍

玛氏：德芙、士力架、脆香米、瑞士糖、真知棒、彩虹糖、箭牌、益达

雀巢：美宝莲、薇姿、科颜氏、植村秀、兰蔻、理肤泉、阿玛尼、脆脆鲨、太太乐、美极、豪吉、徐福记、银鹭

通用汽车：别克、雪佛兰、凯迪拉克、宝骏、沃克斯

上海家化：六神、美加净、佰草集

隆力奇集团：郁美净、宝宝金水、青蛙王子

https://mp.weixin.qq.com/s/VvBYE7yLJLxYmQ7zcriNpw

你身边的这些中国品牌，其实全是美国的

---

中国线下商业的魔幻之处在于，任何一点产品、供应链、品牌、运营、物流等环节的创新所带来的超额利润，都会被“房东”这个角色以“涨房租”的方式吃掉，绝无机会降价让利给消费者。

预制菜遭受的非议只不过是对这一结论的又一次证明而已。

---

2011年1月，上海交通大学的几个学生在闵行校区D32宿舍组了个团队，自称米哈游工作室。

米哈游以自身15%股份的代价，从斯凯网络CEO宋涛手中拿到了100万元的天使投资，这也成了米哈游唯一的融资记录。

https://mp.weixin.qq.com/s/rvPh6fCjCWIZ7Aw3XBwduw

因经营风险过大，原神公司被拒绝上市

---

比亚迪最初叫亚迪电子，1994年创办于深圳市亚迪村，当时为便于工商注册通过，便起名亚迪电子。后因参展推广时，厂家名字均按英文首字母排列，为了推广靠前，便在名字前加了一个比(B)，即比亚迪电子。当时AYD被注册了，叫欧亚达，是1992年注册的。

王传福，1987年毕业于中南工业大学（现中南大学）冶金物理化学专业，同年进入北京有色金属研究总院攻读硕士，1990年毕业后留院工作。

https://baijiahao.baidu.com/s?id=1703969037681671178

比亚迪名字的由来
