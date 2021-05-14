---
layout: post
title:  Mysql, Dubbo, Spring
category: technology 
---

* toc
{:toc}

# Mysql（续）

## 查询语句的执行顺序

![](/images/img3/sql.png)

https://mp.weixin.qq.com/s/CMcgybfgya7ftTUeUFigKg

SQL查询语句总是先执行SELECT？你们都错了

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

# Spring

Spring是一个Java Web应用框架。官网：

http://spring.io/

## Ubuntu安装Eclipse、Spring

1.安装Eclipse

`sudo apt-get install eclipse`

2.安装Spring

`sudo apt-get install libspring-web-portlet-java`

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

# Linux参考资源++

https://mp.weixin.qq.com/s/-U7L8aXoaPXSwZshSpjQ2g

进程间通信

https://mp.weixin.qq.com/s/oKtu3AA9D3y--xMDQ8EARw

携程一次Dubbo连接超时问题的排查

https://mp.weixin.qq.com/s/4o_cSzWeJdLJMObJBhaZlw

计算机系统中的内存

https://www.jianshu.com/p/fad3339e3448

浅析Linux中的零拷贝技术

https://mp.weixin.qq.com/s/Q9BOA88Q6OBaDch1AiS9QA

原来8张图，就可以搞懂零拷贝了

https://mp.weixin.qq.com/s/6R8UcLLjm9gdWud-eNHztw

中断及其初始化

https://mp.weixin.qq.com/s/qwouMWc4CFtqG_jra4xbIg

IDT及中断处理的实现

https://mp.weixin.qq.com/s/pRsXWAv7wgYcN_jlzcA2YA

内存都没了，还能运行程序？

https://mp.weixin.qq.com/s/snQ3T86usv4rXz0MMQvFfQ

如何回答性能优化的问题，才能打动阿里面试官？

https://www.cnblogs.com/zhouyu629/p/3734494.html

一次心惊肉跳的服务器误删文件的恢复过程

https://mp.weixin.qq.com/s/IcEP-JGQbWA7s7yPdIC9vA

80%时间屏蔽了中断，实时性还有救么？

https://mp.weixin.qq.com/s/iKfWSfzauzNjcAvXPNhq0Q

这些算法都不会还学什么操作系统

https://mp.weixin.qq.com/s/gj6Zw8SvOdSZqRx8KP9wWw

20张图揭开内存管理的迷雾，瞬间豁然开朗

https://mp.weixin.qq.com/s/IQYUNzVgSOFUHB9c1SM0Bw

10张图22段代码，万字长文带你搞懂虚拟内存模型和malloc内部原理

https://zhuanlan.zhihu.com/p/370092684

虚拟内存精粹

http://www.ruanyifeng.com/blog/2016/03/systemd-tutorial-commands.html

Systemd入门教程：命令篇

https://mp.weixin.qq.com/s/xOqXM5kFi0CzilDg0EXFKg

Linux内核源码规范解析

https://mp.weixin.qq.com/s/QB-IHiCIWEu3bALm2Dp46Q

操作系统课程知识结构

https://www.zhihu.com/answer/460715569

生产力应用大汇总

https://mp.weixin.qq.com/s/QsgoONKwI7ds8Hnx2Wer6A

Linux从程序到进程

https://mp.weixin.qq.com/s/v9XlJjIQkuVpSudhQIS70A

神秘！申请内存时底层发生了什么？

https://mp.weixin.qq.com/s/V-XT6QuDG522P0bP2e3ULg

咋办，死锁了

https://mp.weixin.qq.com/s/RHAoM8zhFvQl9R8V0ePxNQ

看腾讯这道多线程面试题

https://mp.weixin.qq.com/s/QshDG-nbmBcF1OBZbBFwjg

操作系统与存储：解析Linux内核全新异步IO引擎io_uring设计与实现

https://mp.weixin.qq.com/s/qWXcL90ZAkc7rrhsbuB_Bw

只有170字节，最小的64位Hello World程序这样写成

https://mp.weixin.qq.com/s/5iyWeSeDzuA2cY7YBMhk7w

MMU那些事儿

https://mp.weixin.qq.com/s/0OeeYUgBBVVMtxscvzgJHw

i++是线程安全的吗？

https://mp.weixin.qq.com/s/U0qr1oZYXBBmZnC5vsKYLQ

浅谈linux IO

https://mp.weixin.qq.com/s/3kgwoyYI90XHm1QPqFJAiQ

内存分页不就够了？为什么还要分段？

https://mp.weixin.qq.com/s/9x1JOl4m_mj0WpsVgHu4rg

Linux文件系统与持久性内存介绍

# TensorFlow+++

https://mp.weixin.qq.com/s/n4nEtyRc5G44kj3zmHpd5g

TensorFlow实战——图像分类神经网络模型

https://mp.weixin.qq.com/s/EytvywrsgydXAJQhuUqKvg

简易浣熊识别器是如何实现的

http://www.jianshu.com/p/d443aab9bcb1

在TensorFlow上使用LSTM进行情感分析

https://mp.weixin.qq.com/s/gW_KX6eF9XEsSUO1UzJ3WQ

基于LSTM的情感分析

https://mp.weixin.qq.com/s/KZhL477ApHgQfmM2xFrYJw

Tensorlang：基于TensorFlow的可微编程语言

https://mp.weixin.qq.com/s/_9NJ6QLQArUAD1DKb0KRfA

如何使用TensorFlow mobile部署模型到移动设备

https://mp.weixin.qq.com/s/e_TzQxFLAonLMyYAhte6Cg

face-api.js：在浏览器中进行人脸识别的JavaScript接口
