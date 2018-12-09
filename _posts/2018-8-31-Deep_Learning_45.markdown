---
layout: post
title:  深度学习（四十五）——Attention进阶, AutoDL, 多模态学习
category: DL 
---

# Attention进阶

https://mp.weixin.qq.com/s/y_hIhdJ1EN7D3p2PVaoZwA

阿里北大提出新attention建模框架，一个模型预测多种行为

https://mp.weixin.qq.com/s/Yq3S4WrsQRQC06GvRgGjTQ

打入神经网络思维内部

https://mp.weixin.qq.com/s/MJ1578NdTKbjU-j3Uuo9Ww

基于文档级问答任务的新注意力模型

https://mp.weixin.qq.com/s/C4f0N_bVWU9YPY34t-HAEA

UNC&Adobe提出模块化注意力模型MAttNet，解决指示表达的理解问题

https://mp.weixin.qq.com/s/V3brXuey7Gear0f_KAdq2A

基于注意力机制的交易上下文感知推荐，悉尼科技大学和电子科技大学最新工作

http://mp.weixin.qq.com/s/Bt6EMD4opHCnRoHKYitsUA

结合人类视觉注意力进行图像分类

https://mp.weixin.qq.com/s/POYTh4Jf7HttxoLhrHZQhw

基于双向注意力机制视觉问答pyTorch实现

https://mp.weixin.qq.com/s/2gxp7A38epQWoy7wK8Nl6A

谷歌翻译最新突破，“关注机制”让机器读懂词与词的联系

https://zhuanlan.zhihu.com/p/25928551

用深度学习（CNN RNN Attention）解决大规模文本分类问题-综述和实践

http://blog.csdn.net/leo_xu06/article/details/53491400

视觉注意力的循环神经网络模型

https://mp.weixin.qq.com/s/l4HN0_VzaiO-DwtNp9cLVA

循环注意力区域实现图像多标签分类

https://mp.weixin.qq.com/s/zhZLK4pgJzQXN49YkYnSjA

自适应注意力机制在Image Caption中的应用

https://mp.weixin.qq.com/s/uvr-G5-_lKpyfyn5g7ES0w

基于注意力机制，机器之心带你理解与训练神经机器翻译系统

https://mp.weixin.qq.com/s/ANpBFnsLXTIiW6WHzGrv2g

自注意力机制学习句子embedding

https://mp.weixin.qq.com/s/49fQX8yiOIwDyof3PD01rA

CMU&谷歌大脑提出新型问答模型QANet：仅使用卷积和自注意力，性能大大优于RNN

https://mp.weixin.qq.com/s/c64XucML13OwI26_UE9xDQ

滴滴披露语音识别新进展：基于Attention显著提升中文识别率

https://mp.weixin.qq.com/s/7OYY3L7gL4wVv_EjoosOHA

如何增强Attention Model的推理能力

https://mp.weixin.qq.com/s/9Kt6_DfeYRnhsb10aCSFGw

FAGAN：完全注意力机制（Full Attention）GAN，Self-attention+GAN

https://mp.weixin.qq.com/s/lZOIK5BRXZrmL_Z9crl6sA

机器翻译新突破！“普适注意力”模型：概念简单参数少，性能大增

https://mp.weixin.qq.com/s/JoTzaInn_uAA9oZgMcfskw

计算机视觉技术self-attention最新进展

https://mp.weixin.qq.com/s/jRfOzKO6OlQLokIzipbqUQ

为什么使用自注意力机制？

https://mp.weixin.qq.com/s/h7sLwVXb_UI8jvJU-oe3Cg

Google AI提出“透明注意力”机制，实现更深层NMT模型

https://mp.weixin.qq.com/s/1LYz5SH5rVnPPJ0tZvRQAA

从各种注意力机制窥探深度学习在NLP中的神威

https://zhuanlan.zhihu.com/p/32928645

计算机视觉中的注意力机制

https://zhuanlan.zhihu.com/p/33078323

数字串识别：基于位置的硬性注意力机制

https://zhuanlan.zhihu.com/p/32971586

