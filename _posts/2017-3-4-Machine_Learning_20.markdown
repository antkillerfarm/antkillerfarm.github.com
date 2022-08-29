---
layout: post
title:  机器学习（二十）——关联规则挖掘（2）
category: ML 
---

* toc
{:toc}

# 关联规则挖掘（续）

## Apriori算法

Apriori算法的思路如下：

1.第一次扫描交易数据库D时，产生1-频繁集。在此基础上经过连接、修剪产生2-频繁集。以此类推，直到无法产生更高阶的频繁集为止。

2.在第k次循环中，也就是产生k-频繁集的时候，首先产生k-候选集，k-候选集中每一个项集都是对两个只有一个项不同的属于k-1频繁集的项集连接产生的。

3.k-候选集经过筛选后产生k-频繁集。

从频繁集的定义，我们可以很容易的推导出如下结论：

**如果项目集X是频繁集，那么它的非空子集都是频繁集。**

如果k-候选集中的项集Y，包含有某个k-1阶子集不属于k-1频繁集，那么Y就不可能是频繁集，应该从候选集中裁剪掉。Apriori算法就是利用了频繁集的这个性质。

参考：

http://zhan.renren.com/dmeryuyang?gid=3602888498023976650

小白学数据分析---->关联分析学习算法篇Apriori

http://blog.csdn.net/lizhengnanhua/article/details/9061755

Apriori算法详解之：一、相关概念和核心步骤

https://mp.weixin.qq.com/s/W1Bu_I3p2DO_sT2Nl0582w

Apriori算法原理总结

http://blog.csdn.net/u013250416/article/details/52701633

关联规则DHP算法详解

https://www.jianshu.com/p/1ccf4d450da0

频繁模式挖掘-DHP算法详解

## FP-growth算法

Aprori算法利用频繁集的两个特性，过滤了很多无关的集合，效率提高不少，但是我们发现Apriori算法是一个候选消除算法，每一次消除都需要扫描一次所有数据记录，造成整个算法在面临大数据集时显得无能为力。

FP-Growth算法是韩家炜等人在2000年提出的关联分析算法。它通过构造一个树结构来压缩数据记录，使得挖掘频繁项集只需要扫描两次数据记录，而且该算法不需要生成候选集合，所以效率会比较高。

>注：韩家炜，中国科学技术大学本科（1979）+中科院硕士+威斯康辛大学博士（1985）。美国伊利诺伊大学香槟分校计算机系教授，IEEE和ACM院士。

FpGrowth算法的平均效率远高于Apriori算法，但是它并不能保证高效率，它的效率依赖于数据集，当数据集中的频繁项集的没有公共项时，所有的项集都挂在根结点上，不能实现压缩存储，而且Fptree还需要其他的开销，需要存储空间更大，使用FpGrowth算法前，对数据分析一下，看是否适合用FpGrowth算法。

参考：

http://www.cnblogs.com/fengfenggirl/p/associate_fpgowth.html

数据挖掘系列（2）--关联规则FpGrowth算法

https://mp.weixin.qq.com/s/zD5hwBmMmSxzTj-3YzZKdg

频繁集挖掘FP Tree详解

https://mp.weixin.qq.com/s/ahmVB0ktJ2PG37ErO-vIiQ

FP-growth算法：高效频繁项集挖掘

## 幸存者偏差

二战期间，盟军需要对战斗机进行装甲加厚，以提高生还率，但由于军费有限，只能进行局部升级。那么问题来了，究竟哪个部位最关键，最值得把装甲加厚来抵御敌方炮火呢？人们众口不一，最后一致决定采用统计调查的方式来解决，即：仔细检查每一驾战斗机返回时受到的损伤程度，计算出飞机整体的受弹状况，然后根据大数据分析决定。

