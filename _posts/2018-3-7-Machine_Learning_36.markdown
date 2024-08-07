---
layout: post
title:  机器学习（三十六）——LightGBM, CatBoost
category: ML 
---

* toc
{:toc}

# Kalman filters（续）

https://zhuanlan.zhihu.com/p/40413223

卡尔曼滤波器实现详解

https://mp.weixin.qq.com/s/kBGhHCxq6idOOSGoLX5Kaw

手把手教你写卡尔曼滤波器

https://mp.weixin.qq.com/s/x0twRIdONCp3-qjhFJuCEQ

手把手教你写扩展卡尔曼滤波器

https://mp.weixin.qq.com/s/Nta9ksUkAVoX8arBGm7qqg

手把手教你实现多传感器融合技术

https://zhuanlan.zhihu.com/p/41767489

概率机器人——扩展卡尔曼滤波、无迹卡尔曼滤波

https://mp.weixin.qq.com/s/J27fVvYMRdoAgQtfXsywxg

OpenCV卡尔曼滤波介绍与代码演示

https://zhuanlan.zhihu.com/p/64007212

卡尔曼滤波家族

https://zhuanlan.zhihu.com/p/66646519

IKF(IEKF)推导

https://blog.csdn.net/baidu_21807307/article/details/51843079

浅谈卡尔曼滤波（Kalman Filter）

https://mp.weixin.qq.com/s/ZlyF1GcmpwZoT-3o4T567A

卡尔曼滤波算法及其应用

https://zhuanlan.zhihu.com/p/77327349

如何理解那个能造导弹军舰还把嫦娥送上天的卡尔曼滤波算法Kalman filter?

https://mp.weixin.qq.com/s/vAm0QDp_i2ec5pZbTmdHNA

傻瓜也能懂的卡尔曼滤波器

https://mp.weixin.qq.com/s/na0vVhECfBppTb7BmhRyzA

从限价订单薄中推导预测因子：卡尔曼滤波来搞定！

https://mp.weixin.qq.com/s/v460ql4RnJGbzbV0iZH4kA

深度解读卡尔曼滤波原理

https://mp.weixin.qq.com/s/Jlux3pZ4keVzkwWzgoPrEQ

深入浅出讲解卡尔曼滤波

https://mp.weixin.qq.com/s/BeIEjASATt3Qpef_Ag-cSw

卡尔曼滤波系列——经典卡尔曼滤波推导

https://mp.weixin.qq.com/s/t4GIPMB-6Vq7i8Q5PN6L-w

追狗，从入门到精通

https://mp.weixin.qq.com/s/EZ4JQM2vynTevUjFpW1h_w

追狗，从入门到精通2.0

https://zhuanlan.zhihu.com/p/128520715

自动驾驶定位技术-马尔科夫定位

https://zhuanlan.zhihu.com/p/138684962

自动驾驶感知融合-卡尔曼及扩展卡尔曼滤波(Lidar&Radar)

https://zhuanlan.zhihu.com/p/141059329

自动驾驶感知融合-无迹卡尔曼滤波(Lidar&Radar)

https://zhuanlan.zhihu.com/p/134595781

卡尔曼滤波(Kalman filter)含详细数学推导

https://zhuanlan.zhihu.com/p/166342719

卡尔曼滤波器详解——从零开始(1)

https://zhuanlan.zhihu.com/p/179480833

卡尔曼滤波器详解——从零开始(2)

https://mp.weixin.qq.com/s/3K9qdH9FXnYABpJoJkFcqw

使用卡尔曼滤波平滑时间序列，提高时序预测的准确率

https://zhuanlan.zhihu.com/p/35978617

线性动态系统与卡尔曼滤波

https://mp.weixin.qq.com/s/2rX6iRTYBk47V29fSTAMQQ

图解卡尔曼滤波(Kalman Filter)

https://mp.weixin.qq.com/s/PuvTkDhwbYv8TK8t-Zhcug

基于卡尔曼滤波的注意力机制—广告点击率预估中的用户行为建模   

https://longaspire.github.io/blog/%E5%8D%A1%E5%B0%94%E6%9B%BC%E6%BB%A4%E6%B3%A2/

卡尔曼滤波器

https://zhuanlan.zhihu.com/p/36745755

卡尔曼滤波：从入门到精通

https://zhuanlan.zhihu.com/p/338269917

从全状态观测器到卡尔曼滤波器

