---
layout: post
title:  机器学习（三十八）——Optimizer进阶, 特征工程, 时间序列分析（2）
category: ML 
---

* toc
{:toc}

# Optimizer进阶

https://mp.weixin.qq.com/s/GS3TvS9nZw-CSJds-Aw_ug

UIUC孙若愚：60页论文综述深度学习优化

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

https://mp.weixin.qq.com/s/4uaaeZSXavbVuU8d1AZA6Q

浅谈交替方向乘子法(ADMM)的经典使用

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

https://mp.weixin.qq.com/s/P0qzzyVQke_c-RUF0Faitw

怎么判断一个优化问题是凸优化还是非凸优化？

https://mp.weixin.qq.com/s/scGkuMJ4lZULhmK69vWYpA

中国博士生提出最先进AI训练优化器RAdam，收敛快精度高，网友亲测：Adam可以退休了

https://mp.weixin.qq.com/s/010zXPYu36oLOoSkaA8YMg

RAdam优化器又进化：与LookAhead强强结合，性能更优速度更快（Ranger）

https://mp.weixin.qq.com/s/g5mPfqxtEQBUvJQr0ORVBg

可以丢掉SGD和Adam了，新的深度学习优化器Ranger：RAdam + LookAhead强强结合

https://mp.weixin.qq.com/s/OtmMKR0OWytcUgbCMrSc-A

不是我们喜新厌旧，而是RAdam确实是好用，新的State of the Art优化器RAdam

https://www.zhihu.com/question/305694880

为什么K-FAC这种二阶优化方法没有得到广泛的应用？

https://mp.weixin.qq.com/s/etv5Ucyo2tiu64ZtUygz0A

离线优化器

https://mp.weixin.qq.com/s/zy5ALOInXHIh8LHmihu1UA

在线优化器之FOBOS

https://mp.weixin.qq.com/s/7UhB8mSXQUfOPbDKqqg4rg

非光滑优化的光滑化

https://zhuanlan.zhihu.com/p/92230537

求解稀疏优化问题——半光滑牛顿方法

https://mp.weixin.qq.com/s/aLOd_W3juLuWaQeTdzAPjg

数值优化（1）——引入，线搜索：步长选取条件

https://zhuanlan.zhihu.com/p/68748778

指数移动平均（EMA）的原理

https://mp.weixin.qq.com/s/x7UQhSAiE9VJCzUSZfpytA

大规模锥优化之Splitting Conic Solver(SCS)

https://mp.weixin.qq.com/s/EmWRaAOTNYE0Maf6_r41oA

Adam那么棒，为什么还对SGD念念不忘？一个框架看懂深度学习优化算法

https://mp.weixin.qq.com/s/sXIOEGWdjE4_NWjVIe2d3Q

耶鲁大学等提出AdaBelief的新型优化器，速度快，训练稳，泛化强

# 特征工程

https://mp.weixin.qq.com/s/ibiElLIgrT3wYx3tDYMMTw

理解特征工程

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

一个神奇的特征选择轮子----MLFeatureSelection

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

# 时间序列分析

https://www.kaggle.com/thebrownviking20/everything-you-can-do-with-a-time-series/notebook

时间序列入门教程，从理论到业务实践，Kaggle kernels Master整理分享

https://mp.weixin.qq.com/s/FRSe1mJTvk9U66ta-r9iCQ

手把手教你用Python玩转时序数据，从采样、预测到聚类

https://mp.weixin.qq.com/s/Q82YzANWDMkKWm5k2XmPkA

严谨解决5种机器学习算法在预测股价的应用

https://mp.weixin.qq.com/s/iKM6zMSm1F2icjy79F9Hcg

季节性的分析才不简单，小心不要在随机数据中也分析出季节性

https://mp.weixin.qq.com/s/p8oN4xh-FHnay2eTsk6Gng

基于高阶模糊认知图与小波变换的时间序列预测

https://mp.weixin.qq.com/s/lmJk-iIzxxPmnZa6D8i_nw

一文简述如何使用嵌套交叉验证方法处理时序数据

https://mp.weixin.qq.com/s/05WAZcklXnL_hFPLZW9t7Q

时间序列模型之相空间重构模型

https://mp.weixin.qq.com/s/rIgjtILF7EtuBS5UWCEFcQ

重大事件后，股价将何去何从？

https://mp.weixin.qq.com/s/Y9d55KI64y-uRrWPRbDBzA

Kaggle知识点：时序数据与Embedding

https://mp.weixin.qq.com/s/DxRoTGtdrwqcjXL_ot57eg

如何找到时序数据中线性的趋势

https://mp.weixin.qq.com/s/iDUFr11-YX6oa6bLXWK3iQ

时序特征挖掘的奇技淫巧

https://mp.weixin.qq.com/s/S3xjk9QekWoni0eEvBhlLQ

特征工程之处理时间序列数据

https://mp.weixin.qq.com/s/15HXAIhmtYLbG3MjwEKDSQ

从移动平均到指数平滑

https://mp.weixin.qq.com/s/56so2p7a4wIgo38nVSR44A

时间序列分解总结
