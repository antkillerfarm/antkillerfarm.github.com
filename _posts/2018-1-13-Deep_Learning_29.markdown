---
layout: post
title:  深度学习（二十九）——语音识别, 语音合成, CTC
category: DL 
---

# 花式卷积进阶

## 3D卷积（续）

https://zhuanlan.zhihu.com/p/25912625

C3D network: 用于视频特征提取的3维卷积网络

https://zhuanlan.zhihu.com/p/26350774

SCNN-用于时序动作定位的多阶段3D卷积网络

https://www.jiqizhixin.com/articles/2016-08-03

FusionNet融合三个卷积网络：识别对象从二维升级到三维

http://blog.csdn.net/zouxy09/article/details/9002508

基于3D卷积神经网络的人体行为理解

https://mp.weixin.qq.com/s/YdON6Yzddq2f_QGbQsOY8w

深度三维残差神经网络：视频理解新突破

## 参考

https://mp.weixin.qq.com/s/qReN6z8s45870HSMCMNatw

微软亚洲研究院：逐层集中Attention的卷积模型

http://blog.csdn.net/shuzfan/article/details/77964370

不规则卷积神经网络

# 语音识别

## end-to-end

《exploring neural transducers for end-to-end speech recognition》

|  | CTC | Transducer | Attention |
|:--:|:--:|:--:|:--:|:--:|
| 输出语言模型 | 无 | 有 | 有 |
| 对齐 | 单调，硬 | 单调，硬 | 不单调，软 |
| 解码所需步数 | 输入长度 | 输入长度+输出长度 | 输出长度 |

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

https://mp.weixin.qq.com/s/KBLCrupGIuPa5nVrxcS5WQ

新研究将GRU简化成单门架构，或更适用于语音识别

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

https://mp.weixin.qq.com/s/I2nbzD2QqSYgahI2jLjYTQ

批训练、注意力模型及其声纹分割应用，谷歌三篇论文揭示其声纹识别技术原理

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

https://mp.weixin.qq.com/s/p1mAfM8LBLMQILeNOpbf6Q

改进语音识别性能的数据增强技巧

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

# 语音合成

语音合成（Speech synthesis），有时也叫做text-to-speech (TTS)。

## 教程

http://web.stanford.edu/~jurafsky/slp3/ed3book.pdf

《语音与语言处理》第三版，NLP和语音合成方面的专著

## 开源项目

### Festival

Festival是CMU的Rob Clark和Alan Black发起的项目。

官网：

http://www.cstr.ed.ac.uk/projects/festival/

### eSpeak NG

官网：

https://github.com/espeak-ng/espeak-ng/

### gnuspeech

官网：

https://www.gnu.org/software/gnuspeech/

### 其他

其他的开源项目还有Gnopernicus、Orca、FreeTTS和Automatik Text Reader，但是这些项目目前基本处于不更新的状态了。

## 参考

https://mp.weixin.qq.com/s/bFjXDQlxRbt1ia-DSfYazw

SampleRNN语音合成模型

https://mp.weixin.qq.com/s/xAO7mX64miTXE8E2vZ5q_w

Facebook开源TTS神经网络VoiceLoop：基于室外声音的语音合成

https://mp.weixin.qq.com/s/CVBSvQwnDqT-IVCZV7idog

极限元语音算法专家刘斌：基于深度学习的语音生成问题

https://mp.weixin.qq.com/s/TTPpOOxSLbCgOmAsI9TLiw

百度发布Deep Voice 3：全卷积注意力机制TTS系统

https://mp.weixin.qq.com/s/zWmJ3uXnFtXaI2BotoadHA

从技术到产品，苹果Siri深度学习语音合成技术揭秘

https://mp.weixin.qq.com/s/6xxXOx59lDZx0kUPb_ftBA

漫谈语音合成之Char2Wav模型

https://mp.weixin.qq.com/s/8e4bkyTJIxHZ1y95GshA0Q

开源的语音合成系统WORLD介绍以及使用方法

https://mp.weixin.qq.com/s/JSnyE2k7jqd5GR1lHA6WUg

阿里巴巴Oral论文：用于语音合成的深度前馈序列记忆网络

https://mp.weixin.qq.com/s/H77iom38lTR0KzeFXrdWew

DeepMind与谷歌大脑联手推出WaveRNN，移动端合成高保真音频媲美WaveNet

https://mp.weixin.qq.com/s/p_VjFwwDCu1i_ovUljaoVw

阿里巴巴语音交互智能团队：基于线性网络的语音合成说话人自适应

# CTC

## 概述

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

**损失函数**：对于给定的输入，我们希望训练模型来最大化分配到正确答案的概率。 为此，我们需要有效地计算条件概率$$p(Y \mid X)$$。 函数$$p(Y \mid X)$$也应该是可微的，可以使用梯度下降。

**推理**：在我们训练好模型后，我们希望使用它来推断给定X的最可能的Y。即：

$$Y^*=\mathop{\text{argmax}}_{Y} p(Y \mid X)$$

## 算法推导

为了推导CTC对齐的具体形式，我们首先考虑一种初级的做法：假设输入长度为6，Y=[c,a,t]，对齐X和Y的一种方法是将输出字符分配给每个输入步骤并折叠重复的部分。

这种方法存在两个问题：

1.通常，强制每个输入步骤与某些输出对齐是没有意义的。例如在语音识别中，输入可以具有无声的延伸，但没有相应的输出。

2.我们没有办法产生连续多个字符的输出。考虑这个对齐[h，h，e，l，l，l，o]，折叠重复将产生“helo”而不是“hello”。

为了解决这些问题，CTC为一组允许的输出引入了一个新的标记。 这个新的标记有时被称为空白标记。 我们在这里将其称为$$\epsilon$$，$$\epsilon$$标记不对应任何东西，可以从输出中移除。

CTC允许的对齐是与输入的长度相同。 在合并重复并移除ε标记后，我们允许任何映射到Y的对齐方式。

如果Y在同一行中有两个相同的字符，那么一个有效的对齐必须在它们之间有一个$$\epsilon$$。 有了这个规则，我们就可以区分崩“hello”和“helo”。


