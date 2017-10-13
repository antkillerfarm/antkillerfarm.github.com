---
layout: post
title:  Elasticsearch（二）, WebService, 机器人参考资源
category: technology 
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

# 机器人参考资源

## 课程

http://selfdrivingcars.mit.edu/

MIT 6.S094: Deep Learning for Self-Driving Cars

>主讲：Lex Fridman，MIT博士后。   
>个人主页：   
>http://lexfridman.com/

这个课程不仅提供了课件，还提供了DeepTraffic和DeepTesla两个小工具供学生验证自己的算法。这两个工具是用ConvNetJS编写的。

ConvNetJS是Andrej Karpathy编写的基于JavaScript的DL框架。

官网：

http://cs.stanford.edu/people/karpathy/convnetjs/

代码：

https://github.com/karpathy/convnetjs

## 文章

http://blog.exbot.net/

一个机器人技术方面的网站。

http://www.ros.org/

ROS(Robot Operating System）是一个机器人软件平台，前身是斯坦福人工智能实验室为了支持斯坦福智能机器人STAIR而建立的项目。

https://rsu.data61.csiro.au/people/jalvarez/research_bbdd.php

这个网站提供了一系列用于汽车自动驾驶的视频标注的工具。

>Jose M. Alvarez，西班牙巴塞罗那自治大学博士（2010年）。现为澳大利亚CSIRO研究员。CSIRO是澳大利亚最大的国家级科研机构。

http://www.cnblogs.com/yhlx125/p/4707693.html

SLAM(simultaneous localization and mapping)学习笔记

http://www.cnblogs.com/gaoxiang12/p/5113334.html

视觉SLAM中的数学基础

https://www.zhihu.com/question/25371476

怎样从实际场景上理解粒子滤波（Particle Filter）？

LIDAR：LIght Detection And Ranging

https://zhuanlan.zhihu.com/p/26988866

机器人学习Robot Learning的发展

https://mp.weixin.qq.com/s/tH-1XTEC-S7bIFoDdDK5CQ

如何给机器人一双慧眼：从视觉SLAM技术说起！

http://www.cnblogs.com/gaoxiang12/p/3695962.html

视觉SLAM漫谈

https://mp.weixin.qq.com/s/YLhECwwig9f21zk1-PNiTw

25篇车辆检测与分类DL文章读懂自动驾驶

https://mp.weixin.qq.com/s/vKkjYdf6Ast7wWcW9oIEuA

88美元的自动驾驶“自制原子弹”，最著名黑客详解panda系统

https://zhuanlan.zhihu.com/p/27935902

机器人学习Robot Learning之模仿学习Imitation Learning的发展

https://mp.weixin.qq.com/s/pPPq3b1yj92RaGgIpTAhqQ

一文看透汽车无人驾驶技术、产品和市场

https://mp.weixin.qq.com/s/ZePNQ3l3uMMIlndk20sOVA

基于视频的无监督深度和车辆运动估计

https://mp.weixin.qq.com/s/xr-2cNoSYpCftLI3dV6zEw

如何使用深度强化学习帮助自动驾驶汽车通过交叉路口？




