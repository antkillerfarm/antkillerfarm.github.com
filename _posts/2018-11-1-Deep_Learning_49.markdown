---
layout: post
title:  深度学习（四十九）——语音识别, 细粒度分类, RNN进阶
category: DL 
---

# 语音识别

## end-to-end

《exploring neural transducers for end-to-end speech recognition》

|  | CTC | Transducer | Attention |
|:--:|:--:|:--:|:--:|:--:|
| 输出语言模型 | 无 | 有 | 有 |
| 对齐 | 单调，硬 | 单调，硬 | 不单调，软 |
| 解码所需步数 | 输入长度 | 输入长度+输出长度 | 输出长度 |

## 参考

https://mp.weixin.qq.com/s?__biz=MzI3MTA0MTk1MA==&mid=400189223&idx=1&sn=1cb32bee42de626443ebadbf065ec79c

百度贾磊：汉语语音识别技术重大突破：LSTM+CTC详解

https://www.zhihu.com/question/46829056

语音识别领域的最新进展目前是什么样的水准？

https://www.zhihu.com/question/29168274

语音识别中，如何理解HMM是一个生成模型，而DNN是一个判别模型呢？

https://zhuanlan.zhihu.com/p/24979135

从声学模型算法总结2016年语音识别的重大进步

https://mp.weixin.qq.com/s/zEqgDh6_fnDgXEI8MC9cmg

端对端的深度卷积神经网络在语音识别中的应用

https://mp.weixin.qq.com/s/pimQBFd5uxrZk4dSgUsblg

苹果机器学习期刊“Siri三部曲”之一：通过跨带宽和跨语言初始化提升神经网络声学模型

https://mp.weixin.qq.com/s/u1R7NUg_kgI_mpjIFrO02A

探索Siri背后的技术：将逆文本标准化（ITN）转化为标签问题

https://mp.weixin.qq.com/s/2xpwLVHT8qU68uoV7Uj2cw

小米的语音识别系统是如何搭建的

https://mp.weixin.qq.com/s/cYBMy4TIhcutvrAt0y70Ow

腾讯AI Lab副主任俞栋：过去两年基于深度学习的声学模型进展

https://mp.weixin.qq.com/s/cvSz5Pxe3z54Tl5z3WTbQA

手把手教你在音频分类DCASE2017比赛中夺冠

https://blog.csdn.net/ffmpeg4976/article/details/52347845

语音识别系统及科大讯飞最新实践

http://mp.weixin.qq.com/s/0WNJq4OLZlZETKPf1Ewq7w

浅谈语音测试方案

https://mp.weixin.qq.com/s/b0bOf1bZ2p0yWMzhp66HhA

A flight (to Boston) to Denver-基于转移的顺滑技术研究

https://mp.weixin.qq.com/s/0AvV268s3TZ0z8WwtJv6sw

一文概览语音识别中尚未解决的问题

https://mp.weixin.qq.com/s/T96S0b7Lp9YWR4cRcMQr6A

一文概览基于深度学习的监督语音分离

http://mp.weixin.qq.com/s/xRA9Xh-FTrhbIg0wLnfzhA

温正棋谈语音质检方案：从关键词检索到情感识别

https://mp.weixin.qq.com/s/XUHS4o2G-iGuV9uuOmfBdQ

为什么在说话人识别技术中，PLDA面对神经网络依然坚挺？

https://mp.weixin.qq.com/s/XP4NVYMmKj9RLsgonP3ooQ

无需进行滤波后处理，利用循环推断算法实现歌唱语音分离

https://mp.weixin.qq.com/s/GZI4uvCR3QzZDNddpBX2OQ

深度学习也解决不掉语音识别问题

https://mp.weixin.qq.com/s/E8brCI73IWY3P47IYPxSkg

谷歌发布全新端到端语音识别系统：词错率降至5.6%

http://www.cnblogs.com/qcloud1001/p/7941158.html

详解卷积神经网络（CNN）在语音识别中的应用

https://mp.weixin.qq.com/s/grqKRvv4dwKU26zT1qhq2g

Facebook开源语音识别工具包wav2letter

https://mp.weixin.qq.com/s/OeCiH4n-Y3kigI3ynMyZSg

有趣的研究奥巴马Net：从文本合成真实的唇语口型

https://mp.weixin.qq.com/s/mRmbrUJ2MgeDxbZn_0UiIQ

2017年深度学习总结：文本和语音应用

https://mp.weixin.qq.com/s/xR172RUG3JO59_2cJj_U2A

显著超越流行长短时记忆网络，阿里提出DFSMN语音识别声学模型

https://mp.weixin.qq.com/s/9QrahPP1gDM3eMNgx91spA

深度学习也能实现“鸡尾酒会效应”：谷歌提出新型音频-视觉语音分离模型

https://wenku.baidu.com/view/942891aba98271fe910ef9e3.html

基于深度学习的语音识别

https://mp.weixin.qq.com/s/HSdhkazt1j5phMTLRMjFlQ

