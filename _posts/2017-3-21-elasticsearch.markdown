---
layout: post
title:  Elasticsearch
category: technology 
---

# Elasticsearch

## 概况

Elasticsearch（简称ES）最早是由Shay Banon于2010年发起的开源项目，目前已经成立了公司专门运作该项目。

官网：

https://www.elastic.co

发展到今天（2017.3），Elasticsearch已经由单一的软件，发展成为Elastic Stack。其中包括：

核心组件：ElasticSearch, Logstash, Kibana，即所谓的ELK。

扩展组件：Beats, X-Pack, Cloud

## 简易安装步骤

为了避免版本混乱，目前Elastic Stack将各组件的版本号统一为一个。使用时，各组件版本号应该一致。

>Elastic Stack的版本号为X.Y.Z的形式，例如5.2.3。其中，各组件的Z可以不相同，但X、Y必须一致。

### Elasticsearch

#### 运行

```
cd ES_HOME
bin/elasticsearch
```

#### 测试

`curl -X GET http://localhost:9200/_cat/health?v`

### Logstash

#### 运行

```
cd LS_HOME
bin/logstash -f logstash.conf
```

LS_HOME/logstash.conf：

```
input { stdin { } }
output {
  elasticsearch { hosts => ["localhost:9200"] }
  stdout { codec => rubydebug }
}
```

#### 测试

`curl -X GET http://localhost:9600`

### Kibana

#### 运行

```
cd K_HOME/bin
bin/kibana
```

#### 测试

浏览器访问如下网址：

http://localhost:5601/

### 服务模式

上面提到的运行方式，由于运行日志直接输出到控制台，调试起来比较方便，但有个问题就是不能关闭控制台，否则程序会在关闭时被杀死。

这个问题的解决办法是使用服务模式。

1.Elasticsearch

`bin/elasticsearch -d -p pid`

可用jps命令查找到ES，并kill之。

2.Kibana

`nohup bin/kibana &`

可用ps命令查找到node，并kill之。（Kibana是用node.js写的。）

## ES的存储层次

ES和MySQL的层级对应关系如下：

| mysql | 数据库databases | 表tables | 行rows | 列columns |
|:--:|:--:|:--:|:--:|:--:|
| es | 索引indices | 类型document types | 文档documents | 字段fields |

以如下conf文件为例：

```
input { stdin { } }
output {
  elasticsearch { hosts => ["localhost:9200"] index => "msg" document_type => "true_msg"}
  stdout { codec => rubydebug }
}
```

其查询语句为：

```
GET msg/true_msg/_search
{
  "query": {
    "match_all": {}
  },
  "_source": ["message"]
}
```

## 编写LogStash的conf

LogStash的conf包含3个部分：input,filter,output。其中filter为可选项，其余为必选项。

同一个大括号内的若干命令为顺序执行的关系。使用`if{}else{}`实现条件控制。

### Sample 1

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/helloworld/logstash/logstash-csv.conf

这个示例展示了如何导入csv数据文件，如何创建ES的index和document_type。

由于我们使用的csv文件是GBK编码的，直接导入会有乱码，因此，这个示例还有如何转换文件编码格式的方法。

### Sample 2

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/helloworld/logstash/logstash-csv1.conf

这个示例展示了如何将一个csv文件的前两列写入一个ES表，而将后一列写入另一个ES表。

本例使用了type作为条件，进行条件分支判断。但是这个方法不是最优的，因为csv文件被读取了2次。

### Sample 3

https://github.com/antkillerfarm/antkillerfarm_crazy/blob/master/helloworld/logstash/logstash-csv2.conf

这个示例的目标和Sample 2相同。

这里采用clone filter，生成了另一个type不同的事件，从而达到分支的目的。

## ES的查询示例

### 添加数据

```
PUT /customer/external/1?pretty
{
  "name": "John Doe"
}
```

### 删除数据

`DELETE /customer?pretty`

查询删除：

```
GET msg/true_msg/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}
```

### 关键字查询

**简单查询**：

```
GET msg/true_msg/_search
{
  "query": {
    "match": { "message": "购物车" }
  }
}
```

上面的查询有个问题，只要出现“购物车”三个字中任何一个字的message，都会被选中。

**全字匹配**


```
GET msg/true_msg/_search
{
  "query": {
    "multi_match": {
      "query": "购物车",
      "type": "phrase",
      "slop": 0,
      "fields": ["message"]
    } 
  },
  "highlight": {  
    "pre_tags": [  
      "<b>"  
    ],  
    "post_tags": [  
      "</b>"  
    ],  
    "fragment_size": 100,  
    "number_of_fragments": 2,  
    "fields": {  
      "message": {}  
    }
  }
}
```

上面的例子同时展示了highlight的用法。

**统计词频**

