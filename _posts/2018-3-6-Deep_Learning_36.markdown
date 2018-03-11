---
layout: post
title:  深度学习（三十六）——深度推荐系统, Recursive NN, Spatial Transformer Networks, SOM, 多任务学习
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

## 工具

https://www.librec.net/

这是一个Java写的推荐系统。东北大学的郭贵冰主持的项目。该网址同时也有不少相关论文可供阅读。

## 参考

https://zhuanlan.zhihu.com/p/26237106

深度学习在推荐算法上的应用进展

http://i.dataguru.cn/mportal.php?mod=view&aid=11463

深度学习在推荐领域的应用

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

# Recursive NN

http://blog.csdn.net/qq_26609915/article/details/52119512

递归神经网络（recursive NN）结合自编码（Autoencode）实现句子建模

http://blog.csdn.net/mengmengz07/article/details/51348554

recursive neural network梳理

# Spatial Transformer Networks

论文：

《Spatial Transformer Networks》

参考：

http://www.cnblogs.com/neopenx/p/4851806.html

Spatial Transformer Networks(空间变换神经网络)

http://blog.csdn.net/shaoxiaohu1/article/details/51809605

论文笔记：Spatial Transformer Networks

http://blog.csdn.net/shaoxiaohu1/article/details/51809605

Spatial Transformer Networks

https://mp.weixin.qq.com/s/ciqQMezcB-oM24X8eQqTNg

花式玩耍Spatial Transformation Networks

# SOM

http://www.cnblogs.com/sylvanas2012/p/5117056.html

Self Organizing Maps (SOM): 一种基于神经网络的聚类算法

http://blog.csdn.net/Loyal2M/article/details/11225987

聚类算法实践（3）——PCCA、SOM、Affinity Propagation

# 多任务学习

https://mp.weixin.qq.com/s/guAgXdhZSbEAkERSB1sLRA

多任务学习-Multitask Learning概述

https://mp.weixin.qq.com/s/ZlCI02UdRuFBc-uKqIPE_w

深度学习多任务学习综述

https://mp.weixin.qq.com/s/QXOy2jo4RhCZrD5bSVzBOQ

共享相关任务表征，一文读懂深度神经网络多任务学习

https://mp.weixin.qq.com/s/mm9bXXTEzd8DwyYlMgGMZg

NLP多任务学习：一种层次增长的神经网络结构

https://mp.weixin.qq.com/s/X6FwTgr282hbqgOz3oBX-w

多任务学习概述论文：从定义和方法到应用和原理分析


