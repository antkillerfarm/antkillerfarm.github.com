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

# 俄乌战争-

在俄罗斯一直有军事游玩项目，其内容是游客花钱使用军事装备，比如坦克步战车等，碾压一下报废车，有钱的人多花个一千来元人民币还能干她娘的一炮。不过之前提供的军事装备都是T-55，PT-76等上个世纪五六十年代的，现在都快报废的装备。但是刚刚随手一划，他奶奶地竟然更新出T-80，BMP-2这种前线都缺的装备。

---

俄驻哈萨克斯坦大使公开宣称：“哈萨克斯坦纳粹和民族主义猖獗，俄罗斯将帮助哈萨克斯坦去纳粹化。”

哈萨克知名记者阿尔曼做出回应。概括的说就是别催牛批了，你的京子跟乌萝玩了九个月了，还是这B样，敢对我们发起对线让你们一个都回不去。

https://zhuanlan.zhihu.com/p/650983914

俄罗斯亿万富翁震撼发声：俄罗斯和哈萨克斯坦是父子关系

---

西尔斯基和格拉西莫夫最大的区别就是至少旅长一级会坚定的执行命令，哈尔科夫反击时临时更变了更大胆的计划，92旅旅座可以亲自去前锋督阵。93旅旅座刚上任时下面的军官有人说怪话质疑巴赫穆特不该守，帕里沙直接把指挥部搬进巴赫穆特堵所有人的嘴。

---

俄罗斯军队似乎有按旧地图轰炸乌克兰设施的习惯。比如他们在12月31日袭击基辅市中心时肯定使用了一张旧苏联地图，轰炸了阿尔法维托酒店。因为按照1950年代的纸质地图，这家酒店是在一个老苏联导弹工厂的旁边，尽管如今根本不存在。

---

如果俄罗斯战败，俄罗斯人更有可能认为失败的原因是葡京大统领和现有民主体制过于软弱，无法调动全俄罗斯的资源，因此上台的很有可能是基督教保守主义+威权主义+反康米主义+反民主主义。

---

鹅军整了个新活儿，用冰雹火箭打宣传弹，在里面装上1000格里夫纳的假钞，然后在假钞上写上标语，劝乌克兰士兵迷途知返，不要参加反攻（没错，不是投降是不要参加反攻）。

有没有可能，给领导报方案的时候是真的。或者，装进去的时候是真的，打出来的时候就变成假的了。

---

A：悲催的乌克兰，一代人算是毁了。

B：不用在这假模假样，战争又不是他们发起的，无非又在这扮演汪精卫，感概重庆人民被轰炸的苦。

---

1984年7月12日，越军执行“北光计划”中，动员了相当于一个师的兵力大举越境入侵中国云南的文山、红河边境地区，很多村镇就离边界线不远，可当中国民众听到越南越境是什么反应，无数司机舍生忘死去帮助解放军拉送炮弹，不少司机将一卡车的财物就地抛弃，并在老司机的指引下冲向建水燕子洞弹药库协助军队拉送炮弹。

别尔哥罗德州长声称，将会和别尔哥罗德人民患难与共。然而民众在机场发现了州长夫人。
