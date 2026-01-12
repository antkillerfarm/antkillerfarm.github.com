---
layout: post
title:  深度学习（四十四）——多标签学习, 多模态学习, 深度贝叶斯学习
category: DL 
---

* toc
{:toc}

# 多标签学习

多标签分类问题（也称细粒度分类），通常有两种解决方案，即转换为多个单标签分类问题，或者直接联合研究。前者，可以训练多个分类器，来判断该维度属性的是否，损失函数常使用softmax loss。后者，则直接训练一个多标签的分类器，所使用的标签为0,1,0,0…这样的向量，使用hanmming距离等作为优化目标。

参考：

https://mp.weixin.qq.com/s/sdQ0rWbDDMN_P0B_RiYZmw

分段映射：帮助利用少量样本习得新类别细粒度分类器

https://mp.weixin.qq.com/s/zeN7rjmAnvh_7BbTmScrZw

细粒度分类你懂吗？——fine-gained image classification

https://mp.weixin.qq.com/s/SCsdWLrBDAKzc9NLAK1jxQ

最新综述：多标签学习的新趋势

https://mp.weixin.qq.com/s/LtWMGRBk2sbPDjeC9PmJ7g

弱监督学习下的商品识别：CVPR 2018细粒度识别挑战赛获胜方案简介

https://mp.weixin.qq.com/s/hcoAL1AHm_HtderWU8fSBw

大连理工大学在CVPR18大规模精细粒度物种识别竞赛中获得冠军

https://mp.weixin.qq.com/s/31r9FjuJn9yxrZMnfozkMQ

全卷积注意网络的细粒度识别

https://zhuanlan.zhihu.com/p/24738319

“见微知著”——细粒度图像分析进展综述

https://zhuanlan.zhihu.com/p/42067661

CVPR Look Closer to See Better

https://mp.weixin.qq.com/s/52hm3Cq3TFRnTMfDppivSQ

中山大学等提出HSE：基于层次语义嵌入模型的精细化物体分类

https://zhuanlan.zhihu.com/p/48192930

Object-Part Attention Model for FGVC

https://mp.weixin.qq.com/s/slmod5rW4qRhxGnbNN2J8g

双线性汇合(bilinear pooling)在细粒度图像分析及其他领域的进展综述

https://mp.weixin.qq.com/s/JGQdHS_yqkOMrN_Z3jEb7A

基于深度学习的细粒度图像分类综述

https://mp.weixin.qq.com/s/L-1gkElxsMtT369fgJl86Q

旷视南京研究院魏秀参：细粒度图像分析综述

https://zhuanlan.zhihu.com/p/57086099

细粒度识别之Local Attention Network

https://mp.weixin.qq.com/s/6K4tXPlYLaXhexh6gElP5Q

多标签图像分类综述

https://mp.weixin.qq.com/s/bb3ZsXtiRmPvzQ-lfSXrZQ

基于Pascal VOC2012增强数据的多标签图像分类实战

https://mp.weixin.qq.com/s/2pJt9hlUFhR6mo1ughKkiA

超全深度学习细粒度图像分析：项目、综述、教程一网打尽

https://mp.weixin.qq.com/s/jyIrREnJQv4mW-H9ghO7_A

细粒度图像分类是什么，有什么方法，发展的怎么样

https://mp.weixin.qq.com/s/5Y4sQlt6DvgkAYtncByjzw

基于Pytorch的细粒度图像分类实战

https://mp.weixin.qq.com/s/232DjhM5sqWqPTv7PCaORA

ElementAI提出超复杂多尺度细粒度图像分类Attention模型

https://mp.weixin.qq.com/s/G-4w5jMuN-_zVARPeb0cqA

细粒度实体分类论文综述

https://mp.weixin.qq.com/s/FcSzjphpsWCB-nrtbjs4gg

如何掌握好图像分类算法？

https://mp.weixin.qq.com/s/IeLYy0Pp3HC_UujA0KYn1Q

多标签长尾识别前沿进展

https://mp.weixin.qq.com/s/m3sgoG15dtacGt1_Anrq6Q

使用NTS理解细粒度图像分类

https://mp.weixin.qq.com/s/6fcqXac7ihAeDuwzl_MxPQ

“神奇的”标签增强技术（Label Enhancement）

https://mp.weixin.qq.com/s/uLyllVhO-U5RrT4Q5XpiLA

细粒度多标签分类

https://mp.weixin.qq.com/s/IhPavQZmXIxxUzNSnnFCKg

