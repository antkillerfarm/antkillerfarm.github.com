---
layout: post
title:  深度学习（二十四）——L2 Normalization
category: DL 
---

# LSTM进阶（续）

## 《On the compression of recurrent neural networks with an application to lvcsr acoustic modeling for embedded speech recognition》

![](/images/img2/RNN_SVD.png)

这是另一篇RNN模型压缩的论文。上图中的a是原始RNN，而b是对$$W_h^l$$使用了SVD之后的RNN。

## 《Video Summarization with Long Short-term Memory》

这是一篇用于提取视频关键帧（也叫静态视频摘要）的论文，是南加州大学沙飞小组的作品。

![](/images/img2/dppLSTM.png)

上图是该文提出的DPP LSTM的网络结构图。它的主体是一个BiLSTM，算是中规中矩吧。

该文的创新点在于提出了DPP loss的概念。上图中的$$y_t$$表示帧的分值（越大表示越重要），$$\phi_t$$表示帧之间的相似度。该文的实验表明，将两个特征分开抽取，有助于提升模型的准确度。

这篇论文主要用到了3个数据集：

TVSum dataset: 

https://github.com/yalesong/tvsum

这个需要Yahoo账号和一个高校的邮件地址才行。

SumMe dataset: 

https://people.ee.ethz.ch/~gyglim/vsum/#benchmark

OVP and YouTube datasets: 

https://sites.google.com/site/vsummsite/

需要翻墙。

## LSTM运算加速技巧

LSTM的主要运算量集中在$$W[h_{t-1},x_t]$$上，这里实际上可以用$$W[(h_{t-2}+\Delta h_{t-1}),x_t]$$代替。

由于时间序列通常具有惯性，因此$$\Delta h_{t-1}$$一般包含了大量的0，这对于某些具有跳0功能的硬件来说，是非常有利的。

## Convolutional LSTM Network

论文：

《Convolutional LSTM Network: A Machine Learning Approach for Precipitation Nowcasting》

## 参考

https://mp.weixin.qq.com/s/4IHzOAvNhHG9c8GP0zXVkQ

Simple Recurrent Unit For Sentence Classification

https://mp.weixin.qq.com/s/fCzHbOi7aJ8-W9GzctUFNg

LSTM文本分类实战

http://mp.weixin.qq.com/s/3nwgft9c27ih172ANwHzvg

从零开始：如何使用LSTM预测汇率变化趋势

https://mp.weixin.qq.com/s/M18c3sgvjV2b2ksCsyOxbQ

Nested LSTM：一种能处理更长期信息的新型LSTM扩展

https://www.zhihu.com/question/62399257

如何理解LSTM后接CRF？

https://mp.weixin.qq.com/s/XAbzaMXP3QOret_vxqVF9A

用深度学习LSTM炒股：对冲基金案例分析

https://mp.weixin.qq.com/s/eeA5RZh35BvlFt45ywVvFg

可视化LSTM网络：探索“记忆”的形成

https://mp.weixin.qq.com/s/h-MYTNTLy7ToPPEZ2JVHpw

阿里巴巴论文提出Advanced LSTM：关于更优时间依赖性刻画在情感识别方面的应用

https://mp.weixin.qq.com/s/pv3gQfCayGmsmGKLbMIFpA

神奇！只有遗忘门的LSTM性能优于标准LSTM

https://mp.weixin.qq.com/s/8BPZ_M8EGk3KxkSleYWSNw

训练可解释、可压缩、高准确率的LSTM

https://hanxiao.github.io/2018/06/24/4-Encoding-Blocks-You-Need-to-Know-Besides-LSTM-RNN-in-Tensorflow/

4 Sequence Encoding Blocks You Must Know Besides RNN/LSTM in Tensorflow

https://mp.weixin.qq.com/s/zMCBQ2D21HoDcDgDolmGMA

上海交大：基于近似随机Dropout的LSTM训练加速

https://mp.weixin.qq.com/s/wPYd2jLUPzlPwIZkb_wSbA

深度递归LSTM-LRP非线性时变多因子模型

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
D_{xyz}=C_{xyz} \cdot S_{z}
$$

一般来说，求出C被称作L2 Normalization，而求出D被称作L2 Scale Normalization，S被称为Scale。
