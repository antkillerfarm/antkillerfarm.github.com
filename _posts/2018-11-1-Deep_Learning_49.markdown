---
layout: post
title:  深度学习（四十九）——语音识别, 细粒度分类, AlphaGo
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

# AlphaGo

樊麾讲解AlphaGo与李世石的五番棋：

https://deepmind.com/research/alphago/alphago-games-simplified-chinese/

论文：

《Mastering the game of Go with deep neural networks and tree search》

## Leela Zero

Leela Zero是比利时人Gian-Carlo Pascutto开源的围棋AI。它的算法与AlphaGo Zero相同。而训练采用GTP协议，集合全球算力，进行分布式训练。

官网：

http://zero.sjeng.org/

代码：

https://github.com/gcp/leela-zero

>十多年前，当我还是一个中二青年的时候，就幻想有朝一日能够拿围棋世界冠军。当然，就算再中二，我自己也明白靠实力那是不可能的，当时做梦的法宝是制造一个AI，然后碾压一下所谓的国手。   
>按照当时人们的预计(2000年前后)，这个AI在2030年之前，都不可能造出来，然而，最终的结果实际上只花了一半左右的时间。   
>再之后，随着AI围棋的平民化，我的中二梦终于也有人将之付诸实现了：   
>https://mp.weixin.qq.com/s/npt2zZrKwPnNdY-hsa2RjQ   
>AI再乱围棋圈：“食言之战”柯洁落败；首例素人作弊引风波

这次作弊风波所使用的AI就是Leela Zero，可见目前（2018.5）它的棋力已经超过了顶尖棋手。

## ELF OpenGo

ELF OpenGo是Facebook开源的围棋AI，它是FB的AI游戏框架ELF的一部分。

官网：

https://github.com/pytorch/ELF

## PhoenixGo

PhoenixGo是腾讯微信团队的AlphaGo Zero复刻版。

官网：

https://github.com/Tencent/PhoenixGo

参考：

https://mp.weixin.qq.com/s/tJDmxsuS1QigYS75ZIdzRA

微信团队开源围棋AI技术PhoenixGo，复现AlphaGo Zero论文

## 参考

https://mp.weixin.qq.com/s/Sfv-jzQAkN0PsZOGZUQhkQ

AlphaGo Zero横空出世，DeepMind Nature论文解密不使用人类知识掌握围棋

https://mp.weixin.qq.com/s/oAxouYX7-wDC5okbu--Wuw

Nature重磅：人工智能从0到1, 无师自通完爆阿法狗100-0

https://zhuanlan.zhihu.com/p/30262872

关于AlphaGo Zero

https://zhuanlan.zhihu.com/p/30263585

DeepMind新一代围棋程序AlphaGo Zero再次登上Nature

https://www.zhihu.com/question/66861459

如何评价DeepMind发表在Nature上的AlphaGo Zero？

http://www.alphago-games.com/

AlphaGo的棋谱

https://deepmind.com/blog/alphago-zero-learning-scratch/

AlphaGo Zero官方声明

https://zhuanlan.zhihu.com/mathNote

某牛的专栏，主要讲自制AlphaGo

https://mp.weixin.qq.com/s/DC9QqHdWT0xFnowEBuJDbw

自动化所解读“深度强化学习”：从AlphaGo到AlphaGoZero

https://mp.weixin.qq.com/s/uZtaxRwROCqYmL2k6Muxaw

从阿尔法狗元(AlphaGo Zero)的诞生看终极算法的可能性

https://mp.weixin.qq.com/s/i5OmLu8aNbypiTUmP4teeQ

刘遥行：深入浅出看懂AlphaGo Zero

https://mp.weixin.qq.com/s/aBrwbB_DOGTen-6XL7LGFQ

邓侃：白话蒙特卡洛树搜索和ResNet

https://mp.weixin.qq.com/s/nbTkr0PImlXUSYl91HD91Q

AlphaGo背后的力量：蒙特卡洛树搜索入门指南

https://mp.weixin.qq.com/s/-tH7DQo1cK9gA0bcpBJSDA

AlphaGo Zero：笔记与伪代码

https://mp.weixin.qq.com/s/CJuVoOf7idUChFIn7dH0Lg

围棋中的数学原理

https://mp.weixin.qq.com/s/d46qNFaftt4wxpV4sZnG-w

一张图看懂AlphaGo Zero

https://zhuanlan.zhihu.com/p/31749249

比AlphaGo Zero更强的AlphaZero问世，8小时解决一切棋类！

https://mp.weixin.qq.com/s/L7bZMkqyncwEt6D5tK1OdQ

AlphaZero炼成最强通用棋类AI，DeepMind强化学习算法8小时完爆人类棋类游戏

https://mp.weixin.qq.com/s/tFdnxqV5a5xZrFtB6E0AiQ

新AlphaZero出世称霸棋界，8小时搞定一切棋类！自对弈通用强化学习无师自通！

https://mp.weixin.qq.com/s/qYWsFBKNCKCGUmizX_1sVg

AlphaGo 教学工具终于上线了！

https://mp.weixin.qq.com/s/JxbIeDk8_wnYu_ewUHp29g

深度学习与围棋实战书籍《Deep Learning and the Game of Go》

https://mp.weixin.qq.com/s/gsRnbknytz2FY2dWgdWEYg

精通国际象棋的AI研究员：AlphaZero真的是一次突破吗？

https://mp.weixin.qq.com/s/Przl4ivbNuOFmz4pcYTrpQ

浅述：从Minimax到AlphaZero，完全信息博弈之路（1）

https://zhuanlan.zhihu.com/p/32089487

AlphaZero实战：从零学下五子棋

http://mp.weixin.qq.com/s/72riTTC3w0q9oF5H-51kXA

手把手教你搭建AlphaZero（使用Python和Keras）

https://mp.weixin.qq.com/s/Qw2tT7H1PwDvPgOYy8YUsQ

AlphaGo Zero代码迟迟不开源，TF等不及自己推了一个

https://mp.weixin.qq.com/s/Vq-osjgNXJQu5avGkxQdsw

手把手：AlphaGo有啥了不起，我也能教你做一个

https://mp.weixin.qq.com/s/ajajJ9yJZsOy4Vc0ULBxXg

国际象棋版AlphaZero出来了诶，还开源了Keras实现

https://zhuanlan.zhihu.com/p/41814142

从源码解密AlphaGo Zero背后基本原理

https://www.ifanr.com/630602

AlphaGo的棋局，与人工智能有关，与人生无关
