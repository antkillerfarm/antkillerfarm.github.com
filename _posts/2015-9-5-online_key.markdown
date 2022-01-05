---
layout: post
title:  在线激活流程研究, 芯片杂烩, Actor model, Restful, 微服务
category: technology 
---

* toc
{:toc}

# 在线激活流程研究

在世界范围内，软件的盗版问题都是个令程序员苦恼的问题。相应的，很多反盗版的措施也就应运而生。其中以输入序列号、激活码的产品激活策略应用最为广泛。本文就从流程的角度粗略的描述一下这个过程。之所以文章的题目没有写成“算法研究”，实在是因为我的算法太菜了。

首先当然是老规矩，回顾历史。

## 1.黑暗时代

最早采用软件注册流程的，并不是公认的被盗版大户微软。微软早期的销售策略是向PC厂商按照CPU数量收费。在linux出现之前，能在Intel x86上运行的OS基本只有MS的Dos。所以在最初阶段，厂商虽然对这种被戏称为“微软税”的收费颇有些看法，但却不得不接受。因此，Dos是没有反盗版策略的，因为相关的钱，已经由厂商支付了。这种收费模式和今天的GPS的收费模式类似，美国军方只向生产GPS芯片的公司收费，而全球只有数家公司获得技术可以生产该芯片。而这笔费用对一般消费者来说是透明的。

这种情况一直延续到90年代初，当其他的Dos替代品出现之后，微软的这种做法便有垄断之嫌。在经历了一系列的法律官司之后，其销售策略逐步过度到了今天的状况。但正是这七八年的功夫，奠定了微软的王朝霸业。

所以第一批采用软件注册流程的，是其他的商业公司或者共享软件的作者，其中尤以后者居多。因为这个时代PC还是很昂贵的奢侈品，虽然名为个人电脑，但拥有的人却并不多。很少有大公司为PC写代码。

早期的软件注册流程通常是这样的：

用户将钱交给共享软件作者时，会从他手里获得密码，然后输入密码，即可运行。在我印象中，采用这种方法最典型的是当时的一款Dos游戏——轩辕剑外转：枫之舞。游戏开始前，你需要完成一个拼图游戏，回答程序随机提问的三块拼图的颜色。而答案就在每个正版用户都会获得的一张彩色图片上。

这种方法显然是相当不安全的。如果这张图被人泄露了，那整个机制就会失去作用。事实上，当时就有同学靠强记的方法，完全背下了所有的拼图颜色。用穷举方式破解，运算量也不大。只不过它采用随机提问的方式，避免了人脑的穷举而已。

## 2.封建时代

一元密码，玩到枫之舞的地步，基本上也就黔驴技穷了。于是类似于用户名、密码之类的二元密码出现了。

二元密码从本质上来说可以表示如下：

密码P = F（用户名U）

F表示相关的算法。只有符合F算法的P和U，才能通过程序的验证。如果对U做一些限制和变换，防止破解者的明文穷举攻击，以及F足够复杂的话，这种方法的安全性还是不错的，至少在不知道匹配的P和U的情况下，穷举已经不太可行了。即使是现在，绝大多数的软件注册，仍然采用这种方法。但这种方法也有缺点，首先无法防止一个软件拷贝，在多台机器上使用。其次只要有一对合法的P和U被泄露，这个机制就被破了。

## 3.城堡时代

现在比较流行的在线注册方法，实际上是一种三元密码。典型的就是windows的在线激活方式。在这种方式下，用户获得正版软件的时候，会得到一个序列号。输入序列号之后，程序会生成能标识用户机器的特征码，这个码通常称作“注册码”。程序将序列号和注册码发送到服务器，如果是合法用户的话，会返回一个激活码，从而完成认证过程。这个过程也可以通过电话等手工方式完成。

具体的流程如下图所示：

![alt txt](/images/article/encrypt.jpg)

1)序列号的生成。序列号肯定是要包含产品信息的，通常将这种不变的东西叫做特征值。但是序列号不是一个而是一批，如何生成呢？这就需要随机数的介入了。举个最简单的例子，假设我们使用椭圆方程来创建序列号，那么就可以将特征值定为长轴a和短轴b，使用随机数作为x（当然这里的x可能存在一定的有效定义域），根据方程生成相对应的y。我们就可以将y当作序列号了。只要x符合一定的规则，不是连续的，那么生成的y也就不是连续的。换句话说，不是随便一个号都是合法的序列号。这个步骤通常是用专门的序列号生成工具完成的。

2)注册码的生成。选取能够唯一标识用户的设备硬件信息，如网卡的MAC号、硬盘编号、手机IMEI号等。通过算法F2，生成注册码。为了防止对注册码编码方式的破解，可以让序列号也参与计算，这样即使同一机器上，序列号不同，注册码也不同。

3)激活码的生成与验证。服务器端将激活码和注册码，通过算法F3，生成激活码。由于引入了服务器，我们可以很容易的知道某个序列号是否已经被使用了，从而有效的防止一号多用。程序通过算法F4，从激活码中获得特征值，如果该值与该产品的特征值一致的话。整个验证步骤就结束了。

## 4.帝王时代

道和魔的斗争永无止境。但是随着软件免费，服务收费模式的兴起。越来越多的软件开始放弃使用反盗版措施。所以或许这个帝王时代也就是故事的终点了。

# 芯片杂烩

我接触到的芯片分门别类罗列如下：

