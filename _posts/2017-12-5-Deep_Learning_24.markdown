---
layout: post
title:  深度学习（二十四）——语音识别
category: DL 
---

# 语音识别

## 书籍

《Speech and Language Processing: An introduction to natural language processing, computational linguistics, and speech recognition》，Daniel Jurafsky & James H. Martin著。

>Daniel Jurafsky，1962年生，UCB本科（1983）+博士（1992）。斯坦福大学教授。   
>个人主页：   
>https://web.stanford.edu/~jurafsky/

>James H. Martin，哥伦比亚大学本科+UCB博士。University of Colorado Boulder教授。   
>个人主页：   
>http://www.cs.colorado.edu/~martin/

## 传统方法

http://blog.csdn.net/zouxy09/article/details/9140207

语音信号处理之（一）动态时间规整（DTW）

http://blog.csdn.net/zouxy09/article/details/9141875

语音信号处理之（二）基音周期估计（Pitch Detection）

http://blog.csdn.net/zouxy09/article/details/9153255

语音信号处理之（三）矢量量化（Vector Quantization）

http://blog.csdn.net/zouxy09/article/details/9156785

语音信号处理之（四）梅尔频率倒谱系数（MFCC）

https://my.oschina.net/jamesju/blog/193343

语音特征参数MFCC提取过程详解

https://liuyanfeier.github.io/2017/10/26/2017-10-27-Kaldi%E4%B9%8Bfbank%E5%92%8Cmfcc%E7%89%B9%E5%BE%81%E6%8F%90%E5%8F%96/

kaldi之fbank和mfcc特征提取

http://blog.csdn.net/wxb1553725576/article/details/78048546

Kaldi特征提取之-FBank

## CTC

Connectionist Temporal Classification，是一种改进的RNN模型。它主要解决的是时序模型中，输入数大于输出数，输入输出如何对齐的问题。它由Alex Graves于2006年提出。

论文：

《Connectionist Temporal Classification: Labelling Unsegmented
Sequence Data with Recurrent Neural Networks》

>Alex Graves，瑞士IDSIA研究所博士，DeepMind研究员。

![](/images/img2/CTC.png)

上图展示了音频数据被识别的过程：

1.将音频数据均分成若干段，每段匹配一个音节。

2.合并重复的音节，并去掉分割音节（即上图中的$$\epsilon$$）。

相比于图像识别，人类语音的音节种类要少的多。但麻烦的是，一个音节的长短，会由于语速等因素的改变，而有较大差异。因此如何将数据段和音节匹配，成为了语音识别的难点。

这个问题不只是在语音识别中出现，我们在其他许多地方也能看到它。比如手写体识别、视频行为标注等。

![](/images/img2/CTC_2.png)

我们首先用数学的方式定义对齐问题。假设输入序列为$$X=[x_1,x_2,\dots,x_T]$$，输出序列为$$Y=[y_1,y_2,\dots,y_U]$$，所谓对齐问题就是将X映射到Y。

这里的主要问题在于：

1.X和Y的长度可以变。

2.X和Y的长度比例可变。

3.我们没有一个X和Y之间的准确的对齐（对应元素）。

CTC算法可以克服这些挑战。对于一个给定的X，它给我们一个所有可能Y值的输出分布。我们可以使用这个分布来推断可能的输出或者评估一个给定输出的概率。

并不是所有的计算损失函数和执行推理的方法都是易于处理的。我们要求CTC可以有效地做到这两点。

损失函数：对于给定的输入，我们希望训练模型来最大化分配到正确答案的概率。 为此，我们需要有效地计算条件概率$$p(Y \vert X)$$。 函数$$p(Y \vert X)$$也应该是可微的，可以使用梯度下降。

推理：在我们训练好模型后，我们希望使用它来推断给定X的最可能的Y。即：

$$Y^*=\mathop{\text{argmax}}_{Y} p(Y \mid X)$$

![](/images/img2/full_collapse_from_audio.png)

参考：

https://distill.pub/2017/ctc/

Sequence Modeling With CTC