图像描述：基于项的注意力机制

https://zhuanlan.zhihu.com/p/33158614

图像识别：基于位置的柔性注意力机制

# AutoDL

DL领域目前存在的主要问题之一是：如何设计网络结构和调整超参数。目前的做法，通常依赖于作者的直觉，属于典型的拍脑袋想点子。

既然AI已经能够做很多事了，那么有没有可能，使用AI自动生成网络结构呢？

Google的这两篇论文在这里做了一些尝试：

《Neural Architecture Search With Reinforcement Learning》

《Learning Transferable Architectures for Scalable Image Recognition》

这里主要采用强化学习的方法，在一个广阔的搜索空间中，寻找最合适的网络结构。但对于计算能力提出了很高的要求。论文中提到，他们使用了500块GPU。**有钱真的是可以为所欲为的。**

这里学到的模型，一般被称为NASNet。

## 工具

https://mp.weixin.qq.com/s/9OZiFgziY8fMn-lkmdtiqQ

终结谷歌AutoML的真正杀手！Saleforce开源TransmogrifAI

https://mp.weixin.qq.com/s/GFjYzxf6IdKPvkij8-i_Dg

调参工要凉？微软重磅开源AutoML工具包NNI

https://mp.weixin.qq.com/s/L1nkhc4I6VX2s6S5Tiv0Zw

小白也能搭建深度模型，百度EasyDL的背后你知多少

![](/images/img2/AdaNet.gif)

https://mp.weixin.qq.com/s/HiD-OqAz67cwwchjSyIjWA

AutoML又一利器来了，谷歌宣布开源AdaNet

https://mp.weixin.qq.com/s/o3WqVIxlsukt3_JcY-10LA

AdaNet简介：快速灵活的 AutoML，提供学习保证

## 书籍

https://mp.weixin.qq.com/s/wTwjbripELzqdCjs64Dzzw

告别调参，AutoML新书发布

## 参考

https://mp.weixin.qq.com/s/Ia-8qFLAyY65Nai5PGAw0w

人人都能用的深度学习：当前三大自动化深度学习平台简介

http://blog.csdn.net/u014380165/article/details/78525687

自学网络结构（二）：Learning Transferable Architectures for Scalable Image Recognition

http://blog.csdn.net/u014380165/article/details/78525500

自学网络结构（一）：Neural Architecture Search With Reinforcement Learning

https://www.zhihu.com/question/67477086

如何评价Google最新的论文NASNet？

https://mp.weixin.qq.com/s/Yh8Zj7Jhliy7a9h6mW8FxQ

解读谷歌NASNet：一个大规模图像识别架构!

https://mp.weixin.qq.com/s/SwJs0OUzBlhiIOyFHFBK_g

一文看懂JeffDean等提出的ENAS到底好在哪？

https://mp.weixin.qq.com/s/MkXBtGq4xt5YOh1-uhMBbg

循环神经网络自动生成程序：谷歌大脑提出“优先级队列训练”

https://mp.weixin.qq.com/s/D0HngY-U7_fP4vqDIjvaew

自动选模型+调参：谷歌AutoML背后的技术解析

https://mp.weixin.qq.com/s/vctbsYk4LRwrQ7_Hs7fqkg

谷歌大脑发布神经架构搜索新方法：提速1000倍

https://mp.weixin.qq.com/s/tWO1Qv1aoemC8nHvJXZyeA

清华大学张长水教授：神经网络模型的结构优化

https://mp.weixin.qq.com/s/9qpZUVoEzWaY8zILc3Pl1A

进化算法+AutoML，谷歌提出新型神经网络架构搜索方法

https://mp.weixin.qq.com/s/HJ5caV1bQi7qVDeNXZ21qg

手把手教你用Cloud AutoML做毒蜘蛛分类器

https://zhuanlan.zhihu.com/p/35050923

跬步至千里：揭秘谷歌AutoML背后的渐进式搜索技术

https://mp.weixin.qq.com/s/bjVTpdaKFx4fFtT8BjMNew

