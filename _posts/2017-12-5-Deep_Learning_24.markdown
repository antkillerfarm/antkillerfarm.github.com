---
layout: post
title:  深度学习（二十四）——CTC
category: DL 
---

# 语音识别（续）

## 参考

https://mp.weixin.qq.com/s?__biz=MzI3MTA0MTk1MA==&mid=400189223&idx=1&sn=1cb32bee42de626443ebadbf065ec79c

百度贾磊：汉语语音识别技术重大突破：LSTM+CTC详解

https://www.zhihu.com/question/20398418

语音识别的技术原理是什么？

https://www.zhihu.com/question/46829056

语音识别领域的最新进展目前是什么样的水准？

https://www.zhihu.com/question/29168274

语音识别中，如何理解HMM是一个生成模型，而DNN是一个判别模型呢？

https://zhuanlan.zhihu.com/p/24979135

从声学模型算法总结2016年语音识别的重大进步

https://mp.weixin.qq.com/s/LsVhMaHrh8JgfpDra6KSPw

横向对比5大开源语音识别工具包

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

https://mp.weixin.qq.com/s/8e4bkyTJIxHZ1y95GshA0Q

开源的语音合成系统WORLD介绍以及使用方法

https://mp.weixin.qq.com/s/mRmbrUJ2MgeDxbZn_0UiIQ

2017年深度学习总结：文本和语音应用

https://mp.weixin.qq.com/s/JSnyE2k7jqd5GR1lHA6WUg

阿里巴巴Oral论文：用于语音合成的深度前馈序列记忆网络

https://mp.weixin.qq.com/s/H77iom38lTR0KzeFXrdWew

DeepMind与谷歌大脑联手推出WaveRNN，移动端合成高保真音频媲美WaveNet

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

CTC对齐有一些显著的特性：

首先，X和Y之间允许的对齐是单调的。如果我们前进到下一个输入，我们可以保持相应的输出相同或前进到下一个输入。

第二个属性是X到Y的对齐是多对一的。一个或多个输入元素可以对齐到一个输出元素，但反过来不成立。

这意味着第三个属性：Y的长度不能大于X的长度。

![](/images/img2/full_collapse_from_audio.png)

上图是CTC对齐的一般步骤：

1.输入序列（如音频的频谱图）导入一个RNN模型。

2.RNN给出每个time step所对应的音节的概率$$p_t(a \mid X)$$。上图中音节的颜色越深，其概率p越高。

3.计算各种时序组合的概率，给出整个序列的概率。

4.合并重复并移除空白之后，得到最终的Y。

严格的说，一对(X,Y)的CTC目标函数是：

$$p(Y\mid X)=\sum_{A\in A_{X,Y}}\prod_{t=1}^Tp_t(a_t\mid X)$$

这里的模型通常使用一个RNN来估计每个time step的概率，但是也可以自由使用任何学习算法，在给定一个固定尺寸输入片段的情况下产生一个输出类别的分布。

在实际训练中，针对训练集$$\mathcal{D}$$，一般采用最小化log-likelihood的方式计算CTC loss：

$$\sum_{(X,Y)\in \mathcal{D}}-\log p(Y\mid X)$$

采用穷举法计算上述目标函数，计算量是非常巨大的。我们可以使用动态规划算法更快的计算loss。关键点是，如果两个对齐在同一步已经达到了相同的输出，可以合并它们。如下图所示：

![](/images/img2/CTC_3.png)

这里如果把音节匹配换成掷骰子的例子，就可以看出这实际上和《机器学习（二十二）》中HMM所解决的第二个问题是类似的，而HMM的前向计算正是一种动态规划算法。动态规划算法可参见《机器学习（二十七）》。

下面我们来介绍一下具体的计算方法。

首先使用$$\epsilon$$分隔Y中的符号，就得到了序列Z：

$$Z=[\epsilon,y_1,\epsilon,y_2,\dots,\epsilon,y_U,\epsilon]$$

用$$\alpha_{s,t}$$表示子序列$$Z_{1:s}$$在t步之后的CTC值。显然，我们需要计算的目标$$P(Y\mid X)$$和最后一步的$$\alpha$$有关，但只有计算出了上一步的$$\alpha$$，我们才能计算当前的$$\alpha$$。

![](/images/img2/CTC_4.png)

不同于掷骰子过程中，骰子的每种状态都有可能出现的情况，语音由于具有连续性，因此有些情况实际上是不可能的。比如上图的$$x_1$$就不大可能是后三个符号。

所以，可能的情况实际上只有两种：

### Case 1

![](/images/img2/CTC_6.png)

上图表示的是语音在两个相同token之间切换的情况。（这种情况也就是上面提到的hello例子中，语音在两个l之间过渡的情况。）

在这种情况下，$$\alpha$$的计算公式如下：

$$\alpha_{s,t}=(\alpha_{s-1,t-1}+\alpha_{s,t-1})\cdot p_t(Z_s \mid X)$$

### Case 2

![](/images/img2/CTC_7.png)

上图表示的是语音在两个不同token之间切换的情况。

在这种情况下，$$\alpha$$的计算公式如下：

$$\alpha_{s,t}=(\alpha_{s-2,t-1}+\alpha_{s-1,t-1}+\alpha_{s,t-1})\cdot p_t(Z_s \mid X)$$

