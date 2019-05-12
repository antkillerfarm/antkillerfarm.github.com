---
layout: post
title:  数据库, Mysql, Android研究（二）, Restful
category: technology 
---

# 数据库

## Sqlite

《Inside Sqlite》是最好的参考书，目前已经有人把它翻译成中文，可以在CSDN上找到。

《SQLite Optimization FAQ》另一篇很好的文章。

http://web.utk.edu/~jplyon/sqlite/SQLite_optimization_FAQ.html

关于编译和使用的问题，写的最好的是以下两篇：

http://www.99inf.net/SoftwareDev/VC/39396.htm

http://www.cnblogs.com/giszhang/archive/2008/10/09/1307509.html

VC2005 + SQLite 3.6.3 编译、测试开发手记

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

## 参考

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

# Mysql

## 安装

`sudo apt-get install mysql-server mysql-client mysql-workbench`

其中，mysql-workbench是一个查看mysql的GUI工具。

安装过程中，会提示输入root用户的密码。注意：这里的root是mysql的登录帐号，而不是系统的登录帐号。

·/etc/my.cnf是默认的MySQL配置文件。

## 常用操作

登录方法：

`mysql -h 192.168.4.251 -u root -p`

语句以“;”结尾。

| 名称 | 操作 |
|:--|:--|
| 添加用户 | insert into mysql.user(Host,User,Password) <br/>values("localhost","test",password("1234")); |
| 列出所有数据库 | show database; |
| 切换数据库 | use 数据库名; |
| 列出所有表 | show tables; |
| 显示数据表结构 | describe 表名; |
| 创建自增ID | create table github(id int auto_increment primary key not null,name varchar(256)); |
| 查询头N条记录 | select * from shop_info limit N; |
| 检索记录行 6-15 | select * from table limit 5,10; |
| 删除记录 | delete from shop_info where shop_id="1"; |
| 排序+别名+分组+count | select city_name,count(*) as city_count from shop_info group by city_name <br/>order by city_count desc limit 5; |
| 两列排序+两列相乘 | select shop_id,count(*)*per_pay from shop_info order by per_pay desc,shop_id desc; |
| 每日统计 | select count(shop_id),date(time_stamp) as dates from user_pay <br/>where shop_id='1234' group by dates order by dates asc; |
| 年月日 | select year(ordertime),month(ordertime),day(ordertime) from book; |
| 周数+星期几 | select week(ordertime),weekday(ordertime) from book; |
| 统计表中的记录条数 | select count(*) from user_pay; |
| 统计某一列中不同值的个数 | select count(distinct user_id) from user_pay; |

参考：

http://www.cnblogs.com/wuhou/archive/2008/09/28/1301071.html

Ubuntu安装配置Mysql

http://www.cnblogs.com/wanghetao/p/3806888.html

MySQL添加用户、删除用户与授权

## 执行脚本

mysql命令行下执行：

`source a.sql`

## 导入csv文件

http://www.mysqltutorial.org/import-csv-file-mysql-table/

Import CSV File Into MySQL Table

示例：

{% highlight sql %}
LOAD DATA LOCAL INFILE 'c:/tmp/discounts.csv' 
INTO TABLE discounts 
FIELDS TERMINATED BY ',' 
ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 ROWS;
{% endhighlight %}

上面的语句中，LOCAL必不可少，否则会报如下错误：

`ERROR 1290 (HY000): The MySQL server is running with the --secure-file-priv option so it cannot execute this statement`

## 日志

http://www.cnblogs.com/jevo/p/3281139.html

MySQL日志

## 时间的格式

| 名称 | 格式 |
|:--|:--|
| DATE | YYYY-MM-DD |
| DATETIME | YYYY-MM-DD HH:MM:SS |
| TIMESTAMP | YYYY-MM-DD HH:MM:SS |
| YEAR | YYYY或YY |

## 中间数据的存储

有的时候，SQL中间处理的结果需要存储起来，以备后用。这时有两种办法：

1.创建View。

{% highlight sql %}
CREATE VIEW view_name AS
SELECT column_name(s)
FROM table_name
WHERE condition;
{% endhighlight %}

