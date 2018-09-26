---
layout: post
title:  Tensor2Tensor
category: AI 
---

# Tensor2Tensor

T2T库利用TensorFlow工具来开发，定义了一个深度学习系统中需要的多个部分：数据集、模型架构、优化工具、学习速率衰减计划，以及超参数等等。

最重要的是，T2T在所有这些部分之间实现了标准接口，并配置了当前机器学习的最佳行为方式。

因此，你可以选择任意数据集、模型、优化工具，以及一套超参数，随后运行训练，看看效果如何。

论文：

《Tensor2Tensor for Neural Machine Translation》

代码：

https://github.com/tensorflow/tensor2tensor

参考：

https://cloud.tencent.com/developer/article/1153079

“变形金刚”为何强大：从模型到代码全面解析Google Tensor2Tensor系统

这篇blog不仅对Tensor2Tensor的基本结构做了介绍，也对transformer模型进行了介绍。transformer模型的详解参见《深度学习（二十五）》。

# Tensor2Tensor transformer实战

## 准备数据

tensor2tensor/data_generators/translate_enzh.py

这个脚本包含了很多数据集的下载地址。

我们这里使用的是官方提供英汉翻译数据集：

http://data.statmt.org/wmt18/translation-task/training-parallel-nc-v13.tgz

这个数据集中，中英文是分开的：

training-parallel-nc-v13/news-commentary-v13.zh-en.en

training-parallel-nc-v13/news-commentary-v13.zh-en.zh

上面是训练集，测试集也是类似的。

## 数据预处理

这个过程比较漫长，大约1小时左右，期间CPU全满，而GPU全空，一度让我以为我的GPU相关配置不对。

数据预处理主要完成如下数据转换：

token（文字）->id（词典中的index）

它主要由T2T中的Problem模块进行配置。

例如，英汉翻译的设置一般为：`--problem=translate_enzh_wmt32k`。

Problem可以是一条继承链，比如上例中的继承链为：

TranslateEnzhWmt32k->TranslateProblem->Text2TextProblem->Problem

Problem类是所有Problem的基类。

由于中文符号表中，不仅有字还有词，让我一度以为使用了什么分词工具，后来才发现只是简单的词频统计相关的处理而已。相关代码在：

tensor2tensor/data_generators/text_encoder.py：SubwordTextEncoder.build_from_token_counts

T2T默认使用谷歌内部的subword分词法。这个分词法还有个独立的入口在：

tensor2tensor/data_generators/text_encoder_build_subword.py

除此之外，网上也有人使用Byte Pair Encoding(BPE)分词法进行分词。

tensor2tensor/layers/common_layers.py：embedding

导入词向量：

transformer模型的input/output都是单词在词典中的index，需要通过查询词向量表，将其替换为词向量。

tensor2tensor/layers/modalities.py：SymbolModality

encoder导入：input_emb

decoder导入：target_emb

## 模型

transformer模型定义文件如下：

tensor2tensor/models/transformer.py

这里我采用的是transformer_base_single_gpu的超参，loss可降至0.4左右。如果采用transformer_base的话，就只能降到2.0左右。

num_encoder_layers/num_decoder_layers控制transformer的层数，如果为0，就使用num_hidden_layers的值。

位置信息的函数：

get_timing_signal_1d

## mesh tensorflow

T2T不仅支持单机，还支持网格（Mesh）计算，推出了所谓的mesh tensorflow，简称MTF。

## 参考

https://blog.csdn.net/hpulfc/article/details/81172498

如何使用tensor2tensor自定义数据并训练模型

https://blog.csdn.net/hpulfc/article/details/82625217

tensor2tensor自定义问题，训练模型(bpe篇)

# GNU Octave

GNU Octave是Matlab的一个开源实现。它拥有和后者兼容的语法，类似的IDE，并实现了大部分的基础库。

官网：

https://gnu.org/software/octave/

安装方法:

`sudo apt-get install octave`

# 功率谱

随机过程（设时间序列为$$u(n)$$）二阶统计：

