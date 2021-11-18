---
layout: post
title:  机器学习（三十九）——运筹学, 异常检测, 因果推理
category: ML 
---

* toc
{:toc}

# 运筹学

https://zhuanlan.zhihu.com/p/25579864

人工智能的“引擎”--运筹学，一门建模、优化、决策的科学

https://mp.weixin.qq.com/s/Fn7FtrbCVDlYEXCzMUkTBg

运筹学发展简史之1979-2004

http://www.cnblogs.com/6DAN_HUST/archive/2010/11/11/1874681.html

运筹学——线性规划及单纯形法求解

https://mp.weixin.qq.com/s/2qOjE0B6x9xuyoBw94NqQA

线性规划基础

https://mp.weixin.qq.com/s/AHM60k3_Th3HD-VPBaXhuw

线性规划和整数规划的若干建模技巧

https://mp.weixin.qq.com/s/f24aXWrQkMlzABVatjOf5Q

线性规划的历史、模型及案例

https://mp.weixin.qq.com/s/zV6zi79c1Q2dfCaywuK6Pw

内点法六十年再回首

https://mp.weixin.qq.com/s/aryMyP0r6vov0pUvkoFYng

过去，现在和未来：运筹学为航空业保驾护航

https://mp.weixin.qq.com/s?__biz=MzUxMTYwMzI0OQ==&mid=2247486937&idx=1&sn=6be69679390f59516ee5f077adb8ccfa

运筹优化的剖析与应用

https://mp.weixin.qq.com/s/Ofn-p8NnnO4kLYqTlL0bJg

二次规划（QP）样条路径

https://mp.weixin.qq.com/s/zqesjLOeWIkiq68tLVVmJQ

混合整数规划模型在页岩气开采中的应用：EQT公司的案例

https://mp.weixin.qq.com/s/Ve_Gvp1Y0nIX4DV9_w0S3g

半正定规划(SDP)的形象理解和基本原理

https://mp.weixin.qq.com/s/wspfngdFNq-GeCTUJAse0A

在单纯形法之前

https://mp.weixin.qq.com/s/sGRNiAl7tvPc1tl0Xk2Idw

深度学习和强化学习在组合优化方面有哪些应用？

https://mp.weixin.qq.com/s/MuODCRWqolnh62D4t-hRvQ

临(邻)近增量累积梯度法(PIAG)

https://mp.weixin.qq.com/s/Pvu7JAHvsGJdL0mJIhVwng

多工序、多机台(产线)环境下的排程要点

https://mp.weixin.qq.com/s/oMiNGJOCqjEHoTmfBQv6EA

使用神经网络为A*搜索算法赋能：以个性化路径推荐为例

https://mp.weixin.qq.com/s/D_CQIw1372RTYBeSqPEl9A

鲁棒优化基础

https://mp.weixin.qq.com/s/7x2LS94i7te7JAeWo-d5kA

什么样的整数规划模型是一个好的模型

https://mp.weixin.qq.com/s/hzpnHvqaP9sxQQlLq-_3wg

Robust Optimization in 3D Vision

https://mp.weixin.qq.com/s/T20YMbTBbn1GC3fvInhkkA

深度学习如何影响运筹学？

https://mp.weixin.qq.com/s/VcA1SZNS4LvMny_9dCUByQ

混合整数规划/离散优化的精确算法--分支定界法及优化求解器

https://mp.weixin.qq.com/s/Ol69W-T67eGJW6RDjQJQ6w

胡武华博士：运筹优化理论在物流行业中的应用实践

https://mp.weixin.qq.com/s/cd08Lk4LcxV-qAVLq7-Kjg

整数规划经典方法--割平面法（Cutting Plane Method）

https://mp.weixin.qq.com/s/xhfRPedQ6pLQYrVsK2rgLA

浅谈排队论

https://mp.weixin.qq.com/s/pGvGbtoj96h0DQxKEDXWbg

利用顾客灵活性优化共享汽车规模

https://mp.weixin.qq.com/s/Hx9aTo8hMr-vY9418oWopQ

浅谈旅行商问题（TSP）的启发式算法

https://mp.weixin.qq.com/s/KmPTljxY4eyoYV8hXLxIXg

浅谈滴滴派单算法

