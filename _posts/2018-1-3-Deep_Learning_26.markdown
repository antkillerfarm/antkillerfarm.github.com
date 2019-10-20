---
layout: post
title:  深度学习（二十六）——深度推荐系统（1）
category: DL 
---

# 深度推荐系统

推荐系统一直是AI能够落地且商业前景很好的一个研究方向。自2016年以来，该方向也逐渐被DL所侵蚀，尽管目前从招聘来说，这方面的职位仍以普通ML为主。

2017年5月，我曾面试了一家电商企业。当时给我的感觉，虽然里面的工程师较早接触ML，然而知识老化现象比较严重，对最基本的神经网络知识缺乏必要的了解。这显然给了后来者一个弯道超车的好机会。

## 教程

https://mp.weixin.qq.com/s/3L-HNGBd1xW8SpK6BpEEtg

在线购买率转化高达60%，Amazon推荐系统是如何做到的？这个blog篇幅较长，基本涵盖了推荐系统的各个方面，包括传统算法和深度算法都有涉及。

https://mp.weixin.qq.com/s/aB8PnQuPmhftS18fUrumNg

零基础如何快速搭建一个推荐系统？这篇blog是某牛课程的广告贴，然而从课程目录中，我们还是能够知道一个推荐系统的大致知识点。

https://mp.weixin.qq.com/s/2RRzZcoy4L92n3VZH0o07w

无偏见排序学习：理论与实践135页PPT教程

## 算法

深度推荐系统的算法包括：

https://mp.weixin.qq.com/s/qwDIvXlpP5UIBTwtpqhYsg

Auto-Encoder

https://mp.weixin.qq.com/s/AqgxnfR4h1FBRmmEe6uPqQ

CDL

https://mp.weixin.qq.com/s/WlgUVf1EjpO9UGqjTJN5ww

UWRL

https://mp.weixin.qq.com/s/KII9oNg7kqfco2MngUOGAw

AutoRec

https://mp.weixin.qq.com/s/mnGuPGtdw9d1BzeNpoYYqw

DeepCoNN

https://mp.weixin.qq.com/s/lJDiP7oeiFQSEyxWt_9uBA

NFM

https://mp.weixin.qq.com/s/G4bDj4a05K0kB4IZ6IosiQ

Wide & Deep

https://mp.weixin.qq.com/s/JNGKz4-fWG4ygl7f6UkxcQ

DeepFM

这些算法的演化路径具体来说有两条：其一是从FM开始推演其在深度学习上的各种推广（对应下图的红线），另一条是从embedding+MLP自身的演进特点结合CTR预估本身的业务场景进行推演（对应下图黑线部分）。 

![](/images/img2/DL_recommend.jpg)

这是另一个版本的演化图谱：

![](/images/img3/recommend.jpg)

## Sequential Recommender

![](/images/img3/sequential_recommendation.png)

上图中，灰色表示MLP，橙色表示RNN，黄色表示CNN，蓝色表示Attention，绿色表示GNN。

## 工具

https://www.librec.net/

这是一个Java写的推荐系统。东北大学的郭贵冰主持的项目。该网址同时也有不少相关论文可供阅读。

https://github.com/cheungdaven/DeepRec

这是一个基于Tensorflow的深度推荐系统

## 参考

https://zhuanlan.zhihu.com/learningdeep

一个深度推荐的专栏

https://mp.weixin.qq.com/s/GMHjXa2r_1SG3HsA-bcIOQ

从FM推演各深度CTR预估模型

https://mp.weixin.qq.com/s/4CLFaP4F-CknDcOWSOdiTQ

基于深度学习的序列推荐系统综述：概念、方法与实验评估

https://mp.weixin.qq.com/s/Yz3vv6H4oHyZ-vX71yv5gw

谷歌、阿里等10大深度学习CTR模型最全演化图谱

https://zhuanlan.zhihu.com/p/26237106

深度学习在推荐算法上的应用进展

http://i.dataguru.cn/mportal.php?mod=view&aid=11463

深度学习在推荐领域的应用

https://zhuanlan.zhihu.com/p/33214451

深度学习在推荐系统上的应用

https://mp.weixin.qq.com/s/ouHfqFk_WQk7ZofvabEwZg

推荐系统Wide & Deep Learning详解

https://mp.weixin.qq.com/s/hGvQvddD3i858XSK4z08Ug

主要推荐系统算法总结及Youtube深度学习推荐算法实例概括

https://mp.weixin.qq.com/s/yHtqWJUpCIvTStKW5TINaA

Youtube短视频推荐系统变迁：从机器学习到深度学习

https://mp.weixin.qq.com/s/N1oLs-saWN_ifkWEaWw_Vg

YouTube 2016年公布的基于深度学习的推荐算法

https://mp.weixin.qq.com/s/WzSO_XobY6kesDm4sF-hBg

