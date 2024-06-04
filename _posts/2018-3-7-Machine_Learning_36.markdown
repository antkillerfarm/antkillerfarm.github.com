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

# 苏俄+

阿塞拜疆一个区委的第一书记，要20万卢布；第二书记便宜很多，10万卢布。每层官僚想方设法搞钱，搞到了钱再去找人送礼，好升更大的官、搞更多的钱。

阿塞拜疆第一书记阿利耶夫给勃列日涅夫送了一座纯金的半身像（真人大小的纯金半身像），换了政治局委员和部长会议第一副主席的位子。

丘尔巴诺夫，长得挺帅，34岁娶了41岁的加莉娅，做了勃列日涅夫的女婿。从此扶摇直上，从一名警卫，混到了内务部第一副部长。

---

米哈伊尔·瓦西里耶维奇·罗蒙诺索夫，1711～1765，俄国百科全书式的科学家、语言学家、哲学家和诗人，被誉为俄国科学史上的彼得大帝。1755年创办莫斯科大学。

---

俄罗斯南部喀萨尔人在中世纪建立的强大王国，就是这类有限君主制政体的突出例证。在喀萨尔人的王国里，国王任期届满时，或遇旱涝饥馑、战争失败等标志其精力已经衰退之情况时，都要被处死。

---

纳老扶持大女儿纳扎尔巴耶娃接任参议院院长，老总统的意图路人皆知——他打算在下个总统任期用女儿替换掉过渡人物托卡耶夫。

然而，冥冥之中，天意弄人，只因为纳老的大女婿因争夺权力被废，遂导致外孙倒戈，随后，哈萨克斯坦宫廷上演了“王子复仇记”现实版，这位楞头外孙把纳老的女儿藏在海外的10亿美金赃款给抖了出来。 

---

普京回忆在西德发现被北约特工盯梢的时候故意瞎转了一天，没去吃饭喝水，结果第二天他的车胎就被人扎了。

以后他就学老实了，到饭点就去饭馆，下午茶时间就进咖啡厅，自此他的车胎再没事情。

---

匈牙利有一个菜Goulash(古拉西)，深受匈牙利人喜欢。

1964年4月1日，赫鲁晓夫出访东欧国家匈牙利，他在布达佩斯一所电机厂演讲时提到共产主义，赫鲁晓夫生动的表述共产主义就是匈牙利人民就可以经常吃Goulash
了。

如果直译为“古拉希”，中国读者不知是何物，而在后面加上括号注解又嫌太长。有几个记者知道这个词，也吃过这道菜，说不过是土豆烧牛肉罢了。

---

九评苏共中央的公开信

https://www.marxists.org/chinese/reference-books/sino-soviet-debate/index.htm

中苏论战资料
