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

# 印度+

https://mp.weixin.qq.com/s/EktlkRgHykNgd6Oj_SVcQw

印度封杀中国App，它们的出海成绩有多让人“眼红”？

https://zhuanlan.zhihu.com/p/68046339

印度人是如何把外卖做成百年老店的？

https://zhuanlan.zhihu.com/p/257936464

印度，三天打赢一个欧洲国家（葡属果阿）

https://zhuanlan.zhihu.com/p/259753222

安达曼-尼科巴群岛

https://dy.163.com/article/EUPBGE9G053178UH.html

印度打工仔冲入美军海外基地，强奸美国女兵长达3小时

https://zhuanlan.zhihu.com/p/334430392

印度农民进军首都

https://mp.weixin.qq.com/s/Hn2cnotX7dbSJEFD3z2SMQ

微信“死于”印度

https://mp.weixin.qq.com/s/6NrLtMP2VEa1OpAK99BU4w

搞计算机网络最强的其实不是互联网公司，而是印度的电工！

https://mp.weixin.qq.com/s/n5eLb8WfUqSKHcKxaJDolg

他被称为印度“IT大王”，富可敌国，却精打细算如守财奴

https://mp.weixin.qq.com/s/P-iBxEOhDQfoGdMh9jbAFQ

印度不仅士兵冻成冰雕，高原还用上了冰冻“水晶机枪”？

https://mp.weixin.qq.com/s/513ErE8AUiB56KyhYJgo1w

最疯狂的恐怖袭击，恐怖分子在全城乱窜，见人就杀，如同人间炼狱（印度孟买恐怖袭击）

https://zhuanlan.zhihu.com/p/370607296

莫迪，输了？

https://zhuanlan.zhihu.com/p/376479497

什么是孟加拉国？

https://mp.weixin.qq.com/s/Yk6ehjvJ2TTmPE7hZBOoWQ

世界上最贵的豪宅，属于一个印度人

https://mp.weixin.qq.com/s/aC_orJwyQfl2-92_PTQMcw

虽败犹荣：令英国决心离开印度的1946孟买水兵起义

https://www.zhihu.com/answer/2342300354

为什么每次印度阅兵就要叠罗汉表演各种杂技？

https://mp.weixin.qq.com/s/4Adurm5RDMzN3GV1Bah1Ww

巴基斯坦之变：板球队长出身的网红总理被弹劾下台

https://mp.weixin.qq.com/s/4mn2RqkefkpAD3NsEhqHpQ

后疫情时代的印度真的会成为全球产业链的“救世主”吗？

https://zhuanlan.zhihu.com/p/613560508

印度乱伦为何屡禁不止？

https://zhuanlan.zhihu.com/p/619133532

切断这条走廊，印度会分裂么？
