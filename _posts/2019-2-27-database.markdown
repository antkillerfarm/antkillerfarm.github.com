---
layout: post
title:  数据库（一）
category: technology 
---

* toc
{:toc}

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

https://mp.weixin.qq.com/s/WIF9Oyb5LBoKWtcDjaA45w

OLAP数仓入门：基础篇

https://mp.weixin.qq.com/s/O3p1wQYlUfcaIPbGxs4seQ

OLAP数仓入门：进阶篇

https://mp.weixin.qq.com/s/YSYkvv_74WVDceoJ9XWOjw

关于OLAP数仓，从百万到百亿级数据量实时分析

https://mp.weixin.qq.com/s/IM57guTCRvx7Pz5wDDgOTA

优酷大数据OLAP技术选型

## SQL

![](/images/img3/sql_join.png)

https://www.cnblogs.com/liuqifeng/p/9148831.html

数据库操作命令大全

https://mp.weixin.qq.com/s/O9wBAG1L5SrNGI8JteFirQ

一文搞懂SQL：基础知识和业务实践总结

https://mp.weixin.qq.com/s/WQh-Ro03e9F1Yrhk0tTZ0A

图解SQL的JOIN

https://mp.weixin.qq.com/s/NbV76hDZXKzvark4JEoyMA

我是如何用2个Unix命令给SQL提速的

https://mp.weixin.qq.com/s/2hJzdILGgLwcw8mkCY1uLw

当我们输入一条SQL查询语句时，发生了什么？

https://juejin.im/post/6844903790571700231

SQL语法速成手册

https://blog.csdn.net/horses/article/details/104553075

SQL编程思想：一切皆关系

https://mp.weixin.qq.com/s/CvXKbp5Id6_lOWNs6ntWiA

SQL最大竞争对手（QUEL）的简史

https://mp.weixin.qq.com/s/NgHyZVcb8zoN8tETwCWEmw

为什么阿里巴巴禁止使用存储过程？

https://mp.weixin.qq.com/s/qjNGpfeOLkg9PHRqy6Hstw

5大SQL数据清洗方法！

https://mp.weixin.qq.com/s/QpgGJchCjSr1HvFtSUfzxQ

图解SQL，这也太形象了吧！

https://mp.weixin.qq.com/s/uBOFnLKiVG-rsv5vzbDbtQ

一条sql的执行过程详解

https://mp.weixin.qq.com/s/J_ng048H4eHBm_4VBjFiWw

详解一条SQL的执行过程

https://cloud.tencent.com/developer/article/1475146

SQL语句中where条件后写上1=1是什么意思

https://cnblogs.com/SimpleWu/p/9929043.html

SQL语句性能优化

https://mp.weixin.qq.com/s/UU6_YK_cqoup-bVv8Q_sEw

Insert into select语句引发的生产事故

https://mp.weixin.qq.com/s/Whx50KNUuXORO05bi7PECw

一文详解SQL关联子查询

https://mp.weixin.qq.com/s/VJT4CVor8vE6pl5lzRHsLg

SQL窗口函数是什么？

https://juejin.cn/post/6844903998974099470

8种经常被忽视的SQL错误用法，你有没有踩过坑？

https://mp.weixin.qq.com/s/2da5ZgxPC1Z5VGb766FNCA

程序员必须清楚的10个高级SQL概念！

https://mp.weixin.qq.com/s/q86lPDWMM4NeIkQ4ilMwGg

比开源快30倍的自研SQL Parser设计与实践

https://mp.weixin.qq.com/s/1lBGVj2i5fwo9fMj6wJ47A

如何科学破解慢SQL?

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

## Apache Druid

Apache Druid也是MOLAP。

参考：

https://yuzhouwan.com/posts/5845/

Apache Druid：一款高效的OLAP引擎

https://mp.weixin.qq.com/s/XxSTsHluORTDwtiopAxQ0w

深入分析Druid存储结构

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

https://zhuanlan.zhihu.com/p/157314533

直接干掉RESTful：GraphQL是真的香！

https://mp.weixin.qq.com/s/KalliFefy7fxBYd2xQaAqg

GraphQL最佳入门实战

https://mp.weixin.qq.com/s/VxeRwsYw0qL35BlPsDGbtg

5款好用且有代表性的GraphQL工具

https://mp.weixin.qq.com/s/ea6w5srIG0u7vyYEcuxaVQ

GraphQL最突出的架构优势是什么？

## 分布式算法

Paxos、Raft、Gossip、Nuorum NWR、PBFT、Anti-Entropy、POW、ZAB等等。

https://mp.weixin.qq.com/s/KKrxuVCrjlXXWMPTXQ-fvA

分布式之系统底层原理

https://zhuanlan.zhihu.com/p/130974371

分布式一致性协议概述

https://mp.weixin.qq.com/s/NqLpT47ZWD6oJzfo_tWceA

Paxos（分布式一致性算法）

https://mp.weixin.qq.com/s/OXE7prU9cMuZG8kFydPm9A

Paxos和Raft的前世今生

https://mp.weixin.qq.com/s/ZE3sXRWoZYgbSIPTYIHWoA

浅谈分布式一致性：Raft与SOFAJRaft

https://mp.weixin.qq.com/s/Fj4zERz9PEuNumd_SI0bEA

浅谈CAP和Paxos共识算法

https://mp.weixin.qq.com/s/KTZQjC1JokmwmCkOH8APdg

一文了解分布式一致性算法EPaxos

https://mp.weixin.qq.com/s/42cStPudXRHjs61gjSIgWw

一文读懂区块链与传统数据库之共识机制

https://blog.csdn.net/blockchain_lemon/article/details/84801413

一文读懂11个主流共识算法, 彻底搞懂PoS,PoW,dPoW,PBFT,dBFT这些究竟是什么鬼...

https://zhuanlan.zhihu.com/p/113612578

Chandy-Lamport算法核心解读

https://mp.weixin.qq.com/s/q2XL-_XoRLL1xLzGmzlo6A

分布式快照算法: Chandy-Lamport

https://zhuanlan.zhihu.com/p/130332285

分布式一致性算法-Paxos、Raft、ZAB、Gossip

https://mp.weixin.qq.com/s/ow-6rQYGlvX316h63aCGVg

百度正式开源其Raft一致性算法实现braft

https://zhuanlan.zhihu.com/p/273270802

包教包会的用一张白纸推导出RAFT算法

https://zhuanlan.zhihu.com/p/25664121

比较raft ，basic paxos以及multi-paxos

https://mp.weixin.qq.com/s/JVreQzE8B6nAgPmDHM5aXQ

从Paxos到Multi-Paxos

## Cache

**缓存雪崩**指大量缓存同一时间段集体失效，或者缓存整体不能提供服务，导致大量的请求全部到达数据库，对数据CPU和内存造成巨大压力，严重的会造成数据库宕机。也被称为缓存踩踏（Cache Stampede）。

使用随机过期时间。为每一个key都合理的设计一个过期时间，这样可以避免大量的key在同一时刻集体失效。

https://mp.weixin.qq.com/s/RME5b3plT97nYfUaCl9ePw

关于缓存和数据库强一致的可行方案

https://mp.weixin.qq.com/s/gQAA2-YuvTHrL2IP8Bco6w

缓存与数据库的数据一致性方案介绍

https://mp.weixin.qq.com/s/Gm7S0a5VN9L57wlry4-t6w

缓存关注点——先写DB还是“缓存”？