深度学习之推荐篇

https://mp.weixin.qq.com/s/LKjVfhyhL4GVx6l5WC6-CQ

如何用深度学习实现用户行为预测与推荐

https://mp.weixin.qq.com/s/UrMsMHAkqNHJEl5lhAvLtA

腾讯提出并行贝叶斯在线深度学习框架PBODL：预测广告系统的点击率

http://mp.weixin.qq.com/s/Jiis7j3W3D5GG_ZdxplY7Q

淘宝搜索/推荐系统背后深度强化学习与自适应在线学习的实践之路

https://mp.weixin.qq.com/s/847h4ITQMtUlZcurJ9Vlvg

深度学习在美团点评推荐平台排序中的运用

https://mp.weixin.qq.com/s/AICgNDyWASx_B8NzWcFTqA

一文综述所有用于推荐系统的深度学习方法

https://mp.weixin.qq.com/s/zSBpqhoyROh74UZEItBanA

基于概率隐层模型的购物搭配推送：阿里巴巴提出新型用户偏好预测模型

http://mp.weixin.qq.com/s/nmLNKscP1qxyv_aoSrwEEw

基于大规模图计算的本地算法对展示广告的行为预测

https://mp.weixin.qq.com/s/8hNkntUauCSeVqc2v0QUqA

人工智能如何帮你找到好歌：探秘Spotify神奇的每周歌单

https://zhuanlan.zhihu.com/p/30720579

推荐中的序列化建模：Session-based neural recommendation

https://mp.weixin.qq.com/s/vpxLTcwenvlIvj5D-8uolg

一天造出10亿个淘宝首页，阿里工程师如何实现？

https://mp.weixin.qq.com/s/lZ4FOOVIxsdKvfW45CYCnA

你看到哪版电影海报，由算法决定：揭秘Netflix个性化推荐系统

http://www.cnblogs.com/qcloud1001/p/7483362.html

深度学习在CTR中应用

http://www.cnblogs.com/qcloud1001/p/7513982.html

常见计算广告点击率预估算法总结

https://mp.weixin.qq.com/s/Q8Mt9B1rzbeWXqIInrzSYQ

使用深度学习构建先进推荐系统：近期33篇重要研究概述

https://mp.weixin.qq.com/s/PlsFxKz_Igorh94Ni-78Hg

融合MF和RNN的电影推荐系统

https://mp.weixin.qq.com/s/JKMOhpLWWlrDzymDDEldXw

深度学习大行其道，个性化推荐如何与时俱进？

https://mp.weixin.qq.com/s/FBzd0x4_A9z-r0f3ZKFGuw

携程个性化推荐算法实践

https://mp.weixin.qq.com/s/Q01jy2RtbpBBHGtvtfhEGA

当机器学习遇到推荐系统，悉尼科技大学Liang Hu博士最新分享

https://mp.weixin.qq.com/s/zBcd2vCYZvb_T7De2QhRew

京东公布基于计算机视觉的电商推荐技术！

https://mp.weixin.qq.com/s/rIZUar6sUZXo2S1JwLrHug

AI研究新利器Etymo，妈妈再也不用担心我找不到论文！

https://zhuanlan.zhihu.com/p/33956907

阿里-搜索团队智能内容生成实践

https://mp.weixin.qq.com/s/9nGjIgwzGG2sXvEzvYEptQ

基于“翻译”的推荐系统方案，加州大学圣地亚哥分校最新工作

https://mp.weixin.qq.com/s/Qupo61cnuO_10sVpRe-wQA

DKN:基于深度知识感知的新闻推荐网络

https://mp.weixin.qq.com/s/9xw-FwWEK9VI2wNzc8EhPA

阿里盖坤：解读阿里深度学习实践，CTR 预估、MLR 模型、兴趣分布网络等

https://zhuanlan.zhihu.com/p/35472804

TransNets: Learning to Transform for Recommendation

https://mp.weixin.qq.com/s/_xAB7-LxKRlH76FVsyezzQ

阿里妈妈资深技术专家刘凯鹏解读基于深度学习的智能搜索营销

https://mp.weixin.qq.com/s/CMZHhxAMno2GlnQCjv0BKg

深度学习在CTR预估中的应用

https://mp.weixin.qq.com/s/nboZ6p_l30L__FJNyz6Ohw

深度学习如何应用在广告、推荐及搜索业务？

https://zhuanlan.zhihu.com/p/36564514

基于深度学习的评论文本表示论文引介

https://mp.weixin.qq.com/s/a-lDJuwFYVNupeNhxyXDkA

跨域社交推荐：如何透过用户社交信息“猜你喜欢”？

https://mp.weixin.qq.com/s/FvuWZPNKcnimAGzsfgVdBQ

阿里妈妈公开全新CVR预估模型

https://mp.weixin.qq.com/s/Ya8RZ3TouAapU_xuvMjjMQ

