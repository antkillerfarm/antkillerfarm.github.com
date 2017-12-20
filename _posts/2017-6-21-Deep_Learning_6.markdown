---
layout: post
title:  深度学习（六）——DRN, Bi-directional RNN, Attention
category: DL 
---

# 神经元激活函数进阶（续）

## ReLU的缺点

深度神经网络的训练问题，最早是2006年Hinton使用**分层无监督预训练**的方法解决的，然而该方法使用起来很不方便。

而深度网络的**直接监督式训练**的最终突破，最主要的原因是采用了新型激活函数ReLU。

但是ReLU并不完美。它在x<0时硬饱和，而当x>0时，导数为1。所以，ReLU能够在x>0时保持梯度不衰减，从而缓解梯度消失问题。但随着训练的推进，部分输入会落入硬饱和区，导致对应权重无法更新。这种现象被称为**神经元死亡**。

ReLU还经常被“诟病”的另一个问题是输出具有**偏移现象**，即输出均值恒大于零。偏移现象和神经元死亡会共同影响网络的收敛性。实验表明，如果不采用Batch Normalization，即使用MSRA初始化30层以上的ReLU网络，最终也难以收敛。

为了解决上述问题，人们提出了Leaky ReLU、PReLU、RReLU、ELU、Maxout等ReLU的变种。

## Maxout

Maxout Networks是Ian J. Goodfellow于2013年提出的一大类激活函数。

![](/images/article/maxout.png)

上图是Maxout Networks的结构图。传统的激活函数一般是这样的形式：$$\sigma(Wx+b)$$

Maxout Networks将$$Wx+b$$这部分运算，分成k个组。每组的w和b都不相同。然后对每组计算结果$$z_{ij}$$取最大值。

从这个意义来说，ReLU可以看做是Maxout的特殊情况，即：

$$y=\max(W_1x+b_1,W_2x+b_2)=\max(0,Wx+b)$$

更多的情况参见下图：

![](/images/article/maxout_2.png)

从Maxout Networks的角度来看，ReLU和DropOut实际上是非常类似的。

参考：

http://blog.csdn.net/hjimce/article/details/50414467

Maxout网络学习

## Swish

Swish是Google大脑团队提出的一个新的激活函数：

$$\text{swish}(x)=x\cdot\sigma(x)=\frac{x}{1+e^{-x}}$$

它的图像如下图中的橙色曲线所示：

![](/images/article/swish.png)

Swish可以看作是GLU的特例（Swish的两组参数相同），后者是由facebook提出的：

$$(\boldsymbol{W}_1\boldsymbol{x}+\boldsymbol{b}_1)\otimes \sigma(\boldsymbol{W}_2\boldsymbol{x}+\boldsymbol{b}_2)$$

Swish在原点附近不是饱和的，只有负半轴远离原点区域才是饱和的，而ReLu在原点附近也有一半的空间是饱和的。而我们在训练模型时，一般采用的初始化参数是均匀初始化或者正态分布初始化，不管是哪种初始化，其均值一般都是0，也就是说，初始化的参数有一半处于ReLu的饱和区域，这使得刚开始时就有一半的参数没有利用上。特别是由于诸如BN之类的策略，输出都自动近似满足均值为0的正态分布，因此这些情况都有一半的参数位于ReLu的饱和区。相比之下，Swish好一点，因为它在负半轴也有一定的不饱和区，所以参数的利用率更大。

苏剑林据此提出了自己的激活函数：

$$\max(x, x\cdot e^{-|x|})$$

该函数的图像如上图的蓝色曲线所示。

参考：

https://mp.weixin.qq.com/s/JticD0itOWH7Aq7ye1yzvg

谷歌大脑提出新型激活函数Swish惹争议：可直接替换并优于ReLU？

http://kexue.fm/archives/4647/

浅谈神经网络中激活函数的设计

## 其他激活函数

### hard tanh

$$\text{HardTanh}(x)=\begin{cases}
-1, & x<-1 \\
x, & -1\le x \le 1 \\
1, & x>1 \\
\end{cases}$$

![](/images/article/hard_tanh.png)

### soft sign

$$\text{softsign}(x)=\frac{x}{1+|x|}$$

