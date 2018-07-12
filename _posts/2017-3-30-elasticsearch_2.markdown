---
layout: post
title:  Elasticsearch（二）, scrapy, Flume & Kafka, Flutter, 垃圾筐
category: AI 
---

# Elasticsearch

## ELK的配置部署（续）

5.Bootstrap checks failing

当配置的host不是localhost的时候，ES会进行Bootstrap checks。其主要目的是增加ES能够获得的各种资源。一般不推荐在实际生产环境中，关闭Bootstrap checks。

>max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]

修改/etc/security/limits.conf：（需要root权限）

{% highlight text %}
es  soft    nproc     65536
es  hard    nproc     65536
es  soft    nofile    65536
es  hard    nofile    65536
{% endhighlight %}

重新登录es用户后，修改生效。

在bin/elasticsearch的开头添加：

`ulimit -n 65536`

>注：ulimit增加的资源数，不能超过limits.conf中的数量，否则会报错。

>max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]

修改/etc/sysctl.conf：（需要root权限）

`vm.max_map_count=262144`

执行`sysctl -p`使配置生效。

参考：

http://stackoverflow.com/questions/42300463/elasticsearch-5-x-bootstrap-checks-failing

5.数据存储

默认情况下，data和log都在ES文件夹下的同名文件夹下。可在config/elasticsearch.yml中修改之。

## Java REST Client

ES的Client支持多种语言。其中，Java语言有两种API：Java API和Java REST API。其中，前者对后者的调用进行了封装，但由于REST命令可以直接在kibana中调试，实际使用中，反而后者更方便一些。

Java REST API的示例参见：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/helloworld/elasticsearch/es_client_hello

其中，test1函数给出了基本的查询示例。test2函数给出了json格式查询的示例，test3函数对查询返回的json数据采用jackson的树模型进行解析，test3函数对查询返回的json数据采用jackson的流模型进行解析。

更全面的示例参见：

https://github.com/Top-Q/elasticsearch-client

## logstash-output-jdbc

`bin/logstash-plugin install logstash-output-jdbc`

https://github.com/theangryangel/logstash-output-jdbc/blob/master/examples/mysql.md

## Spring

Spring也提供了对ES的支持。

SpringBoot官方的ES文档：

http://docs.spring.io/spring-data/elasticsearch/docs/current/reference/html/

这篇文章里的Table 2写的很好，可以方便用户快速掌握最常用的查询语法。

SpringBoot官方的ES示例：

https://github.com/spring-projects/spring-boot/tree/master/spring-boot-samples/spring-boot-sample-data-elasticsearch

Spring的问题在于它的发布由于和ES并无关联，因此更新比较迟。而且内部由于使用的是ES Java API，对ES版本有要求，通用性上不如Java REST API。

参考：

http://blog.720ui.com/2016/springboot_02_data_elasticsearch/

Spring Boot揭秘与实战（二）数据存储篇-ElasticSearch

https://juejin.im/entry/58b56c4a8d6d81005765fc73

SpringBoot整合Elasticsearch

## 参考

http://cloud.51cto.com/art/201505/476450.htm

五类Elasticsearch扩展性插件推荐

http://blog.csdn.net/cnweike/article/details/33736429

Elasticsearch基础教程

http://blog.csdn.net/a809146548/article/details/52371110

Logstash使用详解

http://www.cnblogs.com/ajianbeyourself/p/5529575.html

Elasticsearch教程-从入门到精通

http://www.freebuf.com/sectool/78225.html

Elk大数据查询系列：Elasticsearch与Logstash基础篇

http://www.tuicool.com/articles/YR7RRr

ELK搭建实时日志分析平台

http://467754239.blog.51cto.com/4878013/1700828/

ELK 日志分析系统

https://www.ibm.com/developerworks/cn/opensource/os-cn-elk/

集中式日志系统ELK协议栈详解

https://es.xiaoleilu.com/

Elasticsearch权威指南（中文版）

http://udn.yyuap.com/doc/logstash-best-practice-cn/

logstash最佳实践

https://zhuanlan.zhihu.com/p/24428355

使用ElasticSearch踩过的坑

https://zhuanlan.zhihu.com/p/25723815

教你快速使用Tensorflow/Elasticsearch实现全文的图片搜索

http://www.cnblogs.com/buzzlight/p/logstash_elasticsearch_kibana_log.html

使用logstash+elasticsearch+kibana快速搭建日志平台

http://blog.csdn.net/longxibendi/article/details/35237543/

