---
layout: post
title:  机器学习（四十）——模型驱动 vs 数据驱动, 特征工程, 维度灾难, Metaverse
category: ML 
---

* toc
{:toc}

# 模型驱动 vs 数据驱动

最近阅读了这篇文章，深有感慨：

https://mp.weixin.qq.com/s/N7DE0kvf8THhJQwroHj4vA

成不了AI高手？因为你根本不懂数据！听听这位老教授多年心血练就的最实用统计学

>注：吴喜之教授是我国著名的统计学家，退休前在中国人民大学统计学院任统计学教授。吴教授上世纪六十年代就读于北京大学数学力学系，八十年代出国深造，在美国北卡罗来纳大学获得统计学博士学位，是改革开放之后第一批留美并获得统计学博士学位的中国学者。多年来吴教授在国内外数十所高校讲授统计学课程，在国内统计学界享有盛誉。其知名的学生有李舰和刘思喆。

>李舰，从2003年开始，一直把R当作随身武器奋战在统计学和数据分析的第一线，是Rweibo、Rwordseg、tmcn等高质量R包的作者，在业界积累了大量的经验，目前供职于Mango Solutions（中国），任数据总监。

>刘思喆，2012至2016年就职于京东商城，推荐系统平台部高级经理，主要负责和推荐系统离线、在线相关的用户行为、商品特征的建模，以及数据监控平台。因工作业绩，在《京东技术解密》一书中获“数据达人”称号。

# 特征工程

目前的大多数机器学习任务，通常假设训练数据与测试数据共享一个特征空间。然而在实际场景中，训练好的模型通常需要与一个开放环境进行交互，测试集中就会出现新的特征。例如推荐系统中利用用户的年龄、职业等特征训练好了一个推荐模型，后来公司新发布了某个应用，收集到了新的用户数据，这就需要用新的用户特征进行决策。这就是所谓的**特征外推**。

https://mp.weixin.qq.com/s/3c_IYocu3mEIALgYlV6vtw

神经网络如何特征外推？上海交大NeurIPS21—面向开放环境特征外推的图学习解决方案

---

https://mp.weixin.qq.com/s/ibiElLIgrT3wYx3tDYMMTw

理解特征工程

https://www.zhihu.com/question/29316149

特征工程到底是什么？

https://mp.weixin.qq.com/s/3Ce8uMf_Kyt-hEZUYfdh3g

特征工程之特征选择

https://mp.weixin.qq.com/s/tOcyfK68jW7Tr-PGCvdXMA

特征工程最后一个要点:特征预处理

https://mp.weixin.qq.com/s/GWMZ1jwbchE8O0r6EduYtQ

一文讲解特征工程！经典外文PPT及中文解析

https://mp.weixin.qq.com/s/c9iHdgtErVd_iitwny7_zw

Kaggle前1%参赛者经验：特征工程为何如此重要？

https://mp.weixin.qq.com/s/xbPJD0uoRB-T1x09AUYdzg

基于Python的自动特征工程——教你如何自动创建机器学习特征

https://mp.weixin.qq.com/s?__biz=MjM5MTQzNzU2NA==&mid=2651664000&idx=1&sn=ae6dda80df6d6278ae33b7bf7fbadcd2

深度特征合成：自动化特征工程的运作机制

https://mp.weixin.qq.com/s/R1MhoCfnd5drvg2CGLVsPw

哪种特征分析法适合你的任务？Ian Goodfellow提出显著性映射的可用性测试

https://mp.weixin.qq.com/s/XSovbUDVTKe59DDaC1Kl8Q

如何进行特征表达，你知道吗？

https://mp.weixin.qq.com/s/vhr5gXoa0S4-QqFcK7uz-w

模型吞噬特征工程

https://mp.weixin.qq.com/s/zgKbG3r_B8d1qQHnrD2NCg

特征工程宝典《Feature Engineering for Machine Learning》翻译及代码实现

https://mp.weixin.qq.com/s/3Clq9ECs6M52Sg-_xMxJGw

最核心的特征工程方法-分箱算法

