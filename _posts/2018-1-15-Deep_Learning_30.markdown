---
layout: post
title:  深度学习（三十）——Deep Speech, 多任务学习, 数据增强, 自动求导, 信息检索
category: DL 
---

# CTC

## 推断计算（续）

![](/images/img2/CTC_5.png)

上图是一个Beam Width为3的Beam Search。Beam Search的细节可参见《机器学习（二十五）》。

由于语音的特殊性，我们实际上用的是Beam Search的一个变种：

![](/images/img2/CTC_8.png)

如上图所示，所有在合并规则下，能够合并为同一前缀的分支，在后续计算中，都被认为是同一分支。其概率值为各被合并分支的概率和。

此外，如果在语音识别中，能够结合语言模型的话，将可以极大的改善语音识别的准确率。这种情况下的CTC loss为：

$$Y^*=\mathop{\text{argmax}}_{Y} p(Y \mid X)\cdot p(Y)^{\alpha}\cdot L(Y)^{\beta}$$

其中，$$p(Y)^{\alpha}$$是语言模型概率，而$$L(Y)^{\beta}$$表示词嵌入奖励。

## CTC的特性

CTC是条件独立的。

缺点：条件独立的假设太强，与实际情况不符，因此需要语言模型来改善条件依赖性，以取得更好的效果。

优点：可迁移性比较好。比如朋友之间的聊天和正式发言之间的差异较大，但它们的声学模型却是类似的。

CTC是单调对齐的。这在语音识别上是没啥问题的，但在机器翻译的时候，源语言和目标语言之间的语序不一定一致，也就不满足单调对齐的条件。

CTC的输入/输出是many-to-one的，不支持one-to-one或one-to-many。比如，“th”在英文中是一个音节对应两个字母，这就是one-to-many的案例。

最后，Y的数量不能超过X，否则CTC还是没法work。

## CTC应用

### HMM

![](/images/img2/CTC_9.png)

如上图所示，CTC是一种特殊的HMM。CTC的状态图是单向的，这也就是上面提到的单调对齐特性，这相当于给普通HMM模型提供了一个先验条件。因此，对于满足该条件的情况，CTC的准确度要超过HMM。

最重要的是，CTC是判别模型，它可以直接和RNN对接。

### Encoder-Decoder模型

Encoder-Decoder模型是sequence问题最常用的框架，它的数学形式为：

$$
H=encode(X)\\
p(Y\mid X)=decode(H)
$$

这里的H是模型的hidden representation。

CTC模型可以使用各种Encoder，只要保证输入比输出多即可。CTC模型常用的Decoder一般是softmax。

## 参考

https://distill.pub/2017/ctc/

Sequence Modeling With CTC

http://blog.csdn.net/laolu1573/article/details/78791992

Sequence Modeling With CTC中文版

https://mp.weixin.qq.com/s?__biz=MzIzNDQyNjI5Mg==&mid=2247483834&idx=1&sn=3a92eb19858d2cec709af28d2eb69c4a

时序分类算法之Connectionist Temporal Classification

http://blog.csdn.net/u012968002/article/details/78890846

CTC原理

https://www.zhihu.com/question/47642307

语音识别中的CTC方法的基本原理

https://www.zhihu.com/question/55851184

基于CTC等端到端语音识别方法的出现是否标志着统治数年的HMM方法终结？

https://zhuanlan.zhihu.com/p/23308976

CTC——下雨天和RNN更配哦

https://zhuanlan.zhihu.com/p/23293860

CTC实现——compute ctc loss（1）

https://zhuanlan.zhihu.com/p/23309693

CTC实现——compute ctc loss（2）

http://blog.csdn.net/xmdxcsj/article/details/70300591

端到端语音识别（二）ctc。这个blog中还有5篇《CTC学习笔记》的链接。

https://blog.csdn.net/luodongri/article/details/77005948

白话CTC(connectionist temporal classification)算法讲解

## Warp-CTC

Warp-CTC是一个可以应用在CPU和GPU上的高效并行的CTC代码库，由百度硅谷实验室开发。

官网：

https://github.com/baidu-research/warp-ctc

非官方caffe版本：

https://github.com/xmfbit/warpctc-caffe

# Deep Speech

Deep Speech是吴恩达领导的百度硅谷AI Lab 2014年的作品。

