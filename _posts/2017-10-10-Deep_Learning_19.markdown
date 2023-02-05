---
layout: post
title:  深度学习（十九）——LSTM进阶
category: DL 
---

* toc
{:toc}

# LSTM进阶

## 《Long short-term memory》

这是最早提出LSTM这个概念的论文。这篇论文偏重数学推导，实话说不太适合入门之用。但既然是起点，还是有列出来的必要。

## 《LSTM Neural Networks for Language Modeling》

这也是一篇重要的论文。

## 《Sequence to Sequence - Video to Text》

https://vsubhashini.github.io/s2vt.html

![](/images/article/S2VTarchitecture.png)

参考：

https://mp.weixin.qq.com/s/HqffHVMKJkA6H1E9ItQU4A

《sequence to sequence: video to text》视频描述的全文翻译

## 《Long-term Recurrent Convolutional Networks for Visual Recognition and Description》

Long-term Recurrent Convolutional Networks是LSTM的一种应用方式，它结合了LSTM、CNN、CRF等不同网络组件。

![](/images/article/LSTM_X.png)

上图展示了LSTM在动作识别、图片和视频描述等任务中的网络结构。

![](/images/article/LSTM_X_2.png)

上图展示了图片描述任务中几种不同的网络连接方式：

1.单层LRCN。

2.双层LRCN。CNN连接在第一个LSTM层。传统的LSTM只有一个输入，这里的CNN是第二个输入，也就是所谓的静态输入。可参看caffe的LSTM实现。

2.双层LRCN。CNN连接在第二个LSTM层。

![](/images/article/LSTM_X_3.png)

![](/images/article/LSTM_X_4.png)

![](/images/article/LSTM_X_5.png)

这是视频描述任务中LSTM和CRF结合的示例。

## 《Training RNNs as Fast as CNNs》

这篇论文提出了如下图所示的Simple Recurrent Unit（SRU）的新结构：

![](/images/article/SRU.jpg)

由于普通LSTM计算步骤中，很多当前时刻的计算都依赖$$h_{t-1}$$的值，导致整个网络的计算无法并行化。SRU针对这一点去掉了当前时刻计算对于$$h_{t-1}$$的依赖，而仅保留$$C_{t-1}$$（这个计算较为廉价）以记忆信息，大大改善了整个RNN网络计算的并行性。

但是SRU的精度没有LSTM高，需要通过增加layer和filter的数量来达到相同的精度，当然即使这样，计算时间仍然小于LSTM。

参考：

https://mp.weixin.qq.com/s/PsIa3XDFqZlY2tKcvqvddw

Training RNNs as Fast as CNNs

## 《Neural Machine Translation in Linear Time》

该论文是Deepmind的作品，它提出的ByteNet，计算复杂度为线性，也是LSTM的优化方案之一。

## 《Long Short-Term Memory Based Recurrent Neural Network Architectures for Large Vocabulary Speech Recognition》

$$
\begin{array}\\
i_t=\delta(W_{ix}x_t+W_{im}m_{t-1}+W_{ic}c_{t-1}+b_i) \\
f_t=\delta(W_{fx}x_t+W_{fm}m_{t-1}+W_{fc}c_{t-1}+b_i) \\
c_t=f_t\odot c_{t-1}+i_t\odot g(W_{cx}x_t+W_{cm}m_{t-1}+b_c) \\
o_t=\delta(W_{ox}x_t+W_{om}m_{t-1}+W_{oc}c_{t}+b_o) \\
m_t=o_t\odot h(c_t) \\
{\color{blue}{y_t=W_{ym}m_t+b_y}}
\end{array}
$$

上式是LSTM的公式（其中的最后一步在多数模型中，往往直接用$$y_t=m_t$$代替。），从中可以看出类似$$W_{ix}x_t+W_{im}m_{t-1}+W_{ic}c_{t-1}+b_i$$的FC运算占据了LSTM的绝大部分运算量。其中W的参数量为：

$$W={\color{blue}{n_c\times n_c\times 4}}+n_i\times n_c\times 4+{\color{red}{n_c\times n_o}}+n_c\times 3$$

为了精简相关运算，Google的Hasim Sak于2014年提出了LSTMP。

>Hasim Sak，土耳其伊斯坦布尔海峡大学博士，Google研究员。

LSTMP的结构图如下：

![](/images/img2/LSTMP.png)

改写成数学公式就是：

$$
\begin{array}\\
i_t=\delta(W_{ix}x_t+W_{im}r_{t-1}+W_{ic}c_{t-1}+b_i)\\
f_t=\delta(W_{fx}x_t+W_{fm}r_{t-1}+W_{fc}c_{t-1}+b_i)\\
c_t=f_t\odot c_{t-1}+i_t\odot g(W_{cx}x_t+W_{cm}r_{t-1}+b_c)\\
o_t=\delta(W_{ox}x_t+W_{om}r_{t-1}+W_{oc}c_{t}+b_o)\\
m_t=o_t\odot h(c_t)\\
{\color{blue}{r_t = W_{rm}m_t}}\\
{\color{blue}{p_t = W_{pm}m_t}}\\
{\color{blue}{y_t = W_{yr}r_t + W_{yp}p_t + b_y}}
\end{array}
$$

