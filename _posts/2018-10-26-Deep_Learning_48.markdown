---
layout: post
title:  深度学习（四十八）——语义分割进阶, GAN进阶（2）, 问答系统
category: DL 
---

# 语义分割进阶

## U-NET的另类用法

U-NET除了用于语义分割之外，还可用于语音分离——将人声/音乐从原始混合声音数据中分离出来。比如卡拉OK中的常见的原声抑制功能。

论文：

《Singing Voice Separation With Deep U-net Convolutional Networks》

代码：

https://github.com/Xiao-Ming/UNet-VocalSeparation-Chainer

Chainer版本的实现

https://github.com/Jeongseungwoo/Singing-Voice-Separation

Tensorflow版本的实现

这种用途的U-NET和原始U-NET的区别在于：

1.输入和输出是音频数据的时序频谱图，从某种意义上来说，其实就是一张二维图片。

2.输入是包含混音的数据，而输出是纯净的人声/音乐的Mask。混音数据*Mask=纯净声音。由于标注数据比较难获得，因此通常的做法是使用纯音和若干噪声进行合成得到混音数据。

3.由于最终结果不再是像素级的分类问题，因此Loss采用了absolute difference。

从上面的论述可以看出，该论文主要是用到了语义分割网络中**输入和输出的尺寸等大**这个特点，算是一种很灵巧的构思了。

## 参考

https://mp.weixin.qq.com/s/cANlqQAI-A2mC9vnd3imQA

Instance-Aware图像语义分割

https://mp.weixin.qq.com/s/v_TLYYq6cFWuwR9tXM8m-A

如何通过CRF-RNN模型实现图像语义分割任务

https://mp.weixin.qq.com/s/ceCC7Q6yr0QKESeZXi6lWQ

堆叠解卷积网络实现图像语义分割顶尖效果

https://mp.weixin.qq.com/s/V4_euZRcyyxeimXAA_waAg

贾佳亚：最有效的COCO物体分割算法

https://mp.weixin.qq.com/s/M1Oo4ST2aspgZF8UeSUDww

如何妙笔勾檀妆：像素级语义理解

https://mp.weixin.qq.com/s/xalo2XtKtzR5tA_dPFzaJw

一文介绍3篇无需Proposal的实例分割论文

https://mp.weixin.qq.com/s/BL1xZ_YuuPe9frIc9E1fkA

南开大学提出新物体分割评价指标

https://mp.weixin.qq.com/s/3rfZUhio4Bk1RUGkEk5xoQ

ETH Zurich提出新型网络“ROAD-Net”，解决语义分割域适配问题

https://mp.weixin.qq.com/s/qMLCi-CghxvTcwyPnvFxnQ

ConvCRF：一种结合条件随机场与CNN的高效语义分割方法

https://mp.weixin.qq.com/s/deepxMWCpIEe3jk_kanfMg

金字塔注意力网络：一种利用底层像素与高级特征的语义分割网络

https://mp.weixin.qq.com/s/LVD7rry0BajGh9iQg-Y2jw

MIT用AI实现3分钟自动抠图，精细到头发丝

https://mp.weixin.qq.com/s/5n3jpvv_LxnHB0w4hsCEzQ

NVIDIA ECCV18论文:超像素采样网络助力语义分割与光流估计

https://mp.weixin.qq.com/s/MiChpWim5pGlRj88rcQtaA

谷歌等祭出图像语义理解分割神器，PS再也不用专业设计师！

https://mp.weixin.qq.com/s/J6UMzWSpcmSQVGwWKtm2Hw

UC伯克利提出基于自适应相似场的语义分割

https://zhuanlan.zhihu.com/p/43774180

提升密集预测的平滑的空洞卷积

https://mp.weixin.qq.com/s/01D9wYK92i4PjjGcwFTxOw

揭秘阿里巴巴神奇的人物抠图算法内幕

https://mp.weixin.qq.com/s/sjD36kUDQ5iCmIKpR8_rlA

双重注意力网络：中科院自动化所提出新的自然场景图像分割框架

https://mp.weixin.qq.com/s/Sn3N8IxHtgp53Y0VLIPJCQ

17毫秒每帧！实时语义分割与深度估计

https://mp.weixin.qq.com/s/iNz82GUULxDndBIiGSArmQ

新开源！实时语义分割算法Light-Weight RefineNet

# GAN进阶

https://mp.weixin.qq.com/s/c84LMFnIhoDeolc1B4MIVA

AI以假乱真怎么办？TequilaGAN教你轻松辨真伪

https://mp.weixin.qq.com/s/fgL6FtjeF-EgG5jjAGDR7A

GANimation让图片秒变GIF表情包，秒杀StarGAN

https://mp.weixin.qq.com/s/NPUJ89nddF1WHfFv2nn-Hg

GANs有嘻哈：一次学完10个GANs明星模型

https://mp.weixin.qq.com/s/yShYrMFKox30jXajXXQPGw

如何让GAN生成更高质量图像？斯坦福大学给你答案

https://mp.weixin.qq.com/s/qiLFQowjH67XECXBlppUDg

对抗深度学习:鱼(模型准确性)与熊掌(模型鲁棒性)能否兼得？

https://mp.weixin.qq.com/s/1SpHGtjkSfDEFCZuudCm0Q

基于GAN和VAE的跨模态图像生成

https://mp.weixin.qq.com/s/02amaVnLFxeLDBsjG-iN1Q

UBC&腾讯AI Lab提出首个模块化GAN架构，搞定任意图像PS组合

https://mp.weixin.qq.com/s/pf0fNSoNDaI9bZvohRX28A

不再使用人眼评估，你训练的GAN还OK吗？ 

https://mp.weixin.qq.com/s/KeXXi5kwvWAfArv13f6VPg

