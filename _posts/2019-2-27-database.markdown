---
layout: post
title:  数据库
category: technology 
---

# 数据库

## SQL与数据库

2010.3

这几天看到了这篇文章：

http://www.cnbeta.com/articles/104987.htm

Twitter用户暴增20倍 计划弃用MySQL

之前许多课本中的概念顿时浮现在眼前。曾几何时，SQL成了数据库的同义词，以至于离开SQL就没法操作数据库了。但其实SQL所代表的关系型数据库，只是数据库的一类而已。除此之外还有很多其他类型的数据库。

Oracle的广告词，给当时尚在学校的我产生了这样的错觉：“只有数据库，才是处理大量数据的最好方法。”直到后来随着自己技术的进步，才知道这样的想法是如何的荒谬。

一个身边的例子，有一次我们需要处理一批数据，在使用数据库的情况下，需要2天才能处理完，峰值时内存的占用接近4G。但这还不是最糟的，关键的问题是以后数据的规模会越来越大，一旦内存占用超过4G，旧的32位硬件软件就不够用了。（当时我们对硬件了解的不多，不知道Intel X86有PAE模式，以为4G就是32位PC的极限了，其实不然。）

于是，有位同事说，这批数据虽然量大，但规则并不复杂，干脆用最原始的写文件来做吧。结果2天的处理时间缩短为1天，内存占用减少为300M。其实这也没有什么神奇的，一般的关系型数据库通常由三部分组成：SQL解释器、事务引擎和存储查询引擎。如果能够根据具体情况，去掉前两部分，并对第三部分进行优化，完全可能比Oracle做的更好。毕竟在软件这个领域并不存在超现实的东西，Oracle再牛也是跑在相应的硬件、软件之上的，少不了要和CPU、内存、硬盘打交道。

结论：不要太过依赖数据库，有的时候没有数据库，更快，更简单。

## blog

https://www.cnblogs.com/ahu-lichang/

一个数据库+大数据的blog

## 数据库分类

- 键值数据库
- 文档数据库
- 关系型数据库
- 图数据库
- 列族数据库
- 时序数据库
- 搜索引擎

参考：

https://mp.weixin.qq.com/s/SsrDWrcmCHXCb3lZFzJC7w

数据库有哪些分类？应该怎样选择？

## Sqlite

《Inside Sqlite》是最好的参考书，目前已经有人把它翻译成中文，可以在CSDN上找到。

《SQLite Optimization FAQ》另一篇很好的文章。

http://web.utk.edu/~jplyon/sqlite/SQLite_optimization_FAQ.html

## OLTP与OLAP

数据处理大致可以分成两大类：联机事务处理OLTP（on-line transaction processing）、联机分析处理OLAP（On-Line Analytical Processing）。OLTP是传统的关系型数据库的主要应用，主要是基本的、日常的事务处理，例如银行交易。OLAP是数据仓库系统的主要应用，支持复杂的分析操作，侧重决策支持，并且提供直观易懂的查询结果。

OLAP有多种实现方法，根据存储数据的方式不同可以分为ROLAP、MOLAP、HOLAP。

ROLAP表示基于关系数据库的OLAP实现（Relational OLAP）。这种方式查询效率最低。

MOLAP表示基于多维数据组织的OLAP实现（Multidimensional OLAP）。多维数据在存储中将形成"立方块（Cube）"的结构,在MOLAP中对"立方块"的"旋转"、"切块"、"切片"是产生多维数据报表的主要技术。特点是将细节数据和聚合后的数据均保存在cube中，所以以空间换效率，查询时效率高，但生成cube时需要大量的时间和空间。

HOLAP表示基于混合数据组织的OLAP实现（Hybrid OLAP）。如低层是关系型的，高层是多维矩阵型的。这种方式具有更好的灵活性。

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

https://blog.csdn.net/liu251/article/details/40785291

ROLAP、MOLAP和HOLAP联机分析处理区别

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

## Apache Doris

Apache Kylin属于MOLAP，而Apache Doris属于ROLAP。

参考：

https://mp.weixin.qq.com/s/wzVc-FngOBbqbMzJjRqajw

Apache Doris在美团外卖数仓中的应用实践

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

十年前，谷歌发表了 “BigTable” 的论文，论文中很多很酷的方面，其中之一就是它所使用的文件组织方式，这个方法更一般的名字叫Log Structured-Merge Tree。其核心思想就是放弃部分读能力，换取写入的最大化能力。

http://www.open-open.com/lib/view/open1424916275249.html

Log Structured Merge Trees(LSM)原理

https://mp.weixin.qq.com/s/CmYg22NObamkNOqGjKg0-w

解读现代存储系统背后的经典算法

https://mp.weixin.qq.com/s/9L_HOCfRwC_QxjzJyVjKXA

字节跳动在RocksDB存储引擎上的改进实践

## 分布式算法

Paxos、Raft、Gossip、Nuorum NWR、PBFT、Anti-Entropy、POW、ZAB等等

https://zhuanlan.zhihu.com/p/130974371

分布式一致性协议概述

https://mp.weixin.qq.com/s/NqLpT47ZWD6oJzfo_tWceA

Paxos（分布式一致性算法）

https://mp.weixin.qq.com/s/OXE7prU9cMuZG8kFydPm9A

Paxos和Raft的前世今生

https://mp.weixin.qq.com/s/Fj4zERz9PEuNumd_SI0bEA

浅谈CAP和Paxos共识算法

https://zhuanlan.zhihu.com/p/113612578

Chandy-Lamport算法核心解读

https://mp.weixin.qq.com/s/q2XL-_XoRLL1xLzGmzlo6A

分布式快照算法: Chandy-Lamport

https://zhuanlan.zhihu.com/p/130332285

分布式一致性算法-Paxos、Raft、ZAB、Gossip

## 参考

https://mp.weixin.qq.com/s/ActS6PxbtZGqPb0jOn0iFg

不懂数据库索引的底层原理？那是因为你心里没点b树

http://www.ruanyifeng.com/blog/2014/07/database_implementation.html

数据库的最简单实现

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

https://www.jianshu.com/p/0355d9e5ba0e

数据库三大范式

https://www.cnblogs.com/ahu-lichang/p/10899747.html

数据库三大范式（1NF,2NF,3NF）及ER图

https://mp.weixin.qq.com/s/m_JMXU6iMS4SckzWZYtIUA

腾讯分布式数据库TDSQL金融级能力的架构原理解读

https://mp.weixin.qq.com/s/jdPE9WClBuimIHVxJnwwUw

字节跳动自研强一致在线KV&表格存储实践-上篇

https://mp.weixin.qq.com/s/DvUBnWBqb0XGnicKUb-iqg

字节跳动自研强一致在线KV&表格存储实践-下篇

https://mp.weixin.qq.com/s/oV5F_K2mmE_kK77uEZSjLg

字节跳动分布式表格存储系统的演进
