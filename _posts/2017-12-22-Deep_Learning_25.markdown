---
layout: post
title:  深度学习（二十五）——多任务学习, DLSS
category: DL 
---

* toc
{:toc}

# 多任务学习

## 教程

http://cs330.stanford.edu/

CS 330: Deep Multi-Task and Meta Learning

## 论文

《Multi-Task Learning for Dense Prediction Tasks: A Survey》

《Progressive Layered Extraction (PLE): A Novel Multi-Task Learning (MTL) Model for Personalized Recommendations》

## 概述

![](/images/img4/MTL.png)

![](/images/img4/MTL_2.png)

![](/images/img4/Single_MTL.png)

![](/images/img4/Multi_MTL.png)

## 参考

https://mp.weixin.qq.com/s/guAgXdhZSbEAkERSB1sLRA

多任务学习-Multitask Learning概述

https://zhuanlan.zhihu.com/p/355147999

《Multitask Learning》-多任务学习发展的关键节点

https://mp.weixin.qq.com/s/A-CVKTz_moaFzTYywSt2gg

张宇 杨强：多任务学习概述

https://zhuanlan.zhihu.com/p/148132708

最新《深度多任务学习》综述论文

https://zhuanlan.zhihu.com/p/46044947

深度神经网络中多任务学习简介

https://zhuanlan.zhihu.com/p/67524006

多任务学习综述-A Survey on Multi-Task Learning

https://mp.weixin.qq.com/s/QXOy2jo4RhCZrD5bSVzBOQ

共享相关任务表征，一文读懂深度神经网络多任务学习

https://zhuanlan.zhihu.com/p/59413549

Multi-task Learning(Review)多任务学习概述

https://mp.weixin.qq.com/s/X6FwTgr282hbqgOz3oBX-w

多任务学习概述论文：从定义和方法到应用和原理分析

https://blog.csdn.net/CoderPai/article/details/80080455

多任务学习与深度学习

https://mp.weixin.qq.com/s/NpO1UP_mzyaeqW26xLY1Xg

CVPR 2018最佳论文作者亲笔解读：研究视觉任务关联性的Taskonomy

https://mp.weixin.qq.com/s/DUSa3SW1AgvJ0szL030NHQ

一个AI玩57个游戏，DeepMind离真正“万能”的AGI不远了！

https://mp.weixin.qq.com/s/P81I5vl99mV-4StNHmd_6A

作为多目标优化的多任务学习：寻找帕累托最优解

https://mp.weixin.qq.com/s/cyX_FzHF-6df9_AMUDu1aQ

One Model To Learn Them All

https://mp.weixin.qq.com/s/oY2yGrTmIpr3H2qvzYKInA

谷歌一个模型解决所有问题《One Model to Learn Them All》论文深度解读

https://mp.weixin.qq.com/s/VMHyEOn8uyRYt39QXVWUww

西湖大学张岳：自然语言处理中的多任务联合学习（384页PPT）

https://mp.weixin.qq.com/s/5zo-2WB9v2hOvKAC7ZhKuQ

基于学习的多任务框架L2MT，为多任务问题选择最优模型

https://mp.weixin.qq.com/s/MPhKUosKZbLtVjJ1XYGXYA

如何利用深度学习模型实现多任务学习？这里有三点经验

https://mp.weixin.qq.com/s/Oopgglg2G7TwnXeN2DtZhA

多任务+注意力机制的学习

https://mp.weixin.qq.com/s/vMgHCQ03Gt5v6GdgW-pY9A

一个神经网络实现4大图像任务，GitHub已开源

https://mp.weixin.qq.com/s/Zui8FFn1FDP_UoAGMH0L7Q

知其然，知其所以然：基于多任务学习的可解释推荐系统

https://mp.weixin.qq.com/s/8Ablt7Sa86DXTB1dE_RqaA

港中文开源基于PyTorch的多任务人脸识别框架

https://www.zhihu.com/question/345173757

多任务学习成功的原因是引入了别的数据库还是多任务框架本身呢？

