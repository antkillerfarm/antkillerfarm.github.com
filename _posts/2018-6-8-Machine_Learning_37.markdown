---
layout: post
title:  机器学习（三十七）——Optimizer进阶, 推荐系统进阶, 强连通分量算法
category: ML 
---

# Optimizer进阶

## AdaSecant

《ADASECANT: Robust Adaptive Secant Method for Stochastic Gradient》

## 二阶Optimizer

虽然二阶Optimizer的收敛效果优于一阶Optimizer，但由于计算量较大，通常用的较少。

常用的算法有BGFS和L-BFGS。

http://www.cnblogs.com/kemaswill/p/3352898.html

优化算法-BFGS

http://blog.csdn.net/acdreamers/article/details/44728041

L-BFGS算法

https://mp.weixin.qq.com/s/lGrTUYALmKOQkO70DZpbPQ

小改进，大飞跃：深度学习中的最小牛顿求解器

## 参考

https://mp.weixin.qq.com/s/n1Ks8I3Ldgb-u-kVbGBZ5Q

机器学习中的优化方法

https://mp.weixin.qq.com/s/4XOI8Dq6fqe8rhtJjeyxeA

超级收敛：使用超大学习率超快速训练残差网络

http://mp.weixin.qq.com/s/Q5kBCNZs3a6oiznC9-2bVg

Michael Jordan新研究官方解读：如何有效地避开鞍点

https://mp.weixin.qq.com/s/idmt0F49tOCh-ghWdHLdUw

吴恩达导师Michael I.Jordan学术演讲：如何有效避开鞍点。这是Jordan半年后的另一个演讲，有些新内容。

https://mp.weixin.qq.com/s/jVjemfcLzIWOdWdxMgoxsA

超越Adam，从适应性学习率家族出发解读ICLR 2018高分论文

https://mp.weixin.qq.com/s/B9nUwPtgpsLkEyCOlSAO5A

1cycle策略：实践中的学习率设定应该是先增再降

https://mp.weixin.qq.com/s/dseeCB-CRtZnzC3d4_8pYw

AMSGrad能够取代Adam吗

https://mp.weixin.qq.com/s/7E8o1TnvmAvZgB7_AWCunQ

2018值得尝试的无参数全局优化新算法

https://mp.weixin.qq.com/s/T-v9OTcJa5OQ71QmYrFtbg

斯坦福大学提出SGD动量自调节器YellowFin

https://mp.weixin.qq.com/s/6u5W7Lm81Wtczdzp5WCJWw

DeepMind提出新型超参数最优化方法：性能超越手动调参和贝叶斯优化

https://mp.weixin.qq.com/s/0V8B-u5_bRM5Fu9oOAYjqw

清华大学：通过在单纯形上软门限投影的加速随机贪心坐标下降

https://mp.weixin.qq.com/s/LuuvvL9yZ3ucXxRq0pZfsg

优化策略：Label Smoothing Regularization_LSR原理分析

https://zhuanlan.zhihu.com/p/23866364

从梯度下降到Hessian-Free优化

https://mp.weixin.qq.com/s/HPrjEdszBSvVoVS66W-Fjw

2017年深度学习优化算法研究亮点最新综述

https://mp.weixin.qq.com/s/W06YcuGWalDbyUaZa_kZnQ

2017年深度学习优化算法最新综述

https://mp.weixin.qq.com/s/WQ6CxRS-v_y-7PnYY-1ffg

在局部误差边界条件下的随机子梯度方法的加速

https://mp.weixin.qq.com/s/rOltA6fDzWmxcSyoHYqeSg

谷歌提出最新参数优化方法Adafactor，已在TensorFlow中开源

https://mp.weixin.qq.com/s/eTVPLSpZir4A49bhmWAibQ

深度学习中的各种优化算法

https://mp.weixin.qq.com/s/CQpIhVinDPhXpp70WhyYww

当前训练神经网络最快的方式：AdamW优化算法+超级收敛

https://mp.weixin.qq.com/s/y3ThoC2A04q4uWiOsuhUJw

腾讯AI Lab提出误差补偿式量化SGD：显著降低分布式机器学习的通信成本

https://mp.weixin.qq.com/s/aBt0qXPHFvwSs-0MtJYKjQ

一文告诉你Adam、AdamW、Amsgrad区别和联系，助你实现Super-convergence的终极目标

https://mp.weixin.qq.com/s/aZlJNZsSv60ZZi2heGo_Mw

一文简述深度学习优化方法——梯度下降

https://mp.weixin.qq.com/s/DsmjjfInV_yPFWB2oSq-dA

取代学习率衰减的新方法：谷歌大脑提出增加Batch Size

https://mp.weixin.qq.com/s/jgOQGDqDKtbJXbAj3EpI9A

别用大批量mini-batch训练神经网络，用局部SGD！

