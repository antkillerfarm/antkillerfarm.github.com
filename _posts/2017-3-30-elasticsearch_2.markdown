---
layout: post
title:  Elasticsearch（二）, Flume & Kafka, 计算机体系结构
category: AI 
---

# Elasticsearch

## ELK的配置部署（续）

5.Bootstrap checks failing

当配置的host不是localhost的时候，ES会进行Bootstrap checks。其主要目的是增加ES能够获得的各种资源。一般不推荐在实际生产环境中，关闭Bootstrap checks。

>max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]

修改/etc/security/limits.conf：（需要root权限）

```text
es  soft    nproc     65536
es  hard    nproc     65536
es  soft    nofile    65536
es  hard    nofile    65536
```

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

https://mp.weixin.qq.com/s/K44-L0rclaIM40hma55pPQ

滴滴Elasticsearch多集群架构实践

https://mp.weixin.qq.com/s/1hRB3ylkJbcjUe4l-bpCsA

Elaticsearch在蚂蚁金服的实践经验

https://mp.weixin.qq.com/s/Hpy76P0spGJcDmmBCq2vpA

为什么已有Elasticsearch，我们还要重造实时分析引擎AresDB？

https://mp.weixin.qq.com/s/di_CJ7kBwI4ICe4mhz0Tcg

滴滴基于ElasticSearch的一站式搜索中台实践

https://mp.weixin.qq.com/s/-NfKH-2PpA-peq9CU0g1JA

解密Elasticsearch技术，腾讯开源的万亿级分布式搜索分析引擎

https://mp.weixin.qq.com/s/0TMESj2Z-XK2PzwBQo0Mpg

Elasticsearch调优实践

https://mp.weixin.qq.com/s/pT-6-U9mF4ttg0arp6BsVQ

Python+ElasticSearch：有了这个超级武器，你也可以报名参加诗词大会了！

https://mp.weixin.qq.com/s/W61SstvGawgVVqQxNa7GyQ

Elasticsearch入门学习权威指南，719页pdf教您构建分布式实时搜索和分析引擎

https://mp.weixin.qq.com/s/Olz-kvHM-SkC-pZr08r7ow

相关搜索—使用Solr和Elasticsearch，360页pdf

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

## 消息中间件的Style

两种最常见的Style: 消息队列方式(Message queuing)和发布订阅(publish-subscribe)方式。

参考：

https://mp.weixin.qq.com/s/oiaXjFxNcwJenkGuJBPm5Q

消息中间件的"Style"

## 参考

https://mp.weixin.qq.com/s/bjlDHFLwxjej2t8iDhVb1A

Spark Streaming消费Kafka数据的两种方案

https://mp.weixin.qq.com/s/o-zfrJS5Ito1kWPBJUIryg

Kafka相关资源

https://mp.weixin.qq.com/s/TzF6GBb1NI5iE8q2Rxo95Q

Kafka实战：Kafka in Action，209页pdf

https://mp.weixin.qq.com/s/l0AL89M0xPbWMFj6U7yYZw

消息中间件选型分析：从Kafka与RabbitMQ的对比看全局

https://mp.weixin.qq.com/s/oKBUb2IbG_h1xDhL42TbuQ

360开源又一力作——KafkaBridge：让操作kafka更简单！

https://mp.weixin.qq.com/s/P6K1tXuBZHaaizwGVo-22A

Kafka的API那么多，到底该怎么选？

https://mp.weixin.qq.com/s/XvWxaoob_PYpcOt8XpK7mw

杠上Spark、Flink？Kafka为何转型流数据平台

https://mp.weixin.qq.com/s/2kU9QhLP-njpToPckyfK5A

伯克利RISE Lab开源Confluo：吞吐量比Kafka高4到10倍

https://mp.weixin.qq.com/s/QJOfh5gJReQTimu-mAzexg

以Kafka和RocketMQ为例，漫谈消息队列

https://mp.weixin.qq.com/s/6aXcum-FAbvcGcOrkSC4vQ

为什么Kafka会成为微服务架构的事实标准？

https://zhuanlan.zhihu.com/p/87987916

Kafka基本原理

# 计算机体系结构

按照Michael J. Flynn的分类方法，计算机的体系结构可分为如下四类：

**Single instruction stream single data stream (SISD)**

**Single instruction stream, multiple data streams (SIMD)**

**Multiple instruction streams, single data stream (MISD)**

**Multiple instruction streams, multiple data streams (MIMD)**

![](/images/article/simd_mimd.png)

原图地址：

https://en.wikipedia.org/wiki/Flynn%27s_taxonomy

CPU通常是SISD和SIMD的，而GPU则是SIMD的，超级计算机则是MIMD的。

参考：

https://zhuanlan.zhihu.com/p/31271788

SIMD指令集

## 术语

Shared Virtual Memory，SVM

Memory consistency model

Multisample anti-aliasing，MSAA

## 参考

https://blog.csdn.net/do2jiang/article/details/4545889

流水线、超流水线、超标量技术对比

https://blog.csdn.net/edonlii/article/details/8755205

单发射与多发射

https://mp.weixin.qq.com/s/FJX9eeRkxS5nJ4XIhRVwkg

从ServerSwitch到SONiC Chassis：数据中心交换机技术的十年探索历程

# GAN参考资源

https://mp.weixin.qq.com/s/cUFQ6EADa39h2eFoa_Dh0A

最高76%破解成功率！GAN已经能造出“万能指纹”，你的手机还安全吗？

https://mp.weixin.qq.com/s/_tABIMkWX8L5xQFmvPI7rw

有效稳定对抗模型训练过程，伯克利提出变分判别器瓶颈

https://zhuanlan.zhihu.com/p/50790727

SeqGAN: Sequence GAN with Policy Gradient

https://mp.weixin.qq.com/s/bH5yYbwq6NGQJ84xUDhoxg

生成对抗网络在图像翻译上的应用

https://mp.weixin.qq.com/s/3Gsmrl4HbcnXpje0nyAq2w

中国西北大学和北京大学的研究结果是否将终结CAPTCHA验证码时代？

https://zhuanlan.zhihu.com/p/53260242

抛开复杂证明，我们从直觉上理解W-GAN为啥这么好训

https://mp.weixin.qq.com/s/FJA8Tctq_p4Mj-KgNn_OGg

为什么让GAN一家独大？Facebook提出非对抗式生成方法GLANN

https://mp.weixin.qq.com/s/xr9fDv9DFkwi2ImV4RZAHg

换个角度看GAN：另一种损失函数

https://mp.weixin.qq.com/s/U1rrPfJDLgXHRj__XwrMZw

只有条件GAN才能稳定训练？对抗+自监督的无监督方法了解一下

https://mp.weixin.qq.com/s/xHKQlFFkBQLBg2GdZuGPSw

提升GAN训练的技巧汇总

https://mp.weixin.qq.com/s/ctB90bNhaMYvbLE4yketHQ

一文读懂对抗机器学习Universal adversarial perturbations

https://mp.weixin.qq.com/s/zRNtEKS2dKxyQU4VZm6Mwg

用GANs来自动生成音乐

https://mp.weixin.qq.com/s/zwzl-Tel3Avc4Dm7L5FS5A

NLP中的对抗训练+PyTorch实现