http://blog.csdn.net/laolu1573/article/details/78791992

Sequence Modeling With CTC中文版

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

端到端语音识别（二） ctc。这个blog中还有5篇《CTC学习笔记》的链接。

## 参考

https://mp.weixin.qq.com/s?__biz=MzI3MTA0MTk1MA==&mid=400189223&idx=1&sn=1cb32bee42de626443ebadbf065ec79c

百度贾磊：汉语语音识别技术重大突破：LSTM+CTC详解

https://www.zhihu.com/question/20398418

语音识别的技术原理是什么？

https://www.zhihu.com/question/46829056

语音识别领域的最新进展目前是什么样的水准？

https://zhuanlan.zhihu.com/p/27064536

用Wavenet做中文语音识别

https://www.zhihu.com/question/29168274

语音识别中，如何理解HMM是一个生成模型，而DNN是一个判别模型呢？

https://zhuanlan.zhihu.com/p/24979135

从声学模型算法总结2016年语音识别的重大进步

https://mp.weixin.qq.com/s/LsVhMaHrh8JgfpDra6KSPw

横向对比5大开源语音识别工具包

https://mp.weixin.qq.com/s/-NTQG7_-GqGQWrRhiGgAQQ

详述DeepMind wavenet原理及其TensorFlow实现

https://mp.weixin.qq.com/s/bFjXDQlxRbt1ia-DSfYazw

SampleRNN语音合成模型

https://mp.weixin.qq.com/s/zEqgDh6_fnDgXEI8MC9cmg

端对端的深度卷积神经网络在语音识别中的应用

https://mp.weixin.qq.com/s/pimQBFd5uxrZk4dSgUsblg

苹果机器学习期刊“Siri三部曲”之一：通过跨带宽和跨语言初始化提升神经网络声学模型

https://mp.weixin.qq.com/s/u1R7NUg_kgI_mpjIFrO02A

探索Siri背后的技术：将逆文本标准化（ITN）转化为标签问题

https://mp.weixin.qq.com/s/2xpwLVHT8qU68uoV7Uj2cw

小米的语音识别系统是如何搭建的

https://mp.weixin.qq.com/s/xAO7mX64miTXE8E2vZ5q_w

Facebook开源TTS神经网络VoiceLoop：基于室外声音的语音合成

https://mp.weixin.qq.com/s/CVBSvQwnDqT-IVCZV7idog

极限元语音算法专家刘斌：基于深度学习的语音生成问题

https://mp.weixin.qq.com/s/cYBMy4TIhcutvrAt0y70Ow

腾讯AI Lab副主任俞栋：过去两年基于深度学习的声学模型进展

https://mp.weixin.qq.com/s/cvSz5Pxe3z54Tl5z3WTbQA

手把手教你在音频分类DCASE2017比赛中夺冠

https://mp.weixin.qq.com/s/UGhkTavbh21vBhtrrBeTfw

麦克风阵列的语音信号处理技术

https://mp.weixin.qq.com/s/1ZWrTdd3S5zYRyANfFmBOw

声学模型

https://mp.weixin.qq.com/s/mY2__KWvdAd8ZcNm-voSsg

语音识别之解码器技术简介

http://mp.weixin.qq.com/s/-QQjz61VAOVcWE7j-EJPhg

谈谈蚂蚁金服的语音唤醒系统

http://mp.weixin.qq.com/s/0WNJq4OLZlZETKPf1Ewq7w

浅谈语音测试方案

http://mp.weixin.qq.com/s/0Xg_acbGG3pTIgsRQKJjrQ

历经一年，DeepMind WaveNet语音合成技术正式产品化

https://mp.weixin.qq.com/s/KBLCrupGIuPa5nVrxcS5WQ

新研究将GRU简化成单门架构，或更适用于语音识别

https://mp.weixin.qq.com/s/b0bOf1bZ2p0yWMzhp66HhA

A flight (to Boston) to Denver-基于转移的顺滑技术研究

https://mp.weixin.qq.com/s/0AvV268s3TZ0z8WwtJv6sw

一文概览语音识别中尚未解决的问题

