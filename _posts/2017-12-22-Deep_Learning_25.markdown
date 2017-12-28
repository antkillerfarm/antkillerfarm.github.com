---
layout: post
title:  深度学习（二十五）——深度推荐系统
category: DL 
---

# 深度推荐系统

推荐系统一直是AI能够落地且商业前景很好的一个研究方向。自2016年以来，该方向也逐渐被DL所侵蚀，尽管目前从招聘来说，这方面的职位仍以普通ML为主。

2017年5月，我曾面试了一家电商企业。当时给我的感觉，虽然里面的工程师较早接触ML，然而知识老化现象比较严重，对最基本的神经网络知识缺乏必要的了解。这显然给了后来者一个弯道超车的好机会。

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

这是一个Java写的推荐系统。

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

# NN的INT8计算

## 概述

NN的INT8计算是近来NN计算优化的方向之一。这方面的文章以Xilinx的白皮书较为经典：

https://china.xilinx.com/support/documentation/white_papers/c_wp486-deep-learning-int8.pdf

利用Xilinx器件的INT8优化开展深度学习

论文：

《On the efficient representation and execution of deep acoustic models》

参考：

https://www.chiphell.com/thread-1620755-1-1.html

新Titan X的INT8计算到底是什么鬼

https://mp.weixin.qq.com/s/S9VcoS_59nbZWe_P3ye2Tw

减少模型半数内存用量：百度&英伟达提出混合精度训练法

## NN硬件的指标术语

MACC：multiply-accumulate，乘法累加。

FLOPS：Floating-point Operations Per Second，每秒所执行的浮点运算次数。

显然NN的INT8计算主要以MACC为单位。

# NLP参考资源

https://mp.weixin.qq.com/s/7yi67lPjseigeaqTD269mQ

触类旁通，专业技能热度智能分析

https://mp.weixin.qq.com/s/Zo6LjGE__vQMsWyuoZm6Hw

文本特征工程之N-Gram

https://mp.weixin.qq.com/s/PmdARJQC8O42huzxZNSz3A

搭建基于依存句法和短语结构句法结合的金融领域事件元素抽取系统实践

http://www.cnblogs.com/qcloud1001/p/7910255.html

LSF-SCNN：一种基于CNN的短文本表达模型及相似度计算的全新优化模型

http://www.cnblogs.com/qcloud1001/p/8074771.html

神经张量网络（NTN）：探索文本实体之间的关系

https://mp.weixin.qq.com/s/Y-skeJvkWlgkwKBpCjPaKA

中文文本挖掘流程详解

https://zhuanlan.zhihu.com/p/32314500

DL实战课程推荐-从0到1构建一个Chatbot系统

https://mp.weixin.qq.com/s/mNG9P6hvbRIGq7f5Jx4jIw

Facebook开源无监督机器翻译模型和大规模训练语料

https://mp.weixin.qq.com/s/GhJRebSXXq0ZFh3evAqLxw

漫谈神经语言模型之中文输入法

https://mp.weixin.qq.com/s/HMLwjLlFXR4WM92_OnKHbA

基于Apache MXNet，亚马逊NMT开源框架Sockeye论文介绍

https://mp.weixin.qq.com/s/rF4zKCbj3Jh8dDR1239lpA

基于双语主题模型的跨语言层次分类体系匹配

https://mp.weixin.qq.com/s/gQ9dV-IPWHTOLbI6u0D67g

可解释推荐系统：身怀绝技，一招击中用户心理

https://mp.weixin.qq.com/s/M_ZN0YuvegAnc2GXCZTt9Q

初学者指南：神经网络在自然语言处理中的应用

https://mp.weixin.qq.com/s/ouNxUvWC_mxap6P5z6_dFA

教聊天机器人进行多轮对话

https://mp.weixin.qq.com/s/fZv9FgbdQ1bWPoNdl9sF1A

“宝石迷阵”与信息检索

http://www.jianshu.com/p/0273c377c34e

机器学习算法在文本分类中的应用综述

https://mp.weixin.qq.com/s/BcUB3bXrPJF0WQetWs7Law

用深度学习解决自然语言处理中的7大问题，文本分类、语言建模、机器翻译等

https://mp.weixin.qq.com/s/GyE9qdXPGvrq12dMAf4nrQ

论文推荐：QA，增强学习，知识图谱，机器阅读理解

https://mp.weixin.qq.com/s/eCtqMIo3_UDxAR4fWzMjZQ

站在锤子手机背后，小源科技用AI打造短信场景服务

https://mp.weixin.qq.com/s/hkcVhyqiDW--F3QxucgEcA

亚马逊开源神经机器翻译框架Sockeye：基于Apache MXNet的NMT平台

https://mp.weixin.qq.com/s/vgejlnsBlOKMMRt-M3-d-w

深思考：实现人机多轮交互突破是攻克图灵测试的核心

https://mp.weixin.qq.com/s/rqB2COCAKsr3qvVXnhLwMg

数据到文本任务的近期相关工作介绍

https://mp.weixin.qq.com/s/KWWbaE2em4fEBInPoWTq8A

Python实现多种模型(Naive Bayes, SVM, CNN, LSTM, etc)用于推文情感分析

https://mp.weixin.qq.com/s/-K3WIo64_wJb9p6C49Z5fA

基于属性学习和额外知识库的图像描述生成和视觉问答