不久，统计数据很快出炉：盟军飞机普遍受弹最严重的地方是机翼，有的几乎被打成了筛子；相反，受弹最轻的地方是驾驶舱及尾部发动机，许多飞机的驾驶舱甚至连擦伤都没有。

![](/images/article/Survivorship-bias.png)

正当所有人拿着这份确凿无疑的报告准备给机翼加厚装甲时，统计学家Abraham Wald阻拦了他们，同时提出了一个完全相反的方案：加厚驾驶舱与尾部。理由非常简单：这两个位置中弹的飞机，都没有回来。换言之，它们是一份沉默的数据——“死人不会说话”。

最后，盟军高层纷纷听取了这个建议，加固了驾驶舱与尾部，果然空中战场局势得以好转，驾驶员生还率也大大提高。事实证明，这是一个无比英明的措施。

这个事例也被称作“幸存者偏差”（Survivorship bias）。它是一种典型的由于模型不当，导致的“**数据说谎**”。

>注：Abraham Wald，1902～1950，生于奥匈帝国，维也纳大学博士。1938年为躲避纳粹，移民美国，哥伦比亚大学教授。Herman Chernoff的导师。其子Robert M. Wald，为著名理论物理学家，芝加哥大学教授，黑洞理论的提出者之一。

>记者在列车上采访：这位乘客，您买到火车票了吗？   
>乘客甲：买到了！旁边这位呢？   
>乘客乙：买到了。   
>记者随机采访了十几个人，高兴地发现大家都买到了回家的火车票。

参考：

https://mp.weixin.qq.com/s/49YCWbmyoMW-0NyK_aK4Tg

大师告诉你，学习数学有什么用

https://mp.weixin.qq.com/s/5WYSbh-CBBhIy7ZnWrTFrA

从数学的角度看，为什么会有这么多渣男？

## 关联规则评价

“数据说谎”的问题很普遍。再看这样一个例子，我们分析一个购物篮数据中购买游戏光碟和购买影片光碟之间的关联关系。交易数据集共有10,000条记录，如表1所示：

| 表1 | 买游戏 | 不买游戏 | 行总计 |
|:--:|:--|:--:|:--|
| 买影片 | 4000 | 3500 | 7500 |
| 不买影片 | 2000 | 500 | 2500 |
| 列总计 | 6000 | 4000 | 10000 |

假设我们设置得最小支持度为30%，最小自信度为60%。从上面的表中，可以得到：

$$support(买游戏光碟\to 买影片光碟)=4000/10000=40\%$$

$$confidence(买游戏光碟\to 买影片光碟)=4000/6000=66\%$$

这条规则的支持度和自信度都满足要求，因此我们很兴奋，我们找到了一条强规则，于是我们建议超市把影片光碟和游戏光碟放在一起，可以提高销量。

可是我们想想，一个喜欢的玩游戏的人会有时间看影片么，这个规则是不是有问题，事实上这条规则误导了我们。在整个数据集中买影片光碟的概率p(买影片)=7500/10000=75%，而买游戏的人也买影片的概率只有66%，66%<75%恰恰说明了买游戏光碟抑制了影片光碟的购买，也就是说买了游戏光碟的人更倾向于不买影片光碟，这才是符合现实的。

从上面的例子我们看到，支持度和自信度并不总能成功滤掉那些我们不感兴趣的规则，因此我们需要一些新的评价标准，下面介绍几种评价标准：

### 相关性系数

相关性系数的英文名是Lift，这就是一个单词，而不是缩写。

$$\mathrm{lift}(X\Rightarrow Y) = \frac{ \mathrm{supp}(X \cup Y)}{ \mathrm{supp}(X) \times \mathrm{supp}(Y) }$$

$$\mathrm{lift}(X\Rightarrow Y)\begin{cases}
>1, & 正相关 \\
=1, & 独立 \\
<1, & 负相关 \\
\end{cases}$$

实际运用中，正相关和负相关都是我们需要关注的，而独立往往是我们不需要的。显然：