<table>
  <tr>
    <th>类别</th>
    <th>名称</th>
    <th>厂家</th>
  </tr>
  <tr>
    <td>Low MCU（追求低价）</td>
    <td>LPC4088</td>
    <td>NXP</td>
  </tr>
  <tr>
    <td rowspan="2">Hi MCU（追求性能）</td>
    <td>ASAP1826T</td>
    <td>alphascale</td>
  </tr>
  <tr>
    <td>MDM9215M</td>
    <td>Qualcomm</td>
  </tr>
  <tr>
    <td rowspan="3">Wifi Low Power SOC</td>
    <td>QCA4002</td>
    <td>Qualcomm Atheros</td>
  </tr>
  <tr>
    <td>RTL8711AF</td>
    <td>Realtek</td>
  </tr>
  <tr>
    <td>ESP8266</td>
    <td>Espressif(乐鑫)</td>
  </tr>
  <tr>
    <td>BLE SOC</td>
    <td>QN9021</td>
    <td>NXP</td>
  </tr>
  <tr>
    <td rowspan="2">Wifi SOC</td>
    <td>RTL8881AB</td>
    <td>Realtek</td>
  </tr>
  <tr>
    <td>MT7620A</td>
    <td>MTK</td>
  </tr>
  <tr>
    <td rowspan="2">Nand Flash</td>
    <td>MT29F4G08ABBEAH4</td>
    <td>Micron Technology</td>
  </tr>
  <tr>
    <td>HY27UF081G2A</td>
    <td>Hynix</td>
  </tr>
  <tr>
    <td rowspan="4">Wifi Audio</td>
    <td>RTL8871AM</td>
    <td>Realtek</td>
  </tr>
  <tr>
    <td>RT5350F</td>
    <td>Ralink</td>
  </tr>
  <tr>
    <td>AR9331</td>
    <td>Qualcomm Atheros</td>
  </tr>
  <tr>
    <td>ATV3603</td>
    <td>炬力</td>
  </tr>
  <tr>
    <td rowspan="3">Audio Codec</td>
    <td>WM8728</td>
    <td>Wolfson</td>
  </tr>
  <tr>
    <td>TAS5731M</td>
    <td>Texas Instruments</td>
  </tr>
  <tr>
    <td>MAX5556</td>
    <td>MAXIM</td>
  </tr>
</table>

# Actor model

https://elbarco.cn/2017/01/21/introduction-to-actor-model/

并发编程模型之Actor模型

https://www.jianshu.com/p/449850aa8e82

10分钟了解Actor模型

https://www.cnblogs.com/MOBIN/p/7236893.html

Actor模型原理

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

参考：

https://mp.weixin.qq.com/s/GLgWqKQfDR_EhdRdadqh_Q

4种主流的API架构风格对比（RPC、SOAP、REST、GraphQL）

## 测试工具

Web API最有名的测试工具是Postman，但是这是收费软件。开源替代主要有Hoppscotch（原名Postwoman）。

Hoppscotch官网：

https://github.com/hoppscotch/hoppscotch

TPS：Transactions Per Second，意思是每秒事务数，具体事务的定义，都是人为的，可以一个接口、多个接口、一个业务流程等等。一个事务是指事务内第一个请求发送到接收到最后一个请求的响应的过程，以此来计算使用的时间和完成的事务个数。

QPS：Queries Per Second，意思是每秒查询率，是一台服务器每秒能够响应的查询次数（数据库中的每秒执行查询sql的次数），显然，这个不够全面，不能描述增删改，所以，不建议用qps来作为系统性能指标。

https://www.cnblogs.com/uncleyong/p/11059556.html

TPS和QPS的区别

# 微服务

![](/images/img2/FaaS.png)

http://blog.csdn.net/wurenhai/article/details/37659335

微服务(Microservices)

https://mp.weixin.qq.com/s/yPl746zhMFdMVtJaPUoAxg

微服务并非Spring Cloud独角戏，Service Mesh未来大有可为

https://mp.weixin.qq.com/s/tLG7ZP2IyYweCR0ctpwk_Q

日调度5万亿次 腾讯云微服务架构体系TSF深度解读

https://mp.weixin.qq.com/s/bsuveX-E6E2fKZ24mj03nQ

微服务架构技术栈选型手册

https://mp.weixin.qq.com/s/HK9_GfLe1Q215YouTsezvQ

一张图了解微服务架构核心知识点

https://mp.weixin.qq.com/s/j0kiWTlrntzvthUJlxZBsA

数据转换：从单体式应用到微服务的低风险演变

https://mp.weixin.qq.com/s/bUtu2nTs0bybnTvk-iLt6Q

GTS来了！阿里微服务架构下的分布式事务解决方案

https://mp.weixin.qq.com/s/OMnb6sBkxo1RpmkqcgymIA

一文详解微服务架构

https://mp.weixin.qq.com/s/CPNEQpOGED1EW8ds_A6GVw

图文详解：如何给女朋友解释什么是微服务？

https://mp.weixin.qq.com/s/JlD3lmnykJyzksEWlxoy_g

怎样安全地关闭老旧的API？

# Jetty

Jetty是一个Java Servlet容器。官网：

https://www.eclipse.org/jetty/

它与Tomcat的差异在于：

1.Jetty是Eclipse基金会的项目，而Tomcat是Apache基金会的项目。

2.Jetty是轻量级的Web Server，系统开销小，而Tomcat是重量级的Web Server，功能更强大。

3.启动方式不同。Jetty一般是嵌入Java程序中，在程序启动之后，再启动Jetty。而Tomcat是首先启动自己作为后台服务，然后再加载功能性的应用。

参考：

https://github.com/jasonish/jetty-springmvc-jsp-template

一个Jetty+Spring MVC+JSP的demo

https://mp.weixin.qq.com/s/KqrEb_RSOTTVfj90Fo0rQA

Tomcat架构原理解析到架构设计借鉴