https://zhuanlan.zhihu.com/p/52566508

深度神经网络中的多任务学习汇总

https://mp.weixin.qq.com/s/DUsXj46TCZ9w_Dxgw4ZH8g

用于多任务CNN的随机滤波分组，性能超现有基准方法

https://mp.weixin.qq.com/s/-SHLp26oGDDp9HG-23cetg

多任务学习在推荐算法中的应用

https://mp.weixin.qq.com/s/avWy3-mju0d0QjufX_bi4Q

Multi-task Learning的三个实用小知识

https://mp.weixin.qq.com/s/hLJB8yH8V0Ncug77jHU1Bw

模型独立学习：多任务学习与迁移学习

https://zhuanlan.zhihu.com/p/138597214

Multi-task Learning and Beyond: 过去，现在与未来

https://mp.weixin.qq.com/s/gIEJoE9B8mmNx6u6bqLspQ

最新《深度多任务学习》综述论文，22页pdf

https://mp.weixin.qq.com/s/ifTNRW0W7-P_LyfNldtavQ

多任务学习方法在推荐中的演变

https://zhuanlan.zhihu.com/p/269492239

多任务学习优化（Optimization in Multi-task learning）

https://www.cnblogs.com/RyanXing/p/10730829.html

Cross-stitch Networks for Multi-task Learning

https://mp.weixin.qq.com/s/T_UsCkj9hnV7tgOTcaf6Wg

Multi-Task多任务学习， 那些你不知道的事

https://mp.weixin.qq.com/s/vFHYk7202bSZ214Za_SOtQ

2021年浅谈多任务学习

https://mp.weixin.qq.com/s/pvQHUXo83hMfH8kCumXSQA

如何利用多任务学习提升模型性能？

https://mp.weixin.qq.com/s/dcv2pqccrAtg2nNaHTTU2Q

一文"看透"多任务学习

# DLSS

DLSS的全称是Deep Learning Super Sampling，主要作用是通过Tensor Core硬件加速的深度学习对实时渲染的图片实现非常高质量的超采样，从而大幅提升游戏渲染的性能。

DLSS 1.x，刚开始出来这个东西，都是英伟达针对某个游戏进行智能训练，训练的成果文件，放在驱动里面了，采样原生的图画，然后生产新的画面，插入到游戏中，弊端很明显，游戏支持率很低，通过训练插入的画质（只有原生画质的50%渲染率，画质差不少）确实不咋地；

DLSS 2.0时代，NV觉得针对特定游戏训练不太好，这次换成了固定训练模式，让游戏厂家自己来适配，支持DLSS的游戏多了不少；

DLSS 1～2本质上是超分辨率技术（Super Resolution）。

DLSS 3本质上是插帧技术（Frame Generation）。现实中，大多数玩家对分辨率更敏感，对帧率没那么敏感的。

DLSS 3.5本质上是光线重构技术（Ray Reconstruction），只有在光追的时候才能开启。

以上技术名称都叫DLSS，本质上不是同一项技术，甚至是相互独立的。唯一共同点就是都和深度学习相关。

https://zhuanlan.zhihu.com/p/123642175

DLSS 2.0-基于深度学习的实时渲染图像重建

https://www.zhihu.com/question/618638060

如何评价英伟达推出的DLSS3.5?

---

FSR的全称是FidelityFX Super Resolution。

AMD的FSR其实和nvida的NIS（图像锐化）原理一样，都是没有用到AI的，属于机械式的脑补画面。不过它们是免费开源，跨平台的，所以不管是A卡，N卡还是i卡都能支持这项技术。

Intel的XeSS全称Xe Super Sampling，这是英特尔的深度学习超采样增强技术，它的原理和DLSS类似，都需要利用AI。

TSR（Temporal Super Resolution）时序超级分辨率技术是AMD与Epic联合开发的超采样技术，将会内置在未来的虚幻引擎中用于取代传统的TAA（Temporal Antialiasing）。原理类似英伟达的DLSS。

