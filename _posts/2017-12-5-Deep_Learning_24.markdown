---
layout: post
title:  深度学习（二十四）——CTC
category: DL 
---

# CTC

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

**损失函数**：对于给定的输入，我们希望训练模型来最大化分配到正确答案的概率。 为此，我们需要有效地计算条件概率$$p(Y \vert X)$$。 函数$$p(Y \vert X)$$也应该是可微的，可以使用梯度下降。

**推理**：在我们训练好模型后，我们希望使用它来推断给定X的最可能的Y。即：

$$Y^*=\mathop{\text{argmax}}_{Y} p(Y \mid X)$$

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

2.RNN给出每个time step所对应的音节的概率$$p_t(a \vert X)$$。上图中音节的颜色越深，其概率p越高。

3.计算各种时序组合的概率，给出整个序列的概率。

4.合并重复并移除空白之后，得到最终的Y。

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

## Warp-CTC

Warp-CTC是一个可以应用在CPU和GPU上的高效并行的CTC代码库，由百度硅谷实验室开发。

官网：

https://github.com/baidu-research/warp-ctc

非官方caffe版本：

https://github.com/xmfbit/warpctc-caffe