时域——**自相关函数**：

$$r_N(n-k)=E[u_N(n)u_N^*(k)]\tag{1}$$

其中，$$u_N^*(k)$$是$$u_N(k)$$的复共轭。

频域：

$$U_N(\omega)=\sum_{n=-N}^Nu_N(n)e^{-j\omega n}\tag{2}$$

$$S(\omega)=\lim_{N\to\infty}\frac{1}{N}E[\mid U_N(\omega)\mid^2]=\sum_{l=-\infty}^{+\infty}r(l)e^{-j\omega l}\tag{3}$$

其中，$$S(\omega)$$就是**功率谱密度（power spectral density, PSD）**，也称为功率谱（power spectrum）。

自相关函数和功率谱密度组成了傅立叶变换对，这种关系又被称为**EWK（Einstein-Wiener-Khintchine）关系**。

>Einstein最早提出idea，Wiener证明了一个特例，Khintchine做了扩展证明。

>Aleksandr Yakovlevich Khinchin，1894～1959，苏联数学家。莫斯科州立大学毕业，并留校任教，直到去世。苏联概率学派的重要人物。苏联科学院院士。概率论中，著名的Khintchine inequality就是他的成果。

在频域上，我们有Nyquist频率，相应的在时域上，我们也有**Nyquist间隔**：在这个间隔之外，$$S(\omega)$$是周期性的。

**离散时间随机过程的功率谱密度是非负实函数。**

$$S_o(\omega)=\mid H(e^{j\omega})\mid S(\omega)\tag{4}$$

其中，H为系统函数，$$S_o$$输出信号的功率谱密度。

功率谱密度的Cramér表示：

$$u(n)=\frac{1}{2\pi}\int_{-\pi}^{\pi}e^{j\omega n}\mathrm{d}Z(\omega)\tag{5}$$

其中，$$\mathrm{d}Z(\omega)$$被称为**增量过程（increment process）**。

>Harald Cramér，1893～1985，瑞典数学家、统计学家。Stockholm University博士（1917）、教授、校长、瑞典高等教育系统大臣。被誉为“统计理论的巨人”。

由公式2和5，可得：

$$U_N(\omega)=\frac{1}{2\pi}\int_{-\pi}^{\pi}\sum_{n=-N}^N e^{(-j(\omega-v) n)}\mathrm{d}Z(v)\tag{6}$$

我们定义：

$$K_N(\omega)=\sum_{n=-N}^N e^{j\omega n}=\frac{\sin((2N+1)\omega/2)}{\sin(\omega/2)}\tag{7}$$

则公式6可改写为：

$$U_N(\omega)=\frac{1}{2\pi}\int_{-\pi}^{\pi}K_N(\omega-v)\mathrm{d}Z(v)\tag{8}$$

这里的K被称作Dirichlet Kernel。参见《数学狂想曲（一）》的相关章节。

一般来说，在公式8中，$$U_N(\omega)$$是已知的，而$$\mathrm{d}Z(\omega)$$是未知的。从数学上来说，这个积分方程可看做第一类Fredholm积分方程的一个例子。

>Erik Ivar Fredholm，1866～1927，瑞典数学家。Uppsala University博士（1898）+Stockholm University教授。不知道是不是瑞典的保险业比较发达，他和Cramér居然都当过兼职的精算师。。。瑞典皇家科学院院士。

>Uppsala University是瑞典，也是北欧最古老的大学，始建于1477年。

功率谱密度的估计方法主要包括参数法和非参数法两大类。

参数法包括：

1.模型辨识法。基本就是上面提到的ARIMA或者其变种。

2.最小方差无失真响应法（MVDR）。

3.特征分解法。将相关矩阵R分解为两个子空间：信号子空间和噪声子空间。

非参数法包括：

1.周期图法。

2.多窗口法。

一般来说，随机过程的功率谱包含两个分量：确定性分量和连续分量。前者是增量过程$$\mathrm{d}Z(\omega)$$的一阶矩，后者是$$\mathrm{d}Z(\omega)$$的二阶中心矩。

