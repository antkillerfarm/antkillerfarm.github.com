---
layout: post
title:  Elasticsearch（二）, WebService, scrapy, 线程池
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

# WebService

WebService经过近二十年的发展，已经有非常多的框架了。知名的有：Axis1、Axis2、Xfire、CXF、JWS等。

其中的JWS在JDK 1.6之后被集成到JDK中，成为了我学习的首选。

JWS包含了JAX-WS、JAX-RS、JAXB、JAXR、SAAJ、StAX等组件。这里主要涉及的是JAX-WS。

## 教程

参考：

http://www.blogjava.net/zjhiphop/archive/2009/04/29/webservice.html

http://blog.csdn.net/lifetragedy/article/details/7205832

以上两篇是中文blog。

http://java.globinch.com/enterprise-java/web-services/jax-ws/java-jax-ws-tutorial-develop-web-services-clients-consumers/

这个教程虽然是英文的，但质量超过前两篇。且该网站还有其他相关文章，质量也颇高。

## demo

本文相关的demo参见：

https://github.com/him-bhar/jax-ws

这个demo包括jaxws-demo、jaxws-demo-client-stubs和jaxws-demo-client三个模块。

使用这个demo会有若干问题，也可直接采用本人修改之后的代码：

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/java/jax-ws

## 开发模型

JAX-WS 2.0有两种开发过程：**自顶向下**和**自底向上**。自顶向下方式指通过一个WSDL文件来创建Web Service，自底向上是从Java类出发创建Web Service。

**demo选用Server端通过Java Class生成webservice，而客户端通过wsdl生成Java调用类的做法。**

这种方法的优点在于：

1.服务端开发基于Java，基本不需要对WSDL有过多研究，上手简单。

2.客户端通过wsdl生成Java调用类，可以很方便的导入第三方WebService，同样无需对WSDL有过多研究。

综上，这实际上是个**由服务端驱动的开发模型**。

## jaxws-demo的结构

jaxws-demo包含了两个WebService例子分别是rpc_type和document_type。

这两种类型实际上指的是SOAP Binding的类型。两者的区别参见：

http://java.globinch.com/enterprise-java/web-services/soap-binding-document-rpc-style-web-services-difference/

>吐槽一下，虽然这只是个基本的问题，然而中文blog中竟然没有令人满意的文章。   
>这些文章的毛病在于：   
>1.对Java和JWS过于细节，而忽视了交互报文。SOAP Binding类型主要是个通讯协议问题，而不是Java或者JWS的问题。   
>2.没有比较交互报文，就来笼统的谈各自的优缺点的，都是耍流氓。

从上文的交互报文可以看出，document_type虽然更复杂，但也更灵活。从直观来看，前者只有wsdl一个文件，而后者有wsdl和xsd两个文件。

## jaxws-demo的部署

jaxws-demo编译之后，除了生成相应的.class之外，还会生成对应的wsdl和xsd文件。

通常的做法是将这些文件一起部署到诸如Tomcat之类的Web容器中。

但JDK也提供了更简易的做法，使用javax.xml.ws.Endpoint类的publish方法，将WebService绑定到特定的URI上。

这里我们将com.himanshu.poc.jaxws.service.deploy.EndpointPublisherDocument设为主类，并执行程序。

在浏览器上输入：

http://localhost:9999/ws/hellodocument?wsdl

正常情况下会返回一个wsdl文件。

## jaxws-demo-client-stubs

JDK中，有两个和WebService相关的工具：

JAXWS为我们提供了两个工具：

**wsgen**。主要用于Server端通过Java类编译成Webservice及相关的wsdl文件。

**wsimport**。主要用于Client端（调用端）通过wsdl编译出调用Server端的Java文件。

jaxws-demo中，调用wsgen生成的wsdl和xsd文件，在这里被wsimport编译成客户端的桩代码。

由于我使用的JDK是JDK 1.8，因此会遇到如下问题：

`由于 accessExternalSchema属性设置的限制而不允许 'file' 访问, 因此无法读取方案文档'xjc.xsd'。`

解决方法：

{% highlight xml %}
<plugin>
    <groupId>org.jvnet.jax-ws-commons</groupId>
    <artifactId>jaxws-maven-plugin</artifactId>
    <version>2.3</version>
    <configuration>
        <!-- Needed with JAXP 1.5 -->
        <vmArgs>
            <vmArg>-Djavax.xml.accessExternalSchema=all</vmArg>
        </vmArgs>
    </configuration>
</plugin>
{% endhighlight %}

这个问题是由于JDK 1.8以后相关权限变得更加严格所导致的。

参见：

https://my.oschina.net/fuckmylife0/blog/325432

## jaxws-demo-client

jaxws-demo-client就是具体的客户端实现了，可以看出相比于上一步的桩代码，这里的代码文件，数量上要少得多。

# scrapy

scrapy是一个Python写的网页抓取分析工具。网页抓取分析的学名叫做“Web scraping”，可在wiki上获得更多的相关信息。

官网：

https://scrapy.org/

安装：

`sudo apt install python-scrapy`

`scrapy crawl csdn`

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

# 线程池

假设一个服务器完成一项任务所需时间为：T1创建线程时间，T2在线程中执行任务的时间，T3销毁线程时间。如果：T1+T3远大于T2，则可以采用线程池，以提高服务器性能。

一个线程池包括以下四个基本组成部分：

1.线程池管理器（ThreadPool）：用于创建并管理线程池，包括 创建线程池，销毁线程池，添加新任务；

2.工作线程（PoolWorker）：线程池中线程，在没有任务时处于等待状态，可以循环的执行任务；

3.任务接口（Task）：每个任务必须实现的接口，以供工作线程调度任务的执行，它主要规定了任务的入口，任务执行完后的收尾工作，任务的执行状态等；

4.任务队列（taskQueue）：用于存放没有处理的任务。提供一种缓冲机制。

在libupnp项目中，提供了一个简易的线程池实现。

参考：

https://mp.weixin.qq.com/s/KigkxkUDYP8r1SEmDUvCCw

从串行线程封闭到对象池、线程池