南理工最新“深度学习细粒度图像分析”综述论文，带你全面了解细粒度图像识别与检索方法

# 多模态学习

## 参考

https://github.com/HuaizhengZhang/Awsome-Deep-Learning-for-Video-Analysis

深度学习视频分析/多模态学习资源大列表

https://mp.weixin.qq.com/s/ruRkqBEdyj2Dx0WTO5Jhcw

多模态学习研究进展综述

https://mp.weixin.qq.com/s/g3rwPsusYi7gQopOHvdNrA

多模态学习调研

https://mp.weixin.qq.com/s/xzeNAuuDt_VLHDgvIkc-Mg

多模态情感分析简述

https://mp.weixin.qq.com/s/vpBPkjuCebSWh5qPLYHCkw

上海交大提出多模态框架“EmotionMeter”，更精准地识别人类情绪

https://mp.weixin.qq.com/s/BBg04rDtiqU-XrWortufNA

康奈尔&英伟达提出多模态无监督图像转换新方法

http://mp.weixin.qq.com/s/khOINUyrNV3TFfgNRheH0A

卷积神经网络压缩、多模态的语义分析研究

https://mp.weixin.qq.com/s/ywU4L659iRcmIgmV6RtbXA

DeepMind新研究连接听与看，实现“听声辨位”的多模态学习

https://mp.weixin.qq.com/s/1qhcyTXttgKWlw-Oy556Tw

TPAMI2019最新《多模态机器学习综述》

https://mp.weixin.qq.com/s/BczgUuh2FIvP5MG9xh87wQ

多模态多任务学习新论文

https://zhuanlan.zhihu.com/p/427323610

多模态中预训练的演变史

https://mp.weixin.qq.com/s/ipj8qpYRiYbIeXn2PZb1SQ

5G时代下多模态理解做不到位注定要掉队

https://mp.weixin.qq.com/s/UghgWBN7mE8oJdMUvjAjcQ

何晖光：多模态情绪识别及跨被试迁移学习

https://mp.weixin.qq.com/s/EMWpBP5iB1Qrleo3XNjbuQ

IEEE Fellow何晓东&邓力：多模态智能论文综述：表示学习，信息融合与应用

https://mp.weixin.qq.com/s/Yus55s1utTrjuzsrebJu_w

让机器读懂视频：亿级淘宝视频背后的多模态AI算法揭秘

https://mp.weixin.qq.com/s/4AzF6utrQhhEweRIM6zV3A

文本+视觉，跨模态预训练新进展

https://mp.weixin.qq.com/s/dG7Lr5fdmqJQaYOWgkk8iw

如何构建多模态BERT?这份UNC76页《LXMERT: 从Transformer学习跨模态编码表示》PPT告诉您

https://mp.weixin.qq.com/s/QIJ2c4L7KfjVEhIyKayJ-Q

阿里文娱多模态视频分类算法中的特征改进

https://mp.weixin.qq.com/s/THxlQX2MPXua0_N0Ug0EWA

BERT在多模态领域中的应用

https://mp.weixin.qq.com/s/GxQ27vY5naaAXtp_ZTV0ZA

通用的图像-文本语言表征学习：多模态预训练模型 UNITER

https://mp.weixin.qq.com/s/rjWOkwzX3IE59Kc9P9leAQ

格“物”致知：多模态预训练再次入门

https://mp.weixin.qq.com/s/0CUGispeZS04D6NhGkrucw

多模态深度学习：用深度学习的方式融合各种信息

https://mp.weixin.qq.com/s/Tli19SOum_muBoBaTtXKUQ

多模态中NLP与CV融合的一些方式

https://mp.weixin.qq.com/s/ondgiFryYqB6-sf-v4pLXQ

多模态预训练模型简述

https://mp.weixin.qq.com/s/TFHS5lZYFwcjP_SC1dAckA

多模态信息如何嵌入推荐系统？RecSys2021《多模态推荐系统》教程

# 深度贝叶斯学习

https://mp.weixin.qq.com/s/4sDNUZiOiS6VH_oRSnW6HQ

牛津大学YARIN GAL《贝叶斯深度学习》入门教程，336页ppt

https://mp.weixin.qq.com/s/w_phnVwm13P8dU0Ks-b-YA

《贝叶斯深度学习: DL与Bayesian原理 》NeurIPS2019硬核教程

https://mp.weixin.qq.com/s/J_sbJb8i-O8CwhHvVyPv1w

《深度贝叶斯数据挖掘》，附257页PPT下载