https://mp.weixin.qq.com/s/gb7CX8mbQNkMFVe3DIDT6Q

卡尔曼滤波最完整公式推导

https://mp.weixin.qq.com/s/vChWpG_2m53n8Bz-yfCohg

实操教程：用一维卡尔曼滤波器来估计运动物体的位置和速度

https://zhuanlan.zhihu.com/p/408783183

卡尔曼滤波的基本原理（也许是我写过最详细的推导）

https://www.zhihu.com/column/c_1303778703026126848

一个多传感器信息融合的专栏

https://www.zhihu.com/question/41823401

有什么将卡尔曼滤波讲得透彻的书籍或资料？

https://www.zhihu.com/question/588969519

组合导航和卡尔曼滤波，一个美国七十年代提出的理论，一个已经成熟的技术，研究生现在研究还有希望毕业吗？

# LightGBM

LightGBM是微软推出的boosting框架。

代码：

https://github.com/Microsoft/LightGBM

文档：

https://lightgbm.readthedocs.io/en/latest/

参考：

http://www.msra.cn/zh-cn/news/features/lightgbm-20170105

微软亚洲研究院：LightGBM介绍

https://zhuanlan.zhihu.com/p/25308051

比XGBOOST更快--LightGBM介绍

https://www.zhihu.com/question/51644470

如何看待微软新开源的LightGBM?

http://www.cnblogs.com/rocketfan/p/6005353.html

LightGBM中GBDT的实现

https://zhuanlan.zhihu.com/p/28768447

一个例子读懂LightGBM的模型文件

https://zhuanlan.zhihu.com/p/27916208

LightGBM调参指南(带贝叶斯优化代码)

https://mp.weixin.qq.com/s/zopaNDXABhPVqqUur9FFYg

lightgbm算法优化-不平衡二分类问题

https://mp.weixin.qq.com/s/JQasgzl-EpqBey7W6jKCTw

LightGBM大战XGBoost，谁将夺得桂冠？

http://blog.csdn.net/xwd18280820053/article/details/68927422

关于树的几个ensemble模型的比较（GBDT、xgBoost、lightGBM、RF）

https://mp.weixin.qq.com/s/TD3RbdDidCrcL45oWpxNmw

从结构到性能，一文概述XGBoost、Light GBM和CatBoost的同与不同

https://mp.weixin.qq.com/s/8IFKL0thCMBh40wUolbTVw

从XGB到LGB：美团外卖树模型的迭代之路

https://mp.weixin.qq.com/s/LoX987dypDg8jbeTJMpEPQ

终于有人把XGBoost和LightGBM讲明白了，项目中最主流的集成算法！

https://mp.weixin.qq.com/s/l6Fp5WTNH0b_cl2y7Az76Q

深入理解LightGBM

https://mp.weixin.qq.com/s/H9zkyO9oZAysWMyigd1tNw

LightGBM

https://mp.weixin.qq.com/s/RaWeiQwlQjCi1zz5S3tOmA

LightGBM的参数详解以及如何调优

https://mp.weixin.qq.com/s/9gEfkiZyZkoIgwRCYISQgQ

你应该知道的LightGBM各种操作！

https://mp.weixin.qq.com/s/YSDB6SSrU7xlzZ73PrjIAw

比赛杀器LightGBM常用操作总结！

https://mp.weixin.qq.com/s/_y16bW-afo9gOI5ObMBRYQ

Kaggle神器LightGBM最全解读！

https://mp.weixin.qq.com/s/A3b2L_0-atm5jJslhr50Bg

最新LightGBM进展介绍报告，39页ppt

https://mp.weixin.qq.com/s/M0W6-bLZcsdMsKBUAeKDnw

树模型奠基性论文解读——GBM: Gradient Boosting Machine

https://zhuanlan.zhihu.com/p/672584013

lleaves：使用LLVM编译梯度提升决策树将预测速度提升10+倍

# CatBoost

这是Yandex推出的Boost工具包。

官网：

https://catboost.yandex/

论文：

《Fighting biases with dynamic boosting》

代码：

https://github.com/catboost/catboost

官方还提供了一个可视化工具：

https://github.com/catboost/catboost-viewer

参考：

https://mp.weixin.qq.com/s/TAminQXid3qq5b8qkeN1rA

ClickHouse如何结合自家的GNDT算法库CatBoost来做机器学习