https://mp.weixin.qq.com/s/ghfh1x_lsEcoA8PFPXE46w

练手扎实基本功必备：非结构文本特征提取方法

https://mp.weixin.qq.com/s?__biz=MzU2OTA0NzE2NA==&mid=2247515402&idx=1&sn=ee3cd5c64a707246216a532fa3af422b

面向机器学习和数据分析的特征工程

https://mp.weixin.qq.com/s/NKKk8nRd0qn5XhxXgYWknw

手把手带你入门和实践特征工程的万字笔记

https://mp.weixin.qq.com/s/QZeyEN2DDM_etEki7uodMg

一个神奇的特征选择轮子---MLFeatureSelection

https://mp.weixin.qq.com/s/8NI-NayCg_gZmJ6-1FZ_DA

一个Python特征选择工具，助力实现高效机器学习

https://mp.weixin.qq.com/s/LbXHpnC19euqriCtSHeg1Q

UC Berkeley提出特征选择新方法：条件协方差最小化

https://mp.weixin.qq.com/s/V3w5Iu804O6PmnBjmwCbgw

常用文本特征选择

https://mp.weixin.qq.com/s/Rj-ObD-eM5zEfs5fkWamGQ

三大特征选择策略，有效提升你的机器学习水准

https://mp.weixin.qq.com/s/rNipJC5wljzCT6Aq5gvvqw

一款功能强大的特征选择工具（FeatureSelector）

https://mp.weixin.qq.com/s/Bu34hPN0XAj6GmLXuQwVsQ

风控特征—关系网络特征工程入门实践

https://mp.weixin.qq.com/s/thd_dtd4erqSf7p6ZON72w

自动特征工程在推荐系统中的研究

https://zhuanlan.zhihu.com/p/96420594

特征工程架构性好文

https://mp.weixin.qq.com/s/demEVr5ZXKeSLbBIO1XgsQ

AutoFIS: 因数分解模型中用于预测点击率的自动特征交互选择

https://mp.weixin.qq.com/s/Z5cs6X1tFq9uKGfo3aHgmw

简介机器学习中的特征工程

https://mp.weixin.qq.com/s/BNiDjgBpdGQjCY-b96htlQ

机器学习中的特征工程总结

https://mp.weixin.qq.com/s/VBA02WHBJmU77RPLtIzprA

特征工程入门：应该保留和去掉那些特征

https://mp.weixin.qq.com/s/BfZ9BQXtOsEXCkAR3QYHhA

特征工程了解一下

https://mp.weixin.qq.com/s/dPnb7Mho-sQA6euvCdQV7w

类别特征目标编码

https://mp.weixin.qq.com/s/ZJjQY5g95p_s2Te9Rl2zIA

特征选择介绍及4种基于过滤器的方法来选择相关特征

https://mp.weixin.qq.com/s/q635XCJ3tVfesmTF4yvJ_w

categorical feature编码方法小结

https://mp.weixin.qq.com/s/vK5HP7e8d7ZXGFxaYN3G4g

我用特征工程+LR超过了xDeepFM

https://mp.weixin.qq.com/s/DdvXivS7OnAwC59nt5i3bg

天池项目总结，特征工程了解一下！

https://mp.weixin.qq.com/s/ktk8eUnu4-TyU3ob2y1fCA

特征交互新路线：阿里Co-action Network论文解读

https://mp.weixin.qq.com/s/FS7WJ1rG8Kt5Xp6H1InCAg

如何融合深度学习特征向量？

https://mp.weixin.qq.com/s/PAPHQ_Dq7ZqWvuSZQzuWag

样本组织篇

https://mp.weixin.qq.com/s/B0-VSkPhkDJkwpllHahJiQ

Null Importances

https://mp.weixin.qq.com/s/OESIXwjM8nFhz4NhNT1BzQ

使用神经网络的自动化特征工程

https://mp.weixin.qq.com/s/SARm2GlHZHYKAFFRr9buXw

Kaggle所有图像特征汇总

https://mp.weixin.qq.com/s/YFx8E3piOLmdEfpjecRqxw