AMD一直在视觉计算的先进技术方面承受巨大压力，尤其是游戏应用，DLSS 3出来后AMD基本上在新的AAA游戏这边被压制完了，帧生成到现在（2023.8）都没搞定，现在新的DLSS 3.5，如果在RR光追降噪器方面进一步一统江湖，AMD真的在游戏光线追踪方面没得玩了。

---

TAA这一类算法利用了渲染图片的时域连贯性（Temporal coherency），既渲染结果的帧与帧之间大体是连续的，发生高频变化的概率不高。也就是说，我们可以假设需要渲染的场景在上一帧和当前帧几乎一样。

然而天下哪有这等好事，实时渲染或者游戏的图片序列中几乎每一帧都有或多或少的变化，从角色动画，到动态光影，到粒子特效。直接复用过去帧的样本来重建当前帧的图片会使重建的结果中产生很大的错误。

单帧的算法结果会模糊并且会和原生渲染不一致，而且帧间也经常会有抖动。

多帧的算法则需要用各种启发式的方法去解决各种过去帧和当前帧场景样本不一致的问题。

# 棋坛纵横=

柯洁直播时候说过，我一夺冠，比赛就不办了。

1.理光杯在2000年开始举办，理光（中国）投资有限公司主办，北京金华铭办公设备有限公司承办，而且影帝葛优每年都会出席颁奖礼，但在2015年柯洁夺得冠军后，就停办了。

2.利民杯世界围棋星锐最强战由中国围棋协会、中国棋院杭州分院主办，杭州市围棋协会承办，一共办了四届，17年柯洁夺冠后就不办了。

3.百灵杯是贵州百灵集团赞助的世界围棋赛事。从2012年开始到2018年办了四届，柯洁夺冠两次，最后一次柯洁夺冠后不办了。

4.新奥杯世界围棋公开赛，中国棋院，河北省体育局和廊坊市人民政府主办，由新奥集团冠名赞助，就16年办了一届，柯洁夺冠。

5.CCTV贺岁杯围棋赛，是由中国中央电视台举办的一项围棋赛，于春节期间进行，始于2013年。柯洁在16,17年夺冠两次，2020年第八届后停办。

柯洁巅峰期在15年～20年，恰好赶上了围棋比赛最为密集的时期，刷了不少世界冠军。可见柯洁的成功，当然要靠自我奋斗，但是也要考虑到历史的进程。如今经历新冠后的围棋就如同末法时代，而小申却能在这样的末法时代站在巅峰，如今已经拿下七冠，差一冠就赶上柯洁。但柯洁自从21年五月以来就再也没赢过小申，如今已经从七擒孟获，到八败王，再到九居人下，直到前几天的十败的man。

---

王天一终身禁赛+蹲监狱，郑惟桐等排行前几的几个人也终身禁赛（未来不排除蹲监狱的可能），排行靠前的特大基本都是禁赛四年甚至七年以上，排行中上游的大师们也禁赛了一大批，连看上去义正辞严的举报人党大师都一屁股烂事，被禁赛了3年。里面不仅有很多90~00年的中坚选手，还有68年的张申宏、81年的孙勇征、83年的苗利明、85年的郝继超这种知名度很高且早已过巅峰的老选手，以及两个未成年选手……

---

在规则浩繁的第1届本因坊战中，关山利一成为时代宠儿，力压吴清源、木谷实、桥本宇太郎等天之骄子，在四次淘汰赛中累计积分第一，并在六番棋决赛与年长他十八岁的老前辈加藤信战平，根据预赛排名优势直接夺冠，从而于1941年荣登史上第一位“实力制本因坊”，改名“利仙”，地位之高后人难以想象。

https://zhuanlan.zhihu.com/p/20954050444

史上第一家五代职业棋手 延续超过百年的围棋家族 关山利仙后继有人

---

https://zhuanlan.zhihu.com/p/148447349

提子围棋（Atari-Go）和不围棋（NoGo）的计算复杂度
