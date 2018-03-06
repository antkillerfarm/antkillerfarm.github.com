---
layout: post
title:  深度学习（二十五）——Deep Speech, WaveNet, L2 Normalization
category: DL 
---

# CTC（续）

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

# L2 Normalization

L2 Normalization本身并不复杂，然而多数资料都只提到1维的L2 Normalization的计算公式：

$$x=[x_1,x_2,\dots,x_d]\\
y=[y_1,y_2,\dots,y_d]\\
y=\frac{x}{\sqrt{\sum_{i=1}^dx_i^2}}=\frac{x}{\sqrt{x^Tx}}
$$

对于多维L2 Normalization几乎未曾提及，这里以3维tensor：A[width, height, channel]为例介绍一下多维L2 Normalization的计算方法。

多维L2 Normalization有一个叫axis(有时也叫dim)的参数，如果axis=0的话，实际上就是将整个tensor flatten之后，再L2 Normalization。这个是比较简单的。

这里说说axis=3的情况。axis=3意味着对channel进行Normalization，也就是：

$$B_{xy}=\sum_{z=0}^Z \sqrt{A_{xyz}^2}\\
C_{xyz}=\frac{A_{xyz}}{B_{xy}}\\
D_{xyz}=C_{xyz} \cdot S_{xy}
$$

一般来说，求出C被称作L2 Normalization，而求出D被称作L2 Scale Normalization，S被称为Scale。