```
GET twitter/tweet/5/_termvectors?pretty=true
{
  "fields" : ["text"],
  "offsets" : true,
  "payloads" : true,
  "positions" : true,
  "term_statistics" : true,
  "field_statistics" : true
}
```

统计结果包括Field统计和Term统计两类。

**Field统计**：

doc_count：doc的数量，这里的doc通常对应Logstash导入的一条记录。

sum_ttf：所有doc中包含的词的总和。

sum_doc_freq：所有doc中包含的独立词的总和。

例如如下语句：

`twitter test test test`

ttf统计中认为这是4个词，而doc_freq中认为这是2个词，因为test重复出现了3次，这里只能算作1个词。注意这里的独立词是针对doc而言的，在不同doc中出现的test被认为是2个词。

**Term统计**：

term_freq：该词在本doc中出现的次数。

ttf：该词在所有doc中出现的次数。

doc_freq：该词在所有doc中独立出现的次数。

可见_termvectors的查询虽然针对的是某个文件，但返回的结果中，却包含了全局的统计数据。

>注：这里的全局，严格来说只到分片一级。当index比较大的时候，往往数据会保存到多个分片中。这样的话，不同doc所对应的全局统计结果，往往会有所不同。可以通过设置，实现真正的全局统计，但一般无此必要。

## ES中文分词

ES的中文分词功能，以插件的形式提供。主要包括官方维护的Smartcn和Medcl维护的IK、Mmseg。

### Smartcn

安装方法：

`sudo bin/elasticsearch-plugin install analysis-smartcn`

这个例子同时展示了ES在线安装插件的方法。插件安装成功之后，需要重启ES，使之生效。

验证：

```
POST msg/true_msg/_analyze
{
  "analyzer": "smartcn",
  "text": "我爱北京天安门"
}
```

### IK

官网：

https://github.com/medcl/elasticsearch-analysis-ik

IK包含了两个Analyzer:ik_smart,ik_max_word，两个Tokenizer:ik_smart,ik_max_word。

ik_max_word: 会将文本做最细粒度的拆分，比如会将“中华人民共和国国歌”拆分为“中华人民共和国,中华人民,中华,华人,人民共和国,人民,人,民,共和国,共和,和,国国,国歌”，会穷尽各种可能的组合；

ik_smart: 会做最粗粒度的拆分，比如会将“中华人民共和国国歌”拆分为“中华人民共和国,国歌”。

### Mmseg

官网：

https://github.com/medcl/elasticsearch-analysis-mmseg

这里官网的代码有些问题，需要修改。向src/main/assemblies/plugin.xml添加：

`<id>releases</id>`

这个插件包含：

analyzers: mmseg_maxword,mmseg_complex,mmseg_simple

tokenizers: mmseg_maxword,mmseg_complex,mmseg_simple

token_filter: cut_letter_digit

### 其他

汉字/拼音转换：

https://github.com/medcl/elasticsearch-analysis-pinyin

简/繁转换：

https://github.com/medcl/elasticsearch-analysis-stconvert

### Mapping

```
PUT knowledge_lib2
{
  "mappings": {
    "question2": {
      "properties": { 
        "ShortCutCode":    { "type": "text"  }, 
        "ShortCutID":     { "type": "text" , "analyzer": "ik_smart" }, 
        "ShortCutType":      { "type": "text"}  
      }
    }
  }
}
```

参考：

http://www.cnblogs.com/eggTwo/p/4390620.html

ElasticSearch系列学习

http://blog.csdn.net/lvhong84/article/details/23936697

elasticsearch中的mapping简介

## ELK的配置部署

### Elasticsearch

配置文件：

config/elasticsearch.yml

配置文件本身修改难度不大，但部署中还是遇到好多的坑。

1.安装JDK

ES 5.X需要JDK 8才行，然而公司的服务器是JDK 7，为了不干扰已有的服务，需要修改bin/elasticsearch脚本中的JAVA_HOME变量。

同时运行两个JDK的方法还有很多种，比如下面提到的创建新用户，然后修改用户配置的的方法。

2.新建用户

ES不允许以root用户执行。因此需要创建新用户：

`adduser es`

3.

>access denied (javax.management.MBeanTrustPermission register) 

jre/lib/security/java.policy文件中新增

`permission javax.management.MBeanTrustPermission "register";`

4.以es用户的身份解压各压缩包，否则会有一大堆的权限错误。

5.Bootstrap checks failing

当配置的host不是localhost的时候，ES会进行Bootstrap checks。其主要目的是增加ES能够获得的各种资源。一般不推荐在实际生产环境中，关闭Bootstrap checks。

>max file descriptors [4096] for elasticsearch process is too low, increase to at least [65536]

修改/etc/security/limits.conf：（需要root权限）

```
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

