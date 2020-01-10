---
layout: post
title:  数据库, 分布式ID生成器
category: technology 
---

# 数据库

## Sqlite

《Inside Sqlite》是最好的参考书，目前已经有人把它翻译成中文，可以在CSDN上找到。

《SQLite Optimization FAQ》另一篇很好的文章。

http://web.utk.edu/~jplyon/sqlite/SQLite_optimization_FAQ.html

## OLTP与OLAP

数据处理大致可以分成两大类：联机事务处理OLTP（on-line transaction processing）、联机分析处理OLAP（On-Line Analytical Processing）。OLTP是传统的关系型数据库的主要应用，主要是基本的、日常的事务处理，例如银行交易。OLAP是数据仓库系统的主要应用，支持复杂的分析操作，侧重决策支持，并且提供直观易懂的查询结果。

参考：

https://blog.csdn.net/zhangzheng0413/article/details/8271322

OLAP、OLTP的介绍和比较

https://mp.weixin.qq.com/s/80iF3secK_K7uSuuYfUDEw

分布式数据库TiDB是如何结合OLTP和OLAP的？

https://mp.weixin.qq.com/s/-OfYLqVPVioRP1kkIsCFsA

小米大数据：借助Apache Kylin打造高效、易用的一站式OLAP解决方案

https://mp.weixin.qq.com/s/eVJmWwUdYqAIs1CF6UwgaQ

Hulu在OLAP场景下数据缓存技术实战

https://mp.weixin.qq.com/s/2VutJPwMLUW51o2P0P0QLw

Sophon：Hulu智能OLAP缓存层技术实践

## Apache Kylin

Apache Kylin是一个开源的分布式分析引擎，最初由eBay开发贡献至开源社区。它提供Hadoop之上的SQL查询接口及多维分析（OLAP）能力以支持大规模数据，能够处理TB乃至PB级别的分析任务，能够在亚秒级查询巨大的Hive表，并支持高并发。

官网：

http://kylin.apache.org/

参考：

http://d.xiumi.us/board/v5/39rut/82854193

老司机带路系列：多维交叉分析引擎-Kylin

https://mp.weixin.qq.com/s/e0nkJK927ANPCsoFzlBFsg

OLAP系统解析：Apache Kylin和Baidu Palo哪家强？

https://blog.csdn.net/zhangzheng0413/article/details/8271322

OLAP、OLTP的介绍和比较

## GraphQL

GraphQL是在Facebook内部应用多年的一套数据查询语言和runtime。

官网：

http://graphql.org/

中文官网：

http://graphql.cn/

代码：

https://github.com/facebook/graphql

参考：

https://zhuanlan.zhihu.com/p/20638731

GraphQL and Relay浅析

https://mp.weixin.qq.com/s/4AnLu0m2IILGwyPyuK37Fg

基本操作：Go创建GraphQL API

## LSM (Log Structured Merge)

十年前，谷歌发表了 “BigTable” 的论文，论文中很多很酷的方面，其中之一就是它所使用的文件组织方式，这个方法更一般的名字叫Log Structured-Merge Tree。

http://www.open-open.com/lib/view/open1424916275249.html

Log Structured Merge Trees(LSM)原理

https://mp.weixin.qq.com/s/CmYg22NObamkNOqGjKg0-w

解读现代存储系统背后的经典算法

## 参考

https://mp.weixin.qq.com/s/ActS6PxbtZGqPb0jOn0iFg

不懂数据库索引的底层原理？那是因为你心里没点b树

https://blog.csdn.net/zhengzhb/article/details/8590390

SQL查找删除重复行

https://mp.weixin.qq.com/s/09BlPee0-kP-At2aDyDbMw

中国数据库40年历史：隐秘的江湖与恩怨

https://mp.weixin.qq.com/s/4lZ7My6cs4-VJQ1qGhQxZg

AliSQL X-Cluste：基于X-Paxos的高性能强一致MySQL数据库

https://mp.weixin.qq.com/s/tPzBlQGxGq1WEnXz5ggpxg

sysbench在美团点评中的应用

https://mp.weixin.qq.com/s/lJfIkLQaZnN4e9DxX163SA

一款可能解放DBA的分布式数据库RadonDB的体验之旅

