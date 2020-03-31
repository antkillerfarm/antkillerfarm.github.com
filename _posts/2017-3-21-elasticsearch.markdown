---
layout: post
title:  Elasticsearch（一）
category: AI 
---

# Elasticsearch

## 概况

Elasticsearch（简称ES）最早是由Shay Banon于2010年发起的开源项目，目前已经成立了公司专门运作该项目。

官网：

https://www.elastic.co

发展到今天（2017.3），Elasticsearch已经由单一的软件，发展成为Elastic Stack。其中包括：

核心组件：ElasticSearch, Logstash, Kibana，即所谓的ELK。

扩展组件：Beats, X-Pack, Cloud

http://solr-vs-elasticsearch.com/

ElasticSearch和Solr的对比

## 简易安装步骤

为了避免版本混乱，目前Elastic Stack将各组件的版本号统一为一个。使用时，各组件版本号应该一致。

>Elastic Stack的版本号为X.Y.Z的形式，例如5.2.3。其中，各组件的Z可以不相同，但X、Y必须一致。

### Elasticsearch

#### 运行

```bash
cd ES_HOME
bin/elasticsearch
```

#### 测试

`curl -X GET http://localhost:9200/_cat/health?v`

### Logstash

#### 运行

```bash
cd LS_HOME
bin/logstash -f logstash.conf
```

LS_HOME/logstash.conf：

```bash
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

```bash
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

```bash
input { stdin { } }
output {
  elasticsearch { hosts => ["localhost:9200"] index => "msg" document_type => "true_msg"}
  stdout { codec => rubydebug }
}
```

其查询语句为：

```bash
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

<table>
  <tr>
    <th width="15%">名称</th>
    <th width="30%">命令</th>
    <th width="55%">说明</th>
  </tr>
  <tr>
    <td>指定ID添加数据</td>
    <td>
    <pre class="highlight"><code>PUT /customer/external/1
{
  "name": "John Doe"
}</code></pre>
    </td>
    <td></td>
  </tr>
  <tr>
    <td>自动ID添加数据</td>
    <td>
    <pre class="highlight"><code>POST /customer/external/
{
  "name": "John Doe"
}</code></pre>

    </td>
    <td></td>
  </tr>
  <tr>
    <td>删除数据</td>
    <td>
    <pre class="highlight"><code>DELETE /customer</code></pre>
    </td>
    <td></td>
  </tr>
  <tr>
    <td>查询删除</td>
    <td>
    <pre class="highlight"><code>GET msg/true_msg/_delete_by_query
{
  "query": {
    "match_all": {}
  }
}</code></pre>
    </td>
    <td></td>
  </tr>
    <tr>
    <td>简单查询</td>
    <td>
    <pre class="highlight"><code>GET msg/true_msg/_search
{
  "from" : 0,
  "size" : 20,
  "query": {
    "match": { "message": "购物车" }
  }
}</code></pre>
    </td>
    <td>本查询有个问题，只要出现“购物车”三个字中任何一个字的message，都会被选中。同时展示了from和size的用法。更多内容参见：http://www.w2bc.com/article/109597</td>
  </tr>
  <tr>
    <td>全字匹配查询</td>
    <td>
    <pre class="highlight"><code>GET msg/true_msg/_search
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
}</code></pre>
    </td>
    <td>本查询同时展示了highlight的用法</td>
  </tr>
  <tr>
    <td>统计词频</td>
    <td>
    <pre class="highlight"><code>GET twitter/tweet/5/_termvectors
{
  "fields" : ["text"],
  "offsets" : true,
  "payloads" : true,
  "positions" : true,
  "term_statistics" : true,
  "field_statistics" : true
}</code></pre>
    </td>
    <td>统计结果包括Field统计和Term统计两类。<br/>
<strong>Field统计：</strong><br/>
doc_count：doc的数量，这里的doc通常对应Logstash导入的一条记录。<br/>
sum_ttf：所有doc中包含的词的总和。<br/>
sum_doc_freq：所有doc中包含的独立词的总和。<br/>
例如如下语句：<br/>
twitter test test test<br/>
ttf统计中认为这是4个词，而doc_freq中认为这是2个词，因为test重复出现了3次，这里只能算作1个词。注意这里的独立词是针对doc而言的，在不同doc中出现的test被认为是2个词。<br/>
<strong>Term统计：</strong><br/>
term_freq：该词在本doc中出现的次数。<br/>
ttf：该词在所有doc中出现的次数。<br/>
doc_freq：该词在所有doc中独立出现的次数。<br/>
可见_termvectors的查询虽然针对的是某个文件，但返回的结果中，却包含了全局的统计数据。<br/>
<strong>注：</strong>这里的全局，严格来说只到分片一级。当index比较大的时候，往往数据会保存到多个分片中。这样的话，不同doc所对应的全局统计结果，往往会有所不同。可以通过设置，实现真正的全局统计，但一般无此必要。</td>
  </tr>
  <tr>
    <td>查询mapping</td>
    <td>
    <pre class="highlight"><code>GET /index/_mapping/type</code></pre>
    </td>
    <td></td>
  </tr>
  <tr>
    <td>查询index信息</td>
    <td>
    <pre class="highlight"><code>GET /_cat/indices?v</code></pre>
    </td>
    <td>结果一般有Green、Yellow和Red三种系统状态。Green和Red比较好理解，Yellow一般是指服务可用，但没有备份，这在单机版的ES中出现的比较多。</td>
  </tr>
  <tr>
    <td>查询插件信息</td>
    <td>
    <pre class="highlight"><code>GET /_cat/plugins?v</code></pre>
    </td>
    <td></td>
  </tr>
  <tr>
    <td>Mapping</td>
    <td>
    <pre class="highlight"><code>PUT knowledge_lib2
{
  "mappings": {
    "question2": {
      "properties": { 
        "ShortCutCode":{ "type": "text"  }, 
        "ShortCutID":{ "type": "text" ,
	               "analyzer": "ik_smart" }, 
        "ShortCutType":{ "type": "text"}  
      }
    }
  }
}</code></pre>
    </td>
    <td>参考：<br/>
http://www.cnblogs.com/eggTwo/p/4390620.html<br/>
ElasticSearch系列学习<br/>
http://blog.csdn.net/lvhong84/article/details/23936697<br/>
elasticsearch中的mapping简介</td>
  </tr>
  <tr>
    <td>查询数量</td>
    <td>
    <pre class="highlight"><code>GET twitter/tweet/_count</code></pre>
    </td>
    <td></td>
  </tr>
</table>

## ES中文分词

ES的中文分词功能，以插件的形式提供。主要包括官方维护的Smartcn和Medcl维护的IK、Mmseg。

### Smartcn

安装方法：

`sudo bin/elasticsearch-plugin install analysis-smartcn`

这个例子同时展示了ES在线安装插件的方法。插件安装成功之后，需要重启ES，使之生效。

验证：

```bash
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

## ES Cluster

![](/images/article/es_cluster.png)

上图是ES Cluster的示意图。其中：

**Node**：集群中的计算机。

**Shard**：每个Index对应一个Shard。

**Replica**：一个Shard可以有N个Replica。这样的话，一个Index在集群中，就有1+N份数据了。

集群会根据Node的增减，自动调整Shard和Replica的分布。换句话说，只有超过1+N个Node同时掉线，才会导致某个Index的掉线。

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