特征工程方法总结

# 维度灾难

低维空间中习以为常的事情，可能在高维空间中被颠覆。

![](/images/img4/HD.png)

当维度升高时，内接球的体积占比越来越小。

![](/images/img4/HD_2.png)

当维度升高时，绝大部分体积集中在球壳上。

https://mp.weixin.qq.com/s/c8P9KmkQTqNcazcjU9qQFw

机器学习中的维度灾难

https://mp.weixin.qq.com/s/iWIjwThUiVc1ifvf5-cf7w

什么是维度灾难？

# Metaverse

“元宇宙”这一概念源于美国作家 Neal Stephenson 在1992年出版的科幻小说《雪崩》（Snow Crash）。“ meta ” 意为“超越”“元”，与“ Universe ”（宇宙）相结合，即“元宇宙”。简单来说，元宇宙是一个可以映射现实世界、又独立于现实世界的虚拟空间。然而，关于元宇宙的最令人兴奋的不只是技术层面上的构建，更是改变彼此现有社交方式的巨大潜力。

https://mp.weixin.qq.com/s/KmODAixbwztg7A_jiNrm9g

《元宇宙Metaverse》报告，53页ppt

https://mp.weixin.qq.com/s/QzDKFinw9sXXfKNbG0Rchw

清华大学：2021元宇宙研究报告

# 俄乌战争=

在俄罗斯一直有军事游玩项目，其内容是游客花钱使用军事装备，比如坦克步战车等，碾压一下报废车，有钱的人多花个一千来元人民币还能干她娘的一炮。不过之前提供的军事装备都是T-55，PT-76等上个世纪五六十年代的，现在都快报废的装备。但是刚刚随手一划，他奶奶地竟然更新出T-80，BMP-2这种前线都缺的装备。

---

2014年7月，瑞典新纳粹分子、前本土防卫军狙击手米尔凯·斯基尔特加入“亚速”营。但他在2015年8月公开宣布：他在乌克兰的经历改变了自己，自己将不再相信纳粹主义，因为他在乌克兰的所见所闻让自己认识到——自己过去的纳粹思想是错误而愚蠢的。

https://mp.weixin.qq.com/s/wp8CBSc2cBJBddSws2ktzw

乌克兰正在进一步走向分裂！这次不在东部，而是西部。

---

126独立旅是去年春天在赫尔松方向损失最严重的俄军部队，虽然俄罗斯官方没有公布任何具体损失的消息，但去年3月份该旅被授予“近卫”称号——这是克里姆林宫安抚伤亡惨重部队的标准流程。

---

芬兰模式：相当于直接承认割让领土。

https://zhuanlan.zhihu.com/p/619510199

刚刚，芬兰进去了！

---

乌克兰半年时间里已经打出了9584枚GMLRS火箭弹，这意味着其日均射击量达到约50枚；而现在的7日平均射击量只有13枚，储备278枚，猜测美军的GMLRS储量也有点吃不消了（战前一共生产了5万枚，不知道战前美国储备多少）。不过从长期来看，洛马在今年表示目前GMLRS产能是一年一万，并且可以提高到1.2-1.4万，看起来勉强够乌克兰使用（只要节约）。至于炮弹，乌克兰已经用掉了95万枚美国155炮弹，大概是美国战前储备的1/2，而且还在以每天3400枚的速度消耗。乌克兰的炮弹储备仅有10900枚，只够打4天！这意味着一旦美欧对乌克兰弹药断供，就会迅速产生致命的影响。

苏联解体后乌克兰没有防空导弹生产能力（相关的厂家都在俄罗斯），2014年之后乌克兰自然也得不到任何防空导弹补充。于是在战争进行一年之后，曾发挥出重大作用的乌克兰地空导弹系统的弹药即将打光。如果情况继续下去，到2023年5月乌克兰的地面防空系统将基本无法运作，这将给已经装死一年的俄罗斯空天军（VKS）以很大机会。

https://zhuanlan.zhihu.com/p/620436462

2023年2月美军泄密资料简要判读分析评论
