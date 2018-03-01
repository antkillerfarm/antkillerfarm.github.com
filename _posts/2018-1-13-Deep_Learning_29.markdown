---
layout: post
title:  深度学习（二十九）——迁移学习, Spiking Neuron Networks, DNC, OCR
category: DL 
---

# 迁移学习

https://mp.weixin.qq.com/s/HmkTkv7QT08lGtJsHD7EvQ

迁移学习（Transfer Learning）技术概述

https://zhuanlan.zhihu.com/wjdml

《小王爱迁移》系列blog

https://mp.weixin.qq.com/s/BWBrso7O1O3Rfxa4QWZH4g

分分钟学会基于深度学习的图像真实风格迁移！

https://mp.weixin.qq.com/s/5DtTgc9bIrdXQkmuqRm8CA

谷歌大脑迁移学习：减少调参，直接在数据集中学习最佳图像架构

https://mp.weixin.qq.com/s/fEKc6yFZwTPAHjXJlcHA-w

香港科技大学提出L2T框架：学习如何迁移学习

https://mp.weixin.qq.com/s/pbyByPoZ9SVoP9B7pJMxXg

深度卷积网络迁移学习的脸部表情识别

https://www.zhihu.com/question/50996014

什么是One/zero-shot learning？

https://mp.weixin.qq.com/s/SZlFgnUBL0T6yNa-i_WLvg

领域适应性Domain Adaptation、One-shot/zero-shot Learning概述

https://mp.weixin.qq.com/s/sAf2fLLnKHOs433pV_6bSQ

One-shot Learning：孪生网络少样本精准分类

https://mp.weixin.qq.com/s/J8ZmIVKd-4X3hMGGIJWoDQ

一文看懂迁移学习：从基础概念到技术研究！

https://mp.weixin.qq.com/s/qYoTgqwjaUlEycuk9LlonA

迁移学习：6张图像vs13000张图像，超越2013 Kaggle猫狗识别竞赛领先水平

http://mp.weixin.qq.com/s/6Urv6TfUfc-BWV1YqTM1PQ

迁移学习+BPE，改进低资源语言的神经翻译结果

https://zhuanlan.zhihu.com/p/30242073

人脸识别中的迁移学习简介（Transfer Learning）

https://mp.weixin.qq.com/s/rVYWV-LsbmA4QhC6207SWA

14篇论文为你呈现“迁移学习”研究全貌

https://mp.weixin.qq.com/s/l-l1xbUaPNKc-w5XndjCbQ

通过网络结构迁移学习提高图像识别任务的拓展性

https://mp.weixin.qq.com/s/-KssC3yXsG3ZuV8-I6D_nQ

学习迁移架构用于Scalable图像的识别

https://mp.weixin.qq.com/s/NQED6DdCJNpNyzURUOZPnA

迁移学习：机器学习的下一个前沿阵地！

https://mp.weixin.qq.com/s/Hok9D8dAzYrBz7XoFmGE2A

AliExpress：在检索式问答系统中应用迁移学习

https://mp.weixin.qq.com/s/f_vB2AXCytnvoZaqfMeIpw

应用TF-Slim快速实现迁移学习

https://mp.weixin.qq.com/s/R1bKmhADfhQAZmhXL9ObiQ

多重预训练视觉模型的迁移学习

https://mp.weixin.qq.com/s/pDK4qBWArtETARE1fjbbmA

迁移学习在深度学习中的应用

https://mp.weixin.qq.com/s/mB1AEFVdM_s1rk0irST4Ww

迁移学习在图像分类中的简单应用策略

https://mp.weixin.qq.com/s/PDyp_GO0ovWV0KoGTwp_gQ

简述迁移学习在深度学习中的应用

https://mp.weixin.qq.com/s/FHmijTVqQ26osp6PzZsbvQ

付彦伟：零样本、小样本以及开集条件下的社交媒体分析

# Spiking Neuron Networks

除了基于BP算法的NN之外，Spiking Neuron Networks也是一大类NN。Spiking NN和人脑结构更相似，功耗也更小，但是相关训练和数据量化的算法尚不成熟，属于潜力股。

参考：

https://homepages.cwi.nl/~sbohte/publication/paugam_moisy_bohte_SNNChapter.pdf

Computing with Spiking Neuron Networks

https://mp.weixin.qq.com/s/6dpKSaLFVo-ge4gtbG8GQg

简述脉冲神经网络SNN：下一代神经网络

# DNC

https://zhuanlan.zhihu.com/p/27773709

浅析至强RNN可微分神经计算机(DNC)

https://zhuanlan.zhihu.com/p/27964341

浅析至强RNN可微分神经计算机(DNC)-2

https://zhuanlan.zhihu.com/p/28209628

DNC-3滚动分类的模式识别

https://zhuanlan.zhihu.com/p/28433712

DNC4广义线性回归

# OCR

## tesseract

linux下可以使用tesseract作为OCR工具。安装方法：

`sudo apt install tesseract-ocr libtesseract-dev`

使用方法：

`tesseract ./111.png 1 -l chi_sim+eng`

## 参考

https://mp.weixin.qq.com/s/h7HVyGbmtLmNVJp4p0rCRQ

字符识别(OCR)相关工具/库/教材/论文等资源整理

https://zhuanlan.zhihu.com/p/21344595

端到端的OCR：验证码识别(LSTM+CTC)

http://www.jianshu.com/p/86489f1afd36

端到端的OCR：基于CNN的实现

http://www.jianshu.com/p/4fadf629895b

端到端的OCR：LSTM＋CTC的实现

https://mp.weixin.qq.com/s/axpA7Y_Rhiols5bDIdc6jg

Tesseract-OCR 3.0.1训练自己的语言库之图像文字识别

http://mp.weixin.qq.com/s/n8C80a3B54FhrCe-GhhcDA

文档扫描：深度神经网络在移动端的实践

https://mp.weixin.qq.com/s/MYhQt9uC16BadiZKWjPTzA

华中科技大学提出多向文本检测方法：基于角定位与区域分割