论文：

《Deep Speech: Scaling up end-to-end speech recognition》

代码：

https://github.com/mozilla/DeepSpeech

![](/images/img2/Deep_Speech.png)

上图是Deep Speech的网络结构图。网络的前三层和第5层是FC，第4层是双向RNN，Loss是CTC。

主要思路：

1.这里的FC只处理部分音频片段，因此和CNN有异曲同工之妙。

2.论文解释了不用LSTM的原因是：很难并行处理。

参考：

http://blog.csdn.net/xmdxcsj/article/details/54848838

Deep Speech笔记

# Deep speech 2

Deep speech 2是Deep speech原班人马2015年的作品。

论文：

《Deep speech 2: End-to-end speech recognition in english and mandarin》

代码：

https://github.com/PaddlePaddle/DeepSpeech

这个官方代码是PaddlePaddle实现的，由于比较小众，所以还有非官方的代码：

https://github.com/ShankHarinath/DeepSpeech2-Keras

![](/images/img2/Deep_Speech_2.png)

不出所料，这里使用CNN代替了FC，音频数据和图像数据一样，都是局部特征很明显的数据，从直觉上，CNN应该要比FC好使。

至于多层RNN或者GRU都是很自然的尝试。论文的很大篇幅都是各种调参，也就是俗称的“深度炼丹”。

论文附录中，如何利用集群进行分布式训练，是本文的干货，这里不再赘述。

# EESEN

论文：

《EESEN: End-to-End Speech Recognition using Deep RNN Models and WFST-based Decoding》

>苗亚杰，南京邮电大学本科（2008）+清华硕士（2011）+CMU博士（2016）。   
>个人主页：   
>http://www.cs.cmu.edu/~ymiao/

官网：

https://github.com/srvk/eesen

eesen是基于Tensorflow开发的，苗博士之前还有个用Theano开发的叫PDNN的库。

# 多任务学习

https://mp.weixin.qq.com/s/guAgXdhZSbEAkERSB1sLRA

多任务学习-Multitask Learning概述

https://mp.weixin.qq.com/s/A-CVKTz_moaFzTYywSt2gg

张宇 杨强：多任务学习概述

https://mp.weixin.qq.com/s/ZlCI02UdRuFBc-uKqIPE_w

深度学习多任务学习综述

https://mp.weixin.qq.com/s/QXOy2jo4RhCZrD5bSVzBOQ

共享相关任务表征，一文读懂深度神经网络多任务学习

https://mp.weixin.qq.com/s/mm9bXXTEzd8DwyYlMgGMZg

NLP多任务学习：一种层次增长的神经网络结构

https://mp.weixin.qq.com/s/X6FwTgr282hbqgOz3oBX-w

多任务学习概述论文：从定义和方法到应用和原理分析

https://blog.csdn.net/CoderPai/article/details/80080455

多任务学习与深度学习

https://blog.csdn.net/CoderPai/article/details/80087188

利用TensorFlow一步一步构建一个多任务学习模型

https://mp.weixin.qq.com/s/fcFb6WkJVP8TYpoxkQgiWQ

CMU提出“十字绣网络”，自动决定多任务学习的最佳共享层

https://mp.weixin.qq.com/s/i7WAFjQHK1NGVACR8x3v0A

自然语言十项全能：转化为问答的多任务学习

https://mp.weixin.qq.com/s/NpO1UP_mzyaeqW26xLY1Xg

CVPR 2018最佳论文作者亲笔解读：研究视觉任务关联性的Taskonomy

https://mp.weixin.qq.com/s/DUSa3SW1AgvJ0szL030NHQ

一个AI玩57个游戏，DeepMind离真正“万能”的AGI不远了！

https://mp.weixin.qq.com/s/P81I5vl99mV-4StNHmd_6A

作为多目标优化的多任务学习：寻找帕累托最优解

# 数据增强

https://mp.weixin.qq.com/s/GqPfvWwH1T0XFwiZ86cW8A

SamplePairing：针对图像处理领域的高效数据增强方式

https://mp.weixin.qq.com/s/cQtXvOjSXFc4YKn7ANBc_w

谷歌大脑提出自动数据增强方法AutoAugment：可迁移至不同数据集

