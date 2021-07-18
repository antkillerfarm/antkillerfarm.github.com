---
layout: post
title:  Mysql, Dubbo, Spring
category: technology 
---

* toc
{:toc}

# Mysql（续）

## 模糊查询

http://www.cnblogs.com/GT_Andy/archive/2009/12/25/1921914.html

SQL模糊查询

## 三种Join的区别

left join(左联接)返回包括左表中的所有记录和右表中联结字段相等的记录。

right join(右联接)返回包括右表中的所有记录和左表中联结字段相等的记录。

inner join(等值连接)只返回两个表中联结字段相等的行。

http://www.cnblogs.com/pcjim/articles/799302.html

sql之left join、right join、inner join的区别

## 查询语句的执行顺序

![](/images/img3/sql.png)

https://mp.weixin.qq.com/s/CMcgybfgya7ftTUeUFigKg

SQL查询语句总是先执行SELECT？你们都错了

---

当Explain与SQL语句一起使用时，MySQL会显示来自优化器关于SQL执行的信息。也就是说，MySQL解释了它将如何处理该语句，包括如何连接表以及什么顺序连接表等。

https://mp.weixin.qq.com/s/xf4VRBFLhhIAGGZPhbRetQ

不会看Explain执行计划，简历敢写SQL优化？

## MyISAM & Innodb

Mysql底层数据引擎以插件形式设计，最常见的是Innodb引擎和Myisam引擎，用户可以根据个人需求选择不同的引擎作为Mysql数据表的底层引擎。

MyISAM虽然数据查找性能极佳，但是不支持事务处理。Innodb 最大的特色就是支持了ACID兼容的事务功能，而且他支持行级锁。

## 参考

https://mp.weixin.qq.com/s/9tIy1IqjaB0NdKaO76dgCw

47张图带你MySQL进阶！！！

https://mp.weixin.qq.com/s/ok6VD1b5fhG_mY9O3d_VGA

记住，永远不要在MySQL中使用“utf8”

https://mp.weixin.qq.com/s/duYi1Y5jEWSPQbO3Bgsrrw

SQL Server与MySQL中排序规则与字符集相关知识的一点总结

https://mp.weixin.qq.com/s/N7-8vtVUg3MRY2u_NYpAiA

一份值得收藏的的MySQL规范

https://mp.weixin.qq.com/s/YX1XqKVfPS9DpMi_gTFNiA

1000行MySQL学习笔记

https://www.jianshu.com/p/bdc9e57ccf8b

mysql索引篇之覆盖索引、联合索引、索引下推

https://mp.weixin.qq.com/s/PgGDeowQaQ6MlUExZE-TMA

如果是MySQL引起的CPU消耗过大，你会如何优化？

https://mp.weixin.qq.com/s/L3ydmN1TP5YsxJ97WeFXGg

万字总结：学习MySQL优化原理，这一篇就够了！

https://mp.weixin.qq.com/s/vmKPKn9Y1FPY8MWy2Xcu6A

MySQL的自增ID用完了，怎么办？

https://mp.weixin.qq.com/s/H0FI_Ci0YAQyUfaK3OVDJw

InnoDB自增原理都搞不清楚，还怎么CRUD？

https://mp.weixin.qq.com/s/QduIxKOykMmoZp13UGF1XQ

MySQL索引知识点总结

https://mp.weixin.qq.com/s/E0PIOTflCicyPs5fcv0qAg

Mysql高性能优化规范建议

https://mp.weixin.qq.com/s/0AlXjjYIQUU9tkw2LNiR_A

MySQL不会丢失数据的秘密，就藏在它的7种日志里

https://mp.weixin.qq.com/s/b7Qnzh1EIM4wbExwmIkJyA

一个线上SQL死锁异常分析：深入了解事务和锁

https://mp.weixin.qq.com/s/58Xm6IrrTClny6DsNAbxrg

详解一条SQL的执行过程

https://mp.weixin.qq.com/s/Vlsm9rHHaCl02gZmDQfgDA

到底是什么原因才导致 select * 效率低下的

https://www.toutiao.com/i6843323300764975628/

MySQL压力测试工具

https://mp.weixin.qq.com/s/giy44TE0QVHWoQwMg84jNA

MySQL中ORDER BY与LIMIT不要一起用

https://www.cnblogs.com/andy6/p/6959028.html

从Oracle迁移到MySQL的各种坑及自救方案

https://mp.weixin.qq.com/s/txbusDvTKwFZdX94kDp7VQ

10个不为人知的SQL技巧

https://mp.weixin.qq.com/s/qquMk4M81Pjw9fxkostf4Q

小米：Mysql数据实时同步实践

https://shockerli.net/post/1000-line-mysql-note/

一千行MySQL学习笔记

https://mp.weixin.qq.com/s/SCW_3AypO-rSolMcjCxVtA

MySQL事务隔离级别和MVCC

https://mp.weixin.qq.com/s/SMNMM4wmEOGeUJjRy6XSBA

MVCC机制了解吗

https://mp.weixin.qq.com/s/qHJiTjpvDikFcdl9SRL97Q

深入理解MySQL索引底层原理

https://mp.weixin.qq.com/s/KBvNzs_y1RDXoDnwB319JA

MySQL磁盘清理

https://mp.weixin.qq.com/s/t0gfW7UHzkKgNrAVyOfK7Q

吊打MySQL，MariaDB到底强在哪？

https://mp.weixin.qq.com/s/-QPabXE2plPB0lok-Ic9jA

