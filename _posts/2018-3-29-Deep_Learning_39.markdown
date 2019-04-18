---
layout: post
title:  深度学习（三十九）——GAN进阶, 视频处理
category: DL 
---

# GAN进阶

## 参考

https://zhuanlan.zhihu.com/p/41114883

手机照片脑补成超大画幅，这个GAN想象力惊人

https://mp.weixin.qq.com/s?__biz=MzA3MzI4MjgzMw==&mid=2650749368&idx=3&sn=fc20d9e6682c74227282df3133cea06c

基于IR-transformer、IRGAN模型，解读搜狗语义匹配技术

https://mp.weixin.qq.com/s/bKve_tZi9usz4oX0T3S15A

悉尼大学陶大程：遗传对抗生成网络有效解决GAN两大痛点

https://mp.weixin.qq.com/s/UO0pNLcwYN5tE5x_4azVJA

LSGAN：最小二乘生成对抗网络

https://zhuanlan.zhihu.com/p/46629127

生成对抗网络-GAN---一个好老师的重要性

https://mp.weixin.qq.com/s/Sp0EYvaq-1u0mtnrrmFNCQ

为什么说GANs是一个绝妙的艺术创作工具？

https://mp.weixin.qq.com/s/uHEAtuY1_KZdUAdDAwFi_A

以为GAN只能“炮制假图”？它还有这7种另类用途

https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247492415&idx=1&sn=a359e72ee99555f7a2fb4e21b2ad51db

InfoGAN：一种无监督生成方法

https://mp.weixin.qq.com/s/Yf5quOXmzJAy0GnJnvam5g

台湾学者研发新型二元神经元GAN！有望用于AI作曲

https://mp.weixin.qq.com/s/8aL7COItG7lS4q5-3IZCmQ

定制人脸图像没那么难！使用TL-GAN模型轻松变脸

https://mp.weixin.qq.com/s/9t0GvQW-cmakM0E9dWxBcg

旧照片着色修复神器！自注意力GAN效果惊艳

https://mp.weixin.qq.com/s/cUFQ6EADa39h2eFoa_Dh0A

最高76%破解成功率！GAN已经能造出“万能指纹”，你的手机还安全吗？

https://mp.weixin.qq.com/s/_tABIMkWX8L5xQFmvPI7rw

有效稳定对抗模型训练过程，伯克利提出变分判别器瓶颈

https://zhuanlan.zhihu.com/p/50790727

SeqGAN: Sequence GAN with Policy Gradient

https://mp.weixin.qq.com/s/bH5yYbwq6NGQJ84xUDhoxg

生成对抗网络在图像翻译上的应用

https://mp.weixin.qq.com/s/3Gsmrl4HbcnXpje0nyAq2w

中国西北大学和北京大学的研究结果是否将终结CAPTCHA验证码时代？

https://zhuanlan.zhihu.com/p/53260242

抛开复杂证明，我们从直觉上理解W-GAN为啥这么好训

https://mp.weixin.qq.com/s/FJA8Tctq_p4Mj-KgNn_OGg

为什么让GAN一家独大？Facebook提出非对抗式生成方法GLANN

https://mp.weixin.qq.com/s/xr9fDv9DFkwi2ImV4RZAHg

换个角度看GAN：另一种损失函数

https://mp.weixin.qq.com/s/U1rrPfJDLgXHRj__XwrMZw

只有条件GAN才能稳定训练？对抗+自监督的无监督方法了解一下

https://mp.weixin.qq.com/s/xHKQlFFkBQLBg2GdZuGPSw

提升GAN训练的技巧汇总

https://mp.weixin.qq.com/s/8Vt3p8iXFq9rI-ph1Syt-Q

Uber提出基于Metropolis-Hastings算法的GAN改进思想

https://mp.weixin.qq.com/s/kobEEizpP2v5Yy-8stiGgg

让画面更逼真！这个强化超分辨率GAN让老游戏迎来第二春

https://mp.weixin.qq.com/s/tWaKMFZ4dQX7kZlT3tiDAQ

详解GAN的谱归一化（Spectral Normalization）

https://mp.weixin.qq.com/s/I7mpy5P7LFFkop618ANLtA

这份攻略帮你“稳住”反复无常的GAN

https://mp.weixin.qq.com/s/bPieF6CNvM4N1Njx6OHNwg

谷歌大脑：像BigGAN那样生成高清大图不一定需要大量图像标签

