---
layout: post
title:  深度学习（四十七）——无监督/半监督/自监督深度学习, 语音合成
category: DL 
---

# 无监督/半监督/自监督深度学习

https://mp.weixin.qq.com/s/kbqTHIOzAj1aERl4tm-kVA

2017上半年无监督特征学习研究成果汇总

https://mp.weixin.qq.com/s/aCWAU2RXk9fTzfFqOyjqUw

能自主学习的人工突触，为无监督学习开辟新的路径

https://mp.weixin.qq.com/s/9kMz-eNRwC51Fi0-7BfKzA

Active Learning: 一个降低深度学习时间，空间，经济成本的解决方案

https://mp.weixin.qq.com/s/ZvTm9omnIRqPXcLFbZtoeg

深度学习的关键：无监督深度学习简介

https://mp.weixin.qq.com/s/GHjmiB6F2W3Zo8gVllTyyQ

重现“世界模型”实验，无监督方式快速训练

https://mp.weixin.qq.com/s/3_VtdZNKBwNtMEMf2xc7qw

CVPR智慧城市挑战赛：无监督交通异常检测，冠军团队技术分享

https://mp.weixin.qq.com/s/3aAaM1DWsnCWEEbP7dOZEg

伯克利等提出无监督特征学习新方法，代码已开源

https://mp.weixin.qq.com/s/ZDPPWH570Vc6e1irwP1b1Q

精细识别现实世界图像：李飞飞团队提出半监督适应性模型

https://mp.weixin.qq.com/s/X1Alcl7rVfTtZGZ40iXjXw

Spotlight 论文：非参数化方法实现的极端无监督特征学习

https://mp.weixin.qq.com/s/kxEfoSjCF8n2jxlDfMaNDA

半监督学习在图像分类上的基本工作方式

https://mp.weixin.qq.com/s/uUMPUdG2TI10W5RumPaXkA

DeepMind无监督表示学习重大突破：语音、图像、文本、强化学习全能冠军！

https://mp.weixin.qq.com/s/_VC6PGdCjlhcsndpunIteg

何恺明等人提出新型半监督实例分割方法：学习分割Every Thing

https://mp.weixin.qq.com/s/qaxzSSDuuscwL5tt0QCQ0Q

破解人类识别文字之谜：对图像中的字母进行无监督学习

https://mp.weixin.qq.com/s/IsLlzDWnUXe8LVp4Y1Jb_A

35亿张图像！Facebook基于弱监督学习刷新ImageNet基准测试记录

https://mp.weixin.qq.com/s/TEk_i4kEjUqmAqF8LgTVjg

FAIR提出用聚类方法结合卷积网络，实现无监督端到端图像分类

https://mp.weixin.qq.com/s/dSncg1pDHpIFOT4mXrFntA

Yan Lecun自监督学习：机器能像人一样学习吗？ 110页PPT

https://mp.weixin.qq.com/s/W4zwKqkVQN4v-IKzGrkudg

通过传递不变性实现自监督视觉表征学习

https://zhuanlan.zhihu.com/p/30265894

自监督学习近期进展

https://mp.weixin.qq.com/s/cTlXMxcpzc7_5NVsTm1jcA

学习一帧，为整段黑白视频上色：谷歌提出自监督视觉追踪模型

https://mp.weixin.qq.com/s/Amr34SdrPZho1GQpFS7WBA

见微知著：语义分割中的弱监督学习

https://mp.weixin.qq.com/s/zOWA1oKbopZJuYIAYYlKTA

港中文-商汤联合论文：自监督语义分割的混合与匹配调节

https://mp.weixin.qq.com/s/5xlSoC5sgzsAwMYMSFCjnw

TextTopicNet:CMU开源无标注高精度自监督模型

https://mp.weixin.qq.com/s/343DfjOvkaozuxNK89V3zQ

前景目标检测的无监督学习

# 语音合成

语音合成（Speech synthesis），有时也叫做text-to-speech (TTS)。

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

## Tacotron

Tacotron是DeepMind提出的TTS模型。

论文：

《Tacotron: A Fully End-to-End Text-To-Speech Synthesis Model》

代码：

https://github.com/keithito/tacotron

![](/images/img2/Tacotron.png)

![](/images/img2/Tacotron_2.png)

图中的Griffin-Lim reconstruction所使用的算法，是由Daniel W. Griffin和Jae S. Lim于1984年提出的。当然了，这是一种传统的信号处理算法，一般作为新的DL替代品的baseline使用。

>Jae S. Lim，1950年生，韩裔美国人。MIT本硕博（1978）。MIT教授，IEEE Fellow。信号处理领域专家。

Griffin-Lim Algorithm（GLA）的论文：

《Signal Estimation from Modified Short-Time Fourier Transform》

参考：

https://mp.weixin.qq.com/s/MJE2JRYU7KakNKmHkD42CA

谷歌发布TTS新系统Tacotron 2：直接从文本生成类人语音

https://mp.weixin.qq.com/s/uh-Gh8BSxBi-jjG6-d7-UQ

Tacotron一种端到端的Text-to-Speech合成模型

https://www.jiqizhixin.com/articles/2017-03-31-5