https://mp.weixin.qq.com/s/4qQxB4AthVAYKggEV3BHFw

ThunderGBM：快成一道闪电的梯度提升决策树

https://mp.weixin.qq.com/s/LxPkd6PUHWoHQCw-xyE9SQ

大战三回合：XGBoost、LightGBM和Catboost一决高低

https://mp.weixin.qq.com/s/E3pSPsG18053F5GG1Z8jNQ

一文详尽系列之CatBoost

https://mp.weixin.qq.com/s/eCZHpFvtDYnpI6jm2nEtnQ

深入理解CatBoost

https://mp.weixin.qq.com/s/pQ9_0d8sl5Sr5O360Risnw

使用CatBoost进行不确定度估算：模型为何不确定以及如何估计不确定性水平

https://mp.weixin.qq.com/s/o129MwpGzx8tzjUCkz1npw

使用CatBoost和NODE建模表格数据对比测试

# 天文杂谈+

![](/images/img4/Van_Allen_radiation_belts.jpg)

范·艾伦辐射带（Van Allen radiation belts）是地球磁场捕获的太阳宇宙射线和部分银河宇宙射线中的高能带电粒子组成。一般来说，内辐射带里高能质子多，外辐射带里高能电子多。

1958年1月31日，美国第一颗人造卫星探险者一号升空，当升至800千米高空时，星上所载盖革计数器读数突然下降至0。到1958年3月26日探险者三号升空时，又发生了同样的情况。James Van Allen认为，这是因为存在极大量的辐射导致计数器达到饱和而失灵造成的。因此，同年7月26日发射探险者四号时，他在计数器前端加入一小片薄铅以阻挡部分辐射。果然，新的卫星证实了他的猜测。

>James Van Allen，1914～2006，University of Iowa博士（1939）+教授。美国科学院院士。

---

恩克彗星是人们发现的第一颗短周期彗星，至今也还没有找到过周期比它更短的彗星。恩克用6个星期的时间，计算出这颗彗星的轨道，周期为3.3年，并且预言1822年5月24日再回到近日点，果然它准时回来了，成为继哈雷彗星之后，第二颗按预言回归的彗星。

>Johann Franz Encke，1791～1865，德国天文学家。University of Göttingen毕业，导师Gauss。1825年担任柏林天文台台长。Leopold Kronecker的导师。他还发现了土星环的Encke Gap。

---

1929年，余青松接任天文研究所第二任所长后，秉承高鲁的宏愿，费时五载，于公元1935年，建立紫金山天文台。1938年因抗日战争，他主持该台的内迁工作，并在昆明东郊建成了昆明凤凰山天文台。

https://mp.weixin.qq.com/s/vMe5RmD8uBW3AeGDYwbI5A

紫金山天文台的创立（高鲁、余青松）

---

https://view.inews.qq.com/wxn/20211224A03SC600

韦伯望远镜即将发射！梭哈百亿美元上天，是为了看啥？

https://view.inews.qq.com/wxn/20211225A0A9SJ00

韦伯望远镜成功背后：成本约110亿美元，共推迟发射18次

https://view.inews.qq.com/wxn/20211225A08N9G00

“鸽王之王”韦伯终于上天！比哈勃看得更远，烧钱更多

https://mp.weixin.qq.com/s/AIMccIbtB6W5nwu-6yRuJw

“鸽王” 韦伯上天之前，你需要知道这些

https://mp.weixin.qq.com/s/BYw9HJehGC528WkX_q4JLw

情人节将至，韦伯望远镜首次太空“睁眼”！

https://www.sohu.com/a/569342145_161795

韦伯太空望远镜主镜被太空陨石砸中，受损情况比最初想象要大

---

第四宇宙速度（fourth cosmic velocity），是指在地球上发射的物体摆脱银河系引力束缚，飞出银河系所需的最小初始速度。但由于人们尚未知道银河系的准确大小与质量，因此只能粗略估算，其数值在525公里/秒以上。

---

https://mp.weixin.qq.com/s/aY4gFbK_aKWcgwvr0Rk5gQ

“波多黎各，波多黎各，世界最大射电望远镜，倒闭了”（Arecibo望远镜）

https://www.zhihu.com/question/432120058

阿雷西博望远镜馈源舱支撑平台塌了，现已决定退役拆除，如何评价它的一生？

https://zhuanlan.zhihu.com/p/330003494

美国天眼塌了