https://mp.weixin.qq.com/s/ojFo7-gUh73iK3uImFS2-Q

一文道尽主流开源框架中的数据增强

https://mp.weixin.qq.com/s/xJhWu-1FyhIWbFBC5oHMkw

一文道尽深度学习中的数据增强方法（上）

https://mp.weixin.qq.com/s/OctAGrcBB0a6TOGWMmVKUw

深度学习中的数据增强（下）

https://mp.weixin.qq.com/s/lMU6_ywQqneyunqEV6uDiA

如何改善你的训练数据集？

https://mp.weixin.qq.com/s/ooX9Hj5ejO6po6Ghb4zOug

一文解读合成数据在机器学习技术下的表现

https://zhuanlan.zhihu.com/p/33485388

mixup与paring samples ，ICLR2018投稿论文的数据增广两种方式

https://mp.weixin.qq.com/s/_7xFBLPGT0VRTJ22toHJ3g

深度学习中常用的图像数据增强方法

https://mp.weixin.qq.com/s/sXV9epWguGbJEZYo4yNp5Q

如何正确使用样本扩充改进目标检测性能

https://zhuanlan.zhihu.com/p/46833956

图像数据增强之弹性形变（Elastic Distortions）

# 自动求导

https://mp.weixin.qq.com/s/7Z2tDhSle-9MOslYEUpq6g

从概念到实践，我们该如何构建自动微分库

https://mp.weixin.qq.com/s/bigKoR3IX_Jvo-re9UjqUA

机器学习之——自动求导

https://www.jianshu.com/p/4c2032c685dc

自动求导框架综述

https://mp.weixin.qq.com/s/xXwbV46-kTobAMRwfKyk_w

自动求导--Deep Learning框架必备技术二三事

# 信息检索

Information Retrieval是用户进行信息查询和获取的主要方式，是查找信息的方法和手段。狭义的信息检索仅指信息查询（Information Search）。即用户根据需要，采用一定的方法，借助检索工具，从信息集合中找出所需要信息的查找过程。广义的信息检索是信息按一定的方式进行加工、整理、组织并存储起来，再根据信息用户特定的需要将相关信息准确的查找出来的过程。

这方面的DL应用可参见以下的综述文章：

《MatchZoo: A Toolkit for Deep Text Matching》

## ARC-I & ARC-II

《Convolutional neural network architectures for matching natural language sentences》

## DSSM

《Learning deep structured semantic models for web search using clickthrough data》

## CDSSM

《Learning semantic representations using convolutional neural networks for web search》

## MV-LSTM

《A deep architecture for semantic matching with multiple positional sentence representations》

## CNTN

《Convolutional Neural Tensor Network Architecture for Community-Based Question Answering》

## DRMM

《A deep relevance matching model for ad-hoc retrieval》

## MatchPyramid

《Text Matching as Image Recognition》

## Match-SRNN

《Match-SRNN: Modeling the Recursive Matching Structure with Spatial RNN》

## K-NRM

《End-to-End Neural Ad-hoc Ranking with Kernel Pooling》

## 参考

https://mp.weixin.qq.com/s/aZsj1FQnzHOr-YBcy_ljpw

DNN在搜索场景中的应用

https://mp.weixin.qq.com/s/1jgdI-Pt0PtN3oAs0Wh4XA

阿里提出电商搜索全局排序方法，淘宝无线主搜GMV提升5%

https://mp.weixin.qq.com/s/9Fcj5lO-JPfFVnRSSM_56w

深度学习在美团搜索广告排序的应用实践

https://mp.weixin.qq.com/s/wni3F9lKuO4OT32BVe0QDQ

谷歌发大招：搜索全面AI化，不用关键词就能轻松“撩书”

https://mp.weixin.qq.com/s/TrWwp-DBTrKqIT_Pfy_o5w

阿里妈妈首次公开新一代智能广告检索模型，重新定义传统搜索框架

https://mp.weixin.qq.com/s/fZv9FgbdQ1bWPoNdl9sF1A

“宝石迷阵”与信息检索

https://mp.weixin.qq.com/s?__biz=MzIzOTU0NTQ0MA==&mid=2247488366&idx=1&sn=01baaf8b6c6a2c727bb9e0e2101f803b

电商搜索算法技术的演进

