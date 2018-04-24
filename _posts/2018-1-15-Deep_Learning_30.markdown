---
layout: post
title:  深度学习（三十）——Deep Speech, WaveNet
category: DL 
---

# CTC（续）

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

## 推断计算

我们首先看看CTC的正向推断（Inference）是如何计算的。

在前面的章节我们已经指出，由于对齐有很多种可能的情况，采用穷举法是不现实的。

另一个比较容易想到的方法是：每步只采用最大可能的token。这种启发式算法，实际上就是A*算法。

A*算法计算速度快，但不一定能找到最优解。

一般采用改良的Beam Search算法，在准确率和计算量上取得一个trade off。

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

# WaveNet

WaveNet是DeepMind 2016年的作品，主要用于语音合成，也可用于语音识别。

DeepMind在WaveNet方面，按照时间顺序有3篇论文：

《WAVENET: A GENERATIVE MODEL FOR RAW AUDIO》

《Neural Machine Translation in Linear Time》

《Parallel WaveNet: Fast High-Fidelity Speech Synthesis》

代码：

https://github.com/ibab/tensorflow-wavenet

一个Tensorflow实现

https://github.com/buriburisuri/speech-to-text-wavenet

这个Tensorflow实现，利用WaveNet实现了语音识别。

![](/images/img2/WaveNet.png)

![](/images/img2/WaveNet.gif)

参考：

https://www.leiphone.com/news/201609/ErWGa8fs7yR1zn2L.html

DeepMind发布最新原始音频波形深度生成模型WaveNet，将为TTS带来无数可能

https://zhuanlan.zhihu.com/p/27064536

用Wavenet做中文语音识别

https://mp.weixin.qq.com/s/-NTQG7_-GqGQWrRhiGgAQQ

详述DeepMind wavenet原理及其TensorFlow实现

http://mp.weixin.qq.com/s/0Xg_acbGG3pTIgsRQKJjrQ

历经一年，DeepMind WaveNet语音合成技术正式产品化

https://mp.weixin.qq.com/s/u1UnAuGllcWn8Ik5wDPY6w

可视化语音分析：深度对比Wavenet、t-SNE和PCA等算法

# EESEN

论文：

《EESEN: End-to-end speech recognition using deep RNN models and WFST-based decoding》