https://mp.weixin.qq.com/s/jByvPPrlN7zIOPXON4fZJw

生成对抗网络也需要注意力机制

https://mp.weixin.qq.com/s/dViPKDzxsMRRntMusi1Isw

5个最新图像合成GAN架构解读：核心理念、关键成就、商业化路径

https://mp.weixin.qq.com/s/c1zzP9_Ds1oCkFitLZLrCg

MIT本科学神重启基于能量的生成模型，新框架堪比GAN

https://mp.weixin.qq.com/s/eSvrnUCnz74O3txrPq7RlA

Google用更少标签生成图像，还提出一个用于训练评估GAN的库

https://mp.weixin.qq.com/s/CwB7IDaCZBzcQV8IBsYQDw

最新《生成式对抗网络GAN进展》论文

https://mp.weixin.qq.com/s/-SVtuZGOn6Wk9kwe50JJqw

《GAN实战：生成对抗网络深度学习》牛津大学Jakub著作

https://mp.weixin.qq.com/s/QqQ0SoF-kFTQRLxo4Tzm2g

反思基于能量的生成式模型：中山大学研究者从粒子演化角度改进经典的FRAME

https://mp.weixin.qq.com/s/ExsDftIvi8VUdvxfka-1ow

南大和中大“合体”拯救手残党：基于GAN的PI-REC重构网络，“老婆”画作有救了

https://mp.weixin.qq.com/s/RhYVCO0jNlY97ptIUvRRQQ

带你读论文：生成对抗网络GAN论文TOP 10

https://mp.weixin.qq.com/s/ErmGAsSHvCOxCn97yZ4WJQ

关于GAN的灵魂七问

https://mp.weixin.qq.com/s/Q0IUGbVcIYJMdidSVcFzFw

GAN的五个神奇应用场景

https://mp.weixin.qq.com/s/LJfsadhp3WGi0tLXN7jCUA

GAN生成图像综述

https://mp.weixin.qq.com/s/wW8kTQ7eQeCJsHC9ju8VLA

One Day One GAN_keras版[DAY 1]

# 视频处理

## 视频目标分割

视频目标分割任务和语义分割有两个基本区别：

1.视频目标分割任务分割的是一般的、非语义的目标；

2.视频目标分割添加了一个时序模块：它的任务是在视频的每一连续帧中寻找感兴趣目标的对应像素。

![](/images/article/Segmentation.png)

上图是Segmentation的细分，其中的每一个叶子都有一个示例数据集。

基于视频任务的特性，我们可以将问题分成两个子类：

无监督（亦称作视频显著性检测）：寻找并分割视频中的主要目标。这意味着算法需要自行决定哪个物体才是“主要的”。

半监督：在输入中（只）给出视频第一帧的正确分割掩膜，然后在之后的每一连续帧中分割标注的目标。

## 参考

http://mp.weixin.qq.com/s/pGrzmq5aGoLb2uiJRYAXVw

一文概览视频目标分割

https://www.zhihu.com/question/52185576

视频中的目标检测与图像中的目标检测具体有什么区别？

https://mp.weixin.qq.com/s/noljXreGfoMfiZb_n90R3w

模仿人类的印象机制，商汤提出精确实时的视频目标检测方法

http://mp.weixin.qq.com/s/-Av3-ZNi6UGlKNv_jduAeQ

微软新论文：如何利用深度特征流提高视频识别准确率？

https://mp.weixin.qq.com/s/z1APyCxlOEPHn48OeJAHkQ

基于深度学习的视频内容识别

https://mp.weixin.qq.com/s/WMakTEN68KPi7X9kMQetiw

OpenAI:3段视频演示无人驾驶目标检测强大的对抗性样本！

https://mp.weixin.qq.com/s/j5YPHYEPioLiEIDc6lK3kA

在线视频衣物精确检索技术，开启刷剧败明星同款时代

https://mp.weixin.qq.com/s/CXKuSMi0Vd43BGDf5BgoqA

弱监督视频物体识别新方法：香港科技大学联合CMU提出TD-Graph LSTM

https://mp.weixin.qq.com/s/nZVIVJ9z0AWA8VFouNpafg

深度学习之视频摘要简述

https://mp.weixin.qq.com/s/7ccEaDRngVo42OSU6FBlVg

从视频到语句，优必选获TRECVID 2017子任务冠军

https://mp.weixin.qq.com/s/XorPkuIdhRNI1zGLwg-55A