$$\mathrm{lift}(X\Rightarrow Y)=\mathrm{lift}(Y\Rightarrow X)$$

### 确信度

Conviction的定义如下：

$$\mathrm{conv}(X\Rightarrow Y) =\frac{ 1 - \mathrm{supp}(Y) }{ 1 - \mathrm{conf}(X\Rightarrow Y)}$$

它的值越大，表明X、Y的独立性越小。

### 卡方系数

卡方系数是与卡方分布有关的一个指标。参见：

https://en.wikipedia.org/wiki/Chi-squared_distribution

$$\chi^2 = \sum_{i=1}^n \frac{(O_i - E_i)^2}{E_i}$$

>注：上式最早是Pearson给出的。

公式中的$$O_i$$表示数据的实际值，$$E_i$$表示期望值，不理解没关系，我们看一个例子就明白了。

| 表2 | 买游戏 | 不买游戏 | 行总计 |
|:--:|:--|:--:|:--|
| 买影片 | 4000(4500) | 3500(3000) | 7500 |
| 不买影片 | 2000(1500) | 500(1000) | 2500 |
| 列总计 | 6000 | 4000 | 10000 |

表2的括号中表示的是期望值。以第1行第1列的4500为例，其计算方法为：7500×6000/10000。

经计算可得表2的卡方系数为555.6。基于置信水平和自由度$$(r-1)*(c-1)=(行数-1)*(列数-1)=1$$，查表得到自信度为(1-0.001)的值为6.63。

555.6>6.63，因此拒绝A、B独立的假设，即认为A、B是相关的，而$$E(买影片，买游戏)=4500>4000$$,因此认为A、B呈负相关。

### 全自信度

$$
\begin{array}\\
all\_confidence(A,B)=\frac{P(A\cap B)}{max\{P(A),P(B)\}}\\
=min\{P(B|A),P(A|B)\}=min\{confidence(A\to B),confidence(B\to A)\}
\end{array}
$$

### 最大自信度

$$max\_confidence(A,B)=max\{confidence(A\to B),confidence(B\to A)\}$$

### Kulc

$$kulc(A,B)=\frac{confidence(A\to B)+confidence(B\to A)}{2}$$

### cosine距离

$$
\begin{array}\\
cosine(A,B)=\frac{P(A\cap B)}{sqrt(P(A)*P(B))}=sqrt(P(A|B)*P(B|A))\\
=sqrt(confidence(A\to B)*confidence(B\to A))
\end{array}
$$

### Leverage

$$Leverage(A,B) = P(A\cap B)-P(A)P(B)$$

### 不平衡因子

imbalance ratio的定义：

$$IR(A,B)=\frac{|support(A)-support(B)|}{(support(A)+support(B)-support(A\cap B))}$$

全自信度、最大自信度、Kulc、cosine，Leverage是不受空值影响的，这在处理大数据集是优势更加明显，因为大数据中空记录更多，根据分析我们推荐使用kulc准则和不平衡因子结合的方法。

参考：

http://www.cnblogs.com/fengfenggirl/p/associate_measure.html

关联规则评价

https://mp.weixin.qq.com/s/s1Snb4XnIQk1DcK3nESilw

PrefixSpan算法原理详解

# 抗日战争++++

http://www.360doc.com/content/18/1220/07/17283749_803043906.shtml

华北平原的“皇军”哗变——馆陶事件始末

http://ah.ifeng.com/news/wangluo/detail_2015_08/06/4200178_0.shtml

通州事件：一支“伪军”对日本人的“杀戮”

https://zhuanlan.zhihu.com/p/397085288

1983年，山东一农民给领导斟茶时暴露身份，才知：他是16期黄埔生（王延周）

https://mp.weixin.qq.com/s/SKofqgEG4fTRirMPu89IMw

老百姓集体抢劫日军，日本鬼子还忍了，整个抗战时期仅此一例