ELK入门学习资源索引

http://www.jianshu.com/p/0b4346f503e3

探索elasticsearch。该文包含如何使用ES进行TF/IDF的方法

https://my.oschina.net/taogang/blog/983586

ElasticSearch对比Splunk

https://mp.weixin.qq.com/s/jZ_jM6yUPK8Ev7FSwdgTIA

360私有云平台Elasticsearch服务初探

https://mp.weixin.qq.com/s/osOggCYvzun6X6yquD7cYg

浅析ElasticSearch原理

https://mp.weixin.qq.com/s/j_9PCwWoGu9cZM9sD1klog

Elasticsearch性能监控（一）

http://bbotte.com/logs-service/use-elk-processing-logs-multiple-log-file-send/

ELK日志服务使用-filebeat多文件发送

https://www.digitalocean.com/community/tutorials/how-to-map-user-location-with-geoip-and-elk-elasticsearch-logstash-and-kibana

How To Map User Location with GeoIP and ELK (Elasticsearch, Logstash, and Kibana)

https://mp.weixin.qq.com/s/kxBxaK72ovaMHQgumXALmw

Elasticsearch性能监控（二）

http://blog.csdn.net/qq_21383435/article/details/79367457

linux下ElasticSearch.6.2.1与head、Kibana、X-Pack、SQL、IK、PINYIN插件的配置安装

https://mp.weixin.qq.com/s/Wzrt7H9gDIUQn7KEc0qZHQ

在Python中使用Elasticsearch

# scrapy

scrapy是一个Python写的网页抓取分析工具。网页抓取分析的学名叫做“Web scraping”，可在wiki上获得更多的相关信息。

官网：

https://scrapy.org/

安装：

`sudo apt install python-scrapy`

`scrapy crawl csdn`

新建工程：

`scrapy startproject tutorial`

参考：

https://segmentfault.com/a/1190000000583419

一个中文简易教程。

https://github.com/scrapy/dirbot

官方例程。

http://www.cnblogs.com/fengzheng/p/4974509.html

另一个中文简易教程。

https://mp.weixin.qq.com/s/nIcUBS0lRrOwVUHoWmKecw

爬虫系列之使用scrapy框架

https://mp.weixin.qq.com/s/A2QNr4-LTUNy-H8zE0OgXA

教你用Scrapy建立你自己的数据集

https://mp.weixin.qq.com/s/i-umuOi8jGw8dMQmG44liQ

如何租到靠谱的房子？Scrapy爬虫帮你一网打尽各平台租房信息！这篇blog中，还有如何用Kibana处理数据的内容

https://mp.weixin.qq.com/s/hzl8D-AhpCwqZVSLTK56XQ

Python爬虫--Scrapy入门

# Flume & Kafka

Flume和Kafka都是日志系统。

Flume官网：

https://flume.apache.org/

Kafka官网：

https://kafka.apache.org/

以下是它们的比较：

http://www.cnblogs.com/ibyte/p/5830715.html

Flume与Kafka区别

http://www.aichengxu.com/view/2412170

kafka和flume的对比

http://www.cnblogs.com/lishouguang/p/4560858.html

flume使用场景 flume与kafka的比较

https://mp.weixin.qq.com/s/bjlDHFLwxjej2t8iDhVb1A

Spark Streaming消费Kafka数据的两种方案

https://mp.weixin.qq.com/s/o-zfrJS5Ito1kWPBJUIryg

Kafka相关资源

https://mp.weixin.qq.com/s/l0AL89M0xPbWMFj6U7yYZw

消息中间件选型分析：从Kafka与RabbitMQ的对比看全局

# Flutter

Flutter是Google用以帮助开发者在Ios和Android两个平台开发高质量原生应用的全新移动UI框架。Beta1版本于2018年2月27日在2018世界移动大会上公布。

官网：

https://flutter.io/

参考：

https://mp.weixin.qq.com/s/pU75twMDry4VUYtTHeV_IQ

一文深入了解Flutter界面开发

https://mp.weixin.qq.com/s/yPvaB7sLuJoGfsjj7x7wcg

深入理解Flutter的编译原理与优化

# 垃圾筐

## 压缩感知

http://blog.csdn.net/abcjennifer/article/details/7721834

初识压缩感知Compressive Sensing

http://blog.csdn.net/abcjennifer/article/details/7724360

中国压缩传感资源（China Compressive Sensing Resources）

http://blog.csdn.net/xiahouzuoxin/article/details/38820925