斯坦福新深度学习系统 NoScope：视频对象检测快1000倍

https://mp.weixin.qq.com/s/7w5nYWfZO-YOJ4cA47kQXg

无监督视频物体分割新思路：实例嵌入迁移

https://mp.weixin.qq.com/s/PhMPa-e4sbzqWKmFzRZE4Q

实时替换视频背景：谷歌展示全新移动端分割技术

https://mp.weixin.qq.com/s/ovjoHCcR1xYb9N6kyFJUTg

视频广告段落检测——从一个偏门说计算机视觉的发展历史

https://mp.weixin.qq.com/s/0JgwBizaCwvPP9TfLKTang

密歇根大学&谷歌提出TAL-Net：将Faster R-CNN泛化至视频动作定位中

http://mp.weixin.qq.com/s/LAgDobWyK0SOH08GCLXG7A

减少30%流量，增加清晰度：MIT提出人工智能视频缓存新算法

https://mp.weixin.qq.com/s/_ZmbwM-lmS0o2DjAAc_TWQ

美图云+中科院AAAI2018：视频语义理解的类脑智能NOASSOM

https://mp.weixin.qq.com/s/iqLHjbmLOmvfEeEUB_SqSA

计算机视觉视频理解领域的经典方法和最新成果

https://mp.weixin.qq.com/s/LzKsD_vFlA1n-TYOGJkDZg

商汤科技开源DAVIS2017视频目标分割冠军代码

https://mp.weixin.qq.com/s/FiAju9F_MWexstP7FrIquw

凭一张照片找到视频中你所有的镜头，包括背影

https://mp.weixin.qq.com/s/3H0ZJjnPsh1BzALmG0W7og

DAVIS2017视频目标分割冠军代码开源了

https://mp.weixin.qq.com/s/ZqnfSL6U5E9NzE15QMdxtg

腾讯AI Lab提出视频再定位任务，准确定位相关视频内容

https://mp.weixin.qq.com/s/6MXLtUDi_idMYqbHARkbcg

港中文林达华团队提出计算机视觉新方向：电影情节分析

https://mp.weixin.qq.com/s/Np4xyvPrncd7MJ9q1WShBA

Python视频深度学习：计算任意影片中所有演员出镜时间

https://mp.weixin.qq.com/s/Nt4QLX_lbHhszb8fFlmOLA

DeepVS：基于深度学习的视频显著性方法

https://mp.weixin.qq.com/s/NgsSQS6opjOsusTIr9Vx-w

腾讯AI Lab、MIT等机构提出TVNet：可端到端学习视频的运动表征

https://mp.weixin.qq.com/s/26OZ5sLK3floF8I1SNIKuA

时空建模新文解读：用于高效视频理解的TSM 

https://mp.weixin.qq.com/s/TzNqZNEPBewR7neU7Or9nQ

更侧重工业的应用：PRCV2018美图短视频实时分类挑战赛冠军技术方案

https://mp.weixin.qq.com/s/lBu1q5Pyw9dZIxSYXUp2pw

视频语义分割介绍

https://mp.weixin.qq.com/s/qtRV9Sb54o8TnDEhLlB69Q

基于视频的目标检测的发展

https://mp.weixin.qq.com/s/MzVPesFK0vJ1UuQPPSSN2w

百度、MIT等提出StNet：局部+全局的视频时空联合建模

https://mp.weixin.qq.com/s/UeQc3orm2ooZ5zlvrSLzOw

视频内容理解在Hulu的应用与实践

https://mp.weixin.qq.com/s/syZObdxjPv6jq3B_mgP9Sw

拒绝“不可描述”！爱奇艺短视频软色情识别技术解析

https://mp.weixin.qq.com/s/8YpyfdhDypSZOP3dQegQdQ

谷歌大脑提出基于流的视频预测模型，可产生高质量随机预测结果

https://mp.weixin.qq.com/s/-FF3tuEB2V8RlQCjQhu5Bg

人大ML研究组提出新的视频测谎算法

https://mp.weixin.qq.com/s/T-Rg9xLfdYmV8bJESK0h8g

快速端到端嵌入学习用于视频中的目标分割

https://mp.weixin.qq.com/s/pKSrokV_j8Repa-JMloUHg

Google又发大招：高效实时实现视频目标检测

https://mp.weixin.qq.com/s/ySAfdII8291hvTxUBtE5qA

详解爱奇艺ZoomAI视频增强技术的应用