http://mp.weixin.qq.com/s/idz6b2rls97W4Iw6J-ubng

美团点评SQL优化工具SQLAdvisor开源

https://mp.weixin.qq.com/s/jCFjhkwQpj1_P-seQurPqQ

SQL解析在美团点评中的应用

https://mp.weixin.qq.com/s/RME5b3plT97nYfUaCl9ePw

关于缓存和数据库强一致的可行方案

https://mp.weixin.qq.com/s/Al0yvkv0FUPjEBtcxS6Fmg

传统数据仓库和云数据仓库的区别

https://mp.weixin.qq.com/s/GXGSDxukbIAM5W-YSX0pDg

美团点评数据库高可用架构的演进与设想

https://mp.weixin.qq.com/s/crluKkEdvfZlHyF_gQm1ZA

漫谈推荐系统及数据库技术

https://mp.weixin.qq.com/s/CwUW-Ntb4qphrqha24P-Og

漫谈推荐系统及数据库技术（二）——分布式数据库技术

https://mp.weixin.qq.com/s/1pXMCcO6NR1SMBzsyES-cw

分布式数据库又支持关系数据模型了？

https://mp.weixin.qq.com/s/yQSMSLBYg4iauu8yeUfvjw

深度解读！时序数据库HiTSDB：分布式流式聚合引擎

https://mp.weixin.qq.com/s/WQh-Ro03e9F1Yrhk0tTZ0A

图解SQL的JOIN

https://mp.weixin.qq.com/s/NbV76hDZXKzvark4JEoyMA

我是如何用2个Unix命令给SQL提速的

https://mp.weixin.qq.com/s/gQAA2-YuvTHrL2IP8Bco6w

缓存与数据库的数据一致性方案介绍

https://mp.weixin.qq.com/s/m76PFxbcY6_-XyeU7uu4Jg

数据库的最简单实现

https://mp.weixin.qq.com/s/rZuGW9Fe0TA3dNEY7dh_Jg

TimescaleDB比拼InfluxDB：如何选择合适的时序数据库？

https://mp.weixin.qq.com/s/pZnAcjFlBM2I4Hyctd6MHw

图数据库真的比关系数据库更先进吗？

https://mp.weixin.qq.com/s/O3A5gVewRQ11Z8RdPcs-9w

一文看懂Pinterest如何构建时间序列数据库系统Goku

https://mp.weixin.qq.com/s/2hJzdILGgLwcw8mkCY1uLw

当我们输入一条SQL查询语句时，发生了什么？

https://mp.weixin.qq.com/s/5Qcbz6dT20Sa_OvRfbNXNw

如何给新来的师妹解释什么是数据库的脏读、不可重复读和幻读

https://mp.weixin.qq.com/s/1zarqgOh9-3chlBsB4TsuA

物联网时代数据数据库如何选型？

https://mp.weixin.qq.com/s/O94Q1Dxe8TnbCMv9d_hlOg

Uber推出数据湖集成神器DBEvents，支持MySQL、Cassandra等

https://mp.weixin.qq.com/s/AcuFiHgRJg2OcNGtfjRxYA

我们对比了5款数据库，告诉你NewSQL的独到之处

https://mp.weixin.qq.com/s/Gm7S0a5VN9L57wlry4-t6w

缓存关注点——先写DB还是“缓存”？

https://mp.weixin.qq.com/s/q-R7kv4696LooHy3Hn-H1A

分布式系统关注点——缓存背后的“毁灭种子”

https://mp.weixin.qq.com/s/DaspXFLPASYE7N0WHllcYQ

Cassandra的过去、现在、未来

https://mp.weixin.qq.com/s/cLIrRmcS5sbiDVl0cwDlIw

Cassandra在时空数据上的探索

https://mp.weixin.qq.com/s/ufficZ7cCvRFdEpaAfm8Fg

面试官问：讲讲高并发下的接口幂等性怎么实现？

# 分布式ID生成器

http://mp.weixin.qq.com/s/Bk5k6vRG4Rq4iCtmtYDEGQ

Leaf——美团点评分布式ID生成系统

https://mp.weixin.qq.com/s/6J7n3udEyQvUHRHwvALNYw

Leaf：美团分布式ID生成服务开源