https://mp.weixin.qq.com/s/T96S0b7Lp9YWR4cRcMQr6A

一文概览基于深度学习的监督语音分离

https://mp.weixin.qq.com/s/TTPpOOxSLbCgOmAsI9TLiw

百度发布Deep Voice 3：全卷积注意力机制TTS系统

http://mp.weixin.qq.com/s/xRA9Xh-FTrhbIg0wLnfzhA

温正棋谈语音质检方案：从关键词检索到情感识别

https://mp.weixin.qq.com/s/XUHS4o2G-iGuV9uuOmfBdQ

为什么在说话人识别技术中，PLDA面对神经网络依然坚挺？

https://mp.weixin.qq.com/s/zWmJ3uXnFtXaI2BotoadHA

从技术到产品，苹果Siri深度学习语音合成技术揭秘

https://mp.weixin.qq.com/s/I2nbzD2QqSYgahI2jLjYTQ

批训练、注意力模型及其声纹分割应用，谷歌三篇论文揭示其声纹识别技术原理

https://mp.weixin.qq.com/s/XP4NVYMmKj9RLsgonP3ooQ

无需进行滤波后处理，利用循环推断算法实现歌唱语音分离

https://mp.weixin.qq.com/s/GZI4uvCR3QzZDNddpBX2OQ

深度学习也解决不掉语音识别问题

https://mp.weixin.qq.com/s/u1UnAuGllcWn8Ik5wDPY6w

可视化语音分析：深度对比Wavenet、t-SNE和PCA等算法

https://mp.weixin.qq.com/s/w9_D1_VVhk9md4RANaipDg

Mozilla开源语音识别模型和世界第二大语音数据集

https://mp.weixin.qq.com/s/E8brCI73IWY3P47IYPxSkg

谷歌发布全新端到端语音识别系统：词错率降至5.6%

http://www.cnblogs.com/qcloud1001/p/7941158.html

详解卷积神经网络（CNN）在语音识别中的应用

https://mp.weixin.qq.com/s/6xxXOx59lDZx0kUPb_ftBA

漫谈语音合成之Char2Wav模型

https://mp.weixin.qq.com/s/grqKRvv4dwKU26zT1qhq2g

Facebook开源语音识别工具包wav2letter

https://mp.weixin.qq.com/s/OeCiH4n-Y3kigI3ynMyZSg

有趣的研究奥巴马Net：从文本合成真实的唇语口型

## Kaldi

Kaldi是一个语音识别的工具包。官网：

https://github.com/kaldi-asr/kaldi

## HTK

Hidden Markov Model Toolkit是另一个语音识别的工具包。官网：

http://htk.eng.cam.ac.uk/

## WFST

Weighted-Finite-State-Transducer

https://www.microsoft.com/en-us/research/wp-content/uploads/2016/11/ParallelizingWFSTSpeechDecoders.ICASSP2016.pdf

PARALLELIZING WFST SPEECH DECODERS

http://www.cs.nyu.edu/~mohri/pub/csl01.pdf

Weighted Finite-State Transducers in Speech Recognition

## Warp-CTC

Warp-CTC是一个可以应用在CPU和GPU上的高效并行的CTC代码库，由百度硅谷实验室开发。

官网：

https://github.com/baidu-research/warp-ctc

非官方caffe版本：

https://github.com/xmfbit/warpctc-caffe

## Tacotron

论文：

《Tacotron: A Fully End-to-End Text-To-Speech Synthesis Model》

![](/images/img2/Tacotron.png)

![](/images/img2/Tacotron_2.png)

参考：

https://mp.weixin.qq.com/s/MJE2JRYU7KakNKmHkD42CA

谷歌发布TTS新系统Tacotron 2：直接从文本生成类人语音

https://mp.weixin.qq.com/s/uh-Gh8BSxBi-jjG6-d7-UQ

Tacotron一种端到端的Text-to-Speech合成模型

https://www.jiqizhixin.com/articles/2017-03-31-5

谷歌全端到端语音合成系统Tacotron：直接从字符合成语音