## 参考

https://zhuanlan.zhihu.com/p/22142013

深度学习中的激活函数导引

http://blog.csdn.net/u012328159/article/details/69898137

几种常见的激活函数

https://mp.weixin.qq.com/s/Hic01RxwWT_YwnErsJaipQ

什么是激活函数？

https://mp.weixin.qq.com/s/4gElB_8AveWuDVjtLw5JUA

深度学习激活函数大全

https://mp.weixin.qq.com/s/7DgiXCNBS5vb07WIKTFYRQ

从ReLU到Sinc，26种神经网络激活函数可视化

http://www.cnblogs.com/ymjyqsx/p/6294021.html

PReLU与ReLU

# Deep Residual Network

无论采用何种方法，可训练的神经网络的层数都不可能无限深。有的时候，即使没有梯度消失，也存在训练退化（即深层网络的效果还不如浅层网络）的问题。

最终2015年，微软亚洲研究院的何恺明等人，使用残差网络ResNet参加了当年的ILSVRC，在图像分类、目标检测等任务中的表现大幅超越前一年的比赛的性能水准，并最终取得冠军。

论文：

《Deep Residual Learning for Image Recognition》

代码：

https://github.com/KaimingHe/deep-residual-networks

>注：何恺明，清华本科+香港中文大学博士（2011）。先后在MS和Facebook担任研究员。   
>个人主页：http://kaiminghe.com/

残差网络的明显特征是有着相当深的深度，从32层到152层，其深度远远超过了之前提出的深度网络结构，而后又针对小数据设计了1001层的网络结构。

其简化版的结构图如下所示：

![](/images/article/drn.png)

简单的说，就是把前面的层跨几层直接接到后面去，以使误差梯度能够传的更远一些。

DRN的基本思想倒不是什么新东西了，在2003年Bengio提出的词向量模型中，就已经采用了这样的思路。

DRN的实现依赖于下图所示的res block：

![](/images/article/res_block.png)

从中可以看出，所谓残差跨层传递，其实就是将本层ternsor $$\mathcal{F}(x)$$和跨层tensor x加在一起而已。

参考：

https://zhuanlan.zhihu.com/p/22447440

深度残差网络

https://www.leiphone.com/news/201608/vhqwt5eWmUsLBcnv.html

何恺明的深度残差网络PPT

https://mp.weixin.qq.com/s/kcTQVesjUIPNcz2YTxVUBQ

ResNet 6大变体：何恺明,孙剑,颜水成引领计算机视觉这两年

https://mp.weixin.qq.com/s/5M3QiUVoA8QDIZsHjX5hRw

一文弄懂ResNet有多大威力？最近又有了哪些变体？

http://www.jianshu.com/p/b724411571ab

ResNet到底深不深？

https://mp.weixin.qq.com/s/Kgwwq5XOt88WW6KL8gADmQ

你必须要知道CNN模型：ResNet

# Bi-directional RNN

众所周知，RNN在处理长距离依赖关系时会出现问题。LSTM虽然改进了一些，但也只能缓解问题，而不能解决该问题。

研究人员发现将原文倒序（将其倒序输入编码器）产生了显著改善的结果，因为从解码器到编码器对应部分的路径被缩短了。同样，两次输入同一个序列似乎也有助于网络更好地记忆。

基于这样的实验结果，1997年Mike Schuster提出了Bi-directional RNN模型。

>注：Mike Schuster，杜伊斯堡大学硕士（1993）+奈良科技大学博士。语音识别专家，尤其是日语、韩语方面。Google研究员。

论文：

《Bidirectional Recurrent Neural Networks》

下图是Bi-directional RNN的结构示意图：

![](/images/article/Bi_directional_RNN.png)

从图中可以看出，Bi-directional RNN有两个隐层，分别处理前向和后向的时序信息。

除了原始的Bi-directional RNN之外，后来还出现了Deep Bi-directional RNN。

![](/images/article/Deep_Bi_RNN.png)

上图是包含3个隐层的Deep Bi-directional RNN。

参见：

https://mp.weixin.qq.com/s/_CENjzEK1kjsFpvX0H5gpQ