谷歌全端到端语音合成系统Tacotron：直接从字符合成语音

## Tacotron-2

Tacotron2是Tacotron的升级版，也是DeepMind提出的。

论文：

《Natural TTS synthesis by conditioning Wavenet on MEL spectogram predictions》

代码：

https://github.com/Rayhane-mamah/Tacotron-2

![](/images/img2/Tacotron_3.png)

上图是Tacotron-2的体系结构图。除了标明Tacotron-2的网络结构之外，还给出了后续处理的pipeline：

1.Tacotron-2模型负责将Text转换为Mel Spectrum。

2.WaveNet模型负责将Mel Spectrum转换成wave pcm data。

## WaveNet

WaveNet是DeepMind 2016年的作品，主要用于语音合成，也可用于语音识别。

DeepMind在WaveNet方面，按照时间顺序有3篇论文：

《WaveNet: A Generative Model For Raw Audio》

《Neural Machine Translation in Linear Time》

《Parallel WaveNet: Fast High-Fidelity Speech Synthesis》

代码：

https://github.com/ibab/tensorflow-wavenet

一个Tensorflow实现

https://github.com/buriburisuri/speech-to-text-wavenet

这个Tensorflow实现，利用WaveNet实现了语音识别。

除了RNN之外，CNN也经常被用于分析时间序列。为了表明序列之间的因果关系，人们通常采用一种叫做Causal Convolution的结构来进行数据采样。

![](/images/img2/WaveNet_2.png)

上图是Causal Convolution的结构图。从中可以很明显的看出它的局限性：

1.感受野较小。

2.为了增大感受野，需要添加更多的层，从而导致计算复杂和训练困难。

![](/images/img2/WaveNet.gif)

针对Causal Convolution的不足，WaveNet采用了Dilated Convolution的方式进行采样。具体的步骤如上图所示。

![](/images/img2/WaveNet.png)

上图是WaveNet用于语音识别时的网络结构图。

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

https://blog.csdn.net/Kuo_Jun_Lin/article/details/80602776

TCN_时间卷积网络_原理与优势

https://blog.csdn.net/tonygsw/article/details/81280364

因果卷积（causal）与扩展卷积（dilated）

## WaveRNN

WaveRNN是WaveNet的升级版。

论文：

《Efficient Neural Audio Synthesis》

非官方代码：

https://github.com/fatchord/WaveRNN

WaveRNN没有沿用Dilated Convolution，而是采用了另外一种方式加速RNN运算。

![](/images/img2/WaveRNN.png)

上图是WaveRNN的网络结构图。它将生成16bit采样点的任务，分解为生成高8bit和低8bit两个子任务。（这两者在论文中被称作coarse parts和fine parts。）这样最终生成样本的softmax层的大小就由$$2^{16}$$变成了$$2\times 2^8$$了。

这篇论文的另一个重要贡献是提出了Subscale WaveRNN。它的流程如下图所示：

![](/images/img2/WaveRNN_2.png)

1.将原始序列重组为B组（B表示Batch Size，这里为8）。上图中的序号表示在原始序列中的序号。

2.作者发现直接分组并行生成，虽然简单，但效果不好。其原因在于只用到了过去的序列数据，而缺少对整个序列信息的把握。但直接采用全局信息，又不是RNN的做法。为此论文提出了提前量的概念，即上图中的F。

3.我们可以将B组数据看作B个赛道。第1行首先发车（生成数据），当第1行生成了F个数据之后，第2行开始发车。依此类推。显然，只要赛道足够长，则在大多数时间中，都是B个赛车同时再跑。这样就达到了并行运算的效果。如上图中的三个白色方格就是同时生成的。

4.数据生成好之后，再重新组合回去，得到最终的结果序列。

参考：

https://mp.weixin.qq.com/s/H77iom38lTR0KzeFXrdWew

DeepMind与谷歌大脑联手推出WaveRNN，移动端合成高保真音频媲美WaveNet

https://www.monthly-hack.com/entry/2018/02/26/211248

这是个日文blog，有两张图不错。

https://www.jianshu.com/p/b3019f2773ed

语音合成text-to-speech WaveRNN

http://slides.com/smerity/reading-group-efficient-neural-audio-synthesis#/

Efficient Neural Audio Synthesis

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

https://mp.weixin.qq.com/s/p_VjFwwDCu1i_ovUljaoVw

阿里巴巴语音交互智能团队：基于线性网络的语音合成说话人自适应

https://mp.weixin.qq.com/s/imotc0RfPsvA9h5-1nouMA

端到端语音合成及其优化实践(上))

https://mp.weixin.qq.com/s/NlOH0wmToJvDudIDC-aM1g

端到端语音合成及其优化实践(下)

https://mp.weixin.qq.com/s/HLe4DUZWWfdorcgYOj9gzw

语音合成领域的首个完全端到端模型，百度提出并行音频波形生成模型ClariNet

https://zhuanlan.zhihu.com/p/45702794

微信是不是可以来一个文字转语音功能了？

https://mp.weixin.qq.com/s/DB2C-a_xEyoczuNSG9Bt7w

基于深度前馈序列记忆网络，如何将语音合成速度提升四倍？