白话压缩感知（含Matlab代码）

http://blog.csdn.net/abcjennifer/article/details/7748833

压缩感知进阶——有关稀疏矩阵

## 元胞自动机

http://www.doc88.com/p-8939046003005.html

元胞自动机理论及其在计算机仿真模拟中的应用

http://blog.csdn.net/chl033/article/details/5367861

元胞自动机与相关理论和方法

http://blog.csdn.net/deepfuture/article/details/5065284

元胞自动机

http://www.cnblogs.com/Firefly727/articles/1856328.html

元胞自动机

## 聚类

https://mp.weixin.qq.com/s/xGPiaXTnQad3RcMwIELP4w

流式聚类算法

https://wenku.baidu.com/view/4169e77f27284b73f2425047.html

层次聚类

https://mp.weixin.qq.com/s/uSHLJKB0knVcCY759Ul25w

什么是聚类和降维？

https://mp.weixin.qq.com/s/ORLOOhufrInyPdS6fbywOw

深入浅出——基于密度的聚类方法

https://mp.weixin.qq.com/s/Vi4Yb8TOJydj9yL078iNHw

BIRCH层次聚类详解

https://mp.weixin.qq.com/s/_5A3DuVyN6aE9n5OEc19kA

数据科学家必须了解的六大聚类算法：带你发现数据之美

https://mp.weixin.qq.com/s/pDiZt4ydWw-4cYeE4gpjiw

数据科学家需要了解的5大聚类算法

https://www.zhihu.com/question/34554321

用于数据挖掘的聚类算法有哪些，各有何优势？

https://mp.weixin.qq.com/s/7zV370J7nv5wSWxdYa5Plg

DBSCAN聚类算法原理介绍，以及代码实现

https://mp.weixin.qq.com/s/8dB2OQCoZ7_kSxExm32WSA

于剑：聚类理论与算法选讲

## 可视化

![](/images/img2/data_visual.png)

https://mp.weixin.qq.com/s/Ul6Qg2ylCgvDfJTQre4DWQ

2017年数据可视化的七大趋势

https://mp.weixin.qq.com/s/i2Ex8Zmu2ZzHPcoPr-1Rlw

太阳系相关图，教你优雅的打开“可视化”的大门

https://mp.weixin.qq.com/s/q5mPN_rt1Af5L2fnxuTdCA

46款数据可视化分析工具大集合

https://mp.weixin.qq.com/s/tHNd6y2v7GXN2mSIv0qXlg

手把手：一张图看清编程语言发展史，你也能用Python画出来！

https://mp.weixin.qq.com/s/mD732PqDtqYdFZSxZWtvvg

从1维到6维，一文读懂多维数据可视化策略

https://zhuanlan.zhihu.com/c_122608198

一个可视化方面的专栏

## howmuch.net

howmuch.net是一个金融类的数据可视化网站，挺有意思的。比如下图：

![](/images/article/top_10_company.jpg)

网址：

https://howmuch.net/

## Gephi

Gephi是一个针对图和网络的可视化工具。

官网：

https://gephi.org/

## 贝叶斯线性回归

https://mp.weixin.qq.com/s/szTmHY-Yvn7N3s_GzTDiEA

解开贝叶斯黑暗魔法：通俗理解贝叶斯线性回归

https://mp.weixin.qq.com/s/1JSxjkKEUlWOzXCQPTve3A

贝叶斯线性回归简介

## 金融模型

Capital asset pricing model

Fama–French three-factor model

Carhart four-factor model

## 强连通分量算法

http://ishare.iask.sina.com.cn/f/34626295.html

矩阵不可约的充要条件

http://www.cnblogs.com/saltless/archive/2010/11/08/1871430.html

求强连通分量的Tarjan算法

http://blog.csdn.net/dm_vincent/article/details/8554244

求解强连通分量算法之---Kosaraju算法

http://www.cnblogs.com/luweiseu/archive/2012/07/14/2591370.html

强连通分支算法--Kosaraju算法、Tarjan算法和Gabow算法

## LSM (Log Structured Merge)

十年前，谷歌发表了 “BigTable” 的论文，论文中很多很酷的方面，其中之一就是它所使用的文件组织方式，这个方法更一般的名字叫Log Structured-Merge Tree。

http://www.open-open.com/lib/view/open1424916275249.html

Log Structured Merge Trees(LSM)原理

https://mp.weixin.qq.com/s/CmYg22NObamkNOqGjKg0-w

解读现代存储系统背后的经典算法

