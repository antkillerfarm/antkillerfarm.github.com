---
layout: post
title:  Machine Learning之Python篇（二）, 推荐系统的工程细节
category: AI 
---

# Machine Learning之Python篇

## 参考

https://mp.weixin.qq.com/s?__biz=MzIxNjA2ODUzNg==&mid=2651435914&idx=1&sn=b0b6f1cda08798dc35bf0930dc9d9096

盘膝而坐--跟你聊聊APP市场的数据分析

https://mp.weixin.qq.com/s/AVSFbfikczjm7flsQQPC1g

2017年最受数据科学欢迎的Top15个Python库

https://mp.weixin.qq.com/s/jBULfMR0_7Ms9nc5CLLIFQ

Python的开源人脸识别库：离线识别率高达99.38%

https://zhuanlan.zhihu.com/p/30630608

Python和MATLAB交互的基本操作

https://mp.weixin.qq.com/s/drpd1O502TcL54cHSEdl_g

如何为时间序列数据优化K-均值聚类速度？

https://mp.weixin.qq.com/s/Di4xVdD1jOGGCiIB-vdkkg

Python可以被用来做哪些神奇好玩的事情

https://mp.weixin.qq.com/s/-SHD9rXUGQ-yp21RtFhJpg

手把手教你 Python挖掘用户评论典型意见并自动生产报告

https://mp.weixin.qq.com/s/OxNj9fWaEMh8SuQiK52HWg

sklearn中PCA库讲解与实战

https://mp.weixin.qq.com/s/ZbT5ASvnTODkodTJjBGhnw

Python环境下的8种简单线性回归算法

https://mp.weixin.qq.com/s/iL53BraKspGJ1rw7LK-3bA

Logistic回归Python实战，评估销售系统的盈利能力

https://mp.weixin.qq.com/s/JwsG0XzIGr5jiQqrOTGFGQ

8种用Python实现线性回归的方法，究竟哪个方法最高效？

https://mp.weixin.qq.com/s/4j-DgtlxGKCx8SvLEBgjoA

Python上的图模型与概率建模工具包：pomegranate

https://mp.weixin.qq.com/s/2Gd-2BLZy7LdvOenbM2Fsg

用hmmlearn学习隐马尔科夫模型HMM

https://mp.weixin.qq.com/s/yY_-qJoza2xGRqrm40abkg

每个Kaggle冠军的获胜法门：揭秘Python中的模型集成

# 推荐系统的工程细节

推荐系统不仅是算法，还包括若干工程细节。这些细节虽然不算复杂，够不上算法的层面，然而对产品的成败，却有举足轻重的作用。

## 一般架构

![](/images/article/suggest.png)

## 用户数据

用户自然特征：性别，年龄，地域，教育水平，出生日期，职业，星座

用户兴趣特征：兴趣爱好，使用网站，浏览/收藏内容，互动内容，品牌偏好，产品偏好

用户社会特征：婚姻状况，家庭情况，社交/信息渠道偏好

用户消费特征：收入状况，购买力水平，已购商品，购买渠道偏好，最后购买时间，购买频次

## 评价推荐质量

以今日头条类的新闻推荐为例，评估指标一般为：

1. 平均转化率（点击量/曝光量）

2. 人均阅读篇数（点击PV/点击UV）

3. 平均阅读完成率

4. 人均阅读时长（这个会受文章长度影响）

5. 用户互动数据（评论、分享）

对评估指标的选取一般遵循以下原则：

1.用户满意度。

2.预测准确度。

3.覆盖率。

4.多样性。

5.新颖性。

6.惊喜度。

7.信任度。

8.实时性

9.鲁棒性

测试方法一般包括：

A/B测试、双盲交叉验证

需要思考的问题

1.新用户/新商品的冷启动问题。

2.马太效应问题。这在推荐算法中也叫做banana problem。原因是在美国这边的超市，几乎所有人都爱买banana，因为最便宜，也好吃也健康。所以过多的数据量可能会导致一个超市推荐算法疯狂推荐Banana给所有人。

PS：NLP领域的TF-IDF算法可有效防止马太效应问题。

参考：

https://www.zhihu.com/question/26990692

类似今日头条这样的个性化推荐网站怎么评价推荐质量的优劣？

https://www.zhihu.com/question/19660417

为什么那么多牛人成天在研究讨论算法，系统自动推荐的东西还是不能令人满意呢？

## 术语

GMV (Gross Merchandise Volume) 成交额

![](/images/article/pv.jpg)

Click-Through-Rate, 网络广告（图片广告/文字广告/关键词广告/排名广告/视频广告等）的点击到达率，即该广告的点击量（严格的来说，可以是到达目标页面的数量）除以广告的浏览量（PV- Page View）。