深度对抗声学模型训练框架

https://mp.weixin.qq.com/s/bgIJMRZ64En3xMk3IGK-Vw

如何基于迁移学习快速识别出讲话的人是谁？

https://mp.weixin.qq.com/s/jHB5pRsisvh-ZB8uY1oRTA

基于TensorFlow，人声识别如何在端上实现？

https://mp.weixin.qq.com/s/I2XU9u28S6LFoTY4kizoqw

清华大学郑方：语音技术与身份信息的隐私保护

https://mp.weixin.qq.com/s/i7ugVM0mGqohThLW6NjNoQ

阿里开源自研语音识别模型DFSMN，准确率高达96.04%

https://mp.weixin.qq.com/s/o5UAnIOuDsjBWjsKg8wYCg

陶建华：深度神经网络与语音

https://mp.weixin.qq.com/s/DjskKa-KxiCX-hLKEw75Tg

Towards End-to-End Speech Recognition

https://mp.weixin.qq.com/s/xW_KvR5y12_eyp3FvtmBwQ

从概念到应用，腾讯视角深入“解剖”AI平台和语音技术

https://mp.weixin.qq.com/s/2DaBsFnRzqkf9PgvY3tWqw

谷歌神经网络人声分离技术再突破！词错率低至23.4%

https://mp.weixin.qq.com/s/V1W_M-lIJKdyGtuQyBbUPA

词错率2.97%：云从科技刷新语音识别世界纪录

https://mp.weixin.qq.com/s/eK8UxqMsUhDzJ-Ev7azS9w

田正坤：Seq2Seq模型在语音识别中的应用

# 细粒度分类

https://mp.weixin.qq.com/s/sdQ0rWbDDMN_P0B_RiYZmw

分段映射：帮助利用少量样本习得新类别细粒度分类器

https://mp.weixin.qq.com/s/zeN7rjmAnvh_7BbTmScrZw

细粒度分类你懂吗？——fine-gained image classification

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

# RNN进阶

## IndRNN

https://mp.weixin.qq.com/s/cAqpclkkeVrTiifz07HC1g

新型循环神经网络IndRNN：可构建更长更深的RNN

https://mp.weixin.qq.com/s/7-K-nZTijoYCaprRNYXxFg

新型RNN：将层内神经元相互独立以提高长程记忆

## 参考

https://mp.weixin.qq.com/s/SeR_zNZTu4t7kqB6ltNrmQ

从循环到卷积，探索序列建模的奥秘

https://mp.weixin.qq.com/s/_q69BV1r46S9X5wnLuFPSw

关于序列建模，是时候抛弃RNN和LSTM了

https://mp.weixin.qq.com/s/mIuAn4G9l3AKFAswpbaQdA

时间卷积网络（TCN）将取代RNN成为NLP预测领域王者

https://mp.weixin.qq.com/s/m5GRNp6qDfVfC0mkQ4m4Yw

神经语言模型如何利用上下文信息：长距离上下文的词序并不重要

https://mp.weixin.qq.com/s/kuoUnt2Vhz9NhfnNqMFAhQ

DeepMind提出关系RNN：构建关系推理模块，强化学习利器

https://mp.weixin.qq.com/s/wfOzCxe3L2t11VguYLGC9Q

上海交大搞出SRNN，比普通RNN也就快135倍

https://mp.weixin.qq.com/s/f0sv7c-H5o5L_wy2sUonUQ

CNN取代RNN？当序列建模不再需要循环网络

https://mp.weixin.qq.com/s/h3fF6Zvr1rSzSMpqdu8B0A

电子科大提出BT-RNN：替代全连接操作而大幅度提升LSTM效率

https://mp.weixin.qq.com/s/OgN4rVDKH5WABIaRY7CHog

如何让RNN神经元拥有基础通用的注意力能力

https://mp.weixin.qq.com/s/KBLCrupGIuPa5nVrxcS5WQ

新研究将GRU简化成单门架构，或更适用于语音识别

https://mp.weixin.qq.com/s/kQozftKd_n_kYIF7KKCc8g

短视频那么多，快手如何利用GRU实现各种炫酷的语音应用

https://mp.weixin.qq.com/s/xwuM2Vj8G7UyuEyzTyO13A

将CNN与RNN组合使用，天才还是错乱？

https://mp.weixin.qq.com/s/c7XkzjLH1n5EtqdQik618g

Dropout在RNN中的应用综述

https://mp.weixin.qq.com/s/K6LK47_GCTeZJPAW0-Xp4Q

多伦多大学提出可逆RNN：内存大降，性能不减！

https://mp.weixin.qq.com/s/lvaWx7J4HFTvYxy7-B9vYg

周志华等提出RNN可解释性方法，看看RNN内部都干了些什么

https://mp.weixin.qq.com/s/YbdiEHb8ld1pp1ehgBzTOQ

将未来信息作为正则项，Twin Networks加强RNN对长期依赖的建模能力
