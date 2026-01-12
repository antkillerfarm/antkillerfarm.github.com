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

>吴喜之教授是我国著名的统计学家，退休前在中国人民大学统计学院任统计学教授。吴教授上世纪六十年代就读于北京大学数学力学系，八十年代出国深造，在美国北卡罗来纳大学获得统计学博士学位，是改革开放之后第一批留美并获得统计学博士学位的中国学者。多年来吴教授在国内外数十所高校讲授统计学课程，在国内统计学界享有盛誉。其知名的学生有李舰和刘思喆。

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

# 隋唐+

李隆基有一次看波斯回旋舞看开心了，宠幸了一个波斯舞娘，结果一炮就怀了。生下一个早产女婴。这就很尴尬了，本来就被五望七姓diss血统不够高贵，现在还搞了个混血那就是丑闻了。于是李隆基把她养在道堂，叫她虫娘，她妈就什么封赏都没有。

安史之乱后，李隆基被太子尊太上皇架空了，最后几年晚景凄凉，但幸好有这个不受待见的女儿在身旁照顾，他最后良心发现，吩咐孙子唐代宗替他帮姑姑找个好人家嫁了。最后虫娘按公主（寿安公主）身份嫁出去了，也算一个好结局。

---

苏定方斩首10万火烧布达拉宫这段，我还真看过一眼，后面写着西藏某僧人出马，把苏打得屁滚尿流回家了。两件事出自同一本书，苏粉一边吹苏火烧布达拉，一边吹一生无败，问题火烧布达拉后面就是被西藏神僧召唤金人给教育了。。。

---

https://www.zhihu.com/answer/1910458646606881792

古代被屠城后的城市是如何恢复重建的?（张全义重建洛阳城）

---

姚崇儿子用重宝诱惑请老爹死敌张说写墓志铭，张说看在钱的份上欣然提笔，称赞姚崇“位为帝之四辅，才为国之六翮，方为代之轨物，行为人之师表”。等张说回过味来，墓志铭已经被玄宗审查通过了。

# 国际共运+

你要是直接说人民是伟大的，所以一切都应该由人民做主；

放心吧，人民史观的支持者们能找出一万种理由，证明为什么人民群众都是白痴，领导干部才是国家的希望，听人民群众的意见是万万不行的。

---

人民史观是让你用人民群众的视角看待历史，不是让你跪着祈祷“人民必胜”，更不是让你跪在鞑子面前高呼“王爷代表人民的根本利益”。

如果你不承认“人民经常会输”这一事实，你所谓的“人民史观”必然会走向“谁代表人民谁赢”和“谁赢谁代表人民”两条邪路。

PS：这两种说法确实有合理性，但它们尚不能称之为人民史观，顶多算“二十一世纪天命论”。

---

国内的自由主义思潮最早出现在哪里？社科院马研所。

除此之外，社科院政治所、历史所、哲学所、经济所都是重灾区，当时社会上甚至有一句笑话，叫做“社科院是业余反马列的，马研所是专业反马列的，是反对马列主义、XXX思想的第一所”；包括马研所的所长在内，这批人最后多数都逃到了海外。

马恩的理论其实是从启蒙运动时期的古典自由主义发展而来的，马克思和恩格斯其实都是自由主义者，否则他们也不会讲自己追求的社会形态述成“从必然王国向自由王国的飞跃”。

00年代之后，各大高校都认可马院不用进行任何研究，课也不用正经上，甚至论文都不需要查重，跟和尚念经一样就行，唯一的禁忌就是研究马恩，或者说是偷着研究可以，别让大家知道就行。