https://zhuanlan.zhihu.com/p/45298186

Matrix Factorization方法证明总结

https://mp.weixin.qq.com/s/0z4mt8iVk5gLRDwbhznV2g

如何理解深度学习的优化？通过分析梯度下降的轨迹

https://mp.weixin.qq.com/s/5MI1J16sEkr4UR4rSrw1wA

Michael Jordan新研究：采样可以比优化更快地收敛

https://mp.weixin.qq.com/s/g8GLF0rf3IPAjRb9wZaS4w

神经网络的奥秘之优化器的妙用

https://mp.weixin.qq.com/s/i-fE4aISTJ0584aIHJ8R0Q

二阶优化！训练ImageNet仅需35个Epoch

https://mp.weixin.qq.com/s/LY1-F5hEyM40DrvobYRexA

腾讯AI Lab&北大提出基于随机路径积分的差分估计子非凸优化方法

https://mp.weixin.qq.com/s/5KyODpSjkdYJ9q-itQDsAA

自Adam出现以来，深度学习优化器发生了什么变化？

https://mp.weixin.qq.com/s/3FSZOlA2sGQwiPj77ShTIQ

最优化算法鸟视解读

https://mp.weixin.qq.com/s/4hSar7SuCjLkZUjuIfu1Lg

如何选择最适合你的学习率变更策略

https://zhuanlan.zhihu.com/p/32923584

Tensorflow中learning rate decay的奇技淫巧

https://mp.weixin.qq.com/s/qk3cw05ZdlYEKDGRG0fnLg

距离几何优化问题--从美国计算机教授追回被抢车辆谈起

https://mp.weixin.qq.com/s/rHkfb1pZhtzVjzYiTRB4WA

交替方向乘子法（ADMM）的基本原理

https://mp.weixin.qq.com/s/E3Iq8YpIZRZOk7SP-cu1xQ

如何找到全局最小值？先让局部极小值消失吧

https://mp.weixin.qq.com/s/el1E-61YjLkhFd6AgFUc7w

拳打Adam，脚踢SGD：北大提出全新优化算法AdaBound

https://mp.weixin.qq.com/s/TfrJ-rep-TIg345SXursbw

为了围剿SGD大家这些年想过的那十几招

https://mp.weixin.qq.com/s/9laU3EW0B64rwVb7so1BEA

机器学习中的最优化算法总结

https://mp.weixin.qq.com/s/mylRodVvvzI3e0-9-fEzTw

深度研究自然梯度优化，从入门到放弃

# 推荐系统进阶

除了《机器学习（十五～十六）》提及的ALS和PCA之外，相关的算法还包括：

## FM：Factorization Machines

Factorization Machines是Steffen Rendle于2010年提出的算法。

>注：Steffen Rendle，弗赖堡大学博士，现为Google研究员。libFM的作者，被誉为推荐系统的新星。

FM算法实际上是一大类与矩阵分解有关的算法的广义模型。

参考文献1是Rendle本人的论文，其中有章节证明了SVD++、PITF、FPMC等算法，都是FM算法的特例。《机器学习（十四）》中提到的ALS算法，也是FM的特例。

参考文献2是国人写的中文说明，相对浅显一些。

参考：

https://www.ismll.uni-hildesheim.de/pub/pdfs/Rendle2010FM.pdf

http://blog.csdn.net/itplus/article/details/40534885

Factorization Machines 学习笔记（一）预测任务

https://tech.meituan.com/deep-understanding-of-ffm-principles-and-practices.html

深入FFM原理与实践

https://github.com/aksnzhy/xlearn

这是一个集成了FM和FFM等算法的库

https://mp.weixin.qq.com/s/MEjhJycDwssvzXm7DaSp7Q

推荐系统召回模型之全能的FM模型

## PITF

配对互动张量分解（Pairwise Interaction Tensor Factorization）算法，也是最早由Rendle引入推荐系统领域的。

论文：

http://www.wsdm-conference.org/2010/proceedings/docs/p81.pdf

## 其他

https://mp.weixin.qq.com/s/7yjA3_oCI5nSH4tv04BIhQ

HFT

https://mp.weixin.qq.com/s/gHKOArFzUM9Zn8hEsA-1wQ

FISM

https://mp.weixin.qq.com/s/VymwTuKq86JP2PL4v8LyyQ

POI by Friends

https://mp.weixin.qq.com/s/LnV-Oq3pCCeMk9RRhha-Aw

GLSLIM

https://mp.weixin.qq.com/s/xnJq-aBAZW22tP7RQylKLw

iCD

https://mp.weixin.qq.com/s/-IPwfrBz1dtYDupuGv4IjQ

Ensemble

https://mp.weixin.qq.com/s/SC8kNYvexetmDuxfQvwSDw

CKE