https://mp.weixin.qq.com/s/H1diSCPZe6oTouufPY1JkA

机组排班优化及悠桦林的实践

https://mp.weixin.qq.com/s/_bEx-psbGgE4TiyBRuamQQ

二维异形零件下料问题简介

https://mp.weixin.qq.com/s/vuY1tukawF_V2Bz2YSA8wQ

Attain.ai创始人李玉喜：强化学习遇见组合优化

https://mp.weixin.qq.com/s/DbR-Px_-vK-4xOweqsQKxw

基于网络单纯形法求解的最小流费用问题

https://mp.weixin.qq.com/s/KkfiDUWHuctfV7w6wvRwcA

如何让8岁表妹了解最经典的运输问题

---

在1978年，新的廉价航空公司进入市场。这些航空公司以劳动力成本低，操作简单(点对点)，服务简单，规模效应，以及技术变革的优势，能够以远低于全服务航空公司的票价获取利润。

大型航空公司需要一种策略来重新抓住这些以休闲为目的的旅客。然而，对这些巨头们来说，与后起之秀之间的全面价格战几乎是自杀式的。

“美利坚航空公司”（American Airlines）当时的营销副总裁罗伯特·克兰德尔(Robert Crandall)被普遍认为是解决这一问题的创新者。

首先，他们设计了对购买有重大限制的折扣:必须在出发前30天购买，不可退款，并且要求至少7天的停留时间。这些限制是为了防止大多数商务旅行者购买低价机票。（价格维度的策略）

与此同时，美利坚航空公司限制了每趟航班的折扣座位数量，这种方案提供了与新兴航空公司在价格上竞争的手段，同时又不会损害他们的核心商务旅行者收入。（数量维度的策略）

https://mp.weixin.qq.com/s/CYNbgL2UBPub8-ZFFfn2ug

浅谈收益管理与动态定价

---

统计学接受的训练是推断数据之外的内容，而分析学接受的训练是探索数据集的内容。换言之，分析学家对数据中的内容作出结论，而统计学家则对数据中没有的内容作出结论。

https://mp.weixin.qq.com/s/Jr4NlL4dN4hTDf8K4g-N_A

如何识别滥竽充数的“数据骗子”

# 异常检测

Anomaly Detection

1.基于聚类算法的异常检测。

2.基于孤立森林算法的异常检测。

3.基于支持向量机算法的异常检测。

4.基于高斯分布的异常检测。

5.基于马尔可夫链的异常检测。

参考：

https://github.com/yzhao062/anomaly-detection-resources

异常检测参考资源

https://github.com/zhuyiche/awesome-anomaly-detection

异常检测论文大列表：方法、应用、综述

http://chuansong.me/n/377440751130

异常点检测算法（一）

http://www.cnblogs.com/fengfenggirl/p/iForest.html

异常检测算法--Isolation Forest

https://mp.weixin.qq.com/s/xsuLIMPJVCThBGMRlz09Hg

Isolation Forest算法原理详解

http://www.bigdata8.top/front/article/466

异常值检测-滑动均值实现智能告警

https://mp.weixin.qq.com/s/ujG6Fr161kZh3S-lEiTJUg

异常检测（Anomaly Detection）

https://mp.weixin.qq.com/s/_Sds7O1wonVARKkb7qscww

腾讯：机器学习构建通用的数据异常检测平台

https://mp.weixin.qq.com/s/UcMPIf6ZRAhPjn79H2n1ig

异常点检测算法小结

https://mp.weixin.qq.com/s?__biz=MzIxODM4MjA5MA==&mid=2247487342&idx=3&sn=1981a6caec4591a19043d7a6176d359f

异常检测的阈值，你怎么选？给你整理好了...

https://mp.weixin.qq.com/s/ReQpT9KT6_tE8vXM-F_Ejw

从“马蜂窝事件”看，投资人如何避免数据尽职调查背后的交易风险？新时代数据造假特征及应对方法

https://www.zhihu.com/question/30508773

反欺诈(Fraud Detection)中所用到的机器学习模型有哪些？

https://mp.weixin.qq.com/s/jB5o4a6O4rrhMYD0GhKclw

基于机器学习算法的时间序列价格异常检测

https://mp.weixin.qq.com/s/PKX13Fv5fWmgX5IHLYgjmQ