https://mp.weixin.qq.com/s/cDqxmRVQCIqdM5oiUh82YQ

Yee Whye Teh：《贝叶斯深度学习与深度贝叶斯学习》

https://mp.weixin.qq.com/s/JZVl0kygVawdW8qflPps6g

《神经贝叶斯信息处理》教程，220页ppt，国立交通大学

https://mp.weixin.qq.com/s/Zk2YG-IJNhJxTBU8THSM-g

让DL可解释？这一份66页贝叶斯深度学习教程告诉你

https://mp.weixin.qq.com/s/-izo9VUdxN33pwVFGV_tjw

299页PPT带你回顾深度贝叶斯学习最新发展脉络

https://mp.weixin.qq.com/s/UiLyQKhIe2rDYiwPcqyqaw

可跟踪概率模型，209页最新教程

https://mp.weixin.qq.com/s/pHAbxeYBI2q6pUHNrAt1og

贝叶斯学习与未来人工智能

https://mp.weixin.qq.com/s/Zd4rFU7Lebr4zmzxThNyVw

详解珠算：清华大学开源的贝叶斯深度学习库

https://mp.weixin.qq.com/s/RpaOrngeXTKycLb3iCygZw

利用贝叶斯神经网络进行随机动力系统中的学习与策略搜索

https://github.com/bayesgroup/deepbayes-2018

Seminars DeepBayes Summer School 2018

https://mp.weixin.qq.com/s/WCRYppBLdl_M4etUChnfgw

PyMC3和Theano代码构建贝叶斯深度网络

https://mp.weixin.qq.com/s/7mwJpQFWWXJ3dvTAwDFI7Q

贝叶斯卷积神经网络：架起深度学习与统计学的桥梁

https://mp.weixin.qq.com/s/2LkpuchuHs82Sxs5rD8bWA

《深度贝叶斯与序列学习》，279页PPT带你知晓深度贝叶斯序列模型在NLP最新进展

https://zhuanlan.zhihu.com/p/74573041

针对推荐系统的深度贝叶斯多目标学习

https://mp.weixin.qq.com/s/b041h_hbHQYiXCiDHGaD5w

深度贝叶斯自然语言处理，304页ppt带你了解最新研究进展

https://zhuanlan.zhihu.com/p/77140176

构建贝叶斯深度学习分类器

https://mp.weixin.qq.com/s/0e4GHNRCF9xKFELAZ4zRFA

A simple tutorial for Bayesian neural network

https://mp.weixin.qq.com/s/NkRemHPRnEcEwbs5b-Mz9w

贝叶斯编程，378页pdf，Bayesian Programming

https://mp.weixin.qq.com/s/DDg4HTp-APwEIul1ZaFFPQ

最新《贝叶斯推断》教程，125页ppt与视频，DeepMind Shakir Mohamed博士

https://zhuanlan.zhihu.com/p/283633149

Bayesian Deep Learning最新研究总结

# 姿态/行为检测进阶+

https://mp.weixin.qq.com/s/skCOaKf9kRABTX7hkdjGXA

谷歌极速人脸、手、人体姿态分析Blaze算法家族

https://zhuanlan.zhihu.com/p/69042249

2020 Pose Estimation人体骨骼关键点检测综述笔记

https://zhuanlan.zhihu.com/p/164603050

3D Pose Estimation关键点检测的算法整理（2020）

https://mp.weixin.qq.com/s/AW-L_5acaDzGTObUGjTuuw

用AI“驯服”人类幼崽：这个奶爸找到了硬核带娃的乐趣

https://mp.weixin.qq.com/s/P4FxL2jAXaJYZ0ZTY8xtzg

深度学习人体姿态估计：2014-2020全面调研

https://zhuanlan.zhihu.com/p/414173365

记一次坎坷的算法需求实现：轻量级人体姿态估计模型的修炼之路（附MoveNet复现经验）

https://mp.weixin.qq.com/s/zzFb55Yj9j3x_pxeXRlOaw

在线试玩，在体感游戏中打败泰森，这位小哥破解了任天堂“拳无虚发”（MoveNet）

# 数学杂谈+

对于三阶魔方，单说位置的话，还原状态下所有块的位置都是固定不变的，只是中心块的方向可能会不一样（原地旋转）。

https://www.zhihu.com/question/346942533

每一次拼好魔方后小方块的位置都是固定的吗？

---

如果n个人完全随机分n块蛋糕，当人群n足够大时，一个蛋糕都没有的人的比例大约为1/e。