参数法一般在知道相关物理规律时使用，它具有较高的精确度。而非参数法由于只依赖增量过程的一阶矩和二阶中心矩，因此适用范围更广泛，即使不知道系统的物理规律也可以使用。（有些类似万能拟合的GMM）

参考：

https://www.zhihu.com/question/29520851

功率谱密度如何理解？

# 高阶统计

上面讨论的基本都是一阶和二阶统计量，实际上我们还可以使用更高阶的统计量。使用高阶统计量的学科，一般被称为高阶统计学（higher-order statistics）。

## Moment

Moment（矩）的定义为：

$$\mu_n = \int_{-\infty}^\infty (x - c)^n\,f(x)\,\mathrm{d}x$$

其中，c=0时，被称作Raw Moment。c为均值时，被称作Central Moment。如果用$$\mu_n/\sigma^n$$替换$$\mu_n$$，就是所谓的Normalised Moment了。

1阶Raw Moment叫做Mean，2阶Central Moment叫做Variance，3阶Normalised Moment叫做Skewness，4阶Normalised Moment叫做kurtosis。

## Cumulants

Cumulants（累积量）的思想最早是Thorvald Thiele提出的，后来被Ronald Fisher和John Wishart发扬光大。

>Thorvald Nicolai Thiele，1838～1910，丹麦天文学家。哥本哈根大学博士。哥本哈根天文台台长（1978～1907）。曾研究过三体问题。被Ronald Fisher誉为“最伟大的统计学家”。

>John Wishart，1898～1956，苏格兰数学家和农业统计学家。Edinburgh University本科+Cambridge University硕士+University College London博士。导师是Karl Pearson，和Ronald Fisher也有过合作。Royal Society of Edinburgh会员。Cambridge University统计实验室首任主任。

>苏格兰人的自我意识真是强，足球有自己的协会，就连皇家学会也有自己的。

在介绍Cumulants之前，我们首先看一下Moment-generating function：

$$M_X(t) := \operatorname E \left[e^{tX}\right], \quad t \in \mathbb{R}$$

可以看出，MGF和《数学狂想曲（二）》中提到的随机变量的特征函数（Characteristic function）的形式非常类似。

而cumulant-generating function则是MGF的对数，即：

$$K(t)=\log\operatorname{E}\left[e^{tX}\right]$$

对上式进行Maclaurin展开，可得：

$$K(t)=\sum_{n=1}^\infty \kappa_{n} \frac{t^{n}}{n!} = \mu t + \sigma^2 \frac{t^2}{2} + \cdots$$

这里的$$\kappa_{n}$$就是Cumulants了。

## Polyspectrum

Polyspectrum一般翻译成高阶谱或者多谱。

>吐槽一下，我最早看的一本书把Polyspectrum翻译成多谱，结果我以此为关键词搜索，基本一无所获。直到我发现它还有另一个中文名。。。



## 参考

https://www.zhihu.com/question/25344430

随机变量的矩和高阶矩有什么实在的含义？

https://www.zhihu.com/question/43469699

信号的矩和高阶累积量的定义是什么？

http://www.doc88.com/p-1127198771359.html

高阶累积量与高阶谱读书笔记

https://wenku.baidu.com/view/7c4931085727a5e9856a6139.html

高阶谱分析

# 张量分析

在同构的意义下，第零阶张量（r = 0）为标量（Scalar），第一阶张量（r = 1）为向量（Vector），第二阶张量（r = 2）则成为矩阵（Matrix）。

《张量分析》，黄克智著。

>注：黄克智，1927年生，固体力学家。江西中正大学本科+清华硕士+莫斯科大学博士（因应召回国，放弃博士学位）。清华大学工程力学系教授、工程力学研究所所长，中国科学院院士。断裂力学领域权威。

# 拓扑学

《Topopogy Without Tears》，University of New South Wales的Sidney A. Morris著。

该书的中文版：

http://www.topologywithouttears.net/topbookchinese.pdf

