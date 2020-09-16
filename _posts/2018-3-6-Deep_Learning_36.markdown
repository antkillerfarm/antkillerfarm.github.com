---
layout: post
title:  深度学习（三十六）——手势识别, 深度图像压缩, 深度时间序列, 深度树学习
category: DL 
---

* toc
{:toc}

# 手势识别

https://zhuanlan.zhihu.com/p/26630215

浅谈手势识别在直播中的运用

https://zhuanlan.zhihu.com/p/30561160

2017-最全手势识别/跟踪相关资源大列表分享

http://www.sohu.com/a/203306961_465975

浙江大学CSPS最佳论文：使用卷积神经网络的多普勒雷达手势识别

https://www.zhihu.com/question/20131478

我打算只根据手的形状来识别手势。用哪种机器学习算法比较好？

https://www.leiphone.com/news/201502/QM7LdSN874dWXFLo.html

带你了解世界最先进的手势识别技术

https://mp.weixin.qq.com/s/DbvH6jM1VV47xKylbW-pug

掌纹识别近十年进展综述

https://mp.weixin.qq.com/s/mnPh8w3VuG9apprOkugbLA

中科大提出新型连续手语识别框架LS-HAN，帮助“听”懂听障人士

https://mp.weixin.qq.com/s/pUciYFjOKL3ea91fLCy0Yw

基于OpenCV与tensorflow实现实时手势识别

https://mp.weixin.qq.com/s/xtTmPtjCk4FQuQ3RnPZxEg

UC伯克利黑科技：用语音数据预测说话人手势

https://blog.csdn.net/wangyaninglm/article/details/87296595

指纹的对比分析系统概述

https://mp.weixin.qq.com/s/ji8sEzJXp1UNgBHVOui0ng

谷歌开源手势识别器，手机能用，还有现成的 App，但是被我们玩坏了

# 深度图像压缩

Tiny Network Graphics是图鸭科技推出一种基于深度学习的图片压缩技术。由于商业因素，这里没有论文，技术细节也不详，但是下图应该还是有些用的。

![](/images/img2/TNG.png)

还有视频压缩：

论文：

《Deep Learning-Based Video Coding: A Review and A Case Study》

参考：

https://mp.weixin.qq.com/s/YBJwLqqL7aVUTG0LaUbwxw

深度学习助力数据压缩，一文读懂相关理论

https://mp.weixin.qq.com/s/WYsxFX4LyM562bZD8rO95w

图鸭发布图片压缩TNG，节省55%带宽

https://mp.weixin.qq.com/s/meK8UBnVHzA9YspQ2RFp6Q

体积减半画质翻倍，他用TensorFlow实现了这个图像极度压缩模型

https://mp.weixin.qq.com/s/_5tyt7pU0gIXbkmTOVEtDw

嫌图片太大？！卷积神经网络轻松实现无损压缩到20%！

https://mp.weixin.qq.com/s/a4oU8UK_hLMrKXNRQizAag

图鸭科技获CVPR 2018图像压缩挑战赛单项冠军，技术解读端到端图像压缩框架

https://mp.weixin.qq.com/s/VDyPjzXdwMGEsoXQmhrp9g

图鸭科技斩获CVPR图像压缩挑战赛冠军，TNGcnn4p技术全解读

https://mp.weixin.qq.com/s/B7reSwa9sCZqbkYVM5-VOA

图像压缩哪家强？请看这份超详细对比

https://mp.weixin.qq.com/s/K17wlC3tueNBfHkYBUFcQg

基于深度学习的HEVC复杂度优化。这是篇视频压缩的blog。

https://mp.weixin.qq.com/s/exUYS2v5VyRaMdFylWlobw

用循环神经网络进行文件无损压缩：斯坦福大学提出DeepZip

https://mp.weixin.qq.com/s/GEMOfh04XR5IyWWlvZeeng

CLIC图像压缩挑战赛冠军方案解读

https://zhuanlan.zhihu.com/p/78050429

基于深度学习的视频压缩方案介绍

https://mp.weixin.qq.com/s/gNtxBI0Alk70cEujxQmSFQ

如何将图像压缩10倍？阿里工程师有个大胆的想法！这是一篇传统算法的blog。