指数级加速架构搜索：CMU提出基于梯度下降的可微架构搜索方法

https://mp.weixin.qq.com/s/wGt3Xk_ARqHei-ddU9iXNg

算力节省240倍！上交大、MIT新方法低成本达到谷歌AutoML性能

https://mp.weixin.qq.com/s/pXskwKkzCtgIuNtgFVF_wg

ImageNet分类精度再创新高！李飞飞组ECCV Oral提出全新渐进式神经结构搜索

https://mp.weixin.qq.com/s/GLiOZjqC9DSvGEX3xqbHJg

鸡生蛋与蛋生鸡，纵览神经架构搜索方法

https://mp.weixin.qq.com/s/DLpMVOmkvpWqlHIAZojwog

一文看懂深度学习新王者“AutoML”：是什么、怎么用、未来如何发展？

https://mp.weixin.qq.com/s/X6m4ZHDKY4gC-v0YN5g4YA

谷歌云提出渐进式神经架构搜索：高效搜索高质量CNN结构

https://mp.weixin.qq.com/s/0te0JUKYZlSLs3kzsFV-NA

神经网络架构搜索（NAS）综述

https://mp.weixin.qq.com/s/dSAjdx7jsKgI5SQpMY213w

微软&中科大提出新型自动神经架构设计方法NAO

https://mp.weixin.qq.com/s/KvkkmDIY7Y0NBgYF86E2Pw

神经网络突变自动选择AI优化算法，速度提升50000倍！

https://zhuanlan.zhihu.com/p/44576620

让算法解放算法工程师----NAS综述

https://mp.weixin.qq.com/s/3njCAKQsQ8UhL3gqDDHZGQ

近期大热的AutoML领域，都有哪些值得读的论文？

https://mp.weixin.qq.com/s/l6uzHwGSTY5xRSkPD_B2rw

深度学习模型超参数搜索实用指南

https://mp.weixin.qq.com/s/7dNB6V4gHdkN-N2td43NrA

一种简单有效的网络结构搜索

https://mp.weixin.qq.com/s/5H3wk93QoxHteR_HD2hWiA

DeepMind提出架构搜索新方法：使用分层表示，时间短精度高

https://zhuanlan.zhihu.com/p/46350372

语义分割领域开山之作：Google提出用神经网络搜索实现语义分割

https://mp.weixin.qq.com/s/VZVaWmCit5Ey7ZboeMbK7g

AutoML、AutoKeras......这四个“Auto”的自动机器学习方法你分得清吗？

https://mp.weixin.qq.com/s/DQcRqWALLuPpNabCv83Vcg

自动机器学习，神经网络自主编程

https://mp.weixin.qq.com/s?__biz=MzUyMjE2MTE0Mw==&mid=2247486970&idx=2&sn=6878205053d13467f66055a3314eeba1

如何利用高效的搜索算法来搜索网络的拓扑结构

https://mp.weixin.qq.com/s/Y8i25dZmj41dr8nz2WyNbQ

搜索一次就够了：中科院&图森提出通过稀疏优化进行一次神经架构搜索

https://mp.weixin.qq.com/s/45QCGUXJ2VM96PNj-LEFxg

麻省理工HAN Lab提出ProxylessNAS自动为目标任务和硬件定制高效CNN结构

# 多模态学习

https://mp.weixin.qq.com/s/ruRkqBEdyj2Dx0WTO5Jhcw

多模态学习研究进展综述

https://mp.weixin.qq.com/s/vpBPkjuCebSWh5qPLYHCkw

上海交大提出多模态框架“EmotionMeter”，更精准地识别人类情绪

https://mp.weixin.qq.com/s/BBg04rDtiqU-XrWortufNA

康奈尔&英伟达提出多模态无监督图像转换新方法

http://mp.weixin.qq.com/s/khOINUyrNV3TFfgNRheH0A

卷积神经网络压缩、多模态的语义分析研究

https://mp.weixin.qq.com/s/ywU4L659iRcmIgmV6RtbXA

DeepMind新研究连接听与看，实现“听声辨位”的多模态学习
