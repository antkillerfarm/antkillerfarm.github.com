---
layout: post
title:  深度学习（四十九）——语音识别
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