https://mp.weixin.qq.com/s/OkywKX4XygM8VqkL8A1fcA

TIP 2019开源论文：基于深度学习的HEVC多帧环路滤波方法

https://mp.weixin.qq.com/s/Qod-SHNa-El48_n-w5PCLQ

超越H.265，中科大使用多帧数据改进视频压缩新方法

https://zhuanlan.zhihu.com/p/150340687

可逆图像缩放：完美恢复降采样后的高清图片

# 深度时间序列

https://mp.weixin.qq.com/s/cChJO6YrE_XAgeInS7J1Vw

130页序列推荐系统教程重磅发布

https://www.intechopen.com/books/time-series-analysis-data-methods-and-applications

开源新书《时间序列分析，数据/方法/应用》，6章110页pdf带你了解最新进展

https://mp.weixin.qq.com/s/7c9gfwNjuKD4XyrODnyS5w

时间序列预测：理论与实践，379页ppt阐述大规模时序预测工具与方法

https://mp.weixin.qq.com/s/seZ78Z6W_65T0GR_Jk7s1A

神经网络序列数据建模，229页ppt，Modeling Sequential Data with Neural Nets

https://mp.weixin.qq.com/s/zv264-dqDQYRkYmjX_QZpQ

郑宇解读地理传感器时间序列预测问题

https://mp.weixin.qq.com/s/JwRXBNmXBaQM2GK6BDRqMw

Kaggle网站流量预测任务第一名解决方案：从模型到代码详解时序预测

http://mp.weixin.qq.com/s/9fJT0dMLvYQdfMVvSEiG4A

深度学习的时间序列模型评价

https://mp.weixin.qq.com/s/1Gdx-U3DRZtSJoFyCLnD0w

深度学习之Sequence Learning

https://mp.weixin.qq.com/s/SWKWI7BADnX43e-sCBxRPg

神经网络在算法交易上的应用系列——简单时序预测

https://mp.weixin.qq.com/s/Ux7XjLHzlaGFtM1uSMu2gQ

神经网络在算法交易上的应用系列——时序预测+回测

https://mp.weixin.qq.com/s/fUqQo0m7nMLZO85YG-duRw

神经网络在算法交易上的应用系列——多元时间序列

https://mp.weixin.qq.com/s/PS1SqSWckuHuyZP6d5ZUFw

“深度学习”信号处理和时序分析的最后选择？

https://zhuanlan.zhihu.com/p/54471673

基于前馈神经网络的时间序列异常检测算法

https://mp.weixin.qq.com/s/g9upS70qFOCFBMm-T5nI1A

利用深度学习最新前沿预测股价走势

https://mp.weixin.qq.com/s/otwmDtiDfVVID65RQgT4Uw

教你如何鉴别那些用深度学习预测股价的花哨模型？

https://mp.weixin.qq.com/s/xGUcqs3q3yNpVsJ8P7ag_g

以机器学习的视角来看时序点过程的最新进展

https://mp.weixin.qq.com/s/F0z5aEaigQLtlLfDoFIJXQ

时间序列预测：理论与实践教程，300多页PPT带你了解领域最新动态

https://zhuanlan.zhihu.com/p/83130649

深度学习在时间序列分类中的应用

https://mp.weixin.qq.com/s/iPpVT2iY4Ec6oYdsRpPeTQ

淘宝推荐系统中的深度序列匹配模型SDM

https://zhuanlan.zhihu.com/p/103012005

序列特征的处理方法之一：基于注意力机制方法

https://zhuanlan.zhihu.com/p/104216734

序列特征的处理方法之二：基于卷积神经网络方法

https://mp.weixin.qq.com/s/t-hRxqppW5m6abpvYvZTDg

序列循环神经网络，141页ppt，Sequences and Recurrent Network

https://mp.weixin.qq.com/s/pfQiTjUhgwBNz76sKpij1Q

使用2D卷积技术进行时间序列预测

## TCN

https://mp.weixin.qq.com/s/hE0elaJcywb084rmWZzTAw

告别RNN，迎来TCN！股市预测任务是时候拥抱新技术了

https://mp.weixin.qq.com/s/aQxK6JvoEev_jUXHJEGnhg

时间卷积网络TCN：时间序列处理的新模型