基于无监督学习的期权定价异常检测

https://mp.weixin.qq.com/s/l1OKsahXHrFGr4YX441c9A

时序数据异常检测工具/数据集大列表

https://mp.weixin.qq.com/s/CuFbmcoXf4oG2YcDOgO3qw

异常检测基础

https://mp.weixin.qq.com/s/w7SbAHxZsmHqFtTG8ZAXNg

异常检测的N种方法，阿里工程师都盘出来了

https://www.zhihu.com/question/324999831

异常检测（anomaly/ outlier detection）领域还有那些值得研究的问题？

http://mp.weixin.qq.com/s/V-QCRXt5wAfSCiNT955W0A

外卖订单量预测异常报警模型实践

https://mp.weixin.qq.com/s/o_oRW5Ej9FmBq2L2Jz-brQ

AIOps中异常检测的简单应用

https://mp.weixin.qq.com/s/EOBjLqvi_WcSjiDb8b5Nwg

异常检测怎么做，试试孤立随机森林算法

https://mp.weixin.qq.com/s/uz5hjoG-m2PpnQxXZbHbFw

深度学习中的异常实例检测:综述论文

https://mp.weixin.qq.com/s/XXNw9ttT0BetZgqnrr68hw

深度学习异常检测，180页ppt

https://zhuanlan.zhihu.com/p/266513299

Anomaly Detection综述

https://mp.weixin.qq.com/s/mh6RW6XPpyEWZHG7TmBKzg

孤立森林:大数据背景下的最佳异常检测算法之一

https://mp.weixin.qq.com/s/FPsL5R6DFC2j3Wmzc3TJ2g

使用孤立森林进行异常检测

https://mp.weixin.qq.com/s/2J9P_sFjp9TxSvpBRthj0A

工业图像异常检测最新研究总结

https://mp.weixin.qq.com/s/8qR7Ew3oLzOZV2hV1QjMAA

最新49页《深度学习异常检测综述》论文，带你全面了解深度学习异常检测方法

https://mp.weixin.qq.com/s/NDs9xzVih0MHclJv2w1-TA

时序预测竞赛之异常检测算法综述

https://mp.weixin.qq.com/s/LlFQ-73hkgRENQiacwKHEg

四类异常检测算法综述：Isolation Forest、LOF、PCA及DAGMM

https://mp.weixin.qq.com/s/Jks-eZAinOvvWxKr0S-5Lg

使用计算机视觉来做异常检测

https://zhuanlan.zhihu.com/p/116235115

异常检测Anomaly Detection

https://zhuanlan.zhihu.com/p/359025580

基于图的异常检测

https://mp.weixin.qq.com/s/ddE7QchOzaz7Cq1NR3nr9Q

异常检测如何可解释？看这份KDD2021《可解释深度异常检测》教程

https://mp.weixin.qq.com/s/yOoDkTyZMmaFygt5MiGWtw

GAN如何异常检测？最新《生成式对抗网络异常检测》综述论文

https://mp.weixin.qq.com/s/qXIfqwP-ar77QhnooJRYqA

基于图的异常检测，94页ppt

# 因果推理

https://mp.weixin.qq.com/s/doi26r9AVIMbpkZ01wsCZA

北京大学何洋波博士《因果推断和因果图模型》机器学习报告

https://mp.weixin.qq.com/s/L1ZY_qXPMXpStIgtUp7AKQ

为机器学习插上因果推理的翅膀：这是一本系统的因果推理开源书

https://mp.weixin.qq.com/s/ejf0qbIZWpq6Mi4G60ssEQ

隐藏着的因果关系，如何让相同的机器学习模型变得不同

https://mp.weixin.qq.com/s/O4JAv4NWp7NL-CzmaB6GxQ

因果关系到底存不存在：反事实和平行宇宙

https://mp.weixin.qq.com/s/-SdFNMxHzdSenDYHtjfldA

因果推理入门指南-必须的7个步骤

https://mp.weixin.qq.com/s/UcE8lHkaM5RT7ghWz0o-MQ

R. A. Fisher和J. Neyman的分歧

https://mp.weixin.qq.com/s/oZTU7TAEf-gYzlSXdt0_BA

因果推断在阿里文娱用户增长中的应用