https://mp.weixin.qq.com/s/bu9rSno_WmHHisE3lzYnqg

ConvMF

https://mp.weixin.qq.com/s/opJtn5mPVjnfRwr5UZ4aJg

FTRL原理与工程实践

http://www.cnblogs.com/EE-NovRain/p/3810737.html

各大公司广泛使用的在线学习算法FTRL详解

## 冷启动

https://mp.weixin.qq.com/s/ll2Nx7_1Sg7XfuiH-OQnWA

推荐系统如何冷启动？

https://mp.weixin.qq.com/s/ecAA43az0-6UIEPR_maNAw

推荐系统之冷启动问题

https://mp.weixin.qq.com/s/ycxigg0HbiOTRUBPAWk21Q

推荐系统中的冷启动问题和探索利用问题

## 参考

https://mp.weixin.qq.com/s/q9FU19Hpw2eWLLhsY5lYJQ

parameter-free contextual bandits

https://mp.weixin.qq.com/s/T-yCjebTzc_t6D4o5gyQLQ

Collaborative Metric Learning

https://mp.weixin.qq.com/s/9xxLU51eqhc6C81jzHQijQ

简述推荐系统中的矩阵分解

https://mp.weixin.qq.com/s/qmkMJJkMqumbtynM4cLatw

计算广告CTR预估系列

https://zhuanlan.zhihu.com/p/38117606

早期购买行为的分析和预测建模

https://mp.weixin.qq.com/s/nQAqEZW_TJSsbVp-kVcKGA

简谈马尔可夫模型在个性化推荐中的应用

https://mp.weixin.qq.com/s/Zalby_gZsxzrmBq6ZePsiQ

短视频如何做到千人千面？FM+GBM排序模型深度解析

https://mp.weixin.qq.com/s/hkXyqe6tDdgNuLgk3e90JQ

如何发现品牌潜客？目标人群优选算法模型及实践解析

https://mp.weixin.qq.com/s/jCXfu6AHFWnQL118r38Zpw

全球顶级算法赛事Top5选手，跟你聊聊推荐系统领域的“战斗机”

https://mp.weixin.qq.com/s/rzCqz2HasT5zxBQjHy13Tw

前深度学习时代CTR预估模型的演化之路：从LR到FFM

https://mp.weixin.qq.com/s/Sc6VR--Qzk1BpS638SpS9Q

盘点前深度学习时代阿里、谷歌、Facebook的CTR预估模型

https://mp.weixin.qq.com/s/iEMGS1tbNPYXYIM7pKkq4A

达观数据：计算广告系统算法与架构综述

https://mp.weixin.qq.com/s/pWcFuOecG-dZHZ365clDjg

阿里妈妈新突破！深度树匹配如何扛住千万级推荐系统压力

https://mp.weixin.qq.com/s/R6-y-W0CGhlEUPXDkYdtJw

Item-Based协同过滤算法

https://mp.weixin.qq.com/s/C5cokipqnSsgg53chbi3oA

我是怎么走上推荐系统这条（不归）路的……

https://mp.weixin.qq.com/s/RA6Elm6C4gnlg0wIOnvlug

一步步构建推荐系统

https://mp.weixin.qq.com/s/WV0igXcxFuTNKU5MvZrl0Q

再谈亚马逊Item-based推荐系统

https://mp.weixin.qq.com/s/mXE3f2ZO6wQRSURZGmyBpQ

产品聚类

https://mp.weixin.qq.com/s/lvbXtLfa6Z4h_5lCHKn_6A

CTR点击率预估之经典模型回顾

https://mp.weixin.qq.com/s/GowWRaE5mZoPVAPK6_AkrA

如何构建可解释的推荐系统？

https://mp.weixin.qq.com/s/7QrSSEiLjmGjNhFfU6ue0A

推荐系统产品与算法概述

https://mp.weixin.qq.com/s/49o_AobVNs_9Lgm4qbQcPA

一文全面了解基于内容的推荐算法

https://mp.weixin.qq.com/s/uxi0OTxjmn047fE8nbYplw

推荐系统：石器与青铜时代

https://zhuanlan.zhihu.com/p/74978160

揭开Top-N经典算法SLIM和FISM之谜

https://mp.weixin.qq.com/s/S9-GgQTlx2c14mN_3kNQiQ

基于标签的实时短视频推荐系统

# 强连通分量算法

http://ishare.iask.sina.com.cn/f/34626295.html

矩阵不可约的充要条件

http://www.cnblogs.com/saltless/archive/2010/11/08/1871430.html

求强连通分量的Tarjan算法

http://blog.csdn.net/dm_vincent/article/details/8554244

求解强连通分量算法之---Kosaraju算法

http://www.cnblogs.com/luweiseu/archive/2012/07/14/2591370.html

强连通分支算法--Kosaraju算法、Tarjan算法和Gabow算法