https://mp.weixin.qq.com/s/_z0-90xbsCd4Pdi6UlEvnA

分布式ID生成器

https://mp.weixin.qq.com/s/Yk2ZlGCEINGrsTfHI6zpsw

分布式id生成器

# 世说新语

## 2019.12（续）

当年，福特公司请人检修电机，斯泰因梅茨在电机外壳画了一条线，让工作人员在此处打开电机迅速排除了故障。结账时，斯坦门茨要1万美元，还开了个清单：画一条线，1美元；知道在哪儿画线，9999美元。

Charles Proteus Steinmetz，1865~1923，德国数学家和电气工程师，后移民美国。一生有200+项专利，在交流电领域有重大贡献。因祖父和父亲遗传，他是先天性驼背。IEEE Charles Proteus Steinmetz Award就是以他名字命名的。

----

元音大漂移（Great Vowel Shift）是英语发展史上的一次主要的语音转变，开始于14世纪，大体完成于15世纪中期，由大都会和港口城市向乡村的扩散一直持续到16世纪。转变主要体现在英语长元音的变化上。

这种迁移是整体性的改变，就是某种拼写对应的所有单词全部改变了，这比一个个外来语的借此对读音造成的影响要巨大。

https://www.zhihu.com/question/21298527

为什么英语不直接按读音书写？

----

https://zhuanlan.zhihu.com/p/33956621

汉语拼音+注音符号+威妥玛拼音+国语罗马字+注音符号第二式+耶鲁拼音+通用拼音

----

都说舞蹈能长寿，跳舞的陶金36岁走了；

都说早锻炼能长寿，天天在中央电视台教大家早操的马华36岁走了；

都说唱歌能长寿，歌手姚贝娜、叶丹、臧天朔、布仁巴雅尔分别于30多、40多、50多岁走了；

都说笑能长寿，一辈子幽默、说笑话的侯耀文、笑林都50多岁走了，李咏才50岁也走了；

都说勤动脑，多动手指能预防老年痴呆，著名指挥家小泽征尔动脑动眼动表情，动手动脚动全身也痴呆了……

2019年11月11日，称“自然疗法大师”、鼓吹“断食排毒养生”的林海峰，在云南丽江意外身亡，终年51岁。

林海峰朋友向记者表示，出事前几天，林海峰在微信朋友圈发了一条动态，称自己吃了一包过期的红枣，“全身僵硬”，但他没有去医院，通过呕吐的方式，将食物吐出来后，身体恢复了正常。

随后，他删除了这条朋友圈动态。几天后，林海峰和一些朋友去丽江游玩，当日吃过晚饭后，林海峰突发意外倒地。被送到医院后，已经没有生命迹象，大家怀疑是“食物中毒”酿成悲剧。

----

1935-1940年，因为第二次世界大战，在军事上产生了很多应用，如弹道导弹轨迹计算和模拟，如密码破解。人类现有的计算能力和速度无法满足应用的需求，出现一个新领域：Automatic Computer，自动计算机领域。

当时在这个新领域里面几乎没有任何论文，没有任何成型的研究成果。

下面是两个研究人员的研究：

1. 提出问题：一个高速自动计算的机器，应该是具有什么样的结构的系统？

子问题：

a:每个结构应该有什么功能？

b.每个结构应该和其他结构有什么联系？

c.计算机应该具有什么样的基本计算功能？

d.计算机应该如何实现基本计算功能？

提出这个大问题和一系列子问题的人叫冯*诺依曼。

2. 提出问题：自动计算机的运算和人脑的运算有什么异同？

子问题：

a. 什么是人类智能？

b.人类智能如何分类？如何测定？

c. 如何测定机器“智能”和人类智能的接近程度？

提出这个问题的人叫图灵。

----

Ola和Makinde谁是第一个FB黑人工程师不清楚，但是两个人都在硅谷有着极大的影响。Makinde先是加入Pinterest成为它的早期员工，为Pinterest爆炸性的增长立下汗马功劳，现在自己创办了/dev/color帮助其他有色人种进入到IT行业，Ola在FB近十年，留下了代码提交数量多年无人能破的纪录，现在退休正在享受甜蜜的私人生活。

----

https://www.zhihu.com/question/55530692/answer/790409921

阿富汗历史