给Cycle-GAN加上时间约束，CMU等提出新型视频转换方法Recycle-GAN

https://mp.weixin.qq.com/s/IzVTkH7fEiS4gAUIyA_IrA

谷歌GAN 实验室来了！迄今最强可视化工具，在浏览器运行GAN

https://mp.weixin.qq.com/s/1jBpz55pPgM8oxlUZLSWXA

ICML2018对抗生成网络论文评述

https://mp.weixin.qq.com/s/uQpmP7pJ8wnEgyJrvvHgwg

基于解剖结构的面部表情生成

https://mp.weixin.qq.com/s/cLCtxS1frJYvC5tLsQAWrQ

用神经网络生成音乐

https://mp.weixin.qq.com/s?__biz=MzIwOTc2MTUyMg==&mid=2247485368&idx=1&sn=1da8bd2490fa2fe16dbe25aea79dcd63

交互式GAN Lab让生成对抗网络轻松实现可视化！

https://mp.weixin.qq.com/s?__biz=MzIwMTc4ODE0Mw==&mid=2247491630&idx=1&sn=394ffec2969cff23f63022526684f259

杜伦大学提出GANomaly：无需负例样本实现异常检测

https://mp.weixin.qq.com/s/BEuA5icaQwRJGsJJLnAFVg

用机器学习生成图片：GAN的局限性以及如何GAN的更爽

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

# 问答系统

GA-Reader

Match-LSTM

Bi-DAF

R-Net

QA-Net

S-Net

R3

## 参考

https://mp.weixin.qq.com/s/IahvlkiACOAjicX68teA0A

套路深深深几许？管窥问答系统、阅读理解的小江湖

https://mp.weixin.qq.com/s/h51gh-9LISEBdJx8X--Lsg

近期有哪些值得读的QA论文？

https://mp.weixin.qq.com/s/lB44D_RFBqbNB6YX5gEULg

近期值得读的QA论文！专题论文解读

http://www.taodocs.com/p-4795349.html

基于大规模问答语料的问题检索系统

https://wenku.baidu.com/view/d38ab1e8856a561252d36fab.html

短文本相似度计算在用户交互式问答系统中的应用

https://mp.weixin.qq.com/s/5ajhhc4mpIhoqkl5VkGnaw

深度学习实现问答机器人

https://mp.weixin.qq.com/s/2aKoOx18RB0GGQTzb3Tjzg

Facebook开源DrQA的PyTorch实现：基于维基百科的问答系统

https://mp.weixin.qq.com/s/4TC7180LmD7V6u1CdVYRFw

漫谈机器阅读理解之Facebook提出的DrQA系统

https://mp.weixin.qq.com/s/SYUq1Qd-IOmVjgs7Kp4OAg

如何打造智能问答引擎

https://mp.weixin.qq.com/s/Mzycy0chQUNWjJYdpERBOw

一文详解维基百科的开放性问答系统

https://www.cnblogs.com/combfish/p/6708667.html

(QA-LSTM)自然语言处理：智能问答IBM保险QA QA-LSTM实现笔记

https://mp.weixin.qq.com/s/BhDy55Mj5oMArO0MKwvnKw

基于异构社交网络学习的社区问答方法，同时建模问题、回答和回答者

https://mp.weixin.qq.com/s/bZgis3dxMbv6AnLNyk04Og

用数据做酷的事！手把手教你搭建问答系统

https://mp.weixin.qq.com/s/nIMk-xl8Wzy1ANcT3ApAng

百度提出问答模型GNR：检索速度提高25倍

https://mp.weixin.qq.com/s/eDA6-BqPmjGBOPsOom0VIw

自动组合神经网络做问答系统！

https://mp.weixin.qq.com/s/vazeuiFvCC8aIhP2uAXoew

达观数据智能问答技术研究

https://mp.weixin.qq.com/s/eaxyhk93PbEFzHk-qiniOQ

ReQuest: 使用问答数据产生实体关系抽取的间接监督

https://mp.weixin.qq.com/s/1VYdCLw6q4rS91_YV4SclA

理解智能问答系统（Ⅰ）

https://mp.weixin.qq.com/s/x4yXjl0YXytAgp8a64ebXg

智能问答开源项目之YodaQA（Ⅱ）

https://mp.weixin.qq.com/s/2VPfk7jX0eBP2NZMB9igBg

智能问答之使用UIMA进行文本挖掘（Ⅲ）

https://mp.weixin.qq.com/s/NohUxQKHw8Z3kX_riGM10g

智能问答之答案抽取（Ⅳ）

https://mp.weixin.qq.com/s/EU0O-WR0ynI04NsPZ-bn2g

无从下手落地问答系统？实用百度开源框架了解一下

https://mp.weixin.qq.com/s/F1z7evNDqghEzSD-_Xbgfw

基于卷积深度相关性计算的社区问答方法，建模问题和回答的匹配关系

https://mp.weixin.qq.com/s/_klvAhH-K8tcYmamlr6FcA

Logistic Regression Models分析交互式问答

https://mp.weixin.qq.com/s/_XyqNPkInWfy-gUQtHjNIg

基于深度神经网络的自动问答系统概述

https://mp.weixin.qq.com/s/GyE9qdXPGvrq12dMAf4nrQ

论文推荐：QA，增强学习，知识图谱，机器阅读理解

https://mp.weixin.qq.com/s/6dKticG2I2zqlxnZ3W0ZgQ

“猜你所想，答你所问”，携程智能客服算法实践

https://mp.weixin.qq.com/s/yiAC6SddIFFo3bR351b_0Q

Embodied Question Answering

https://mp.weixin.qq.com/s/L227SZXQPinNOkKJjwr58w

85页《基于神经方法的人机对话系统》，微软+谷歌出品