结合堆叠与深度转换的新型神经翻译架构：爱丁堡大学提出BiDeep RNN

# Attention

倒序句子这种方法属于“hack”手段。它属于被实践证明有效的方法，而不是有理论依据的解决方法。

大多数翻译的基准都是用法语、德语等语种，它们和英语非常相似（即使汉语的词序与英语也极其相似）。但是有些语种（像日语）句子的最后一个词语在英语译文中对第一个词语有高度预言性。那么，倒序输入将使得结果更糟糕。

还有其它办法吗？那就是Attention机制。

![](/images/article/attention.png)

上图是Attention机制的结构图。y是编码器生成的译文词语，x是原文的词语。上图使用了双向递归网络，但这并不是重点，你先忽略反向的路径吧。重点在于现在每个解码器输出的词语$$y_t$$取决于所有输入状态的一个权重组合，而不只是最后一个状态。a是决定每个输入状态对输出状态的权重贡献。因此，如果$$a_{3,2}$$的值很大，这意味着解码器在生成译文的第三个词语时，会更关注于原文句子的第二个状态。a求和的结果通常归一化到1（因此它是输入状态的一个分布）。

Attention机制的一个主要优势是它让我们能够解释并可视化整个模型。举个例子，通过对attention权重矩阵a的可视化，我们能够理解模型翻译的过程。

![](/images/article/attention_2.png)

我们注意到当从法语译为英语时，网络模型顺序地关注每个输入状态，但有时输出一个词语时会关注两个原文的词语，比如将“la Syrie”翻译为“Syria”。

如果再仔细观察attention的等式，我们会发现attention机制有一定的成本。我们需要为每个输入输出组合分别计算attention值。50个单词的输入序列和50个单词的输出序列需要计算2500个attention值。这还不算太糟糕，但如果你做字符级别的计算，而且字符序列长达几百个字符，那么attention机制将会变得代价昂贵。

attention机制解决的根本问题是允许网络返回到输入序列，而不是把所有信息编码成固定长度的向量。正如我在上面提到，我认为使用attention有点儿用词不当。换句话说，attention机制只是简单地让网络模型访问它的内部存储器，也就是编码器的隐藏状态。在这种解释中，网络选择从记忆中检索东西，而不是选择“注意”什么。不同于典型的内存，这里的内存访问机制是弹性的，也就是说模型检索到的是所有内存位置的加权组合，而不是某个独立离散位置的值。弹性的内存访问机制好处在于我们可以很容易地用反向传播算法端到端地训练网络模型（虽然有non-fuzzy的方法，其中的梯度使用抽样方法计算，而不是反向传播）。

论文：

《Learning to combine foveal glimpses with a third-order Boltzmann machine》

《Learning where to Attend with Deep Architectures for Image Tracking》

《Neural Machine Translation by Jointly Learning to Align and Translate》

参考：

http://blog.csdn.net/malefactor/article/details/50550211

自然语言处理中的Attention Model

https://yq.aliyun.com/articles/65356

图文结合详解深度学习Memory & Attention

http://www.cosmosshadow.com/ml/%E7%A5%9E%E7%BB%8F%E7%BD%91%E7%BB%9C/2016/03/08/Attention.html

Attention

http://geek.csdn.net/news/detail/50558

深度学习和自然语言处理中的attention和memory机制

https://zhuanlan.zhihu.com/p/25928551

用深度学习（CNN RNN Attention）解决大规模文本分类问题-综述和实践

http://blog.csdn.net/leo_xu06/article/details/53491400

视觉注意力的循环神经网络模型

https://mp.weixin.qq.com/s/XrlveG0kwij2qNL45TZdBg

Attention的另类用法

https://zhuanlan.zhihu.com/p/31547842

深度学习中Attention Mechanism详细介绍：原理、分类及应用

https://zhuanlan.zhihu.com/p/32089282

Attention学习笔记

https://mp.weixin.qq.com/s/0yb-YRGe-q4-vpKpuE4D_w

多种注意力机制互补完成VQA（视觉问答）

https://mp.weixin.qq.com/s/LQ7uv0-AakkHE5b17yemqw

Awni Hannun：序列模型Attention Model中的问题与挑战