MySQL锁系统总结

https://mp.weixin.qq.com/s/sRFmW57KUY3yyyRkyw0L4A

MySQL深入学习总结

https://mp.weixin.qq.com/s/5brbbLOJrnwZxnF5RpbMqA

MySQL隐式转换的坑，一起来看看究竟！

https://mp.weixin.qq.com/s/pb3U4oYDSnmx0ZhLnVy58A

MySQL为Null会导致5个问题，个个致命！

https://mp.weixin.qq.com/s/WD804KImL5jCvq-EJUdmSw

100道MySQL数据库经典面试题解析

# 数据库参考资源

https://www.cnblogs.com/ahu-lichang/p/10899747.html

数据库三大范式（1NF,2NF,3NF）及ER图

https://mp.weixin.qq.com/s/m_JMXU6iMS4SckzWZYtIUA

腾讯分布式数据库TDSQL金融级能力的架构原理解读

https://mp.weixin.qq.com/s/jdPE9WClBuimIHVxJnwwUw

字节跳动自研强一致在线KV&表格存储实践-上篇

https://mp.weixin.qq.com/s/DvUBnWBqb0XGnicKUb-iqg

字节跳动自研强一致在线KV&表格存储实践-下篇

https://mp.weixin.qq.com/s/PPlrrtcdrtKT3afgWOU8Pg

为什么数据库不应该使用外键

https://mp.weixin.qq.com/s/k1sK-QQjVqdJnpWNCV0pUA

并发环境下，先操作数据库还是先操作缓存？

https://mp.weixin.qq.com/s/I3ca2HAuSTtUJSZyHNe1vA

数据库读写分离要注意哪些细节

https://mp.weixin.qq.com/s/oV5F_K2mmE_kK77uEZSjLg

字节跳动分布式表格存储系统的演进

https://mp.weixin.qq.com/s/ZjnRzI18plggKTv_nPBsEw

字节跳动表格存储中的事务

https://mp.weixin.qq.com/s/thg9xOKI-GWfADF5tXh29A

面试官：了解数据库连接池吗？

# Dubbo

Dubbo是一个分布式服务框架，致力于提供高性能和透明化的RPC远程服务调用方案，是阿里巴巴SOA服务化治理方案的核心框架，每天为2,000+个服务提供3,000,000,000+次访问量支持，并被广泛应用于阿里巴巴集团的各成员站点。

>阿里巴巴算是国内开源较多的IT企业了。但是早期仅仅满足于开源本身，对于开源项目的维护没有章法。Dubbo就是典型一例，开源之后的数年，没有任何官方升级和维护。社区由于官方的缺位，也没有大的动静。直到2016年，才纳入正轨。

官网：

http://dubbo.io/

官网的用户指南写的不错，非常值得一看。

https://mp.weixin.qq.com/s/bcwIMIir2RHPbQQr8HgTOQ

如何快速开发一个Dubbo应用？

https://mp.weixin.qq.com/s/fnrGjiywiySA8iAZh_cF0Q

阿里巴巴新开源项目Nacos发布第一个版本，助力构建Dubbo生态

https://mp.weixin.qq.com/s/AAcQRHZPvW11jvlbrLfRJA

携程的Dubbo之路

https://mp.weixin.qq.com/s/ZW4tO01gC65kZgOUappL9Q

漫话：什么是RPC

https://blog.csdn.net/m0_38110132/article/details/81481454

直观讲解--RPC调用和HTTP调用的区别

https://juejin.cn/post/6963642641506369566

为什么说Dubbo不适合传输文件？

# Spring

Spring是一个Java Web应用框架。官网：

http://spring.io/

## Ubuntu安装Eclipse、Spring

1.安装Eclipse

`sudo apt install eclipse`

2.安装Spring

`sudo apt install libspring-web-portlet-java`

注意：ubuntu软件仓库中还有一个叫做spring的游戏引擎，不要弄错了。

http://www.mkyong.com/spring/quick-start-maven-spring-example/

Maven+Spring hello world example

http://wiki.jikexueyuan.com/project/spring/

Spring 教程

## Restful

http://spring.io/guides/gs/rest-service/

## Spring Boot

https://www.tianmaying.com/tutorial/deploy-spring-boot-application

部署Spring Boot应用

http://docs.spring.io/spring-boot/docs/current/reference/html/common-application-properties.html

Spring Boot默认的配置文件

https://mp.weixin.qq.com/s/7I5OfGnGtXk2u9PtGLObCA

七个开源的Spring Boot前后端分离项目

## WebService

https://spring.io/guides/gs/producing-web-service/

http://localhost:9999/ws/countries.wsdl

# WSL

Cygwin：提供了兼容POSIX接口的应用层接口，性能不佳。

MinGW：直接接Win32 API，只移植了GNU工具链，功能不全。

MSYS2：MinGW+Cygwin部分工具包+pacman。

WSL（Windows Subsystem for Linux），刚出来的时候叫“Bash on Ubuntu on Windows”：POSIX接口直接接到NT内核，性能超过Cygwin。

WSL2：虚拟机，有独立的Linux内核。和VirtualBox之类类似。

https://www.zhihu.com/question/50144689

win10 linux子系统和cygwin有什么不同？

https://mp.weixin.qq.com/s/ZCkboBBFYFm57pLEGftpCw

双系统的日子结束了：Windows和Linux将合二为一

https://zhuanlan.zhihu.com/p/57774611

盘点与Cygwin相似和相反的项目