LSTMP的主要思想是对$$m_t$$做一个映射，只有部分数据$$r_t$$参与recurrent运算，其余部分$$p_t$$直接输出即可（这一步是可选项，所以用虚框表示）。

这样W的参数量为：

$$W={\color{blue}{n_c\times n_r\times 4}}+n_i\times n_c\times 4+{\color{red}{n_r\times n_o+n_c\times n_r}}+n_c\times 3$$

参数量公式用蓝色和红色标出修改前后对应的部分，可以看出计算量有了明显下降。

参考：

http://blog.csdn.net/xmdxcsj/article/details/53326109

模型压缩lstmp

## Android NN

Android NN除了支持原始版本的LSTM之外，还可支持peephole、CIFG、LSTMP、Layer Normalization等变种，以及它们的组合。其公式如下：

$$\begin{eqnarray*}
i_t =& \sigma(LN(W_{xi}x_t+W_{hi}h_{t-1}+W_{ci}C_{t-1})+b_i) & \\
f_t =& \sigma(LN(W_{xf}x_t+W_{hf}h_{t-1}+W_{cf}C_{t-1})+b_f) & \\
C_t =& clip(f_t \odot C_{t-1} + i_t \odot g(LN(W_{xc}x_t+W_{hc}h_{t-1})+b_c),\ t_{cell}) & \\ o_t =& \sigma(LN(W_{xo}x_t+W_{ho}h_{t-1}+W_{co}C_t)+b_o) & \\ & & \\
 & clip(W_{proj}(o_t \odot g(C_t))+b_{proj},\ t_{proj}) & if\ there\ is\ a\ projection; \\
h_t =& & \\ & o_t \odot g(C_t) & otherwise. \\ 
\end{eqnarray*}$$

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

https://mp.weixin.qq.com/s/3djAWJs6ecDdPSpQMxqmrg

清华、李飞飞团队等提出强记忆力E3D-LSTM网络

https://mp.weixin.qq.com/s/__qC6Wzy4jaGNGzWr3eOIg

引入额外门控运算，LSTM稍做修改，性能便堪比Transformer-XL

# DRL参考资源++

https://mp.weixin.qq.com/s/zRXs3BCEqXUruw746rZusw

牛津大学联合7家单位发布AutoRL综述，还在手动调参吗？你已经落后了

https://mp.weixin.qq.com/s/wPWV6kxkRlYb9dMf6ADWkQ

史上首次，强化学习算法控制核聚变登上Nature：DeepMind让人造太阳向前一大步

https://mp.weixin.qq.com/s/U0K79ELLj4wsOR4sd5G4Vw

Vicarious详解新型图式网络：赋予强化学习泛化能力

https://mp.weixin.qq.com/s/C8hsGkHGtoaS9Vzm6Ub4tw

Berkeley提出“随机搜索”训练线性策略，提高RL的性能

https://mp.weixin.qq.com/s/uppNSwxNrw4_8NGBQv85xw

今日头条首次改进DQN网络，解决推荐中的在线广告投放问题

https://mp.weixin.qq.com/s/JtUuFdTK4Q5YwnVj3BFU2w

全参数化分布，提升强化学习中的收益分布拟合能力

https://mp.weixin.qq.com/s/amXiNKJPEkAnu2m5NAERVw

Top-K Oﬀ-Policy Correction

https://mp.weixin.qq.com/s/kNtzy9-6GbsRhlL-mxksew

基于强化学习的人机对话

https://mp.weixin.qq.com/s/w3SsadgKaL8-tlzYLvMm-A

讲真？一天就学会了自动驾驶——强化学习在自动驾驶的应用

https://mp.weixin.qq.com/s/nnWuIPk_6mI9IAKIUbx6KQ

深度强化学习解决交通控制问题

https://mp.weixin.qq.com/s/RNJonPJL9JY5OH2-1sZMQw

中山大学HCP实验室：基于树状结构策略的渐进强化学习

https://mp.weixin.qq.com/s/G2sFkuvSeYmhkFjjPRGI-Q

强化学习如何用于推荐？新南威尔士首篇《深度强化学习推荐系统》综述论文

https://mp.weixin.qq.com/s/fMjjGCzef-3SVIOlUf2EFA

强化学习如何用于推荐？厦大最新《强化学习推荐系统》综述论文

https://mp.weixin.qq.com/s/8cV3Z_vkC0_cyfO2nVKoSw

华人博士用强化学习回收了SpaceX火箭

https://mp.weixin.qq.com/s/_-WSoeOqXMhR7S0PtyYixQ

深度强化学习探索算法最新综述，近200篇文献揭示挑战和未来方向