深度协同过滤：用神经网络取代内积建模

https://mp.weixin.qq.com/s/CyelwdWX4yPVp8o79itonQ

阿里提出联合预估算法JUMP：点击率和停留时长预测效果最优

https://mp.weixin.qq.com/s/HS2zcnA1YSRU8-DrodJ6Qw

淘宝用强化学习优化商品搜索后，总收入能提高2%

https://mp.weixin.qq.com/s/8MQ1G-JWGAw5Ev1Ws3V_ig

IJCAI 2018国际广告算法大赛迁移学习夺冠，中国包揽冠亚季军

https://mp.weixin.qq.com/s/QCGemlo8CYIz6imMde_cmg

推荐系统中的机器学习算法与评估实战

https://mp.weixin.qq.com/s/v8L0E6G-ShOiyvaymw8MYg

一文搞懂DeepFM的理论与实践

https://mp.weixin.qq.com/s/u4onsUU4G4DNm4F5ytiNhQ

互联网广告CTR预估新算法：基于神经网络的DeepFM原理解读

https://mp.weixin.qq.com/s/QaMjHRZ5fiN2zBJLIeGrSg

阿里推出DeepInsight平台：可视化理解深度神经网络CTR预估模型

https://mp.weixin.qq.com/s/JEOfAXg4nDepMtzPCgElrg

SIGIR 2018最佳论文：深入分析流行度在推荐系统中的作用

https://mp.weixin.qq.com/s/5wdGemYBtkYUvq1n5ioyFg

详解Wide&Deep理论与实践

https://mp.weixin.qq.com/s/pA1SSEnwC884LBZGiH3jhg

基于改进注意力循环控制门，品牌个性化排序升级系统来了

https://mp.weixin.qq.com/s/V6tjQzfzsekXuoXhbXbKSQ

一文搞懂阿里Deep Interest Network

https://mp.weixin.qq.com/s/ro3D_d-ZPP0TasqPr4PgPA

推荐系统特征构建新进展：极深因子分解机模型

https://mp.weixin.qq.com/s/MoUd99mCR3Xyu_FnV8jLKA

机器如何“猜你喜欢”？深度学习模型在1688的应用实践

https://mp.weixin.qq.com/s/O_BYzrw5YAbLkfVDqcmwmw

36页最新《深度学习在推荐系统上的应用》综述论文，209篇参考论文

https://mp.weixin.qq.com/s/frUPjpCQGlZc7Dag_v7r3A

2018年最全的推荐系统干货

https://mp.weixin.qq.com/s/3NStfKyn3rGmLUrgk4b9lQ

想了解推荐系统最新研究进展？请收好这16篇论文

https://mp.weixin.qq.com/s/7I8k3sKZfaGr8vj1WHbS7g

99页推荐系统情感分析教程发布

https://mp.weixin.qq.com/s/5ryEfgQnX-JXCvhwzKSCjA

点击率预估界的“神算子”是如何炼成的？

https://mp.weixin.qq.com/s/npZg46MK5MZ7h9cqvrAYiQ

机器如何猜你所想？阿里小蜜预测平台揭秘

https://zhuanlan.zhihu.com/p/46755166

利用评论文本交互提升推荐系统

https://zhuanlan.zhihu.com/p/47393033

基于知识的新闻推荐

https://mp.weixin.qq.com/s/jHKgKnl-SbZltl76Ln_Fgg

AutoML详解及其在推荐系统中的应用、优缺点

https://mp.weixin.qq.com/s/0FdosHMzKiYL8XCx_XEPFA

双十一疯狂剁手，你知道阿里是如何跟踪用户兴趣演化的吗？

https://zhuanlan.zhihu.com/p/49410727

利用分类学数据来增强序列推荐的能力

https://mp.weixin.qq.com/s/jdRu-cishwV8qBmGLTFJCA

美团“猜你喜欢”深度学习排序模型实践

https://mp.weixin.qq.com/s/u7GJstj5E_7y1rwkUbpAXg

推荐系统的可解释性浅谈

https://mp.weixin.qq.com/s/3j367YCViJtY9iB386p8jA

简洁易用可扩展，一个基于深度学习的CTR模型包

https://mp.weixin.qq.com/s/hxwyVCjK40urSTUZoDsQig

Deep Neural Network for YouTube Recommendations

https://mp.weixin.qq.com/s/fiUcm143ieCe3KkcJ0V5LQ

京东如何在仓储库存部署时保证“啤酒尿裤”的高效履约？

https://mp.weixin.qq.com/s/n9j9QuMe3fIPQFQKk5Tgjw

Airbnb: 深度学习在搜索排序业务中的探索与演进（一）

https://mp.weixin.qq.com/s/ejQW9EI48kJ4pvIlh7H3Dw

Airbnb: 深度学习在搜索排序业务中的探索与演进（二）
