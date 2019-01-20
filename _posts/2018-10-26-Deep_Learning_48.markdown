---
layout: post
title:  深度学习（四十八）——语义分割进阶, 问答系统, AI前沿
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

https://github.com/antkillerfarm/antkillerfarm_crazy/tree/master/python/ml/tensorflow/Singing-Voice-Separation

Jeongseungwoo的版本一次加载了全部的数据集到内存中，对PC的要求较高（估计起码要32GB内存才能跑），这是我修改后的版本。用户可以根据自己PC的性能，调整batch size。

https://github.com/f90/Wave-U-Net

另一个Tensorflow版本的实现。这哥们还有个使用Semi-supervised adversarial学习分离人声的项目（他也是该项目论文的一作）：

https://github.com/f90/AdversarialAudioSeparation

![](/images/img2/wave_u_net.png)

这种用途的U-NET和原始U-NET的区别在于：

1.输入和输出是音频数据的时序频谱图，从某种意义上来说，其实就是一张二维图片。

2.输入是包含混音的数据，而输出是纯净的人声/音乐的Mask。混音数据*Mask=纯净声音。由于标注数据比较难获得，因此通常的做法是使用纯音和若干噪声进行合成得到混音数据。

3.由于最终结果不再是像素级的分类问题，因此Loss采用了absolute difference。

从上面的论述可以看出，该论文主要是用到了语义分割网络中**输入和输出的尺寸等大**这个特点，算是一种很灵巧的构思了。

这方面的数据集主要有：

***CCMixter：***

https://members.loria.fr/ALiutkus/kam/

这个数据集的每个文件夹下有3个wav文件：

source-01.wav：纯音乐。

source-02.wav：人声。

mix.wav：混合后的声音。

***MUSDB18：***

https://sigsep.github.io/datasets/musdb.html

类似这样用法的还有：

《Learning to See in the Dark》

代码：

https://github.com/cchen156/Learning-to-See-in-the-Dark

![](/images/img2/SID.png)

如上图所示，该文的目标是使用神经网络替换传统的相机ISP过程。由于输入和输出的尺寸等大，照例又到了U-NET出场的时间。

为了实现这一目标，作者收集了一个新的原始图像数据集，在弱光条件下快速曝光。同时，每个微光图像都有相应的长曝光、高质量的参考图像。

参考：

https://mp.weixin.qq.com/s/cr0BJLkyN2kW35-w1pebGQ

学习在黑暗中看世界（Learning to See in the Dark）

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

https://mp.weixin.qq.com/s/1wqguIqDS4FNsS67Yj77Qw

牛津大学&Emotech首次严谨评估语义分割模型对对抗攻击的鲁棒性

https://zhuanlan.zhihu.com/p/48198502

Non-local Neural Networks论文笔记

https://mp.weixin.qq.com/s/EzfvKzs8Ue8i9x9TFgZ-CQ

爱奇艺蒙版AI：弹幕穿人过，爱豆心中坐

https://mp.weixin.qq.com/s/19uMhoNXEygLRTYT2PbsYQ

图像分割技术介绍

https://mp.weixin.qq.com/s/-wwDenAWRGCvJmprxsl15Q

基于多特征地图和深度学习的实时交通场景分割

https://mp.weixin.qq.com/s/ygWCfLnakHIwLVk7hRAKNg

全景分割任务介绍及其最新进展

https://mp.weixin.qq.com/s/ZN9ZYPTcgVP2c9mCx9Ox3g

全景分割这一年，端到端之路

https://mp.weixin.qq.com/s/x95XWQW2euTEcUW5vkIEoA

何恺明组又出神作！最新论文提出全景分割新方法（Panoptic FPN）

https://zhuanlan.zhihu.com/p/54510782

DANet论文笔记

https://mp.weixin.qq.com/s/94XHMHejqKmkFVA0Vzfrsw

有关语义分割的奇技淫巧有哪些？

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

# AI前沿

人工智能前沿7大热点：

1.强化学习

2.元学习

3.模仿学习

4.机器人

5.概念与抽象

6.感知与意识

7.因果推理

参考：

https://mp.weixin.qq.com/s/QtO-4ARpAQf0kXfALqMKSQ

DeepMind-深度学习: AI革命及其前沿进展

https://mp.weixin.qq.com/s/sji2HVli-Y7am37YjTbq3w

谷歌Jeff Dean 64页 PPT 讲述《用深度学习解决挑战性问题》

https://mp.weixin.qq.com/s/AQrgvjFPXUpqfqQQgOFN9A

36页最新深度学习综述论文：算法、技术、应用，181篇参考文献

https://mp.weixin.qq.com/s/cGKsZYxrVP7hVnv7Jli9Zg

MIT课程全面解读2019深度学习最前沿
