---
layout: post
title:  在线激活流程研究, 芯片杂烩, Mysql
category: technology 
---

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

http://www.cnblogs.com/wanghetao/p/3806888.html

## 执行脚本

mysql命令行下执行：

`source a.sql`

## 导入csv文件

http://www.mysqltutorial.org/import-csv-file-mysql-table/

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

## 三种Join的区别

left join(左联接)返回包括左表中的所有记录和右表中联结字段相等的记录。

right join(右联接)返回包括右表中的所有记录和左表中联结字段相等的记录。

inner join(等值连接)只返回两个表中联结字段相等的行。

http://www.cnblogs.com/pcjim/articles/799302.html

# OpenAI Gym

Openai gym是一个用于开发和比较强化学习（reinforcement learning，RL）算法的工具包，与其他的数值计算库兼容，如tensorflow或者theano库。现在主要支持的是python语言，以后将支持其他语言。

官网：

https://gym.openai.com/

参考：

http://tech.163.com/16/0510/09/BMMOPSCR00094OE0.html