View并不在数据库中存储数据，而是在查询时，执行其中的select语句（每次查询，都会执行），生成中间结果。因此，View从原理来说，更像是一种语法糖，而非存储机制。

2.使用select语句创建table。

`Create table new_table_name (Select * from old_table_name);`

这种方法会将中间结果存储到数据库中，下次使用的时候，就无需重新生成了。但缺点是原table中的更新不会体现到新table中，只适合处理历史数据。

## 模糊查询

http://www.cnblogs.com/GT_Andy/archive/2009/12/25/1921914.html

SQL 模糊查询

## 三种Join的区别

left join(左联接)返回包括左表中的所有记录和右表中联结字段相等的记录。

right join(右联接)返回包括右表中的所有记录和左表中联结字段相等的记录。

inner join(等值连接)只返回两个表中联结字段相等的行。

http://www.cnblogs.com/pcjim/articles/799302.html

sql之left join、right join、inner join的区别

## 参考

https://mp.weixin.qq.com/s/ok6VD1b5fhG_mY9O3d_VGA

记住，永远不要在MySQL中使用“utf8”

https://mp.weixin.qq.com/s/duYi1Y5jEWSPQbO3Bgsrrw

SQL Server与MySQL中排序规则与字符集相关知识的一点总结

https://mp.weixin.qq.com/s/N7-8vtVUg3MRY2u_NYpAiA

一份值得收藏的的MySQL规范

https://mp.weixin.qq.com/s/YX1XqKVfPS9DpMi_gTFNiA

1000行MySQL学习笔记

https://www.cnblogs.com/andy6/p/6959028.html

从Oracle迁移到MySQL的各种坑及自救方案

# Android研究

## Flutter

Flutter是Google用以帮助开发者在Ios和Android两个平台开发高质量原生应用的全新移动UI框架。Beta1版本于2018年2月27日在2018世界移动大会上公布。

官网：

https://flutter.io/

参考：

https://mp.weixin.qq.com/s/pU75twMDry4VUYtTHeV_IQ

一文深入了解Flutter界面开发

https://mp.weixin.qq.com/s/yPvaB7sLuJoGfsjj7x7wcg

深入理解Flutter的编译原理与优化

https://mp.weixin.qq.com/s/SWP7Nu9DEUhvAyvdIG-Ixw

聊一聊Flutter Engine线程管理与Dart Isolate机制

https://mp.weixin.qq.com/s/cJjKZCqc8UuzvEtxK1BJCw

Flutter的原理及美团的实践

https://mp.weixin.qq.com/s/l6xvmnLE6HfRtw6upo6yUA

高效开发与高性能并存的UI框架——携程Flutter实践

https://mp.weixin.qq.com/s/vcbHMtaJEkZhSgiRBST1YA

移动开发这十年

## Litho

Litho是Facebook推出的一套高效构建Android UI的声明式框架，主要目的是提升RecyclerView复杂列表的滑动性能和降低内存占用。

官网：

https://fblitho.com/

参考：

https://mp.weixin.qq.com/s/RS7O7prvkCvKyxkK3YQxtA

Litho的使用及原理剖析

## 参考

https://mp.weixin.qq.com/s/twfpUMf9CfXcgwtFFkJ4Ig

Android整体设计及背后意义

https://mp.weixin.qq.com/s/eEuNPtTaPwJ7hSghgeU32g

Android Hook技术防范漫谈

# Restful

相比于WebService，Restful是一种简单的多的编程风格。

比如我们使用搜索引擎的时候，输入的地址：

`https://www.bing.com/search?q=java+restful`

就是一个典型的Restful请求。

有关Restful风格的内容参见：

https://segmentfault.com/a/1190000006735330

Restful应用分析

http://www.drdobbs.com/web-development/restful-web-services-a-tutorial/240169069

RESTful Web Services: A Tutorial

http://www.ibm.com/developerworks/library/ws-restful/index.html

RESTful Web services: The basics

还是那句老话，讨论一个通讯格式或协议，不讨论交互报文的都不是好文章，或者至少不是一个入门的好文章。

常见的Web框架如Spring、Struts都提供了对Restful的支持。

专门负责Restful的框架还有Jersey。其官网：

https://jersey.java.net/

示例：

https://github.com/feuyeux/jax-rs2-guide-II