https://mp.weixin.qq.com/s/mIuAn4G9l3AKFAswpbaQdA

时间卷积网络（TCN）将取代RNN成为NLP预测领域王者

https://blog.csdn.net/Kuo_Jun_Lin/article/details/80602776

TCN_时间卷积网络_原理与优势

https://mp.weixin.qq.com/s/iLNuBSwWQ13sc4XfNzPuHw

RNN已老，TCN崛起！李飞飞团队提出口语语音识别新方法

## 时空序列

顾名思义，时空序列问题包含了时间和空间两个方面的因素。

https://mp.weixin.qq.com/s/x2Tpd1-prppwjRla-yvTZw

什么是时空序列问题？这类问题主要应用了哪些模型？主要应用在哪些领域？

https://mp.weixin.qq.com/s/mEt1K5OdJ4IZ7ugxEgWLVQ

Convolutional LSTM Network-paper reading

https://mp.weixin.qq.com/s/hnzjG09DDtiM4GUVIdxWPA

时空序列模型之STGCN

https://mp.weixin.qq.com/s/iiJIHVv84namKHtdvHaMBg

时空序列预测模型之轨迹GRU

https://mp.weixin.qq.com/s/bE_k9D9RFYXKn7dnTBb7AA

时空序列预测模型之CubicLSTM

https://mp.weixin.qq.com/s/XZWReRiIHqykN8yZulBiOg

混合时空图卷积网络：更精准的时空预测模型

https://mp.weixin.qq.com/s/40iqr-Yg4-CGe6QEsLpxhw

时空序列预测模型之LightNet

# 深度树学习

决策树是传统ML领域的王者，对于如何将之深度化，一般有两个方向：

- 树结构的深度化。代表：gcForest。

- 树+DL。一般被称为深度树学习。

## gcForest

http://mp.weixin.qq.com/s/aDKLcITA6TBZDyNmuAU4Bw

周志华教授gcForest（多粒度级联森林）算法预测股指期货涨跌

https://mp.weixin.qq.com/s/GU9-rH0gFan620Jhc1HTDg

周志华提出的gcForest能否取代深度神经网络？

https://mp.weixin.qq.com/s/dEmox_pi6KGXwFoevbv14Q

周志华：首个基于森林的自编码器，性能优于DNN

http://mp.weixin.qq.com/s/IfEgSOIkIPA-YtC9NQW1ng

非神经网络的深度模型gcForest

https://mp.weixin.qq.com/s/N80l9PZQposbIOKXbv8ayw

周志华：最新实验表明gcForest已经是最好的非深度神经网络方法

https://mp.weixin.qq.com/s/8QP5X9Hxi_6qyfxP4O0Gwg

周志华团队和蚂蚁金服合作：用分布式深度森林算法检测套现欺诈

https://mp.weixin.qq.com/s/bE9BZQ6wCICvrgomdySDuw

周志华组提出可做表征学习的多层梯度提升决策树

https://mp.weixin.qq.com/s/AwvSTF8j0AinS-EgmPFJTA

周志华团队：深度森林挑战多标签学习，9大数据集超越传统方法

## 深度树学习

https://mp.weixin.qq.com/s/GO7bXBY0cVfGIEEAtp0sKg

什么时候以及为什么基于树的模型可以超过神经网络模型？

https://mp.weixin.qq.com/s/bjOVQu0FZyTWQRlwEn8IVA

基于深度树学习的Zero-shot人脸检测识别

https://mp.weixin.qq.com/s/pWcFuOecG-dZHZ365clDjg

阿里妈妈新突破！深度树匹配如何扛住千万级推荐系统压力

https://mp.weixin.qq.com/s/sw16_sUsyYuzpqqy39RsdQ

阿里妈妈深度树检索技术（TDM）及应用框架的探索实践

https://mp.weixin.qq.com/s/EFDmHH8oUmJk-rG5PNnsAg

阿里妈妈深度树匹配技术演进：TDM->JTM->BSAT

https://mp.weixin.qq.com/s/6r8y7tMqo53lnACWG1K4xA

深度树学习用于Zero-shot人脸的反欺诈

https://mp.weixin.qq.com/s/NBVPlFGO12PhMTF0dUL2hw

DeepGBM:使用树蒸馏提升在线预测任务下深度模型效果
